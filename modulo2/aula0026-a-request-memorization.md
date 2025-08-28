# Request Memoization no Next.js

## O que é Request Memoization?

Request Memoization é uma técnica do Next.js que evita requisições duplicadas durante o processamento de uma única solicitação no servidor. Ela armazena (memoiza) o resultado de chamadas assíncronas, como `fetch` ou consultas a banco de dados, e reaproveita esses resultados quando a mesma chamada é feita com os mesmos parâmetros, otimizando a performance.

### Exemplo Prático

Imagine uma página que chama repetidamente uma função para buscar dados:

```jsx
// app/page.tsx
async function getData() {
  return await fetch("https://api.exemplo.com/users").then((res) => res.json());
}

export default async function Page() {
  const data1 = await getData();
  const data2 = await getData(); // Chamada repetida
  const data3 = await getData(); // Chamada repetida
  return (
    <div>
      <pre>{JSON.stringify(data1, null, 2)}</pre>
      <pre>{JSON.stringify(data2, null, 2)}</pre>
      <pre>{JSON.stringify(data3, null, 2)}</pre>
    </div>
  );
}
```

- **Sem memoization**: Cada chamada a `getData()` faria uma nova requisição à API, mesmo com os mesmos parâmetros.
- **Com memoization**: O Next.js executa apenas **uma requisição** e armazena o resultado em cache, reutilizando-o nas chamadas subsequentes durante o mesmo ciclo de renderização.

## Como Funciona Internamente?

O Next.js implementa a memoization envolvendo funções como `fetch` ou consultas assíncronas com um sistema de cache. Ele gera uma **chave única** com base nos parâmetros da requisição (como URL e opções no caso de `fetch`, ou argumentos de uma função). Se uma chamada idêntica é detectada, o valor em cache é retornado, evitando nova execução.

### Benefícios

- **Redução de chamadas**: Evita múltiplas requisições idênticas, diminuindo a carga em APIs ou bancos de dados.
- **Melhor performance**: Acelera o Server-Side Rendering (SSR) e React Server Components.
- **Eficiência**: Menor consumo de recursos em serviços externos.

## Como Utilizar?

Use a função `cache` do React para memoizar funções assíncronas, especialmente em consultas a bancos de dados:

```tsx
import { cache } from "react";
import db from "@/lib/db";

export const getItem = cache(async (id: string) => {
  const item = await db.item.findUnique({ where: { id } });
  return item;
});
```

- **O que acontece?** A função `getItem` é memoizada. Se chamada várias vezes com o mesmo `id` durante uma única renderização, apenas **uma consulta** ao banco de dados é feita, e o resultado é reutilizado.

### Quando Usar?

- Em páginas ou componentes que realizam múltiplas chamadas assíncronas com os mesmos parâmetros.
- Em Server Components ou SSR, onde a memoization é aplicada automaticamente para `fetch` e funções memoizadas com `cache`.

### Limitações

- O cache é válido apenas durante o ciclo de vida de uma única requisição no servidor.
- Não substitui soluções de cache persistente (como `getStaticProps` ou ferramentas como Redis).
