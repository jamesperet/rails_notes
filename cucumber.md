# Cucumber

Behavior driven development

### Instalação

1- no arquivo ```Gemfile```:

	group :test do
	  gem "rspec"
	  gem "rspec-rails"
	  gem "webrat"
	  gem 'cucumber-rails'
	  gem 'database_cleaner'
	end

2- execute o comando ```$ bundle install```.

3- Execute o comando ``rake db:test:clone`` para criar as tabelas no banco de dados de testes.

4- Execute o gerador para iniciar o cucumber em um projeto rails:

``rails generate cucumber:install``

``rails generate rspec:install``

Uma pasta chamada ```app/features``` foi criada.

5- Para rodar o cucumber, utilize o comando rake: ```$ cucumber features -n```


### Features

1. Cenario
2. Feature
3. Steps
4. Background
5. Paths
6. Factories