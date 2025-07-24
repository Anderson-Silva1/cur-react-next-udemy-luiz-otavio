# 📐 Layout no Next.js (App Router)

No Next.js (com App Router), o componente `layout.tsx` é um ponto central da estrutura de renderização de páginas. Ele é renderizado automaticamente para **todas as rotas abaixo da pasta onde está definido** e tem como função **envolver o conteúdo da aplicação**, garantindo consistência visual e funcional.

---

## 🧱 Estrutura Recomendada de Layout

Um layout pode conter a estrutura base da aplicação, como:

- `Header`: Cabeçalho com navegação principal.
- `Footer`: Rodapé com links ou informações adicionais.
- `Aside`: Seção lateral (opcional).
- `Conteiner`: Componente de container com padding, largura máxima etc.

### Exemplo completo:

```tsx
// app/layout.tsx

import type { Metadata } from "next";

import "./globals.css";

import { Conteiner } from "@/components/Template/Conteiner";
import { Header } from "@/components/Template/Header";
import Footer from "@/components/Template/Footer";

export const metadata: Metadata = {
  title: "The Blog",
  description: "Um blog para estudos com Next.js",
};

export default function RootLayout({
  children,
}: Readonly<{
  children: React.ReactNode;
}>) {
  return (
    <html lang="pt-BR">
      <body>
        <Conteiner>
          <Header />
          {children}
          <Footer />
        </Conteiner>
      </body>
    </html>
  );
}
```

---

# 🧠 Metadata no Next.js

A API de `metadata` do Next.js permite definir informações úteis para **SEO, redes sociais, título da aba, descrição da página e mais**.

### 📌 Exemplo básico:

```tsx
export const metadata: Metadata = {
  title: "The Blog - Um blog de estudos",
  description: "Aprenda Next.js com exemplos práticos",
};
```

Essa metadata será aplicada automaticamente no `<head>` da página.

---

## 🧩 Metadata com título dinâmico (template)

Você pode usar a sintaxe de `template` para definir padrões reutilizáveis para o título da aba:

```tsx
export const metadata: Metadata = {
  title: {
    default: "The Blog - Um blog de estudos",
    template: "%s | The Blog",
  },
  description: "Aprenda Next.js com exemplos práticos",
};
```

### 🧾 Explicação:

- `default`: Título padrão usado quando a rota **não define** seu próprio título.
- `template`: Quando uma página define `metadata.title`, ela será aplicada aqui. Exemplo:

  - Se a página define: `title: "Contato"`, o resultado será `Contato | The Blog`

---

## 🧰 Outras opções de Metadata

A interface `Metadata` do Next.js permite ainda outras propriedades úteis:

### 📷 Open Graph (para redes sociais)

```tsx
export const metadata: Metadata = {
  openGraph: {
    title: "The Blog",
    description: "Conteúdo técnico com Next.js",
    url: "https://theblog.com",
    siteName: "The Blog",
    images: [
      {
        url: "https://theblog.com/og-image.png",
        width: 1200,
        height: 630,
        alt: "Imagem do Blog",
      },
    ],
    locale: "pt_BR",
    type: "website",
  },
};
```

### 📱 Twitter Cards

```tsx
export const metadata: Metadata = {
  twitter: {
    card: "summary_large_image",
    title: "The Blog",
    description: "Conteúdo técnico com Next.js",
    images: ["https://theblog.com/twitter-image.png"],
    creator: "@theblog_author",
  },
};
```

### 🌍 Metadata baseadas em localização ou idioma

```tsx
export const metadata: Metadata = {
  alternates: {
    canonical: "https://theblog.com",
    languages: {
      "en-US": "https://theblog.com/en",
      "pt-BR": "https://theblog.com/pt",
    },
  },
};
```

---

## 🛡️ Boas Práticas

- Centralize o máximo possível de metadados no `layout.tsx` para manter consistência.
- Use metadata específica nas páginas que precisam de SEO individualizado.
- Sempre utilize `lang="pt-BR"` no `<html>` se o conteúdo principal estiver em português.
- Utilize `openGraph` e `twitter` para melhorar o compartilhamento social.

---

Se quiser um layout com suporte a temas (modo claro/escuro), providers, autenticação ou contextos globais (como Clerk, Auth.js ou ThemeProvider), posso te ajudar a montar uma versão avançada também. Basta pedir. ✅
