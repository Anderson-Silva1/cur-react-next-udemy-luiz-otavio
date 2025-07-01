# **useRef**

O useRef é um dos hooks fundamentais do React que, à primeira vista, pode parecer simples, mas que oferece uma gama poderosa de funcionalidades para os desenvolvedores.

Essencialmente, ele permite a criação de referências a elementos do DOM e a manutenção de valores mutáveis que não disparam uma nova renderização do componente. Compreender seus dois principais casos de uso é crucial para escrever componentes mais eficientes e performáticos.

## Criando um `useRef`

```tsx
const taskNameRef = useRef(null);
```

Agora tudo o que adicionarmos dentro da variável `taskNameRef` será salva dentro da mesma

## current

É o retorno do que está dentro do Ref

```tsx
taskNameRef.current.value;
```

Dessa forma dará um erro, pois o valor poderá ser null, paracorrigir isso podemos cololcar um operador `?` para tornar o value opcional, ou seja, pode ter ou não

```tsx
taskNameRef.current?.value;
```

Com isso,precisamos fazer uma última coisa, passar nosso `ref` para nosso input pela propriedade especial do `useRaf` chamada: `ref`

```tsx
<Input type="text" id="taskName" placeholder="Digite Algo" ref={taskNameRef} />
```

Com isso, tudo o que for digitado dentro desse `input` será atribuído para dentro da variável `taskNameRef`

---

Iremos perceber um erro no nosso input em questão do atributo `ref` pois ele não existe, para resolver isso vamos usar um carinha chamado `forwardRef`, onde ele vai permitir nosso componente de input receber o atributo `ref`

```tsx
export const Input = forwardRef<HTMLInputElement, InputProps>((props, ref) => {
  return <input className={styles.input} {...props} ref={ref} />;
});
```

### VAMOS USAR ASSIM DENTRO DO NOSSO CÓDGO ATÉ O MOMENTO
