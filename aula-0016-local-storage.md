# Local Storage

Local Storage é um banco de dados de 5mb de armazenamento máximo que é usando no navegador para salvar informações do usuário, como no nosso caso `theme`

O Local Storage usa informações do tipo JSON para armazenar dados, ou seja:

```json
"theme": "dark"
```

Algo mais ou menos assim, usando o padrão: `chave`: `valor`

## Criando e atualizando dados no Local Storage

Para criar ou atualizar podemos usar o método `setItem`, que recebe 2 parâmetros, `chave`, `valor`

```tsx
localStorage.setItem("theme", "dark");
```

Para atualizar podemos fazer assim:

```tsx
localStorage.setItem("theme", "light");
```

## Acessando dados do LocalStorage

Podemos acessar dados do local storage usando o método `getItem`, que recebe um parâmetro, a chave do nosso dado:

```tsx
localStorage.getItem("theme");
```

E para acessá-la podemos usar o `lazy initialization` do nosso state, e pegar ela

```tsx
const [theme, setTheme] = useState(() => {
  const storageTheme = localStorage.getItem("theme") || "dark";

  return storageTheme;
});
```

Dessa forma podemos pegar o dado do localStorage, e se ele não existir, ou alguém apagou, ele pegará o valor de dark
