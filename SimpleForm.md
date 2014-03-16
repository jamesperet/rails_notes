# SimpleForm

O **SimpleForm** é um **gem** que simplifica drasticamente a criação de formularios.

Para utiliza-lo, adicione no o SimpleForm no ```Gemfile```

<pre class="prettyprint languague-ruby">
gem 'simple_form'
</pre>

Depois execute o comando para instalar o **gem**:

```$ bundle install```

Para iniciar o SimpleForm no projeto, execute o comando:

```$ rails g simple_form:install```

Esse comando ira criar alguns arquivos no projeto e instalar o SimpleForm. Também é possivel passar a variavel ```--bootstrap``` para inicializar o SimpleForm preparado para o Twitter Bootstrap.

Para utilizar o Simple Form, em vez de usar a função ```form_for```, vamos usar a função ```simple_form_for```.

#### Exemplo de formulário com o SimpleForm
<pre class="prettyprint languague-erb">
<%= simple_form_for(@product) do |f| %>
  <%= f.error_notification %>
  <%= f.input :name %>
  <%= f.input :price, hint: "price should be in USD" %>
  <%= f.input :released_on, label: "Release Date" %>
  <%= f.input :discontinued %>
  <%= f.input :rating, collection: 1..5, as: :radio_buttons %>
  <%= f.association :publisher %>
  <%= f.association :categories, as: :check_boxes %>
  <%= f.error :base %>
  <%= f.button :submit %>
<% end %>
</pre>
#### Funções

- **input**<br>
Input padrão que vai ser definido pelo tipo de variavel definida no modelo.
<br>Exemplo:```f.input :name```

- **association**<br>
Utilizado para associações. Automaticamente coloca as associações possiveis na lista do formulario.
Exemplo: ```f.association :categories```

- **button**<br>
Utilizado para enviar o formulario.
<br>Exemplo: ```f.button :submit```

- **error_notification**<br>
Retorna as *mensagens flash* de erro e sucesso.
<br>Exemplo: ```f.error_notification```

- **error**<br>
Retorna o erro de validação de um campo especifico.
<br>Exemplo: ```f.error :username, :id => 'user_name_error'```

#### Variaveis

- ```:hint => 'preço em reais'```
- ```:label => 'Nome completo'```
- ```:placeholder => 'exemple@email.com'```
- ```:collection => 1..5```
- ```:as => radio_buttons```

#### Custom Wrappers

O **simple_form** envolve os campos do formulario em *divs*. Para criar seus proprios *wrappers*, adicione a seguinte função no arquivo ```config/initializers/simple_form.rb```:

	config.wrappers :basic, :tag => :span do |b|
	    b.use :placeholder
	    b.use :label_input
	    #component.use :hint,  :wrap_with => { :tag => :span, :class => :hint }
	    #component.use :error, :wrap_with => { :tag => :span, :class => :error }
	  end

Depois para utilizar esse *wrapper*:

	f.input :email, wrapper: "basic", label: false

#### Links

- [SimpleForm (GitHub)](https://github.com/plataformatec/simple_form)
- [RailsCasts - SimpleForm Revised](http://railscasts.com/episodes/234-simple-form-revised)

<hr><a class="btn btn-mini" href="readme.md">voltar</a>