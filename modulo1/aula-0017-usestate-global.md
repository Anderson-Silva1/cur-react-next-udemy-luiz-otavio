# **useState de forma Global**

Podemos usar o useState para compartilhar estados dentro de nossos componentes atravéz dos `props`, porém dependendo da proporção desses state pode ser inviável

> Podemos usar o useState de forma global em nossa aplicação, basta criar nosso estado em um componente pai dos mesmos elementos que terá acesso a esse State

## Exemplo prático

**Elemento PAI**

```tsx
const App = () => {

const [name, setName] = useState("Anderson")

    return (
        <Filho1 name={name} setName={setName}></Filho1>
        <Filho1 name={name} setName={setName}></Filho1>
    )
}
```

Aqui vemos um caso real de como podemos fazer essa compatilhação de States do useState via props.

## Por que se tornaria inviável em alguns casos??

**Pense na possível hipótese:**

> Precisamos ter nosso State name dentro de um componente que é neto do filho do elemento `Filho`

Seria inviável fazer isso por causa da complexidade, precisaríamos passar o mesmo State via props pelo menos por uns 4 elementos.

## Prop Drilling

É o conceito disso que falamos anteriormente, passar State por props para 3 ou mais níveis

> Se nosso State está sendo passado por 3 ou mais níveis, já devemos considerar usar o contexto
