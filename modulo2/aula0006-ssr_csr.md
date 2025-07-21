# Client-Side Rendering (CSR) vs Server-Side Rendering (SSR) no Next.js (App Router)

No **Next.js**, utilizando o modelo de roteamento moderno (`App Router`), todos os componentes são, por padrão, **Server Components** — ou seja, são renderizados no servidor.

## ✅ Server Components (SSR por padrão)

- **Não têm acesso ao DOM** nem às APIs do navegador.
- Não é possível usar `console.log()`, `window`, `document`, `useState`, `useEffect`, ou qualquer outro recurso client-side.
- São ideais para carregamento de dados assíncronos, interações com banco de dados, chamadas a APIs seguras e renderização otimizada.
- Entregam HTML estático já pronto para o navegador, otimizando performance, tempo de carregamento e SEO.

## 🔁 Quando usar Client Components (CSR)

Se o seu componente precisa de:

- Interatividade do usuário
- Hooks do React como `useState`, `useEffect`, `useReducer`, etc.
- Eventos de clique, entrada de dados ou animações que dependem do DOM

Você deve marcá-lo como um **Client Component**, adicionando a diretiva no topo do arquivo:

```tsx
"use client";
```

Isso permite que o componente seja renderizado no lado do cliente (CSR) e tenha acesso total ao ambiente do navegador.

> ⚠️ **Atenção**: ao tornar um componente client-side, **você perde os benefícios do SSR** naquele componente, como renderização assíncrona no servidor e carregamento eficiente de dados.

## 🔒 Segurança e Vazamento de Dados

É crucial **não expor dados sensíveis ou lógicos de servidor** em componentes Client. Como o código client-side é incluído no bundle enviado ao navegador, qualquer dado usado ali poderá ser inspecionado pelo usuário.

Evite deixar:

- Dados confidenciais em props
- Funções de lógica de backend
- Chaves de API ou tokens

## 📌 Boas Práticas de Composição

- Nunca tente importar um **Server Component dentro de um Client Component**.

  - O fluxo de renderização **deve vir de cima para baixo**, ou seja:

    - Um Server Component pode conter Client Components.
    - Mas um Client Component **não pode conter** Server Components.

### ✅ Correto:

```tsx
// Server Component
import MeuComponenteCliente from "./MeuComponenteCliente";

export default function Page() {
  return (
    <div>
      <MeuComponenteCliente />
    </div>
  );
}
```

### ❌ Incorreto:

```tsx
"use client"; // Client Component

import MeuComponenteServidor from "./MeuComponenteServidor"; // ERRO
```

## 📚 Estratégia recomendada

1. Crie componentes pequenos e bem definidos.
2. Separe os de lógica e carregamento de dados (SSR) daqueles de interatividade (CSR).
3. Use o padrão:

   - `layout.tsx` e `page.tsx` como Server Components.
   - Componentes interativos (ex: formulários, botões, switches) como Client Components com `'use client'`.

---

### 💡 Resumo

| Tipo de Componente | Acesso ao DOM | Acesso a Hooks do React | Ideal para...                     |
| ------------------ | ------------- | ----------------------- | --------------------------------- |
| Server Component   | ❌            | ❌                      | SEO, performance, lógica de dados |
| Client Component   | ✅            | ✅                      | Interatividade, eventos, hooks    |

Utilize com sabedoria os "super poderes" do SSR e do CSR no Next.js, compondo seus componentes conforme a necessidade real da aplicação. Assim, você garante eficiência, segurança e uma arquitetura escalável. 🚀
