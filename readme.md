# Estacionamento Rotativo

- Alunos: @[CaioMicael](https://github.com/CaioMicael) e @[FelipeMacedo](https://github.com/FelipeMacedoK)
- Tema: 14 - Estacionamento / Rotativo

## Descrição
Aplicação em JavaScript que calcula a tarifa de estacionamento rotativo com:
- entradas/saídas por timestamps;
- arredondamento por blocos (15 minutos);
- faixas de tarifa, teto e adicional noturno;
- desconto para PNE (25%) e tratamento de mensalista (valor fixo).

O objetivo do projeto é aplicar conceitos de programação funcional: funções puras, imutabilidade e uso de funções de ordem superior.

## Como usar
1. Os campos "Entrada" e "Saída" vêm pré-preenchidos com o horário atual ao abrir a página.
2. Marque `PNE` para aplicar desconto de 25%.
3. Marque `Mensalista` se for caso; o valor mensal será aplicado (campo `Mensalista - Valor mensal`).
4. Clique em `Calcular`.
5. Veja o resumo, o detalhamento por faixa e a ocupação média exibidos abaixo do formulário.

## Regras de cálculo (implementadas)
- Arredondamento para cima em blocos de 15 minutos.
- Faixas:
  - Até 30 min — R$ 5,00
  - Até 1 h — R$ 8,00
  - 1 h até 3 h — R$ 10,00 / hora (horas arredondadas para cima)
  - 3 h até 6 h — R$ 8,00 / hora (horas arredondadas para cima)
  - A partir de 6 h (com limite de 12h) — R$ 60,00 (fixo)
- Adicional noturno: R$ 20,00 se o intervalo tocar 22:00–06:00.
- PNE: desconto de 25% sobre o total calculado.
- Mensalista: aplica valor fixo mensal (pode receber desconto PNE se marcado).

## Exemplos de entrada e saída

1) Curta permanência (20 minutos)
- Entrada: 23/10/2025 10:00
- Saída: 23/10/2025 10:20
- Tempo arredondado: 30 min
- Resultado:
  - Total por veículo: R$ 5,00


2) Permanência 1h20
- Entrada: 23/10/2025 09:00
- Saída: 23/10/2025 10:20
- Tempo arredondado: 90 min -> 2 horas
- Resultado:
  - Total por veículo: R$ 20,00

3) Período noturno
- Entrada: 23/10/2025 21:30
- Saída: 23/10/2025 23:15
- Tempo arredondado: 105 min -> 2 horas (R$ 20,00) + adicional noturno R$ 20,00
- Resultado:
 - Total por veículo: R$ 40,00

4) Mensalista com desconto PNE
- Marcar `Mensalista` e `PNE`
- Resultado:
  - Mensalista: R$ 375,00 (R$ 500 - 25%)

## Como a programação funcional foi aplicada
- Funções puras:
  - `parseDate`, `minutesBetween`, `roundToBlock`, `validateInputs`, `overlapsNight`, `calcFeePure` são puras (não modificam estado externo).
- Imutabilidade:
  - Objetos retornados por validação e cálculo são congelados.
  - Internamente o breakdown é retornado como cópias também congeladas.
- Funções de ordem superior:
  - Uso de `filter` em validação para construir lista de erros.
  - Uso de `map` para formatar breakdown antes de renderizar.
- Separação de responsabilidades:
  - Lógica de negócio e validação separadas da manipulação do DOM, que apenas consome os resultados.
- Reutilização:
  - Funções pequenas e reutilizáveis (helpers, validadores, cálculo).

## Invariantes e validações
- O cálculo garante Total ≥ 0.
- Validações puras garantem que:
  - Entrada e saída sejam válidos.
  - Saída seja posterior à entrada.
  - Valor mensal seja número não-negativo.
