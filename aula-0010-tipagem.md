# Tipagem TypeScript

O JavaScript possui uma tipagem dinâmica, já o TypeScript possui uma tipagem estática, ou seja, precisamos definir o tipo de um determinaod dado, seja ele uma variável ou o retorno de um bloco de código.

## Type

No TypeScript, o `type` é uma ferramenta (ou keyword) que usamos para declarar tipos personalizados. Em outras palavras: **`ele permite que você crie alias de tipos ou tipos compostos, facilitando a legibilidade, manutenção e escalabilidade do código.`**

**👉 Definição formal:**
O type cria um Type Alias — um nome simbólico para um tipo, seja ele primitivo, complexo ou uma combinação de vários.

```ts
type somaProps = {
  a: number;
  b: number;
};

function soma({ a, b }: somaProps) {
  return a + b;
}
```

## Tipo geral para usar em React quando se trata de elementos

Usamos o tipo `React.ReactNode`

```tsx
type HeadingProps = {
  children: React.ReactNode;
};

export function Heading({ children }: HeadingProps) {
  return <h1>{children}</h1>;
}
```
