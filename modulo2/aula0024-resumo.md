# Grande resumo

## SSR -> Server Side Rendering (Renderizado do lado servidor)

Aqui será renderizado tudo no servidor... Em next isso será um componente que terá acesso á funções asyncronas... São páginas dinâmicas...

Vantagem:

- Uso de Funções Assyncronas
- Aceddo direto do servidor
- Segurânça ao usar dados sensíveis, pois não vai parao Bandle da aplicação

Desvantagem:

- Não conseguimos usar aqui dentro Eventos do Navegador ou Hooks do React

## CSR -> Client Side Rendering (Renerizado do lado cliente)

Aqui será renderizado tudo no cliente... Em next isso será um componente que terá acesso á funções de cliente como: Eventos & Hooks... São páginas dinâmicas...

Vantagem:

- Uso de Eventos
- Uso de Hooks do React

Desvantagens:

- Não conseguimos usar funções Assyncronas nativamente, precisamos fazer gambiarra...
- Podemos vazar dados sensíveis. Tudo que estiver dentro dessa função, irá para o Bandle da aplicação

## Static / SSG -> Static Site Generate (Gerar Site Estático)

São páginas Estáticas que serão pre renderizadas no lado servidor e servido para o cliente... São páginas que não vão mudar o conteúdo dinâmicamente...

Vantagem:

- São carregadas muito rápido
- Tenho o HTML "pronto"

Desvantagens:

- Podem estar salvas no cache e não atualizar quando queremos

## Dynamic -> Rotas Dinâmicas

Aqui não teremos nada de conteúdo para renderizar... Toda vêz que o usuário acessar essa Rota, o Next vai fazer uma requisição no Servidor paraatualizar seu conteúdo

Vatagem:

- Conteúdo Atualizado Dinâmicamente

Desvantagem:

- Pode ser mais custosa de ser renderizada

## ISR -> Incremental Static Regeneration

São rotas que fazem a junção dos dois tipos de rotas do Next: Static & Dynamic

Exemplo: Tenho uma site de Ecomerce usando rotas Estaticas, porém a cada 4 horas eu quero que essas rotas Estáticas atualizem seu conteúdo... Conseguimos fazer isso configurando as rotas...

Exemplo 2: Tenho uma site de Ecomerce usando rotas Estaticas, porém quando eu mudar algo no conteúdo eu quero que essas rotas Estáticas atualizem seu conteúdo... Conseguimos fazer isso configurando as rotas...

## Rotas que esaremos no Projeto

Aqui teremos nosso CRUD (Create, Reader, Updated, Delete)

### / (Pública) - Static

- Lista de Posts renderizada ao usuário

### /post/[slug] (Pública) - Static

- Post específoco renderizado ao usuário

### /admin/post (Privada) - Dynamic

- Ler dados do meu Banco de Dados (R)
- Deletar dados do meu Banco de dados (D)

### /adminpost/[id] (Privada) - Dynamic

- Atualizar dados do meu Banco de Dados (U)

### /admin/post/new (Privada) - Dynamic

- Criar dados no Meu Banco de Dados (C)

## /admin/login (Pública) - Dynamic

- Fazer o Login de usuário
