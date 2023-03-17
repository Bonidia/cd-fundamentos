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

# Amostras de Dados para Experimentos

```{admonition} Importante
:class: tip
Para execução dos códigos, é necessário instalar as seguintes bibliotecas: 

 - *!pip install -U scikit-learn*
 - *!pip install -U imbalanced-learn*
```

## Procedimentos para Re-Amostragem de Dados - Hold-Out


```{code-cell}
:tags: [remove-output]
import pandas as pd

df = pd.read_csv('diabetes.csv')
```

```{code-cell}
# Importando o método hold-out 
from sklearn.model_selection import train_test_split

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Aplicando a técnica para hold-out 
training_set, test_set, train_labels, test_labels = train_test_split(X,  
                                                                     y,  
                                                                     test_size=0.3,
                                                                     random_state=12,
                                                                     stratify=y)

# test_size -> Indica o tamanho do teste
# random_state -> Fixa a geração de números aleatórios
# stratify -> Mantém a proporção das classes
```

## Procedimentos para Re-Amostragem de Dados - Validação Cruzada


```{code-cell}
# Importando o método StratifiedKFold
from sklearn.model_selection import StratifiedKFold

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Escolhendo o número de splits e semente do random_state
folds = StratifiedKFold(n_splits=10, shuffle=True, random_state=20)

# Utilizando um loop para selecionar os conjuntos de treino e teste
for train_index, test_index in folds.split(X, y):
    X_train, X_val = X[train_index], X[test_index]
    y_train, y_val = y[train_index], y[test_index]
    
    # Observando o tamanho de cada conjunto amostrado
    print(X_train.shape)
    print(y_train.shape)
    print(X_val.shape)
    print(y_val.shape)
```

## Procedimentos para Re-Amostragem de Dados - Deixe-Um-De-Fora


```{code-cell}
# Importando o método deixe-um-de-fora
from sklearn.model_selection import LeaveOneOut

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values # Atributos preditivos
y = df.iloc[:, -1].values # Atributo alvo

# Inicializando o método do deixe-um-de-fora
loo = LeaveOneOut()

# Utilizando um loop para selecionar os conjuntos de treino e teste
for train_index, test_index in loo.split(X, y):
    X_train, X_val = X[train_index], X[test_index]
    y_train, y_val = y[train_index], y[test_index]
    
    # Observando o tamanho de cada conjunto amostrado
    print(X_train.shape)
    print(y_train.shape)
    print(X_val.shape)
    print(y_val.shape)
```

## Procedimentos para Re-Amostragem de Dados - Bootstraping


```{code-cell}
# Importando o método ShuffleSplit
from sklearn.model_selection import ShuffleSplit

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values # Atributos preditivos
y = df.iloc[:, -1].values # Atributo alvo

# Inicializando o método para bootstrap com a definição da quantidade de vezes que a amostragem será feita, o tamanho do teste e a semente do random_state
ss = ShuffleSplit(n_splits=1000, test_size=0.25, random_state=3)

# Utilizando um loop para selecionar os conjuntos de treino e teste
for train_index, test_index in ss.split(X, y):
    X_train, X_val = X[train_index], X[test_index]
    y_train, y_val = y[train_index], y[test_index]
    
    # Observando o tamanho de cada conjunto amostrado
    print(X_train.shape)
    print(y_train.shape)
    print(X_val.shape)
    print(y_val.shape)
```

```{code-cell}
# Importando o método resample
from sklearn.utils import resample

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Aplicando o método de bootstrapping manualmente
n_splits = 20
for i in range(n_splits):
    split = resample(X, n_samples=50, replace=True, stratify=y, random_state=0)
    
    # Observando o conjunto amostrado 
    print(split)
    print('\n')
```

## Dados Desbalanceados - Undersampling


```{code-cell}
# Importando o método Counter para contagem dos exemplos das classes
from collections import Counter

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Verificando se o conjunto de dados é desbalanceado
print('Dataset shape %s' % Counter(y))
```

```{code-cell}
# Importando o método RUS
from imblearn.under_sampling import RandomUnderSampler

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Dividindo dados em treinamento e teste com hold-out
train_set, test_set, train_labels, test_labels = train_test_split(X,  
                                                                  y,  
                                                                  test_size=0.3,
                                                                  random_state=12,
                                                                  stratify=y)

# Aplicando RUS
rus = RandomUnderSampler(random_state=42)
train_res, train_labels_res = rus.fit_resample(train_set, train_labels)

print('Dataset shape %s' % Counter(train_labels))
print('Resampled dataset shape %s' % Counter(train_labels_res))
```

## Dados Desbalanceados - Oversampling


```{code-cell}
# Importando o método ROS
from imblearn.over_sampling import RandomOverSampler

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Dividindo dados em treinamento e teste com hold-out
train_set, test_set, train_labels, test_labels = train_test_split(X,  
                                                                  y,  
                                                                  test_size=0.3,
                                                                  random_state=12,
                                                                  stratify=y)

# Aplicando ROS
rs = RandomOverSampler()
train_res, train_labels_res = rs.fit_resample(train_set, train_labels)

print('Dataset shape %s' % Counter(train_labels))
print('Resampled dataset shape %s' % Counter(train_labels_res))
```

```{code-cell}
# Importando o método SMOTE
from imblearn.over_sampling import SMOTE

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Dividindo dados em treinamento e teste com hold-out
train_set, test_set, train_labels, test_labels = train_test_split(X,  
                                                                  y,  
                                                                  test_size=0.3,
                                                                  random_state=12,
                                                                  stratify=y)

# Aplicando Synthetic Minority Oversampling TEchnique (SMOTE)
s = SMOTE()
train_res, train_labels_res = s.fit_resample(train_set, train_labels)

print('Dataset shape %s' % Counter(train_labels))
print('Resampled dataset shape %s' % Counter(train_labels_res))
```
