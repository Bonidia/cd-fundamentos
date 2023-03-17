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

# Python para Ciência de Dados


## Manipulação de Dados Tabulares com Pandas - Funções Básicas

```{code-cell}
:tags: [remove-output]
import pandas as pd
```

```{code-cell}
cidades = pd.DataFrame(
    [
        ['Paraná', 'Londrina', 575377, 1356.00],
        ['São Paulo', 'São Carlos', 254484, 1508.00],
        ['Santa Catarina', 'Florianópolis', 508826, 1798.00],
        ['Paraná', 'Curitiba', 1963726, 2293.00],
        ['São Paulo', 'Campinas', 1223237, 1710.00]
    ], columns=['Estado', 'Cidade', 'Habitantes', 'Salário-Médio'])

cidades
```

```{code-cell}
:tags: [remove-output]
df = pd.read_csv('qualidade_vinho.csv', sep=',', header=0)
df.head(2)
```

```{code-cell}
cidades.head(2) # Retorna um novo dataframe com as n primeiras amostras
```

```{code-cell}
cidades.tail(2) # Retorna um novo dataframe com as n últimas amostras
```

```{code-cell}
cidades.shape # Retorna uma tupla com o número de linhas e colunas
```

```{code-cell}
cidades.info() # Retorna vazio e imprime na tela as informações gerais do dataframe
```

```{code-cell}
cidades.columns # Retorna uma lista com o nome de todas as colunas
```

```{code-cell}
cidades['Salário-Médio'].describe() # Retorna um objeto DataFrame com as medidas estatísticas para cada coluna
```

## Tipos de Dados com Pandas


```{code-cell}
cidades.dtypes
cidades.info()
```

```{code-cell}
cidades['Habitantes'] = cidades['Habitantes'].astype('float64')
cidades.dtypes
```

```{code-cell}
cidades['Data'] = ['3-8-2021', '3-9-2021', '3-10-2021', '3-11-2021', '3-12-2021']

cidades['Data'] = pd.to_datetime(cidades['Data'], format='%d-%m-%Y') # Dia, Mês, Ano

cidades
```

```{code-cell}
cidades['Data'] = cidades['Data'].dt.strftime('%d-%m-%Y') # Dia, Mês, Ano
cidades # Novo Formato
```

## Renomeando Colunas


```{code-cell}
cidades.rename(columns={'Habitantes': 'N Habitantes', 'Data': 'Data - Censo'}, inplace=True)
cidades
```

## Selecionando Linhas e Colunas


```{code-cell}
cidadesHab = cidades[['N Habitantes', 'Salário-Médio']] # Seleciona as duas colunas com os nomes especificados
cidadesHab
```

```{code-cell}
cidades.loc[[1, 2]] # Seleciona as duas linhas com os nomes especificados (note que o "rótulo" de cada linha está presente na forma de números inteiros)
```

```{code-cell}
cidades.iloc[1:3] # Seleciona as linhas do dataframe original, referentes ao índice determinado
```

```{code-cell}
cidades.iloc[:, 0:2] # Seleciona todas as linhas e as duas primeiras colunas do dataframe original
```

```{code-cell}
cidades.iloc[1:3, :3] # Seleciona as duas primeiras linhas e as três primeiras colunas
```

```{code-cell}
cidades.iloc[:, [1, 3, 0, 2]] # Seleciona todas as linhas e as colunas determinadas
```

## Adicionando e Removendo Colunas


```{code-cell}
cidades.insert(2, 'Sigla', ['PR', 'SP', 'SC', 'PR', 'SP']) # Adiciona novos dados no índice dois
cidades.insert(2, 'Sigla-Repetida', cidades['Sigla']) # Adiciona uma coluna repetida "Sigla"
cidades
```

```{code-cell}
cidades.pop('Sigla-Repetida')

# ou

cidades = cidades.drop(['Sigla-Repetida'], axis=1)
cidades
```

## Operações Básicas - Consultas

```{code-cell}
cidades.query('`Salário-Médio` > 1400.0')
```

```{code-cell}
cidades.query('Estado == "São Paulo"')
```

```{code-cell}
cidades.query('`N Habitantes` > 500000 and `N Habitantes` < 1300000')
```

## Operações Básicas - Ordenação


```{code-cell}
cidades.sort_values(by='Cidade', inplace=True)
cidades
```

```{code-cell}
cidades.sort_values(by='Salário-Médio', inplace=True)
cidades
```

## Operações Básicas - Combinando DataFrames


```{code-cell}
cidades_salarios = pd.DataFrame(
    [
        ['Paraná', 'Londrina', 1356.00],
        ['São Paulo', 'São Carlos', 1508.00],
        ['Santa Catarina', 'Florianópolis', 1798.00],
        ['Paraná', 'Curitiba', 2293.00],
        ['São Paulo', 'Campinas', 1710.00]
    ], columns=['Estado', 'Cidade', 'Salário-Médio'])

cidades_salarios
```

```{code-cell}
cidades_pop = pd.DataFrame(
    [
        ['Londrina', 575377],
        ['São Carlos', 254484],
        ['Florianópolis', 508826],
        ['Curitiba', 1963726],
        ['Campinas', 1223237]
    ], columns=['Cidade', 'Habitantes'])

cidades_pop
```

```{code-cell}
pd.merge(left=cidades_salarios, right=cidades_pop, on="Cidade")
```

```{code-cell}
pd.merge(left=cidades_salarios, right=cidades_pop, on="Cidade", how="outer")
```

```{code-cell}
cidades_df1 = pd.DataFrame(
    [
        ['Londrina', 575377],
        ['São Carlos', 254484],
        ['Florianópolis', 508826],
    ], columns=['Cidade', 'Habitantes'])

cidades_df1
```

```{code-cell}
cidades_df2 = pd.DataFrame(
    [
        ['Curitiba', 1963726],
        ['Campinas', 1223237]
    ], columns=['Cidade', 'Habitantes'])

cidades_df2
```

```{code-cell}
pd.concat([cidades_df1, cidades_df2], ignore_index=True)
```

## Operações Básicas - Salvando DataFrames


```{code-cell}
cidades.to_csv("my_df.csv")
cidades.to_html("my_df.html")
cidades.to_json("my_df.json")
```
