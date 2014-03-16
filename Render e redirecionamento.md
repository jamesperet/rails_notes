# Render e redirecionamento 
O **Controlador** tem o papel de definir qual ação vai ser tomada e no final ele deve renderizar um **"View"** ou redirecionar para uma **URL** ou outro **controlador**.

### Exemplo

	Class DemoController < ApplicationController

		def index
			render('hello')
		end

		def hello
			render(:text => 'Hello World!')
		end

		def other_hello
			redirect_to(:action => 'hello')`
		end

		def my_url
			redirect_to("http://jamesperet.com")
		end

	end

Neste exemplo, ao entrar na página ```demo/index```, o browser irá carregar a página ```demo/hello```, mas a URL no browser continua sendo a do "index". 

Ao entrar na página ```demo/hello``` diretamente, o rails retorna apenas o texto "Hello World".

Ao entrar na página ```demo/other_hello``` o browser é redirecionado para o controllador "hello", que vai apenas renderizar texto e a URL no browser será a da página "hello".

### Sintaxe das funções para render e redirecionamento 

* Default: a página de template (view) tem o mesmo nome que a ação que está sendo chamada.
* ```render (:action => 'hello')``` - função depreciada.
* ```render (:template => 'demo/hello')```  - função depreciada.
* ```render ('demo/hello')```  - Só é necessario especificar o controlador se ele for diferente do atual.
* ```render ('hello')``` - Metodo correto para renderizar o template de um View.
* ```render (:text => 'Hello World')``` - renderiza apenas texto.
* ```redirect_to(:action => 'other_hello')``` - redireciona o browser para outro controlador.
* ```redirect_to("http://jamesperet.com")``` - Redireciona o browser para uma URL externa.

<a class="btn btn-mini" href="readme.md">voltar</a>