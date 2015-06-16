# Testes

Em vez de ficar manualmente *debbugando* a cada modificação que for feita no aplicativo, o ideial é escrever **testes** que façam esse trabalho altomaticamente e mais rapido.

Exitem varios tipos de testes:

<table class="table table-bordered ">
	<tbody>
	  <tr>
	    <td style="width: 30%; padding-top: 8px; text-align: left; padding-left: 5px;">Unit Testing</td>
	    <td style="width: 50%;">Models</td>
	  </tr>
	  <tr>
	    <td style="width: 50%; padding-top: 8px; text-align: left; padding-left: 5px;">Functional Testing</td>
	    <td style="width: 50%;">Controllers/Views, requests</td>
	  </tr>
	  <tr>
	    <td style="width: 50%; padding-top: 8px; text-align: left; padding-left: 5px;">Integration Testing</td>
	    <td style="width: 50%;">User stories, sequences of requests</td>
	  </tr>
	  <tr>
	    <td style="width: 50%; padding-top: 8px; text-align: left; padding-left: 5px;">Performance Testing</td>
	    <td style="width: 50%;">How fast are requests handled</td>
	  </tr>
	  <tr>
	    <td style="width: 50%; padding-top: 8px; text-align: left; padding-left: 5px;">Load Testing</td>
	    <td style="width: 50%;">How many requests ca ben handled</td>
	  </tr>
	  <tr>
	    <td style="width: 50%; padding-top: 8px; text-align: left; padding-left: 5px;">Security Testing</td>
	    <td style="width: 50%;">Security vulnerabilities</td>
	  </tr>
	</tbody>
</table>

### Frameworks e ferramentas para testes

- Test::Unit (Incluso no Ruby e o Rails utiliza como default)
- Rspec
- Selenium
- Watir
- [Cucumber](cucumber.md)
- Webrat
- Mocha
- Shoulda
- Rcov
- Autotest

-----------------

[Index](index.md)

<!-- Highlight syntax for Mou.app, insert at the bottom of the markdown document  -->
 
<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://yandex.st/highlightjs/7.3/styles/github.min.css">
<script>
  hljs.initHighlightingOnLoad();
</script>
