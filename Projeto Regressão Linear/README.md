<p align="center">
  <img src="../Imagens/brasao-pucminas-versao-2025.png" alt="Logo do Projeto" width="100">
</p>

# Análise Preditiva de Preços de Imóveis (King County, EUA)
### Trabalho Final: Técnicas de Amostragem e Regressão Linear


**Autor:** Gabriel Fratucci dos Reis  
**Ano:** 2025  
**Tecnologias:** Python, Pandas, Scikit-learn, Statsmodels

---

## 📋 Sobre o Projeto

Este projeto aplica técnicas de **Regressão Linear Múltipla** para prever preços de imóveis no Condado de King (incluindo Seattle), Washington, EUA. 
O objetivo principal é desenvolver um modelo estatístico capaz de estimar o valor de venda com base em características estruturais e geográficas.

O trabalho abrange a limpeza e o tratamento de dados, amostragem estratificada, a validação de pressupostos e interpretação dos coeficientes do modelo.

---

## 💾 Sobre os Dados

* **Fonte:** [Kaggle - KC House Data](https://www.kaggle.com/)
* **Dimensão:** 21.613 observações e 21 variáveis.
* **Variável Alvo:** Preço do imóvel (`price`).

---

## ⚙️ Metodologia e Processamento

### 1. Limpeza e Tratamento de Dados
Identificação e remoção de inconsistências que poderiam enviesar o modelo:
* **Outliers e Erros:** Remoção de registros ilógicos (ex: casas com 33 quartos mas média de 3; imóveis sem banheiros ou quartos; áreas `sqft_lot` desproporcionais).
* **Engenharia de Atributos:**
    * Criação de `house_age` (Idade do imóvel) para substituir o ano de construção.
    * Flag `was_renovated` para indicar se houveram reformas no imóvel.
* **Transformação Logarítmica:** Aplicada à variável `price` (target) para corrigir assimetria à direita e aproximar a distribuição de uma normal.

<p align="center">
  <img src="../Imagens/Graf_Distribuicao.png" alt="Gráfico da Distribuição de Preços" width="900">
</p>

### 2. Amostragem Estratificada
Considerando o tamanho da base, poderiamos rodar treinar e testar o modelo sobre todo o conjunto de dados, mas para fins de estudo, vamos coletar uma amostra. 
Para evitar viés de seleção e garantir representatividade, não utilizamos amostragem aleatória simples.
* **Estratégia:** Amostragem baseada na variável `grade` (nota de avaliação da casa).
* **Motivo:** A variável `grade` apresentou a maior correlação com o preço, sendo crucial garantir que todas as categorias de "notas" estivessem representadas proporcionalmente no treino/teste.

### 3. Seleção de Variáveis (Feature Selection)
Após análise da Matriz de Correlação de Pearson, removemos variáveis redundantes ou ruidosas:

| Variável Removida | Motivo |
| :--- | :--- |
| `sqft_above` | Alta correlação (0.88) com `sqft_living`. |
| `sqft_lot15` | Alta correlação (0.80) com `sqft_lot`. |
| `yr_built` | Redundante com a nova variável `house_age`. |
| `sqft_living15` | Redundante (0.73) com `sqft_living`. |
| `zipcode` | Variável nominal tratada como numérica (gerava ruído). |

<p align="center">
  <img src="../Imagens/Matriz-Correlacao.png" alt="Gráfico de Correlação" width="900">
</p>

---

## 📊 Resultados do Modelo

### Iteração e Ajustes
1.  **Multicolinearidade:** O primeiro treino apresentou $R^2 = 76.3\%$, mas com alta multicolinearidade devido à disparidade de escalas.
2.  **Padronização:** Aplicação do `StandardScaler`. Isso revelou que a variável `grade` possui o maior peso na decisão do modelo.
3.  **Teste de Hipótese:** A variável `bedrooms` mostrou-se estatisticamente irrelevante para este modelo específico, podendo ser removida para otimizar os critérios de informação (AIC/BIC).

<p align="center">
  <img src="../Imagens/Primeiro-Teste.png" alt="Resumo do Modelo" width="600">
</p>

### Métricas Finais

* **R² (R-squared):** `0.76` (O modelo explica 76% da variação dos preços).
* **AIC:** `208.0` (Baixo valor, indicando bom equilíbrio entre simplicidade e erro).
* **Fatores de Influência:**
    * 🟢 `grade` (Coef: 0.2121) e `sqft_living` (Coef: 0.1787) são os maiores influenciadores positivos.
    * 🔴 Quantidade de quartos apresentou P-valor alto (>0.05), indicando irrelevância estatística neste cenário.


<p align="center">
  <img src="../Imagens/Teste-Normalizado.png" alt="Resumo do Modelo" width="600">
</p>

### Análise de Resíduos
O histograma de resíduos aproximou-se de uma normal, indicando que o modelo não está enviesado (viciado), embora tenda a **subestimar** imóveis de valores muito altos (luxo).
<p align="center">
  <img src="../Imagens/Resultados-Finais.png" alt="Gráfico Final" width="600">
</p>

---

## 🚀 Conclusão

O modelo final é funcional e estatisticamente robusto para a maioria dos casos no Condado de King. 
A padronização e a seleção criteriosa de variáveis permitiram um ajuste de 76%, com destaque para a importância da área útil e da nota de avaliação (`grade`) na composição do preço.

---
