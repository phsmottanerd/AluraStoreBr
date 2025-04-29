# 🛍️ **ALURA STORE** - Análise de Vendas e Desempenho das Lojas 📊

## 🚀 **Objetivo do Projeto**

Sejam muito bem-vindos ao nosso **primeiro desafio** da **Especialização de Data Science** da Alura! 🎓

O **Desafio Alura Store** tem como objetivo ajudar o **Sr. João** a decidir **qual loja vender** de sua rede de e-commerce, para investir em um novo negócio.

Você foi contratado como **Analista de Dados** para analisar as **4 lojas** da **Alura Store**. O desafio é entender o **desempenho** de cada uma delas com base em várias métricas, como **faturamento**, **avaliação dos clientes**, **produtos vendidos** e **custo do frete**, e decidir qual delas tem o pior desempenho.

---

## 📋 **Descrição do Desafio**

**O Sr. João tem 4 lojas** e você deverá realizar as seguintes análises:

- **Faturamento Total** 📈
- **Categorias Mais Populares** 🛒
- **Média de Avaliação dos Clientes** ⭐
- **Produtos Mais e Menos Vendidos** 📦
- **Custo Médio do Frete** 💸

Com essas informações, você deverá criar gráficos e um relatório para apresentar qual loja o Sr. João deve vender para investir em um novo negócio.

---

## Conclusões
Descrição das análises...

## 🔧 **Ferramentas Usadas** 🛠️

Este projeto foi desenvolvido utilizando as seguintes ferramentas e bibliotecas:

- **Python** 🐍 (linguagem de programação principal)
- **Google Colab** ☁️ (ambiente para rodar o código)
- **Matplotlib** 📊 (para visualização de gráficos)
- **Pandas** 📈 (para manipulação dos dados)
- **GitHub** 💻 (para versionamento e controle de código)

---
# -*- coding: utf-8 -*-
"""AluraStoreBr.

Automaticamente gerado pelo Colab.

O arquivo original está localizado em:
    https://colab.research.google.com/drive/1B4fTLZK3V3GDsvQkFU9xEO3odB7ulLA8

### Importação dos dados
"""

import pandas as pd

url = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_1.csv"
url2 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_2.csv"
url3 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_3.csv"
url4 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_4.csv"

loja = pd.read_csv(url)
loja2 = pd.read_csv(url2)
loja3 = pd.read_csv(url3)
loja4 = pd.read_csv(url4)

loja.head()

"""# 1. Análise do faturamento"""

# Calcular o faturamento por categoria
loja['Faturamento'] = loja['Preço'] * loja['Quantidade de parcelas']

faturamento_por_categoria = loja.groupby('Categoria do Produto')['Faturamento'].sum()

# Exibir resultados
print(faturamento_por_categoria)

import matplotlib.pyplot as plt
import seaborn as sns

# Ajuste do tamanho do gráfico
plt.figure(figsize=(10, 6))

# Gráfico de barras
sns.barplot(x=faturamento_por_categoria.values, y=faturamento_por_categoria.index, palette="viridis")

# Título e rótulos
plt.title('Faturamento por Categoria de Produto')
plt.xlabel('Faturamento (R$)')
plt.ylabel('Categoria')

# Mostrar o gráfico
plt.tight_layout()
plt.show()

import matplotlib.pyplot as plt

# Dicionário com os dados que você me passou
faturamento = {
    'brinquedos': 65412.04,
    'eletrodomesticos': 1376501.27,
    'eletronicos': 1720525.18,
    'esporte e lazer': 169179.18,
    'instrumentos musicais': 369322.34,
    'livros': 34360.24,
    'moveis': 777243.14,
    'utilidades domesticas': 51092.72
}

# Plotando gráfico de pizza
plt.figure(figsize=(10, 10))
plt.pie(faturamento.values(), labels=faturamento.keys(), autopct='%1.1f%%', startangle=140)
plt.title('Faturamento por Categoria de Produto')
plt.axis('equal')  # Mantém o círculo redondo
plt.show()

import pandas as pd
import matplotlib.pyplot as plt

# Caso ainda não tenha unido todas as lojas:
loja_completa = pd.concat([loja, loja2, loja3, loja4])

# Garantir que os nomes das colunas não tenham espaços
loja_completa.columns = loja_completa.columns.str.strip()

# Converter avaliações para número (garantir que não há textos ou NaN)
loja_completa['Avaliação da compra'] = pd.to_numeric(loja_completa['Avaliação da compra'], errors='coerce')

# Calcular média por vendedor
avaliacao_media = loja_completa.groupby('Vendedor')['Avaliação da compra'].mean().sort_values(ascending=False)

# Exibir os valores no console
print(avaliacao_media)

import matplotlib.pyplot as plt

# Plotando gráfico de barras horizontais
plt.figure(figsize=(10, 6))
plt.barh(avaliacao_media.index, avaliacao_media.values, color='skyblue')
plt.xlabel('Média de Avaliação')
plt.title('Média de Avaliação por Vendedor')
plt.gca().invert_yaxis()  # Deixa o maior no topo
plt.grid(axis='x', linestyle='--', alpha=0.7)
plt.tight_layout()
plt.show()

"""# 4. Produtos Mais e Menos Vendidos"""

# Agrupando por produto e somando a quantidade de parcelas
produtos_vendidos = loja_completa.groupby('Produto')['Quantidade de parcelas'].sum().sort_values(ascending=False)

# Produtos mais vendidos (os que têm maior soma de quantidade de parcelas)
produtos_mais_vendidos = produtos_vendidos.head(10)

# Produtos menos vendidos (os que têm menor soma de quantidade de parcelas)
produtos_menos_vendidos = produtos_vendidos.tail(10)

# Exibindo os resultados
print("Produtos Mais Vendidos:")
print(produtos_mais_vendidos)

print("\nProdutos Menos Vendidos:")
print(produtos_menos_vendidos)

import matplotlib.pyplot as plt

# Dados dos produtos mais e menos vendidos
produtos_mais_vendidos = [
    ('Secadora de roupas', 649),
    ('Cômoda', 627),
    ('Pandeiro', 625),
    ('Bicicleta', 614),
    ('Celular Plus X42', 611),
    ('Cama king', 603),
    ('Jogo de panelas', 601),
    ('Micro-ondas', 600),
    ('Bateria', 595),
    ('Violão', 595)
]

produtos_menos_vendidos = [
    ('Guitarra', 497),
    ('Tablet ABXY', 497),
    ('Cubo mágico 8x8', 487),
    ('Boneca bebê', 474),
    ('Mochila', 471),
    ('Mesa de centro', 469),
    ('Dinossauro Rex', 458),
    ('Celular ABXY', 447),
    ('Jogo de copos', 435),
    ('Smartwatch', 432)
]

# Convertendo para listas
produtos_mais_vendidos_nome = [produto[0] for produto in produtos_mais_vendidos]
produtos_mais_vendidos_quantidade = [produto[1] for produto in produtos_mais_vendidos]

produtos_menos_vendidos_nome = [produto[0] for produto in produtos_menos_vendidos]
produtos_menos_vendidos_quantidade = [produto[1] for produto in produtos_menos_vendidos]

# Criando gráficos
fig, axes = plt.subplots(1, 2, figsize=(14, 7))

# Gráfico de barras horizontal para os mais vendidos
axes[0].barh(produtos_mais_vendidos_nome, produtos_mais_vendidos_quantidade, color='teal')
axes[0].set_title('Produtos Mais Vendidos')
axes[0].set_xlabel('Quantidade de Parcelas')
axes[0].invert_yaxis()  # Inverter para os maiores valores ficarem no topo

# Gráfico de barras horizontal para os menos vendidos
axes[1].barh(produtos_menos_vendidos_nome, produtos_menos_vendidos_quantidade, color='orange')
axes[1].set_title('Produtos Menos Vendidos')
axes[1].set_xlabel('Quantidade de Parcelas')
axes[1].invert_yaxis()  # Inverter para os menores valores ficarem no topo

# Ajustando o layout
plt.tight_layout()
plt.show()

"""# 5. Frete Médio por Loja"""

import pandas as pd

# Carregar os dados
url = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_1.csv"
url2 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_2.csv"
url3 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_3.csv"
url4 = "https://raw.githubusercontent.com/alura-es-cursos/challenge1-data-science/refs/heads/main/base-de-dados-challenge-1/loja_4.csv"

loja = pd.read_csv(url)
loja2 = pd.read_csv(url2)
loja3 = pd.read_csv(url3)
loja4 = pd.read_csv(url4)

# Juntar os dataframes
dados = pd.concat([loja, loja2, loja3, loja4])

# Calcular o frete médio por loja
frete_medio_por_loja = dados.groupby('Local da compra')['Frete'].mean()

# Mostrar o resultado
print(frete_medio_por_loja)

import matplotlib.pyplot as plt
import seaborn as sns

# Dados de frete médio por estado
dados_frete = {
    'GO': 38.129811,
    'MA': 35.129564,
    'MG': 33.460381,
    'MS': 33.694141,
    'MT': 32.691573,
    'PA': 30.633504,
    'PB': 33.910674,
    'PE': 35.720970,
    'PI': 36.948903,
    'PR': 34.569335,
    'RJ': 33.935633,
    'RN': 40.090987,
    'RO': 46.347240,
    'RR': 113.673032
}

# Gráfico de barras do frete médio por estado
plt.figure(figsize=(10, 6))
sns.barplot(x=list(dados_frete.keys()), y=list(dados_frete.values()), palette='Blues_d')
plt.title('Frete Médio por Estado')
plt.xlabel('Estado')
plt.ylabel('Frete Médio (R$)')
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

## 📝 **Passo a Passo para Execução**

1. **Clonar o Repositório**:
   - Para começar, clone este repositório:

   ```bash
   git clone https://github.com/SEU_USUARIO/AluraStore.git
