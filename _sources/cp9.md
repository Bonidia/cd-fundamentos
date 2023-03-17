---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# Avaliação, Ajuste e Seleção de Modelos

```{admonition} Importante
:class: tip
Para execução dos códigos, é necessário instalar as seguintes bibliotecas: 

 - *!pip install -U scikit-learn*
```

## Avaliação para Regressão


```{code-cell}
:tags: [remove-output]
# Pipeline de Regressão
import pandas as pd
from sklearn.datasets import fetch_california_housing

# Carregando os dados
califa_dataset = fetch_california_housing()

# Separando atributos preditivos do atributo alvo
data = pd.DataFrame(califa_dataset.data, columns=califa_dataset.feature_names)
target = califa_dataset.target

from sklearn.model_selection import train_test_split

# Aplicando a técnica de hold-out 
train, test, train_labels, test_labels = train_test_split(data,
                                                          target,
                                                          test_size=0.2,
                                                          random_state=12)
                                                          
# Importando o regressor baseado em Árvore de Decisão
from sklearn.tree import DecisionTreeRegressor

dt = DecisionTreeRegressor(random_state=42)

# Treinando o modelo e coletando predições
dt.fit(train, train_labels)
preds_dt = dt.predict(test)

# Importando as métricas de avaliação para regressão
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

# Exibindo os valores de avaliação de performance
print(f"MSE: {mean_squared_error(test_labels, preds_dt)}")
print(f"RMSE: {mean_squared_error(test_labels, preds_dt, squared=False)}")
print(f"MAE: {mean_absolute_error(test_labels, preds_dt)}")
print(f"R2_score: {r2_score(test_labels, preds_dt)}")
```

## Avaliação para Classificação


```{code-cell}
:tags: [remove-output]
# Pipeline de Classificação
import pandas as pd

# Leitura dos dados
df = pd.read_csv('diabetes.csv')

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

from sklearn.model_selection import train_test_split

# Aplicando a técnica de hold-out 
training_set, test_set, train_labels, test_labels = train_test_split(X,  
                                                                     y,  
                                                                     test_size=0.3,
                                                                     random_state=12,
                                                                     stratify=y)

# Importando o classificador baseado em Árvore de Decisão
from sklearn.tree import DecisionTreeClassifier

dt = DecisionTreeClassifier(random_state=42)

# Treinando o modelo e coletando predições
dt.fit(training_set, train_labels)
preds_dt = dt.predict(test_set)

# Importando as métricas de avaliação para classificação
from sklearn.metrics import accuracy_score, f1_score, recall_score, precision_score, roc_auc_score

# Exibindo os valores de avaliação de performance
print(f"Acurácia: {accuracy_score(test_labels, preds_dt)}")
print(f"F1-Score: {f1_score(test_labels, preds_dt)}")
print(f"Revocação (Recall): {recall_score(test_labels, preds_dt)}")
print(f"Precisão: {precision_score(test_labels, preds_dt)}")
print(f"AUC: {roc_auc_score(test_labels, preds_dt)}")

# Utilizando uma função que facilita a conferência de diversas métricas
from sklearn.metrics import classification_report

print(classification_report(test_labels, preds_dt))
```