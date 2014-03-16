# Migration

Uma **migração** é um conjunto de instrução para o **banco de dados**, escrito em Ruby. Elas servem para *"migrar"* o banco de dados de um estado para outro. 

As intruções vão para **cima** e para **baixo** ou seja, é possivel ir para frente um estado e depois se necessario, voltar para o estado anterior.

Ao fazer migrações, uma copia do atual esquema do banco de dados é guardado no arquivo ```app/db/schema.rb```

## Metodo 1

```$ rails generate migration initial_infos```

O comando ira gerar um arquivo na pasta ```app/db/migrate```.

## Metodo 2

```$ rails generate model User```

O comando ira gerar um arquivo chamado "create_users" na pasta ```app/db/migrate```.

## Criando migrações especificas

Se você está criando uma migração para outros propositos (como por exemplo adicionando um campo a uma tabela já existente), é possivel usar o seguinte gerador:

```$ rails generate migration AddPartNumberToProducts part_number:string```

Para esse comando funcionar é preciso usar o formato ```AddXXXToYYY``` ou ```RemoveXXXFromYYY``` junto com uma lista dos nomes das colunas e seus formatos.

## Tipos de colunas

- binary
- boolean
- date
- datetime
- decimal
- float
- integer
- string
- text
- time
- foreign key (ex: ```references :user```)

## Opções de coluna
- :limit => size
- :default => value
- :null => true/false
- :precision => number
- :scale => numver


## Exemplo de arquivo de migração

	class CreateUsers < ActiveRecord::Migration
		def self.up
		  create_table :users do |t|
			t.string "first_name", :limit => 25
			t.string "last_name", :limit => 50
			t.string "email", :default => "", :null => false
			t.string "password", :limit => 40
			t.timestamps
		  end
		end

	  def self.down
		drop_table :users
	  end
	end

## Executando as migrações

```$ rake db:migrate```

Ao executar o comando, o rails vai rodar todas as migrações que ainda não foram executadas.

É possivel escolher em qual **"Enviorment"** a migração vai rodar adicionando a variavel ```RAILS_ENV=``` e adicionar o nome do ambiente (production, development, testing). O *default* é sempre o **development**.

## Reverter migrações

```$ rake db:migrate VERSION=0```

Esse comando vai reverter as migrações para a primeira versão, apagando a tabela no banco de dados. 

Também é possivel reverter para uma versão intermediaria utilizando o número no começo do nome do arquivo da migração.

## Metodos de migração de tabelas

- ```create_table(table, options) do |t|```
- ```drop_table(table)```
- ```rename_table(table, new_name)```
- ```add_column(table, column, type, options)```
- ```remove_column(table, column)```
- ```rename_column(table, column, new_name)```
- ```change_column(table, column, type, options)```
- ```add_index(table, column, options)```
- ```remove_index(table,column)```
- ```execute("any SQL string")```

#### Links

- [Rails Guides - Migrations](http://guides.rubyonrails.org/migrations.html)


<a class="btn btn-mini" href="readme.md">voltar</a>