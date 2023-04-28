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

# Qualidade de Dados


## Valores Ausentes

```{code-cell}
:tags: [remove-output]
import pandas

df = pd.read_csv('nome_do_arquivo.csv')
```

```{code-cell}
# Identificando dados ausentes
df.isnull().sum()

# Remoção de objetos com valor ausente em qualquer atributo preditivo;
df_obj = df.dropna(how='any')

# Remoção de objetos com valor ausente em todos os atributos preditivos
df_obj = df.dropna(how='all')

# Remoção de objetos com valor ausente em qualquer/todos os atributos preditivos selecionados
df_obj = df.dropna(how='any', subset=['Coluna1', 'Coluna2'])
df_obj = df.dropna(how='all', subset=['Coluna1', 'Coluna2'])
```

```{code-cell}
# Remoção de atributo preditivo com valor ausente em qualquer objeto
df_pred = df.dropna(axis='columns')

# Remoção de atributo preditivo com valor ausente em todos os objetos
df_pred = df.dropna(axis='columns', how='all')

# Remoção de atributo preditivo com valor ausente em um número determinado de objetos
df_pred = df.dropna(axis='columns', thresh=3)
```

## Preenchimento de Valores


```{code-cell}
# Preencher com um valor constante
df_tratamento = df.fillna(value=0)

# Preencher com a média
df["Coluna"].fillna(df["Coluna"].mean())

# Preencher com a mediana
df["Coluna"].fillna(df["Coluna"].median())

# Preencher com a moda
df["Coluna"].fillna(df["Coluna"].mode())

# Preencher com o valor do próximo exemplo
df_tratamento = df.fillna(method="bfill")
```

## Valores Redundantes


```{code-cell}
# Encontrando dados duplicados
df.duplicated()

# Especificar a coluna que deseja encontrar dados duplicados
df.duplicated(["Coluna1", "Coluna2"])

# Apresentando dados duplicadas
df[df.duplicated(keep=False)]

# Contando dados duplicados
df.duplicated().sum()

# Removendo dados duplicadas
df.drop_duplicates()

# Removendo dados duplicados de uma coluna específica
df.drop_duplicates(["Coluna"])
```

## Valores Outliers


```{code-cell}
from scipy import stats

z_df = df.apply(stats.zscore)
df_filtered = df[(z_df < 3).all(axis=1)]
```