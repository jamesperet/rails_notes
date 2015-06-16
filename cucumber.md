# Cucumber

*Behavior driven development*

O Cucumber é um framework de alto nivel para criação de testes baseado em historias de interações de usuários. Sua grande vantagem é que os testes são escritos em uma lingua comum como o inglês (ou outras linguas utilizando plugins).

## Instalação

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


## Como funciona

1. Cenario
2. Feature
3. Steps
4. Background
5. Paths
6. Factories

#### Links

* [RailsCast Cucumber Tutorial](https://www.evernote.com/l/AALzqmI0XeBD7rP9wL83vkSYnKMwjdSvduA) [(local version)](evernote:///view/124245/s2/f3aa6234-5de0-43ee-b3fd-c0bf37be4498/f3aa6234-5de0-43ee-b3fd-c0bf37be4498/)

-----------------

[Index](index.md)

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
