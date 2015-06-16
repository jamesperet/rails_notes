# Console

## Configurações

O arquivo ```~/.irbrc``` serve para configurar o console. Ele fica no root da pasta de usuário no sistema operacional. Esse arquivo funciona como um arquvo de *.rb* comum e vai ser rodado toda vez que o IRB iniciar.

Para editar ou criar esse arquivo, digite no terminal:

```pico ~/.irbrc```

Para sair do editor **pico** aperte **ctrl + x** para fechar o arquivo e confirme que quer salvar e o nome do arquivo apertando **enter**.

## Truques

### HIRB

O HIRB serve para melhorar a visualização do *ActiveRecord*. Ele muda o jeito como o IRB mostra os dados retornados para o usuário, criando uma tabela que pode ser customizada.

Muito util para poder visualizar os dados do que você está fazendo no console do rails e não ficar precisando caçar as informações perdidas dentro de um *hash* gigante.

#### Instalação

Adicione ao ```/Gemfile```:

	  gem 'hirb'
	  
Depois adicione ao arquivo ```~/.irbrc``` as seguintes configurações: 

	require 'hirb'
	Hirb.enable 

### Wirble

O wirble é um **gem** que serve para melhorar o *console* do rails.

#### Instalação

Adicione ao ```/Gemfile```:

	  gem 'wirble'
	
Depois adicione ao arquivo ```~/.irbrc``` as seguintes configurações:

	begin
	  require 'wirble'
	  # init wirble
	  Wirble.init
	  # enable color
	  Wirble.colorize
	  Wirble::Colorize::DEFAULT_COLORS
	rescue LoadError => err
	  $stderr.puts "Couldn't load Wirble: #{err}"
	end
	
#### Links

* [Mais informações](http://pablotron.org/software/wirble/)
* [Readme.md](http://pablotron.org/software/wirble/README)

-----------------

[Index](index.md)

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
