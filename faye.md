# Faye

[Faye](http://faye.jcoglan.com/) é um sistema para subscrever e enviar mensagens baseado no protocolo *Bayeux* (Pub/Sub). O **Faye** providencia um **servidor** de mensagens para ser usado com *node.js* ou *Ruby* além de uma biblioteca em *Javascript* para os **clientes** que funciona em todos os browsers modernos.

## Instalação

Primeiro instale o gem executando o comando: ```gem install faye```

Adicione o **faye** e o servidor **thin** ao gemfile:

	gem 'faye'
	gem 'thin'
	
Em seguida rode o comando ```bundle install``` para instalar os *gems*.

Crie o arquivo ```faye.ru``` na raiz da pasta do projeto. Este arquivo vai inicializar o servidor **faye**.

	require 'faye'
	Faye::WebSocket.load_adapter('thin')
	faye_server = Faye::RackAdapter.new(:mount => 'faye', :timeout => 45)
	run faye_server
	
Depois rode o seguinte comando para inicializar o servidor:

```rackup faye.ru -s thin -E product```

Para fazer os dois servidores iniciarem juntos em *development*, modifique o arquivo ```Procfile```:

	web: bundle exec unicorn -p $PORT -c ./config/unicorn.rb
	faye: rackup faye.ru -s thin -E production
	
Depois execute o comando ```foreman start``` para iniciar o servidor

O servidor **faye** vai iniciar em ```0.0.0.0:9292```. Este servidor vai estar servidondo um arquivo *javascript* que precisa ser adicionado no layout da página no rails:

	<%= javascript_include_tag :defaults, "http://localhost:9292/faye.js" %>

	
Em seguida, vamos criar a função para subscrever e enviar mensagens para o servidor. Abra o arquivo ```app/assets/javascripts/application.js```:

	$(function() {
		var faye = new Faye.Client('http://localhost:9292/faye');
		faye.subscribe("messages/new", function(data){
			alert(data);
		});
	});
	
Para fazer um teste, é possivel enviar uma mensagem via **curl**. Primeiro entre ná página no navegador, depois no terminal execute o comando:

``` curl http://localhost:9292/faye -d 'message={"channel":"/messages/new", "data":"hello" }'```

Ao executar esse comando, o navegador ira receber uma mensagem com o conteúdo do comando.

## Exemplo: Chat

Adicione um *view* com uma lista e um formulário para novas mensagens:

	<ul id="chat">
		<%= render @messages %>
	</ul>
	
	<%= form_for Message.new, remote => true do |f| %>
		<%= f.text_field :content %>
		<%= f.submit "Send" %>
	<% end %>
	
Crie mais um *view* com o nome de ```create.js.erb``` para resposta do servidor em *JSON*:

	<% broadcast "/messages/new" do %>
		$("#chat").append("<%= escape_javascript render(@message) %>");
	<% end %>
	$("#new_message")[0].reset();
	
Toda vez que uma mensagem for enviada pela página e criada no servidor, o bloco de codigo dentro da função ```broadcast``` vai ser enviado via o faye para todos os clientes que estão assinando este canal.

Agora precisamos criar a função ```broadcast```. No arquivo ```app/helpers/application_helper.rb``` adicione a função:

	module ApplicationHelper
		def broadcast(channel, &block)
			message = { :channel => channel, :data => capture(&block)}
			uri = URI.parse("http://localhost:9292/faye")
			Net::HTTP.post_form(uri, :message => message.to_json)
		end
	end
	
Pelo fato de estarmos usando a biblioteca **Net:HTTP**, precisamos requerer ela em algum lugar do aplicativo rails. Um bom lugar para se fazer isso é no arquivo ```config/application.rb```.

	require "rails/all"
	require "net/http"
	
Por ultimo temos que fazer uma mudança no arquivo ```app/assets/javascripts/application.js```. Em vez de um alerta, o navegador precisa executar o codigo que for recebido do servidor **faye**:

	$(function() {
		var faye = new Faye.Client('http://localhost:9292/faye');
		faye.subscribe("messages/new", function(data){
			eval(data);
		});
	});
	
Agora ao enviar uma mensagem no formulário da página, todas as instancias abertas da página iram receber a modificação instantaneamente.

## Segurança

É necessário criar um protocolo de segurança, se não qualquer um pode tentar enviar uma mensagem via **curl** para o servidor e rodar *javascript* em todos os clientes que estnao subscrevendo ao sistema do **Faye**.

Para resolver isso, precisamos criar uma extenção no servidor **Faye** para lidar com a segurança. Veja mais informações [aqui](http://faye.jcolan.com/ruby.html).

Basicamente vamos criar uma nova classe que vai conter funções para mensagens entrando e saindo do servidor. Cada uma dessas funções vai fazer um cheque e se algo estiver errado a função levanta um *erro*. Depois criamos uma instancia dessa classe e adicionamos ela como uma extenção do servidor **Faye**.

Primeiro vamos criar um **token** que vai ser enviado junto com as mensagens pelo servidor. Se a mensagem não conter o **token**, o servidor vai levantar um erro.

No arquivo ```config/application.yml``` adicione uma nova **env variable**:

	FAYE_SECRET_OKEN: 12345678901234567890

Depois precisamos modificar a função **broadcast** em ```app/helpers/application_helper.rb```:

	module ApplicationHelper
		def broadcast(channel, &block)
			message = { :channel => channel, :data => capture(&block), ext => {:auth_token => env['FAYE_SECRET_OKEN']}}
			uri = URI.parse("http://localhost:9292/faye")
			Net::HTTP.post_form(uri, :message => message.to_json)
		end
	end

Por último, no arquivo ```faye.ru``` precisamos criar a classe que vai lidar com a autenticação:

	require 'faye'
	
	class ServerAuth
		def incoming(message, callback)
			if message['channel'] !~ %r{^/meta/}
				if message['ext']['auth_token'] != env['FAYE_SECRET_OKEN']
					message['error'] = 'Invalid authentication token'
				end
			end
			callback.call(message)
		end
	end
	
	faye_server = Faye::RackAdapter.new(:mount => 'faye', :timeout => 45)
	faye_server.add_extension(ServerAuth.new)
	run faye_server
	
Ao reiniciar o servidor e tentar enviar uma nova mensagem via **curl** sem o token de autenticação, vamos receber um erro:

	$ curl http://localhost:9292/faye -d 'message={"channel":"/messages/new", "data":"hello" }
	HTTP/1.1 400 Bad Request
	Content-Type: application/json
	Connection: close
	Server: thin 1.2.11 codename Bat-Shit Crazy
	content-Length: 11
	
Mas ao enviar a mensagem atravez da página em rails, a autenticação vai funcionar e a mensagem vai ser recebida por todos que estão subscrevendo a página.

## Outros recursos



-----------------

[Index](index.md)

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
