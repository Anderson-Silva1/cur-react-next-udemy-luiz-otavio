# Data Cache no Next.js

## O que é Data Cache?

O **Data Cache** é um mecanismo integrado do Next.js que armazena resultados de requisições `fetch` no servidor, permitindo sua reutilização entre diferentes solicitações, usuários e dispositivos. Diferente do _Request Memoization_, que é temporário e válido apenas durante uma única renderização, o Data Cache é **persistente** e permanece até ser invalidado ou atualizado.

## Como Funciona?

O Next.js estende a API `fetch` nativa, adicionando uma layer de cache no servidor. Quando uma requisição é feita, o resultado é armazenado no **Data Cache** e pode ser reutilizado, reduzindo chamadas externas desnecessárias. O comportamento do cache é controlado por opções específicas:

- **Padrão**: Cache indefinido (resultados armazenados até uma mudança manual ou revalidação).
- **Revalidação**: Configura um tempo de expiração para o cache (Incremental Static Regeneration - ISR).
- **Sem cache**: Ignora o cache, buscando dados novos a cada requisição.

### Exemplos Práticos

#### 1. Cache Indefinido (Padrão)

```jsx
const data = await fetch("https://api.exemplo.com/users").then((res) =>
  res.json()
);
```

- **Comportamento**: O resultado é armazenado no Data Cache e reutilizado em todas as renderizações futuras, até uma nova implantação ou revalidação.

#### 2. Cache com Revalidação (ISR)

```jsx
const data = await fetch("https://api.exemplo.com/users", {
  next: { revalidate: 60 }, // Cache válido por 60 segundos
}).then((res) => res.json());
```

- **Comportamento**: O cache é reutilizado por 60 segundos. Após esse período, o Next.js busca novos dados na próxima requisição, mantendo a eficiência do ISR.

#### 3. Sem Cache

```jsx
const data = await fetch("https://api.exemplo.com/users", {
  cache: "no-store",
}).then((res) => res.json());
```

- **Comportamento**: A requisição é feita diretamente à API em todas as renderizações, ideal para dados em tempo real, como saldos bancários.

## Diferença entre Data Cache e Request Memoization

| **Aspecto** | **Data Cache**                                      | **Request Memoization**                         |
| ----------- | --------------------------------------------------- | ----------------------------------------------- |
| **Escopo**  | Persiste entre requisições, usuários e dispositivos | Válido apenas durante uma única renderização    |
| **Duração** | Até revalidação ou nova implantação                 | Até o fim do ciclo de renderização              |
| **Uso**     | Cache de dados no servidor                          | Evita chamadas duplicadas na mesma renderização |

## Benefícios do Data Cache

- **Performance**: Reduz chamadas externas, acelerando o carregamento de páginas.
- **Escalabilidade**: Reutiliza dados para múltiplos usuários, diminuindo carga em APIs.
- **Flex Flexibilidade**: Permite controle fino com opções como `revalidate` e `no-store`.

## Como Utilizar?

Use `fetch` em Server Components com as opções desejadas:

```jsx
// Exemplo com cache padrão
async function getUsers() {
  const data = await fetch("https://api.exemplo.com/users").then((res) =>
    res.json()
  );
  return data;
}

// Exemplo com revalidação
async function getUsersWithISR() {
  const data = await fetch("https://api.exemplo.com/users", {
    next: { revalidate: 60 },
  }).then((res) => res.json());
  return data;
}
```

### Quando Usar?

- **Cache padrão**: Para dados que mudam raramente (ex.: lista de produtos).
- **Revalidação (ISR)**: Para dados que precisam de atualização periódica (ex.: notícias).
- **Sem cache**: Para dados dinâmicos em tempo real (ex.: preços de ações).

## Limitações

- O Data Cache reside no servidor, não no navegador, e é gerenciado pelo Next.js.
- Não substitui soluções de cache persistente externas (ex.: Redis).
- Requer configuração adequada para evitar dados desatualizados em cenários dinâmicos.
