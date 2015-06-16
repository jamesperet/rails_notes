# Links e URLs

## links

Em um **View**, para criar um **link** utilize a função ```link_to```. Essa função gera uma tag ```<a>``` em HTML.

#### Exemplo
	
	<%= link_to("click here", {:action => 'index'}) %>

## URLs

Para gerar apenas uma URL, utilize o comando ```url_for```.


### Opções

* ```:anchor``` - Especifica o *anchor* na url. Pode ser usada para ir direto a um *ID* na proxima pagina.
* ```:only_path``` - Boolean que especifica se o metodo deve retornar apenas a URL ou todas as informações (protocol, host, port).
* ```:format``` - Adiciona uma extensão ao final da URL 

### Exemplos de URLs

	# http://www.example.com/posts
	url_for :controller => 'posts', :action => 'index'
	
	# http://www.example.com/posts/5
	url_for :controller => 'posts', :action => 'index', :id => 5
	
	# http://www.example.com/posts/5/edit
	url_for :controller => 'posts', :action => 'edit', :id => 5
	
	# http://www.example.com/posts.xml
	url_for :controller=>'posts', :action=>'index', :format=>:xml
	
### namespace

Se você estiver usando um *namespace*, é possivel gerar uma url do seguinte jeito:

	namespace :admin do
	  resources :products
	end
	then you can:
	
	url_for([:admin, @product])
	and:
	
	url_for([:edit, :admin, @product])

-----------------
## Informações adicionais

* [Rails routing - RailsGuides](http://guides.rubyonrails.org/routing.html)

-----------------

[Index](index.md)

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
