# Gerando um "Controller" e um "View"

Um aplicativo Rails utiliza a arquitetura **MVC** (Model-View-Controller).  O **"controller"** serve para indentificar o que está sendo requisitado, carrega os modelos com informações e disponibiliza tudo isso para o **"view"**.

Para gerar um **"controller"** e um **"View"**, digite o comando no terminal:

```$ rails generate controller demo index``` 

O rails vai gerar os arquivos e hierarquia de pastas:

- ```app/controllers/demo_controller.rb```
- ```app/views/demo/index.html.erb```

Alem disso ele vai gerar alguns outros arquivos (testes e helpers) e uma rota no  ```config/routes.rb``` para o novo **"View"** que foi criado: ```get "demo/index"```.

<a class="btn btn-mini" href="readme.md">voltar</a>