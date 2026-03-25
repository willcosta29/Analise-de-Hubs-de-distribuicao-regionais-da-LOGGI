# Análise Exploratória de Dados Logísticos - Loggi (Distrito Federal)

Este projeto realiza uma análise exploratória e um processo completo de **Data Wrangling** sobre dados logísticos da startup Loggi, com foco nas operações no Distrito Federal (DF).

A análise utiliza dados geoespaciais para cruzar informações de entregas com dados demográficos (população, densidade urbana) das Regiões Administrativas (RAs) do DF. O objetivo é avaliar a eficiência da infraestrutura de distribuição existente, considerando a alta variação na densidade demográfica entre as três regiões atendidas (DF-0, DF-1 e DF-2).

## 🎯 Contexto e Objetivos

O principal objetivo é analisar os dados de entrega da Loggi no Distrito Federal para avaliar a eficiência da infraestrutura de distribuição. A análise foca em responder às seguintes questões de negócio:

1.  **A quantidade de HUBs de distribuição está subdimensionada?**
2.  **A distribuição de entregas por HUB está equilibrada?**

## 📂 Fontes de Dados

Para esta análise, foram utilizadas as seguintes fontes de dados:

* `deliveries.json`: Arquivo JSON contendo dados brutos das entregas, incluindo informações aninhadas sobre a origem (HUB) e os pontos de entrega.
* `distrito-federal.shp`: Shapefile com os limites geográficos do Distrito Federal.
* `bairros-df.shp` / `bairros-df.shx`: Shapefiles com os polígonos das Regiões Administrativas (RAs) do DF.
* `dimRA.xlsx`: Planilha Excel com dados demográficos e de área das Regiões Administrativas (População, Densidade Urbana, etc.).

## 🛠️ Tecnologias e Bibliotecas

O projeto foi desenvolvido em Python, utilizando as seguintes bibliotecas principais:

* **Pandas:** Para manipulação e análise de dados tabulares.
* **GeoPandas:** Para manipulação e análise de dados geoespaciais.
* **Matplotlib** & **Seaborn:** Para visualização de dados (gráficos e mapas).
* **Tabula-py** & **PyPDF2:** Para extração de dados de arquivos PDF (conforme listado na instalação).
* **Google Colab:** Como ambiente de desenvolvimento da análise.

## 📈 Metodologia da Análise

O projeto foi estruturado nas seguintes etapas:

1.  **Carregamento dos Dados:** Leitura do arquivo principal `deliveries.json`.
2.  **Data Wrangling (Limpeza e Transformação):**
    * Desaninhamento (flattening) dos dados JSON das colunas `origin` (HUBs) e `deliveries` (entregas).
    * Utilização do método `pd.json_normalize` para a coluna `origin`.
    * Utilização do método `explode` para expandir a lista de entregas, criando uma linha por entrega.
    * Extração de informações detalhadas de cada entrega (tamanho, ID, lat/lng).
    * Junção (merge) dos dados transformados em um DataFrame limpo e estruturado.
3.  **Análise Exploratória (EDA):**
    * Verificação de tipos de dados, valores nulos e valores únicos.
    * Análise estatística descritiva das variáveis (ex: `delivery_size`, `vehicle_capacity`).
4.  **Manipulação Geoespacial:**
    * Carregamento dos shapefiles do DF e das Regiões Administrativas (RAs) com GeoPandas.
    * Carregamento dos dados demográficos das RAs via arquivo Excel.
    * Criação de GeoDataFrames para os HUBs e para os pontos de entrega a partir de suas coordenadas (lat/lng).
    * Junção geoespacial (`gpd.sjoin`) para identificar a qual RA pertence cada entrega.
5.  **Visualização e Insights:**
    * Criação de mapas para visualizar a distribuição dos HUBs e das entregas por região (DF-0, DF-1, DF-2).
    * Plotagem de um gráfico de barras com a proporção de entregas por HUB.
    * Criação de um mapa coroplético (choropleth map) da Densidade Urbana das RAs vs. a localização dos HUBs.

## 💡 Principais Insights e Conclusões

A análise dos dados permitiu extrair as seguintes observações operacionais:

* **Desequilíbrio na Distribuição:** Existe um claro desequilíbrio na carga de entregas. O **HUB DF-1** concentra a maior proporção de entregas (aprox. 48%), seguido pelo **DF-2** (aprox. 34%) e pelo **DF-0** (aprox. 18%).
* **Eficiência Operacional e Custos:** O HUB DF-1 opera em uma região geográfica muito mais concentrada. Em contraste, os HUBs DF-0 e DF-2 cobrem áreas muito maiores e mais dispersas, sugerindo custos operacionais (como combustível e tempo) significativamente mais altos para atender suas respectivas regiões.
* **Oportunidades de Otimização (HUB DF-0):** O HUB DF-0 está visivelmente subutilizado. Há um potencial de explorar melhor as Regiões Administrativas de alta densidade urbana que estão em sua área de cobertura, como Brazlândia (ID 4) e Itapoã (ID 28).
* **Posicionamento Estratégico dos HUBs:** O mapa de densidade urbana revela que os HUBs poderiam estar melhor posicionados. Em particular, o **HUB DF-0** está localizado longe de áreas de alta densidade. Um reposicionamento mais ao sul, próximo a Paranoá (ID 7), poderia otimizar as rotas e reduzir custos operacionais.

## 🚀 Como Executar o Projeto

1.  **Acessar o Google Colab:**
    * Acesse [colab.research.google.com] (https://colab.research.google.com/github/willcosta29/Analise-de-Hubs-de-distribuicao-regionais-da-LOGGI/blob/main/Projeto_Analise_exploratoria_de_dados.ipynb)

2.  **Abrir o Notebook:**
    * Faça o upload do arquivo `.ipynb` do projeto para o Colab (Arquivo > Upload notebook...).
    * Se o repositório estiver no GitHub, você pode abri-lo diretamente (Arquivo > Abrir notebook > GitHub > [Colar URL do repositório] > Selecionar o notebook).

3.  **Carregar os Dados:**
    * Faça o upload dos arquivos de dados (`.json`, `.shp`, `.shx`, `.xlsx`, etc.) para o ambiente do Colab. Você pode fazer isso manualmente pela aba "Arquivos" à esquerda ou montar seu Google Drive.
    * *Nota: Lembre-se de ajustar os caminhos dos arquivos no código (`pd.read_json`, `gpd.read_file`, etc.) para corresponder ao local onde você carregou os dados no Colab (ex: `/content/deliveries.json`).*

4.  **Instalar Dependências:**
    * O Google Colab já vem com `pandas`, `matplotlib` e `seaborn` pré-instalados. Você precisará instalar o `geopandas` e outras bibliotecas específicas. Execute a seguinte célula no início do notebook:
    ```python
    !pip install geopandas tabula-py PyPDF2
    ```

5.  **Executar a Análise:**
    * Com as dependências instaladas e os dados carregados, execute as células do notebook (Ambiente de execução > Executar tudo).
