# Postgres

Para criar um banco de dados postgres primeiro faça o login no banco de dados:

``$ psql --username=admin``

Use o comando ``\list`` para listar as bases de dado.

Depois crie dois novos bancos de dados com os seguintes comandos:

``CREATE DATABASE demo_app_development``

``CREATE DATABASE demo_app_test``

``GRANT ALL PRIVILEGES ON DATABASE demo_app_development TO demo_app_user;``

``GRANT ALL PRIVILEGES ON DATABASE demo_app_test TO demo_app_user;``

Para mudar o dono do banco de dados:

``ALTER DATABASE name OWNER TO new_owner;``

Para mudar a permissão de um usuário:

``ALTER USER username CREATEDB;``

Por ultimo configure o arquivo ``database.yml `` com as informações de login e database que foram criadas:

	development:
	  adapter: postgresql
	  encoding: unicode
	  database: demo_app_development
	  pool: 5
	  username: admin
	  password: password1

	test:
	  adapter: postgresql
	  encoding: unicode
	  database: demo_app_test
	  pool: 5
	  username: admin
	  password: password1
