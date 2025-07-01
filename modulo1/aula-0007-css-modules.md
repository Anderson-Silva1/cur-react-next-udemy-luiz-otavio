# **CSS MODULE**

É o CSS modular de cada componente , ou o CSS com escopo, pois tratar tudo em um único arquivo CSS pode nos trazer muitos bugs e problemas de estilizações.

Para resolver isso usamos o CSS Module:

1. primeiro criamos nosso componente na pasta `componentes`

2. Criamos agora nosso arquivo CSS com o nome `<nome do componente.module.css>` o que possibilita usar um aliás em JavaScript, e por convenção usamos `styles`

```css
.heading {
  color: red;
}
```

```tsx
import styles from "./Heading.module.css";

export function Heading() {
  return <h1 className={styles.heading}>Hello world!</h1>;
}
```

Dessa forma se em outro arquivo de CSS Module tiver a classe `heading` não entrará em conflito com essa classe, e será uma classe independente dessa.

> Para usarmos classes HTML dentro de um componente React, não podemos usar a palavra `class` pois faz parte da sintaxe do javaScript, então usamos a palavra `classname`

Para nos ajudar a trabalhar com o CSS Module, vamos instalar uma extenção chamada `CSS Modules` e com ela o VSCode nos ajuda a saber quais são as classes CSS que existem dentro de um arquivo CSS Module

---

Podemos ter mais de uma classe de CSS Modules para um elementos HTML

```tsx
<h1 className={`${styles.heading} ${styles.textRed}`}>Hello world!</h1>
```

Caso nossa classe tenha um "-" ou algo assim, também podemos contornar essa situação

```tsx
<h1 className={`${styles.heading} ${styles["text-red"]}`}>Hello world!</h1>
```
