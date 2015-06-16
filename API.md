# API

Neste documento vou explicar como criar uma **API** (Application Programming Interface) para um aplicativo **Rails**. Vou começar criando uma **API** simples, depois mostrando como criar versões dela, depois vou mostrar como criar um sistema de tokens para adicionar segurança e por ultimo vou criar um sistema de autenticação de usúarios via oAuth.

## Criando uma API basica

Em Qualquer controlador do rails, é possivel criar uma resposta em **JSON** além da resposta em **HTML**.

<pre><code class="ruby">
class ProductsController < ApplicationController
	def index
		@products = Products.all
		respond_to do |format|
			format.html
			format.json { render json @products }
		end
	end
end
</code></pre>

Ao entrar na página ```http://localhost:300/products.json```, você vai receber um arquivo em **JSON** com o conteúdo dos produtos.

## Criando rotas para a API

Para criar uma rota como ```http://localhost:3000/api```, modifique o arquivo ```config/rountes.rb```

	Store::Application.routes.draw do
		namespace :api do
			# /api/...   API::
			namespace :api, defaults: {format: 'json'} do
				namespace :v1 do
					resources :products
				end
				namespace :v2 do
					resources :products
				end
			end
		end
		
		resources :products
		root to: 'products#index'
	end

## O Controlador da API

Cria o arquivo ```/app/controllers/api/v1/products_controller.rb```:

	module Api
		module V1
			class ProductsController < ApplicationController # ou API::BaseController
				
				# Hacks para que certos atributos continuem `backword compatiple`
				# Aqui estamos modificando a funcionalidade da classe neste controlador
				class Product < ::Product
					def as_json(options = {})
						super.merge(released_on released_at.to_date)
					end
				end
				
				respond_to :json
				
				def index
					respond_with Product.all
				end
				
				def show
					respond_with Product.find(params[:id])
				end
				
				def create
					respond_with Product.cerate(params[:product])
				end
				
				def update
					respond_with Product.update(params[:id], params[:product])
				end
				
				def destroy
					respond_with Product.destroy(params[:id])
				end
			end
		end
	end

## Usando accept headers para versão da API

Em vez de usar um endereço como ```http://localhost:3000/api/v1/```, alguns sites como o [github](http://github.com) usam o **accept header** para especificar a versão do API.

Para configurar as rotas para ceitar o **accept header**, modifique o arquivo ```config/rountes.rb```

	require 'api_constraints'

	Store::Application.routes.draw do
		namespace :api, defaults: {format: 'json'} do
			scope module: :v1, constraints: ApiContraints.new(version: 1) do
				resources :products
			end
			scope module: :v2, constraints: ApiConstraints.new(version: 2, default: true) do
				resources :products
			end
		end
		
		resources :products
		root to: 'products#index'
	end

Depois no arquivo ```lib/api_constraints.rb```:

	class ApiConstraints
		def initialize(options)
			@version = options[:version]
			@default = options[:default]
		end
		
		def matches?(req)
			@default || req.headers['Accept'].include?("application/vnd.example.v#{@version}")
		end
	end
	
Agora ao visitar a url ```http://localhost:3000/api/products```, você vai acessar o **API** v2 e receber o arquivo em **JSON**.

Para acessar outra versão da **API** via *curl* no *terminal*:

``` curl -H 'Accept: application.vnd.example.v1' http://localhost:3000/api.products ```

## Autenticação basica

O jeito mas simples de restringir acesso ao **API** é usando a autenticação basica provida pelo **rails**.


No arquivo ```/app/controllers/api/v1/products_controller.rb```:

	module Api
		module V1
			class ProductsController < ApplicationController
				http_basic_authenticate_with name: "admin", password:  "secret"
				respond_to :json
				...

Para testar, no terminal, digite o comando:

<pre><code class="bash">
	$ curl http://localhost:3000/api/products
	HTTP Basic: Access denied.
	
	$ curl http://localhost:3000/api/products -I
	HTTP/1.1 401 Unauthorized
	WWW-Authenticate: Basic realm="Application"
	Content-Type: text/html; charset=utf-8
	...
	
	$ curl http://localhost:3000/api/products -u 'admin:secret'
	JSON response
	...
</code></pre>

## Access Token

Em vez de usar um sistema de autenticação, podemos restringir acesso a API requerindo um **token de acesso**. Se na chamada para o API o cliente não enviar o **token**, o acesso será negado.

Primeiro vamos criar um modelo para guardar os **tokens**. Execute o comando:

 ```rails generate model api_key access_token```
 
É possivel tb adicionar outras colunas como ```user_id``` para guardar o dono do **token**, ou ```expires_at``` para destruir chaves antigas ou a colona ```role``` para adicionar permissões diferentes para cada **token**.
 
Em seguida vamos modificar o modelo em ```app/models/api_key.rb```. Quando um novo *api_key* for criado, ele vai gerar um número randomico unico para está chave.

	class ApiKey < ActiveRecord::Base
		before_create :generate_access_token
		
	private
	
		def generate_access_token
			begin
				self.access_token = SecureRandom.hex
			end while self.class.exists?(access_token: access_token)
		end
	end
	
Para testar, entre no [console](rails_console.md) e digite o comando: ```ApiKey.create!```. Isso vai gerar um novo **access_token** com um número hexadecimal unico.

Agora vamos restringir o acesso ao **API** modificando o controlador. No arquivo ```/app/controllers/api/v1/products_controller.rb```:

	module Api
		module V1
			class ProductsController < ApplicationController
				before_filter :restrict_access
				respond_to :json
				
				...
				
			private
			
				def restrict_access
					api_key = ApiKey.find_by_access_token(params[:access_token])
					head :unauthorized unless api_key
				end
			end
		end
	end


Para acessar o **API** com o **access_token**, podemos utilizar a seguinte url:

```http://localhost:3000/api/products?access_token=c576f0136149a2e2d9127b3901015545```

Este metodo não é muito seguro, por que as pessoas podem copiar e colar os codigos de acesso junto com a url sem querer. Para melhorar este exemplo, vamos passar o **access_token** atravez de um parametro no *head* do *request* para o servidor.

Vamos mudar de novo o arquivo ```/app/controllers/api/v1/products_controller.rb```:

	def restrict_access
		authenticate_or_request_with_http_token do |token, options|
			ApiKey.exists?(access_token: token)
		end
	end
	
Para testar, no terminal vamos accessar de novo a url do **API** mas desta vez passando o **access_token** junto com o *header*:

```curl http://localhost:3000/api/products -H 'Authorization: Token token="c576f0136149a2e2d9127b3901015545"'```

## Autenticação de usuários com OAuth

A questão das soluções anteriores é que estamos criando um sistema de autenticação global. Não temos como saber exatamente qual usuário está acessando a **API**. Então se você necessista de autenticação de usuário via o **API**, o jeito mais comum para fazer isso é utilizando o [OAuth2](http://oauth.net/2/).

> **OAauth** é um protocolo aberto para autorização segura de um **API** de um jeito simples e comum para aplicativos *web*, *mobile* e *desktop*.

O protocolo **OAuth** foi criado pela equipe do twitter e várias outras empresas adotaram e usam o protocolo como o Google, Facebook e GitHub. Ele é utilizado para que um aplicavo possa acessar informações de outro aplicativo através da autorização de um usúario.

O *gem* [doorkeeper](https://github.com/doorkeeper-gem/doorkeeper) serve para simplificar a criação de um serviço de **OAuth** em um aplicativo *rails*.

### Configurando o DoorKeeper

Primeiro, mude para ```false``` a configuração ```whitelist_attributes``` no arquivo ```config/application.rb```:

	config_active_record.whitelist_attributes = false
	
Depois no ```gemfile``` adicione no fim:

	gem 'doorkeeper'
	
Execute o comando ```bundle install``` para instalar o *gem* e depois rode os comandos para instalar o **doorkeeper** e migrar o banco de dados:

	rails g doorkeeper:install
	rake db:migrate
	
Varias novos arquivos e tabelas no banco de dados vão ser criados. Em seguida modifique o arquivo ```config/initializers/doorkeeper.rb```:

	DoorKeeper.configure do
		
		resource_owner_authenticator do |routes|
			#raise "Please configure doorkeeper"
			# Put your resource owner authentication logic here
			# If you want to use named routes from your app you need
			# to call them on routes object eg.
			# routes.new_user_session_path
			User.find_by_id(session[:user_id]) || redirect_to(routes.new_user_session_url)
		end
		
		...


### Repositorios

* [doorkeeper](https://github.com/doorkeeper-gem/doorkeeper) - Doorkeeper is an OAuth 2 provider for Rails and Grape
* [ionic-doorkeeper](https://github.com/emilsoman/ionic-doorkeeper) - Sample ionic app that authenticates with a Rails API protected with Doorkeeper

### Links

* [OAuth Implicit Grant with Grape, Doorkeeper and AngularJS](http://codetunes.com/2014/oauth-implicit-grant-with-grape-doorkeeper-and-angularjs/)
* [How can I pre-authorize a client app for my user on my oauth provider that uses doorkeeper?](http://stackoverflow.com/questions/11129676/how-can-i-pre-authorize-a-client-app-for-my-user-on-my-oauth-provider-that-uses)
* [Using Resource Owner Password Credentials flow](https://github.com/doorkeeper-gem/doorkeeper/wiki/Using-Resource-Owner-Password-Credentials-flow)
* [Doorkeeper + Devise](http://morodeercoding.blogspot.ru/)
* [The OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749)


## Criando um API com o Grape gem

### Repositorios

* [grape](https://github.com/intridea/grape) - An opinionated micro-framework for creating REST-like APIs in Ruby.
* [wine_bouncer](https://github.com/antek-drzewiecki/wine_bouncer) - A Ruby gem that allows Oauth2 protection with Doorkeeper for Grape Api's
* [grape-doorkeeper](https://github.com/fuCtor/grape-doorkeeper) - Integration Grape with Doorkeeper* 

### Links

* [Build Great APIS with Grape](http://www.sitepoint.com/build-great-apis-grape/)
* [Introduction to building APIs with Grape](http://codetunes.com/2014/introduction-to-building-apis-with-grape/)
* [Building RESTful API using Grape in Rails](http://funonrails.com/2014/03/building-restful-api-using-grape-in-rails/)
* [OAuth 2.0 Tutorial: Protect Grape API with Doorkeeper](http://blog.yorkxin.org/posts/2013/11/05/oauth2-tutorial-grape-api-doorkeeper-en/)
* [authenticate grape api with doorkeeper](http://stackoverflow.com/questions/26472217/authenticate-grape-api-with-doorkeeper)
* [Grape API authentication using Devise Auth Token](http://funonrails.com/2014/03/api-authentication-using-devise-token/)

## Autenticação apenas com o Devise

### Links

* [APIs with Devise](http://soryy.com/blog/2014/apis-with-devise/)
* [API JSON authentication with Devise](https://gist.github.com/jwo/1255275)

## API Reference

* [A Well Designed API Approach](https://www.airpair.com/rest/posts/a-well-designed-api-approach)



-----------------

[Index](index.md)

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
