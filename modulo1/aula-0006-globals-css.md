# CSS GLOBAL

Vamos aprender a importar e exportar o CSS Global usando o React

---

## Padrão do VITE

Geralmente temos apenas um CSS Global em nossa aplicação que será importada no mais alto componente do nosso sistema.

Geralmente o nome do CSS Global da nossa aplicação é como o nome do mais alto componente do nosso sistema, mudando apenas a extenção.

Mas, popularmente usamos como CSS Global ou Globals CSS: `App.css` dentro da pasta `SRC`.

Esse arquivo CSS será importado como um arquivo mesmo, e tudo o que tiver nesse CSS será aplicado no componente que foi importado, no nosso caso o `App.tsx`

```tsx
import "./App.css";

function App() {
  return (
    <>
      <h1>App.css</h1>
    </>
  );
}

export default App;
```

---

## Padrão que usaremos

Criamos uma pasta chamada `styles` e dentro dela um arquivo chamado `global.css` (este para estilização global do nosso projeto) e `theme.css` (este para estilização de temas do projeto, tal como nossas variáveis CSS's)

Ao importar esses arquivos `CSS's`, vamos importar o `theme.css` primeiro, pois queremos que as variáveis existam para que o CSS possa encontrá-las
