# 📝 Renderizando Markdown com Segurança no React

Markdown é uma linguagem de marcação leve, ideal para criar textos formatados de forma simples e legível. É amplamente usada em arquivos `README`, documentações técnicas, blogs e editores de conteúdo devido à sua sintaxe intuitiva e versatilidade.

## ❔ Por que evitar HTML puro?

Embora o HTML seja poderoso para estruturar conteúdo, seu uso direto pode ser arriscado, pois permite a execução de scripts JavaScript embutidos. Isso cria vulnerabilidades para ataques como **XSS (Cross-Site Scripting)**, onde códigos maliciosos podem ser injetados, comprometendo a segurança dos usuários ou da aplicação.

## 🛠️ Solução: Usando `react-markdown`

A biblioteca [`react-markdown`](https://github.com/remarkjs/react-markdown) oferece uma forma segura de renderizar Markdown como componentes React. Ela converte strings Markdown em elementos React sem executar JavaScript embutido, garantindo proteção contra XSS. Além disso, suporta plugins para personalizar e estender a funcionalidade de renderização.

## 📦 Instalação das Dependências

Para começar, instale os pacotes necessários:

```bash
npm install react-markdown rehype-sanitize remark-gfm
```

- **`react-markdown`**: Converte Markdown em componentes React.
- **`rehype-sanitize`**: Remove tags e atributos HTML potencialmente perigosos, garantindo a segurança do conteúdo gerado.
- **`remark-gfm`**: Adiciona suporte ao **GitHub Flavored Markdown (GFM)**, que inclui recursos como tabelas, listas de tarefas, links automáticos e mais.

## 🖥️ Exemplo de Código em React

Aqui está um exemplo de como implementar um componente para renderizar Markdown com segurança:

```tsx
import ReactMarkdown from "react-markdown";
import rehypeSanitize from "rehype-sanitize";
import remarkGfm from "remark-gfm";

interface MarkdownContentProps {
  contentMarkdown: string;
}

export function MarkdownContent({ contentMarkdown }: MarkdownContentProps) {
  return (
    <div className="prose dark:prose-invert max-w-none">
      <ReactMarkdown
        rehypePlugins={[rehypeSanitize]}
        remarkPlugins={[remarkGfm]}
      >
        {contentMarkdown}
      </ReactMarkdown>
    </div>
  );
}
```

### Explicação do Código

- **`"use client"`** (opcional): Se necessário, adicione esta diretiva para indicar que o componente é executado no lado do cliente (React Client Component).
- **`interface MarkdownContentProps`**: Define o tipo da prop `contentMarkdown`, que recebe a string Markdown a ser renderizada.
- **`prose`**: Classe do Tailwind CSS que aplica estilos tipográficos elegantes ao conteúdo Markdown.
- **`dark:prose-invert`**: Ajusta os estilos para modo escuro, garantindo boa legibilidade.
- **`max-w-none`**: Remove a largura máxima padrão da classe `prose` para maior flexibilidade.
- **`rehypePlugins` e `remarkPlugins`**: Configuram os plugins `rehype-sanitize` e `remark-gfm` para segurança e suporte a GFM.

## 🎨 Estilizando com Tailwind CSS v4

O Tailwind CSS v4 simplifica a estilização de conteúdo Markdown com o plugin `@tailwindcss/typography`, que fornece classes utilitárias para tipografia consistente e responsiva.

### Instalação do Tailwind CSS e Plugin de Tipografia

```bash
npm install -D tailwindcss @tailwindcss/typography
```

### Configuração no `globals.css`

Adicione o plugin de tipografia diretamente no arquivo CSS global:

```css
@import "tailwindcss";
@plugin "@tailwindcss/typography";
```

Essa configuração torna as classes `prose` disponíveis sem a necessidade de modificar o arquivo `tailwind.config.js`.

### Exemplo Aprimorado com Tailwind CSS v4

```tsx
import ReactMarkdown from "react-markdown";
import rehypeSanitize from "rehype-sanitize";
import remarkGfm from "remark-gfm";

interface MarkdownContentProps {
  contentMarkdown: string;
}

export function MarkdownContent({ contentMarkdown }: MarkdownContentProps) {
  return (
    <div className="prose dark:prose-invert max-w-none prose-a:text-blue-600 prose-a:hover:text-blue-800 prose-img:mx-auto md:prose-lg w-full overflow-hidden transition-colors duration-200">
      <ReactMarkdown
        rehypePlugins={[rehypeSanitize]}
        remarkPlugins={[remarkGfm]}
      >
        {contentMarkdown}
      </ReactMarkdown>
    </div>
  );
}
```

### Detalhes do Estilo

- **`prose-a:text-blue-600`**: Define a cor dos links como azul.
- **`prose-a:hover:text-blue-800`**: Aplica uma transição de cor ao passar o mouse sobre links.
- **`prose-img:mx-auto`**: Centraliza imagens horizontalmente.
- **`md:prose-lg`**: Aumenta o tamanho da tipografia em telas maiores (≥768px).
- **`transition-colors duration-200`**: Suaviza transições de cores para uma melhor experiência visual.

## 🛡️ Boas Práticas para Renderização Segura

1. **Sanitização Obrigatória**: Sempre use `rehype-sanitize` para evitar vulnerabilidades XSS.
2. **Suporte a GFM**: Inclua `remark-gfm` para suportar recursos populares do GitHub, como tabelas e listas de tarefas.
3. **Estilização Consistente**: Utilize o plugin `prose` do Tailwind para garantir uma aparência profissional e responsiva.
4. **Testes de Conteúdo**: Valide a renderização com diferentes tipos de Markdown (tabelas, listas, imagens, etc.) para garantir compatibilidade.
5. **Acessibilidade**: Certifique-se de que o conteúdo renderizado segue boas práticas de acessibilidade, como contraste adequado e suporte a leitores de tela.

## 🔗 Recursos Úteis

- [Documentação do `react-markdown`](https://github.com/remarkjs/react-markdown): Guia oficial da biblioteca.
- [CommonMark](https://commonmark.org/help/): Referência para a sintaxe Markdown padrão.
- [Playground do `react-markdown`](https://remarkjs.github.io/react-markdown/): Teste a renderização de Markdown interativamente.
- [Tailwind CSS Typography](https://tailwindcss.com/docs/typography-plugin): Documentação do plugin de tipografia do Tailwind.

---

> Com essa abordagem, você pode renderizar Markdown de forma segura, estilizada e responsiva, garantindo uma experiência de usuário robusta e protegida contra vulnerabilidades.
