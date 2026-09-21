# 🧠 SVM: Previsão de Propensão à Compra de Carros

Projeto desenvolvido no Módulo SVM da formação em Ciência de Dados da EBAC, com o objetivo de aplicar o algoritmo Support Vector Machine (SVM) para classificação da propensão de clientes à compra de um carro.

A atividade utiliza a mesma base de dados empregada anteriormente no projeto de XGBoost, permitindo comparar o desempenho de diferentes algoritmos de Machine Learning aplicados ao mesmo problema de classificação.

As etapas 1, 2, 3 e 4 seguem o mesmo processamento realizado anteriormente no projeto de XGBoost, uma vez que utilizam a mesma base de dados e possuem as mesmas necessidades iniciais de preparação. A partir da etapa 5, o desenvolvimento passa a ser específico do SVM, incluindo a padronização dos dados, treinamento com diferentes kernels e comparação dos resultados com o XGBoost.

## 🎯 Objetivo do Projeto

Desenvolver modelos de Support Vector Machine (SVM) capazes de prever se um cliente possui propensão à compra de um carro, representada pela variável `Purchased`.

O projeto também busca:
- Aplicar SVM em um problema de classificação binária;
- Comparar diferentes kernels;
- Avaliar o impacto da padronização dos dados;
- Analisar o desempenho dos modelos por meio de métricas de classificação;
- Comparar os resultados do SVM com o modelo XGBoost desenvolvido anteriormente.

## 🧩 Etapas Desenvolvidas

### 1. 📥 Carregamento e diagnóstico da base

Esta etapa segue o mesmo procedimento realizado no projeto anterior de XGBoost, pois a base de dados utilizada é a mesma.

Inicialmente, foi realizada a importação da base `CARRO_CLIENTES.csv` e a verificação de sua estrutura.

Foram analisados:
- Tipos de dados;
- Variáveis categóricas;
- Dados faltantes;
- Identificadores;
- Estrutura geral da base.

A única variável categórica identificada foi `Gender`.

#### 🗑️ Exclusão da variável `User ID`

A coluna `User ID` foi removida por representar apenas um identificador único de cliente.

Apesar de possuir formato numérico, essa variável não representa uma característica comportamental ou estatística relevante para a previsão e poderia introduzir ruído ao modelo.

### 2. 🔤 Codificação da variável categórica

Assim como na atividade anterior, a variável `Gender` precisou ser transformada para um formato numérico.

Foi utilizado o LabelEncoder, permitindo que a variável categórica fosse utilizada pelos algoritmos de Machine Learning.

Após a transformação, todas as variáveis utilizadas no modelo passaram a estar em formato numérico.

### 3. 📊 Análise de correlação

Também seguindo o mesmo procedimento realizado no projeto de XGBoost, foi construída uma matriz de correlação para investigar a associação linear entre as variáveis e o alvo `Purchased`.

Os principais resultados foram:

| Variável | Correlação com `Purchased` |
---      | ---
 `Age` |	0.62
`AnnualSalary` |	0.36
 Gender` |	-0.05

 A variável `Age` apresentou a maior associação linear positiva com o alvo, seguida por AnnualSalary.

 Já `Gender` apresentou uma correlação próxima de zero, indicando uma associação linear praticamente inexistente com `Purchased`.

    A correlação foi utilizada como ferramenta exploratória e não representa, isoladamente, importância preditiva ou causalidade.

### 4. ✂️ Separação entre treino e teste

A base foi dividida entre:

- **X:** variáveis independentes;
- **y:** variável alvo `Purchased`.

Posteriormente, os dados foram separados em conjuntos de treinamento e teste utilizando:

- **80% para treinamento;**
- **20% para teste;**
- `random_state=42`.

Assim como no projeto anterior, não foi realizado balanceamento das classes nem uma busca exaustiva de hiperparâmetros, pois o escopo da atividade prioriza a aplicação e interpretação do modelo, além da comparação com o XGBoost desenvolvido anteriormente.

Até esta etapa, o processamento segue essencialmente o mesmo fluxo utilizado no projeto de XGBoost.

### 5. ⚙️ Padronização dos dados e treinamento do SVM

A partir desta etapa, o processamento passa a ser específico do Support Vector Machine.

Como o SVM é sensível à escala das variáveis, foi utilizada a técnica StandardScaler para padronizar os dados.

O `StandardScaler` foi ajustado exclusivamente sobre os dados de treinamento e posteriormente aplicado aos dados de teste, evitando vazamento de informações entre os conjuntos.

#### Configuração inicial

Foi utilizado o classificador `SVC` com:

- Kernel: `linear`;
- `C = 1.0`;
- `random_state = 1`.

#### 🔹 SVM com Kernel Linear

O primeiro modelo foi treinado utilizando um kernel linear, buscando estabelecer uma fronteira de decisão linear entre as classes.

#### Resultados

O modelo apresentou:

- **Acurácia:** 81%;
- **Precisão da classe 1:** 89%;
- **Recall da classe 1:** 64%;
- **F1-Score da classe 1:** 0.74;
- **Falsos positivos:** 7.

O modelo apresentou maior capacidade de identificação da classe de não compradores, enquanto apresentou um recall mais conservador para compradores.

### 6. 🔵 SVM com Kernel Polinomial

Em seguida, foi desenvolvido um segundo modelo SVM utilizando o kernel polinomial (poly).

A configuração utilizada foi:

- Kernel: `poly`;
- `C = 1.0`;
- `random_state = 1`.

O objetivo foi verificar se uma fronteira de decisão capaz de representar relações não lineares poderia melhorar a classificação em relação ao kernel linear.

#### Resultados

O modelo apresentou:

- **Acurácia:** 82%;
- **Precisão da classe 1:** 91%;
- **Recall da classe 1:** 66%;
- **F1-Score da classe 1:** 0.76;
- **Falsos positivos:** 6.

Em comparação ao kernel linear, houve uma pequena melhoria nas principais métricas, especialmente na precisão e no F1-Score da classe de compradores.

### 7. 📈 Comparação entre os modelos SVM

Os dois modelos foram avaliados utilizando:

- Accuracy;
- Precision;
- Recall;
- F1-Score;
- Matriz de confusão.



| Modelo | Acurácia | Precisão Classe 1 | Recall Classe 1 | F1-Score Classe 1 | 
--- | --- | --- | --- | --- 
| SVM Linear |  81% | 89% | 64% | 0.74 |
| SVM Polinomial | 82% | 91% | 66% | 0.76 |

Entre os dois modelos SVM desenvolvidos, o kernel polinomial apresentou desempenho ligeiramente superior, aumentando a acurácia de 81% para 82% e o F1-Score da classe de compradores de 0.74 para 0.76.

Apesar da melhoria, ambos os modelos apresentaram recall relativamente baixo para a classe 1, indicando que uma parcela relevante dos compradores reais não foi identificada pelo modelo.

### 8. 🆚 Comparação com o XGBoost

Como a mesma base de dados e o mesmo problema de classificação foram utilizados no projeto anterior, os resultados do SVM foram comparados com o modelo XGBoost desenvolvido anteriormente.

| Modelo | Acurácia | Precisão Classe 1 | Recall Classe 1 | F1-Score Classe 1 | 
--- | --- | --- | --- | --- 
| SVM Linear |  81% | 89% | 64% | 0.74 |
| SVM Polinomial | 82% | 91% | 66% | 0.76 |
| XGBoost | 91% | 93% | 86% | 0.89 |

O XGBoost apresentou os melhores resultados entre os modelos comparados, com 91% de acurácia, 93% de precisão, 86% de recall e F1-Score de 0.89 para a classe de compradores.

A principal diferença está no recall: enquanto o SVM polinomial identificou 66% dos compradores reais, o XGBoost alcançou 86%.

Isso demonstra que, para esta base e configuração utilizada na atividade, o XGBoost apresentou maior capacidade de identificar corretamente os clientes pertencentes à classe de compradores.

## 🔎 Principais Insights

### 📌 Idade e salário

A análise de correlação identificou `Age` e `AnnualSalary` como as variáveis com maior associação linear com a variável alvo.

### 📌 SVM Linear vs. Polinomial

A utilização do kernel polinomial proporcionou uma pequena melhoria em relação ao kernel linear, indicando que uma fronteira de decisão não linear pode representar melhor alguns padrões presentes nos dados.

### 📌 SVM vs. XGBoost

O XGBoost apresentou desempenho superior aos dois modelos SVM avaliados, principalmente na identificação dos compradores reais.

Enquanto o SVM polinomial apresentou 66% de recall para a classe 1, o XGBoost alcançou 86%, reduzindo significativamente a quantidade de falsos negativos.

## 🛠️ Tecnologias e Ferramentas

- Python
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Support Vector Machine (SVM)
- SVC
- StandardScaler
- LabelEncoder
- XGBoost
- Jupyter Notebook
- Git
- GitHub

## 🎯 Competências Demonstradas

- Data Loading
- Data Cleaning
- Data Preparation
- Data Preprocessing
- Feature Encoding
- Label Encoding
- Data Standardization
- Exploratory Data Analysis
- Correlation Analysis
- Train/Test Split
- Supervised Machine Learning
- Binary Classification
- Support Vector Machine
- SVM Linear Kernel
- SVM Polynomial Kernel
- Model Evaluation
- Accuracy
- Precision
- Recall
- F1-Score
- Confusion Matrix
- Model Comparison
- XGBoost
- Interpretação de resultados de Machine Learning

## 📌 Conclusão

O projeto demonstrou a aplicação do Support Vector Machine (SVM) em um problema de classificação de propensão à compra de carros.

As etapas iniciais de carregamento, tratamento, codificação, análise de correlação e separação dos dados foram mantidas em relação ao projeto anterior de XGBoost, uma vez que ambos utilizam a mesma base e o mesmo problema de negócio.

A partir da etapa de modelagem, o projeto passou a utilizar procedimentos específicos do SVM, principalmente a padronização dos dados e a avaliação de diferentes kernels.

Entre os modelos SVM testados, o kernel polinomial apresentou desempenho ligeiramente superior ao kernel linear, alcançando 82% de acurácia e F1-Score de 0.76 para a classe de compradores.

Entretanto, na comparação com o XGBoost desenvolvido anteriormente, o XGBoost apresentou desempenho superior nas métricas avaliadas, especialmente no recall da classe de compradores e no F1-Score.

O exercício permitiu, portanto, compreender na prática como diferentes algoritmos de classificação podem apresentar comportamentos distintos sobre a mesma base de dados, além de reforçar a importância da padronização, escolha do algoritmo, avaliação por múltiplas métricas e comparação entre modelos.