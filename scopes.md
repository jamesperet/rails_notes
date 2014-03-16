# Scopes

É possivel definir **ActiveRelation** querys dentro de um modelo. Isso é util para salvar buscas que são muito utilizadas. Também é possivel passar parametros para um **scope**.

## Exemplo Simples

Primeiro declare o **scope**:

	Class Page < ActiveRecord::Base
	
		scope :pub, where(:status => "published")
	
	end

 Depois para acessa-lo:

```pages = Page.pub```

## Exemplo com parametros

Para utilizar parametros em um scope, utilizamos um operador chamado **lambda**:

	Class Page < ActiveRecord::Base
	
		scope :search, lambda{ |query| where([title LIKE ?", "%#{query}&"])}
	
	end

Depois para utilizar o scope com o parametro:

```pages = Page.search("Contact")```

<a class="btn btn-mini" href="readme.md">voltar</a>