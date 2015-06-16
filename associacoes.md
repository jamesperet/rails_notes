# Associações
Para interligar tabelas atravez dos **"forein_keys"**, criamos associações enre os modelos.

Existem 3 tipos de associações: 1 para 1, 1 para varios e varios para varios.

## 1 para 1

Tipo de associação de um objeto para outro. Util para quebrar uma tabela grande em varias partes. Sempre defina os dois lados da associação!

#### Exemplo

- subject-page
	- Page has_one :subject
	- Subject has_one :page

No modelo **Subject**:

	class Subject < ActiveRecord::Base
		has_one :page
	end


No modelo **Page**:

	class Page < ActiveRecord::Base
		has_one :subject, {:foreign_key => "subject_id"}
	end
 
Para utilizar as associações, carregue um objeto e chame o metodo associado para ter o objeto retornado:

```subject.page``` ou ```page.subject```

## 1 para varios

As associações de um objeto para varios são muito usadas dentro dos aplicativos em rails. Elas utilizm o termo no plural e retornam um array de objetos.

A classe com o ```belongs_to``` que deve ter o **foreign key**

#### Exemplo

- user-photos
	- User has_many :photos
	- Photo belongs_to :user

No modelo **User**:

	class User < ActiveRecord::Base
		has_many :photos
	end

No modelo **Photo**:

	class Photo < ActiveRecord::Base
		belongs_to :user
	end

#### Metodos

- ```user.photos```
- ```user.photos << photo```
- ```user.photos = [photo, photo, photo]```
- ```user.photos.delete(photo)```
- ```user.photos.clear```
- ```user.photos.empty?```
- ```user.photos.size```
- ```user.photos[1]```

## varios para varios

Este tipo de associação é utilizada quando um objeto está associado a varios outros objetos, mas não exclusivamente.

Para este tipo de associação, primeiro é necessario criar uma **join table**, ou seja, uma tabela simples que não tem um **primary key** e tem apenas duas colunas com os **ids** dos dois objetos das duas tableas sendo relacionados.

#### Join Table

A nova tabela que vai guardar as inforamções da associação tem algumas regras de nomeação especificas:

- Primeira_tabela + _ + segunda_tabela
- Os dois nomes são no plural
- Os nomes entram em ordem alfabetica
- Nome *default* que pode ser configurado

#### Exemplo

- posts-tags
	- Post has_and_belongs_to_many :tags
	- Tags has_and_belongs_to_many :posts

O nome da tabela vai ser: "posts_tags".

1 - Criar o arquivo de migração: 

```rails generate migration CreatePostsTagsJoin```

2 - Editar o arquivo de migração:

	class CreatePostsTagsJoin < ActiveRecord::Migration
		def self.up
		  create_table :posts_tags, :id => false do |t|
			t.integer "post_id"
			t.integer "tag_id"
		  end
		  add_index :posts_tags, ["post_id", "tag_id"]
		end
		def self.down
		  drop_table :posts_tags
		end
	end

3 - Rodar a migração:

```rake:db:migrate```

4 - No arquivo do modelo Post:

	class Post < ActiveRecord::Base
		has_and_belongs_to_many :tags
	end

5 - No arquivo do modelo Tag:

	class Tag < ActiveRecord::Base
		has_and_belongs_to_many :blog_posts, :class_name => "Post" 
	end

## Associações Avançadas

Outra maneira de fazer associações de varios objeos para varios outros é criando um modelo proprio para administrar essas associaçãos e outras informações.

A diferença é que a tabela precisa de um **primary key** e não precisa seguir nenhuma conveção para o nome.

#### Exemplo

Neste exemplo vamos criar um log de edição das páginas do sistema. 

- User has_many :page_edits
- PageEdit belongs_to :user
- Page has_many :page_edits
- PageEdit belongs_to :page

1 - Criar um modelo: 

```rails generate model PageEdit```

2 - Editar o arquivo de migração:

	class CreatePageEdits < ActiveRecord::Migration
		def self.up
		  create_table :posts_tags do |t|
			t.references :page
			t.references :user
			t.string "new_content"
			t.timestamps
		  end
		  add_index :page_edits, ["page_id", "user_id"]
		end
		def self.down
		  drop_table :posts_tags
		end
	end

3 - Rodar a migração:

```rake:db:migrate```

4 - No arquivo do modelo Page:

	class Page < ActiveRecord::Base
		has_many :page_edits
	end

5 - No arquivo do modelo User:

	class User < ActiveRecord::Base
		has_many :page_edits
	end

6 - No arquivo do modelo PageEdit:

	class User < ActiveRecord::Base
		belongs_to :page
		belongs_to :editor, :class_name => "User", :foreign_key => "user_id"
	end

#### Traversing


-----------------
## Informações adicionais

* [A Rule of Thumb for Strong Parameters](http://patshaughnessy.net/2014/6/16/a-rule-of-thumb-for-strong-parameters)

-----------------

[Index](index.md)

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
