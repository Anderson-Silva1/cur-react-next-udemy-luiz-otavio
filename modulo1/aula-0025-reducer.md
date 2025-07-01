# **useReducer**

É uma forma de dividir a atualização de um estado, ou seja, com ele podemos separar cada atualização do nosso estado (mais usado quando trabalhamos de estados em Objetos e Listas)

## Criando **useReducer**

criamos uma lista constante que recebe nosso `useReducer()`

```tsx
const [] = useReducer();
```

Dentro dessa lista teremos o nome do nosso estado, e uma função de disparar ações, que por converção chamamos ela de `dispatch`

```tsx
const [state, dispatch] = useReducer();
```

> **OBS**: Esse estado não necessariamente vem de um `useState`

Já dentro do nosso `useReducer`, teremos como parâmetro uma função e um valor inicial do nosso state, no nosso caso será um número, para exemplificação

```tsx
const [state, dispatch] = useReducer(() => {}, 0);
```

Dentro do primeiro parâmetro do `useReducer()` que é uma função, teremos 2 parâmetros:

1. Nosso Estado, que por convenção chamamos de `state`

2. Nossa ação, que por convenção chamamos de `action`

```tsx
const [state, dispatch] = useReducer((state, action) => {}, 0);
```

Essa mesma função deve retornar **SEMPRE** o `state` atual ou o `state` atualizado

```tsx
const [state, dispatch] = useReducer((state, action) => {
  return state;
}, 0);
```

Pediremos para renderizar na tela e nosso componente ficará assim:

```tsx
const App = () => {
  const [numero, dispatch] = useReducer((state, action) => {
    return state;
  }, 0);

  return <h1>O número é {numero}</h1>;
};

export default App;
```

## Alterando Estado com o useReducer

Dentro do `useReducer` precisaremos de um ponto de ação para atualizar nosso estado, no nosso caso será um botão com o evento de onClick()

```html
<button onClick="Função">Botão</button>
```

Dentro desse evento de onClick será o `dispatch()` retornando alguma coisa, no nosso caso uma String com o nome de `INCREMENTAR`, para fins didátidos, porém geralmente trabalhamos com objetos dentro do `dispatch()`

```tsx
<button onClick={dispatch("INCREMENTAR")}>Botão</button>
```

Essa função `dispatch()`, será a nossa `action` dentro do primeiro parâmetro do nosso `useReducer()`

```tsx
const App = () => {
  const [numero, dispatch] = useReducer((state, action) => {
    console.log(action); // retorna: INCREMENTAR
    return state;
  }, 0);

  return (
    <>
      <h1>O número é {numero}</h1>
      <button onClick={dispatch("INCREMENTAR")}>Botão</button>
    </>
  );
};

export default App;
```

Já dentro da função do nosso `useReducer()` usaremos a estrutura de decisão `switch` para alterar nosso estado

Esse `switch` recebará um argumento, que será a nossa `action`, e dentro dela podemos retornar nosso estado atualizado usando o `case`

```tsx
const App = () => {
  const [numero, dispatch] = useReducer((state, action) => {
    switch (action) {
      case "INCREMENTAR":
        return state + 1;
    }

    return state;
  }, 0);

  return (
    <>
      <h1>O número é {numero}</h1>
      <button onClick={dispatch("INCREMENTAR")}>INCREMENTAR</button>
    </>
  );
};

export default App;
```

Ecemplo mais pratico:

```tsx
const App = () => {
  const [numero, dispatch] = useReducer((state, action) => {
    switch (action) {
      case "INCREMENTAR":
        return state + 1;
      case "DECREMENTAR":
        return state - 1;
      case "ZERAR":
        return 0;
    }

    return state;
  }, 0);

  return (
    <>
      <h1>O número é {numero}</h1>
      <button onClick={dispatch("INCREMENTAR")}>INCREMENTAR</button>
      <button onClick={dispatch("DECREMENTAR")}>DECREMENTAR</button>
      <button onClick={dispatch("ZERAR")}>ZERAR</button>
    </>
  );
};

export default App;
```

DE FORMA SIMPLES, ASSIM USAMOS O `useReducer`
