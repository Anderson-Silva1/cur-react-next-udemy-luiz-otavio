# Estudo sobre Static Site Generation (SSG) em Next.js

## O que é Static Site Generation (SSG)?

Static Site Generation (SSG) é uma técnica de renderização em que as páginas de um site são pré-geradas como arquivos HTML estáticos durante o processo de build. Em vez de gerar o conteúdo dinamicamente a cada requisição do usuário, como ocorre no Server-Side Rendering (SSR), ou no cliente, como no Client-Side Rendering (CSR), o SSG cria arquivos estáticos que podem ser servidos diretamente por um servidor web ou uma Content Delivery Network (CDN). Isso resulta em tempos de carregamento extremamente rápidos, maior segurança e melhor escalabilidade.

No contexto do Next.js, o SSG é uma das principais estratégias de renderização, junto com SSR e Incremental Static Regeneration (ISR). Ele é particularmente útil para páginas com conteúdo que não muda frequentemente, como blogs, páginas de marketing, documentações ou portfólios.

---

## Como o SSG funciona no Next.js?

No Next.js, o SSG é implementado por padrão para páginas que não requerem dados dinâmicos ou quando os dados são buscados no momento do build usando funções como `getStaticProps` e `getStaticPaths`. Durante o comando `next build`, o Next.js gera arquivos HTML para cada página, que são então armazenados e servidos diretamente nas requisições subsequentes.

### Benefícios do SSG no Next.js

1. **Performance**: Como as páginas são pré-renderizadas, o tempo de carregamento é reduzido, pois não há necessidade de processamento no servidor ou no cliente no momento da requisição.
2. **SEO**: Páginas estáticas são facilmente rastreáveis por motores de busca, já que o HTML completo está disponível no momento do carregamento.
3. **Escalabilidade**: Arquivos estáticos podem ser distribuídos globalmente por CDNs, reduzindo a carga no servidor e melhorando a experiência do usuário.
4. **Segurança**: Como não há execução de código no lado do servidor para cada requisição, sites estáticos são menos vulneráveis a ataques como injeção SQL ou cross-site scripting (XSS).
5. **Custo reduzido**: Servir arquivos estáticos é mais econômico, já que não requer processamento intensivo no servidor.

### Limitações do SSG

- **Conteúdo estático**: SSG é ideal para conteúdo que não muda com frequência. Para dados altamente dinâmicos, como feeds em tempo real, o SSR ou ISR pode ser mais apropriado.
- **Tempo de build**: Sites com muitas páginas podem ter tempos de build longos, especialmente se dependem de chamadas a APIs externas.
- **Limite de páginas**: Dependendo da plataforma de hospedagem (como Vercel), pode haver limitações no número de páginas geradas (por exemplo, \~8.000 páginas).

---

## Implementando SSG no Next.js

O Next.js oferece duas funções principais para implementar SSG: `getStaticProps` e `getStaticPaths`. Abaixo, exploramos como usá-las com exemplos práticos.

### 1. Páginas Estáticas sem Dados Externos

Se uma página não depende de dados externos, o Next.js a renderiza automaticamente como estática durante o build. Exemplo:

```jsx
// pages/about.js
import React from "react";

const About = () => {
  return <div>Sobre Nós</div>;
};

export default About;
```

Ao executar `next build`, o Next.js gera um arquivo HTML estático para a página `/about`. Esse arquivo pode ser servido diretamente por um servidor web ou CDN.

### 2. Páginas Estáticas com Dados Externos (`getStaticProps`)

Quando uma página precisa de dados externos, você pode usar a função `getStaticProps` para buscar esses dados no momento do build. Exemplo:

```jsx
// pages/blog.js
import React from "react";

const Blog = ({ posts }) => {
  return (
    <div>
      <h1>Blog</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
};

export async function getStaticProps() {
  const res = await fetch("https://api.exemplo.com/posts");
  const posts = await res.json();

  return {
    props: {
      posts,
    },
  };
}

export default Blog;
```

Nesse exemplo:

- `getStaticProps` faz uma chamada à API durante o build.
- Os dados retornados são passados como `props` para o componente `Blog`.
- O Next.js gera um arquivo HTML estático com os dados incorporados.

### 3. Rotas Dinâmicas com SSG (`getStaticPaths` + `getStaticProps`)

Para páginas com rotas dinâmicas (ex.: `/posts/[id]`), você precisa usar `getStaticPaths` para informar ao Next.js quais caminhos devem ser pré-renderizados. Exemplo:

```jsx
// pages/posts/[id].js
import React from "react";

const Post = ({ post }) => {
  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </div>
  );
};

export async function getStaticPaths() {
  const res = await fetch("https://api.exemplo.com/posts");
  const posts = await res.json();

  const paths = posts.map((post) => ({
    params: { id: post.id.toString() },
  }));

  return {
    paths,
    fallback: false, // Retorna 404 para rotas não pré-geradas
  };
}

export async function getStaticProps({ params }) {
  const res = await fetch(`https://api.exemplo.com/posts/${params.id}`);
  const post = await res.json();

  return {
    props: {
      post,
    },
  };
}

export default Post;
```

Explicação:

- `getStaticPaths` define todas as rotas dinâmicas que serão pré-renderizadas.
- `getStaticProps` busca os dados para cada rota específica.
- A opção `fallback: false` garante que rotas não pré-geradas retornem um erro 404. Alternativamente, `fallback: true` ou `fallback: 'blocking'` permite gerar páginas sob demanda, mas isso pode afetar a performance inicial.

### 4. Incremental Static Regeneration (ISR)

O ISR é uma extensão do SSG que permite atualizar páginas estáticas após o build sem precisar reconstruir todo o site. Isso é útil para sites com conteúdo que muda esporadicamente. Exemplo:

```jsx
// pages/blog.js
import React from "react";

const Blog = ({ posts }) => {
  return (
    <div>
      <h1>Blog</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </div>
  );
};

export async function getStaticProps() {
  const res = await fetch("https://api.exemplo.com/posts");
  const posts = await res.json();

  return {
    props: {
      posts,
    },
    revalidate: 60, // Revalida a página a cada 60 segundos
  };
}

export default Blog;
```

Com o ISR, a página é gerada estaticamente no build, mas pode ser atualizada em segundo plano quando um usuário faz uma requisição, respeitando o intervalo de `revalidate`. Isso combina a performance do SSG com a flexibilidade do SSR.

---

## Configurando Exportação Estática

Para criar um site completamente estático, você pode configurar o Next.js para exportar os arquivos HTML com o comando `next export`. Isso é útil para hospedagem em servidores que só servem arquivos estáticos, como GitHub Pages ou AWS S3. Exemplo de configuração:

```javascript
// next.config.js
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: "export",
};

module.exports = nextConfig;
```

Após executar `next build && next export`, os arquivos estáticos são gerados no diretório `out`. No entanto, observe que algumas funcionalidades, como ISR e API routes, não são suportadas em exportações estáticas puras.

---

## Casos de Uso do SSG no Next.js

O SSG é ideal para:

- **Blogs e sites de conteúdo**: Páginas de blog, notícias ou documentação que não mudam com frequência.
- **Sites de marketing**: Landing pages ou sites promocionais que priorizam velocidade e SEO.
- **Portfólios**: Sites pessoais ou profissionais com conteúdo estático.
- **E-commerce com conteúdo fixo**: Páginas de produtos que não mudam com frequência podem se beneficiar do SSG com ISR.

---

## Comparação com Outras Estratégias de Renderização

- **SSG vs. SSR**: SSG gera páginas no build, enquanto SSR gera páginas a cada requisição. SSG é mais rápido, mas menos adequado para dados em tempo real.
- **SSG vs. CSR**: CSR depende do JavaScript no cliente para renderizar conteúdo, o que pode ser mais lento e menos amigável para SEO. SSG oferece melhor performance e SEO.
- **SSG vs. ISR**: ISR é uma evolução do SSG, permitindo atualizações dinâmicas sem rebuild completo. Use ISR quando o conteúdo muda esporadicamente.

---

## Melhores Práticas para SSG no Next.js

1. **Otimizar chamadas de API**: Centralize a lógica de busca de dados em funções reutilizáveis (ex.: em uma pasta `lib/`) para evitar duplicação de código.
2. **Usar CDNs**: Hospede os arquivos estáticos em uma CDN para melhorar a performance global.
3. **Gerenciar rotas dinâmicas com cuidado**: Use `fallback: 'blocking'` ou `true` para sites com muitas páginas dinâmicas, mas esteja ciente do impacto na experiência do usuário.
4. **Aproveitar ISR para conteúdo dinâmico**: Para sites com atualizações frequentes, configure intervalos de revalidação apropriados.
5. **Testar performance**: Use ferramentas como Google Lighthouse para verificar a performance e SEO do site gerado.

---

## Conclusão

Static Site Generation no Next.js é uma abordagem poderosa para criar sites rápidos, escaláveis e otimizados para SEO. Com funções como `getStaticProps`, `getStaticPaths` e ISR, o Next.js oferece flexibilidade para lidar com diferentes casos de uso, desde sites simples até aplicações complexas com milhares de páginas. Ao combinar SSG com CDNs e boas práticas de desenvolvimento, é possível criar experiências web de alta qualidade com custos reduzidos e maior segurança.

Para mais detalhes, consulte a documentação oficial do Next.js: nextjs.org/docs.
