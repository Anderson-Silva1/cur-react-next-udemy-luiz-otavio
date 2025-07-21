# 🎡 Criando o nosso `SpinLoader`

O componente `SpinLoader` é um **indicador visual de carregamento** (loader) com animação contínua de rotação. Ele pode ser utilizado em diversas partes da aplicação para indicar ao usuário que algo está sendo processado ou carregado em segundo plano.

Trata-se de uma solução elegante, reutilizável e compatível com o ecossistema React, usando utilitários do TailwindCSS para estilização.

---

## 🚧 Implementação

```tsx
interface SpinLoaderProps {
  backgroundSize?: string;
}

export const SpinLoader = ({ backgroundSize }: SpinLoaderProps) => {
  return (
    <div
      className={`flex items-center justify-center ${
        backgroundSize ? backgroundSize : "min-h-screen min-w-screen"
      }`}
    >
      <div className="h-10 w-10 animate-spin rounded-full border-6 border-slate-900 border-t-transparent"></div>
    </div>
  );
};
```

---

## 🧠 Conceitos aplicados

### 🧱 Props dinâmicas

- A prop `backgroundSize` permite customizar o tamanho do contêiner pai. Por padrão, ele ocupa toda a tela (`min-h-screen min-w-screen`), mas você pode sobrescrevê-lo com algo como `h-[300px] w-full`, por exemplo.

### 🌀 Estilização do Loader

```tsx
<div className="h-10 w-10 animate-spin rounded-full border-6 border-slate-900 border-t-transparent"></div>
```

- `h-10 w-10`: Define altura e largura do spinner.
- `rounded-full`: Transforma a `div` num círculo perfeito.
- `border-6`: Espessura da borda do círculo.
- `border-slate-900`: Cor da borda.
- `border-t-transparent`: Faz o topo do círculo ficar transparente, criando o efeito de rotação.
- `animate-spin`: Aplica a animação de rotação contínua, fornecida pelo Tailwind.

---

## ✅ Uso recomendado

O `SpinLoader` pode ser utilizado nos seguintes contextos:

- Durante carregamento de dados assíncronos (APIs);
- Ao renderizar rotas com carregamento em `Next.js`;
- Em formulários que realizam `submit` com feedback visual;
- Em dashboards que requerem interações com gráficos ou relatórios;
- Sempre que o usuário precisar de uma indicação visual de que algo está "em andamento".

---

## 💡 Exemplo de uso

```tsx
import { SpinLoader } from "./components/SpinLoader";

export default function Home() {
  const isLoading = true;

  return (
    <main>
      {isLoading ? (
        <SpinLoader backgroundSize="h-[300px] w-full" />
      ) : (
        <Content />
      )}
    </main>
  );
}
```

---

## 🧩 Considerações finais

- O componente é simples, porém altamente reutilizável e personalizável.
- Sua combinação com Tailwind oferece flexibilidade para diferentes tamanhos e contextos.
- Pode ser extendido facilmente para suportar mensagens de loading ou variantes de tamanho e cor.

> "Interfaces intuitivas também precisam de feedbacks visuais claros. Um bom loader melhora a experiência do usuário mesmo nos tempos de espera."
