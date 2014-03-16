# Associações Polimorficas

Associações polimorficas servem para criar um modelo que pertence a varios outros modelos. Por exemplo, uma classe de comentarios que está associada a varios modelos como posts, fotos e eventos.

```$ rails g model comment content:text commentable_id:integer commentable_type```

Esse primeiro comando gera o modelo para os comentarios. As ultimas duas variaveis servem para guardar o id e o tipo de objeto que o comentario pertence. 

Para melhorar a performance do sistema de comentarios, adicione o ```commentable_id``` e o ```commentable_type``` no index da tabela no arquivo de migração ```db/migrations/xxxxxxxxxxxxxx_create_comments.rb```:

	classCreateComments < ActiveRecord::Migration
	  defchange
	    create_table :commentsdo |t|
	      t.text :content
	      t.belongs_to :commentable, polymorphic: true
	
	      t.timestamps
	    end
	    add_index :comments, [:commentable_id, :commentable_type]
	  end
	end

No exemplo acima, as duas variaveis ```commentable_id``` e o ```commentable_type``` foram substituidas por ```t.belongs_to :commentable, polymorphic: true```. Esse é outro metodo de declarar as associações polimorficas.

Rode o comando ```$ rake db:migrate``` para criar a nova tabela no banco de dados.

Entre no modelo dos comentarios ```app/models/comment.rb``` e adicione a associação e retire os atributos que não são necessarios:
	
	classComment < ActiveRecord::Base
	  attr_accessible :content
	  belongs_to :commentable, polymorphic:true
	end

Depois, adicione o outro lado da associação para cada modelo que vai ter comentarios:

	classPost < ActiveRecord::Base
	  attr_accessible :title, :content
	  has_many :comments, as: :commentable
	end

Com isso agora é possivel criar e acessar comentarios igual a qualquer outra associação.

Para criar o CRUD do sistema de comentarios, primeiro crie um controlador:
```$ rails g controller comments index new```

