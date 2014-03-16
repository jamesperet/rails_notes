# Capybara

#### Navegação

* ```visisit(path)``` - Navega até o *path* utilizando o **GET**.

#### Links e botões

* ```click_link(locator)``` - Acha um link por *ID* ou texto e clica nele. Isso faz com que o **Capybara** carregue uma nova página.
* ```click_button(locator)``` - Acha um botão com *ID*, texto ou *value* e clica-lo. Isso causa o **Capybara** a enviar o formulário.
* ```click_on(locator)``` - Acha um botão ou link por *ID*, texto ou *value* e clica nele.

#### Interagindo com formulários

* ```fill_in(locator, {:with => text})``` - Localiza um campo ou area de texto e insere o texto dado. O campo pode ser localizado pelo seu nome, ID ou label.
* ```choose(locator)``` - Localiza um botão radial e marca. O botão radial pode ser localizado pelo seu nome, ID ou label.
* ```check(locator)``` - Localiza um check box e o marca. O check box pode ser localizado pelo seu nome, ID ou label.
* ```uncheck(locator)``` - Localiza e desmarca um check box. O check box pode ser localizado pelo seu nome, ID ou label.
* ```attach_file(locator, path)``` - Encontra um campo para atachar arquivos  e adiciona um arquivo baseado em seu *path*. O campo pode ser localizado pelo seu nome, ID ou label.
* ```select(value, {:from => locator})``` - Encontra um select box e seleciona uma de suas opções. Se o select box suportar seleçnoes multiplas, esta função pode ser chamada multiplas vezes. O select box pode ser encontrado pelo seu nome, ID, ou label.

#### Querying

* ```page.has_css?(css_selector)``` - Checa se um elemento identificado por uma tag CSS esta na página ou dentro de um elemento.
* ```page.has_xpath?(xpath_expression)``` - Checa se um elemento identificado por um XPath esta na página ou dentro de um elemento.
* ```page.has_button?(locator)``` - Encontra o elemento indentificado pelo localizador na página ou dentro do elemento atual.
* ```page.has_checked_field?(locator)``` - Encontra o elemento indentificado pelo localizador na página ou dentro do elemento atual.
* ```page.has_unchecked_field?(locator)``` - Encontra o elemento indentificado pelo localizador na página ou dentro do elemento atual.
* ```page.has_field?(locator, options = {})``` - Encontra o elemento indentificado pelo localizador na página ou dentro do elemento atual.
* ```page.has_link?(locator, options = {})``` - Encontra o elemento indentificado pelo localizador na página ou dentro do elemento atual.
* ```page.has_select?(locator, options = {})``` - Encontra o elemento indentificado pelo localizador na página ou dentro do elemento atual.
* ```page.has_table?(locator, options = {})``` - Encontra o elemento indentificado pelo localizador na página ou dentro do elemento atual.
* ```page.has_content?(text)``` - Checa se uma pagina ou o node atual contem o texto dado, ignorando HTML tags e normalizando o espaço em branco.

Todas os metodos *has_xxx?* retornam *verdadeiro* ou *falso* e também possuem um metodo contrario *has_no_xxx?* para checar a o contrario.

Esses metodos devem ser usados junto com assertações do *Test::Unit* ou do *RSpec*.

Exemplo *Test::Unit* 
```assert(page.has_content?("orange juice"))```

Exemplo *RSpec* 
```page.should have_content("orange juice")```

#### find

* ```find_field(locator)``` - Encontra um campo em um formulario na página.
* ```find_link(locator)``` - Encontra um link na página.
* ```find(selector)``` - Encontra o primeiro elemento na página utilizando um seletor CSS ou cria um erro.
* ```find(:xpath, selector)``` - Utiliza o XPath para achar o primeiro elemento na página. O Capybara pode utilizar o XPath como *default* com ```Capybara.default_selector = xpath```.
* ```all(selector)``` - Retorna todos os elementos em uma array que sejam iguais ao seletor da busca.

Uma referencia completa de funções de busca pode ser encontrada [aqui](http://rubydoc.info/github/jnicklas/capy-bara/master/Capybara/Node/Finders).

Os metodos *find* parecem com os *has_xx?*, mas a diferença é que eles retornam um *Capybara::Element* em vez de verdadeiro ou falso. É possivel invocar outros metodos em um elemento encontrado, como por exemplo:

```page.find('#main-nav').click_link('Login')```

#### Scoping

* ```within(locator, &block)```
* ```within_fieldset(locator, &block)```
* ```within_frame(frame_id, &block)```
* ```within_table(locator, &block)```
* ```within_window(handle, &block)```

Esses metodos limitam os outros metodos a um scope especifico da página. Exemplo:

	within('#search') do 
		click_button('Search')	end

---

<a class="btn btn-mini" href="readme.md">voltar</a>

