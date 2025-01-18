# NOAA Shoreline Data Explorer

## _Objetivo_

1. **Entender a estrutura dos arquivos geoespaciais** e como eles se relacionam.
2. **Explorar e manipular dados no formato shapefile** usando Python.
3. **Aprender sobre sistemas de coordenadas (CRS)**, entender suas diferenças e como transformá-los.

Para isso, utilizamos o dataset fornecido pelo **NOAA** (_National Oceanic and Atmospheric Administration_), que contém dados precisos da linha costeira dos Estados Unidos. Esses dados são amplamente utilizados em cartas náuticas e estudos geoespaciais.

## _Dataset_

O dataset fornecido está dividido em 4 arquivos principais, com funções específicas:

| Arquivo  | Função                                                                   |
| -------- | ------------------------------------------------------------------------ |
| **.shp** | Contém os dados geométricos (polígonos, pontos ou linhas).               |
| **.dbf** | Armazena os dados tabulares associados às geometrias.                    |
| **.prj** | Define o sistema de coordenadas (CRS) para posicionar os dados no mundo. |
| **.shx** | Índice das geometrias para acesso rápido às informações.                 |

## _Detalhes dos Arquivos_

### **1. Arquivo .shp**

- Contém **quatro colunas** principais: `'Status'`, `'Acquisitn'`, `'Type'` e `'geometry'`.
- A coluna `'geometry'` armazena as formas geométricas que representam os polígonos da linha costeira.

### **2. Arquivo .dbf**

- Similar ao `.shp`, este arquivo armazena as colunas `'Status'`, `'Acquisitn'`, e `'Type'`.
- Não contém a coluna `'geometry'`.

### **3. Arquivo .prj**

- É um arquivo simples em formato **WKT (Well-Known Text)** que define o sistema de coordenadas do shapefile.
- O CRS fornecido pelo NOAA é **NAD83**, específico para a América do Norte.
- Para análise global ou integração com outros datasets, realizamos a conversão para **WGS84** (padrão mundial):
  - **Por que converter?** Apesar das diferenças entre NAD83 e WGS84 serem pequenas (milímetros a metros), é uma boa prática utilizar WGS84 para garantir compatibilidade.

### **4. Arquivo .shx**

- Este arquivo é o **índice das geometrias** do shapefile e ajuda no acesso rápido às informações geométricas.
- Contém apenas a coluna `'geometry'`, semelhante ao `.shp`.

## _Observações Importantes_

- No site do NOAA, é possível selecionar **períodos de tempo** para observar como o terreno costeiro muda ao longo dos anos.
- Infelizmente, o dataset exportado não contém essas informações temporais.
- Seria interessante explorar a variação **geoespacial ao longo do tempo**, avaliando como o litoral evolui em função da erosão, alterações climáticas e ações humanas.

---

## _Ferramentas Utilizadas_

1. **Linguagem:** Python 3.12
2. **Bibliotecas:**
   - `geopandas` para manipulação dos shapefiles.
   - `pyproj` para transformação de sistemas de coordenadas (CRS).
   - `pandas` para manipulação de dados tabulares.

### _Exemplo de Código_

Transformação de CRS para **WGS84**:

```python
import geopandas as gpd

# Abrir o shapefile original
gdf = gpd.read_file("CUSP_IN_PROGRESS/CUSP_IN_PROGRESS.shp")

# Verificar o CRS original
print("CRS Original:", gdf.crs)

# Converter para WGS84
gdf_wgs84 = gdf.to_crs("EPSG:4326")

# Salvar o shapefile atualizado
gdf_wgs84.to_file("CUSP_IN_PROGRESS_WGS84.shp")
```
