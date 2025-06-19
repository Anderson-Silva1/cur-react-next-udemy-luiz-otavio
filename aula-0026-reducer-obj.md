# **useReducer com Objeto**

Aprendemos aula passada a usar o `useReduce` de forma simplificada

Nessa agora vamos usar o `useReducer` com um objeto que se torna mais próximo da realidade

---

Primeiro precisamos entender algumas coisas:

1. **useReducer** => Hook do React que recebe o Reducer e um estado inicial;

2. **reducer** => Função que recebe o estado atual e uma função, e retorna o novo estado;

3. **state** => O estado atual;

4. **actions** => Ação disparada, geralmente é um objeto com type e (opcionalmente) um payload;

5. **type** => O tipo da ação, geralmente uma string (pode ser um enum, uma constante, etc);

6. **payload** => Os dados extras enviados junto com a action, se necessário para atualizar o estado.

---

Sabendo disso, vamos para nosso `useReducer` básico

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

Usando um objeto nesse caso seria assim:

```tsx
const App = () => {
  const [numero, dispatch] = useReducer(
    (state, { type, payload }) => {
      switch (action.type) {
        case "INCREMENTAR":
          return state + payload;
      }

      return state;
    },
    { numero: 0 }
  );

  return (
    <>
      <h1>O número é {numero}</h1>
      <button onClick={dispatch({ type: "INCREMENTAR", payload: 10 })}>
        INCREMENTAR
      </button>
    </>
  );
};

export default App;
```

Nossa action se tornou `{type, payload}`

Eu gosto de separar o `reducer` do `useReducer`

```tsx
const App = () => {
  const reducer = (state, { type, payload }) => {
    switch (action.type) {
      case "INCREMENTAR":
        return state + payload;
    }

    return state;
  };

  const [numero, dispatch] = useReducer(reducer, { numero });

  return (
    <>
      <h1>O número é {numero}</h1>
      <button onClick={dispatch({ type: "INCREMENTAR", payload: 10 })}>
        INCREMENTAR
      </button>
    </>
  );
};

export default App;
```
