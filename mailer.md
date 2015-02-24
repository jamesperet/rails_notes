# Mailer

A classe **ActionMailer** possibilita o aplicativo Rails a enviar emails e funciona como do mesmo jeito que um *Controller* e seus *Views*. Os arquivos dessa classe ficam na pasta ```app/mailers``` e os *Views* ficam em ```app/views```. 

## Criando um novo Mailer

Para criar uma nova classe, use o gerador na linha de comando:

```rails generate mailer UserMailer```

Os seguintes arquvos vão ser gerados:

	create  app/mailers/user_mailer.rb
	create  app/mailers/application_mailer.rb
	invoke  erb
	create    app/views/user_mailer
	create    app/views/layouts/mailer.text.erb
	create    app/views/layouts/mailer.html.erb
	invoke  test_unit
	create    test/mailers/user_mailer_test.rb
	create    test/mailers/previews/user_mailer_preview.rb

## Editando o Mailer


O arquivo ```app/mailers/user_mailer.rb``` contem um novo mailer vazio
	
	class UserMailer < ApplicationMailer
	end
	
Vamos agora criar uma novo metodo para enviar um email de boas vindas aos usuários:

	class UserMailer < ApplicationMailer
	  default from: 'notifications@example.com'
	 
	  def welcome_email(user)
	    @user = user
	    @url  = 'http://example.com/login'
	    mail(to: @user.email, subject: 'Welcome to My Awesome Site')
	  end
	end

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
