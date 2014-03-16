# Criando um banco de dados mySQL

Para criar um banco de dados para o projeto, no terminal digite o seguinte comando:

```$ mysql -u root -p```

O terminal vai pedir a senha do sistema e entrar no banco de dados. A seguir confira se um banco de dados já existe para o projeto *"demo_project"*:

```SHOW DATABASES;```

Para criar o banco de dados:

```CREATE DATABASE demo_project_development;```

Depois é preciso dar acesso para um novo usuario ao banco de dados, para não comprometer a segurança do sistema utilizando a senha **"root"**.

```CREATE USER 'db_user'@'localhost' IDENTIFIED BY 'pass1234'; ```


```GRANT ALL PRIVILAGES ON demo_project_development.* TO 'user'@'localhost' IDENTIFIED BY 'password1234';```

Para publicação do site, mude o servidor de **localhost** para o dominio onde o sistema vai funcionar na internet.

Depois sai do bando de dados:

```exit```

Depois disso entre no arquivo ```config/database.yml``` e configure o novo usuário e senha para o banco de dados do sistema.

Para entrar de novo no banco de dados, utilize o comando:

```$ mysql -u user -p demo_project_development```

<a class="btn btn-mini" href="readme.md">voltar</a>