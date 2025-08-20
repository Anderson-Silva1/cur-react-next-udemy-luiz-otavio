atualizado pelo grok

# 📚 Estudo: Padrão de Projeto Repository

> **Nota pessoal**: Este documento é meu guia de estudo sobre o padrão Repository. Vou resumir o que é, por que usar, como implementar e incluir exemplos práticos para fixar o conteúdo. O objetivo é entender bem o padrão para aplicá-lo em projetos reais, como em frameworks como NestJS ou em arquiteturas como DDD.

---

## 💡 O que é o Padrão Repository?

O **Repository** é um padrão de projeto que atua como uma **camada de abstração** entre a **lógica de negócios** (regras da aplicação) e a **persistência de dados** (como bancos de dados, arquivos, etc.). Ele encapsula as operações de acesso a dados, escondendo os detalhes de como os dados são armazenados ou recuperados.

- **Objetivo principal**: Desacoplar a lógica de negócios da forma como os dados são gerenciados, tornando o código mais limpo, testável e fácil de manter.
- **Onde é usado?** Em arquiteturas como **DDD (Domain-Driven Design)**, **Clean Architecture**, ou em frameworks como NestJS, Spring Boot e Laravel.

> **Anotação**: Pense no Repository como um "intermediário" que conversa com o banco de dados (ou outro armazenamento) e entrega os dados para a aplicação sem que ela precise saber como os dados são buscados ou salvos.

---

## 🌟 Por que usar o Repository?

Estudando o padrão, percebi que ele tem vários benefícios que ajudam no desenvolvimento de aplicações modernas:

1. **Centralização do acesso a dados**: Todas as operações de dados ficam em um só lugar (o repositório).
2. **Desacoplamento**: A lógica de negócios não precisa saber se os dados vêm de um banco SQL, NoSQL ou arquivo JSON.
3. **Testabilidade**: Como o repositório segue uma interface, posso criar mocks para testes unitários.
4. **Manutenibilidade**: O código fica mais organizado e fácil de atualizar (ex.: mudar de SQLite para PostgreSQL).
5. **Flexibilidade**: Posso adicionar filtros, paginação ou outras funcionalidades no repositório sem mexer na lógica de negócios.

> **Anotação**: Isso é ótimo para projetos que podem crescer ou mudar de tecnologia no futuro. Também facilita escrever testes, já que posso simular o comportamento do banco de dados.

---

## 🧱 Como implementar o padrão Repository?

Para entender como funciona, vou estruturar a implementação em três partes principais:

1. **Modelo de dados** (o que vou armazenar).
2. **Interface do repositório** (o contrato que define as operações).
3. **Implementação concreta** (como os dados são realmente manipulados, ex.: com JSON ou banco de dados).

### 📌 1. Modelo de Dados

Primeiro, preciso definir o tipo de dado que vou manipular. No exemplo, usarei um modelo de "Post" (como em um blog).

```ts
// src/models/post-model.ts
export type PostModel = {
  id?: string; // Opcional para criação (o banco pode gerar)
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

> **Anotação**: Notei que o `id` é opcional porque, ao criar um post, o banco de dados (ou o repositório) pode gerar o ID automaticamente. Os campos `createdAt` e `updatedAt` também são gerenciados pelo repositório.

### 📌 2. Interface do Repositório

A interface define o **contrato** que qualquer implementação do repositório deve seguir. Isso garante que, se eu mudar a fonte de dados (ex.: de JSON para PostgreSQL), a lógica de negócios não será afetada.

```ts
// src/repositories/post-repository.ts
import { PostModel } from "@/models/post-model";

export interface PostRepository {
  /**
   * Recupera todos os posts com opções de filtro e paginação
   * @param options Filtros opcionais (ex.: published, pagination)
   * @returns Lista de posts
   */
  findAll(options?: {
    published?: boolean;
    limit?: number;
    offset?: number;
  }): Promise<PostModel[]>;

  /**
   * Recupera um post por ID
   * @param id Identificador do post
   * @throws NotFoundError se o post não for encontrado
   */
  findById(id: string): Promise<PostModel>;

  /**
   * Cria um novo post
   * @param post Dados do post a ser criado
   * @returns Post criado
   */
  create(
    post: Omit<PostModel, "id" | "createdAt" | "updatedAt">
  ): Promise<PostModel>;

  /**
   * Atualiza um post existente
   * @param id Identificador do post
   * @param post Dados a serem atualizados
   * @throws NotFoundError se o post não for encontrado
   */
  update(id: string, post: Partial<PostModel>): Promise<PostModel>;

  /**
   * Deleta um post
   * @param id Identificador do post
   * @throws NotFoundError se o post não for encontrado
   */
  delete(id: string): Promise<void>;
}
```

> **Anotação**: A interface é como uma "promessa" do que o repositório deve fazer. Os métodos CRUD (`create`, `read`, `update`, `delete`) são padrão, e o `findAll` suporta filtros e paginação, o que é comum em APIs reais.

### 📌 3. Implementação Concreta (JSON)

Para praticar, implementei o repositório usando um arquivo JSON como "banco de dados". Isso é útil para protótipos ou testes, mas em produção usaria um banco real.

```ts
// src/repositories/json-post-repository.ts
import { PostModel } from "@/models/post-model";
import { PostRepository } from "./post-repository";
import { resolve } from "path";
import { readFile, writeFile } from "fs/promises";
import { v4 as uuidv4 } from "uuid";

const ROOT_DIR = process.cwd();
const JSON_POSTS_FILE_PATH = resolve(
  ROOT_DIR,
  "src",
  "db",
  "seeds",
  "posts.json"
);

// Erro personalizado para "não encontrado"
class NotFoundError extends Error {
  constructor(message: string) {
    super(message);
    this.name = "NotFoundError";
  }
}

export class JsonPostRepository implements PostRepository {
  private async readFromDisc(): Promise<PostModel[]> {
    try {
      const jsonContent = await readFile(JSON_POSTS_FILE_PATH, "utf-8");
      const parsedJson = JSON.parse(jsonContent);
      return parsedJson.posts;
    } catch (error) {
      throw new Error(`Failed to read posts from file: ${error.message}`);
    }
  }

  private async writeToDisc(posts: PostModel[]): Promise<void> {
    try {
      await writeFile(
        JSON_POSTS_FILE_PATH,
        JSON.stringify({ posts }, null, 2),
        "utf-8"
      );
    } catch (error) {
      throw new Error(`Failed to write posts to file: ${error.message}`);
    }
  }

  async findAll(
    options: {
      published?: boolean;
      limit?: number;
      offset?: number;
    } = {}
  ): Promise<PostModel[]> {
    const posts = await this.readFromDisc();
    let filteredPosts = posts;

    // Aplicar filtro de published, se fornecido
    if (options.published !== undefined) {
      filteredPosts = posts.filter(
        (post) => post.published === options.published
      );
    }

    // Aplicar paginação
    const offset = options.offset || 0;
    const limit = options.limit || filteredPosts.length;
    return filteredPosts.slice(offset, offset + limit);
  }

  async findById(id: string): Promise<PostModel> {
    const posts = await this.readFromDisc();
    const post = posts.find((post) => post.id === id);
    if (!post) {
      throw new NotFoundError(`Post with id: "${id}" not found`);
    }
    return post;
  }

  async create(
    post: Omit<PostModel, "id" | "createdAt" | "updatedAt">
  ): Promise<PostModel> {
    const posts = await this.readFromDisc();
    const newPost: PostModel = {
      ...post,
      id: uuidv4(),
      createdAt: new Date().toISOString(),
      updatedAt: new Date().toISOString(),
    };
    posts.push(newPost);
    await this.writeToDisc(posts);
    return newPost;
  }

  async update(id: string, post: Partial<PostModel>): Promise<PostModel> {
    const posts = await this.readFromDisc();
    const postIndex = posts.findIndex((p) => p.id === id);
    if (postIndex === -1) {
      throw new NotFoundError(`Post with id: "${id}" not found`);
    }
    const updatedPost = {
      ...posts[postIndex],
      ...post,
      updatedAt: new Date().toISOString(),
    };
    posts[postIndex] = updatedPost;
    await this.writeToDisc(posts);
    return updatedPost;
  }

  async delete(id: string): Promise<void> {
    const posts = await this.readFromDisc();
    const postIndex = posts.findIndex((p) => p.id === id);
    if (postIndex === -1) {
      throw new NotFoundError(`Post with id: "${id}" not found`);
    }
    posts.splice(postIndex, 1);
    await this.writeToDisc(posts);
  }
}
```

> **Anotação**: Essa implementação usa um arquivo JSON como "banco fake". Aprendi que:
>
> - O método `readFromDisc` centraliza a leitura do arquivo, evitando repetição.
> - O `writeToDisc` permite salvar alterações (create, update, delete).
> - O `uuid` gera IDs únicos, o que é útil para simular um banco de dados.
> - O `NotFoundError` ajuda a tratar erros de forma mais clara.

### 📌 4. Exemplo com Banco de Dados (Prisma)

Para entender como o Repository funciona em um projeto real, estudei uma implementação com **Prisma** (um ORM para Node.js). Aqui, o repositório conversa com um banco de dados real.

```ts
// src/repositories/prisma-post-repository.ts
import { PostModel } from "@/models/post-model";
import { PostRepository } from "./post-repository";
import { PrismaClient } from "@prisma/client";

class NotFoundError extends Error {
  constructor(message: string) {
    super(message);
    this.name = "NotFoundError";
  }
}

export class PrismaPostRepository implements PostRepository {
  constructor(private readonly prisma: PrismaClient) {}

  async findAll(
    options: {
      published?: boolean;
      limit?: number;
      offset?: number;
    } = {}
  ): Promise<PostModel[]> {
    return this.prisma.post.findMany({
      where: { published: options.published },
      take: options.limit,
      skip: options.offset,
    });
  }

  async findById(id: string): Promise<PostModel> {
    const post = await this.prisma.post.findUnique({ where: { id } });
    if (!post) {
      throw new NotFoundError(`Post with id: "${id}" not found`);
    }
    return post;
  }

  async create(
    post: Omit<PostModel, "id" | "createdAt" | "updatedAt">
  ): Promise<PostModel> {
    return this.prisma.post.create({
      data: {
        ...post,
        createdAt: new Date().toISOString(),
        updatedAt: new Date().toISOString(),
      },
    });
  }

  async update(id: string, post: Partial<PostModel>): Promise<PostModel> {
    const existingPost = await this.findById(id);
    return this.prisma.post.update({
      where: { id },
      data: { ...post, updatedAt: new Date().toISOString() },
    });
  }

  async delete(id: string): Promise<void> {
    await this.findById(id); // Garante que o post existe
    await this.prisma.post.delete({ where: { id } });
  }
}
```

> **Anotação**: A diferença principal é que o Prisma lida diretamente com o banco de dados. O repositório traduz as operações do Prisma para o contrato da interface `PostRepository`, mantendo a lógica de negócios isolada.

### 📌 5. Injeção de Dependência (Exemplo com NestJS)

Para usar o repositório em uma aplicação real, aprendi que frameworks como **NestJS** usam injeção de dependência para gerenciar instâncias do repositório. Isso facilita trocar a implementação (ex.: de JSON para Prisma).

```ts
// src/post/post.module.ts
import { Module } from "@nestjs/common";
import { PostRepository } from "@/repositories/post-repository";
import { JsonPostRepository } from "@/repositories/json-post-repository";

@Module({
  providers: [
    {
      provide: PostRepository,
      useClass: JsonPostRepository, // Pode ser trocado por PrismaPostRepository
    },
  ],
  exports: [PostRepository],
})
export class PostModule {}
```

```ts
// src/post/post.service.ts
import { Injectable, Inject } from "@nestjs/common";
import { PostRepository } from "@/repositories/post-repository";

@Injectable()
export class PostService {
  constructor(
    @Inject(PostRepository) private readonly postRepository: PostRepository
  ) {}

  async getAllPosts() {
    return this.postRepository.findAll();
  }

  async getPostById(id: string) {
    return this.postRepository.findById(id);
  }

  async createPost(post: Omit<PostModel, "id" | "createdAt" | "updatedAt">) {
    return this.postRepository.create(post);
  }
}
```

> **Anotação**: A injeção de dependência é poderosa porque permite trocar a implementação do repositório sem mudar o código do serviço. Isso é ótimo para testes e para quando o projeto muda de tecnologia.

---

## 📆 Quando usar o Repository?

Depois de estudar, entendi que o padrão Repository é ideal em alguns cenários específicos:

- Quando quero seguir **boas práticas** de arquitetura limpa ou DDD.
- Quando preciso fazer **testes unitários** (posso mockar o repositório).
- Quando há chance de **mudar o banco de dados** (ex.: de JSON para MongoDB).
- Quando quero **organizar o código** e separar a lógica de negócios da persistência.

> **Anotação**: O Repository é como um "escudo" que protege a aplicação de mudanças no banco de dados. Isso me economiza tempo no futuro se o projeto crescer ou mudar.

---

## 🔍 Dicas e Observações

1. **Erros Personalizados**: Usar erros como `NotFoundError` ajuda a tratar falhas de forma clara e evita erros genéricos.
2. **Filtros e Paginação**: Métodos como `findAll` devem suportar filtros (ex.: `published: true`) e paginação (`limit`, `offset`) para APIs reais.
3. **Testes**: O padrão Repository facilita testes porque posso criar mocks da interface sem depender de um banco real.
4. **Escalabilidade**: Em projetos grandes, o repositório pode incluir métodos mais complexos, como consultas com joins ou transações.

> **Anotação**: Preciso praticar a escrita de testes unitários para o repositório. Isso vai me ajudar a entender melhor como mockar dependências.

---

## 🔗 Resumo Final

O padrão Repository é uma forma de **organizar e desacoplar** o acesso a dados na aplicação. Ele cria uma camada entre a lógica de negócios e o armazenamento, facilitando:

- Manutenção do código.
- Troca de tecnologias de persistência.
- Testes unitários com mocks.
- Escalabilidade com filtros e paginação.

A implementação com JSON é boa para aprender, mas em projetos reais, usaria um banco de dados com um ORM como Prisma. A injeção de dependência (como no NestJS) é a cereja do bolo, pois torna tudo mais flexível.

> **Anotação final**: Vou tentar implementar esse padrão em um projeto pessoal com NestJS e Prisma para fixar o aprendizado. Também quero estudar mais sobre testes unitários para repositórios e como usar transações em bancos relacionais.
