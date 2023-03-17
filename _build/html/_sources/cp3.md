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


## Gráficos de Barras ou Colunas

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