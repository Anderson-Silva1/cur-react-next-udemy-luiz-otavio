# Input controlado

É Quando queremos pegar o valor atual de um input ao ser digitado.

## Value

Os valores do Input são salvos dentro de `value`

```tsx
<Input type="text" id="taskName" placeholder="Digite Algo" value={"Teste"} />
```

## onChange

É um evento JavaScript que tem como finalidade pegar cada caractere ou cada atualização do input a fim de salvá-lo

Dentro dele, teremos nosso `event`, e esse `event` terá o `target` (próprio input), e esse `target` terá o nosso value, que é o valor da tecla que estamos passando para o input

```tsx
<Input
  type="text"
  id="taskName"
  placeholder="Digite Algo"
  value={"teste"}
  onChange={(e) => e.target.value}
/>
```

### event

`event` aqui pode ser tratado como apenas a letra `e` para poupar digitar todo

## Criando um input controlado

Primeiro usamos o `useState` para criar o estado do nosso value

```tsx
const [taskName, setTaskName] = useState("");
```

Depois colocamos o valor de `value`

```tsx
<Input
  type="text"
  id="taskName"
  placeholder="Digite Algo"
  value={taskName}
  onChange={(e) => e.target.value}
/>
```

Depois usamos nosso onChange para atualizar nosso estado

```tsx
<Input
  type="text"
  id="taskName"
  placeholder="Digite Algo"
  value={taskName}
  onChange={(e) => setTaskName(e.target.value)}
/>
```
