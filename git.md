# Git

### Iniciando um projeto

Para inicicar um projeto com o Git via terminal:

1. Entre no diretorio do projeto
2. ```$ git init```
3. ```$ git add REAdME.md```
4. ```$ echo "Hello Git World"```
5. ```$ git commit -m 'Initial upload'```

### Commit

Para ver as alterações feitas em um projeto:

```git status```

Depois adicione os arquivos que vão ser colocados dentro do commit com o comando:

```git add .```

Por ultimo faça o commit:

```git commit -m "Meu primeiro commit no git"```

### Retirando arquivos que estão em "Staging"

```git reset FILE```

Esse comando ira retirar um arquivo especifico da area de *staging* antes do commit.

### Modificar o ultimo commit

```git commit --amend```

### Remover um arquivo que já está no repositorio

```git rm file.txt```

Para apenas ignorar o arquivo e suas modificações não entrem no "Staging Index", crie uma regra no arquivo ```.gitignore``` e utilize o comando:

```git rm --cached file.txt```

### Criar um branch

```$ git checkout iss53```

Faça algum commit nesse branch e depois volte para o master:

```$ git checkout master```

Para juntar os dois branchs utilize o comando:

```$ git merge iss53```


### Remotes

Um ```remote``` é um servidor que vai guardar outra copia do codigo. Pode ser um servidor como o GitHub para guardar e compartilhar o codigo ou pode ser um servidor de produção como o Heroku que vai rodar o codigo do aplicativo.

Para criar um remote:

```git remote add origin https://github.com/username/Hello-World.git```

e para enviar codigo para ele:

```git push origin master```

Para deletar um remote:

```git remote rm origin```

e para listar os remotes

```git remote```

### Tags

Tags são utilizadas para marcar versões do seu codigo.

Para criar uma tag com a versão e uma mensagem, utilize o comando:

```git tag -a v1.4 -m 'my version 1.4'```

Para listar suas tags utilize o comando:

```git tag```

Para ver informações sobre uma tag: 

```git show v1.4```

Ao usar o comando ```git push```, as **tags** não são transferidas junto com o resto do projeto. É necessario transferir cada **tag** separadamente. Exemplo:

```git push origin v1.4```

### Links

- [12 curated git tips and workflow](http://durdn.com/blog/2012/12/05/git-12-curated-git-tips-and-workflows/)
- [Git Branching - Basic Branching and Merging](http://git-scm.com/book/en/Git-Branching-Basic-Branching-and-Merging)

<a class="btn btn-mini" href="readme.md">voltar</a>