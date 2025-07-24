# 🚀 **Guia Completo: Configurando Drizzle ORM com SQLite 3 em Projetos TypeScript**

O **Drizzle ORM** é uma biblioteca moderna, leve e altamente tipada para mapeamento objeto-relacional (ORM) em projetos JavaScript e TypeScript. Ele combina desempenho, simplicidade e uma integração robusta com TypeScript, permitindo que desenvolvedores interajam com bancos de dados de forma segura e intuitiva, sem abrir mão do controle sobre o SQL. Neste guia, vamos configurar o Drizzle ORM para usar o **SQLite 3**, um banco de dados leve, armazenado em um único arquivo, ideal para protótipos, aplicações locais ou sistemas embarcados.

Vamos criar um projeto que gerencia uma tabela de **posts** (publicações), com campos como título, slug, conteúdo e autor. O guia inclui instalação, configuração, definição de esquemas, conexão com o banco, geração de migrações e exemplos de uso, tudo explicado passo a passo.

---

## 🧱 **Etapa 1: Instalando as Dependências**

Para começar, precisamos instalar os pacotes necessários para integrar o Drizzle ORM com o SQLite 3. Vamos usar dois comandos para instalar dependências de produção e desenvolvimento.

### 🔹 **Passo 1.1 – Instalar o Drizzle ORM e o driver SQLite**

```bash
npm install drizzle-orm better-sqlite3
```

**Explicação:**

- **`npm install`**: Comando do npm (Node Package Manager) para instalar pacotes no projeto, adicionando-os ao diretório `node_modules` e atualizando o arquivo `package.json`.
- **`drizzle-orm`**: Pacote principal do Drizzle ORM, que fornece funções para definir tabelas, realizar consultas (SELECT, INSERT, UPDATE, DELETE) e gerenciar o banco de dados com segurança tipada.
- **`better-sqlite3`**: Driver oficial para SQLite no Node.js, conhecido por ser síncrono, rápido e eficiente. É recomendado pelo Drizzle para integração com SQLite devido ao seu desempenho e suporte a transações.

**O que acontece?** Após executar esse comando, o `package.json` lista `drizzle-orm` e `better-sqlite3` como dependências de produção, e os pacotes são baixados para o projeto.

### 🔹 **Passo 1.2 – Instalar dependências de desenvolvimento**

```bash
npm install -D @types/better-sqlite3 drizzle-kit
```

**Explicação:**

- **`-D`**: Flag que indica que os pacotes são dependências de desenvolvimento, usadas apenas durante o desenvolvimento (ex.: para gerar migrações ou compilar TypeScript), não em produção.
- **`@types/better-sqlite3`**: Fornece tipagens TypeScript para o `better-sqlite3`, garantindo que o TypeScript reconheça as APIs do driver, oferecendo autocompletar e verificação de tipos no código.
- **`drizzle-kit`**: Ferramenta CLI (interface de linha de comando) do Drizzle, usada para:
  - Gerar arquivos de migração com base no esquema.
  - Aplicar migrações ao banco.
  - Executar o Drizzle Studio, uma interface web para explorar o banco de dados.

**O que acontece?** Esses pacotes são adicionados como dependências de desenvolvimento no `package.json`, e o `drizzle-kit` permite gerenciar o esquema do banco.

---

## ⚙️ **Etapa 2: Criando o Arquivo de Configuração `drizzle.config.ts`**

Na raiz do projeto, criamos o arquivo `drizzle.config.ts` para configurar o **Drizzle Kit**, a ferramenta que gerencia migrações e sincroniza o esquema com o banco SQLite.

### 📄 **Arquivo: `drizzle.config.ts`**

```tsx
import { defineConfig } from "drizzle-kit";

export default defineConfig({
  out: "./src/db/drizzle/migrations",
  schema: "./src/db/drizzle/schema.ts",
  dialect: "sqlite",
  dbCredentials: {
    url: "./db.sqlite3",
  },
});
```

**Explicação linha por linha:**

- **`import { defineConfig } from "drizzle-kit";`**

  - Importa a função `defineConfig` do pacote `drizzle-kit`. Essa função cria uma configuração tipada para o Drizzle Kit, garantindo que as opções fornecidas sejam válidas e consistentes.

- **`export default defineConfig({ ... });`**

  - Exporta a configuração padrão do Drizzle Kit, usando `defineConfig` para encapsular as opções em um objeto. Isso é necessário para que o Drizzle Kit saiba como processar migrações e se conectar ao banco.

- **`out: "./src/db/drizzle/migrations"`**

  - Define o diretório onde os arquivos de migração serão gerados. Quando você executar o comando `npx drizzle-kit generate:sqlite`, o Drizzle Kit criará arquivos SQL (ou JavaScript) na pasta `src/db/drizzle/migrations`. Esses arquivos contêm instruções para criar ou modificar tabelas no banco.
  - Exemplo: Um arquivo de migração pode conter o comando SQL para criar a tabela `posts`.

- **`schema: "./src/db/drizzle/schema.ts"`**

  - Especifica o caminho para o arquivo que contém as definições das tabelas do banco (neste caso, `schema.ts`). O Drizzle Kit usa esse arquivo para entender a estrutura do banco e gerar migrações correspondentes.
  - **Nota:** No guia original, havia um erro de digitação (`shemas.ts`). Aqui, corrigimos para `schema.ts`, que é o nome do arquivo fornecido posteriormente.

- **`dialect: "sqlite"`**

  - Informa ao Drizzle Kit que o banco de dados usado é o SQLite. Isso garante que os comandos SQL gerados sejam compatíveis com a sintaxe do SQLite.

- **`dbCredentials: { url: "./db.sqlite3" }`**
  - Define as credenciais de conexão com o banco SQLite. No caso do SQLite, a "conexão" é simplesmente o caminho para o arquivo do banco (`db.sqlite3`), que será criado na raiz do projeto se não existir.
  - O SQLite armazena todo o banco em um único arquivo, tornando-o portátil e fácil de gerenciar.

**Observação:** Certifique-se de que o diretório raiz do projeto tem permissões de escrita, pois o SQLite criará o arquivo `db.sqlite3` automaticamente ao aplicar as migrações.

---

## 🗂️ **Etapa 3: Estrutura de Pastas Recomendadas**

Organizar o projeto de forma clara facilita a manutenção e escalabilidade. A estrutura sugerida é:

```
src/
  db/
    drizzle/
      index.ts       ← Configuração da conexão com o banco
      schema.ts      ← Definição das tabelas (esquema)
      migrations/    ← Arquivos de migração gerados pelo Drizzle Kit
```

**Explicação:**

- **`src/db/drizzle/`**: Pasta dedicada ao Drizzle ORM, contendo:
  - **`index.ts`**: Configura a conexão com o SQLite e inicializa o Drizzle ORM.
  - **`schema.ts`**: Define as tabelas do banco e seus tipos TypeScript.
  - **`migrations/`**: Diretório criado automaticamente pelo comando `npx drizzle-kit generate:sqlite` para armazenar arquivos de migração (SQL ou JavaScript).

Essa estrutura mantém o código relacionado ao banco de dados organizado e separado do restante da aplicação.

---

## 🧬 **Etapa 4: Definindo o Esquema do Banco de Dados**

No arquivo `schema.ts`, definimos a tabela `posts`, que será usada para armazenar publicações com campos como título, slug, conteúdo e autor.

### 📄 **Arquivo: `src/db/drizzle/schema.ts`**

```tsx
import { integer, sqliteTable, text } from "drizzle-orm/sqlite-core";
import { InferInsertModel, InferSelectModel } from "drizzle-orm";

export const postsTable = sqliteTable("posts", {
  id: text("id").primaryKey(),
  title: text("title").notNull(),
  slug: text("slug").notNull().unique(),
  excerpt: text("excerpt").notNull(),
  content: text("content").notNull(),
  coverImageUrl: text("cover_image_url").notNull(),
  published: integer("published", { mode: "boolean" }).notNull(),
  createdAt: text("created_at").notNull(),
  updatedAt: text("updated_at").notNull(),
  author: text("author").notNull(),
});

export type PostTableSelectMode = InferSelectModel<typeof postsTable>;
export type PostTableSelectMode = InferInsertModel<typeof postsTable>;
```

**Explicação linha por linha:**

- **`import { integer, sqliteTable, text } from "drizzle-orm/sqlite-core";`**

  - Importa funções específicas para SQLite do módulo `drizzle-orm/sqlite-core`:
    - **`sqliteTable`**: Função para definir tabelas compatíveis com o SQLite.
    - **`text`**: Define colunas do tipo `TEXT` (strings no SQLite).
    - **`integer`**: Define colunas do tipo `INTEGER`, usado aqui para um campo booleano.

- **`import { InferInsertModel, InferSelectModel } from "drizzle-orm";`**

  - Importa utilitários do Drizzle ORM para gerar tipos TypeScript automaticamente:
    - **`InferSelectModel`**: Cria um tipo para registros retornados por consultas `SELECT` (leitura).
    - **`InferInsertModel`**: Cria um tipo para objetos usados em operações `INSERT` (inserção).

- **`export const postsTable = sqliteTable("posts", { ... });`**

  - Define uma tabela chamada `posts` no banco SQLite. O primeiro argumento (`"posts"`) é o nome da tabela no banco. O segundo argumento é um objeto que define as colunas da tabela.

- **Colunas da tabela `posts`:**

  - **`id: text("id").primaryKey()`**
    - Cria uma coluna `id` do tipo `text`, que armazena uma string (ex.: `"post-1"` ou um UUID). O método `.primaryKey()` define esta coluna como a chave primária, garantindo que cada valor seja único e não nulo.
  - **`title: text("title").notNull()`**
    - Define uma coluna `title` do tipo `text` para armazenar o título do post (ex.: `"Meu Primeiro Post"`). O método `.notNull()` torna a coluna obrigatória.
  - **`slug: text("slug").notNull().unique()`**
    - Define uma coluna `slug` do tipo `text` para URLs amigáveis (ex.: `"meu-primeiro-post"`). É obrigatória (`.notNull()`) e única (`.unique()`), evitando duplicatas.
  - **`excerpt: text("excerpt").notNull()`**
    - Define uma coluna `excerpt` do tipo `text` para o resumo do post (ex.: `"Um breve resumo"`). É obrigatória.
  - **`content: text("content").notNull()`**
    - Define uma coluna `content` do tipo `text` para o conteúdo completo do post. É obrigatória.
  - **`coverImageUrl: text("cover_image_url").notNull()`**
    - Define uma coluna `cover_image_url` do tipo `text` para a URL da imagem de capa (ex.: `"http://example.com/image.jpg"`). É obrigatória.
  - **`published: integer("published", { mode: "boolean" }).notNull()`**
    - Define uma coluna `published` do tipo `integer`, usada como booleano (`0` para `false`, `1` para `true`). O parâmetro `{ mode: "boolean" }` instrui o Drizzle a tratar a coluna como booleana no TypeScript. É obrigatória.
  - **`createdAt: text("created_at").notNull()`**
    - Define uma coluna `created_at` do tipo `text` para a data de criação do post (ex.: `"2025-07-21T21:36:00Z"`). É obrigatória.
  - **`updatedAt: text("updated_at").notNull()`**
    - Define uma coluna `updated_at` do tipo `text` para a data da última atualização. É obrigatória.
  - **`author: text("author").notNull()`**
    - Define uma coluna `author` do tipo `text` para o nome ou identificador do autor (ex.: `"João Silva"`). É obrigatória.

- **`export type PostTableSelectMode = InferSelectModel<typeof postsTable>;`**

  - Cria um tipo TypeScript chamado `PostTableSelectMode` para registros retornados por consultas `SELECT`. O utilitário `InferSelectModel` gera automaticamente o tipo com base na definição da tabela, resultando em algo como:
    ```tsx
    type PostTableSelectMode = {
      id: string;
      title: string;
      slug: string;
      excerpt: string;
      content: string;
      coverImageUrl: string;
      published: boolean;
      createdAt: string;
      updatedAt: string;
      author: string;
    };
    ```

- **`export type PostTableSelectMode = InferInsertModel<typeof postsTable>;`**
  - **Erro no código original**: Há uma duplicação aqui. O código define `PostTableSelectMode` duas vezes, mas a segunda linha deveria ser `PostTableInsertMode`. A correção é:
    ```tsx
    export type PostTableInsertMode = InferInsertModel<typeof postsTable>;
    ```
  - Essa linha cria um tipo TypeScript chamado `PostTableInsertMode` para objetos usados em operações `INSERT`. O utilitário `InferInsertModel` infere o tipo com base nas colunas, incluindo todas as colunas obrigatórias (como `id`, já que não é autoincrementável).

**Tabela resumo dos campos da tabela `posts`:**

| Campo           | Tipo      | Obrigatório | Descrição                            |
| --------------- | --------- | ----------- | ------------------------------------ |
| `id`            | `text`    | ✅ Sim      | Chave primária (ex.: UUID ou string) |
| `title`         | `text`    | ✅ Sim      | Título do post                       |
| `slug`          | `text`    | ✅ Sim      | URL amigável e única                 |
| `excerpt`       | `text`    | ✅ Sim      | Resumo do conteúdo                   |
| `content`       | `text`    | ✅ Sim      | Conteúdo completo do post            |
| `coverImageUrl` | `text`    | ✅ Sim      | URL da imagem de capa                |
| `published`     | `boolean` | ✅ Sim      | Indica se o post está publicado      |
| `createdAt`     | `text`    | ✅ Sim      | Data de criação (formato ISO)        |
| `updatedAt`     | `text`    | ✅ Sim      | Data da última atualização           |
| `author`        | `text`    | ✅ Sim      | Nome ou ID do autor                  |

**Dica:** Os utilitários `InferSelectModel` e `InferInsertModel` eliminam a necessidade de escrever tipos manualmente, garantindo que o código TypeScript esteja sempre sincronizado com o esquema do banco.

---

## 🛠️ **Etapa 5: Criando a Conexão com o Banco de Dados**

No arquivo `index.ts`, configuramos a conexão com o SQLite e inicializamos o Drizzle ORM. O guia original inclui um exemplo com `resolve` e `logger`, que é uma boa prática para projetos robustos.

### 📄 **Arquivo: `src/db/drizzle/index.ts`**

```tsx
import { drizzle } from "drizzle-orm/better-sqlite3";
import { postsTable } from "./schema";
import Database from "better-sqlite3";
import { resolve } from "path";

const sqliteDataBasePath = resolve(process.cwd(), "./db.sqlite3");
const sqliteDataBase = new Database(sqliteDataBasePath);

export const drizzleDb = drizzle(sqliteDataBase, {
  schema: {
    posts: postsTable,
  },
  logger: true,
});
```

**Explicação linha por linha:**

- **`import { drizzle } from "drizzle-orm/better-sqlite3";`**

  - Importa a função `drizzle` do módulo específico para SQLite (`drizzle-orm/better-sqlite3`). Essa função inicializa o Drizzle ORM com suporte ao driver `better-sqlite3`.

- **`import { postsTable } from "./schema";`**

  - Importa a tabela `postsTable` definida em `schema.ts`. Isso é necessário para passar a definição da tabela para o Drizzle ORM.

- **`import Database from "better-sqlite3";`**

  - Importa a classe `Database` do pacote `better-sqlite3`, usada para criar uma conexão com o arquivo do banco SQLite.

- **`import { resolve } from "path";`**

  - Importa a função `resolve` do módulo nativo `path` do Node.js. Essa função resolve caminhos de arquivos de forma absoluta, garantindo que o caminho do banco seja correto independentemente do diretório de execução.

- **`const sqliteDataBasePath = resolve(process.cwd(), "./db.sqlite3");`**

  - Usa `resolve` para criar um caminho absoluto para o arquivo `db.sqlite3`, combinando:
    - `process.cwd()`: Retorna o diretório de trabalho atual do Node.js (raiz do projeto).
    - `"./db.sqlite3"`: O caminho relativo para o arquivo do banco.
  - Isso evita problemas com caminhos relativos em diferentes ambientes.

- **`const sqliteDataBase = new Database(sqliteDataBasePath);`**

  - Cria uma nova instância do banco SQLite, apontando para o arquivo `db.sqlite3`. Se o arquivo não existir, o SQLite o criará automaticamente na primeira interação.

- **`export const drizzleDb = drizzle(sqliteDataBase, { schema: { posts: postsTable }, logger: true });`**
  - Inicializa o Drizzle ORM, criando a instância `drizzleDb` com:
    - `sqliteDataBase`: A conexão com o banco SQLite.
    - `{ schema: { posts: postsTable } }`: Um objeto que mapeia as tabelas do esquema. Aqui, associamos o nome `posts` à tabela `postsTable` definida em `schema.ts`.
    - `logger: true`: Ativa o logger do Drizzle, que exibe as consultas SQL geradas no console, útil para depuração.
  - A instância `drizzleDb` é usada para todas as operações no banco (ex.: inserir, consultar, atualizar).

---

## 🧪 **Etapa 6: Gerando e Aplicando Migrações**

Depois de configurar o esquema e a conexão, precisamos criar a tabela `posts` no banco SQLite usando migrações.

### 🔹 **Passo 6.1 – Gerar migrações**

```bash
npx drizzle-kit generate:sqlite
```

**Explicação:**

- Este comando analisa o arquivo `schema.ts` e gera arquivos de migração na pasta `src/db/drizzle/migrations` (definida em `out` no `drizzle.config.ts`).
- Os arquivos de migração contêm comandos SQL para criar ou modificar tabelas com base no esquema definido.
- O sufixo `:sqlite` especifica que as migrações são para o SQLite, garantindo compatibilidade com o dialeto.

**Exemplo de arquivo de migração gerado (em `migrations/0000_initial.sql`):**

```sql
CREATE TABLE `posts` (
	`id` text PRIMARY KEY NOT NULL,
	`title` text NOT NULL,
	`slug` text NOT NULL,
	`excerpt` text NOT NULL,
	`content` text NOT NULL,
	`cover_image_url` text NOT NULL,
	`published` integer NOT NULL,
	`created_at` text NOT NULL,
	`updated_at` text NOT NULL,
	`author` text NOT NULL
);
--> statement-breakpoint
CREATE UNIQUE INDEX `posts_slug_unique` ON `posts` (`slug`);
```

**Explicação do SQL gerado:**

- **`CREATE TABLE `posts` (...);`**: Cria a tabela `posts` com todas as colunas definidas em `schema.ts`.
- **`PRIMARY KEY`**: Define `id` como chave primária.
- **`NOT NULL`**: Aplica a restrição de não-nulidade às colunas marcadas com `.notNull()`.
- **`CREATE UNIQUE INDEX `posts_slug_unique`ON`posts` (`slug`);`**: Cria um índice único para a coluna `slug`, garantindo que seus valores sejam exclusivos.
- **`--> statement-breakpoint`**: Um marcador usado pelo Drizzle Kit para separar comandos SQL, útil em migrações incrementais.

### 🔹 **Passo 6.2 – Aplicar migrações**

```bash
npx drizzle-kit push:sqlite
```

**Explicação:**

- Este comando executa os arquivos de migração gerados, criando a tabela `posts` no arquivo `db.sqlite3`.
- Se o arquivo `db.sqlite3` não existir, o SQLite o criará automaticamente.
- O comando `push:sqlite` aplica as alterações diretamente, sincronizando o banco com o esquema.

**Observação:** Certifique-se de que o arquivo `drizzle.config.ts` está correto (com `schema.ts` em vez de `shemas.ts`) antes de executar esses comandos.

---

## 🧾 **Etapa 7: Exemplo de Uso do Drizzle ORM**

Com a tabela criada, podemos usar a instância `drizzleDb` para realizar operações no banco, como inserir e consultar posts. Aqui está um exemplo prático.

### 📄 **Arquivo: `src/main.ts`**

```tsx
import { drizzleDb } from "./db/drizzle";
import { postsTable } from "./db/drizzle/schema";
import { eq } from "drizzle-orm";

// Inserir um post
async function addPost() {
  await drizzleDb.insert(postsTable).values({
    id: "post-1",
    title: "Meu Primeiro Post",
    slug: "meu-primeiro-post",
    excerpt: "Um resumo do post",
    content: "Conteúdo completo do post",
    coverImageUrl: "http://example.com/image.jpg",
    published: true,
    createdAt: new Date().toISOString(),
    updatedAt: new Date().toISOString(),
    author: "João Silva",
  });
  console.log("Post inserido!");
}

// Consultar um post
async function getPost(id: string) {
  const post = await drizzleDb
    .select()
    .from(postsTable)
    .where(eq(postsTable.id, id));
  console.log("Post encontrado:", post);
}

// Executar as funções
async function main() {
  await addPost();
  await getPost("post-1");
}

main().catch((error) => {
  console.error("Erro:", error);
});
```

**Explicação linha por linha:**

- **`import { drizzleDb } from "./db/drizzle";`**

  - Importa a instância `drizzleDb` do arquivo `index.ts`, que gerencia a conexão com o banco SQLite.

- **`import { postsTable } from "./db/drizzle/schema";`**

  - Importa a definição da tabela `postsTable` do arquivo `schema.ts`, necessária para referenciar a tabela em consultas.

- **`import { eq } from "drizzle-orm";`**

  - Importa a função `eq` do Drizzle ORM, usada para criar condições de igualdade em consultas (ex.: `WHERE id = 'post-1'`).

- **`async function addPost() { ... }`**

  - Define uma função assíncrona para inserir um novo post na tabela `posts`.

- **`await drizzleDb.insert(postsTable).values({ ... })`**

  - Insere um registro na tabela `posts` usando o método `.insert()`. O objeto passado para `.values()` contém valores para todas as colunas obrigatórias, tipadas pelo `PostTableInsertMode`. Como `id` é do tipo `text`, ele deve ser fornecido manualmente.

- **`console.log("Post inserido!");`**

  - Exibe uma mensagem no console após a inserção bem-sucedida.

- **`async function getPost(id: string) { ... }`**

  - Define uma função assíncrona para consultar um post pelo seu `id`.

- **`const post = await drizzleDb.select().from(postsTable).where(eq(postsTable.id, id));`**

  - Executa uma consulta `SELECT` na tabela `posts` com a condição `WHERE id = ?`. O resultado é um array de objetos tipados como `PostTableSelectMode`.

- **`console.log("Post encontrado:", post);`**

  - Exibe o resultado da consulta no console. Se o post existir, o array conterá um objeto com as propriedades do post (ex.: `{ id: "post-1", title: "Meu Primeiro Post", ... }`).

- **`async function main() { ... }`**

  - Define uma função principal que executa as operações de inserção e consulta de forma sequencial.

- **`main().catch((error) => { console.error("Erro:", error); });`**
  - Executa a função `main` e captura qualquer erro, exibindo-o no console para facilitar a depuração.

**Executando o exemplo:**

1. Compile o TypeScript (se necessário):
   ```bash
   npx tsc
   ```
2. Execute o arquivo:
   ```bash
   node dist/main.js
   ```
   Isso insere um post no banco e exibe o post com `id: "post-1"`. Se o logger estiver ativado (`logger: true` em `index.ts`), as consultas SQL serão exibidas no console.

---

## 🔍 **Etapa 8: Explorando o Banco com Drizzle Studio**

O Drizzle Kit inclui o **Drizzle Studio**, uma interface web para visualizar e gerenciar o banco de dados.

```bash
npx drizzle-kit studio
```

**Explicação:**

- Este comando inicia um servidor web local (geralmente em `http://localhost:3000`) onde você pode:
  - Visualizar a estrutura da tabela `posts`.
  - Consultar os dados inseridos.
  - Executar consultas SQL interativamente.
- É uma ferramenta útil para verificar se as migrações foram aplicadas corretamente e inspecionar os dados no banco.

---

## 🛠️ **Etapa 9: Scripts no `package.json` (Opcional)**

Para facilitar a execução de comandos frequentes, adicione scripts ao `package.json`:

```json
{
  "scripts": {
    "db:generate": "drizzle-kit generate:sqlite",
    "db:push": "drizzle-kit push:sqlite",
    "db:studio": "drizzle-kit studio",
    "start": "ts-node src/main.ts"
  }
}
```

**Explicação:**

- **`db:generate`**: Executa `npx drizzle-kit generate:sqlite` para gerar migrações.
- **`db:push`**: Executa `npx drizzle-kit push:sqlite` para aplicar migrações.
- **`db:studio`**: Inicia o Drizzle Studio.
- **`start`**: Executa o arquivo `main.ts` usando `ts-node` (instale com `npm install -D ts-node` se necessário).

Agora, você pode rodar os comandos com:

```bash
npm run db:generate
npm run db:push
npm run db:studio
npm run start
```

---

## ✅ **Conclusão**

Ao seguir este guia, você configurou um ambiente funcional com **Drizzle ORM e SQLite 3**, incluindo:

- **Instalação**: Configurou `drizzle-orm`, `better-sqlite3`, `drizzle-kit` e `@types/better-sqlite3`.
- **Configuração**: Criou `drizzle.config.ts` para definir o esquema, migrações e conexão com o banco.
- **Estrutura**: Organizou os arquivos em `src/db/drizzle` com `schema.ts` (tabela `posts`) e `index.ts` (conexão).
- **Esquema**: Definiu a tabela `posts` com colunas tipadas e gerou tipos TypeScript automáticos.
- **Migrações**: Gerou e aplicou migrações para criar a tabela no banco.
- **Uso**: Implementou exemplos de inserção e consulta de posts com segurança tipada.

A configuração é escalável, type-safe e pronta para ser integrada em projetos maiores, como APIs com **Next.js** ou **Express.js**.

---

## 🚀 **Próximos Passos**

Para expandir este projeto, considere:

1. **Adicionar mais tabelas**: Crie uma tabela `users` para gerenciar autores e use relações (ex.: `foreignKey`) no `schema.ts`.
2. **Consultas avançadas**: Implemente filtros, ordenação ou `JOIN` para consultas mais complexas.
3. **Integração com frameworks**:
   - **Next.js**: Use o Drizzle ORM em rotas API ou server actions.
   - **Express.js**: Crie endpoints REST para gerenciar posts.
4. **Automação de datas**: Adicione lógica para atualizar automaticamente `createdAt` e `updatedAt` ao inserir ou atualizar posts.

Se quiser exemplos detalhados para algum desses tópicos (ex.: código para integração com Next.js ou consultas complexas), é só pedir!
