# Nested Model

Rails **gem** para simplificar o manuseio de varios modelos associados em um único formulario. Ele faz isso de um jeito discreto utilizando *jQuery*.

#### Instalação

Adicione o **gem** no ```gemfile``` e rode o comando ```$ bundle install```:

	gem "nested_form"
	
Adicione o **gem** ao *Asset Pipeline* no arquivo ```application.js```:

	//= require jquery_nested_form
	
Se você não está utilizando o *Asset Pipeline*, rode o seguinte comando para instalar o **nested_form**:

```$ rails g nested_form:install```

Inclua o *JavaScript* gerado no layout:

	<%= javascript_include_tag :defaults, "nested_form" %> 
	
#### Utilização

#### Links

- [nested_form (GitHub)](https://github.com/ryanb/nested_form)
- [Dynamic Nested Forms in Rails 3 (madebydna blog)](http://blog.madebydna.com/all/code/2010/10/07/dynamic-nested-froms-with-the-nested-form-gem.html)