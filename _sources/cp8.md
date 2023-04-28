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

# Modelagem de Dados

```{admonition} Importante
:class: tip
Para execução dos códigos, é necessário instalar as seguintes bibliotecas: 

 - *!pip install -U scikit-learn*
```

## Aplicação dos Algoritmos de Modelagem: Python


```{code-cell}
:tags: [remove-output]
# Pipeline de Classificação
import pandas as pd

# Fazendo a leitura dos dados
df = pd.read_csv('diabetes.csv')

# Utilizando hold-out como técnica de re-amostragem
from sklearn.model_selection import train_test_split

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Aplicando a técnica de hold-out 
training_set, test_set, train_labels, test_labels = train_test_split(X,  
                                                                     y,  
                                                                     test_size=0.3,
                                                                     random_state=12,
                                                                     stratify=y)

# Importando o classificador baseado em Árvore de Decisão
from sklearn.tree import DecisionTreeClassifier

# Inicializando o classificador
dt = DecisionTreeClassifier(random_state=42)

# Ajustando o modelo aos dados
dt.fit(training_set, train_labels)

# Coletando os valores previstos para o conjunto de teste
preds_dt = dt.predict(test_set)

# Importando a métrica de acurácia para avaliação das respostas
from sklearn.metrics import accuracy_score

# Calculando a acurácia com base nas respostas esperadas
print(accuracy_score(preds_dt, test_labels))
```

```{code-cell}
:tags: [remove-output]
# Pipeline de Regressão
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

# Inicializando o modelo
dt = DecisionTreeRegressor(random_state=42)

# Ajustando o modelo aos dados
dt.fit(train, train_labels)

# Coletando os valores previstos para o conjunto de teste
preds_dt = dt.predict(test)

# Importando a métrica do erro médio quadrático para avaliação do desempenho
from sklearn.metrics import mean_squared_error

# Calculando o erro das predições com base no valor esperado
print(mean_squared_error(test_labels, preds_dt))
```
