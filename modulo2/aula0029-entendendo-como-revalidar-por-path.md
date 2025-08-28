# Entendendo como Revalidar por Path (caminho)

Podemos revalidar por caminho e por tag

vamos nos aprofundar sobre revalidação por Path

---

## Intodução a **SERVER ACTION**

Saõ opções para configurar o comportamento das Ações do Servidor no seu aplicativo Next.js.

Para usar uma **server action** precisamos usar a diretiva `use server`

Quando criamos funções sem essa diretiva estamos criando componentes do lado servidor, quando usamos server actions estamos criando funções que serão execultadas apenas dentro do servidor... Não ha problemas em usar `server actions` em componentes clientes...

Geralmente criamos uma pasta na raíz ou no SRC do nosso projeto chamada **actions**, e aqui dentro estará todas as nossas actions

## Revalidando pelo Path

Vamos criar uma server actions que vai execultar a função `RevalidatePath()` para revalidar endereço

### Usando a tag html `<form action=''></form>`

Em next podemos usar o form para enviar dados do frontend para o backend...

```tsx

```
