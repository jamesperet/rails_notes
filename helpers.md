# Helpers

## Text

## Stylesheet

O Rails guarda os arquivos de css na pasta: ```/public/stylesheets``` e os arquivos por *default* precisam ter a extensão **".css"**.

Os stylesheets devem ser definidos no ```<head>``` do arquivo de template.

Normalmente uma declaração de stylesheet em **HTML** seria a seguinte:

	<lin href="stylesheets/styles.css" rel="stylesheet" type="text/css" media="all" /> 

É possivel usar um **helper** do rails para simplificar o processo:

	<%= stylesheet_link_tag('public', :media => 'all') %> 

Esse **helper** do rails cria o mesmo codigo **HTML** que foi mostrado anteriormente. 

É possivel carregar mais de um estilo por vez, separados por virgulas.

Os tipos de media que podem ser utilizados são: **all**, **screen**, **print**, **mobile** e **tablet**.

<a class="btn btn-mini" href="readme.md">voltar</a>