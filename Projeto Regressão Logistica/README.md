<p align="center">
  <img src="../Imagens/brasao-pucminas-versao-2025.png" alt="Logo do Projeto" width="100">
</p>

# Previsão de Pagamento de Dividendos (Dividend Yield > 6%) usando Regressão Logística
### Trabalho Final: Modelos de Regressão

**Autor:** Gabriel Fratucci dos Reis

**Ano:** 2026  
**Tecnologias:** Python, Pandas, Scikit-learn, Statsmodels

### 1. Seleção da Base de Dados

A base de dados foi construída a partir da consolidação de duas fontes principais:

Dados Contábeis e Financeiros: Extraídos dos demonstrativos padronizados da CVM (Comissão de Valores Mobiliários), contendo métricas como Lucro Por Ação (LPA), Fluxo de Caixa Livre (FCL) e Payout Contábil.

Dados de Mercado: Histórico de cotações extraído via biblioteca yfinance para capturar o preço de fechamento ajustado do último pregão de cada ano.
A união foi feita utilizando o CNPJ e o código CVM como chaves primárias.

### 2. Contextualização do Problema

O mercado financeiro possui alta volatilidade e a escolha de ativos pagadores de dividendos é uma estratégia consolidada. O problema consiste em prever se uma empresa listada na B3 será classificada como uma "Boa Pagadora" no ano seguinte.

- Variável Resposta (Target): Y_Bom_Pagador (Binária).
- Critério: Recebe 1 se o Dividend Yield (Dividendos Pagos / Valor de Mercado) for $\ge$ 0.06 (6%), e 0 caso contrário.

### 3. Processamento de Dados

O pipeline de tratamento envolveu as seguintes etapas:Higienização de Chaves: 

- Padronização de CNPJs (preenchimento com zeros à esquerda até 14 dígitos) para cruzamento exato (Left Join) entre as bases da CVM e da B3.
- Criação da Variável Alvo: Cálculo do Dividend Yield utilizando os dividendos pagos em $T+1$ divididos pelo valor de mercado em $T$.
- Tratamento de Outliers (Winsorização): O mercado financeiro possui valores extremos que distorcem modelos logísticos. Foi aplicada a técnica de winsorização, limitando os dados aos percentis 5% e 95% para as variáveis independentes.
- Tratamento de Nulos: Exclusão de linhas sem dados financeiros suficientes para o cálculo das variáveis independentes (LPA, FCL, Payout_Contabil).
