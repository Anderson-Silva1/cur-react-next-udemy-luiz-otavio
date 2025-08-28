# Cache do NEXT.JS

Quando Fazemos o Build da nossa aplicação usando o comando `npm run build`, o next faz a transformação das páginas Estáticas (Static) para realmente Estática...

> Quando estamos em modo de Desenvolvimento, todaas as páginas assumem o comportamento Dinâmico (Dynamic)

Ao fazermos Build da nossa aplicação é gerado um relatório do que foi feito, que será mais ou menos assim:

```txt
Route (app)                                 Size  First Load JS
┌ ○ /                                      185 B         110 kB
├ ○ /_not-found                            136 B         101 kB
└ ƒ /post/[slug]                           496 B         107 kB
+ First Load JS shared by all             101 kB
  ├ chunks/4bd1b696-daa26928ff622cec.js  53.2 kB
  ├ chunks/684-7de0ff74ee7d88a0.js       45.9 kB
  └ other shared chunks (total)          1.99 kB
```

Nesse relatório vemos algumas informações:

- **○** (Static) Representa o conteúdo estático

- **ƒ** (Dynamic) Representao conteúdo dinâmico

---

## Entendendo melhor o Cache

Vamos criar uma rota chamada: `cache-teste`

Dentro dela vamos renderizar um componente que vai pegar um conteúdo dinâmico, por exemplo, vamos usar a função `Date.now()` para pegar a hora exata do momento

Em modo de desenvolvimento ele sempre será renderizado novamente, pois em modo de DESENVOLVIMENTO todas as rotas são dinâmicas

Porém se fizermos o `npm run build` da nossa aplicação vamos ter uma rota estática e a mesma não será mais renderizada ao fazer o reload....

## Quando eu terei uma rota dinâmica?

Quando minha rata renderizar algo dinâmico como:

- Cookies
- Header
- Search Params

Com isso o Next assume esta rota como dinâmica...

Vamos criar a rota `[id]/page.tsx` dentro da rota `cache-teste`

Fazendo o `npm run build` teremos agora:

```txt
Route (app)                                 Size  First Load JS
┌ ○ /                                      185 B         110 kB
├ ○ /_not-found                            142 B         101 kB
├ ○ /cache-teste                           142 B         101 kB
├ ƒ /cache-teste/[id]                      142 B         101 kB
└ ƒ /post/[slug]                           496 B         107 kB
+ First Load JS shared by all             101 kB
  ├ chunks/4bd1b696-daa26928ff622cec.js  53.2 kB
  ├ chunks/684-7de0ff74ee7d88a0.js       45.9 kB
  └ other shared chunks (total)          1.99 kB
```

A rota `/cache-teste` é uma rota estática

A rota `/cache-teste/[id]` é uma rota dinâmica
