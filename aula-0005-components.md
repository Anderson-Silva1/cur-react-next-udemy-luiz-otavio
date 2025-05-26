# **Componentes**

Antes de falar sobre os componentes precisamos entender o que é `jsx` ou `tsx`

## jsx ou tsx

Neste curso iremos usar o `tsx` na maioria dos casos.

Mas basicamente, `jsx` ou `tsx` é um tipo de sintaxe que permite (dentro do JavaScript ou TypeScript) usar HTML, possibilitando a criação de componentes React's

## O que é um componente?

Componente é uma função JavaScript que retorna HTML

```tsx
import { useState } from "react";
import reactLogo from "./assets/react.svg";
import viteLogo from "/vite.svg";
import "./App.css";

function App() {
  return <h1>Componente React</h1>;
}

export default App;
```

> Usamos os componentes em dois casos
>
> 1. Quando queremos reultilizar códigos
>
> 2. Para melhorar a legibilidade do nosso projeto e quebrar a lógica em pedaços

Temos algumas regras de criação, são elas:

1. Os componentes precisam ser nomeados em padrão PascalCase, ou seja, com a primeira letra maiúscula e a cada separação de palavras por espaço, também começam com a letra maiúscula, ex: **_ComponenteHeader_**, **_ComponenteMain_**...

2. Os componentes só podem retornar um elemento pai para todos os filhos, ou seja, não conseguiremos retornar duas tags div... Para isso usamos uma div fantasma ou os `fragments` (`<> </>`)

```tsx
import { useState } from "react";

function App() {
  return (
    <>
      <h1>Componente React</h1>
      <p>Teste 123</p>
    </>
  );
}

export default App;
```

3. Quando o retorno do nosso componente é mais de uma linha, precisamos usar o "()" para o return considerar todo o código e não só a primeira linha.

```tsx
import { useState } from "react";

function App() {
  return (
    <div>
      <h1>Componente React</h1>
    </div>
  );
}

export default App;
```
