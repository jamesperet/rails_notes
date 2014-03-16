# Records
Cada entrada de um objeto salvo no banco de dados é um **record**. É possivel criar records nos **controladores**, **modelos** ou atravez do [Rails Console](rails_console.md).

## Criar
Para criar um novo objeto baseado em um modelo:
 
```user = User.new```

O objeto é criado, mas ele ainda não foi salvo no banco de dados. Para ver o estado do objeto:

```user.new_record?``` - Retorna **true** se o objeto ainda não foi salvo.

Para modificar parametros de um objeto:

```user.first_name = "James"```
	
Depois para salvar o objeto:

```user.save```

Também é possivel criar um objeto, configurar seus parametros e salva-lo em um unico comando:

```user = User.new(:first_name => "James", :last_name => "Peret")```

## Modificar

Para achar uma entrada de um objeto atravez de seu **ID**:

```user = User.find(1)```

Depois um metodo para modificar parametros de um objeto em um unico comando:

```user.update_attributes(:first_name => "Gabriel")```

## Deletar

Para deletar um objeto no banco de dados, primeiro ache o objeto e depois execute o comando:

```user.destroy```

Esse comando apenas remove o objeto do banco de dados, mas ele continua carregado na memoria.

```user.frozen?``` - Retorna true se o objeto foi apagado do banco de dados.

## Busca Simples

- Busca por ID primario, retorna o objeto ou um **erro**:

	```Page.find(2)```

- Busca dinamica, retorna o objeto ou **NIL**:

	```Page.find_by_id(2)```

	```Page.find_by_title("About Us")```

- Achar todos os objetos:

	```Page.all```

- Achar o primeiro ou ultimo objetos no banco de dados:

	```Page.first```

	```Page.last```

## Busca com ActiveRelation

Buscas utilizando ActiveRelation são apenas executadas quando necessario, diferente das buscas simples onde são executadas imediatamente. Outro detalhe é que as buscas com ActiveRelation normalmente retornam um array.

Essas buscas utilizam o metodo ```where(condition)```, por exemplo:

```Page.where(:author => "James Peret")```

Essa busca retorna um Active Relation object, que pode ser linkado a outras buscas e consições:

```Page.where(:author => "James Peret").order("id ASC")```

O objeto com a busca é da classe ```ActiveRecord::Relation```, para ver isso:

```page_query.class```

Para ver a busca que vai ser gerada em SQL:

```page_query.to_sql```

Para iniciar uma busca com todos os objetos:

```pages = Page.scoped```

#### Tipos de expressões de condições

- **String**: metodo mais simples para passar condições para uma busca. Porem cuidado com SQL Injection!

	```author = 'James Peret' AND status='published'```

- **Array**: utilizado para previnir SQL Injection.

	```["author = ? AND status = published", "James Peret"]```

- **Hash**: seguro de SQL Injection, mas limitado (não pode usar LIKE e outras expressões).

	```{:author => "James Peret", :status => "published"}```

#### order, limit, offset

- order(sql_fragment)
	- formato: ```table_name.column_name``` + ```ASC``` ou ```DESC``` 
	- É possivel combinar mais de uma ordem, separadas por virgula.
- limit(integer)
- offset(integer)

Exemplo: ```Page.order("id ASC").limit(10).offset(30)```

<a class="btn btn-mini" href="readme.md">voltar</a>