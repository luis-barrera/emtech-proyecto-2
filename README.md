# Synergy Logistics

**Synergy Logistics** es una empresa dedicada a la intermediación de servicios de importación y exportación de diferentes productos. Actualmente la empresa cuenta con una base de datos que refleja las rutas más importantes que opera desde el año 2015, con su respectivo origen y destino, año, producto, modo de transporte y valor total. Nuestro propósito, es que a partir de estos datos se genere un análisis que sirva de la base para la estructuración de su estrategia operativa.

Los datos que nos fueron proporcionados por Synergy Logistics son los siguientes.


```python
import pandas as pd
import numpy as np
df = pd.read_csv("synergy_logistics_database.csv")
(df.head(1)
 .transpose()
 .reset_index()
 .rename(columns={'index': 'Nombre de Columna', 0: 'Ejemplo'}))
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Nombre de Columna</th>
      <th>Ejemplo</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>register_id</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>direction</td>
      <td>Exports</td>
    </tr>
    <tr>
      <th>2</th>
      <td>origin</td>
      <td>Japan</td>
    </tr>
    <tr>
      <th>3</th>
      <td>destination</td>
      <td>China</td>
    </tr>
    <tr>
      <th>4</th>
      <td>year</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>5</th>
      <td>date</td>
      <td>31/01/15</td>
    </tr>
    <tr>
      <th>6</th>
      <td>product</td>
      <td>Cars</td>
    </tr>
    <tr>
      <th>7</th>
      <td>transport_mode</td>
      <td>Sea</td>
    </tr>
    <tr>
      <th>8</th>
      <td>company_name</td>
      <td>Honda</td>
    </tr>
    <tr>
      <th>9</th>
      <td>total_value</td>
      <td>33000000</td>
    </tr>
  </tbody>
</table>
</div>



Con el fin de crear una estrategia operativa para el año 2021, tenemos analizar los datos proporcionados para obtener 3 enfoques:
- Rutas de importación y exportación
- Medios de transporte usados
- Valor de las importaciones y exportaciones por rutas.

## 10 rutas con más demandas

### Rutas de importaciones


```python
rutas_importaciones = df.loc[df["direction"] == "Imports"]
rutas_importaciones = (rutas_importaciones
                         .groupby(['direction', 'origin', 'destination'],
                                  group_keys=False).count())
rutas_importaciones.reset_index()
rutas_importaciones.drop(['register_id', 'year', 'date', 'product', 'transport_mode', 'company_name'], axis=1, inplace=True)
rutas_importaciones.rename(columns={'total_value': 'count'}, inplace=True)
rutas_importaciones.sort_values('count', ascending=False).reset_index().head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>direction</th>
      <th>origin</th>
      <th>destination</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Imports</td>
      <td>Singapore</td>
      <td>Thailand</td>
      <td>273</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Imports</td>
      <td>Germany</td>
      <td>China</td>
      <td>233</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Imports</td>
      <td>China</td>
      <td>Japan</td>
      <td>210</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Imports</td>
      <td>Japan</td>
      <td>Mexico</td>
      <td>206</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Imports</td>
      <td>China</td>
      <td>Thailand</td>
      <td>200</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Imports</td>
      <td>Malaysia</td>
      <td>Thailand</td>
      <td>195</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Imports</td>
      <td>Spain</td>
      <td>Germany</td>
      <td>142</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Imports</td>
      <td>Mexico</td>
      <td>USA</td>
      <td>122</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Imports</td>
      <td>China</td>
      <td>United Arab Emirates</td>
      <td>114</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Imports</td>
      <td>Brazil</td>
      <td>China</td>
      <td>113</td>
    </tr>
  </tbody>
</table>
</div>



### Rutas exportaciones


```python
rutas_exportaciones = df.loc[df["direction"] == "Exports"]
rutas_exportaciones = rutas_exportaciones.groupby(['direction', 'origin', 'destination'], group_keys=False).count()
rutas_exportaciones.reset_index()
rutas_exportaciones.drop(['register_id', 'year', 'date', 'product', 'transport_mode', 'company_name'], axis=1, inplace=True)
rutas_exportaciones.rename(columns={'total_value': 'count'}, inplace=True)
rutas_exportaciones.sort_values('count', ascending=False).reset_index().head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>direction</th>
      <th>origin</th>
      <th>destination</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Exports</td>
      <td>South Korea</td>
      <td>Vietnam</td>
      <td>497</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Exports</td>
      <td>Netherlands</td>
      <td>Belgium</td>
      <td>437</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Exports</td>
      <td>USA</td>
      <td>Netherlands</td>
      <td>436</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Exports</td>
      <td>China</td>
      <td>Mexico</td>
      <td>330</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Exports</td>
      <td>Japan</td>
      <td>Brazil</td>
      <td>306</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Exports</td>
      <td>Germany</td>
      <td>France</td>
      <td>299</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Exports</td>
      <td>South Korea</td>
      <td>Japan</td>
      <td>279</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Exports</td>
      <td>Australia</td>
      <td>Singapore</td>
      <td>273</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Exports</td>
      <td>Canada</td>
      <td>Mexico</td>
      <td>261</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Exports</td>
      <td>China</td>
      <td>Spain</td>
      <td>250</td>
    </tr>
  </tbody>
</table>
</div>



### Rutas importaciones y exportaciones


```python
rutas = df.groupby(['direction', 'origin', 'destination'], group_keys=False).count()
rutas.reset_index()
rutas.drop(['register_id', 'year', 'date', 'product', 'transport_mode', 'company_name'], axis=1, inplace=True)
rutas.rename(columns={'total_value': 'count'}, inplace=True)
rutas.sort_values('count', ascending=False).reset_index().head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>direction</th>
      <th>origin</th>
      <th>destination</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Exports</td>
      <td>South Korea</td>
      <td>Vietnam</td>
      <td>497</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Exports</td>
      <td>Netherlands</td>
      <td>Belgium</td>
      <td>437</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Exports</td>
      <td>USA</td>
      <td>Netherlands</td>
      <td>436</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Exports</td>
      <td>China</td>
      <td>Mexico</td>
      <td>330</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Exports</td>
      <td>Japan</td>
      <td>Brazil</td>
      <td>306</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Exports</td>
      <td>Germany</td>
      <td>France</td>
      <td>299</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Exports</td>
      <td>South Korea</td>
      <td>Japan</td>
      <td>279</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Exports</td>
      <td>Australia</td>
      <td>Singapore</td>
      <td>273</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Imports</td>
      <td>Singapore</td>
      <td>Thailand</td>
      <td>273</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Exports</td>
      <td>Canada</td>
      <td>Mexico</td>
      <td>261</td>
    </tr>
  </tbody>
</table>
</div>



Si comparamos esta tabla con la de la suma de los montos


```python
rutas = df.groupby(['direction', 'origin', 'destination'], group_keys=False).sum()
rutas.reset_index()
rutas.drop(['register_id', 'year'], axis=1, inplace=True)
rutas.sort_values('total_value', ascending=False).reset_index().head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>direction</th>
      <th>origin</th>
      <th>destination</th>
      <th>total_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Exports</td>
      <td>China</td>
      <td>Mexico</td>
      <td>12250000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Exports</td>
      <td>Canada</td>
      <td>Mexico</td>
      <td>8450000000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Exports</td>
      <td>South Korea</td>
      <td>Vietnam</td>
      <td>6877007000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Exports</td>
      <td>France</td>
      <td>Belgium</td>
      <td>5538069000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Exports</td>
      <td>France</td>
      <td>United Kingdom</td>
      <td>5427000000</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Exports</td>
      <td>China</td>
      <td>South Korea</td>
      <td>4790000000</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Exports</td>
      <td>USA</td>
      <td>Mexico</td>
      <td>4710000000</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Exports</td>
      <td>South Korea</td>
      <td>Japan</td>
      <td>4594000000</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Exports</td>
      <td>Germany</td>
      <td>Italy</td>
      <td>4541000000</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Exports</td>
      <td>China</td>
      <td>Germany</td>
      <td>4090000000</td>
    </tr>
  </tbody>
</table>
</div>



No se recomienda elegir las rutas con más demandas, ya que no son estas precisamente las que más monto generan.

## Medios de transporte más importantes


```python
transportes = df.groupby(['transport_mode'], group_keys=False).sum()
transportes.drop(['register_id', 'year'], axis=1, inplace=True)
transportes.sort_values('total_value', ascending=False).reset_index()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>transport_mode</th>
      <th>total_value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Sea</td>
      <td>100530622000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Rail</td>
      <td>43628043000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Air</td>
      <td>38262147000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Road</td>
      <td>33270486000</td>
    </tr>
  </tbody>
</table>
</div>



Los 3 medios de transporte más importantes para Synergy Logistics son: Mar, Ferrocarril y Aire.
El que genera menos valor es por Carretera, por lo que pueden reducir este.

Se recomienda encontrar maneras de completar las rutas por carretera a través de los otros medios de transporte para no perder totalmente las importaciones y exportaciones en esas rutas.

## 80% del valor de exportaciones y importaciones


```python
rutas = df.groupby(['direction', 'origin', 'destination'], group_keys=False).sum()
gran_total = rutas['total_value'].sum()
rutas.reset_index()
rutas.drop(['register_id', 'year'], axis=1, inplace=True)
rutas.sort_values('total_value', ascending=False, inplace=True)

pd.set_option('display.float_format', '{:.10f}'.format)
rutas['porcentaje'] = rutas['total_value'].cumsum() / gran_total
rutas_80 = rutas.loc[rutas['porcentaje'] <= 0.8]
rutas_80.reset_index()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>direction</th>
      <th>origin</th>
      <th>destination</th>
      <th>total_value</th>
      <th>porcentaje</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Exports</td>
      <td>China</td>
      <td>Mexico</td>
      <td>12250000000</td>
      <td>0.0567941318</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Exports</td>
      <td>Canada</td>
      <td>Mexico</td>
      <td>8450000000</td>
      <td>0.0959704921</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Exports</td>
      <td>South Korea</td>
      <td>Vietnam</td>
      <td>6877007000</td>
      <td>0.1278540546</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Exports</td>
      <td>France</td>
      <td>Belgium</td>
      <td>5538069000</td>
      <td>0.1535299584</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Exports</td>
      <td>France</td>
      <td>United Kingdom</td>
      <td>5427000000</td>
      <td>0.1786909178</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Imports</td>
      <td>Mexico</td>
      <td>Japan</td>
      <td>1143000000</td>
      <td>0.7776517159</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Imports</td>
      <td>USA</td>
      <td>India</td>
      <td>1133000000</td>
      <td>0.7829045936</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Exports</td>
      <td>Spain</td>
      <td>Russia</td>
      <td>1085000000</td>
      <td>0.7879349310</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Exports</td>
      <td>India</td>
      <td>United Arab Emirates</td>
      <td>1037000000</td>
      <td>0.7927427281</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Exports</td>
      <td>USA</td>
      <td>Netherlands</td>
      <td>1032187000</td>
      <td>0.7975282109</td>
    </tr>
  </tbody>
</table>
<p>61 rows × 5 columns</p>
</div>



Elegimos solos los países de acuerdo al origin.


```python
count_origin = rutas_80.groupby(['origin']).count().reset_index()
count_origin.drop(['total_value'], axis=1, inplace=True)
count_origin.rename(columns={'porcentaje': 'count'}, inplace=True)
count_origin
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>origin</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Belgium</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Canada</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>China</td>
      <td>11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>France</td>
      <td>5</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Germany</td>
      <td>5</td>
    </tr>
    <tr>
      <th>5</th>
      <td>India</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Italy</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Japan</td>
      <td>7</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Malaysia</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Mexico</td>
      <td>2</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Netherlands</td>
      <td>1</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Russia</td>
      <td>5</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Singapore</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>South Korea</td>
      <td>6</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Spain</td>
      <td>2</td>
    </tr>
    <tr>
      <th>15</th>
      <td>USA</td>
      <td>10</td>
    </tr>
    <tr>
      <th>16</th>
      <td>United Kingdom</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



Veamos los países presentes en el 100% de las importaciones y exportaciones.


```python
count_origin_100 = rutas.groupby(['origin']).count().reset_index()
count_origin_100.drop(['total_value'], axis=1, inplace=True)
count_origin_100.rename(columns={'porcentaje': 'count'}, inplace=True)
count_origin_100
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>origin</th>
      <th>count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Australia</td>
      <td>7</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Austria</td>
      <td>6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Belgium</td>
      <td>4</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Brazil</td>
      <td>7</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Canada</td>
      <td>7</td>
    </tr>
    <tr>
      <th>5</th>
      <td>China</td>
      <td>16</td>
    </tr>
    <tr>
      <th>6</th>
      <td>France</td>
      <td>13</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Germany</td>
      <td>13</td>
    </tr>
    <tr>
      <th>8</th>
      <td>India</td>
      <td>6</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Italy</td>
      <td>14</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Japan</td>
      <td>18</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Malaysia</td>
      <td>2</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Mexico</td>
      <td>13</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Netherlands</td>
      <td>6</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Russia</td>
      <td>10</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Singapore</td>
      <td>5</td>
    </tr>
    <tr>
      <th>16</th>
      <td>South Korea</td>
      <td>10</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Spain</td>
      <td>8</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Switzerland</td>
      <td>4</td>
    </tr>
    <tr>
      <th>19</th>
      <td>USA</td>
      <td>15</td>
    </tr>
    <tr>
      <th>20</th>
      <td>United Arab Emirates</td>
      <td>1</td>
    </tr>
    <tr>
      <th>21</th>
      <td>United Kingdom</td>
      <td>7</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Vietnam</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



Comparamos los países y vemos aquellos fuera del 80%, estos son los que podemos ignorar.


```python
paises_descartados = []

list_100 = count_origin_100['origin'].tolist()
list_80 = count_origin['origin'].tolist()

for pais in list_100:
    if pais not in list_80:
        paises_descartados.append(pais)
        
paises_descartados
```




    ['Australia',
     'Austria',
     'Brazil',
     'Switzerland',
     'United Arab Emirates',
     'Vietnam']



## Conclusión general

A Synergy Logistics le conviene implementar dos estrategias: basada en las rutas que le generan el 80% del valor total y también quedarse con los 3 mejores medios de transportes.

En el primer caso, estaría ahorrando mucho dinero al quitar instalaciones que tiene en 6 países.
Ya que tener presencia en países en los que no hay tanto valor no es tan rentable.
Se estaría enfocando en 17 de 23 países en caso de seguir esta estrategia, lo cual son 6 países menos.

En el caso de descartar el medio de transporte que menos valor genera, se tendría que consultar si es viable que los otros medios de transporte cubran estas rutas.
De lo contrario, tendríamos problemas porque el valor para el Carretera es casi el mismo que por Aire y estaríamos perdiendo mucho valores.

Lo que no se recomienda es seguir con la estrategia de usar el total de las exportaciones o importaciones.
Ya que solo unas cuantas de estas rutas están también adentro del top 10 de rutas que más valor tienen. 
