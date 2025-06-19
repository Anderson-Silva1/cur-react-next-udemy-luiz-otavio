# Trabalhando com o Tempo

Para converter um segudo em minutos, basta dividir os segundos por 60

```cmd
60 / 60
>> 1
```

Mas precisaremos cuida dos restos tbm

```cmd
65 / 60
>> 1,083333333333333
```

Para tratar isso podemos usar a função `floor()` do javaScript (que é um módulo da biblioteca `Math`), ela vai arredondar nosso número mara o inteiro mais próximo do zero

```js
let tempo = 65 / 60; // 1,083333333333333

Math.floor(tempo); // >> 1;
```

Depois precisamos transformar em string, para isso usamos a função `String`

```js
let tempo = 65 / 60; // 1,083333333333333

String(Math.floor(tempo)); // >> "1";
```

Depois queremos que quando tenhs apenas um número, o "ZERO" apareça na esquerda dele, para isso usamos a função `padStart`, u método de `string` que recebe 2 parâmetros:

1. Quantas casas decimais precisaremos ter

2. Caso não tenha as casas decimais declaradas no primeiro parâmetro ele vai retornar na esquerda o que valor que tiver aqui

```js
let tempo = 65 / 60; // 1,083333333333333

String(Math.floor(65 / 60)).padStart(2, "0"); // >> "01";
```

Nesse caso estamos definindo duas casas decimais, caso não tenha, teremos a `string` "0" à esquerda.
