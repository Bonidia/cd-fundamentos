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

# Engenharia de Características

```{admonition} Importante
:class: tip
Para execução dos códigos, é necessário instalar as seguintes bibliotecas: 

 - *!pip install -U scikit-learn*
```

## Redução de Dimensionalidade - Agregação de Atributos


```{code-cell}
:tags: [remove-output]
import pandas as pd

df = pd.read_csv('diabetes.csv')
```

```{code-cell}
# Importando a Técnica PCA e Método de normalização (Z-score)
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Pressuposto do PCA é que os dados seguem uma distribuição normal
sc = StandardScaler()
X = sc.fit_transform(X)

# Atribuindo a técnica PCA e indicando o número de componentes principais desejados
pca = PCA(n_components=2)

# Ajustando e aplicando aos dados
X_pca = pca.fit_transform(X)

# A variação explicada informa quanta informação (variação) pode ser atribuída a cada um dos componentes principais.
pca.explained_variance_ratio_
```


## Redução de Dimensionalidade - Seleção de Atributos - Ordenação


```{code-cell}
#Importando os métodos necessários
from sklearn.preprocessing import MinMaxScaler
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import f_classif
from numpy import set_printoptions

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Escalando os dados agora com o método MinMax (apenas para fins de compreensão)
sc = MinMaxScaler()
X = sc.fit_transform(X)

# Método de seleção: Análise de Variância (ANOVA) -> F_classif
# k = números de atributos que se deseja selecionar no ranking
fs = SelectKBest(score_func=f_classif, k=3)
fs.fit(X, y)

# Exibindo a classificação
set_printoptions(precision=3)
print(fs.scores_)

# Aplicando a seleção
X_selected = fs.transform(X)
```

```{code-cell}
#Importando os métodos necessários
from sklearn.preprocessing import MinMaxScaler
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import chi2
from numpy import set_printoptions

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Escalando os dados agora com o método MinMax - Apenas para fins de compreensão
sc = MinMaxScaler()
X = sc.fit_transform(X)

# Método de seleção: Qui-quadrado (Chi-Squared) -> chi2
# k = números de atributos que se deseja selecionar no ranking
fs = SelectKBest(score_func=chi2, k=3)
fs.fit(X, y)

# Exibindo a classificação
set_printoptions(precision=3)
print(fs.scores_)

# Aplicando a seleção
X_selected = fs.transform(X)
```

## Redução de Dimensionalidade - Seleção de Atributos - Complementaridade


```{code-cell}
#Importando os métodos necessários
from sklearn.preprocessing import MinMaxScaler
from sklearn.feature_selection import SelectKBest
from sklearn.feature_selection import mutual_info_classif
from numpy import set_printoptions

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Escalando os dados agora com o método MinMax - Apenas para fins de compreensão
sc = MinMaxScaler()
X = sc.fit_transform(X)

# Método de seleção: Informação Mútua -> mutual_info_classif
# k = números de atributos que se deseja selecionar no ranking
fs = SelectKBest(score_func=mutual_info_classif, k=3)
fs.fit(X, y)

# Exibindo a classificação
set_printoptions(precision=3)
print(fs.scores_)

# Aplicando a seleção
X_selected = fs.transform(X)
```

```{code-cell}
#Importando os métodos necessários
from sklearn.preprocessing import MinMaxScaler
from sklearn.feature_selection import RFE

# Algoritmo de indução para verificar o desempenho dos subconjuntos de atributos
from sklearn.ensemble import RandomForestClassifier

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Escalando os dados agora com o método MinMax - Apenas para fins de compreensão
sc = MinMaxScaler()
X = sc.fit_transform(X)

# Instanciando o classificador para fazer a seleção de maneira recursiva nos subconjuntos
rf = RandomForestClassifier()

# Seleção de 4 atributos
n_features = 4

# Método de seleção: Eliminação Recursiva de Atributos (Recursive Feature Elimination) utilizando modelo preditivo
rfe = RFE(rf, n_features_to_select=n_features) 

# Ajuste do modelo para os objetos e seleção dos atributos
rfe = rfe.fit(X, y)

# Transformação dos dados iniciais para a nova quantidade de dimensões 
X_features = rfe.transform(X)

print("Num Atributos: %s" % (rfe.n_features_))
print("Atributos selecionados: %s" % (rfe.support_))
print("Ranking Atributos: %s" % (rfe.ranking_))
```

```{code-cell}
#Importando os métodos necessários
from sklearn.preprocessing import MinMaxScaler
from sklearn.feature_selection import SelectFromModel

# Algoritmo de indução para verificar o desempenho dos subconjuntos de atributos
from sklearn.ensemble import RandomForestClassifier

# Separando os dados em atributos preditivos (X) e atributo alvo (y)
X = df.iloc[:, :-1].values
y = df.iloc[:, -1].values

# Escalando os dados agora com o método MinMax - Apenas para fins de compreensão
sc = MinMaxScaler()
X = sc.fit_transform(X)

# Instanciando o classificador para fazer a seleção baseada na importância calculada de cada característica
rf = RandomForestClassifier()

# Valor limiar de importância para seleção dos atributos
threshold_value=0.06

# Método de seleção: Seleção dos K-melhores atributos com base na importância dos atributos para um modelo
sfm = SelectFromModel(rf, threshold=threshold_value) 

# Ajuste do modelo para os objetos e seleção dos atributos
sfm.fit(X, y)

# Aplicando a seleção
X_features = sfm.transform(X)
```