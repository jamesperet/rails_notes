# Routes

Configurações do applicativo Rails onde são definidos os roteamentos de páginas.

```app -> config -> routes.rb```

### Tipos de rotas

#### Simple route
Roteamento simples de páginas unicas.

```get "demo/index"``` = ```match "demo/index", :to => "demo#index"```

#### Default route
Roteamento automatico de páginas baseado em varios parametros.

```match ':controller(/:action(/:id(.:format)))'```

#### Root route
Rota para o index do sistema.

```root :to => "demo#index"```

### Detalhes

- Sempre reiniciar o servidor depois de mudar configurações de roteamento.
- O sistema vai escolher a primeira rota compativel com a página requisitada que for achado no arquivo de rotas.
- Escrever a rota para o index no começo do arquivo.

<a class="btn btn-mini" href="readme.md">voltar</a>