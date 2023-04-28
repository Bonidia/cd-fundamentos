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

# Transformação de Dados

```{admonition} Importante
:class: tip
Para execução dos códigos, é necessário instalar as seguintes bibliotecas: 

 - *!pip install AnonymizeDF*
```

## Anonimização de Dados


```{code-cell}
:tags: [remove-output]
# Importando as bibliotecas
import pandas as pd
from anonymizedf.anonymizedf import anonymize

# Criando um DataFrame com as 3 primeiras amostras do exemplo anterior
dados = {'Nome': ['Ana', 'Bárbara', 'Cláudia'],
        'Altura': [175, 162, 161],
        'Idade': [25, 37, 45],
        'Pressão': [132, 90, 140],
        'Temperatura': [38, 37, 39],
        'Sexo': ['Feminino', 'Feminino', 'Feminino'],
        'Escolaridade': ['Superior', 'Nenhuma', 'Superior'],
        'Diagnóstico': ['Doente', 'Saudável', 'Saudável']}

df = pd.DataFrame(dados)
        
# Anonimizando os dados
an = anonymize(df)
an.fake_names('Nome')
an.fake_whole_numbers('Altura')
an.fake_whole_numbers('Idade')
an.fake_whole_numbers('Pressão')
an.fake_whole_numbers('Temperatura')
an.fake_categories('Sexo')
an.fake_categories('Escolaridade')
an.fake_categories('Diagnóstico')

# an.fake_dates('Coluna') 
# an.fake_decimal_numbers('Coluna')

# Dados anonimizados
df.iloc[:, 8:16]
```

## Conversão de Valores entre Diferentes Tipos


```{code-cell}
cidades = pd.DataFrame(
    [
        ['Paraná', 'Londrina', 575377, 1356.00, 'Quinto', 'Não'],
        ['São Paulo', 'São Carlos', 254484, 1508.00, 'Quarto', 'Não'],
        ['Santa Catarina', 'Florianópolis', 508826, 1798.00, 'Segundo', 'Sim'],
        ['Paraná', 'Curitiba', 1963726, 2293.00, 'Primeiro', 'Sim'],
        ['São Paulo', 'Campinas', 1223237, 1710.00, 'Terceiro', 'Não']
    ], columns=['Estado', 'Cidade', 'Habitantes', 'Salário-Médio', 'Classificação', 'Capital'])
```

```{code-cell}
# Importando a função de transformação ordinal do scikit-learn
from sklearn.preprocessing import OrdinalEncoder

ordem = ['Primeiro', 'Segundo', 'Terceiro', 'Quarto', 'Quinto']
codificador = OrdinalEncoder(categories=[ordem])
cidades['Classificação'] = codificador.fit_transform(cidades[['Classificação']])
```

```{code-cell}
# Importando a função de transformação binária do scikit-learn
from sklearn.preprocessing import OneHotEncoder

encoder = OneHotEncoder()

estados = encoder.fit_transform(cidades[['Estado']]).toarray()
estados = pd.DataFrame(estados, columns=encoder.categories_[0])
cidades = pd.concat([cidades, estados], axis=1)

# Removendo a coluna original com os dados nominais
cidades.drop('Estado', axis=1, inplace=True)
```

```{code-cell}
estados = pd.get_dummies(cidades['Estado'])
cidades = pd.concat([cidades, estados], axis=1)

# Removendo a coluna original com os dados nominais
cidades.drop('Estado', axis=1, inplace=True)
```

```{code-cell}
# Importando a função de transformação de scikit-learn
from sklearn.preprocessing import LabelEncoder

encoder = LabelEncoder()
cidades['Capital'] = encoder.fit_transform(cidades[['Capital']])
```

```{code-cell}
# Criando o DataFrame de idade
df = pd.DataFrame({'Idade': [42, 15, 67, 55, 
                             1, 29, 75, 89, 4,
                             10, 15, 38, 22, 77]})
                        
df['Label'] = pd.cut(x=df['Idade'], bins=[0, 3, 17, 70, 99],
                     labels=['Bebê', 'Criança', 
                             'Adulto', 'Idoso'])
df
```

## Transformação de Valores Numéricos


```{code-cell}
# Importando a biblioteca numpy
import numpy as np

#Criando um atributo randômico usando distribuição beta para teste 
dados = np.random.beta(a=4, b=15, size=300)

#Transformação logarítmica
dados_log = np.log(dados)
dados_log 

#Transformação de raiz quadrada
dados_sqrt = np.sqrt(dados)
dados_sqrt

#Transformação Módulo (valor absoluto)
dados_absoluto = np.absolute(dados_log )
dados_absoluto
```

```{code-cell}
# Importando a função MinMaxScaler
from sklearn.preprocessing import MinMaxScaler

df = pd.DataFrame({'Valores': [1, 18, 0.5, 20, 10, 0.1, 15]})
                        
scaler = MinMaxScaler(feature_range=[0, 1])
df_minmax = scaler.fit_transform(df)
```

```{code-cell}
# Importando a função  StanderScaler
from sklearn.preprocessing import StandardScaler

df = pd.DataFrame({'Valores': [1, 18, 0.5, 20, 10, 0.1, 15]})
                        
scaler = StandardScaler()
df_normal = scaler.fit_transform(df)
df_normal
```