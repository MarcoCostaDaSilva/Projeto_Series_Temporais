# Projeto_Series_Temporais
Projeto autoral de ciência de dados focado em análise por séries temporais.

# Análise de Séries Temporais por Biomas Brasileiros [2000 – 2023].

## Objetivo:
O objetivo deste projeto é analisar o desmatamento nos diferentes biomas brasileiros ao longo dos anos, utilizando séries temporais para identificar padrões e tendências. A análise busca compreender as dinâmicas de desmatamento entre 2000 e 2023, oferecendo insights sobre a preservação ambiental.

# Base de Dados:

Os dados utilizados neste projeto são reais e foram extraídos da base **Desmatamento PRODES**, disponível no portal **Base dos Dados**. 

### Sobre a Base PRODES:
O **PRODES** (Projeto de Monitoramento do Desmatamento na Amazônia Legal por Satélite) é um sistema desenvolvido pelo **Instituto Nacional de Pesquisas Espaciais (INPE)**. Ele realiza monitoramento anual do desmatamento utilizando imagens de satélite, cobrindo os principais biomas brasileiros:  
- **Amazônia**  
- **Caatinga**  
- **Cerrado**  
- **Mata Atlântica**  
- **Pampa**  
- **Pantanal**

Link para acessar a base de dados:  
[Desmatamento PRODES - Base dos Dados](https://basedosdados.org/dataset/e5c87240-ecce-4856-97c5-e6b84984bf42?table=d7a76d45-c363-4494-826d-1580e997ebf0&utm_term=&utm_campaign=Conjuntos+de+dados+-+Gratuito&utm_source=adwords&utm_medium=ppc&hsa_acc=9488864076&hsa_cam=20482085189&hsa_grp=153440174377&hsa_ad=680653189837&hsa_src=g&hsa_tgt=dsa-2258116265900&hsa_kw=&hsa_mt=&hsa_net=adwords&hsa_ver=3&gad_source=1&gclid=Cj0KCQiA4rK8BhD7ARIsAFe5LXJy_x3GysEN36I6fnwN9vXZLE2mK4cMcWPPwRRAsmHb_v1idCCWPcoaAkZYEALw_wcB)  

---

## Técnica Estatística:
A técnica utilizada foi **Séries Temporais**, que permite analisar e visualizar a evolução do desmatamento ao longo dos anos. Essa abordagem ajuda a identificar padrões sazonais, tendências de longo prazo e variações específicas em cada bioma.

## Explicação Breve da Série Temporal:
Séries temporais são ferramentas poderosas para o estudo de dados dependentes do tempo. No contexto deste projeto, elas foram escolhidas porque:
1. Permitem uma análise cronológica clara dos impactos no desmatamento.
2. Ajudam a comparar tendências específicas entre os biomas.
3. Facilitam a visualização de períodos críticos para tomada de decisão em políticas públicas.

---

## Etapas da Análise:

### **1. Carregamento e Visualização dos Dados:**
Os dados foram carregados diretamente da planilha compartilhada via URL e processados no **Pandas**.  
```python
# Importar bibliotecas
import pandas as pd

# Carregar os dados
sheet_url = "https://docs.google.com/spreadsheets/d/1t2oGIqW4dmGaN9ndGHTwkJYDZNhbOYifywlSf5TSBi0/export?format=csv"
df = pd.read_csv(sheet_url)

# Visualizar as primeiras linhas
df.head()
```

### **2. Limpeza e Organização dos Dados:**
   
A etapa de limpeza e organização dos dados está organizada na seguinte ordem:

**2.1 - Ordenação Cronológica:**

Primeiro, os dados foram ordenados cronologicamente pela coluna ano, para garantir que as análises respeitem a sequência temporal dos eventos.

```python
# 1. Ordenar os dados cronologicamente pelo ano
df = df.sort_values(by='ano')

# Visualizar as primeiras linhas do DataFrame
df.head()
```

**2.2 - Verificação de Valores Faltantes:**

Em seguida, verificados se há valores faltantes nas colunas do DataFrame. 

```python
# Verificar se há valores faltantes em cada coluna
dados_faltantes = df.isnull().sum()
```

Exibir a quantidade de dados faltantes:
```python
print(dados_faltantes)
```

**2.3 - Verificação dos Tipos de Dados:**

```python
# Verificar os tipos de dados de cada coluna
tipos_de_dados = df.dtypes
```

Exibir os tipos de dados:
```python
print(tipos_de_dados)
```


**2.4 - Análise dos Biomas:**

Em seguida, verificados quais são os valores únicos na coluna bioma:

```python

# Exibir os valores únicos na coluna 'bioma'
biomas_unicos = df['bioma'].unique()

# Exibir os biomas
print(biomas_unicos)
```

**2.5 - Correção de Valores Inconsistentes:**

Depois de verificar os valores únicos, realizadas as correções necessárias, como no caso de valores que apresentaram caracteres estranhos.

```python
# Corrigir os valores da coluna 'bioma'
df['bioma'] = df['bioma'].replace({'Mata Atl��ntica': 'Mata Atlântica', 'Amaz��nia': 'Amazônia'})

# Verificar se a correção foi feita corretamente
biomas_unicos_corrigidos = df['bioma'].unique()
print(biomas_unicos_corrigidos)
```

**2.6 - Ordenação dos Biomas:**

Agora que os valores da coluna bioma foram corrigidos, os biomas de forma alfabética para facilitar a análise.

```python
# Ordenar os biomas por ordem alfabética
df['bioma'] = pd.Categorical(df['bioma'], categories=sorted(df['bioma'].unique()), ordered=True)

# Verificar a ordenação
biomas_ordenados = df['bioma'].unique()
print(biomas_ordenados)
```

**2.7 - Verificação dos Anos Presentes:**

Verificação dos anos presentes no conjunto de dados.

```python
# Verificar os anos presentes na planilha
anos_presentes = df['ano'].min(), df['ano'].max()

# Exibir os anos
print(f"A planilha abrange os anos de {anos_presentes[0]} até {anos_presentes[1]}.")
```

### **3. Visualização dos Dados**
   
A visualização ajuda a interpretar os dados de maneira mais intuitiva e a identificar tendências, variações e padrões ao longo do tempo. Neste projeto, o gráfico gerado mostra o desmatamento por bioma ao longo dos anos, com cada bioma representado por um gráfico diferente.

**3.1 - Importação das Bibliotecas:**

Primeiro, a biblioteca matplotlib é importada para criar os gráficos. O pyplot é utilizado para gerar as visualizações.

```python
import matplotlib.pyplot as plt
```

**3.2 - Definição das Cores para os Biomas:**

A variável biomas_cores define um dicionário com a correspondência entre cada bioma e uma cor específica. 

```python
# Lista dos biomas e cores para cada um
biomas_cores = {
    'Amazônia': 'green',
    'Caatinga': 'brown',
    'Cerrado': 'orange',
    'Mata Atlântica': 'blue',
    'Pampa': 'purple',
    'Pantanal': 'red'
}
```

**3.3 - Configuração do Layout do Gráfico:**

Utilizei o plt.subplots() para criar uma figura com múltiplos subgráficos (2 linhas e 3 colunas), onde cada subgráfico exibe o desmatamento de um bioma específico ao longo dos anos. O título geral é adicionado com fig.suptitle().

```python
# Configurar o layout: 2 linhas e 3 colunas
fig, axes = plt.subplots(2, 3, figsize=(15, 10))
fig.suptitle('Desmatamento por Bioma ao longo dos anos', fontsize=16)
```

**3.4 - Iteração pelos Biomas e Geração dos Gráficos:**

Através de um loop, cada gráfico é gerado para um bioma específico. Para isso, a função zip() é utilizada para combinar os eixos (axes.flatten()) com os biomas e cores, permitindo iterar por cada combinação e gerar os gráficos.

```python
# Iterar pelos biomas e gerar gráficos
for ax, (bioma, cor) in zip(axes.flatten(), biomas_cores.items()):
    # Filtrar os dados para o bioma atual
    dados_bioma = df[df['bioma'] == bioma]
    
    # Agrupar os dados por ano e calcular a média da área desmatada
    dados_bioma_agrupado = dados_bioma.groupby('ano')['desmatado'].mean().reset_index()
    
    # Criar o gráfico para o bioma
    ax.plot(dados_bioma_agrupado['ano'], dados_bioma_agrupado['desmatado'], linestyle='-', color=cor)
    ax.set_title(bioma)  # Título do gráfico
    ax.set_xlabel('Ano')  # Rótulo do eixo X
    ax.set_ylabel('Área Desmatada (km²)')  # Rótulo do eixo Y
    ax.grid(True, linestyle='--', alpha=0.7)  # Adicionar grade
    
    # Ajustar os ticks do eixo X
    ax.set_xticks(dados_bioma_agrupado['ano'][::2])  # Mostrar anos de 2 em 2
    ax.tick_params(axis='x', rotation=45)
```

Explicação do código:


**Filtragem dos Dados:** Para cada bioma, os dados são filtrados usando df[df['bioma'] == bioma]

**Agrupamento por Ano:** Os dados são agrupados por ano usando groupby('ano'), e a média da área desmatada ('desmatado') é calculada para cada ano com mean(). Isso nos dá uma visão geral do desmatamento por ano.

**Gráfico:** O método ax.plot() é utilizado para criar o gráfico. Para cada bioma, ele plota o ano no eixo X e a área desmatada no eixo Y, utilizando a cor definida no dicionário biomas_cores.

**Personalização do Gráfico:** O título de cada gráfico é configurado com ax.set_title(bioma), os eixos X e Y são rotulados com ax.set_xlabel() e ax.set_ylabel(), e uma grade é adicionada para facilitar a leitura do gráfico.

**Ajuste nos Ticks do Eixo X:** A configuração ax.set_xticks(dados_bioma_agrupado['ano'][::2]) exibe apenas os anos de 2 em 2 no eixo X, para tornar os gráficos mais legíveis. A rotação dos rótulos do eixo X é feita com ax.tick_params(axis='x', rotation=45).



**3.5 - Ajuste no Layout e Exibição do Gráfico:**


Para evitar que os gráficos se sobreponham, utilizei plt.tight_layout(), que ajusta automaticamente o espaço entre os gráficos. O parâmetro rect=[0, 0, 1, 0.95] garante que o título geral do gráfico não sobreponha os subgráficos.

```python
# Ajustar o layout para evitar sobreposição
plt.tight_layout(rect=[0, 0, 1, 0.95])  # Deixar espaço para o título geral
plt.show()
```

Grafico 1: Desmatamento por bioma ao longo dos anos.

![Gráfico1](https://drive.google.com/uc?export=view&id=160EqYa-GMHZEgeXEbOe8wv4Y7HN1VEzr)



```python
plt.figure(figsize=(12, 8))
plt.title('Desmatamento por Bioma ao longo dos anos', fontsize=16)
plt.xlabel('Ano', fontsize=12)
plt.ylabel('Área Desmatada (km²)', fontsize=12)
```

- Cada bioma é analisado individualmente, permitindo identificar tendências específicas.
  
- **Amazônia** e **Pantanal** destacam-se com as maiores áreas desmatadas ao longo dos anos, ambos ultrapassando 1.300 km² em 2022.
  
- **Caatinga, Cerrado e Mata Atlântica** apresentam tendências crescentes, mas com áreas desmatadas menores em relação à Amazônia e Pantanal.

- **Pampa** apresenta uma curva de crescimento mais lenta, mas ainda contínua.
  

 **3.6 - Segundo Gráfico:**



**plt.figure(figsize=(12, 8)):** Define o tamanho da figura do gráfico, sendo 12 unidades de largura e 8 de altura.

**plt.title(...):** Adiciona o título ao gráfico, que descreve o que está sendo mostrado, no caso, o desmatamento por bioma ao longo dos anos. 

**plt.xlabel(...) e plt.ylabel(...):** Adicionam rótulos aos eixos X e Y, indicando que o eixo X representa os anos e o eixo Y a área desmatada em quilômetros quadrados.


**Iteração sobre os Biomas:**

```python
for bioma, cor in biomas_cores.items():
    # Filtrar os dados para o bioma atual
    dados_bioma = df[df['bioma'] == bioma]
    
    # Agrupar os dados por ano e calcular a média da área desmatada
    dados_bioma_agrupado = dados_bioma.groupby('ano')['desmatado'].mean().reset_index()
    
    # Adicionar a linha do bioma ao gráfico
    plt.plot(
        dados_bioma_agrupado['ano'], 
        dados_bioma_agrupado['desmatado'], 
        label=bioma, 
        color=cor, 
        linestyle='-'
    )
```


- O **loop for bioma, cor in biomas_cores.items()**: percorre todos os biomas e suas respectivas cores (armazenadas no dicionário biomas_cores).

  
- **dados_bioma = df[df['bioma'] == bioma]:** Filtra os dados do DataFrame para incluir apenas as linhas correspondentes ao bioma atual.
  
- **dados_bioma_agrupado = dados_bioma.groupby('ano')['desmatado'].mean().reset_index():** Agrupa os dados filtrados por ano e calcula a média da área desmatada para cada ano.
  
- **plt.plot(...):** Adiciona uma linha ao gráfico para cada bioma. A linha é desenhada com os valores do ano no eixo X e da área desmatada no eixo Y, com a cor e o estilo definidos pelas variáveis cor e '-' (linha contínua).



Legenda e Grade: 

```python
plt.legend(title='Biomas', loc='upper right', fontsize=10)
plt.grid(True, linestyle='--', alpha=0.7)
```

- **plt.legend(...):** Adiciona uma legenda ao gráfico, indicando os biomas representados pelas linhas. A legenda está posicionada no canto superior direito (loc='upper right'), com o título "Biomas" e a fonte de tamanho 10.
  
- **plt.grid(True, linestyle='--', alpha=0.7):** Ativa a grade do gráfico, usando linhas tracejadas ('--') e com transparência de 70% (alpha=0.7).


Ajuste dos Ticks do Eixo X
```python
plt.xticks(dados_bioma_agrupado['ano'][::2], rotation=45)
```

- **plt.xticks(...):** Ajusta os ticks do eixo X para mostrar apenas a cada dois anos ([::2]), e rotaciona os rótulos dos anos em 45 graus para melhorar a legibilidade.

Exibir o Gráfico:
```python
plt.tight_layout()
plt.show()
```

- **plt.tight_layout():** Ajusta automaticamente os elementos do gráfico (como título, legendas, eixos) para evitar sobreposição e garantir que o gráfico se ajuste bem dentro da área disponível.
  
- **plt.show():** Exibe o gráfico na tela.


![Grafico2](https://drive.google.com/uc?export=view&id=1rwrPlU9V7O-CCsQVYBznDc8FRqz6X1QN)


- A **Amazônia** lidera em área desmatada, seguida pelo Pantanal.
  
- O **Cerrado** ocupa uma posição intermediária, enquanto **Mata Atlântica, Caatinga e Pampa** apresentam níveis mais baixos de desmatamento.
  
- A visualização conjunta reforça as disparidades entre os biomas e a necessidade de ações específicas para cada região.


## **Conclusões:**

Ambos os gráficos evidenciam um crescimento contínuo no desmatamento em todos os biomas analisados, o que sugere a intensificação da pressão sobre os recursos naturais ao longo dos anos. A Amazônia, por sua vasta extensão e importância ecológica, apresenta o maior desafio de preservação. A visualização combinada fornece uma base sólida para priorizar políticas públicas e medidas de conservação em regiões mais vulneráveis.











