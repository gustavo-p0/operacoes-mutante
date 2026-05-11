# Laboratório: Teste de Mutação com a "Calculadora Mutante"

Bem-vindo ao trabalho prático de Teste de Mutação. O objetivo deste projeto é usar a ferramenta StrykerJS para avaliar e fortalecer uma suíte de testes que, à primeira vista, parece boa.

## Contexto

Este repositório contém uma biblioteca de cálculos simples. A suíte de testes inicial em `__tests__/` foi projetada para ter uma alta **cobertura de código**, mas esconde fraquezas que só o **teste de mutação** pode revelar.

Sua missão é atuar como um engenheiro de qualidade para encontrar essas fraquezas e escrever testes mais robustos para corrigi-las.

---

## Entendendo o Teste de Mutação

Para ter sucesso neste laboratório, é crucial entender como a ferramenta funciona nos bastidores e o que significa ter um teste "robusto".

### O Processo
1. **O Stryker copia o código e aplica o mutante:** Ele isola o código em um "sandbox" e altera intencionalmente a lógica original.
   ```javascript
   // Exemplo de mutante inserido no Sandbox do Stryker
   // (trocando um operador de igualdade)
   return n % 2 !== 0; 

2. **O Stryker roda os testes:** A sua suíte de testes atual é executada contra esse código recém-alterado (mutante).
3. **Resultado da execução:**
* O teste `expect(isPar(100)).toBe(true)` é executado.
* Como o código foi quebrado de propósito, a função retorna `false`, mas o seu teste espera `true`.
* O teste **falha**.


4. **Veredito:** O Stryker marca o mutante como **morto (Killed)**.

### A Analogia: Predador e Presa

Pense no teste como o **"predador"** e no mutante como a **"presa"**.

* Se o mutante passa despercebido (ou seja, o teste continua passando mesmo com o código quebrado), ele **sobrevive**.
* Se o teste falha, ele detecta o defeito e o mutante **morre**.

A "morte" do mutante é, na verdade, um sinal de sucesso: a falha do teste revelou que o código mudou, e o teste fez o seu trabalho ao detectar essa mudança.

### O que significa ter um Teste "Robusto"?

Se um teste "mata" o mutante, isso quer dizer que ele é robusto! A robustez de um teste é exatamente a sua **capacidade de detectar falhas e mudanças** no comportamento do código.

* **Teste Fraco:** O teste passa mesmo no código mutante. Ele apenas executa o código, mas não verifica o comportamento corretamente (não viu a mudança).
* **Teste Robusto:** O teste falha no código mutante. Ele é específico e estrito quanto ao comportamento esperado.

O teste `expect(isPar(100)).toBe(true)` é considerado robusto porque ele **especifica exatamente o que espera**, e não apenas "roda" a função. Se o mutante alterar um `===` para `!==`, o teste irá falhar e detectar a anomalia imediatamente.

---

## Tarefas

1. **Prepare o ambiente:** Clone o repositório e rode `npm install`.
2. **Análise Inicial:**
* Rode `npm test` para ver os testes passando.
* Rode `npm run coverage` e anote o percentual de cobertura.


3. **Análise de Mutação:**
* Configure o StrykerJS (`npx stryker init`).
* Execute a análise com `npx stryker run` (ou `npm run mutate` após configurar o script).
* Abra o relatório HTML e analise os **mutantes que sobreviveram**.


4. **Aprimore os Testes:**
* Para cada mutante sobrevivente, entenda por que ele não foi "morto" pela sua suíte de testes atual.
* Adicione novos testes (ou melhore os existentes) em `__tests__/operacoes.test.js`, focando em asserções mais específicas e robustas para matar esses mutantes.


5. **Validação Final:**
* Rode o Stryker novamente e verifique se a sua pontuação de mutação aumentou, idealmente alcançando mais de **95%**.

---
# 95%?
A exigência de **95%** (e não 100% ou um número menor) não é por acaso! No universo de testes de software, essa é uma meta estratégica por alguns motivos bem específicos:

#### 1. Por que não 100%? (O problema dos "Mutantes Equivalentes")

Buscar 100% de pontuação em testes de mutação costuma ser perda de tempo devido ao que chamamos de **Mutantes Equivalentes**.

Às vezes, o Stryker altera o código de um jeito que a lógica muda, mas o *resultado final* do comportamento continua exatamente o mesmo.

* *Exemplo:* O código original verifica `if (x < 10)` considerando apenas números inteiros. O Stryker muda para `if (x <= 9)`.
* O código foi "mutado", mas matematicamente dá no mesmo. Nenhum teste vai falhar. O mutante "sobrevive", derruba a sua nota para menos de 100%, mas **não há nenhum defeito real**.

Por isso, o mercado aceita que algo entre 90% e 95% já representa um código extremamente blindado.

#### 2. A diferença entre "Cobertura de Código" e "Teste de Mutação"

NOte que nesse laboratório que é fácil conseguir **100% de Cobertura de Código** (Code Coverage). Isso só significa que o teste passou por todas as linhas.

Porém, ter 100% de cobertura de código costuma render apenas uns **60% a 70% de score de mutação**. Buscar os 95% no Stryker te obriga a sair da zona de conforto e escrever testes que não apenas "rodam o código", mas que realmente verificam cada detalhe lógico (como números negativos, zeros, limites, etc.).
