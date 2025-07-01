# Vamos configurar nosso tema claro e escuro

Tema claro:

```css
:root[data-theme="light"] {
  --gray-100: #0a0f1a;
  --gray-200: #181f2e;
  --gray-300: #272f43;
  --gray-400: #363d56;
  --gray-500: #454f6a;
  --gray-600: #555f7d;
  --gray-700: #aab3cc;
  --gray-800: #cdd3e1;
  --gray-900: #e6e9f0;

  --text-default: #0a0f1a;
  --text-muted: #272f43;

  --link-color: #0b8a60;
  --link-hover: #065f46;
}
```

Isso nos dis que, se no html tiver o atributo `date-theme="light"` ele assumirá o tema claro, caso não terá o tema escuro, e vamos salvar no `local Storage`

---

Aprendemos sobr eo que é `preventDefault()` que é uma função que tira o comportamento padrão do elemento

```tsx
<a href="https://google.com" alt="Link exemplo">
  Clique a
</a>
```

Ao clicar nesse link, a página será redirecionada a página do Google

---

### Usando o `preventDefault()`

Precisamos ter o `event` do elemento, e no `event` pegamos o `preventDefault()`

```tsx
const handleExemploOnchange = (event) => {
  event.preventDefault();
  console.log("Evento interrompido!!");
};

<a href="https://google.com" alt="Link exemplo" onClick={handleExemploOnchange}>
  Clique a
</a>;
```

Dessa forma o evendo do Link de redirecionamento será interrompido
