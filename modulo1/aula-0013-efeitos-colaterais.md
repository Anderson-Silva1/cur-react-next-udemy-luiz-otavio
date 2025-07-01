# **Vamos aprender sobre o useState**

useState é um hook do React que trata eventos e altera elementos

> Se criatmor uma função contador que vai atualizar um componente adicionando o numero 1, veremos que não dará certo, muda no console.log, porém na tela não

> Então precisamos dizer ao React que quando um elemento ou uma variável mudar ele precisa re-renderizar-lá.

Para isso usamos o useState().

## Sintaxe

Declaramos um array contante, com dois elementos, sendo o primeiro: A variável a ser monitorada, e o segundo a variável modificada. E tudo isso recebe o useState e dentro do useStare como parâmetros teremos o valor padrão da variável monitorada

```tsx
const [count, setCount] = useState(0);
```

> Por convenção teremos o nome da variável mofificada (que será uma função) como set<Nome Variável> no padrão `camelcase`

---

### Código de atualização de estado!

```tsx
const [count, setCount] = useState(0);

console.log(count); // 0

setCount(1);

console.log(count); // 1
```

---

### Usando Função de CallBack

```tsx
const [count, setCount] = useState(0);

console.log(count); // 0

setCount((prevState) => {
  return prevState;
});

console.log(count); // 1
```

Isso serve no caso se quiser tratar ou validar os dados do useState

---

### Contador

```tsx
const App = () => {
  const [count, setCount] = useState(0);

  const handleClickMore = () => {
    setCount(count++);
  };

  const handleClickSub = () => {
    setCount(count--);
  };

  return (
    <>
      <button onClick={handleClickMore}>+</button>
      <button onClick={handleClickSub}>-</button>
      <span>{count}</span>;
    </>
  );
};
```

---

### Regra

O useState, por padrão, vai atualizar o componente apenas uma vêz, o que significa se chamarmos a função `setCount(count++)` mais de uma vêz, só será somado `+ 1` uma vêz

Porém se usarmos uma função de CallBack, o valor do componente atualizado será internamente armazenado e será feito a atualização quantas vezes necessário

```tsx
const [count, setCount] = useState(0);

  const handleClickMore = () => {
    setCount((prevState) => {
        prevState + 1
    });

        setCount((prevState) => {
        prevState + 1
    });

        setCount((prevState) => {
        prevState + 1
    });
  };

  const handleClickSub = () => {
    setCount((prevState) => {
        prevState - 1
    })

    setCount((prevState) => {
        prevState - 1
    })

    setCount((prevState) => {
        prevState - 1
    })
  };

  return (
    <>
      <button onClick={handleClickMore}>+</button>
      <button onClick={handleClickSub}>-</button>
      <span>{count}</span>;
    </>
  );
};
```

### lazy initialization

É uma forma de colocar um dado inicial dentro de um state, e pode também ser uma função de callback

```tsx
const [count, setCount] = useState(() => 0);
```

Dessa forma podemos usar uma função de callback para ter o primeiro valor inicializador do nosso state
