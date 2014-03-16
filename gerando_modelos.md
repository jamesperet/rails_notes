# Gerando Modelos

O seguinte comando gera um novo modelo em um aplicativo Rails. O nome tem que ser escrito em **"CamelCase"** e tem que ser uma termo no **singular**.

```$ rails generate model Page```

Arquivos gerados:

- ```db/migrate/201200101000000_create_pages.rb``` - Arquivo inicial de migração
- ```app/models/page.rb``` - Classe **Page** que herda funções da classe **ActiveRecord::Base** 

## Exemplo de modelo

	class User < ActiveRecord::Base
		# Para utilizar uma tabela que não seja a default "pages"
		# set_table_name("weird_page")
	end

<a class="btn btn-mini" href="readme.md">voltar</a>