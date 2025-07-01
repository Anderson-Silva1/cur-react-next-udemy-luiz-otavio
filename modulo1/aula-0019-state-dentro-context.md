# **Mover nosso `estado` para dentro do `context`**

Primeiramente para facilitar nosso trabalho, vamos criar um componente chamado `TaskContextProvider` para trabalhar com nossos estados

```tsx
const TaskContextProvider = ({ children }: TaskContextProviderProps) => {
  const [state, setState] = useState(initialState);

  return (
    <TaskContext.Provider value={{ state, setState }}>
      {children}
    </TaskContext.Provider>
  );
};
```

Esse componente terá o `useState` que iremos
