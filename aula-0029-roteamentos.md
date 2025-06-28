# **Roteamentos**

Vamos usar o `react-router`, que é um lib externa que nos ajuda a criar nossas rotas

## Instalação

`npm install react-router` - Este é na versão 7 dessa lib

`npm install react-router-dom` - Este é na versão 6 ou abaixo dessa lib

Para aprender mais sobre a versão 6 - [Clique aqui](https://www.youtube.com/watch?app=desktop&v=rvS-TdtM8Ak)

## Como usar?

Ele vai funcionar como um contexto, onde precisamos que ele envolva o principal componente da nossa aplicação, no caso a `<App />`

Envoçveremos nossa `<App />` por um carinha chamado `<BrowserRouter> </BrowserRouter>`

```tsx
function App() {
  return (
    <TaskContextProvider>
      <BrowserRouter>
        <Home />
      </BrowserRouter>
    </TaskContextProvider>
  );
}

export default App;
```

Dentro desse componente `<BrowserRouter>` teremos outro carinha chamado `<Routes> </Routes>` e dentro dele teremos outro componente, `<Router />`

Esse `<Router />` recebe como atributo:

1. `path` => URL da rota
2. `element` => Componente a ser renderizado

```ts
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
  </Routes>
</BrowserRouter>
```

No `path` temos o `"/"`, pois queremos pegar a rota principal do nosso projeto

Caso queiramos colocar uma rota existente, basta colocar ela dentro de `path`

```ts
<BrowserRouter>
  <Routes>
    <Route path="/about-pomodoro" element={<About />} />
  </Routes>
</BrowserRouter>
```

## Rota não enncontrada

Para r+pegar uma rota não encontrada usamos o `"*"` dentro de `path`

```tsx
<BrowserRouter>
  <Routes>
    <Route path="/" element={<Home />} />
    <Route path="*" element={<NotFound />} />
  </Routes>
</BrowserRouter>
```

## Componente `Link`

Esse `BroserRouter` tem um contexto, e dentro do nosso codigo podemos usar esse componente (`Link`) para criamos nossa SPA (Single Page Aplication)

Esse componente `Link`, é o que o nome já diz, e ele vai ter a função de uma tag `<a> </a>`

Porém dentro do `Link` não temos o atributo `href`, temos o `to` para referenciar qual rota queremos ir

E Dessa forma, todo o nosso siet é carregado apenas uma vêz, e quando acessarmos uma rota diferente, tudo o que já foi carregado antes, não será carregado novamente...

```tsx
<nav className={styles.menu}>
  <Link to="/" className={styles.icon}>
    <HomeIcon />
  </Link>
  <Link to="/history" className={styles.icon}>
    <HistoryIcon />
  </Link>
  <Link to="/config" className={styles.icon}>
    <BoltIcon />
  </Link>
  <Link to="" className={styles.icon} onClick={handleThemeChange}>
    <SunIcon />
  </Link>
</nav>
```
