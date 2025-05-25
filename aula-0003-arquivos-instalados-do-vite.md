# **Arquivos e pastas instalados pelo vite**

## node_modules

É o local onde estará todos os arquivos de dependência da nossa aplicação

## public

É o local onde estará os arquivos estáticos como:

- Imagens gerais do projeto como um favicon

- Arquivos...

Essa pasta torna os arquivos dentro dela publicos em nosso projeto, podendo eles serem acessados simplesmente pela "/", exemplo:

- "/logo.fav"

## src

Local onde estará a codificação do nosso projeto, tudo estará denntro dele

Nele conterá as pastas `components`, `lib`, `hooks`... Dependendo da configuração e organização do projeto.

Dentro da pasta `src` temos:

> Falaremos mais sobre ele em breve!!

## .gitignore

É um arquivo de configuração do git que vai definir o que não subirá para nosso repositório, como por exemplo nossas variáveis de ambiente ou a própria `node_modules` pelo seu tamanho.

## eslint.config.js

É o arquivo de configuração do eslint, onde ele vai verificar em todo o nosso projeto possíveis erros que podem acontecer antes de mandarmos ele para a produção (para os clientes usarem)

## index.html

É o arquivo html do nosso projeto, é lá que o javascript vai mostrar a sua lógica como rotas, páginas, variáveis...

## package-lock.json

É o arquivo de segurânça de um projeto NODE, é nele que serão contidas todas as versões das dependências de todos os tempos do nosso projeto.

Podemos enxergar ela como uma documentação.

## package.json

É o principal arquivo de um projeto NODE

É lá que conterá as configurações do nosso projeto, como:

- Nome;

- Versão;

- Tipode de importação (Common Js ou ES Module);

- scripts (comandos CLI do nosso projeto);

- Dependências e Dependências de desenvolvimentos.

## README.md

É o arquivo de documentação do nosso projeto que será acessível a todos no github, onde explicaremos sobre nosso projeto.

É basicamente a documentação visual.

## tsconfig.json

É o arquivo de configuração do typeScript, porém ele apenas carrega os arquivos `tsconfig.app.json` e `tsconfig.node.json`

## tsconfig.app.json

Arquivo de configuração do TypeScript para nosso projeto

## tsconfig.node.json

Arquivo de configuração do TypeScript para o node

## vite.config.ts

Arquivo de configuração do VITE
