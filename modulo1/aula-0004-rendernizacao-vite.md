# **Entendendo como o vite renderiza HTML**

Dentro do nosso projeto, há um arquivo chamado `index.html`, e esse arquivo é um arquivo `.html` normal, porém dentro da tag `<main>` temos uma div com um ID `root`

```html
<div id="root"></div>
```

Dentro desse mesmo arquivo html temos uma tag `script` que está importando o script `main.tsx` dentro da pasta `src`

```html
<script type="module" src="/src/main.tsx"></script>
```

---

## main.tsx

Este arquivo vai procurar a div com o id `root` dentro do nosso html

```js
document.getElementById("root");
```

Ele vai usar o React para renderizar o componente `App`

```js
createRoot(document.getElementById('root')!).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

> `StrictMode` é uma espécie de debugador do Vite

---

## App

É o compoente que vai conter a estrutura do nosso projeto em React

```js
import { useState } from "react";
import "./App.css";

function App() {
  return (
    <>
      <div>Componente App</div>
    </>
  );
}

export default App;
```
