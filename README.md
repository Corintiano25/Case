<h1 align="center">Análise das assinaturas e de churns da empresa</h1>

<h3 align="center">Pedro Henrique de Almeida Mascarenhas</h3>

Tabela de conteúdos
=================
<!--ts-->
   * 1 - Problema
   * 2 - Tecnologia utilizada
   * 3 - Análise do código e resultados
      * 3.1 - Função para construir gráficos de barras
      * 3.2 - Leitura dos dados preparados pela equipe de engenharia de dados
      * 3.3 - Analise dos status das assinaturas
      * 3.4 - Gráfico de pizza mostrando as situações das assinaturas
      * 3.5 - Calcular a quantidade de assinaturas canceladas entre os anos de 2016 e 2021
      * 3.6 - Calcular a quantidade de churns por ano
      * 3.7 - Separando os cancelamentos das assinaturas por faixa etária dos assinantes
      * 3.8 - Calcular os churns por mês, no ano de 2020, onde houve maior quantidade de cancelamentos de assinaturas
      * 3.9 - Análise das assinaturas pausadas
      * 3.10 - Construção dos gráficos de barras

<!--te-->

## Problema

A empresa vem sofrendo cancelamentos de assinaturas entre 2016 até 2021. O objetivo desta análise é analisar os status das assinaturas e propor ideias para mudar essa situação.

## Tecnologia utilizada

- Python 3

## Análise do código e resultados

### Função para construir gráficos de barras

Como foram criados 4 gráficos de barras no código, se criou uma função que constrói gráficos de barras de cor vermelha. Para isso, se importou <i>matplotlib</i>. A função chama na ordem: Dados do eixo x (de prefêrencia uma lista), dados do eixo y (de prefêrencia uma lista), legenda do eixo x (uma string), legenda do eixo y (uma string) e o título do gráfico (uma string).

```python
# Importar o módulo matplotlib
from matplotlib import pyplot as plt

# Função para fazer um gráfico de barras

def barras(eixo_x, eixo_y, legenda_x, legenda_y, titulo_do_grafico):
    plt.bar(eixo_x, eixo_y, color = 'red') # Opções do gráfico de barras
    plt.xlabel(legenda_x) # Legenda do eixo x
    plt.ylabel(legenda_y) # Legenda do eixo y
    plt.title(titulo_do_grafico) # Título do gráfico
    plt.show() # Mostra o gráfico
```

### Leitura dos dados preparados pela equipe de engenharia de dados

Aqui se abriu o arquivo de extenção .csv importando o módulo <i>csv</i>. Assim, foi possível ler os dados do arquivo <i>data-test-analytics.csv</i> e importá-los para uma matriz.
<br> OBS: É importante notar que neste caso, para o código funcionar, é necessário que 
o arquivo <i>data-test-analytics.csv</i> esteja na mesma pasta do progama.

``` python
# Importar o módulo csv
import csv

# Ler o arquivo feito pela engenharia de dados
with open('data-test-analytics.csv', encoding = 'cp850') as csv_file:
    
    csv_reader = csv.reader(csv_file, delimiter=',')

    csv_reader.__next__()

    # Matriz para adicionar os dados
    matriz = []
    contador = 0
    
    # Adicona linha a linha na matriz de dados
    for row in csv_reader:
        matriz.append(row)
```

### Analise dos status das assinaturas

Após a construção da matriz com os dados, se armazenou as porcentagens de assinaturas ativas, canceladas e pausadas na lista <i>assinaturas_porcentagem</i>. As porcentagens
foram armazenadas até a segunda casa decimal, através da função <i>round</i>.

``` python

# Numero de assinantes iniciais
assinantes_iniciais = len(matriz)

# Ver as porcentagens de assinaturas ativas e pausadas

assinaturas_porcentagem = [0, 0, 0] # A primeira dimensão são as assinaturas ativas. A segunda, as pausadas e a terceira, as canceladas

for i in range(assinantes_iniciais):
    if matriz[i][8] == "active":
        assinaturas_porcentagem[0] = assinaturas_porcentagem[0] + 1 # Calcula o número de assinaturas ativas

    elif matriz[i][8] == "canceled":
        assinaturas_porcentagem[2] = assinaturas_porcentagem[2] + 1 # Calcula o número de assinaturas canceladas
        
    else:
        assinaturas_porcentagem[1] = assinaturas_porcentagem[1] + 1 # Calcula o número de assinaturas pausadas

# Calcular em porcentagem, até a segunda casa decimal
assinaturas_porcentagem[0] = round((assinaturas_porcentagem[0] / assinantes_iniciais) * 100, 2)
assinaturas_porcentagem[1] = round((assinaturas_porcentagem[1] / assinantes_iniciais) * 100, 2)
assinaturas_porcentagem[2] = round((assinaturas_porcentagem[2] / assinantes_iniciais) * 100, 2)

print('Em porcentagem, as assinaturas ativas são', assinaturas_porcentagem[0], ', de canceladas', assinaturas_porcentagem[2], 'e de porcentagem pausadas,', assinaturas_porcentagem[1])
```

### Gráfico de pizza mostrando as situações das assinaturas

Após separar as situações das assinaturas, se construiu um gráfico de pizzas apra ilustar melhor a situação atual. Em <i>legendas</i> se colocou as legendas de cada item e em <i>cores</i>, as cores de cada pizza. Em <i>plt.tilte</i> se coloca o título do gráfico.

``` python
# Fazer gráfico de pizza dos status das assinaturas hoje

legenda = ['Ativas', 'Pausadas', 'Canceladas']
cores = ['lightgray', 'orchid', 'red']

plt.pie(assinaturas_porcentagem, labels = legenda, colors = cores, autopct='%1.2f%%') # Opções do gráfico de pizza

plt.legend(legenda, loc = 3) # Legenda do gráfico
plt.title('Situação das assinaturas até o fim de 2021 (Em porcentagem)') # Título do gráfico
plt.show() # Mostra o gráfico
```

<div align="center">
<img src="https://user-images.githubusercontent.com/99688544/153971766-64764669-f863-4e79-ae0c-7061fa3381e9.png" width="700px" />
</div>
<div align="center" >Figura 1. Imagem do gráfico de pizza mostrando a situação das assinaturas.</div>
```

### Calcular a quantidade de assinaturas canceladas entre os anos de 2016 e 2021

Aqui foi necessário fazer uma lista para armazenar a quantidade de cancelamentos por ano em <i>cancelamentos_por_ano</i>. Para fazer a separação de assinaturas canceladas, se
usou a estrutura de repetição <i>for</i> e a estrutura condicional <i>if matriz[j][8] == "canceled":</i>. Cada ítem de <i>cancelamentos_por_ano</i> foi aumentada dem 1 cada vez
que o ano de cancelamento da assinatura era lido.

``` python
# Adicionar os cancelamentos em uma lista

cancelamentos_ano = [] # Lista dos cancelamentos por ano (entre 2016 até 2021)

for l in range(6):
    cancelamentos_ano.append(0)
    
# Ver quantas assinaturas foram canceladas em cada ano
can = 0
for j in range(assinantes_iniciais):
    if matriz[j][8] == "canceled":

        if matriz[j][3][6] == '1': # Se for cancelada entre 2016 a 2019

            if matriz[j][3][7] == '6':
                cancelamentos_ano[0] = cancelamentos_ano[0] + 1

            elif matriz[j][3][7] == "7":
                cancelamentos_ano[1] = cancelamentos_ano[1] + 1

            elif matriz[j][3][7] == "8":
                cancelamentos_ano[2] = cancelamentos_ano[2] + 1

            elif matriz[j][3][7] == "9":
                cancelamentos_ano[3] = cancelamentos_ano[3] + 1

        elif matriz[j][3][6] == '2': # Se for cancelada entre 2020 a 2021

            if matriz[j][3][7] == "0":
                cancelamentos_ano[4] = cancelamentos_ano[4] + 1

            elif matriz[j][3][7] == "1":
                cancelamentos_ano[5] = cancelamentos_ano[5] + 1
   
print("Total de assinaturas canceladas:", assinaturas_porcentagem[2] * 100, "e cancelamentos por ano (entre 2016, até o final de 2021): ", cancelamentos_ano)
```
### Calcular a quantidade de churns por ano

Para analisar a quantidade de churns por ano, se criou uma lista de 6 elementos para armazenar a quantidade de churns <i>churn_ano</i> (em porcentagem). Sempre tomando o cuidado
de atualizar a quantidade de assinaturas ativas no ano seguinte.

``` pyhton
# Calcular os Churn por ano

churn_ano = [] # Lista dos churn por ano (entre 2016 até 2021)

for m in range(6): # Adicionar 6 zeros na lista
    churn_ano.append(0)

# Copiar a variavel em uma nova para não perder a original
assinantes = assinantes_iniciais

# Ver quantos Churns tivemos por ano (em porcentagens)

for n in range(len(churn_ano)): # Calcula os churns
    churn_ano[n] = round((cancelamentos_ano[n] / assinantes) * 100 , 2)
    assinantes = assinantes - cancelamentos_ano[n] # Atualiza o número de assinantes ativos

print('Porcentagem de churns por ano (entre 2016, até o final de 2021):' , churn_ano)
```

### Separando os cancelamentos das assinaturas por faixa etária dos assinantes

Primeiramente se criou a lista <i>cancelamento_faixa_etaria</i> de 7 valores para armazenar a quantidade de pessoas que cancelaram suas assinaturas por faixa etária (1940_1949,
1950_1959, 1960_1969, 1970_1979, 1980_1989, 1990_1999 e 2000 - em diante, respectivamente). Para fazer isto, foi montada uma estrutura de repetição <i>for</i> para procuar as assinaturas caneladas em 2020 e adicioná-las na matriz <i>canceladas_2020</i>.

``` python
# Faixa etária dos assinantes que cancelaram no ano de maior de maior cancelamento de assinaturas (2020)

# Criar uma lista os cancelamentos no ano de maior cancelamento de assinaturas (2020), por faixa estaria (1940_1949, 1950_1959, 1960_1969, 1970_1979, 1980_1989, 1990_1999 e 
2000 - em diante, respectivamente)
cancelamento_faixa_etaria =[]

for o in range(7):
    cancelamento_faixa_etaria.append(0)    

canceladas_2020 =[] # Matriz com as assinaturas que foram canceladas em 2020

for p in range(assinantes_iniciais):
    if matriz[p][8] == "canceled": # Procurar as assinaturas canceladas
        
        # Encontrar as assinaturas que foram cancelas em 2020
        if matriz[p][3][6] == '2' and matriz[p][3][7] == "0":
            canceladas_2020.append(matriz[p])

```

Aqui se criou uma estrutura <i>for</i> para encontrar as décadas de nascimentos de todos aqueles que cancelaram suas assinaturas em 2020 e adicioná-los na lista <i>cancelamento_faixa_etaria</i>.

```python
for k in range(len(canceladas_2020)):

    # Separar por ano de nascimento

    if canceladas_2020[k][7][6] == '4': # Ver se nasceu entre 1940 - 1949
        cancelamento_faixa_etaria[0] = cancelamento_faixa_etaria[0] + 1
        
    elif canceladas_2020[k][7][6] == '5': # Ver se nasceu entre 1950 - 1959
        cancelamento_faixa_etaria[1] = cancelamento_faixa_etaria[1] + 1
        
    elif canceladas_2020[k][7][6] == '6': # Ver se nasceu entre 1960 - 1969
        cancelamento_faixa_etaria[2] = cancelamento_faixa_etaria[2] + 1
        
    elif canceladas_2020[k][7][6] == '7': # Ver se nasceu entre 1970 - 1979
        cancelamento_faixa_etaria[3] = cancelamento_faixa_etaria[3] + 1

    elif canceladas_2020[k][7][6] == '8': # Ver se nasceu entre 1980 - 1989
        cancelamento_faixa_etaria[4] = cancelamento_faixa_etaria[4] + 1

    elif canceladas_2020[k][7][6] == '9': # Ver se nasceu entre 1990 - 1999
        cancelamento_faixa_etaria[5] = cancelamento_faixa_etaria[5] + 1

print('Cancelamentos por faixa etaria:', cancelamento_faixa_etaria)
```

### Calcular os churns por mês, no ano de 2020, onde houve maior quantidade de cancelamentos de assinaturas

Para fazer esta estapa, se criou a lista <i>churn_2020</i>, com 12 espaços. E para as preencher, foi usado uma estrutura de repetição <i>for</i> onde se procurou e adicionou os meses em que as assinaturas foram canceladas na lista <i>churn_2020</i>. 

``` python
# Mostrar os chruns por mês no ano de 2020

# Número de assinaturas até janeiro de 2020 (Assinaturas inicias - os cancelamentos de assinaturas entre 2016 até 2019)
ativas_2020 = assinantes_iniciais - cancelamentos_ano[3] - cancelamentos_ano[2] - cancelamentos_ano[1] - cancelamentos_ano[0] 

churn_2020 = []

for q in range(12):
    churn_2020.append(0)

# Calcular os cancelamentos por mês que aconteceram em 2020

for r in range(len(canceladas_2020)):

    if canceladas_2020[r][7][0] == '0': # Se estiver entre janeiro e setembro

        if canceladas_2020[r][7][1] == '1': # Janeiro
            churn_2020[0] = churn_2020[0] + 1

        elif canceladas_2020[r][7][1] =='2': # Fevereiro
            churn_2020[1] = churn_2020[1] + 1

        elif canceladas_2020[r][7][1] == '3': # Março
            churn_2020[2] = churn_2020[2] + 1

        elif canceladas_2020[r][7][1] == '4': # Abril
            churn_2020[3] = churn_2020[3] + 1

        elif canceladas_2020[r][7][1] == '5': # Maio
            churn_2020[4] = churn_2020[4] + 1

        elif canceladas_2020[r][7][1] == '6': # Junho
            churn_2020[5] = churn_2020[5] + 1

        elif canceladas_2020[r][7][1] == '7': # Julho
            churn_2020[6] = churn_2020[6] + 1

        elif canceladas_2020[r][7][1] == '8': # Agosto
            churn_2020[7] = churn_2020[7] + 1

        elif canceladas_2020[r][7][1] == '9': # Setembro
            churn_2020[8] = churn_2020[8] + 1

    elif canceladas_2020[r][7][0] == '1': # Se estiver entre outubro e dezembro
    
        if canceladas_2020[r][7][1] == '0': # Outubro
            churn_2020[9] = churn_2020[9] + 1

        elif canceladas_2020[r][7][1] == '1': # Novembro
            churn_2020[10] = churn_2020[10] + 1

        elif canceladas_2020[r][7][1] == '2': # Dezembro
            churn_2020[11] = churn_2020[11] + 1

```

Os churns por mês foram calculados aqui, já que antes foram apenas calculados os cancelamentos. Para se calcular os churns, se armazenou as assinaturas ativas até o final de 2019 em <i>ativas_2020</i>. Para se calcular os churns, se utilizou uma estrutura de repetição, sempre lembrando de atualizar o número de assinaturas ativas em cada mês. 

```python

cancel_2020 = 0 # Canceladas em cada mês de 2020

# Calcular os chruns de 2020
for i in range(len(churn_2020)):
    cancel_2020 = churn_2020[i] # Atualiza os cancelamentos por mês
    churn_2020[i] = 100 * (churn_2020[i] / ativas_2020)
    ativas_2020 = ativas_2020 - cancel_2020
```

### Análise das assinaturas pausadas

Na última parte do código, se analiza as assinaturas que foram pausadas. Para isso, se criou duas matrizes para armazenar as assinaturas que foram pausadas, com cada uma contendo 12 espaços. Em seguida se criou um mecanismo para procurar as assinaturas que foram pausadas no ano de 2021 <i>if matriz[t][8] == "paused" and matriz[t][13][6] == '2' and matriz[t][13][7] == '1':</i>, procurando as assinaturas pausadas em cada mês.

```python
# Listas para armazenar as assinaturas pausadas
pausadas_2020 = []
pausadas_2021 = []

for s in range(12):
    pausadas_2020.append(0)
    pausadas_2021.append(0)

# Ver os mêses que foram a ultima compra das assinaturas pausadas
for t in range(assinantes_iniciais):

    if matriz[t][8] == "paused" and matriz[t][13][6] == '2' and matriz[t][13][7] == '1': # Última compra em 2021

        if matriz[t][13][0] == '0':

            if matriz[t][13][1] == '1': # Janeiro
                pausadas_2021[0] = pausadas_2021[0] + 1

            elif matriz[t][13][1] == '2': # Fevereiro
                pausadas_2021[1] = pausadas_2021[1] + 1

            elif matriz[t][13][1] == '3': # Março
                pausadas_2021[2] = pausadas_2021[2] + 1

            elif matriz[t][13][1] == '4': # Abril
                pausadas_2021[3] = pausadas_2021[3] + 1

            elif matriz[t][13][1] == '5': # Maio
                pausadas_2021[4] = pausadas_2021[4] + 1

            elif matriz[t][13][1] == '6': # Junho
                pausadas_2021[5] = pausadas_2021[5] + 1

            elif matriz[t][13][1] == '7': # Julho
                pausadas_2021[6] = pausadas_2021[6] + 1

            elif matriz[t][13][1] == '8': # Agosto
                pausadas_2021[7] = pausadas_2021[7] + 1

            elif matriz[t][13][1] == '9': # Setembro
                pausadas_2021[8] = pausadas_2021[8] + 1

        elif matriz[t][13][0] == '1':
        
            if matriz[t][13][1] == '0': # Outubro
                pausadas_2021[9] = pausadas_2021[9] + 1

            elif matriz[t][13][1] == '1': # Novembro
                pausadas_2021[10] = pausadas_2021[10] + 1

            elif matriz[t][13][1] == '2': # Dezembro
                pausadas_2021[11] = pausadas_2021[11] + 1
```

Aqui se realizou um mecanismo semelhante ao anterior, mas para procurar as assinaturas pausadas no ano de 2020.

```python
    if matriz[t][8] == "paused" and matriz[t][13][6] == '2' and matriz[t][13][7] == '0': # Última compra em 2020

        if matriz[t][13][0] == '0':

            if matriz[t][13][1] == '1': # Janeiro
                pausadas_2020[0] = pausadas_2020[0] + 1

            elif matriz[t][13][1] == '2': # Fevereiro
                pausadas_2020[1] = pausadas_2020[1] + 1

            elif matriz[t][13][1] == '3': # Março
                pausadas_2020[2] = pausadas_2020[2] + 1

            elif matriz[t][13][1] == '4': # Abril
                pausadas_2020[3] = pausadas_2020[3] + 1

            elif matriz[t][13][1] == '5': # Maio
                pausadas_2020[4] = pausadas_2020[4] + 1

            elif matriz[t][13][1] == '6': # Junho
                pausadas_2020[5] = pausadas_2020[5] + 1

            elif matriz[t][13][1] == '7': # Julho
                pausadas_2020[6] = pausadas_2020[6] + 1

            elif matriz[t][13][1] == '8': # Agosto
                pausadas_2020[7] = pausadas_2020[7] + 1

            elif matriz[t][13][1] == '9': # Setembro
                pausadas_2020[8] = pausadas_2020[8] + 1

        elif matriz[t][13][0] == '1':
        
            if matriz[t][13][1] == '0': # Outubro
                pausadas_2020[9] = pausadas_2020[9] + 1

            elif matriz[t][13][1] == '1': # Novembro
                pausadas_2020[10] = pausadas_2020[10] + 1

            elif matriz[t][13][1] == '2': # Dezembro
                pausadas_2020[11] = pausadas_2020[11] + 1   
 ```

### Construção dos gráficos de barras

Uma vez construida a função <i>barras</i>, foi só preciso chamar a função para construir os gráficos: Qquantidade de chunrs entre 2016 e 2021 (Figura 2), Cancelamentos realizados por 2020, por faixa etária (Figura 3), Churns por mês em em 2020  (Figura 4) e Churns em 2020, em cada mês (Figura 5).

``` python
# Fazer um gráfico de barras para mostrar os churns por ano

anos = ["2016", "2017", "2018", "2019", "2020", "2021"] # Eixo x
# A variavel churn_ano vai ser o eixo y

barras(anos, churn_ano, 'Ano', 'Churn (Em porcentagem)', 'Quantidade de chruns entre 2016 até o fim de 2021')

```
<div align="center">
<img src="https://user-images.githubusercontent.com/99688544/153973782-f9623d62-543f-47b2-83c9-fdfc2b88b70c.png" width="700px" />
</div>
<div align="center" >Figura 2. Imagem do gráfico de barra mostrando a quantidade de chunrs entre 2016 e 2021.</div>


```python
# Fazer um gráfico de barras para mostrar os cancelamentos por faixa etária no ano de maior churn (2020)

faixa_etaria = ["1940-1949", "1950-1959", "1960-1969", "1970-1979", "1980-1989" , "1990-1999", "2000, para cima"] # Eixo x
# A variacel cancelamentos_ano vai ser o eixo y

barras(faixa_etaria, cancelamento_faixa_etaria, 'Faixa etária', 'Assinaturas canceladas', 'Quantidade de cancelamentos em 2020 por faixa etária')

```
<div align="center">
<img src="https://user-images.githubusercontent.com/99688544/153978718-964bc6e4-11b4-4a42-a31b-785df5f90228.png" />
</div>
<div align="center" >Figura 3. Imagem do gráfico de barra mostrando os cancelamentos realizados por 2020, por faixa etária.</div>

```python
# Fazer um gráfico de barras para mostrar os churns por mês no ano de maior churn (2020)

meses = ["Jan", "Fev", "Mar", "Abr", "Mai", "Jun", "Jul", "Ago", "Set", "Out", "Nov", "Dez"] # Eixo x
# A variacel cancelamentos_ano vai ser o eixo y

barras(meses, churn_2020, 'Meses', 'Churn (Em porcentagem)', 'Quantidade de chruns por mês, no ano de 2020')

```
<div align="center">
<img src="https://user-images.githubusercontent.com/99688544/153981320-c5e1ac59-e9f7-46bf-ac28-776ec630f8ed.png" />
</div>
<div align="center" >Figura 4. Imagem do gráfico de barra mostrando os churns em 2020, em cada mês.</div>

```python
meses = ["Dez-2020", "Jan-2021"] # Eixo x
# A variavel cancelamentos_ano vai ser o eixo y

barras(meses, (pausadas_2020[11], pausadas_2021[0]), 'Meses', 'Assinaturas pausadas', 'Quantidade de assinaturas pausadas por mês')
```

<div align="center">
<img src="https://user-images.githubusercontent.com/99688544/153984426-2ad70d95-41ac-4e0f-9036-c0c6c11cd499.png" />
</div>
<div align="center" >Figura 5. Imagem do gráfico de barra mostrando as assinaturas pausadas em 2020 e 2021.</div>

