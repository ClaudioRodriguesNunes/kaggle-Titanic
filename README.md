# Titanic – Machine Learning from Disaster  
**Desafio Kaggle | Projeto End-to-End de Ciência de Dados**

---

## 🇧🇷 Versão em Português

## 📌 Visão Geral do Projeto

Este projeto aborda o clássico desafio do Kaggle **“Titanic: Machine Learning from Disaster”**, com foco principal em:

- Análise Exploratória de Dados (EDA) cuidadosa
- Engenharia de atributos explicável
- Tratamento adequado de valores ausentes sem vazamento de dados
- Modelagem com baseline interpretável
- Pipeline de Machine Learning reproduzível e limpo

O objetivo **não** é maximizar o score no leaderboard, mas demonstrar **método, rigor técnico e capacidade de tomada de decisão baseada em dados**.

---

## 🎯 Objetivo

Prever se um passageiro sobreviveu ao desastre do Titanic a partir de informações demográficas e socioeconômicas disponíveis **antes do evento**.

Variável alvo:
- `Survived` (0 = Não sobreviveu, 1 = Sobreviveu)

---

## 🧠 Metodologia

### 1️⃣ Análise Exploratória de Dados (EDA)

Principais observações:

- A sobrevivência está fortemente associada a sexo, classe social e papel social
- Valores ausentes concentram-se principalmente em:
  - `Age`
  - `Cabin`
  - `Embarked`
- Estrutura familiar e contexto social influenciam a chance de sobrevivência

As decisões de engenharia de atributos foram guiadas por evidências observadas no EDA.

---

### 2️⃣ Engenharia de Atributos

Foram criadas as seguintes features:

- **FamilySize**  
  FamilySize = SibSp + Parch + 1

- **HasCabin**  
  Indicador binário da existência de cabine registrada.

- **TitleClean**  
  Extraído do nome do passageiro, representando papel social:
  - `Mr`, `Mrs`, `Miss`, `Master`
  - Demais títulos agrupados como `Rare`

- **Fare_log**  
  Transformação logarítmica do valor do ingresso para reduzir assimetria.

---

### 3️⃣ Imputação de Idade (Sem Vazamento)

A idade não foi imputada por média ou mediana global.  
Foi utilizada a **mediana condicional** baseada em:

- `TitleClean`
- `Pclass`

Vantagens:
- Preserva estrutura social e econômica
- Evita regras arbitrárias
- Não utiliza a variável alvo
- Evita vazamento de dados

Uma nova coluna `Age_imputed` foi criada, preservando a variável original.

---

### 4️⃣ Estratégia de Modelagem

#### Modelo Base: Árvore de Decisão

Foi utilizada uma **Árvore de Decisão regularizada**, equilibrando interpretabilidade e desempenho.

Parâmetros principais:
DecisionTreeClassifier(max_depth=5, min_samples_leaf=20, random_state=42)

Motivos da escolha:
- Regras explícitas e interpretáveis
- Importância das variáveis facilmente analisável
- Excelente baseline para dados tabulares

---

### 5️⃣ Pipeline e Validação

Foi utilizado um pipeline completo do `scikit-learn`:

- One-Hot Encoding para variáveis categóricas
- Variáveis numéricas sem escala
- Separação treino/validação com estratificação

Resultados:
- Acurácia de validação interna ≈ **0.81**
- Baixo overfitting (diferença pequena entre treino e validação)

---

### 6️⃣ Experimento com Modelo Avançado (Opcional)

Foi testado um modelo de **Gradient Boosting**.

Resultado:
- Acurácia maior no treino
- Aumento significativo do gap treino-validação
- Evidência de overfitting

Decisão:
O modelo não foi adotado para a submissão final, priorizando generalização e interpretabilidade.

---

## 📤 Submissão no Kaggle

- Modelo final: **Árvore de Decisão Regularizada**
- Score público no Kaggle: **~0.76**

Esse resultado é consistente com:
- Modelagem conservadora
- Ausência de tuning agressivo
- Pipeline correto e sem vazamento

---

## 📁 Estrutura do Repositório

data/
  train.csv
  test.csv
src/
  eda.py
  model.py
submission.csv
README.md

---

## 🧩 Principais Aprendizados

- Engenharia de atributos pode ser mais importante que modelos complexos
- Modelos interpretáveis facilitam aprendizado e comunicação
- Nem todo ganho de performance justifica aumento de complexidade
- Entender **por que** o modelo funciona é essencial

---

## 👤 Autor

**Claudio Rodrigues Nunes**  
Estudante de Ciência da Computação | Análise de Dados e Machine Learning

---
---

## 🇺🇸 English Version

## 📌 Project Overview

This project addresses the classic Kaggle challenge **“Titanic: Machine Learning from Disaster”**, focusing on:

- Careful Exploratory Data Analysis (EDA)
- Explainable feature engineering
- Proper handling of missing values without data leakage
- Interpretable baseline modeling
- Clean and reproducible machine learning pipeline

The goal is **not** leaderboard optimization, but to demonstrate **solid data science methodology and decision-making**.

---

## 🎯 Objective

Predict whether a passenger survived the Titanic disaster based on demographic and socioeconomic information available **before the event**.

Target variable:
- `Survived` (0 = No, 1 = Yes)

---

## 🧠 Methodology

### 1️⃣ Exploratory Data Analysis (EDA)

Key insights:

- Survival is strongly associated with gender, passenger class, and social role
- Missing values are concentrated in:
  - `Age`
  - `Cabin`
  - `Embarked`
- Family structure and social context influence survival chances

EDA guided all feature engineering decisions.

---

### 2️⃣ Feature Engineering

Engineered features include:

- **FamilySize**  
  FamilySize = SibSp + Parch + 1

- **HasCabin**  
  Binary flag indicating the presence of a cabin record.

- **TitleClean**  
  Extracted from passenger names to represent social roles:
  - `Mr`, `Mrs`, `Miss`, `Master`
  - Other titles grouped as `Rare`

- **Fare_log**  
  Log transformation applied to reduce skewness.

---

### 3️⃣ Age Imputation (No Leakage)

Age was not imputed using a global mean or median.  
Instead, **conditional medians** based on:

- `TitleClean`
- `Pclass`

This approach:
- Preserves social and economic structure
- Avoids arbitrary assumptions
- Does not use the target variable
- Prevents data leakage

A separate `Age_imputed` feature was created.

---

### 4️⃣ Modeling Strategy

#### Baseline Model: Decision Tree

A **regularized Decision Tree** was used to balance performance and interpretability.

Key parameters:
DecisionTreeClassifier(max_depth=5, min_samples_leaf=20, random_state=42)

---

### 5️⃣ Pipeline & Validation

A full `scikit-learn` pipeline was implemented:

- One-Hot Encoding for categorical variables
- Numerical features passed through
- Stratified train/validation split

Results:
- Internal validation accuracy ≈ **0.81**
- Low overfitting

---

### 6️⃣ Advanced Model Experiment (Optional)

A **Gradient Boosting** model was tested.

Outcome:
- Higher training accuracy
- Larger train–validation gap
- Evidence of overfitting

Decision:
The model was not adopted for final submission to preserve generalization.

---

## 📤 Kaggle Submission

- Final model: **Regularized Decision Tree**
- Public Kaggle score: **~0.76**

This score reflects:
- Conservative modeling choices
- No aggressive tuning
- Clean and leakage-free pipeline

---

## 📁 Repository Structure

data/
  train.csv
  test.csv
src/
  eda.py
  model.py
submission.csv
README.md

---

## 🧩 Key Takeaways

- Feature engineering often matters more than model complexity
- Interpretable models enhance learning and communication
- Not all performance gains justify added complexity
- Understanding why a model works is essential

---

## 👤 Author

**Claudio Rodrigues Nunes**  
Computer Science Student | Data Analysis & Machine Learning
