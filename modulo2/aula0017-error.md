# Error.tsx | Error Boundaries no Next.js

O arquivo `error.tsx` é um recurso especial do Next.js utilizado para capturar erros não tratados ou exceções que ocorrem em componentes dentro de uma árvore de rotas específica. Ele implementa **Error Boundaries**, permitindo o tratamento gracioso de erros em aplicações React, garantindo uma melhor experiência do usuário ao exibir mensagens de erro personalizadas.

## Escopo de Atuação

Quando o arquivo `error.tsx` está localizado em uma rota, como `/post/teste`, ele captura erros que ocorrem apenas nessa rota ou em rotas aninhadas abaixo dela (rotas filhas). Erros em rotas acima ou em outras partes da aplicação não serão interceptados por este arquivo, respeitando a hierarquia de roteamento do Next.js.

## Parâmetros do Componente

O componente `error.tsx` recebe duas propriedades (props) que podem ser usadas para gerenciar o erro:

- `error`: Um objeto do tipo `Error` que contém informações sobre a exceção ou erro ocorrido.
- `reset`: Uma função que, quando chamada, tenta re-renderizar a rota para recuperar o estado anterior, permitindo uma tentativa de recuperação do erro.

## Exemplo de Implementação

Segue um exemplo de código para o arquivo `error.tsx`:

```tsx
"use client";

import { ErrorMessage } from "@/components/ErrorMessage";

interface ErrorPageProps {
  error: Error & { digest?: string };
  reset: () => void;
}

export default function ErrorPage({ error, reset }: ErrorPageProps) {
  // Log do erro para análise (pode ser enviado a um serviço de monitoramento)
  console.error("Erro capturado:", error);

  return (
    <div className="error-container">
      <ErrorMessage
        title="Algo inesperado aconteceu!"
        contentTitle="ERRO"
        content={
          error.message || "Ocorreu um erro inesperado. Tente novamente."
        }
        typeError
        funcReset={() => reset()}
      />
    </div>
  );
}
```

### Explicação do Código

- **`"use client"`**: Indica que o componente é um Client Component, necessário para interatividade no lado do cliente, como a função `reset`.
- **`interface ErrorPageProps`**: Define o tipo das props, incluindo o campo opcional `digest` no objeto `error`, que pode ser usado pelo Next.js para rastrear erros específicos.
- **`console.error`**: Registra o erro no console para depuração. Em produção, é comum enviar esses logs para ferramentas de monitoramento como Sentry ou LogRocket.
- **`ErrorMessage`**: Um componente personalizado que exibe a mensagem de erro de forma amigável, com um título, descrição e uma opção de tentar novamente via `reset`.

## Boas Práticas

1. **Monitoramento de Erros**: Os erros capturados devem ser enviados para um serviço de monitoramento (ex.: Sentry, New Relic) para análise por equipes técnicas. Isso ajuda a identificar e corrigir problemas rapidamente.
2. **Feedback Visual**: Forneça uma interface clara e amigável ao usuário, explicando o erro e oferecendo ações como "Tentar Novamente" ou links para outras partes do site.
3. **Uso do `reset`**: A função `reset` deve ser usada com cuidado, garantindo que a re-renderização não cause loops infinitos ou degrade a experiência do usuário.
4. **Estilização**: Utilize CSS (como no exemplo com `className="error-container"`) para estilizar a página de erro, mantendo consistência com o design da aplicação.
5. **Testes**: Simule erros em ambientes de desenvolvimento para garantir que o `error.tsx` funcione corretamente e que as mensagens sejam claras.

## Considerações Adicionais

- O arquivo `error.tsx` deve estar no mesmo diretório da rota que deseja proteger. Por exemplo, para a rota `/post/teste`, coloque o arquivo em `/app/post/teste/error.tsx`.
- Ele funciona apenas no lado do cliente, já que depende do React para gerenciar o estado do erro.
- Para erros no lado do servidor (como em `getServerSideProps` ou APIs), considere usar outros mecanismos, como o tratamento de erros em `API Routes` ou middlewares.
- Em aplicações grandes, centralize os componentes de erro reutilizáveis (como `ErrorMessage`) para manter a consistência visual e funcional.
