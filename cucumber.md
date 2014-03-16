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

3- Execute o gerador para iniciar o cucumber em um projeto rails:

```$ rails generate cucumber:install```

Uma pasta chamada ```app/features``` foi criada.

4- Para rodar o cucumber, utilize o comando rake: ```$ rake cucumber```

cucumber features -n

### Features

1. Cenario
2. Feature
3. Steps
4. Paths