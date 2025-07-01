# **Contexts**

Vamos aprender sobre contextos em React:

> Em React, "**Contextos**" (**Context API**) são uma forma de compartilhar dados entre componentes sem a necessidade de passar props manualmente através de todos os níveis da árvore de componentes. Eles oferecem uma maneira de "transmitir" valores para componentes filhos, independentemente de quão profundos eles estejam aninhados.

O Contexto em React resolve o problema de `prop drilling`, que é passar um state ou simplesmente dados de um coponente Pai para seus filhos que em alguns casos está muito abaixo na hierarquia, o que significa que será preciso (em muitos casos) passar esses props de State para componentes que não vão precisar dele diretamente

## Criando um Contexto

Para criar um contexto vamos primeiro criar uma pasta chamada `contexts` dentro de `SRC`

Dentro dessa pasta (**contexts**) vamos criar nosso contexto, um arquivo chamado `taskModel`, que será o nome do arquivo com a extenção .tsx

### createContext

Criamos uma variável const com o nome do nosso contexto e exportamos ela (gosto de exportar como módulo), e colocamos ela para receber o método `createContext`, que cria uum contexto para nós, e dentro dele (como parâmetro) teremos o valor inicial do nosso contexto (não usamos ele na prática, usaremos o **Provider**)

> Essa variável precisa estar no padrão PascalCase, com a primeira letra maiúscula pois se trata de um componente React

```tsx
import { createContext } from "react";

const TaskModel = createContext({ chave: "valor" });
```

**TaskModel** é nosso Context.

## Acessando nosso Context

Basta ir em nosso componente e usar o hook `useContext()`

```tsx
import "./styles/theme.css";
import "./styles/global.css";
import { useContext } from "react";
import { taskModel } from "./contexts/taskModel";
import { AboutPomodoro } from "./pages/AboutPomodoro";

function App() {
  console.log(useContext(taskModel));

  return <AboutPomodoro />;
}

export default App;
```

Nesse caso estamos dando um `console.log()` no nosso contexto `TaskModel` e está sendo retornado um `objeto`, esse objeto é os dados do nosso contexto: `{ chave: "valor" }`

> Se a intenção for apenas ler dados do contexto, ele pode ser usado como somente leitura. No entanto, para que o contexto permita alterações e modificações de estado, é fundamental usar o Provider para disponibilizar tanto os dados quanto as funções necessárias para atualizá-los aos componentes filhos.

## Usando Provider

Usamos esse carinha quando estarmos tratando de Estados, no qual precisaremos alterar e modificar o mesmo (ESTADO)

Precisamos primeiro Chamar nosso Provider dentro de um componete superior que está acima de todos os outros componentes que precisarão desse Estado, no nosso caso no APP

Ele receberá um `value` e esse value será o valor a ser passado para todos os componentes

```tsx
import "./styles/theme.css";
import "./styles/global.css";

import { TaskModel } from "./contexts/taskModel";
import { NotFound } from "./pages/NotFound";

function App() {
  return (
    <TaskModel.Provider value={{ teste: "teste" }}>
      <NotFound />;
    </TaskModel.Provider>
  );
}

export default App;
```

Dessa forma tudo o que tiver dentro de `<TaskModel.Provider> </TaskModel.Provider>` terá o valor de `value`, e ao usar o `useContext(TaskModel)` não será mais o valor padrão (`{ chave: "valor" }`) que definimos quando criamos o contexto, será o valor de `value`: `{ teste: "teste" }`

```tsx
import { createContext } from "react";

export interface TaskStateModel {
  task: [];
  secondsRemaining: number;
  formatSecondsRemaining: string;
  activeTask: boolean;
  currentCycle: number;
  config: {
    workTime: number;
    shortBreackTime: number;
    longBreakTime: number;
  };
}

const initialTask: TaskStateModel = {
  task: [],
  secondsRemaining: 0,
  formatSecondsRemaining: "",
  activeTask: false,
  currentCycle: 0,
  config: { workTime: 25, shortBreackTime: 5, longBreakTime: 15 },
};

export const taskModel = createContext({ initialTask });
```
