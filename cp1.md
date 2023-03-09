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

# Conceitos Gerais da Linguagem Python


## A Linguagem Python

```{code-cell}
print('Primeiro código!')
```

## Tipos Básicos e Variáveis

```{code-cell}
valor_int = 3  # Tipo inteiro
valor_float = 1.5  # Tipo ponto flutuante
string = "Olá Mundo!"  # Tipo string
var_bool = True  # Tipo booleano

print(type(valor_int))
print(type(valor_float))
print(type(string))
print(type(var_bool))
```

```{code-cell}
valor_int  = 15
valor_float = 1.5
soma = valor_int + valor_float
print(soma)
print(type(soma))
```

```{code-cell}
# Conversão de um valor do tipo inteiro para um valor do tipo string
a = 100
b = str(a)
print(b)
print(type(b))

# Conversão de um valor do tipo float para um valor do tipo inteiro
c = 12.0
d = int(c)
print(d)
print(type(d))

# Conversão de um valor do tipo inteiro para um valor do tipo float
e = 10
f = float(e)
print(f)
print(type(f))
```

## Expressões

```{code-cell}
a = 10
b = 3

# Soma do valor de duas variáveis
soma = a + b

# Subtração do valor de duas variáveis
sub = a - b

# Divisão do valor de duas variáveis
div = a / b

# Multiplicação do valor de duas variáveis
mult = a * b

print('Soma: %d' % (soma))
print('Subtração: %d' % (sub))
print('Divisão: %d' % (div))
print('Multiplicação: %d' % (mult))
```

```{code-cell}
# Resto de uma divisão de um valor do tipo inteiro por outro valor do tipo inteiro
res = a % b

# Resultado de elevar um valor do tipo inteiro a outro valor do tipo inteiro
exp = a ** b

# Piso, parte inteira de uma divisão de valores do tipo inteiro
pis = a // b

print('Resto: %d' % (res))
print('Exponencial: %d' % (exp))
print('Piso da divisão: %d' % (pis))
```

```{code-cell}
nome = 'Robson'
idade = 28
altura = 1.85

print('Meu nome é %s, tenho %d anos e %.2f de altura!' % (nome, idade, altura))
```

```{code-cell}
aux_01 = 2
aux_02 = 3

print('O valor da primeira variável é {} enquanto o da segunda é {}'.format(aux_01, aux_02))
```

## Operadores Relacionais

```{code-cell}
a = 5
b = 2

c = a < b
d = a > b
e = a == b

print('Valor de c:', c)
print('Valor de d:', d)
print('Valor de e:', e)
print(a >= b)
print(b <= b)
```

```{code-cell}
a = 10
b = 2

a += b  # a + b
print(a)

a -= b  # a - b
print(a)

a /= b  # a / b
print(a)

a *= b  # a * b
print(a)
```

```{code-cell}
:tags: [remove-output]
nome = input('Qual seu nome? ')
idade = int(input('Qual sua idade? '))
altura = float(input('Qual sua altura? '))
peso = float(input('Qual seu peso? '))

print('Nome: %s' % nome)
print('Idade: %d' % idade)

imc = peso / ( altura * altura )
print('O IMC =', imc)
print('Muito abaixo do peso:', imc < 17)
print('Abaixo do peso normal:', imc >= 17 and imc < 18.5)
print('Peso dentro do normal:', imc >= 18.5 and imc < 25.0)
print('Acima do peso normal:', imc >= 25 and imc < 30)
print('Muito acima do peso:', imc >= 30)
```

## Comandos Condicionais - if

```{code-cell}
:tags: [remove-output]
nota = int(input('Digite uma nota: '))

if nota < 60:
    print('Reprovado')
else:
    print('Aprovado')
```

```{code-cell}
:tags: [remove-output]
nota = int(input('Digite uma nota: '))

if nota < 30:
    print('Reprovado')
elif nota <= 50:
    print('Recuperação')
else:
    print('Aprovado')
```

```{code-cell}
:tags: [remove-output]
n1 = int(input('Digite o primeiro número: '))
n2 = int(input('Digite o segundo número: '))

if n1 > n2:
    print(n1, 'maior que', n2)
elif n1 < n2:
    print('%d maior que %d' % (n2, n1))
else:
    print('Os números são iguais')
```

## Comandos de Repetição - while

```{code-cell}
:tags: [remove-output]
while (expressão condicional):
    bloco de comandos
```

```{code-cell}
contador = 1

while contador <= 5:
    print(contador)
    contador += 1
```

```{code-cell}
:tags: [remove-output]
nota = -1
while nota < 0 or nota > 10:
    nota = int(input('Digite uma nota válida de 0 a 10: '))

print('Nota: %s' % nota)
```

## Comandos de Repetição - for

```{code-cell}
:tags: [remove-output]
for (número de iterações):
    bloco de comandos
```

```{code-cell}
print(list(range(5)))
print(list(range(1, 5)))
print(list(range(2, 10, 3)))
```

```{code-cell}
for i in range(5):
    print('Bem-vindo ao curso de Python!')
```

```{code-cell}
for i in range(0, 5):
    print(i)
```

```{code-cell}
for i in range(0, 10, 2): # Varia de 0 a 10, mas aumentando de 2 em 2
    print(i)
```

```{code-cell}
:tags: [remove-output]
total = 0

for i in range(0, 5):
    num = int(input('Digite um num: '))
    total += num

print('Soma: %s' % (total))
print('Media: %s' % (total/5))
```

```{code-cell}
tab=int(input('Tabuada do número: '))

for i in range(10):
    print('%d x %d = %d' % (tab, i + 1, tab * (i + 1)))
```