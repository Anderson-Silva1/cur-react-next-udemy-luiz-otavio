# **preventDefault**

É um método do `event` de um elemento que vai parar o comportamento padrão dele, ex: se for uma tag `ancora/link` o `<a> </a>`, o comportamento padrão de redirecionar a uma página será interrompido

```tsx
const handleSubmit = (event) => {
  event.preventDefault();
};
```

Se estivermos usando o TpeScript esse trexo de código dará um erro de tipo no `event`, pois o tipo dele está `any` ou `qualquer coisa`

Para solucionar isso, podemos tipar com o tipo desse evento, que é: `React.FormEvent<HTMLFormElement>`

Nosso Form ficará assim:

```ts
type handleSubmitEvent = React.FormEvent<HTMLFormElement>;

const handleSubmit = (event: handleSubmitEvent) => {
  event.preventDefault();
};
```

Dessa forma o erro desaparecerá
