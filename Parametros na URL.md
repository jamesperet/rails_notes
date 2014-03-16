# Parametros na URL

Para utilizar os parametros GET e POST que vem em uma URL, utilizamos o **hash** ```params```.

## Exemplos

```/demo/hello/1?page=3&name=james```

Uma URL com estes parametros seria convertida no **hash**:

	{ :action => 'hello', :id => 1, :page => 3, :name => 'james', :controller => 'demo' }

Esses valores podem ser acessados no **controlador**, **modelo** ou no **view** atravez do **hash** ```params``` e do nome da variavel:

	params[:id]
	params['name']
	params[:page].to_i

## Detalhes

- Todos os ```params``` são strings.
- ```params``` são *"HashWithIndifferentAccess"* ou seja ```params[:id]``` é igual a ```params['id]```.
- ```params.inspect``` mostra um array com todos os paramentros.

<a class="btn btn-mini" href="readme.md">voltar</a>