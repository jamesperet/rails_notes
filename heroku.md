# Heroku Server

### Configurando o Heroku Toolbelt e chaves SSH

Primeiro faça o download do Heroku toolbelt e instale no computador. Depois rode o seguinte comandos para logar no heroku:

	heroku login

Apos fazer o login, se ocorrer algum problema de autenticação, é por que as chaves SSH não estão criadas ou atualizadas. Para cria-las, entre na pasta e execute os comandos:

	cd ~/.ssh)
	ssh-keygen -t rsa -f id_rsa
	heroku keys:add "id_rsa.pub"

### Configuração do aplicativo

Coloque o seguinte codigo no arquivo ```config/application.rb```:

	config.assets.initialize_on_precompile = false

Depois modifique o arquivo ```config/enviorments/production.rb```:

	  config.assets.compile = true

### Criando um applicativo no servidor do Heroku

	$ heroku apps:create example
	Creating example... done, stack is cedar
	http://example.herokuapp.com/ | git@heroku.com:example.git

### Mudando o remote de um projeto git

	git remote rm heroku
	git remote add heroku git@heroku.com:yourappname.git

### Populate the database

	heroku db:push

Or just use the ```db/seeds.db``` file.



