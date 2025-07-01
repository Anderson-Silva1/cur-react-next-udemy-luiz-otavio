# **Efeitos colaterais**

Aprenderemos sobre os efeitos colaterais e especificamente sobre o hook `useEffect()`

## Sobre

useEffect é um hook que vai tratar efeitos colaterais, como alteraçõs manuais da DOM ou consumo de dados de uma API ou simplesmente algo aue o React não está monitorando

> Efeito colateral é tudo aquilo que o React Não espera que acaonteça, `como uma algo dentro do useState que não tem correlação com a variável monitorada, e etc...`

## Usando o eseEffect

Podemos usar o useEffect de 3 formas diferentes

1. De forma pura

2. Usando um array de dependências vazio

3. Usando um array de dependências

### 1. De forma pura

Usando dessa forma, sempre que qualquer componente for atualizado ou mesmo a página renderizada ou re-renderizada, ele execultará o useEffect

```tsx
import { useEffect } from "react";

useEffect(() => {
  console.log("useEffect sem dependências");
});
```

### 2. Usando um array de dependências vazio

Usando dessa forma, o useEffect será execultado apenas uma vêz, na hora do loading da página, ou quando abrirmos a primeira vêz. Usamos isso geralmente para fazermos consultas a bancos de dasdos, que geralmente são custosas para serem feitas.

```tsx
useEffect(() => {
  console.log(
    "useEffect com array de dependências vazio, execultada sempre uma vêz!!"
  );
}, []);
```

### 3. Usando um array de dependências

Usando dessa forma, teremos um estado (state) responsável pela execução do useEffect, sempre que esse state atualizar, o useEffect será execultado.

```tsx
useEffect(() => {
  console.log(
    "useEffect com array de dependências, execultada sempre quando o componente monitorado atualiza!!"
  );
}, [theme]);
```

> O `useEffect` sempre será execultado pelo menos uma quando abrimos a página

## Boa pratica (clean up function)

Uma boa pratica é limpar toda a sujeira do nosso useEffect, pois dentro dele se não tivermos cuidado ele pode gerar funções como um evento de assistir um elemento html como listen, e não parar a execução do mesmo, e quando o useEffect ser execultado de novo ele vai gerar um novo listen sem deletar o antigo, e isso polui a página, causando lentidão e travamento

Podemos solucionar isso usando uma função de retorno

```tsx
useEffect(() => {
  console.log(
    "useEffect com array de dependências, execultada sempre quando o componente monitorado atualiza!!"
  );

  return () => {
    console.log(
      "Aqui podemos desfazer cada operação feita dentro do useEffect"
    );
  };
}, [theme]);
```

### Fluxo

o useEffect será execultado, e depois, quando ele for ser execultado novamente, será execultado antes a função de retorno

> Isso serve para todos os tipos de usos do useEffect
