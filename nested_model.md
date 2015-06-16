# Nested Model

Rails **gem** para simplificar o manuseio de varios modelos associados em um único formulario. Ele faz isso de um jeito discreto utilizando *jQuery*.

## Instalação

Adicione o **gem** no ```gemfile``` e rode o comando ```$ bundle install```:

	gem "nested_form"
	
Adicione o **gem** ao *Asset Pipeline* no arquivo ```application.js```:

	//= require jquery_nested_form
	
Se você não está utilizando o *Asset Pipeline*, rode o seguinte comando para instalar o **nested_form**:

```$ rails g nested_form:install```

Inclua o *JavaScript* gerado no layout:

	<%= javascript_include_tag :defaults, "nested_form" %> 
	
## Utilização

Escrever...

-----------------

## Informações adicionais

- [Working with nested forms and a many-to-many association in Rails 4](http://www.createdbypete.com/articles/working-with-nested-forms-and-a-many-to-many-association-in-rails-4/)
- [Dynamic Nested Forms in Rails 3 (madebydna blog)](http://blog.madebydna.com/all/code/2010/10/07/dynamic-nested-froms-with-the-nested-form-gem.html)
- [ActiveRecord nested attributes](http://api.rubyonrails.org/classes/ActiveRecord/NestedAttributes/ClassMethods.html#method-i-accepts_nested_attributes_for)

## Recursos

- [ryanb/nested_form](https://github.com/ryanb/nested_form) - ruby gem para simplificar o processo de criação de nested forms 


-----------------

[Index](index.md)

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
