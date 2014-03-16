# Rake

Ruby helper command.

```$ rake -T``` - Mostra uma lista com todos os comandos **rake** instalados.

```$ rake -D db:migrate``` - Mostra uma descrição da tarefa

O arquivo ```rakefile``` configura as **tarefas** do rake. É possivel escrever as proprias tarefas e coloca-las na pasta ```app/lib/tasks/```.

## Exemplo

```$ rake db:schema:dump RAILS_ENV=production``` - Exemplo de comando utilizando uma variavel do **rails**.

<a class="btn btn-mini" href="readme.md">voltar</a>