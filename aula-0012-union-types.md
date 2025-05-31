# Union Types

É uma forma de definir sequência de opções únicas em TypeScript, nela usamos a "|" para fazer a separação entre as opções, exemplo:

> Temos um pomponente Input, e esse componente deve receber o `type` por `props`, e esse `type` deve ser `text` ou `number`

Então fazemos assim:

Componente:

```tsx
type InputProps = {
  type: "text" | "number";
};

export const Input = ({ type }: InputProps) => {
  return <input type={type} />;
};
```

Ao chamar o componente no componente pai:

```tsx
import { Input } from "./input"

<Input type="text">
<Input type="number">
<Input type="teste">
```

`<Input type="teste">` dará um erro no TypeScript, da mesma forma se omitirmos esse atributo HTML

---

### Atributos opcionais

Podemos usar atributos opcionais, basta colocar "?" no final da chave do nosso Union Types

```tsx
type InputProps = {
  type: "text" | "number";
  id?: string;
};

export const Input = ({ type, id }: InputProps) => {
  return <input type={type} id={id} />;
};
```

Dessa forma, podemos omitir o atributo ID

```tsx
import { Input } from "./input"

<Input type="text" id="1">
<Input type="number">
<Input type="teste" id="2">
```

E não será retornado um error

---

### Intersection

É uma forma de somar ou fazer a junção de objetos, no nosso caso de priedades HTML para nosso componente.

Usamos ela como sua sintaxe o & para fazer essa junção

Podemos usar um carinha chamado `React.ComponentProps<tag html>`

```tsx
type InputProps = {} & React.ComponentsProps<input>;

export const Input = ({ type, id }: InputProps) => {
  return <input type={type} id={id} />;
};
```

Podemos usar o `interface`

```tsx
interface InputProps {} extends  React.ComponentsProps<input>;

export const Input = ({ type, id }: InputProps) => {
  return <input type={type} id={id} />;
};
```
