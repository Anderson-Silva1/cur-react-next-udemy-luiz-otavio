# Router Cache no Next.js

## O que é Router Cache?

O **Router Cache** é um mecanismo do Next.js que opera no **lado do cliente** (navegador), diferente dos outros caches (Request Memoization, Data Cache e Full Route Cache), que residem no servidor. Ele armazena o estado de rotas visitadas, incluindo:

- **Payload JSON**: Dados recebidos do servidor.
- **HTML renderizado**: Estrutura da página já processada.
- **Estado do React**: Componentes e estados associados à rota.

Isso permite **navegação instantânea** entre páginas sem recarregar ou fazer novas requisições ao servidor, desde que os dados estejam no cache.

## Como Funciona?

O Router Cache é ativado quando o usuário navega entre páginas usando o componente `<Link>` do Next.js. Ele armazena as rotas visitadas ou pré-carregadas no navegador, permitindo transições rápidas e suaves.

### Fluxo Prático

1. O usuário acessa a página inicial (`/`).
2. O servidor entrega o HTML e o payload JSON, que são salvos no **Router Cache** no navegador.
3. O usuário navega para `/products` usando um `<Link>`.
4. O Next.js verifica o Router Cache. Se `/products` já está cacheado (via prefetch ou visita anterior), a página é renderizada instantaneamente sem nova requisição.
5. Ao voltar para `/`, o cache é usado novamente, evitando recarregamento.

## O Papel do Prefetch

O **prefetch** é uma funcionalidade do `<Link>` que melhora a experiência de navegação ao carregar recursos de rotas em segundo plano, antes mesmo do usuário clicar no link. Ele é essencial para o funcionamento eficiente do Router Cache.

### Como o Prefetch Funciona?

- Quando uma página contém um `<Link>` (ex.: `<Link href="/products">`), o Next.js automaticamente faz o **prefetch** do payload JSON e do HTML da rota destino (`/products`) assim que a página atual carrega.
- Esses dados são armazenados no **Router Cache** no navegador.
- Ao clicar no `<Link>`, a navegação é instantânea, pois os dados já estão disponíveis localmente.

#### Exemplo com `<Link>`

```jsx
import Link from "next/link";

export default function Page() {
  return (
    <nav>
      <Link href="/products">Produtos</Link>
      <Link href="/about">Sobre</Link>
    </nav>
  );
}
```

- **O que acontece**:

  1. Ao carregar a página, o Next.js faz o **prefetch** de `/products` e `/about` em segundo plano.
  2. Os payloads dessas rotas são salvos no Router Cache.
  3. Ao clicar em `<Link href="/products">`, a página `/products` é renderizada imediatamente, sem requisição ao servidor.

- **Sem prefetch**:
  - Se o prefetch estiver desativado (`<Link href="/products" prefetch={false}>`), a rota só será buscada no momento do clique, resultando em um pequeno atraso.

### Diferença entre `<Link>` e `<a>`

- **`<Link>`**:

  - Usa navegação no lado do cliente (client-side navigation).
  - Aciona o prefetch automaticamente (a menos que desativado).
  - Reutiliza dados do Router Cache para transições rápidas.
  - Não causa recarregamento completo da página.

- **`<a>`**:
  - Faz um recarregamento completo da página (full page reload).
  - Ignora o Router Cache.
  - Perde os benefícios de navegação instantânea.

### Configurações do Prefetch

- **Padrão**: O prefetch é ativado automaticamente para todas as rotas vinculadas por `<Link>` em produção, desde que sejam rotas estáticas ou com revalidação (ISR).
- **Desativar prefetch**: Use `prefetch={false}` no `<Link>` para evitar o pré-carregamento, útil em casos de rotas dinâmicas ou para economizar recursos.

  ```jsx
  <Link href="/products" prefetch={false}>
    Produtos
  </Link>
  ```

- **Prefetch manual**: Use `router.prefetch('/rota')` com o hook `useRouter` para carregar rotas sob demanda.

## Duração do Router Cache

- O Router Cache é mantido durante a **sessão do usuário** no navegador.
- Ele é limpo quando:
  - O usuário recarrega a página (F5 ou reload completo).
  - O navegador é fechado.
  - O cache atinge o limite de tamanho ou tempo (configurável, mas por padrão é mantido por toda a sessão).

## Comparação com Outros Caches

| **Cache**               | **Onde Vive**       | **Duração**                 | **O que Evita**                                       |
| ----------------------- | ------------------- | --------------------------- | ----------------------------------------------------- |
| **Request Memoization** | Servidor (request)  | Durante uma renderização    | Chamadas duplicadas no mesmo request                  |
| **Data Cache**          | Servidor (global)   | Entre requests              | Buscar dados repetidamente                            |
| **Full Route Cache**    | Servidor (global)   | Entre requests              | Re-renderizar rotas inteiras                          |
| **Router Cache**        | Cliente (navegador) | Durante a sessão do usuário | Novas requisições ao servidor para rotas já visitadas |

## Benefícios

- **Navegação instantânea**: Transições entre páginas são rápidas, graças ao prefetch e ao Router Cache.
- **Melhor UX**: Evita recarregamentos completos, mantendo a experiência fluida.
- **Economia de recursos**: Reduz requisições ao servidor para rotas já cacheadas.

## Limitações

- Funciona apenas com navegação via `<Link>` ou `router.push`/`router.replace`.
- Não armazena dados permanentemente; o cache é perdido ao recarregar a página ou fechar o navegador.
- Rotas dinâmicas (ex.: com `dynamic: "force-dynamic"`) não são cacheadas.

## Quando Usar?

- Para melhorar a navegação em aplicações com muitas páginas estáticas ou com ISR.
- Em cenários onde a experiência do usuário é prioridade, como em e-commerces ou blogs.
- Para evitar requisições desnecessárias em rotas frequentemente acessadas.
