# Array

Essa aula é para mostrar como podemos criar `Array's` de forma fácil para criar "dados" para nosso projeto

## Forma simples

```js
const array = [1, 2, 3, 4, 5];
```

## Usando contrutores

```js
const array = new Array(); // []
```

```js
const array = new Array(1, 2, 3, 4, 5); // [1, 2, 3, 4, 5]
```

```jsx
Array.from({ length: 20 }).map(() => {
  return (
    <tr>
      <td>Estudar React</td>
      <td>15 min</td>
      <td>13/10/2004 08:00</td>
      <td>Andamento</td>
      <td>Foco</td>
    </tr>
  );
});
```

Podemos retornar JSX
