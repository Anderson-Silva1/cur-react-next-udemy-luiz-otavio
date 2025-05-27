# Props

> São propriedades que podem ser passados de um elemento pai para um elemento filho

Essas propriedades são passadas de um componente pai nos proprios componentes filhos pelos seus atributos HTML.

```tsx
<Heading atributo1="123" />
```

E são recebidas pelos parâmetros do componente filho

```tsx
export function Heading(props) {
  return <h1>{props.atributo1}</h1>;
}
```

`props` é o nome (por convenção que usamos para usar o `props`), e esse `props` será um objeto onde podemos desestruturá-lo.

```tsx
export function Heading({ atributo1 }) {
  return <h1>{atributo1}</h1>;
}
```

## Props Children

É uma propriedade especial do props que vai pegar tudo que tiver dentro do componente (estando ele dentro do componente pai) e colocar dentro dessa chave do objeto props

```tsx
function App() {
  return (
    <>
      <Heading>Teste</Heading>
      <Heading>Teste</Heading>
      <Heading>Teste</Heading>
      <Heading>Teste</Heading>
    </>
  );
}

export default App;
```

```tsx
export function Heading({ children }) {
  return <h1>{children}</h1>;
}
```

Resultado

```txt
Teste 1
Teste 2
Teste 3
Teste 4
```
