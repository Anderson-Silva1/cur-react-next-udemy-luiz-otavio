# Página `not-found.tsx` no Next.js

A página `not-found.tsx` é um dos arquivos **especiais** do framework Next.js. Ela é utilizada automaticamente para renderizar uma interface personalizada quando o usuário tenta acessar uma **rota que não existe**.

## 📁 Estrutura do Arquivo

Este arquivo deve ser criado dentro da pasta `app/`, que representa a raiz das rotas no modelo de roteamento baseado em diretórios do Next.js.

**Exemplo de estrutura:**

```
/app
  └── not-found.tsx
```

Ao criarmos esse arquivo, o Next.js entende que deve usá-lo como **página de erro 404**.

---

## 💻 Exemplo de Implementação

```tsx
import Link from "next/link";

export default function NotFoundPage() {
  return (
    <div className="flex min-h-[60vh] flex-col items-center justify-center space-y-6 text-center">
      <h1 className="text-destructive text-6xl font-bold">404</h1>
      <h2 className="text-2xl font-semibold md:text-3xl">
        Página não encontrada
      </h2>
      <p className="text-muted-foreground max-w-md">
        Ops! Parece que a página que você tentou acessar não existe ou foi
        removida.
      </p>
      <Link href="/">
        <button className="mt-4 cursor-pointer rounded-4xl bg-slate-700 p-4 font-bold text-white">
          Voltar para a página inicial
        </button>
      </Link>
    </div>
  );
}
```

---

## 🧠 Quando ela é acionada?

Essa página será exibida automaticamente **sempre que uma rota inexistente for acessada**. Ou seja, ela serve como fallback padrão para URLs inválidas dentro do seu projeto.

---

## 🧪 Uso programático com `notFound()`

Além do uso automático, você também pode lançar a página de "não encontrada" manualmente dentro de qualquer componente de servidor (Server Component) usando o helper `notFound()` do Next.js.

**Exemplo:**

```tsx
import { notFound } from "next/navigation";

export default function MeuComponente({ params }: { params: { id: string } }) {
  const item = buscarItemPorId(params.id);

  if (!item) {
    notFound();
  }

  return <div>{item.nome}</div>;
}
```

> ⚠️ A função `notFound()` **só pode ser usada em componentes server-side** no Next.js. Se você tentar usá-la em client components, ela não funcionará.

---

## ✅ Boas práticas

- Personalize a página com uma identidade visual condizente com o restante do seu sistema.
- Inclua links de navegação úteis (como "Voltar para a Home").
- Use mensagens claras e empáticas para melhorar a experiência do usuário.

---

## 🧾 Conclusão

A página `not-found.tsx` é uma ferramenta fundamental para melhorar a usabilidade e a experiência do usuário em aplicações Next.js. Ela trata com elegância os acessos inválidos e oferece a oportunidade de orientar o usuário de volta a caminhos válidos dentro da aplicação.
