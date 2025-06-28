# **Adapter Link**

`Link` é um a componenente da lib externa que é modificada de tempos em tempos, e para não quebrar nosso código, ou melhor, caso ele (Link) mude algo, não precisaremos nos preoculparmos, pois não mexeremos no nosso código diretamente, criaremos um componente para nosso `Link`

---

### Vamos primeiro separar nossa rota da nossa aplicação

Criaremos uma pasta chamada `routers`, e dentro dela terá as nossas rotas, no nosso caso, um arquivo `MainRouter.tsx`, e dentro dele todo o nosso esquema de Rotas

```tsx
const MainRouter = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about-pomodoro" element={<About />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
    </BrowserRouter>
  );
};

export default MainRouter;
```

---

### Problema de ao mudar de rota, a página não ir para o início

Se você for espero, percebeu que quando mudamos de página,ou melhor, de rota, a página não inicia no começo dela, ele fica no meio, para resolver isso podemos pegar um hook especial do `react-router` que pega a `URL` em que estamos

#### useLocation()

É um hook da lib `react-router` que consegue pegar informações da URL em que estamos, porém ele só funciona se estiver dentro dpo componente `BrowserRouter`, que tem um contexto

Para resolver isso podemos criar um subcomponente dentro do componente para renderizar ele dentro do contexto de `BrowserRouter`

Esse subcomponente se chamará `WindowsScroll`, e ele precisa rertornar algo, pois um componente não pode ser uma função vazia ou um `void function`, por isso, nesse caso, esse subcomponente retornará o valor `null`, que não rendenizará nada na tela, porém todas as funções dele serão execultadas

Dentro desse subcomponente teremos uma variável `scroll` que receberá nosso `useLocation()`, e um `useEffect()` recebendo essa variável dentro do array de dependências

Esse `useEffect` vai conter o objeto `window` execultando o método `scrollTo` que recebe como parâmetro um objeto com as propriedades:

- top: 0
- behavior: "smooth"

```tsx
const WindowsScroll = () => {
  const scroll = useLocation();

  useEffect(() => {
    window.scrollTo({ top: 0, behavior: "smooth" });
  }, [scroll]);

  return null;
};

const MainRouter = () => {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about-pomodoro" element={<About />} />
        <Route path="*" element={<NotFound />} />
      </Routes>
      <WindowsScroll />
    </BrowserRouter>
  );
};

export default MainRouter;
```

---

## Agora vamos criar nosso Adapter Link

Vamos criar um componente chamado `RouterLink`, e dentro dele importaremos o `Link` do `react-router` e exportaremos para nossa aplicação

```tsx
interface RouterLinkProps extends React.ComponentProps<"a"> {
  children: React.ReactNode;
  href: string;
}

const RouterLink = ({ href, children, ...props }: RouterLinkProps) => {
  return (
    <Link to={href} {...props}>
      {children}
    </Link>
  );
};

export default RouterLink;
```
