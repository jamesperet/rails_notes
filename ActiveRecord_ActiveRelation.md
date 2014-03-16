# ActiveRecord

Baseado no padrão de design *"active record"*, um sistema em rails tem objetos inteligentes, com funções para inserir, atualizar e deletar entradas no banco de dados. 

## Exemplo
	user = User.new
	user.first_name = 'James'
	user.save # SQL INSERT

	user.last_name = 'Peret'
	user.sabe #SQL UPDATE

	user.delete #SQL DELETE

# ActiveRelation

Função introduzida no Rails v3.0, serve para simplificar a procura de querys complexos no banco de dados. Também serve para que o sistema apenas faça a busca no banco de dados no momento certo.

## Exemplo
	users = User.where(:first_name => "James")
	users = users.order("last_name ASC").limit(5)
	users = users. include(:articles_authored)

O Codigo acima utilizando ActiveRelations iria criar um query em SQL parecido com o seguinte:

	SELECT users.*, articles*
	FROM users
	LEFT JOIN articles ON (users.id = articles.author_id)
	WHERE users.first_name = 'James'
	ORDER BY last_name ASC LIMIT 5

<a class="btn btn-mini" href="readme.md">voltar</a>