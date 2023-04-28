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

## Estruturas de Dados - Listas

```{code-cell}
lista = []

print(lista)
```

```{code-cell}
lista_num = [1, 2, 3]
lista_nomes = ['André', 'Ângelo', 'Robson']
lista_misturada = [37, 'Ângelo', 'Robson', 9, 0, 'Maria', 2]

print(lista_num)
print(lista_nomes)
print(lista_misturada)
```

```{code-cell}
for item in lista_num:
    print(item)
    
print(lista_nomes[0])
print(lista_nomes[1])
```

```{code-cell}
lista = [1, 2, 3, 3, 3, 4, 5, 6, 7]

print('Tamanho da lista: %d' % len(lista))

lista.append(8)
print(lista)

lista.pop()
print(lista)

lista.pop(0)
print(lista)

lista.remove(2)
print(lista)
```

```{code-cell}
lista.insert(0, 1)  # Posição 0, item 1
print(lista)

print(lista.count(3))

print(lista.index(1))

lista.clear()
print(lista)
```

```{code-cell}
lista = [1, 2, 3, 4, 5]

print(lista[::-1])  # Inverte a ordem dos itens em uma lista

print(lista[0:2])  # Retorna os valores que estão nas posições do intervalo

print(lista[-1])  # Retorna o último item da lista

lista.sort()  %# Ordenar

print(lista)

lista[0] = 2  # Alterar item

print(lista)
```

## Estruturas de Dados - Tuplas

```{code-cell}
tupla_num = (1, 2, 3, 4)
print(tupla_num)
```

```{code-cell}
print(tupla_num.count(1))
print(tupla_num.index(2))
print(len(tupla_num))
```

```{code-cell}
# Convertendo tupla em lista
tupla_conv = list(tupla_num)

# Adicionando um item na última posição
tupla_conv.append(5)
print("Elemento 5 adicionado: %s" % (tupla_conv))

# Removendo o item adicionado
tupla_conv.pop()
print("Elemento 5 removido: %s" % (tupla_conv))
```

## Estruturas de Dados - Conjuntos

```{code-cell}
frutas = ['maçã', 'laranja', 'maçã', 'pera', 'laranja', 'banana']

frutas_n_duplicados = set(frutas)

print(frutas_n_duplicados)
```

## Estruturas de Dados - Dicionários

```{code-cell}
dic = {}  # chave e valor

dic = dict()

print(dic)
```

```{code-cell}
# Criação de um dicionário com preço de frutas por kg
dic_frutas = {'Maçã': 5.50,
              'Banana': 2.50,
              'Pera': 6.50,
              'Uva': 7.20}

# Inserção de um novo item, fruta e seu preço, em um dicionário
dic_frutas['Manga'] = 6.20

print(dic_frutas)
```

```{code-cell}
# Alteração do preço de uma fruta, banana, armazenada em um dicionário
dic_frutas['Banana'] = 1.50
print(dic_frutas)
```

```{code-cell}
# Consulta do preço de uma fruta armazenada em um dicionário
print('Uva: %s' % dic_frutas['Uva'])
print('Pera: %s' % dic_frutas['Pera'])
```

```{code-cell}
:tags: [remove-output]
# Criação de um dicionário com o desempenho de atletas
atletas = {}  

# Atribuição de um nome fictício, início, a um dicionário
nome_atleta = 'início'  

# Inclusão do nome de atletas em um dicionário
while nome_atleta != '': 
    nome_atleta = input('Atleta: ')
    if nome_atleta != '':
        saltos = []  # Lista para armazenar a distância dos saltos
        for i in range(0, 2):  # 2 saltos
            saltos.append(float(input('%s Salto: ' % (i + 1))))
        
        # Adicionando uma lista de saltos ao dicionário
        atletas[nome_atleta] = saltos 


# Percorrer um dicionário  retornando seus itens  
print('\nResultado Final:')
for nome_atleta, saltos in atletas.items():
    print('Atleta: %s' % nome_atleta)
    print('Saltos: ', str(saltos))
    print('Média dos Saltos: %.2f m' % (sum(saltos)/2))
```

```{code-cell}
:tags: [remove-output]
dic_frutas = {'Maçã': 5.50,
              'Banana': 2.50,
              'Pera': 6.50}

for fruta, valor in dic_frutas.items():
    print('Fruta: %s' % fruta)
    print('Preço: %s' % valor)
```

## Funções

```{code-cell}
:tags: [remove-output]
def nome_funcao(parametro_um, parametro_dois, ...):
    comando
    ...
    comando
    return dados
```

```{code-cell}
# Trecho que define a função
def soma(x, y, z):
    return x + y +z

# Trecho em que a função é chamada
s = soma(10, 20, 40)
print(s)
```

```{code-cell}
# Trecho que define a função
def par_impar(n):
    if n % 2 == 0:
        return True
    else:
        return False

# Trecho em que a função é chamada
print(par_impar(10))
print(par_impar(15))
```

```{code-cell}
# Trecho que define a função
def imc(nome, idade, altura, peso):
    print('Nome: %s' % nome)
    print('Idade : %d' % idade)
    
    imc = peso / ( altura * altura )
    print('O IMC = ', imc)

    if imc < 17:
        print('Muito abaixo do peso')
    elif imc >= 17 and imc < 18.5:
        print('Abaixo do peso normal')
    elif imc >= 18.5 and imc < 25.0:
        print('Peso dentro do normal')
    elif imc >= 25 and imc < 30:
        print('Acima do peso normal')
    else:
        print('Muito acima do peso')

# Trecho em que a função é chamada
imc('Robson', 28, 1.85, 98)
```

## Funções de Entrada e Saída

```{code-cell}
doc = open('doc.txt', 'r')
for linha in doc.readlines():
    print(linha)
doc.close()
```

```{code-cell}
# Utilização da função split() para decompor uma \textit{string} em uma lista.
doc = open('doc.txt', 'r')

# Decomposição do \textit{string} separando componentes por vírgulas para gerar um arquivo do tipo csv
for linha in doc:
    valores = linha.split(',')
    print(valores[0], valores[1], valores[2], valores[3])
```

```{code-cell}
doc = open('doc.txt', 'a')

# Adição de uma nova linha
doc.write('Iris, ')
doc.write('01414, ')
doc.write('8.5, ')
doc.write('7.5 ')
doc.write("\n")  # Quebra de linhas
doc.close()

# Leitura do arquivo com a nova linha
doc = open('doc.txt', 'r')
for linha in doc.readlines():
    print(linha)
doc.close()
```