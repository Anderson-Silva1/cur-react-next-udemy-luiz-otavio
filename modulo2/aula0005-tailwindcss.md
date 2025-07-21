# **Vamos aprender sobre o Tailwind CSS**

O **Tailwind CSS** é a mais recente versão do popular framework utilitário de CSS. Ele oferece uma abordagem moderna e altamente produtiva para criar interfaces responsivas com facilidade, diretamente no HTML/JSX. Esta versão traz melhorias de performance, novos recursos e suporte avançado ao modo escuro e responsividade.

---

## 🚀 O que é o Tailwind CSS?

Tailwind é um **framework CSS utilitário-first**, ou seja, ao invés de escrever regras CSS personalizadas, você compõe os estilos diretamente na classe dos elementos HTML usando utilitários pré-definidos.

> 🎯 O foco está na **agilidade no desenvolvimento**, na **padronização visual** e na **redução de CSS não utilizado** no projeto final.

---

## 📦 Novidades do Tailwind CSS 4.1

### ✅ Melhorias de performance com o novo compilador baseado em Rust

- Mais rápido na geração dos estilos
- Build mais leve

### ✅ Suporte oficial a CSS Layers (camadas)

- Maior controle sobre sobreposição de estilos

### ✅ Melhor integração com Variáveis CSS e Tokens de Design

- Permite aplicar temas dinâmicos com facilidade

### ✅ Atualizações no suporte ao modo escuro (`dark mode`)

- Suporte a múltiplos modos de ativação
- Mais controle sobre comportamento em diferentes dispositivos

### ✅ Novos utilitários e melhorias

- Novos utilitários para grid, scroll, interações e animações
- Aprimoramento dos plugins `forms`, `typography`, `aspect-ratio`, entre outros

---

## Usando as classes

Usamos as classes CSS baseadana nomeclatura que o Tailwind nos oferece:

```tsx
<p className="text-red-500 bg-gren-300 font-bold">Teste</p>
```

## Usando classes particulares

Classes particulares são classes que nós mesmos criamos, conseguimos criar ela no `globals.css`

```css
@import "tailwindcss";

:root {
  --background: #ffffff;
  --foreground: #171717;
}

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --font-sans: var(--font-geist-sans);
  --font-mono: var(--font-geist-mono);
}

@media (prefers-color-scheme: dark) {
  :root {
    --background: #0a0a0a;
    --foreground: #ededed;
  }
}

body {
  background: var(--background);
  color: var(--foreground);
  font-family: Arial, Helvetica, sans-serif;
}
```

1. O Tailwind é imoportado dentro do arquivo CSS

2. Criamos um pseudo-seletor do CSS chamado `root` e dentro dele criamos nossas variáveis CSS

3. `@theme` é uma diretiva ao `theme` que está dentro do arquivo de configuração do `tailwindCSS` que está oculto

4. `inline` significa que a variável que será criada será injertada dentro do próprio CSS

5. Dentro de `@theme` teremos novas variáveis que recebem as variáveis CSS que criamos

6. `@media (prefers-color-scheme: dark)` esse sódigo informa que ele será usado quando o tema for escuro, dentro dela teremos um novo pseudo-seletor `root`

7. Podemos usar as estilizações padrões do nosso projeto

---

## 🌙 Modo Escuro

A versão 4.1 oferece melhor suporte ao modo escuro com diferentes estratégias:

```ts
// tailwind.config.ts
export default {
  darkMode: "class", // ou 'media'
};
```

- `"class"`: o modo escuro é ativado com uma classe CSS (`.dark`)
- `"media"`: o modo escuro segue as configurações do sistema operacional

Uso:

```html
<div class="bg-white dark:bg-zinc-900 text-black dark:text-white">
  Conteúdo adaptável ao tema
</div>
```

---

## 📁 Estrutura típica usando Tailwind com Next.js

```bash
src/
  app/
    layout.tsx        # Importa o CSS global com Tailwind
    page.tsx          # Página principal estilizada com classes utilitárias
  styles/
    globals.css       # Importa as diretivas do Tailwind
```

### Exemplo do `globals.css`

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

---

## 📚 Links úteis

- [Documentação oficial do Tailwind CSS](https://tailwindcss.com/docs)
- [Lista de utilitários disponíveis](https://tailwindcss.com/docs/utility-first)
- [Guia de migração para o Tailwind 4](https://tailwindcss.com/blog/tailwindcss-v4)

---

> 💡 O Tailwind 4.1 é ideal para projetos modernos que prezam por **rapidez, consistência visual e responsividade avançada**. Ao dominá-lo, você acelera o desenvolvimento de interfaces profissionais com muito menos esforço.
