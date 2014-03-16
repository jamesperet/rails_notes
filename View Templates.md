# ERB View Templates

Páginas de template para mostrar conteúdo. Elas utilizam o **ERB** ou *"Embed Ruby Code"* para processar conteudo dinâmico escrito em **Ruby** para **HTML**.
 
### Exemplo de nome de arquivo

```views/demo/index.html.erb```

### Codigo de embed
	
	<% code %>
	<%= code %>

A primeira versão apenas processa o codigo ruby dentro dos tags. A segunda versão com o sinal de igual processa e retorna o **"output"** do codigo em Ruby.

### Exemplo de arquivo ERB
	
	<h1>Demo#hello</h1>
	<p>Hello World</p>
	
	<%= 1 + 1 %>

	<% target = "world" %>
	<%= "Hello #{target}" %>

<a class="btn btn-mini" href="readme.md">voltar</a>