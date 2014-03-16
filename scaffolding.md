# Scaffolding

É possivel usar o comando **scaffold** para gerar de uma vez todos os componentes do **CRUD**:

```$ rails generate scaffold post title:string body:text```

Esse comando cria o model, o controller (com as actions: index, show, new, create, edit, update e destroy) e as views para as actions criadas.

O comando gerou altomaticamente uma **migração** com um nome parecido com ```db/migrate/20120803224002_create_posts.rb```.

	class CreatePosts < ActiveRecord::Migration
	  def change
	    create_table :posts do |t|
	      t.string :title
	      t.text :body
	
	      t.timestamps
	    end
	  end
	end

Note que essa **migração** é um pouco diferente das padrões.

Depois é necessario rodar as migrações com o comando:

```$ rake db:migrate```

Inicie o servidor com o comando ```$ rails server``` e entre na URL com o browser: [http://localhost:3000/posts](http://localhost:3000/posts).

<img src="imgs/exemplo_scaffolding.jpg" class="img-polaroid">

Acima, uma página de listagem de posts criada pelo comando scaffold.

*Referencia: [Blog do Dmitry Nix - Scaffold Exemplo e a Estrutura no Rails](http://blog.dmitrynix.com/scaffold-exemplo-e-a-estrutura-no-rails/)*

<a class="btn btn-mini" href="readme.md">voltar</a>