grok

# Estudo Detalhado sobre Drizzle ORM: Foco no Query Builder e Operações CRUD

Olá! Vou criar um estudo completo e detalhado sobre o Drizzle ORM, baseado no seu exemplo com o Query Builder. Vou explicar **tudo** de forma explícita, sem omitir nenhum detalhe relevante. Começarei com uma introdução ao Drizzle ORM para contextualizar, depois explicarei a configuração básica (pois sem ela, os exemplos não funcionam), e em seguida mergulharei no Query Builder para consultas (SELECT), adicionando mais funções além das que você mencionou. Depois, mostrarei como fazer inserções (INSERT), atualizações (UPDATE) e deleções (DELETE) – interpretei "UMA SOLICITAÇÃO" como possivelmente um erro de digitação para "UMA ATUALIZAÇÃO" ou "UMA DELEÇÃO", mas para ser completo, cobrirei todas as operações CRUD principais.

Estrutura da resposta:

- **Seção 1: Introdução ao Drizzle ORM**
- **Seção 2: Configuração Básica**
- **Seção 3: Definição de Schema (Tabelas)**
- **Seção 4: Query Builder para Consultas (SELECT) – Explicação Detalhada e Exemplos**
- **Seção 5: Funções Adicionais no Query Builder**
- **Seção 6: Inserções (INSERT)**
- **Seção 7: Atualizações (UPDATE)**
- **Seção 8: Deleções (DELETE)**
- **Seção 9: Considerações Avançadas e Boas Práticas**

Todos os códigos serão em TypeScript (como no seu exemplo), e explicarei **linha por linha** onde necessário. Assumirei um banco de dados PostgreSQL para os exemplos (o Drizzle suporta vários, como MySQL, SQLite, etc.), mas os conceitos são semelhantes. Usei a documentação oficial do Drizzle ORM (versão mais recente em 2025) para garantir precisão.

**Nota importante:** O Drizzle ORM é uma biblioteca leve e type-safe para construir consultas SQL em TypeScript. Ele não executa as consultas automaticamente; você precisa de um driver de banco de dados (como `pg` para PostgreSQL). Os exemplos usam `drizzle-orm` e `drizzle-orm/pg-core`.

## Seção 1: Introdução ao Drizzle ORM

- **O que é Drizzle ORM?** Drizzle ORM é uma biblioteca open-source para Node.js/TypeScript que permite interagir com bancos de dados SQL de forma type-safe, sem a necessidade de um ORM tradicional pesado. Ele foca em compor consultas SQL de forma programática, gerando SQL puro e parametrizado para evitar injeções SQL. Ao contrário de ORMs como Prisma ou TypeORM, o Drizzle não abstrai o SQL completamente – ele incentiva o uso de SQL real, mas com tipagem forte.

- **Vantagens:**

  - Type-safe: Detecta erros em tempo de compilação (ex: campos inexistentes em tabelas).
  - Leve: Não tem runtime overhead significativo.
  - Flexível: Suporta PostgreSQL, MySQL, SQLite, e drivers como LibSQL, Neon, etc.
  - Query Builder: Permite construir consultas de forma fluida (chaining methods).
  - Schema-first: Você define o schema em código TypeScript, que pode gerar migrações.

- **Desvantagens:** Requer conhecimento básico de SQL, pois não abstrai tudo.

- **Versão atual (em agosto 2025):** Baseado na documentação, a versão é ~0.32.x ou superior, com suporte a novos features como JSONB queries avançadas.

## Seção 2: Configuração Básica

Para usar o Drizzle, você precisa instalar pacotes e conectar ao banco de dados. Explicação passo a passo:

1. **Instalação de Pacotes:**

   - No terminal: `npm install drizzle-orm` (pacote principal).
   - Para PostgreSQL: `npm install pg` (driver) e `npm install @drizzle-team/pg-core` (core para PostgreSQL).
   - Para migrações (opcional, mas recomendado): `npm install drizzle-kit`.

2. **Conexão ao Banco de Dados:**

   - Crie um arquivo `db.ts` (ou similar):

     ```ts
     import { drizzle } from "drizzle-orm/node-postgres"; // Importa o drizzle para Node.js com PostgreSQL.
     import { Client } from "pg"; // Importa o client do pg para conexão.

     // Configuração da conexão: Aqui definimos as credenciais do banco. Em produção, use variáveis de ambiente (ex: process.env.DATABASE_URL).
     const client = new Client({
       host: "localhost", // Endereço do servidor do banco (ex: localhost para desenvolvimento).
       port: 5432, // Porta padrão do PostgreSQL.
       user: "seu_usuario", // Usuário do banco.
       password: "sua_senha", // Senha do banco.
       database: "seu_banco", // Nome do banco de dados.
     });

     await client.connect(); // Conecta ao banco de dados de forma assíncrona. Isso é necessário antes de qualquer query.

     // Instancia o Drizzle com o client conectado. Isso cria o objeto 'drizzleDb' que usaremos para queries.
     export const drizzleDb = drizzle(client);
     ```

     - **Explicação linha por linha:**
       - Linha 1-2: Importamos as dependências necessárias.
       - Linha 5-10: Criamos um client PostgreSQL com configurações. Cada opção é autoexplicativa: host é o servidor, port é a porta, etc.
       - Linha 12: Conectamos ao banco. `await` garante que a conexão seja estabelecida antes de prosseguir.
       - Linha 15: Exportamos o `drizzleDb`, que é o handler principal para todas as operações.

3. **Executando Migrações:** Use `drizzle-kit` para gerar e aplicar schemas (explicado na próxima seção).

## Seção 3: Definição de Schema (Tabelas)

O Drizzle requer que você defina as tabelas em código TypeScript. Isso garante tipagem.

- Crie um arquivo `schema.ts`:

  ```ts
  import { pgTable, serial, text, integer } from "drizzle-orm/pg-core"; // Importa funções para definir tabelas PostgreSQL.

  // Define a tabela 'tabelaTeste'. O nome da tabela no banco será "tabelaTeste".
  export const tabelaTeste = pgTable("tabelaTeste", {
    id: serial("id").primaryKey(), // Coluna 'id': Tipo serial (auto-incremento), chave primária.
    name: text("name").notNull(), // Coluna 'name': Tipo texto, não nulo.
    age: integer("age").default(0), // Coluna 'age': Tipo inteiro, default 0.
  });
  ```

  - **Explicação linha por linha:**
    - Linha 1: Importa `pgTable` para criar tabelas, e tipos como `serial` (auto-incremento), `text` (string), `integer` (int).
    - Linha 4: `pgTable('nome_tabela', { colunas })` – Primeiro argumento é o nome da tabela no banco.
    - Linha 5: `id: serial('id').primaryKey()` – Cria coluna 'id' que auto-incrementa e é PK.
    - Linha 6: `name: text('name').notNull()` – Coluna 'name' como texto, obrigatório.
    - Linha 7: `age: integer('age').default(0)` – Coluna 'age' como inteiro, default 0 se não fornecido.

- Para aplicar no banco: Use `drizzle-kit generate:pg` e `drizzle-kit push:pg` (configurado em `drizzle.config.ts`).

Agora, com schema definido, podemos usar `tabelaTeste` em queries.

## Seção 4: Query Builder para Consultas (SELECT) – Explicação Detalhada e Exemplos

O Query Builder do Drizzle permite construir consultas SQL de forma fluida. Ele usa métodos encadeados (chaining) e gera SQL parametrizado.

- **Funções Principais Explicadas:**

  - `.select({ campos })`: Especifica os campos a serem selecionados. O objeto `{}` define aliases ou campos (ex: { id: tabelaTeste.id }). Se omitido, seleciona todos os campos (`*`).
  - `.from(tabela)`: Especifica a tabela principal da query. Obrigatório.
  - `.where(condição)`: Adiciona cláusula WHERE. Usa operadores como `eq` (equal), `and`, `or`, `gt` (greater than), `gte` (greater or equal), `lt` (less than), `lte` (less or equal), `not`, `inArray`, etc.
  - `.toSQL()`: Gera o SQL e parâmetros sem executar. Útil para debugging.
  - `await query`: Executa a query e retorna resultados (array de objetos).

- **Exemplo Base (do seu código, explicado linha por linha):**

  ```ts
  import { eq } from "drizzle-orm"; // Importa operadores SQL como 'eq' (equal).

  // Cria a query base: Seleciona campos específicos da tabela.
  const query = drizzleDb
    .select({
      id: tabelaTeste.id,
      name: tabelaTeste.name,
      age: tabelaTeste.age,
    }) // Seleciona apenas 'id', 'name', 'age'. Use { ...tabelaTeste.$inferSelect } para todos.
    .from(tabelaTeste) // Especifica a tabela fonte.
    .where(eq(tabelaTeste.id, "idTeste")); // WHERE id = 'idTeste'. 'eq' é o operador de igualdade.

  console.log(query.toSQL().sql); // Gera SQL: select "id", "name", "age" from "tabelaTeste" where "tabelaTeste"."id" = ?
  console.log(query.toSQL().params); // Parâmetros: ["idTeste"] – Previne SQL injection.

  const result = await query; // Executa a query assincronamente. 'result' será array como [{ id: 'idTeste', name: '...', age: ... }].
  ```

  - **Explicação linha por linha:**
    - Linha 1: Importa `eq` – Função que cria condição SQL "campo = valor".
    - Linha 4: `drizzleDb.select({ ... })` – Inicia o builder, selecionando campos. O objeto mapeia nomes para colunas (type-safe).
    - Linha 5: `.from(tabelaTeste)` – Define a tabela.
    - Linha 6: `.where(eq(tabelaTeste.id, 'idTeste'))` – Adiciona WHERE. Pode encadear com `and(eq(...), or(...))` para condições complexas.
    - Linha 8-9: `.toSQL()` retorna { sql: string, params: array } – SQL com placeholders (?) e params separados.
    - Linha 11: `await query` – Executa e fetcha dados.

- **Versão Alternativa (Montando Separadamente, como no seu exemplo):**

  ```ts
  let query = drizzleDb
    .select({
      id: tabelaTeste.id,
      name: tabelaTeste.name,
      age: tabelaTeste.age,
    })
    .from(tabelaTeste); // Query base sem WHERE.

  query = query.where(eq(tabelaTeste.id, "idTeste")); // Adiciona WHERE depois. Note: Query é imutável, então reatribua.

  console.log(query.toSQL().sql); // Mesmo output que acima.

  query = query.orderBy(desc(tabelaTeste.name)); // Adiciona ORDER BY name DESC. 'desc' é função para descending.

  console.log(query.toSQL().sql); // Agora com ORDER BY.

  const result = await query; // Executa.
  ```

  - **Diferença:** Permite construir dinamicamente (ex: condicionais if para adicionar .where).

## Seção 5: Funções Adicionais no Query Builder

Adicionando mais funções além das básicas (where, orderBy). Todas são encadeáveis.

- **.innerJoin(tabela2, on: condição)**: Join interno.

  - Exemplo: Assuma outra tabela `outraTabela` com foreign key para `tabelaTeste.id`.

    ```ts
    import { asc } from "drizzle-orm"; // Importa asc para ascending.

    const query = drizzleDb
      .select({ teste: tabelaTeste, outra: outraTabela }) // Seleciona campos de ambas.
      .from(tabelaTeste)
      .innerJoin(outraTabela, eq(tabelaTeste.id, outraTabela.testeId)) // JOIN ON id = testeId.
      .where(gt(tabelaTeste.age, 18)) // WHERE age > 18. 'gt' é greater than.
      .orderBy(asc(tabelaTeste.name)) // ORDER BY name ASC.
      .limit(10) // LIMIT 10 resultados.
      .offset(5); // OFFSET 5 (pulação para paginação).

    // SQL gerado: select ... from "tabelaTeste" inner join "outraTabela" on "tabelaTeste"."id" = "outraTabela"."testeId" where "tabelaTeste"."age" > ? order by "tabelaTeste"."name" asc limit ? offset ?
    // Params: [18, 10, 5]
    ```

    - Explicações: `.innerJoin` requer tabela e condição (usando eq). `.limit(n)` limita resultados. `.offset(n)` pula n linhas. `.orderBy(asc(coluna))` ou `desc(coluna)` para ordenação.

- **.groupBy(colunas)**: Agrupa resultados.

  - Exemplo: `query.groupBy(tabelaTeste.age)` – GROUP BY age.

- **.having(condição)**: Condição pós-GROUP BY.

  - Exemplo: `query.having(gt(sql`count(_)`, 1))` – HAVING count(_) > 1. Use `sql` para expressões raw.

- **.leftJoin, .rightJoin, .fullJoin**: Joins alternativos, semelhantes a innerJoin.

- **Operadores Adicionais para Where/Having:**

  - `and(cond1, cond2)`: AND entre condições.
  - `or(cond1, cond2)`: OR.
  - `not(cond)`: NOT.
  - `inArray(coluna, [valores])`: IN (lista).
  - `between(coluna, min, max)`: BETWEEN.
  - `isNull(coluna)`, `isNotNull(coluna)`: NULL checks.
  - `like(coluna, '%padrão%')`: LIKE para patterns.
  - Exemplo complexo: `.where(and(eq(tabelaTeste.id, 'id'), or(gt(tabelaTeste.age, 30), lt(tabelaTeste.age, 18))))`.

- **Subqueries e Raw SQL:** Use `sql` para partes custom: `sql`sum(${tabelaTeste.age}) as totalAge`` em select.

## Seção 6: Inserções (INSERT)

- **Função Principal: .insert(tabela).values(dados)**

  - Explicação: Insere um ou múltiplos registros. Retorna os inserts se configurado (ex: .returning() para PostgreSQL).
  - Exemplo:

    ```ts
    const novosDados = { name: "João", age: 25 }; // Objeto com valores (id auto-gerado).

    const insertQuery = drizzleDb
      .insert(tabelaTeste) // Especifica tabela para insert.
      .values(novosDados) // Valores a inserir. Pode ser array para múltiplos: .values([{...}, {...}]).
      .returning({ insertedId: tabelaTeste.id }); // Retorna o id inserido (opcional, suportado em PG).

    const result = await insertQuery; // Executa. result: [{ insertedId: 1 }]

    // SQL gerado: insert into "tabelaTeste" ("name", "age") values (?, ?) returning "id"
    // Params: ['João', 25]
    ```

    - Linha por linha: `.insert(tabela)` inicia. `.values(obj ou array)` adiciona dados. `.returning({ alias: coluna })` retorna campos pós-insert.

## Seção 7: Atualizações (UPDATE)

- **Função Principal: .update(tabela).set(dados).where(condição)**

  - Explicação: Atualiza registros baseados em WHERE.
  - Exemplo:

    ```ts
    const updateQuery = drizzleDb
      .update(tabelaTeste) // Tabela a atualizar.
      .set({ age: 30 }) // Campos a setar (pode ser parcial).
      .where(eq(tabelaTeste.id, "idTeste")) // Condição para update.
      .returning({ updatedName: tabelaTeste.name }); // Retorna campos atualizados.

    const result = await updateQuery; // Executa.

    // SQL: update "tabelaTeste" set "age" = ? where "tabelaTeste"."id" = ? returning "name"
    // Params: [30, 'idTeste']
    ```

    - Notas: Sem .where, atualiza todos! Use .set(sql`age = age + 1`) para expressões.

## Seção 8: Deleções (DELETE)

- **Função Principal: .delete(tabela).where(condição)**

  - Explicação: Deleta registros baseados em WHERE.
  - Exemplo:

    ```ts
    const deleteQuery = drizzleDb
      .delete(tabelaTeste) // Tabela a deletar.
      .where(eq(tabelaTeste.id, "idTeste")) // Condição.
      .returning({ deletedId: tabelaTeste.id }); // Retorna ids deletados.

    const result = await deleteQuery; // Executa.

    // SQL: delete from "tabelaTeste" where "tabelaTeste"."id" = ? returning "id"
    // Params: ['idTeste']
    ```

    - Notas: Sem .where, deleta tudo!

## Seção 9: Considerações Avançadas e Boas Práticas

- **Transações:** Use `drizzleDb.transaction(async (tx) => { await tx.insert(...); await tx.update(...); })` para atomicidade.
- **Prepared Statements:** Para performance: `const prepared = drizzleDb.select(...).prepare(); await prepared.execute({ params });`.
- **Erros Comuns:** Sempre await queries. Verifique tipos no schema para evitar erros.
- **Debugging:** Use .toSQL() sempre.
- **Migrações:** Use drizzle-kit para versionar schemas.
- **Suporte a Bancos:** Ajuste imports para MySQL (`mysql-core`) se necessário.
- **Documentação Oficial:** Consulte https://orm.drizzle.team para updates.

Isso cobre tudo de forma explícita! Se precisar de mais exemplos ou ajustes, avise.
