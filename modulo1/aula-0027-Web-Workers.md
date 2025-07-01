# Web Workers

Vamos entender e implementar Web Workers em um projeto prático!

---

## O que é um Web Worker?

O Web Worker é um recurso da Web API que permite executar scripts JavaScript em uma _thread_ separada da principal (_main thread_). Isso evita que operações pesadas travem a interface do usuário (UI).

> **Tradução corporativa:** É como delegar uma tarefa demorada para o estagiário enquanto você continua atendendo o cliente. 😉

---

## Como funciona?

- O navegador cria uma nova _thread_ paralela para o Web Worker.
- O script do Worker é executado de forma isolada, sem acesso direto ao DOM (documento HTML).
- A comunicação entre o script principal e o Worker é feita via mensagens:
  - `postMessage` para enviar dados.
  - `onmessage` para receber dados.
- Ideal para tarefas pesadas como:
  - Cálculos matemáticos intensos;
  - Processamento de arquivos grandes;
  - Manipulação de imagens;
  - Lógicas complexas que não envolvam o DOM diretamente.

---

## Estrutura do Projeto

Vamos criar um diretório chamado `workers` dentro da pasta `src` do seu projeto. Nele, criaremos o arquivo `timerWorker.js`.

**timerWorker.js**:

```js
// workers/timerWorker.js

self.onmessage = function (e) {
  const resultado = e.data * 2;
  self.postMessage(resultado);
};
```

### Explicando linha a linha:

```js
self.onmessage = function (e) { ... };
```

> Sempre que o Worker receber uma mensagem da thread principal, essa função será executada.

- `self`: é o objeto global dentro do Worker (equivalente ao `window` no contexto do navegador, mas sem acesso ao DOM).
- `onmessage`: é o _listener_ padrão para escutar mensagens.
- `e`: representa o evento recebido.
- `e.data`: representa os dados enviados pela thread principal.

---

```js
const resultado = e.data * 2;
```

> Multiplica o valor recebido por 2 e armazena na variável `resultado`.

Esse valor processado é o que chamamos de **payload**, ou seja, o conteúdo principal sendo transmitido e processado:

Exemplos de payload:

- Um valor numérico vindo do usuário;
- Um objeto JSON com dados complexos;
- Um array de informações;
- Imagens ou dados binários.

---

```js
self.postMessage(resultado);
```

> Envia o resultado de volta para o script principal.

- `postMessage`: é o método que envia dados da thread secundária (Worker) para a thread principal (navegador).

---

## Como a comunicação acontece

A comunicação é sempre feita via mensagens:

- A **Main Thread** envia dados para a **Worker Thread** usando `worker.postMessage()`.
- A **Worker Thread** recebe esses dados com `self.onmessage`.
- A **Worker Thread** processa e responde usando `self.postMessage()`.
- A **Main Thread** escuta as respostas com `worker.onmessage`.

> É importante lembrar que Workers não têm acesso ao `window`, `document`, nem ao DOM. Por isso usamos `self` como referência global.

---

## Importando corretamente em projetos modernos

Em ferramentas como **Vite**, **Webpack** ou **Babel**, você precisa importar o Worker com `new URL` para que ele seja corretamente interpretado pelo sistema de _build_.

```js
const myWorker = new Worker(new URL("worker.js", import.meta.url));
```

---

## Exemplo completo

### Arquivo da Main Thread (ex: `App.js`):

```js
const worker = new Worker(
  new URL("../workers/timerWorker.js", import.meta.url)
);

worker.postMessage(5); // Enviando o valor 5

worker.onmessage = function (e) {
  console.log("Recebido na Main Thread:", e.data);
  // Saída: Recebido na Main Thread: 10
};
```

### Arquivo do Worker (ex: `timerWorker.js`):

```js
self.onmessage = function (e) {
  console.log("Recebido no Worker:", e.data);

  const resultado = e.data * 2;

  self.postMessage(resultado);
};
```

---

## Conclusão

Web Workers são uma solução robusta para evitar travamentos na UI ao executar tarefas pesadas. Com eles, você melhora a experiência do usuário, dividindo responsabilidades entre threads e garantindo fluidez na interface.

Use com sabedoria e garanta que os Workers sejam bem isolados e comunicativos!

---

> **Dica final:**
> Web Workers são como assistentes silenciosos: trabalham duro nos bastidores e te deixam brilhar na frente do cliente. 😉
