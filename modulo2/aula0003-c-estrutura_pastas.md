# **Estrutura de Pastas do Next.js**

Ao criar um projeto com `create-next-app`, o Next.js gera uma estrutura de arquivos bem definida, pensada para produtividade e escalabilidade. Abaixo explicamos cada parte dessa estrutura.

---

## 📁 **node_modules**

Contém todas as bibliotecas e dependências instaladas via `npm` ou `yarn`. Não deve ser editada manualmente.

---

## 📁 **public/**

Pasta onde colocamos **arquivos estáticos**, como:

- Imagens (`.jpg`, `.png`, `.svg`)
- Vídeos
- Fontes
- Favicon (`favicon.ico`)

> Tudo dentro da pasta `public` pode ser acessado diretamente pela URL: `http://localhost:3000/nome-do-arquivo`

---

## 📁 **src/**

Pasta principal onde fica todo o **código da aplicação** (componentes, páginas, estilos, hooks, etc).

> Utilizar a estrutura `src/` é uma boa prática para manter o projeto mais organizado.

### 📁 src/app/

Essa pasta é a base do **App Router**, novo sistema de roteamento do Next.js.

Cada subpasta representa uma **rota** e pode conter:

- `page.tsx`: arquivo que representa a página da rota
- `layout.tsx`: define o layout compartilhado entre páginas
- `loading.tsx`: componente de carregamento para a rota
- `error.tsx`: componente de erro específico da rota

Exemplo:

```bash
src/app/
  page.tsx         → rota '/'
  about/
    page.tsx       → rota '/about'
  blog/
    layout.tsx     → layout da seção 'blog'
    page.tsx       → rota '/blog'
```

---

## ⚙️ Arquivos de Configuração

### 📄 **.gitignore**

Informa ao Git quais arquivos/pastas **não devem ser versionados**, como `node_modules`, arquivos temporários, etc.

### 📄 **eslint.config.mjs**

Configuração do **ESLint**, ferramenta que ajuda a manter a padronização e qualidade do código.

### 📄 **next.config.ts** e **next-env.d.ts**

- `next.config.ts`: configurações específicas do Next.js, como domínios de imagens, redirects e middleware.
- `next-env.d.ts`: arquivo gerado automaticamente para integração com o TypeScript.

### 📄 **postcss.config.mjs**

Arquivo de configuração para uso do Tailwind CSS e outros plugins PostCSS.

### 📄 **tsconfig.json**

Arquivo de configuração do **TypeScript**. Define regras de compilação e caminhos do projeto.

### 📄 **README.md**

Documentação inicial do projeto. Ideal para anotar objetivos, instruções de uso e comandos úteis.

---

## 📦 Arquivos de Gerenciamento de Dependências

### 📄 **package.json**

Define:

- Nome do projeto
- Versão
- Scripts
- Dependências e devDependencies

### 📄 **package-lock.json**

Trava a versão exata das dependências, garantindo que a aplicação rode da mesma forma em todas as máquinas.

---

## 📜 Scripts do `package.json`

Exemplo de configuração de scripts:

```json
"scripts": {
  "dev": "next dev --turbopack",
  "build": "next build",
  "start": "next start",
  "lint": "next lint"
}
```

| Script    | Descrição                                                               |
| --------- | ----------------------------------------------------------------------- |
| **dev**   | Roda o projeto em ambiente de desenvolvimento com Turbopack             |
| **build** | Gera os arquivos otimizados para produção                               |
| **start** | Executa o projeto em ambiente de produção                               |
| **lint**  | Executa o ESLint para verificar e corrigir problemas de estilo e padrão |

---

> 📌 **Dica**: para rodar um script, utilize o comando:

```bash
npm run nome-do-script
```

Exemplo:

```bash
npm run dev
```

---

Essa estrutura robusta e modular do Next.js proporciona uma base sólida para construir aplicações escaláveis e performáticas.
