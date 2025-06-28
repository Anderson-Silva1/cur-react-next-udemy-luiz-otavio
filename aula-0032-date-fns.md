# **Date-fns**

É uma lib que usamos para formatar dia e hora

```bash
>> npm install date-fns
```

## Data atual

Para fazer isso instanciamos a data do momento com o classe `Date`

```tsx
const dataAtual = new Date();
```

`dataAtual` agora terá um valor numérico: "`2025-06-27T23:26:48.045Z`"

Depois usamos o método `format` do `date-fns`, onde ele recebe o valor para servir como base para a formatação e uma `string`, essa String tem letras chaves:

### Hora

| Token      | Significado                               | Exemplo          | Observações      |
| ---------- | ----------------------------------------- | ---------------- | ---------------- |
| `H`        | Hora (0–23) sem zero à esquerda           | `0`, `9`, `23`   | 24h              |
| `HH`       | Hora (0–23) com zero à esquerda           | `00`, `09`, `23` | 24h              |
| `h`        | Hora (1–12) sem zero                      | `1`, `9`, `12`   | 12h              |
| `hh`       | Hora (1–12) com zero                      | `01`, `09`, `12` | 12h              |
| `a` / `aa` | AM/PM                                     | `AM`, `PM`       | `aa` = abreviado |
| `m`        | Minutos (0–59)                            | `5`, `45`        | Sem zero         |
| `mm`       | Minutos com zero                          | `05`, `45`       |                  |
| `s`        | Segundos (0–59)                           | `7`, `59`        | Sem zero         |
| `ss`       | Segundos com zero                         | `07`, `59`       |                  |
| `S`        | Milissegundos (dígito mais significativo) | `1`              | de 0–9           |
| `SS`       | Milissegundos (dois dígitos)              | `01`, `99`       | de 0–99          |
| `SSS`      | Milissegundos (completo)                  | `001`, `999`     | de 0–999         |

---

### Dia

| Token   | Significado                 | Exemplo                    | Observações           |
| ------- | --------------------------- | -------------------------- | --------------------- |
| `d`     | Dia do mês                  | `1`, `31`                  | Sem zero              |
| `dd`    | Dia com zero                | `01`, `31`                 |                       |
| `do`    | Dia ordinal                 | `1º`, `2º`, `31º`          | Depende da `locale`   |
| `D`     | Dia do ano                  | `1` a `365`                |                       |
| `DD`    | Dia do ano com zero         | `001`, `365`               |                       |
| `E`     | Dia da semana numérico      | `1` a `7`                  | Domingo = 7 em `ptBR` |
| `e`     | Dia da semana local (1 a 7) | `2`, `6`                   | Depende do locale     |
| `eee`   | Abreviação do dia           | `seg`, `dom`               | locale                |
| `eeee`  | Nome completo do dia        | `segunda-feira`, `domingo` | locale                |
| `eeeee` | Letra inicial do dia        | `S`, `T`, `Q`              | locale                |

---

### Mês

| Token   | Significado          | Exemplo               | Observações |
| ------- | -------------------- | --------------------- | ----------- |
| `M`     | Mês (1–12)           | `1`, `12`             | Sem zero    |
| `MM`    | Mês com zero         | `01`, `12`            |             |
| `MMM`   | Nome abreviado       | `jan`, `dez`          | locale      |
| `MMMM`  | Nome completo        | `janeiro`, `dezembro` | locale      |
| `MMMMM` | Letra inicial do mês | `j`, `d`              | locale      |

---

### Semana

| Token | Significado          | Exemplo    | Observações        |
| ----- | -------------------- | ---------- | ------------------ |
| `w`   | Semana do ano (1–53) | `3`, `45`  |                    |
| `ww`  | Semana com padding   | `03`, `45` |                    |
| `W`   | Semana do mês        | `1`, `5`   | Raro, mas útil     |
| `k`   | Hora do dia (1–24)   | `1`, `24`  | Alternativa ao `H` |

---

### Trimestre

| Token | Significado       | Exemplo    |
| ----- | ----------------- | ---------- |
| `Q`   | Trimestre         | `1`, `4`   |
| `Qo`  | Trimestre ordinal | `1º`, `4º` |

---

### Ano

| Token  | Significado          | Exemplo | Observações                                     |
| ------ | -------------------- | ------- | ----------------------------------------------- |
| `yy`   | Ano com dois dígitos | `25`    | 2025 → `25`                                     |
| `yyyy` | Ano completo         | `2025`  | Recomendado                                     |
| `y`    | Ano sem padding      | `2025`  | igual ao `yyyy`, mas sem forçar zero à esquerda |

---

### Era

| Token  | Significado   | Exemplo                               |
| ------ | ------------- | ------------------------------------- |
| `G`    | Era abreviada | `AC`, `DC`                            |
| `GGGG` | Era completa  | `Antes de Cristo`, `Depois de Cristo` |

---

```ts
const dataFormatada = format(dataAtual, "dd/MM/yyyy"); // 31/12/2025
const dataFormatada = format(dataAtual, "dd/MM/yyyy HH:mm GGGG"); // 27/06/2025 21:39 Anno Domini
```

Se pararmos para ver está em inglês... Para mudarmos para portugês podemos usar um terceiro parâmetro, que será um objeto com a chave `locale` e essa chaverecebe o valor ``ptBR` que vem de dentro do date-fns

```ts
import { ptBR } from "date-fns/locale";

const dataFormatada = format(dataAtual, "dd/MM/yyyy HH:mm GGGG", {
  locale: ptBR,
});
```

## Método `add` e `sub`

Podemos, baseado em uma data, adicionar ou redizir:

- Segundos
- Horas
- Dias
- Meses
- Anos
- E outros...

```ts
const dataAtual = new Date();

const dataAdd7Dias = addDays(dataAtual, 7);
const dataAdd7Horas = addHours(dataAtual, 7);
const dataAdd30Segundos = addSeconds(dataAtual, 30);
const dataAdd7Anos = addYears(dataAtual, 7);

const dataSub7Dias = subDays(dataAtual, 7);
const dataSub7Horas = subHours(dataAtual, 7);
const dataSub30Segundos = subSeconds(dataAtual, 30);
const dataSub7Anos = subYears(dataAtual, 7);
```

## Passando valoeres para `Date`

Podemos passar o ANO, MÊS (index, ou seja, Janeiro é 0), DIA

```ts
const data = new Date(2025, 9, 20);

const formate = format(dataFinal, "dd/MMMM/yyyy", {
  locale: ptBR,
});

console.log(formate); // 26/outubro/2025
```

## Diferença entre as datas `differenceIn`

Podemos ter a diferença em dias, meses, anos... entre duas datas...

Essa função (`differenceIn`) recebe dois valores, e primeiro será nossa data a ser diferenciada e o segundo nossa data atual

```ts
import {
  differenceInDays,
  differenceInHours,
  differenceInMonths,
} from "date-fns";

const dataAtual = new Date();
const dataFinal = new Date(2025, 11, 26);

const diferenca = differenceInDays(dataFinal, dataAtual);
const diferencaH = differenceInHours(dataFinal, dataAtual);
const diferencaM = differenceInMonths(dataFinal, dataAtual);

console.log(diferenca); // 181
console.log(diferencaH); // 4345
console.log(diferencaM); // 5
```

## Verificar se está entre duas datas com o `isWithinInterval`

Vamos ter nossa data, e uma variável que recebe um objeto, esse objeto tem duas chaves:

- start
- end

E ambas terão datas, start será a menor data, e end será a maior data

Caso nossa dataValidada não esteja dentro dessa margem, retornará `false`, caso contrário `true`

```ts
import { isWithinInterval } from "date-fns";

const dataValidada = new Date();
const intervalo = {
  start: new Date(2025, 8, 1),
  end: new Date(2025, 11, 1),
};

const diferenca = isWithinInterval(dataValidada, intervalo);

console.log(diferenca); // false
```

## Verifica se datas são equivalentes (`isSame `)

Verifica se datas estão no mesmo mês

```ts
import { isSameDay, isSameMonth, isSameYear } from "date-fns";

const data1 = new Date(2025, 11, 1);
const data2 = new Date(2025, 1, 1);

const mesmoMes = isSameMonth(data1, data2);
const mesmoDia = isSameDay(data1, data2);
const mesmoAno = isSameYear(data1, data2);

console.log(mesmoMes, mesmoDia, mesmoAno); // false false true
```
