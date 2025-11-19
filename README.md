# 📊 Análise de Regressão Linear: Gastos com Alimentação

> Projeto prático da Unidade 4: Estatística Aplicada aos Negócios.

Este repositório documenta a aplicação de **Regressão Linear Simples** para analisar o comportamento de consumo familiar. O estudo combina embasamento teórico sobre o Método dos Mínimos Quadrados e Análise de Resíduos com uma aplicação prática utilizando Python.

---

## 📑 Índice

1. [Visão Geral](#-visão-geral)
2. [Dataset](#-dataset)
3. [Fundamentos Teóricos](#-fundamentos-teóricos)
4. [Análise Prática e Resultados](#-análise-prática-e-resultados)
5. [Questionário de Validação](#-questionário-de-validação)
6. [Como Executar](#-como-executar)

---

## 📝 Visão Geral

O objetivo central deste projeto é responder à pergunta de negócios:
**"A Renda Familiar Anual é um bom preditor para os Gastos Anuais com Alimentação?"**

Utilizamos a biblioteca `statsmodels` e `pandas` para criar um modelo preditivo, validar suas premissas estatísticas e comparar sua eficácia contra outras variáveis (como o endividamento familiar).

---

## 📂 Dataset

Os dados (`Consumer_Food.csv`) consistem em 200 observações simuladas baseadas em normas nacionais dos EUA.

| Variável | Tipo | Descrição |
| :--- | :--- | :--- |
| `Annual Food Spending ($)` | **Dependente (y)** | Valor gasto anualmente em alimentação. |
| `Annual Household Income ($)` | **Independente (x)** | Renda anual total da família. |
| `Non mortgage household debt ($)` | Numérica | Dívidas familiares (exceto hipoteca). |
| `Region` | Categórica | 1: Nordeste, 2: Centro-Oeste, 3: Sul, 4: Oeste. |
| `Location` | Categórica | 1: Metropolitana, 2: Fora de área metropolitana. |

---

## 📚 Fundamentos Teóricos

A análise baseia-se nos conceitos abordados no material didático:

*   **Modelo de Regressão:** A busca por uma equação linear $\hat{y} = b_0 + b_1x$ que minimize a soma dos erros ao quadrado (SSE).
*   **Coeficiente de Determinação ($R^2$):** Mede a % da variância de $y$ explicada por $x$.
    *   Fórmula: $R^2 = 1 - (SSE / SS_{yy})$.
*   **Análise de Resíduos:** Estudo da diferença entre o valor real e o previsto ($y - \hat{y}$). Para o modelo ser válido, os resíduos devem ter distribuição normal e variância constante.
*   **Correlação vs. Causalidade:** A regressão prova associação linear, não necessariamente causa e efeito.

---

## 📈 Análise Prática e Resultados

Através da execução do código Python, obtivemos os seguintes insights:

### 1. Qualidade do Ajuste (Renda vs. Alimentação)
*   **R-Quadrado ($R^2$):** `0.7385`
*   **Interpretação:** Aproximadamente **73.85%** da variação nos gastos com comida é explicada pela renda familiar. É um ajuste considerado forte para dados socioeconômicos.

### 2. Análise de Resíduos
*   **Normalidade:** O histograma e o gráfico Q-Q Plot demonstraram que os resíduos seguem uma distribuição **aproximadamente normal**, validando os testes de significância (Teste t).

### 3. Comparação de Modelos (Renda vs. Dívida)
*   Ao tentar prever a *Dívida Familiar* usando a *Renda*, o modelo falhou drasticamente.
*   **R-Quadrado (Dívida):** `~0.02` (apenas 2%).
*   **Conclusão:** A renda é um excelente preditor para consumo de alimentos, mas um péssimo preditor linear para o nível de endividamento.

---

## ✅ Questionário de Validação

Resumo das afirmativas analisadas durante o estudo:

| Afirmativa | Status | Justificativa |
| :--- | :---: | :--- |
| A variável a ser prevista é chamada de dependente ($y$). | **Verdadeiro** | Definição padrão de terminologia. |
| A regressão simples captura relações não lineares. | **Falso** | Regressão simples foca apenas em relações lineares. |
| O gráfico de dispersão dá uma ideia inicial do ajuste. | **Verdadeiro** | Permite visualizar a tendência linear antes do cálculo. |
| O resíduo é a diferença entre o valor real e a média. | **Falso** | Resíduo é a diferença entre o valor real e o **previsto** ($y - \hat{y}$). |
| O coeficiente de correlação é o quadrado do $R^2$. | **Falso** | É o inverso: $R^2$ é o quadrado da correlação ($r$). |
| $R^2$ é a % da variabilidade explicada pelo modelo. | **Verdadeiro** | Interpretação correta do coeficiente de determinação. |
| Regressão é prova de causa e efeito. | **Falso** | Regressão indica associação estatística, não causalidade. |
| O modelo explicou 73.85% da variabilidade. | **Verdadeiro** | Resultado obtido via `statsmodels`. |
| A distribuição dos resíduos é aproximadamente normal. | **Verdadeiro** | Verificado via Histograma e Q-Q Plot. |
| O modelo de dívida é tão bom quanto o de alimentação. | **Falso** | O modelo de dívida teve desempenho muito inferior. |

---

## 🚀 Como Executar

Para reproduzir as análises, você precisará de Python instalado com as bibliotecas `pandas`, `matplotlib`, `seaborn` e `statsmodels`.

```python
import pandas as pd
import statsmodels.api as sm
import matplotlib.pyplot as plt
import seaborn as sns
import io

# 1. Carregar Dados (Cole o CSV aqui ou carregue o arquivo)
# df = pd.read_csv('Consumer_Food.csv')

# 2. Definir Variáveis
X = sm.add_constant(df['Annual Household Income ($)'])
y = df['Annual Food Spending ($)']

# 3. Criar Modelo
model = sm.OLS(y, X).fit()

# 4. Exibir Estatísticas
print(model.summary())

# 5. Gráficos de Resíduos
fig, ax = plt.subplots(1, 2, figsize=(12, 5))
sns.histplot(model.resid, kde=True, ax=ax[0])
sm.qqplot(model.resid, line='45', fit=True, ax=ax[1])
plt.show()