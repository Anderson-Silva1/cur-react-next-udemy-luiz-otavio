# Entendendo o Full Route Cache no Next.js

O **Full Route Cache** é um mecanismo fundamental do Next.js que otimiza a renderização de rotas, armazenando o resultado completo de páginas renderizadas no servidor. Isso inclui o HTML gerado e o payload dos React Server Components (RSC), permitindo que as páginas sejam servidas rapidamente sem re-renderização desnecessária. Neste material de estudo, exploramos em profundidade esse conceito, seus termos equivalentes, considerações iniciais, e como configurar e alterar seu comportamento. As explicações são expandidas para maior clareza, com exemplos práticos e comparações, mantendo o foco no escopo fornecido.

## Termos Equivalentes ao Full Route Cache

O Full Route Cache é frequentemente referido por outros nomes na documentação e comunidade do Next.js, refletindo diferentes aspectos de sua funcionalidade. Entender esses termos ajuda a conectar conceitos e evitar confusões:

- **Automatic Static Optimization (Otimização Estática Automática)**: Refere-se à capacidade do Next.js de automaticamente otimizar rotas para renderização estática, ativando o cache sem configuração manual. Isso ocorre por padrão em produção, melhorando a performance ao pré-renderizar páginas.

- **Static Site Generation (SSG - Geração de Site Estático)**: Envolve a geração de páginas HTML estáticas durante o build do projeto. Essas páginas são cacheadas e servidas diretamente, ideais para conteúdo que não muda frequentemente. O Full Route Cache é o mecanismo subjacente que armazena esses artefatos gerados.

- **Static Rendering (Renderização Estática)**: Processo de renderizar rotas no servidor de forma estática, seja no build ou na primeira visita. Diferente da renderização dinâmica (SSR), que ocorre a cada requisição, a estática usa o cache para reutilizar o output renderizado.

Esses termos destacam o foco em performance e escalabilidade, mas todos convergem para o Full Route Cache como o cache servidor que persiste o resultado renderizado.

## Considerações Iniciais

Antes de mergulhar na configuração, é essencial compreender os princípios básicos do Full Route Cache. Essas considerações fornecem o contexto para entender como o Next.js gerencia rotas estáticas e dinâmicas, especialmente em diferentes ambientes (desenvolvimento vs. produção).

1. **Modo de Desenvolvimento vs. Produção**:

   - No modo de desenvolvimento (`next dev`), todas as rotas são tratadas como **dinâmicas** por padrão. Isso facilita a depuração e o hot reloading, mas desativa o Full Route Cache para evitar inconsistências durante edições de código. Cada acesso à rota resulta em uma re-renderização completa no servidor.
   - Em produção (`next build` seguido de `next start`), o Next.js prioriza otimizações, ativando o Full Route Cache para rotas que podem ser estáticas. Isso reduz a carga no servidor e acelera as respostas.

2. **Rotas Dinâmicas e Re-renderização**:

   - Uma rota dinâmica é re-renderizada no servidor para cada acesso de usuário. Isso significa que o HTML é gerado dinamicamente com base em dados em tempo real, sem armazenamento no Full Route Cache. Útil para páginas personalizadas (ex.: perfis de usuário), mas menos eficiente para conteúdo estável.

3. **Rotas Estáticas e o Full Route Cache**:

   - Rotas estáticas são renderizadas apenas uma vez (ou periodicamente, se configurado). A primeira visita de um usuário aciona a renderização, e o resultado (HTML + RSC payload) é armazenado no Full Route Cache no servidor.
   - Usuários subsequentes recebem a versão cacheada, garantindo consistência e velocidade. Isso é ideal para páginas como blogs ou landing pages, onde o conteúdo não varia por usuário.

4. **Rotas ISR (Incremental Static Regeneration)**:

   - ISR permite que rotas estáticas sejam re-renderizadas periodicamente, combinando os benefícios da estática (cache) com atualizações controladas.
     - **Revalidação por Tempo**: Defina um intervalo (em segundos) para que a rota expire. Após o prazo, a próxima visita retorna a versão cacheada (stale), enquanto uma re-renderização ocorre em background. A versão atualizada é cacheada para visitas futuras.
     - **Revalidação Sob Demanda**: Use funções como `revalidatePath` ou `revalidateTag` para invalidar o cache manualmente (ex.: após atualização de dados via webhook ou ação do usuário).
   - Importante: A revalidação só ocorre quando um usuário acessa a rota. Se ninguém visitar, o cache antigo persiste. Além disso, o primeiro usuário após expiração vê a versão stale, enquanto o próximo vê a fresca.

5. **Diferença entre Static e SSG**:
   - **Static**: Renderização ocorre na primeira visita em runtime (Lazy Static Rendering). O cache é preenchido dinamicamente.
   - **SSG**: Renderização total durante o build, usando funções como `generateStaticParams`. Todas as páginas são pré-geradas e cacheadas antecipadamente, ideal para sites com caminhos conhecidos.

Essas considerações destacam que o Full Route Cache é otimizado para produção, priorizando eficiência sem sacrificar flexibilidade.

## Observação Importante

Em produção, toda rota no Next.js assume o comportamento do **Full Route Cache** por padrão, a menos que configurada explicitamente para ser dinâmica. Isso significa que, sem intervenções, as páginas são renderizadas estaticamente e cacheadas no servidor, promovendo performance automática. Essa otimização é uma das forças do Next.js, mas requer atenção para cenários onde dados em tempo real são essenciais.

## Como Alterar o Comportamento do Full Route Cache

O Next.js oferece configurações granulares para controlar se uma rota usa o Full Route Cache (estática) ou é dinâmica. Essas opções são definidas no nível da rota (arquivo `page.tsx` ou similar) e permitem transições entre modos. Vamos explorar cada uma em detalhes.

### Usando `export const dynamic`

Essa constante define o modo de renderização da rota, influenciando diretamente o Full Route Cache:

- **Valores Possíveis**:

  - `"auto"` (padrão): O Next.js otimiza automaticamente para estática se possível (sem dados dinâmicos como `cookies` ou `headers`).
  - `"force-dynamic"`: Força renderização dinâmica, desativando o Full Route Cache. Cada acesso re-renderiza a página.
  - `"force-static"`: Força renderização estática, ativando o cache mesmo em rotas com elementos dinâmicos.
  - `"error"`: Gera erro se a rota não puder ser estática, útil para depuração.

- **Impacto**:
  - Transforma estática em dinâmica: Use `"force-dynamic"` para páginas que precisam de dados frescos.
  - Transforma dinâmica em estática: Use `"force-static"` para cachear rotas com parâmetros.

Exemplo:

```tsx
export const dynamic = "force-dynamic"; // Desativa Full Route Cache
```

### Usando `export const revalidate`

Essa constante controla a revalidação de rotas estáticas, transformando-as em dinâmicas ou ISR:

- **Funcionamento**: Recebe um número de segundos para o intervalo de revalidação. Valor `0` desativa o cache completamente (torna dinâmica).
- **Combinação com `dynamic`**:
  - Sozinha: Transforma estática em dinâmica (sem cache).
  - Com `dynamic = "force-static"`: Cria ISR, revalidando periodicamente enquanto mantém o cache.

Exemplo de ISR:

```tsx
export const dynamic = "force-static";
export const revalidate = 3600; // Revalida a cada 1 hora
```

- **Detalhes da Revalidação**: Após o intervalo, a próxima visita retorna o cache stale e aciona background re-render. Isso garante disponibilidade enquanto atualiza.

### Usando `params` e Rotas Dinâmicas

- **Efeito Básico**: Parâmetros dinâmicos (ex.: `[id]`) tornam a rota dinâmica por padrão, desativando o Full Route Cache para permitir personalização por URL.
- **Combinando com `dynamic = "force-static"`**: Força estática, cacheando a renderização na primeira visita. Todas as variantes (ex.: `/post/1`, `/post/2`) são cacheadas individualmente.

- **Função `generateStaticParams()`**: Gera caminhos estáticos durante o build (SSG). Retorna um array de objetos com valores para os params.

  - Exemplo: Gera SSG para IDs específicos, cacheando-os no Full Route Cache.

- **`export const dynamicParams`**:
  - `true` (padrão): Permite caminhos dinâmicos não pré-gerados (renderizados na primeira visita).
  - `false`: Limita a caminhos de `generateStaticParams()`. Caminhos desconhecidos retornam 404.

Exemplo Completo:

```tsx
import { formatHours } from "@/utils/formatHours";

type IdCacheTesteParams = {
  params: Promise<{ id: string }>;
};

export const dynamicParams = false;

export const generateStaticParams = () => {
  return [{ id: "1" }, { id: "2" }, { id: "3" }];
};

const IdCacheTesteParams = async ({ params }: IdCacheTesteParams) => {
  const hora = Date.now();
  console.log(await params);
  return (
    <>
      <h1 className="text-5xl font-bold">{formatHours(hora)}</h1>
    </>
  );
};

export default IdCacheTesteParams;
```

- **Saída do Build**:

  - Gera SSG para `/cache-teste/1`, `/2`, `/3`. Outros IDs resultam em 404.
  - Terminal mostra rotas pré-renderizadas (● para SSG).

- **Explicação Detalhada**: Com `dynamicParams = false`, o Next.js garante SSG apenas para caminhos conhecidos, otimizando o cache para cenários previsíveis. Isso evita renderizações desnecessárias em runtime, mas requer manutenção da lista de params.

## Benefícios e Quando Usar

- **Vantagens**: Reduz latência, escalabilidade para alto tráfego, SEO melhorado com HTML estático.
- **Casos de Uso**: Páginas estáticas para conteúdo fixo; ISR para dados semi-dinâmicos; Dinâmicas para personalização em tempo real.
- **Limitações**: Não adequado para dados voláteis; revalidação depende de acessos.

Este guia expandido fornece uma base sólida para dominar o Full Route Cache, com foco em explicações detalhadas e práticas.
