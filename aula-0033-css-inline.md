# CSS Inline

Podemos usar o CSS inline em JSX, basta acionar o atributo `style`, e fazer ele receber um objeto

```tsx
<p
  style={
    {
      /*Objeto de propriedades*/
    }
  }
>
  Teste
</p>
```

Dentro desse objeto podemos colocar nossas propriedades CSS como chaves desse objeto

Caso a propriedade tenha um "-", usamos o padrão `camelCase` para escrevê-lá,

Os valores dessas chaves serão `strings` que são os valores das propriedades CSS

```tsx
<p
  style={{
    textAlign: "center",
    color: "red",
  }}
>
  Teste
</p>
```
