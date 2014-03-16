# Twitter Bootstrap

O gem **twitter-bootstrap-rails** serve para simplificar o processo de utilizar o [Twitter Bootstrap Framework](http://twitter.github.com/bootstrap/) com o Rails.

Por *default* o Rails utiliza o **SASS** como **pre-processador** de CSS. O Twitter Bootstrap utiliza o **Less** como pré-processador.

O gem **twitter-bootstrap-rails** instala o pré-processador de Less no rails alem de disponibilizar varios comandos para gerar layouts rapidamente.

Para começar, modifique o arquivo ```Gemfile```:
<pre class="prettyprint languague-ruby">
group :assets do
	gem "therubyracer"
	gem "less-rails" #Sprockets (what Rails 3.1 uses for its asset pipeline) supports LESS
	gem 'twitter-bootstrap-rails'
end
</pre>

O **gem** do **twitter-bootstrap** é usado apenas como um asset, por causa do **Less** que vai compilar o **.css**, e não é necessario no enviorment de produção.

Execute o comando a seguir para instalar o gem do twitter-bootstrap-rails e alguns outros gems como o Less.

```$ bundle install```

Em seguida instale o twitter-bootstrap-rails gem:

```$ rails g bootstrap:install```

Esse comando vai instalar o bootstrap, que vai criar os arquivos de css na pasta de layouts.

Com tudo instalado, é possivel utilizar o comando a seguir para criar layouts rapidamente:

```$ rails g bootstrap:layout application fixed```

Esse comando gera um arquivo de layout com todas as configurações do twitter-bootstrap. É possivel criar um layout **fixed** ou **fluid**.

Também é possivel usar o comando de **scaffold** para gerar o CRUD de um modelo e depois rodar um comando para estiliza-lo com o twitter-bootstrap. Primeiro gere o **scafold**:

```$ rails g scaffold product name price:decimal --skip-stylesheets```

Depois faça a migração:

```$ rake db:migrate```

Em seguida re-crie o layout com o coamndo:

```$ rails g bootstrap:themed products -f```

Para esse comando funcionar é preciso especificar o nome do modelo gerado pelo **scaffold**, neste caso "products" e é preciso passar a opção **-f** para sobre-escrever os arquivos de layout gerados anteriormente.

#### body padding-top

Outro detalhe para resolver depois, caso necessario, é colocar o um **padding** no body para criar um espaço no layout entre a navbar e o conteudo. O gem do twitter-bootstrap-rails cria um arquivo para modificar o css padrão: ```app/assets/stylesheets/bootstrap_and_override.css.less```

<pre class="prettyprint languague-less">
@import "twitter/bootstrap/bootstrap";

body { padding-top: 60px; }

@import "twitter/bootstrap/responsive";
</pre>

#### Flash Messages

Para criar mensagens de sucesso e erro ao editar os formularios, utilize este codigo no View desejado ou no ```layouts/application.html.erb```

	<% flash.each do |name, msg| %>
	  <div class="alert alert-<%= name == :notice ? "success" : "error" %>">
	    <a class="close" data-dismiss="alert">×</a>
	    <%= msg %>
	  </div>
	<% end %>

Esse codigo vai loopar entre as **flash messages** e escreve-las de acordo com as classes do twitter bootstrap.

#### Validação com SimpleForm

Primeiro crie a validação no **modelo**:

<pre class="prettyprint languague-ruby">
class Product < ActiveRecord::Base
	validates_presence_of :name, :price
end
</pre>

Adicione o SimpleForm no ```Gemfile```:

<pre class="prettyprint languague-ruby">
gem 'simple_form'
</pre>

Depois execute o comando para instalar o SimpleForm no projeto:

```$ rails g simple_form:install --bootstrap```

Agora mude o codigo do formulario no arquivo ```_form.html.erb```:

	<%= simple_form_for @product, :html => { :class => 'form-horizontal' } do |f| %>
	  <fieldset>
	    <legend><%= controller.action_name.capitalize %> Product</legend>
	
	    <%= f.input :name %>
	    <%= f.input :price %>
	
	    <div class="form-actions">
	      <%= f.submit nil, :class => 'btn btn-primary' %>
	      <%= link_to 'Cancel', products_path, :class => 'btn' %>
	    </div>
	  </fieldset>
	<% end %>

#### Customizando a importação de arquivos

Escolha exatamente quais compontentes do Twitter Bootstrap vão ser incluidos no sistema da seguinte forma:

Para mudar o CSS, retire as seguintes linhas no arquivo ```app/assets/stylesheets/bootstrap_and_override.css.less```

<pre class="prettyprint languague-less">
@import "twitter/bootstrap/bootstrap";
@import "twitter/bootstrap/responsive";
</pre>

E substitua com todos os parametros do bootstrap:

<pre class="prettyprint languague-less">
// CSS Reset
@import "reset.less";

// Core variables and mixins
@import "variables.less"; // Modify this for custom colors, font-sizes, etc
@import "mixins.less";

// Grid system and page structure
@import "scaffolding.less";
@import "grid.less";
@import "layouts.less";

// Base CSS
@import "type.less";
@import "code.less";
@import "forms.less";
@import "tables.less";

// Components: common
@import "sprites.less";
@import "dropdowns.less";
@import "wells.less";
@import "component-animations.less";
@import "close.less";

// Components: Buttons & Alerts
@import "buttons.less";
@import "button-groups.less";
@import "alerts.less"; // Note: alerts share common CSS with buttons and thus have styles in buttons.less

// Components: Nav
@import "navs.less";
@import "navbar.less";
@import "breadcrumbs.less";
@import "pagination.less";
@import "pager.less";

// Components: Popovers
@import "modals.less";
@import "tooltip.less";
@import "popovers.less";

// Components: Misc
@import "thumbnails.less";
@import "labels.less";
@import "progress-bars.less";
@import "accordion.less";
@import "carousel.less";
@import "hero-unit.less";

// Utility classes
@import "utilities.less"; // Has to be last to override when necessary

body { padding-top: 60px; }

@import "twitter/bootstrap/responsive";

// Set the correct sprite paths
@iconSpritePath: asset-path('twitter/bootstrap/glyphicons-halflings.png');
@iconWhiteSpritePath: asset-path('twitter/bootstrap/glyphicons-halflings-white.png');

@navbarBackground: #555;
@navbarBackgroundHighlight: #888;
@navbarText: #eee;
@navbarLinkColor: #eee;

.navbar .brand {
  color: #FAFFB8;
}
</pre>

Para modificar os arquivos javascript, modifique o arquivo ```app/assets/javascripts/application.js``` para incluir os scripts desejados:

<pre class="prettyprint languague-javascript">
//= require twitter/bootstrap/bootstrap-transition
//= require twitter/bootstrap/bootstrap-alert
//= require twitter/bootstrap/bootstrap-modal
//= require twitter/bootstrap/bootstrap-dropdown
//= require twitter/bootstrap/bootstrap-scrollspy
//= require twitter/bootstrap/bootstrap-tab
//= require twitter/bootstrap/bootstrap-tooltip
//= require twitter/bootstrap/bootstrap-popover
//= require twitter/bootstrap/bootstrap-button
//= require twitter/bootstrap/bootstrap-collapse
//= require twitter/bootstrap/bootstrap-carousel
//= require twitter/bootstrap/bootstrap-typeahead
</pre>

#### Links

- [Twitter Bootstrap Framework](http://twitter.github.com/bootstrap/)
- [twitter-bootstrap-rails no GitHub](https://github.com/seyhunak/twitter-bootstrap-rails)
- [SimpleForm 2.0 + Bootstrap](http://blog.plataformatec.com.br/2012/02/simpleform-2-0-bootstrap-for-you-with-love/)
- [variables.less](https://github.com/twitter/bootstrap/blob/master/less/variables.less)
- [RailsCasts - Twitter bootstrap basic](http://railscasts.com/episodes/328-twitter-bootstrap-basics)
- [RailsCasts - More on Twitter Bootstrap (pro)](http://railscasts.com/episodes/329-more-on-twitter-bootstrap)

---

<a class="btn btn-mini" href="readme.md">voltar</a>
 