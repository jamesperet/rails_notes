# jQuery File Upload

Para transformar um esquema de upload normal para AJAX usando jQuery, para upload de multiplos arquivos por vez, primeiro crie o formulario e a classe do [CarrierWave](CarrierWave.md). Depois modifique o formulario:

	<%= form_for Painting.new do |f| %>
	  <%= f.label :image, "Upload image:" %>
	  <%= f.file_field :image, multiple: true, name: "painting[image]" %>
	<% end %>

Depois adicione o **gem** no ```Gemfile```:

	group :assets do
	  gem 'jquery-fileupload-rails'
	end

Depois no arquivo *javascript* ```app/assets/javascripts/application.js```, adicione a linha depois do jQuery:

	//= require jquery-fileupload/basic

No arquivo *javascript* do **modelo**, exemplo ```app/assets/javascripts/painting.js.coffe```

	jQuery ->
		$('#new_image').fileupload()
			dataType "script"

No controlador ```app/controllers/paintings_controller.rb```, modifique a ação **create** para devolver um arquivo javascript.

	def create
	  @painting = Painting.create(params[:painting])
	end

Agora crie o arquivo ```app/views/paintings/create.js.erb```.

	<% if @painting.new_record? %>
	  alert("Failed to upload painting: <%= j @painting.errors.full_messages.join(', ').html_safe %>");
	<% else %>
	  $("#paintings").append("<%= j render(@painting) %>");
	<% end %>

No modelo dos **paintibgs** ```app/models/painting.rb```

	before_create :default_name
	
	def default_name
	  self.name ||= File.basename(image.filename, '.*').titleize if image
	end

Isso ira criar um nome *default* para os *paintings* criados.