# Drilezzz

Drizzle ORM é um ORM (Object-Relational Mapping) leve e performante para TypeScript, projetado com foco na experiência do desenvolvedor. Ele permite mapear entidades de banco de dados para objetos programáveis em JavaScript/TypeScript, facilitando a interação com bancos de dados relacionais como PostgreSQL, MySQL e SQLite. Diferentemente de outros ORMs que abstraem muito o SQL, o Drizzle abraça a sintaxe SQL, oferecendo uma API semelhante ao SQL com segurança de tipos, o que reduz a curva de aprendizado para quem já conhece SQL.

### Principais Características:

- **Leve e sem dependências**: Com apenas ~7.4kb (minificado e comprimido), é altamente otimizado e não possui dependências externas. [github.com](https://github.com/drizzle-team/drizzle-orm)
- **Suporte a múltiplos bancos**: Compatível com PostgreSQL, MySQL, SQLite e outros, incluindo ambientes serverless como Neon, Supabase e Vercel Postgres. [github.com](https://github.com/drizzle-team/drizzle-orm) | [orm.drizzle.team](https://orm.drizzle.team/docs/overview)
- **API SQL-like e relacional**: Oferece tanto uma API de construção de consultas semelhante ao SQL quanto uma API relacional para lidar com relações complexas, como one-to-many e many-to-many. [orm.drizzle.team](https://orm.drizzle.team/docs/rqb) | [orm.drizzle.team](https://orm.drizzle.team/docs/relations)
- **Segurança de tipos**: Gera tipos TypeScript automaticamente a partir do esquema do banco, garantindo consistência e segurança no código. [betterstack.com](https://betterstack.com/community/guides/scaling-nodejs/drizzle-orm/)
- **Ferramentas complementares**: Inclui o Drizzle Kit, uma CLI para gerenciar migrações de esquemas, e o Drizzle Studio, para explorar e manipular dados no banco.[github.com](https://github.com/drizzle-team/drizzle-orm) | [orm.drizzle.team](https://orm.drizzle.team/drizzle-studio/overview)
- **Serverless-ready**: Projetado para funcionar bem em ambientes serverless, como Cloudflare Workers e Deno, sem proxies de dados adicionais. [github.com](https://github.com/drizzle-team/drizzle-orm)

### Exemplo Básico:

```typescript
import { pgTable, serial, text } from "drizzle-orm/pg-core";
import { drizzle } from "drizzle-orm/node-postgres";
import { Pool } from "pg";

// Definir esquema
export const users = pgTable("users", {
  id: serial("id").primaryKey(),
  name: text("name").notNull(),
});

// Conectar ao banco
const pool = new Pool({ connectionString: process.env.DATABASE_URL });
const db = drizzle(pool);

// Consultar dados
async function getUsers() {
  const result = await db.select().from(users).where(eq(users.id, 42));
  console.log(result);
}
```

### Vantagens:

- **Flexibilidade**: Permite compor consultas de forma modular, com filtros condicionais e subconsultas, mantendo a previsibilidade do SQL gerado.[](https://orm.drizzle.team/docs/data-querying)
- **Desempenho**: Gera exatamente uma consulta SQL por operação, otimizando performance em ambientes serverless.[](https://orm.drizzle.team/docs/data-querying)
- **Comunidade ativa**: Suporte via Discord e atualizações frequentes, como suporte a RLS (Row-Level Security) e melhorias no Drizzle Kit.

### Limitações:

- **Migrações manuais**: Diferente de outros ORMs, como Prisma, o suporte a migrações automáticas ainda é limitado, exigindo ferramentas externas ou gerenciamento manual.[](https://blog.logrocket.com/drizzle-orm-adoption-guide/)
- **Curva de aprendizado**: Para desenvolvedores não familiarizados com SQL, pode ser mais desafiador em comparação com ORMs mais abstratos, como Prisma.[](https://blog.logrocket.com/drizzle-orm-adoption-guide/)
- **Documentação**: Alguns usuários relatam que a documentação pode ser confusa ou incompleta em certos casos.[](https://blog.logrocket.com/drizzle-orm-adoption-guide/)

### Comparação com Outros ORMs:

Diferentemente do Prisma, que oferece uma abstração maior e um DSL próprio, o Drizzle é mais próximo do SQL puro, apelando para desenvolvedores que preferem controle granular sobre suas consultas. Enquanto o Prisma foca em simplicidade para iniciantes, o Drizzle prioriza performance e flexibilidade para cenários complexos, como consultas com múltiplos JOINs.[](https://blog.logrocket.com/drizzle-orm-adoption-guide/)[](https://www.prisma.io/docs/orm/more/comparisons/prisma-and-drizzle)

### Casos de Uso:

- Projetos TypeScript/JavaScript que exigem alta performance e integração com bancos relacionais.
- Aplicações serverless ou em ambientes como Next.js, Deno ou Bun.[](https://refine.dev/blog/drizzle-react/)
- Desenvolvedores que preferem sintaxe SQL-like com segurança de tipos.

Para mais detalhes, consulte a documentação oficial em [orm.drizzle.team](https://orm.drizzle.team) ou explore exemplos práticos no [GitHub do Drizzle](https://github.com/drizzle-team/drizzle-orm).[](https://github.com/drizzle-team/drizzle-orm)[](https://orm.drizzle.team/docs/get-started)

<br />

# Instalação e configuração

Para instalar e configurar o **Drizzle ORM** em um projeto TypeScript/JavaScript, siga os passos abaixo. Vou detalhar o processo para um ambiente com Node.js, utilizando PostgreSQL como exemplo, mas o Drizzle também suporta MySQL, SQLite e outros bancos de dados.

### Pré-requisitos

- **Node.js** (versão 16 ou superior recomendada).
- Um banco de dados relacional configurado (ex.: PostgreSQL, MySQL ou SQLite).
- Um projeto TypeScript/JavaScript inicializado com `npm` ou `yarn`.
- TypeScript instalado (`npm install typescript --save-dev` caso ainda não esteja configurado).

### Passo 1: Instalação do Drizzle ORM

1. Instale o Drizzle ORM e o driver do banco de dados desejado. Para PostgreSQL, por exemplo:

   ```bash
   npm install drizzle-orm @types/pg pg
   ```

   - `drizzle-orm`: Pacote principal do Drizzle.
   - `pg`: Driver para PostgreSQL.
   - `@types/pg`: Tipos TypeScript para o driver `pg`.

   Para outros bancos, substitua pelo driver correspondente:

   - MySQL: `npm install drizzle-orm mysql2`
   - SQLite: `npm install drizzle-orm better-sqlite3`

2. (Opcional) Instale o **Drizzle Kit** para gerenciar esquemas e migrações:
   ```bash
   npm install drizzle-kit --save-dev
   ```

### Passo 2: Configuração do Banco de Dados

1. Configure a conexão com o banco de dados. Crie um arquivo, como `db.ts`, com a configuração do cliente do banco. Exemplo para PostgreSQL:

   ```typescript
   import { drizzle } from "drizzle-orm/node-postgres";
   import { Pool } from "pg";

   // Configuração da conexão
   const pool = new Pool({
     connectionString: process.env.DATABASE_URL, // Ex.: postgres://user:password@localhost:5432/dbname
   });

   // Inicializar o Drizzle
   export const db = drizzle(pool);
   ```

   - Substitua `DATABASE_URL` pela URL de conexão do seu banco. Você pode usar um arquivo `.env` para gerenciar variáveis de ambiente:
     ```bash
     npm install dotenv
     ```
     Em um arquivo `.env`:
     ```
     DATABASE_URL=postgres://user:password@localhost:5432/dbname
     ```
     Carregue o `.env` no seu código:
     ```typescript
     import "dotenv/config";
     ```

### Passo 3: Definir o Esquema do Banco

Crie um arquivo para definir o esquema do banco, como `schema.ts`. Exemplo com uma tabela de usuários:

```typescript
import { pgTable, serial, text, timestamp } from "drizzle-orm/pg-core";

// Definir tabela
export const users = pgTable("users", {
  id: serial("id").primaryKey(),
  name: text("name").notNull(),
  email: text("email").notNull().unique(),
  createdAt: timestamp("created_at").defaultNow(),
});
```

### Passo 4: Configurar o Drizzle Kit (para Migrações)

1. Crie um arquivo de configuração `drizzle.config.ts` na raiz do projeto:

   ```typescript
   import type { Config } from "drizzle-kit";

   export default {
     schema: "./schema.ts", // Caminho para o arquivo de esquema
     out: "./drizzle", // Pasta onde as migrações serão geradas
     driver: "pg", // Driver do banco (pg para PostgreSQL, mysql2 para MySQL, better-sqlite3 para SQLite)
     dbCredentials: {
       connectionString: process.env.DATABASE_URL!,
     },
   } satisfies Config;
   ```

2. Gere as migrações:

   ```bash
   npx drizzle-kit generate:pg
   ```

   Isso cria arquivos SQL na pasta `drizzle` com as instruções para criar/atualizar tabelas.

3. Aplique as migrações ao banco:
   ```bash
   npx drizzle-kit push:pg
   ```
   Isso sincroniza o esquema com o banco de dados.

### Passo 5: Usar o Drizzle ORM

Agora você pode usar o Drizzle para interagir com o banco. Exemplo de consultas em um arquivo como `index.ts`:

```typescript
import { db } from "./db";
import { users } from "./schema";
import { eq } from "drizzle-orm";

// Inserir um usuário
async function addUser(name: string, email: string) {
  await db.insert(users).values({ name, email });
  console.log("Usuário adicionado!");
}

// Consultar usuários
async function getUsers() {
  const result = await db.select().from(users).where(eq(users.id, 1));
  console.log(result);
}

addUser("João", "joao@example.com");
getUsers();
```

### Passo 6: Executar o Projeto

1. Compile o TypeScript (se aplicável):
   ```bash
   npx tsc
   ```
2. Execute o projeto:
   ```bash
   node index.js
   ```

### Dicas Adicionais

- **Drizzle Studio**: Para explorar o banco de dados visualmente, execute:

  ```bash
  npx drizzle-kit studio
  ```

  Isso inicia uma interface web local para visualizar e manipular dados.

- **Variáveis de Ambiente**: Sempre use `.env` para proteger credenciais sensíveis.
- **Documentação**: Consulte a [documentação oficial do Drizzle](https://orm.drizzle.team) para mais detalhes sobre consultas avançadas, relações e configuração.
- **Erros Comuns**:
  - Certifique-se de que o banco de dados está acessível e a URL de conexão está correta.
  - Verifique se o driver do banco corresponde ao configurado no `drizzle.config.ts`.

### Resumo

A instalação do Drizzle ORM é direta: instale os pacotes necessários, configure a conexão com o banco, defina o esquema e use o Drizzle Kit para migrações. A configuração é altamente flexível, permitindo personalização para diferentes bancos e ambientes (como serverless). Para exemplos mais avançados, como consultas relacionais ou suporte a RLS, consulte o [repositório no GitHub](https://github.com/drizzle-team/drizzle-orm) ou a comunidade no Discord.

Se precisar de ajuda com um caso específico ou configuração avançada, é só perguntar!
