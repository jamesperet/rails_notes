# Instance Variables

De acordo com a arquitetura **MVC**, o **controlador** agrega as informações do **modelo** e repassa para o **View** renderiza-las no browser. 

Para isso, o **controlador** declara as **"instance variables"** que são repassadas para o **View**.

### Variaveis em Ruby:

	normal_variable
	@intance_variable

### Exemplo

No arquivo ```app/controllers/demo_controller.rb```:

	def instance_variable_test
		@array = [1,2,3,4]
	end

Depois no arquivo ```app/views/demo/instance_variable_test.html.erb```:

	<% @array.each do |n| %>
	  <%= n %> <br/>
	<% end %>

HTML renderizado pelo browser:

	1<br/>
	2<br/>
	3<br/>
	4<br/>

<a class="btn btn-mini" href="readme.md">voltar</a>