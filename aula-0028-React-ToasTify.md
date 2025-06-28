# **React Toastify**

O React-Toastify é uma biblioteca para React que permite exibir notificações do tipo toast de forma rápida, elegante e não intrusiva. Os toasts são pequenas mensagens temporárias que aparecem na tela, geralmente no canto superior direito, e desaparecem sozinhas após alguns segundos.

## Documentação do Recact Toastify > [Clique aqui](https://fkhadra.github.io/react-toastify/introduction/)

## 📦 Principais Características do React-Toastify

| Recurso                      | Descrição                                                                                 |
| ---------------------------- | ----------------------------------------------------------------------------------------- |
| 🎨 Estilização automática    | Estilo bonito por padrão (dark/light), com possibilidade de customização via CSS ou tema. |
| ⏰ Autodestrutivo            | Os toasts somem automaticamente após um tempo configurável.                               |
| 🧠 Gerenciamento de fila     | Gerencia múltiplos toasts em fila, evitando poluição visual.                              |
| 🔁 Atualização em tempo real | Pode atualizar ou remover um toast programaticamente.                                     |
| 🛠️ Fácil de usar             | Com poucos comandos, já está funcionando.                                                 |

## Instalação

Comando de instalação: `npm install react-toastify`

Depois vamos usar a colinha que temos na documentação dentro do nosso mais alto componente, no caso, o <App />

```ts
<ToastContainer
  position="top-right"
  autoClose={5000}
  hideProgressBar={false}
  newestOnTop={false}
  closeOnClick={false}
  rtl={false}
  pauseOnFocusLoss
  draggable
  pauseOnHover
  theme="light"
  transition={Bounce}
/>
```

Depois basta usar o objeto `toast`

```tsx
toast.info("Mensagem teste");
```

Esse objeto terá alguns métodos, e são eles:

1. `toast.info()`: Mensagens de informação
2. `toast.error()`: Mensagens de erro
3. `toast.warn()`: Mensagens de alerta
4. `toast.warning()`: Mensagens de alerta
5. `toast.success()`: Mensagens de sucesso

---

### EXTRA

Quando estamos trabalhando com bibliotecas externas, como no caso o react-toastify, é uma boa prática termos um adapter

#### `adapter`

É basicamente um adaptador para nosso projeto, pois ao invéz de adicionarmos dentro do nosso componente em si, criamos uma função execultando essa biblioteca que importaremos dentro do nosso componente em si

Pois se algo estiver errado ou essa lib atualizar, pode quebrar nosso código quando formos resolver o bug

E estando em uma função separada, podemos ter um maior controle sobre imprevistos

### Criamos a pasta `adapters` e dentro, no nosso caso, um objeto com métodos sendo exportado, esse objeto será chamado de `showmessage`

```ts
import { toast } from "react-toastify";

export const showMessage = {
  success: (msg: string) => toast.success(msg),
  info: (msg: string) => toast.info(msg),
  error: (msg: string) => toast.error(msg),
  warn: (msg: string) => toast.warn(msg),
  warning: (msg: string) => toast.warning(msg),
};
```
