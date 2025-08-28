# Full Route Cache no Next.js

## O que é Full Route Cache?

O **Full Route Cache** é um mecanismo do Next.js que armazena o resultado completo de uma rota renderizada (HTML + payload JSON) no servidor. Ele serve a página pronta para múltiplos usuários, evitando re-renderizações desnecessárias. É diferente de outros caches do Next.js:

- **Request Memoization**: Evita chamadas duplicadas dentro de uma única renderização.
- **Data Cache**: Armazena resultados de requisições `fetch` entre renderizações.
- **Full Route Cache**: Guarda a página inteira (HTML + dados) para reutilização.

### Analogia Simples

- **Request Memoization**: “Não repete cálculos na mesma tarefa.”
- **Data Cache**: “Não repete chamadas externas entre tarefas.”
- **Full Route Cache**: “Entrega a tarefa pronta sem recalcular nada.”

## Como Funciona?

Quando uma rota (ex.: `/dashboard`) é renderizada, o Next.js gera:

- **HTML inicial**: Para exibição no navegador.
- **Payload JSON**: Dados usados pelo React para hidratação.

O **Full Route Cache** armazena esses artefatos no servidor. Em acessos futuros, a rota é servida diretamente do cache, sem reprocessar, até que o cache expire ou seja invalidado.

### Exemplos Práticos

#### 1. Página Estática (SSG)

```jsx
export default function Page() {
  return <h1>Olá, mundo!</h1>;
}
```

- **Comportamento**: A rota é totalmente cacheada no Full Route Cache. Qualquer acesso (refresh ou outro dispositivo) recebe o HTML pronto do cache.
- **Uso**: Ideal para páginas que não mudam, como páginas institucionais.

#### 2. Página com Fetch e Revalidação (ISR)

```jsx
export default async function Page() {
  const data = await fetch("https://api.exemplo.com/posts", {
    next: { revalidate: 60 },
  }).then((res) => res.json());
  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

- **Comportamento**: A página renderizada é armazenada no Full Route Cache por 60 segundos. Durante esse período, todos os usuários recebem a mesma versão cacheada. Após 60s, a próxima requisição re-renderiza a página, atualizando o cache.
- **Uso**: Perfeito para conteúdo que atualiza periodicamente, como notícias.

#### 3. Página Dinâmica (Sem Cache)

```jsx
export default async function Page() {
  const data = await fetch("https://api.exemplo.com/users", {
    cache: "no-store",
  }).then((res) => res.json());
  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

- **Comportamento**: O Full Route Cache é desativado. Cada requisição gera uma nova renderização no servidor (SSR puro).
- **Uso**: Indicado para dados em tempo real, como dashboards dinâmicos.

## Full Route Cache x SSG Clássico

### SSG Clássico (Pages Router)

No Next.js antes do App Router, as páginas estáticas eram geradas no momento do build (`getStaticProps`) e salvas como arquivos HTML estáticos:

```jsx
export async function getStaticProps() {
  const data = await fetch("https://api.exemplo.com/posts").then((res) =>
    res.json()
  );
  return { props: { data } };
}
```

- **Comportamento**: HTML gerado no build, servido diretamente em cada requisição. Só muda com um novo deploy.
- **Limitação**: Menos flexibilidade para atualizações dinâmicas.

### Full Route Cache (App Router)

No App Router (Next.js 13+), o Full Route Cache é mais dinâmico:

- Armazena a página renderizada no servidor, não apenas no build.
- Permite controle granular com opções como `revalidate` ou `no-store`.
- Suporta **Incremental Static Regeneration (ISR)** para atualizações automáticas.

```jsx
export default async function Page() {
  const data = await fetch("https://api.exemplo.com/posts", {
    next: { revalidate: 60 },
  }).then((res) => res.json());
  return <pre>{JSON.stringify(data, null, 2)}</pre>;
}
```

- **Comportamento**: O HTML e os dados são cacheados na primeira renderização. Após 60s, a próxima requisição atualiza o cache automaticamente.

## Benefícios

- **Performance**: Entrega páginas prontas, reduzindo o tempo de resposta.
- **Escalabilidade**: Reutiliza renderizações para múltiplos usuários.
- **Flexibilidade**: Suporta cache estático (SSG), revalidação (ISR) ou renderização dinâmica (SSR).

## Quando Usar?

- **Cache indefinido**: Para páginas estáticas que raramente mudam (ex.: páginas “Sobre”).
- **Revalidação (ISR)**: Para conteúdo que precisa de atualizações periódicas (ex.: blog).
- **Sem cache**: Para páginas dinâmicas que requerem dados em tempo real (ex.: painel de usuário).

## Limitações

- O cache é armazenado no servidor, não no navegador.
- Páginas dinâmicas com `no-store` ou parâmetros dinâmicos (ex.: `dynamic: "force-dynamic"`) ignoram o Full Route Cache.
- Requer configuração cuidadosa para evitar dados desatualizados.
