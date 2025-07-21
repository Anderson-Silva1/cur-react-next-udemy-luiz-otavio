# **Vamos aprender sobre o App Router no Next.js**

O **App Router** é a nova abordagem de roteamento introduzida no Next.js a partir da versão 13. Ele é pensado para criar aplicações modernas com mais flexibilidade, modularidade e performance, substituindo gradualmente o tradicional `pages/` router.

---

## 🚀 O que é o App Router?

- O App Router utiliza a pasta `app/` dentro do `src/` ou na raiz do projeto.
- Ele introduz conceitos como **layouts aninhados**, **rotas segmentadas**, **streaming de dados**, e **loading states** nativos.
- Traz uma experiência mais próxima do que a React propõe com **Server Components** e **Client Components**.

> 🎯 Objetivo: permitir uma construção mais robusta, reutilizável e performática das rotas da aplicação.

---

## 📁 Estrutura Base do App Router

```bash
src/app/
  layout.tsx       # Layout principal da aplicação
  page.tsx         # Página raiz ('/')
  about/
    page.tsx       # Rota '/about'
    loading.tsx    # Tela de loading para '/about'
  dashboard/
    layout.tsx     # Layout específico para rotas dentro de /dashboard
    page.tsx       # Página '/dashboard'
    settings/
      page.tsx     # Página '/dashboard/settings'
```

---

## 📄 Arquivos Especiais

### `layout.tsx`

Define um layout comum entre várias páginas. Permanece intacto durante a navegação entre rotas internas.

```tsx
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="pt-BR">
      <body>{children}</body>
    </html>
  );
}
```

### `page.tsx`

Representa uma **rota individual**. Cada pasta com `page.tsx` será transformada em uma rota correspondente.

### `loading.tsx`

Componente exibido enquanto a rota está sendo carregada.

### `error.tsx`

Permite tratar **erros localmente** para uma rota específica.

### `not-found.tsx`

Página personalizada para erros 404 em uma rota específica.

### `template.tsx`

O `template.tsx` é semelhante ao `layout.tsx`, mas é **reinstanciado a cada navegação**. Isso significa que ele **não preserva estado ou efeitos** entre trocas de rotas.

Use `template.tsx` quando você **precisar isolar o layout por navegação**, como em formulários multi-etapas ou páginas com comportamento que precisa ser resetado a cada visita.

```tsx
export default function Template({ children }: { children: React.ReactNode }) {
  return <div className="p-4 border border-dashed">{children}</div>;
}
```

> ⚠️ Diferente de `layout.tsx`, que é persistente, `template.tsx` **não compartilha estado ou cache** com outras instâncias da rota.

---

## 🧠 Conceitos Importantes

### 🔸 Layouts Aninhados

Cada subpasta pode ter seu próprio layout, o que permite compartilhar estruturas visuais (como menus, barras laterais, etc.) entre rotas relacionadas.

### 🔸 Server Components x Client Components

- Por padrão, os arquivos do App Router são tratados como **Server Components** (executados no servidor).
- Se precisar usar hooks do React (como `useState`, `useEffect`, etc.), adicione no topo do arquivo:

```tsx
"use client";
```

### 🔸 Metadata (SEO)

No App Router, podemos definir metadados por rota diretamente via exportações:

```tsx
export const metadata = {
  title: "Página Inicial",
  description: "Bem-vindo ao meu site",
};
```

---

## ⚙️ Comparação com o Pages Router

| Funcionalidade                | Pages Router | App Router      |
| ----------------------------- | ------------ | --------------- |
| Base de rotas                 | `pages/`     | `app/`          |
| Layouts aninhados             | ❌           | ✅              |
| Roteamento automático         | ✅           | ✅              |
| Server Components             | ❌           | ✅              |
| Loading/Error per route       | ❌           | ✅              |
| SEO com Metadata              | 🔧 Manual    | ✅ Simplificado |
| Layout Resetável (`template`) | ❌           | ✅              |

---

## 🛠️ Dica de Organização

Para projetos grandes, é recomendado criar subpastas dentro de `app/` como:

```bash
src/app/
  components/
  hooks/
  utils/
  services/
```

Você pode também combinar com o uso de **import aliases** (`@/`) para facilitar a importação de arquivos.

---

## 📚 Links úteis

- [Documentação oficial do App Router](https://nextjs.org/docs/app)
- [Guia de Migração do Pages para App Router](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts)
- [Sobre o arquivo template.tsx no App Router](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#templates)

---

> 🧭 O App Router representa o futuro do desenvolvimento com Next.js. Se você está começando hoje, **opte por essa abordagem** e já esteja alinhado com as melhores práticas do ecossistema React moderno.
