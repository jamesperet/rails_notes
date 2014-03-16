# Instalação

1. Cheque para ver se o Ruby já está instalado na sua maquina:

	```$ Ruby -v```
2. Cheque a versão do Ruy Gems e faça o update:
	
	```$ gem -v```

	```$ gem update```

	```$ sudo gem install mysql2```

3. Instale o rails:
	
	```$ gem list```
	```$ sudo gem install rails```

4. Instale e inicie o banco de dados mySQL:

	```$ ruby -e "$(curl -fsskl raw.github.com/mxcl/homebrew/go)"```

	```$ brew install mysql```

	```$ mysql_install_db --verbose --user='user' --basedir="$(brew --prefix mysql)" --datadir=/usr/local/var/mysql --tmldir=/tmp```

	```$ mysql.server start```

5. Para mudar a senha do banco de dados:

	```$ mysql```

	```SET PASSWORD for root@localhost=PASSWORD('1234');```

	```exit```
	
<a class="btn btn-mini" href="readme.md">voltar</a>