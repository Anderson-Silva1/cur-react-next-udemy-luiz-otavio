# Entendendo como revaliddar por tempo rotas **STATIC**

Precisamos desse tipo de rota quando queremos velocidade de carregamento (pois o HTML estará em **cache**, poder das rotas **static**) e atualização do conteúdo (poder das rotas **dynamic**)

Chamamos esstipo de rota de **rota ISR**

Para criar uma ISR é muito simples, basta ter uma página Static e a variável **revalidate** sendo exprtada do mesmo arquivo.

## Exemplo usando **params**

```tsx
// Já é o padrão, porém só para deixar explícito para você
export const dynamicParams = true;

// Temporisa o limite de espera até a página se revalidada ou regerada em segundos, no caso 10 segundos
export const revalidate = 10;

// Gera as páginas dinâmicas estaticamente...
export const generateStaticParams = () => {
  // Não estamos passando nada aqui, mas como o "dynamicParams" está "true", as páginas serão geradas por demanda, caso "dynamicParams" seja "false", todas essas rotas dinâmicas não serão encontradas
  return [];
};

export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  return <>{id}</>;
}
```

## Exemplo usando **export const dynamic = "force-static"**

```tsx
// Transforma a página que seria dynamic por causa dos params em static
export const dynamic = "force-static";

// Temporisa o limite de espera até a página se revalidada ou regerada em segundos, no caso 10 segundos
export const revalidate = 10;

export default async function Page() {
  return <>Hello world!!</>;
}
```

---

> ### Pontos importantes
>
> Não podemos fazer cálculos dentro do revalidate
