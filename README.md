# 🚢 Desafio Titanic (Kaggle) - Documentação do Projeto

Este repositório contém o pipeline completo de Ciência de Dados para o desafio clássico do **Titanic** no Kaggle. O objetivo principal do projeto é prever a sobrevivência dos passageiros a partir de seus dados demográficos e de viagem.

---

## 📋 Estrutura do Pipeline de Código

### 1. ⚙️ Configuração e Carregamento de Dados
* **Acesso ao Drive:** O ambiente foi configurado integrando o Google Drive para leitura direta das bases `train.csv` e `test.csv`.
* **Bibliotecas Principais:** Utilização de `pandas` para manipulação, `numpy` para cálculos matemáticos, além de `matplotlib` e `seaborn` para as visualizações de dados.

### 2. 🧹 Tratamento de Dados & Limpeza
* **Eliminação de Colunas Redundantes/Alta Cardinalidade:** Remoção inicial de variáveis de texto cru que não agregariam valor preditivo direto de forma simples: `Name`, `Ticket` e `Cabin`.
* **Imputação de Valores Nulos:**
  - **`Age`:** Substituição dos valores ausentes pela média geral aproximada de idade.
  - **`Embarked`:** Preenchimento de nulos com a moda `'S'` (Porto de Southampton).
  - **`Fare`:** Imputação de valores ausentes pela média da tarifa para evitar quebras no conjunto de teste.

### 3. 🧪 Engenharia de Atributos (*Feature Engineering*)
* **Tratamento de Categóricas:** 
  - Mapeamento direto do gênero para uma flag numérica binária (`MaleCheck`).
  - Mapeamento ordinal estruturado da variável de embarque (`Embarked`: S = 0, C = 1, Q = 2).
* **Criação de Variáveis Familiares:**
  - **`Alone`:** Variável booleana identificando se o passageiro embarcou sem acompanhantes.
  - **`Family`:** Total de parentes a bordo a partir da soma direta das variáveis `SibSp` e `Parch`.

### 4. ⚖️ Otimização por Escalonamento (`RobustScaler`)
* **Visualização:** Identificação de forte assimetria e presença acentuada de outliers nas variáveis contínuas `Age` e `Fare` por meio de gráficos boxplot.
* **Tratamento:** Aplicação do `RobustScaler` (que utiliza mediana e intervalos interquartílicos), gerando uma grande estabilidade no modelo KNN (com salto na acurácia local de **71.5% para 83.7%**).

### 5. 🔍 Seleção de Variáveis (*Feature Selection*)
* **Análise de Correlação:** Mapeamento das correlações lineares das colunas por meio de matrizes de calor (*heatmaps*).
* **Importância das Variáveis:** Utilização do algoritmo de Árvore de Decisão (`feature_importances_`) e dos coeficientes da Regressão Logística para avaliar a relevância individual das features.
* **Redução de Redundância:** Exclusão definitiva das colunas originais `SibSp` e `Parch`, priorizando os atributos mais agregados (`Alone` e `Family`).

---

## 🤖 Modelagem e Avaliação de Performance

O desempenho dos modelos foi medido pela métrica de **Acurácia** usando divisão clássica em base de treino (80%) e validação local (20%):

| Modelo | Acurácia Inicial | Após Escalonamento (`RobustScaler`) | Com Seleção Final de Features |
| :--- | :---: | :---: | :---: |
| **Regressão Logística** | 79.88% | 81.00% | **80.44%** |
| **KNN (k=3)** | 71.50% | **83.79%** | **81.00%** |
| **Árvore de Decisão** | 78.21% | 77.65% | **78.21%** |

---

## 📂 Arquivos de Submissão Gerados

Durante o desenvolvimento das otimizações, foram exportados os seguintes arquivos prontos para submissão no Kaggle:
* **`resultado1.csv`:** Previsões iniciais com dados numéricos brutos usando Regressão Logística.
* **`resultado2.csv`:** Primeira evolução utilizando engenharia básica em colunas de texto (gênero/embarque) com Regressão Logística.
* **`resultado3.csv`:** Submissão gerada a partir do modelo **KNN** otimizado com escala de dados robusta.
* **`resultado_rl.csv`:** Submissão utilizando o modelo de **Regressão Logística** final após a seleção de variáveis e engenharia de atributos (apresentando melhor robustez e generalização na submissão oficial).
