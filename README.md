<h1 align="center">Análise das assinaturas e de churns da empresa</h1>

<h3 align="center">Pedro Henrique de Almeida Mascarenhas</h3>

## Problema

A empresa vem sofrendo cancelamentos de assinaturas entre 2016 até 2021. O objetivo desta análise é analisar os status das assinaturas e propor ideias para mudar essa situação.

## Tecnologia utilizada

- Python 3

## Metodologia e resultados

### Leitura dos dados preparados pela equipe de engenharia de dados

Aqui se abriu o arquivo de extenção .csv importando o módulo <i>csv</i>. Assim, foi possível ler os dados do arquivo <i>data-test-analytics.csv</i> e importá-los para uma matriz.
Também se importou o módulo <i>matplotlib</i> para construir os gráficos posteriormente. <br> OBS: É importante notar que neste caso, para o código funcionar, é necessário que 
o arquivo <i>data-test-analytics.csv</i> esteja na mesma pasta do progama.

``` python
# Importar o módulo csv
import csv

# Importar o módulo matplotlib
from matplotlib import pyplot as plt

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

### Construção do gráfico dos status das assinaturas em 2021

Uma vez armazenadas os status das assinaturas em <i>assinaturas_porcentagem</i>, se construiu um gráfico de pizza mostrando os status das assinaturas em 2021. <i>plt.legend</i>
é a função para configurar a legenda do gráfico; <i>plt.title</i>, o título do gráfico e <i>plt.show()</i>, mostra o gráfico. (Figura 1)

``` python
# Fazer gráfico de pizza dos status das assinaturas hoje

legenda = ['Ativas', 'Pausadas', 'Canceladas']
cores = ['lightgray', 'orchid', 'red']

plt.pie(assinaturas_porcentagem, labels = legenda, colors = cores, autopct='%1.2f%%') # Opções do gráfico de pizza

plt.legend(legenda, loc = 3) # Legenda do gráfico
plt.title('Situação das assinaturas até o fim de 2021 (Em porcentagem)') # Título do gráfico
plt.show() # Mostra o gráfico
```
 
### Calcular a quantidade de assinaturas canceladas entre os anos de 2016 e 2021

Aqui foi necessário fazer uma lista para armazenar a quantidade de cancelamentos por ano em <i>cancelamentos_por_ano</i>. Para fazer a separação de assinaturas canceladas, se
usou a estrutura de repetição <i>for</f> e a estrutura condicional <i>if matriz[j][8] == "canceled":</i>. Cada ítem de <i>cancelamentos_por_ano</i> foi aumentada dem 1 cada vez
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






