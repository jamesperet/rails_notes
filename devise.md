# Devise

O **devise** é um plugin que adiciona a funcionalidade de login para um projeto. Ele resolve todos os aspectos de **autenticação de usuários**, como login, logout, cadastro de novos usuários e recuperação de senhas.

#### Instalação

Primeiro adicione a linha no ```Gemfile```:
	
	gem 'devise'

Depois rode o comando ```$ bundle install``` para instalar o **gem**.

Inicie o devise no projeto:

```$ rails g devise:install```

Esse comando instala o **devise** no projeto e mostra algumas instruções para faze-lo funcionar.

Adicione a seguinte linha no arquivo ```config/environments/development.rb``` para configurar o sistema de email:

	# and don't forget the other environments
	config.action_mailer.default_url_options = { :host => 'localhost:3000' }


Depois é necessario criar um modelo para os usuários:

```$ rails generate devise user```

Note que se já existe um modelo ```user```, o devise vai apenas adicionar a funcionalidade ao modelo existente.

Execute depois o comando ```$ rake db:migrate``` para executar a migração.

Agora é necessario adicionar no **View** links para poder fazer o login e o logout. Adicione o seguinte codigo no arquivo ```layouts/application.html.erb```.

	<% if user_signed_in? %>
	  Logged in as <strong><%= current_user.email %></strong>.
	  <%= link_to 'Edit profile', edit_user_registration_path %> |
	  <%= link_to "Logout", destroy_user_session_path, method: :delete %>
	<% else %>
	  <%= link_to "Sign up", new_user_registration_path %> |
	  <%= link_to "Login", new_user_session_path %>
	<% end %>

Com isso é possivel ter um sistema de autenticação de usuarios basico, funcional em apenas alguns passos simples.

#### Rotas

Para mudar as rotas das páginas de sign_in e sign_out, crie uma rota no arquivo ```config/routes.rb```.

	devise_for :users, path_names: {sign_in: "login", sign_out: "logout"}

É necessario ter uma rota **root** para que o devise funcione direito.

	root :to => "home#index"

#### Mudando o layout do devise

Por *default*, o devise esconde os arquivos do **view**. Rode o comando para gera-los no projeto:

```$ rails g devise:views```

Esse comando irá gerar os varios arquivos que compõe o **view** na pasta ```app/views/devise/```.

#### Mudando os parametros de login

Para utilizar outros paramentros como por exemplo usar o **username** em vez do **email**, é necessario criar uma variavel no banco de dados e alterar algumas variavéis de configuração.

Primeiro rode o comando:

```$ rails g migration add_username_to_users username```

Depois no arquivo ```config/initializers/devise.rb``` modifique as variaveis:

	config.authentication_keys = [ :username ]
	config.case_insensitive_keys = [ :username ]
	config.strip_whitespace_keys = [ :username ]

No arquivo do modelo ```app/models/user.rb```, adicione o ```attr_accessible :username``` e valide as presença dos dados por que o **devise** não valida atributos customizados: ```validates_presence_of :attribute, :on => :create, :message => "can't be blank"```

Além disso vai ser necessario mudar os parametros utilizados no **View**.

#### Restringindo acesso

Para limitar o acesso a certas páginas a apenas usuários loggados, adicione a seguinte linha no controlador que controla as páginas desejadas (ex: ```app/controllers/posts_controller.rb```).

	before_filter :authenticate_user!, except: [:index, :show]

#### Helpers

Verificar se o usuário está logado:

	user_signed_in?

Objeto com o atual usuário logado:

	current_user

#### Custom Layouts

Para definir um layout diferente do que o padrão para a classe do **devise**, modifique o arquivo ```app/controllers/appication_controller.rb``` adicionando uma função para determinar o layout a ser usado:

	class ApplicationController < ActionController::Base
	  layout :layout_by_resource
	
	  protected
	
	  def layout_by_resource
	    if devise_controller?
	      "layout_name_for_devise"
	    else
	      "application"
	    end
	  end
	end

#### Links

- [Devise (GitHub)](https://github.com/plataformatec/devise/)
- [RailsCats - Devise(revised)](https://github.com/plataformatec/devise/)

---

<a class="btn btn-mini" href="readme.md">voltar</a>


 
