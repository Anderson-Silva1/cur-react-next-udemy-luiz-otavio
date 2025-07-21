# 📁 Padrão de Projeto: Repository

O **padrão de projeto Repository** é amplamente utilizado no desenvolvimento de aplicações modernas, especialmente em arquiteturas orientadas a domínio (DDD – Domain-Driven Design). Ele é um dos pilares quando se busca escalabilidade, testabilidade e separação de responsabilidades. É comum encontrá-lo em frameworks como NestJS, Spring Boot, Laravel, AdonisJS, entre outros.

---

## 💡 O que é o Padrão Repository?

O **Repository** atua como uma **camada de abstração entre a lógica de negócios e a camada de persistência de dados** (como um banco de dados relacional ou um arquivo JSON). A principal ideia é encapsular as operações de acesso a dados, permitindo que a aplicação não dependa diretamente da forma como os dados são armazenados ou recuperados.

---

## 🌟 Objetivos do Repository Pattern

- 📚 **Centralizar o acesso aos dados** em um único ponto da aplicação.
- 🔌 **Isolar a lógica de persistência** da lógica de negócio.
- 🥪 **Facilitar testes unitários** por meio de injeção de dependência e mocks.
- 🔄 **Permitir mudanças na tecnologia de armazenamento** sem impactar a lógica da aplicação.
- 🧼 **Melhorar a legibilidade e manutenibilidade** com código mais limpo e coeso (princípio da responsabilidade única).

---

## 🧱 Estrutura de Implementação

### 📌 1. Modelagem do tipo base

```ts
// src/models/post-model.ts
export type PostModel = {
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

---

### 📌 2. Interface do repositório

Essa interface define o contrato que qualquer implementação de `PostRepository` deve seguir.

```ts
// src/repositories/post-repository.ts
import { PostModel } from "@/models/post-model";

export interface PostRepository {
  findAll(): Promise<PostModel[]>;
  findById(id: string): Promise<PostModel>;
}
```

---

### 📌 3. Implementação concreta com JSON (mock database)

```ts
// src/repositories/json-post-repository.ts
import { PostModel } from "@/models/post-model";
import { PostRepository } from "./post-repository";
import { resolve } from "path";
import { readFile } from "fs/promises";

const ROOT_DIR = process.cwd();
const JSON_POSTS_FILE_PATH = resolve(
  ROOT_DIR,
  "src",
  "db",
  "seeds",
  "posts.json"
);

export class JsonPostRepository implements PostRepository {
  private async readFromDisc(): Promise<PostModel[]> {
    const jsonContent = await readFile(JSON_POSTS_FILE_PATH, "utf-8");
    const parsedJson = JSON.parse(jsonContent);
    return parsedJson.posts;
  }

  public async findAll(): Promise<PostModel[]> {
    return this.readFromDisc();
  }

  public async findById(id: string): Promise<PostModel> {
    const posts = await this.findAll();
    const post = posts.find((post) => post.id === id);
    if (!post) {
      throw new Error(`Post with id: "${id}" not found`);
    }
    return post;
  }
}

export const postRepository: PostRepository = new JsonPostRepository();
```

---

### 🔍 Detalhes importantes

#### 📂 `readFromDisc()` como função privada

Esta função é de uso interno e serve para centralizar a leitura dos dados do "banco de dados fake" (no caso, um arquivo JSON):

```ts
private async readFromDisc(): Promise<PostModel[]> {
  const jsonContent = await readFile(JSON_POSTS_FILE_PATH, "utf-8");
  const parsedJson = JSON.parse(jsonContent);
  return parsedJson.posts;
}
```

#### 🔐 Uso de `process.cwd()`

Ao invés de usar caminhos relativos, utilizamos `process.cwd()` para obter o diretório raiz da aplicação, o que é extremamente útil em ambientes como o **Next.js**, onde o cache de arquivos e o comportamento em produção podem afetar a leitura de arquivos estáticos.

```ts
const ROOT_DIR = process.cwd();
const JSON_POSTS_FILE_PATH = resolve(
  ROOT_DIR,
  "src",
  "db",
  "seeds",
  "posts.json"
);
```

---

## 📆 Quando utilizar Repository?

- Quando quiser aplicar **boas práticas de arquitetura limpa**;
- Quando pretende usar **injeção de dependência** e testes unitários;
- Em projetos com alta chance de **migração de tecnologias de banco** (Ex: SQLite → PostgreSQL);
- Ao implementar **DDD** ou **Clean Architecture**.

---

## 🔗 Conclusão

O padrão Repository é um excelente aliado na manutenção de código limpo, desacoplado e pronto para evoluir com o projeto. Ele promove não só uma melhor organização, mas também robustez e testabilidade no ciclo de vida da aplicação.

> "O Repository não é apenas uma abstração: é um contrato de confiabilidade entre o mundo da persistência e o mundo da regra de negócio."
