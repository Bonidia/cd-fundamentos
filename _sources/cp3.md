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

# Visualização para Exploração de Dados

```{admonition} Importante
:class: tip
Para execução dos códigos, é necessário instalar as seguintes bibliotecas: 

 - *!pip install seaborn*
 - *!pip install matplotlib*
 - *!pip install wordcloud*
 - *!pip install plotly==5.13.1*
```

```{code-cell}
:tags: [remove-output]
import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from wordcloud import WordCloud
import plotly
import plotly.graph_objects as go
plotly.offline.init_notebook_mode(connected=True)
```

## Gráficos de Barras ou Colunas

```{code-cell}
:tags: [remove-output]
data = df_brasil.groupby(by=['STATE']).mean()['GDP_CAPITA']
ax = data.plot.bar(figsize=(10,5))

_ = ax.set(xlabel='Estados', ylabel='Produto interno Bruto (PIB) per capita')
```

```{code-cell}
:tags: [remove-output]
data = df_brasil.groupby(by=['REGION']).agg({'CITY': 'count', 'POPULATION_2018': 'sum'})

data['CITY'] = (data['CITY'] / data['CITY'].sum()) * 100
                
data['POPULATION_2018'] = (data['POPULATION_2018'] / data['POPULATION_2018'].sum()) * 100

ax = data.plot.barh(figsize=(10,5))

_ = ax.set(xlabel='Porcentagem com relação ao Brasil', ylabel='Região')
_ = ax.legend(['Porcentagem de cidades por região no Brasil', 'Porcentagem de pessoas por região no Brasil'])
```

## Gráfico de Setor

```{code-cell}
:tags: [remove-output]
# Especifica a sequência de cores a ser utilizada na visualização
colors = ['lightskyblue', 'red', 'blue', 'green', 'gold']
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(15, 15))

_ = df_brasil.groupby(by=['REGION']).sum()['TAXES'].plot.pie(colors=colors, autopct='%1.1f%%', ax=axes[0])
_ = df_brasil.groupby(by=['REGION']).sum()['GDP'].plot.pie(colors=colors, autopct='%1.1f%%', ax=axes[1])

_ = axes[0].set(xlabel='Distribuição do volume de impostos pagos por região do Brasil em 2016', ylabel='')
_ = axes[1].set(xlabel='Distribuição do PIB por região do Brasil em 2016', ylabel='')
```

## Gráficos de Dispersão

```{code-cell}
:tags: [remove-output]
# O valor de despesas de alguns municípios não foi disponibilizado na base de dados
data = df_brasil.query("MUN_EXPENDIT != 0")

f, ax = plt.subplots(figsize=(10, 6))
plt.yscale('log')

sct_plot = sns.scatterplot(x=data['IDHM'],
                           y=data['MUN_EXPENDIT'],
                           hue=data['REGION'])

sct_plot.set_xlabel(xlabel = 'IDH do município', fontsize = 12)
sct_plot.set_ylabel(ylabel = 'Gastos do município', fontsize = 12)
```

```{code-cell}
:tags: [remove-output]
f, ax = plt.subplots(figsize=(8, 8))
sns.scatterplot(x=data.LONG,
                y=data.LAT ,
                hue=data['IDHM'],
                size=data['MUN_EXPENDIT'])

_ = plt.title("Distribuição de municípios brasileiros de acordo com o seu IDH, suas despesas e a região onde se encontram."
```

```{code-cell}
:tags: [remove-output]
df = df_brasil[['WHEELED_TRACTOR', 'PLANTED_AREA', 'AREA', 'REGION']]

# Para filtrar municípios sem tratores registrados
df = df[df['WHEELED_TRACTOR'] != 0]

df['WHEELED_TRACTOR'] = np.log(df['WHEELED_TRACTOR'])
df['PLANTED_AREA'] = np.log(df['PLANTED_AREA'])
df['AREA'] = np.log(df['AREA'])

_ = sns.pairplot(df, hue="REGION")
```

## Gráficos de Linhas

```{code-cell}
:tags: [remove-output]
data = df_brasil.groupby('STATE').sum()[['POPULATION_2010', 'POPULATION_2018']]

plot = data.plot.line(figsize=(15,5))
_ = plt.xticks(range(0,len(data.index)), labels=data.index, rotation=45)
_ = plot.set(xlabel='Estados', ylabel='População')
```

## Gráficos de Radar

```{code-cell}
:tags: [remove-output]
data = df_brasil.groupby(by=['REGION']).agg({'CITY': 'count', 
                                       'POPULATION_2018': 'sum',
                                       'AREA': 'sum',
                                       'GDP': 'sum',
                                       'COMP_TOT': 'sum',
                                       'TAXES': 'sum'})

# Colocando os valores individuais em proporção do valor total
for col in data.columns:
    data[col] = data[col] / sum(data[col])
    
fig = go.Figure(
    data=[go.Scatterpolar(r=data.values[0], theta=data.columns, fill='toself', name=data.index[0]),
          go.Scatterpolar(r=data.values[1], theta=data.columns, fill='toself', name=data.index[1]),
          go.Scatterpolar(r=data.values[2], theta=data.columns, fill='toself', name=data.index[2]),
          go.Scatterpolar(r=data.values[3], theta=data.columns, fill='toself', name=data.index[3]),
          go.Scatterpolar(r=data.values[4], theta=data.columns, fill='toself', name=data.index[4])],
    layout=go.Layout(
        polar={'radialaxis': {'visible': True}},
        showlegend=True
    )
)

fig.show()
```

## Gráficos de Coordenadas Paralelas

```{code-cell}
:tags: [remove-output]
data = df_brasil.query("REGION == 'SUL' or \
                               REGION == 'NORDESTE'")

# Criando codificação para visualização das variáveis categóricas
category_rural_urban = data.RURAL_URBAN.astype('category').cat
category_region = data.REGION.astype('category').cat

# Declarando o gráfico com Plotly
fig = go.Figure(data=
    go.Parcoords(
        line = dict(color = category_region.codes),
        dimensions = list([
            dict(label = 'Área Total Média', values = data['AREA']),
            dict(label = 'Populaçao em 2018', values = data['POPULATION_2018']),
            dict(tickvals = [0, 1, 2, 3, 4],
                 ticktext = category_rural_urban.categories,
                 label = 'Tipologia Rural/Urbano', values = category_rural_urban.codes),
            dict(label = 'PIB per capita', values = data['GDP_CAPITA']),
            dict(label = "Região", tickvals = [0, 1], ticktext=category_region.categories, values = category_region.codes),
        ])
    )
)

fig.show()
```

```{code-cell}
:tags: [remove-output]
data = df_brasil.query("REGION == 'SUL' or \
                               REGION == 'NORDESTE'")

# Criando codificação para visualização das variáveis categóricas
category_rural_urban = data.RURAL_URBAN.astype('category').cat
category_state = data.REGION.astype('category').cat

# Declarando o gráfico com Plotly
fig = go.Figure(data=
    go.Parcoords(
        line = dict(color = category_state.codes,),
                    #showscale = True),
        dimensions = list([
            dict(label = 'Área Total Média', values = data['AREA']),
            dict(label = 'Populaçao em 2018', values = data['POPULATION_2018']),
            dict(tickvals = [0, 1, 2, 3, 4],
                 ticktext = category_rural_urban.categories,
                 label = 'Tipologia Rural/Urbano', values = category_rural_urban.codes),
            dict(label = 'PIB per capita', values = data['GDP_CAPITA']),
            dict(label = "Região", tickvals = [0, 1], ticktext=category_state.categories, values = category_state.codes),
        ])
    )
)

fig.show()
```

## Histogramas

```{code-cell}
:tags: [remove-output]
f, ax = plt.subplots(figsize=(14, 6))

filtro = df_brasil['GDP_CAPITA'] > df_brasil['GDP_CAPITA'].quantile(0.95)

ax = df_brasil[filtro]['STATE'].hist(histtype='bar', grid=False)
_ = ax.set(xlabel='Estado', ylabel='Proporção de PIB per capita')
```

## Gráfico de Caixa - BoxPlot

```{code-cell}
:tags: [remove-output]
ax = df_brasil.boxplot(column='IDHM', by='STATE', figsize=(15,6))
_ = ax.set(xlabel='', ylabel='')
plt.title('IDH dos municípios de cada estado.')
plt.suptitle('')
```

## Gráficos de Violino

```{code-cell}
:tags: [remove-output]
fig, axes = plt.subplots(nrows=1, ncols=2, figsize=(12, 5))

violin_hdi = sns.violinplot(x = 'CAPITAL', y = 'IDHM', data = df_brasil, palette = "Set3", ax=axes[0])
violin_hdi.set_xlabel(xlabel = 'É Capital?', fontsize = 12)
violin_hdi.set_ylabel(ylabel = 'IDH', fontsize = 12)
violin_hdi.set_title(label = 'Capital vs IDH', fontsize = 15)

box_hdi = sns.boxplot(x = 'CAPITAL', y = 'IDHM', data = df_brasil, palette = "Set3", ax=axes[1])
box_hdi.set_xlabel(xlabel = 'É Capital?', fontsize = 12)
box_hdi.set_ylabel(ylabel = 'IDH', fontsize = 12)
box_hdi.set_title(label = 'Capital vs IDH', fontsize = 15)
```

## Nuvens de Palavras

```{code-cell}
:tags: [remove-output]
text = ' '.join(df_brasil['CITY'])

# Remoção de palavras repetidas irrelevantes
stop_words = ['De', 'Do', 'Da']

# Mapeamento da frequência de cada palavra e produção da nuvem de palavras
wordcloud = WordCloud(background_color="white",
                      max_words=len(df_brasil),
                      max_font_size=70,
                      stopwords=stop_words,
                      height=300,
                      width=600).generate(text)

plt.figure(figsize=(20,12))
plt.imshow(wordcloud, interpolation="bilinear")
plt.axis("off")
```

## Mapas de Calor

```{code-cell}
:tags: [remove-output]
f, ax = plt.subplots(figsize=(8, 6))
ax = sns.heatmap(df_brasil.corr())
```
