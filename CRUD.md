# CRUD

Acrônimo para **Create**, **Read**, **Update**, **delete**. O CRUD se refere a um conjunto de funções que que manipulam um objeto dentro do sistema. Em um aplicativo Rails, o ideal é que cada **modelo** deve ter um **controlador** com suas funções de **CRUD**.

<br>

<table class="table table-bordered">
	<thead>
	  <tr>
	    <th style="text-align: center" class="span2">CRUD</th>
	    <th style="text-align: center">Ação</th>
	    <th style="text-align: center">Descrição</th>
	  </tr>
	</thead>
	<tbody>
	  <tr>
	    <td rowspan="2"><br>CREATE</td>
	    <td style="text-align: center">new</td>
	    <td style="text-align: left; font-size: 14px;">Formulario para criar nova entrada.</td>
	  </tr>
	  <tr>
	    <td style="text-align: center">create</td>
	    <td style="text-align: left; font-size: 14px;">Processar a nova entrada.</td>
	  </tr>
	  <tr>
	    <td rowspan="2"><br>READ</td>
	    <td style="text-align: center">list</td>
	    <td style="text-align: left; font-size: 14px;">Listar as entradas.</td>
	  </tr>
	  <tr>
	    <td style="text-align: center">show</td>
	    <td style="text-align: left; font-size: 14px;">Mostrar uma entrada unica.</td>
	  </tr>
	  <tr>
	    <td rowspan="2"><br>UPDATE</td>
	    <td style="text-align: center">edit</td>
	    <td style="text-align: left; font-size: 14px;">Formulario para editar uma entrada.</td>
	  </tr>
	  <tr>
	    <td style="text-align: center">update</td>
	    <td style="text-align: left; font-size: 14px;">Processar a edição de uma entrada.</td>
	  </tr>
	  <tr>
	    <td rowspan="2"><br>DELETE</td>
	    <td style="text-align: center">delete</td>
	    <td style="text-align: left; font-size: 14px;">Formulario para deletar uma entrada.</td>
	  </tr>
	  <tr>
	    <td style="text-align: center">destroy</td>
	    <td style="text-align: left; font-size: 14px;">Processar a ação de deletar uma entrada.</td>
	  </tr>
	</tbody>
</table>

O **CRUD** normalmente vai dentro de um **Controlador** que tem um nome em plural e está linkado a apenas um **modelo**.

Ele também ajuda a criar URLs limpas e simples:

- paginas/nova
- fotos/deletar
- usuario/editar

## Read

### List

Para criar uma função basica para listar todos os objetos de um modelo, primeiro precisamos criar um metodo no **controlador**. Atravez das convenções do Rails, o nome desse metodo vai ser igual ao nome do template do **View**. Precisamos carregar também uma **instance variable** no controlador com a lista de objetos. Essa variavel vai estar disponivel no View.

#### Exemplo

1- Levando em consideração que o modelo **Paginas** foi criado e tem o paramentro **titulo**, alem de entradas no banco de dados.

2- Gere um controlador: ```$ rails generate controller paginas```

3- No novo arquivo do controlador ```app/controllers/paginas_controller.rb```:

	class PaginasController < ApplicationController
		def lista
			 @paginas = Pagina.order("paginas.titulo ASC")
		edn
	end

4- Crie o arquivo ```app/views/paginas/lista.html.erb```

	<h1>Lista de páginas:</h1>
	<ul>
		<% @paginas.each do |pagina| %>
			<li>
				<%= link_to(pagina.titulo {:action =>'mostrar', :id => pagina.id}, :class => 'link') %>
			</li>
		<% end %>
	</ul>

5- (re-)inicie o servidor com o comando ```$ rails server``` e entre na página ```http://localhost:3000/paginas/lista``` para ver a lista de páginas no sistema.

### Show

Para criar a ação basica de mostrar um objeto, é necessario ter o *id** ou algum outro parametro para achar a entrada que está sendo procurada.

#### Exemplo
 

<a class="btn btn-mini" href="readme.md">voltar</a>