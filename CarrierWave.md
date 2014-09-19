# Upload de arquivos com o CarrierWave

O Carrierwave é um *gem* para simplificar a criação de um sistema de upload de arquivos no Ruby on Rails.


## Instalação

Adicione no ```Gemfile```:

```ruby
gem "carrierwave"
```

Instale o **gem** e depois gere um novo objeto:

```bash
bundle install
rails g uploader image
```

Depois crie uma migração para associar as imagens a outra classe:

```bash
rails g migration add_image_to_posts image:string
```

Execute o  comando ```$ rake db:migrate``` para criar o campo na tabela no banco de dados. Depois entre no arquivo ```models/post.rb``` e adicione o uploder:

```ruby
class post < ActiveRecord::Base
  attr_accessible :name, :content, :image
  mount_uploader :image, ImageUploader
end
```

Depois modifique o formulario que vai conter o campo de upload:

```erb
<%= form_for @post, html => {:multipart => true} do |f| %>
	<%= f.file_field :image %>
	<%= f.submit %>
<% end %>
```

Para utilizar a imagem no **view**:

```erb
<%= image_tag @post.image.to_s %>
```


## Modificar a pasta onde os arquivos são salvos

No **uploader** que foi gerado, modifique o arquivo ```app/uploaders/image_uploader.rb``` e defina onde os arquivos vão ser salvos:

```ruby
class ImageUploader < CarrierWave::Uploader::Base
  storage :file

  def store_dir
    "uploads/#{model.class.to_s.underscore}/#{mounted_as}/#{model.id}"
  end

end
```


## Upload de arquivo via URL

Para carregar um arquivo externo vindo de uma URL, modifique o formulario de upload:

```erb
<%= form_for @painting, :html => {:multipart => true} do |f| %>
	<%= f.file_field :image %>
    <%= f.text_field :remote_image_url %>
  	<%= f.submit %>
<% end %>
```

No modelo onde que carrega as imagens ```models/post.rb```, adicione o ```attr_accessible :remote_image_url```. É importante usar essa variavel por que o **CarrierWave** vai automaticamente procurar o arquivo pela URL e fazer o upload e processamento.


## Processamento de imagens com o rmagik

Para fazer modificações na imagem como criar versões *thumbnail*, é possivel utilizar o **rmagick** junto com o **ImageMagick**.

Primeiro é necessario instalar o **ImageMagick**.

Adicione no ```Gemfile``` o **rmagick** e execute o ```$ bundle install```:

```ruby
gem "rmagick"
```

No arquivo ```app/uploaders/image_uploader.rb```, inclua o ```CarrierWave::RMagik```, defina a pasta onde as imagens serão guardadas e defina o processamento da nova versão da imagem:

```ruby
class ImageUploader < CarrierWave::Uploader::Base

  include CarrierWave::RMagick

  version :thumb do
    process :resize_to_limit => [200, 200]
  end

end
```

Para fazer o display da imagem **thumb**:

```erb
<%= image_tag @post.image.thumb if painting.image? %>
```


------

[voltar](../)