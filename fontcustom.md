# Criando uma fonte customizada

## Instalação do [Font Custom](http://fontcustom.com/)

Primeiro instale o [XQuartz](https://xquartz.macosforge.org)

Depois rode os seguintes comandos para instalar a ferramenta:
	
	brew install fontforge --with-python
	brew install eot-utils
	gem install fontcustom


## Utilização

Rode o comando ```fontcustom compile``` para compilar e criar os arquivos da sua nova fonte.

Para criar um arquivo de configuração, rode o comando ```fontcustom config /path/to/vectors```.

## Links

* [FontCustom](http://fontcustom.com/)
* [FontCustom GitHub Page](https://github.com/FontCustom/fontcustom/)

-----------------

[Index](index.md)

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
