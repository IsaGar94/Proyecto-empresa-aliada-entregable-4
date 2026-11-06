## 1. Propósito del proyecto

En este entregable se desarrolla la creación de una base de datos desde jupiter y utilizando **SQLite**, con el propósito de almacenar y organizar la información contenida en los archivos. Se crea una base de datos llamada `PRODUCTOS.db`, dentro de la cual se generan cinco tablas correspondientes a las dimensiones y la tabla de hechos del proyecto. Posteriormente, se realiza la lectura de los archivos de origen y la inserción de sus registros en las tablas correspondientes.

Finalmente, se ejecutan consultas SQL para verificar la información almacenada y realizar cruces entre distintas tablas, con el objetivo de obtener información relacionada con las ventas, categorías, regiones, periodos y segmentos.

## 2. Objetivo

Crear una base de datos relacional Jupiter y leerla en SQLite, construir las tablas necesarias e insertar la información proveniente de archivos CSV y Excel para facilitar su consulta y análisis mediante instrucciones SQL.

## 3. Archivos de entrada

- `DIM_CATEGORY.csv`
- `DIM_CALENDAR.xlsx`
- `FACT_SALES.csv`
- `DIM_PRODUCT.xlsx`
- `DIM_SEGMENT.xlsx`

## 4. Herramientas y tecnologías utilizadas

- **Python:** lenguaje utilizado para ejecutar el proceso.
- **SQLite:** sistema de gestión de bases de datos.
- **sqlite3:** biblioteca para establecer la conexión y ejecutar instrucciones SQL.
- **Pandas:** apoyo para el manejo de datos.
- **CSV:** lectura de archivos delimitados por comas.
- **OpenPyXL:** lectura de archivos de Excel.
- **xlrd:** biblioteca instalada como parte del entorno de trabajo.

## 5. Desarrollo del proyecto

### 5.1. Creación de la base de datos

Se utiliza la biblioteca `sqlite3` para establecer una conexión con SQLite y crear la base de datos `PRODUCTOS.db`. Mediante la conexión se obtiene un cursor (`cur`), que permite ejecutar instrucciones SQL para crear tablas, insertar registros y realizar consultas.

### 5.2. Lectura de los archivos de origen

Se realiza la lectura de los archivos que contienen la información del proyecto:

- Los archivos CSV se leen mediante la biblioteca `csv`.
- Los archivos Excel se leen mediante `openpyxl`, obteniendo los valores de la hoja activa.

Los registros se almacenan en listas para posteriormente recorrerlos e insertarlos en las tablas correspondientes.

### 5.3. Creación de las tablas

Dentro de la base de datos se crean cinco tablas:

#### 5.3.1. DIM_CATEGORY

Almacena la información de las categorías.

| Campo | Tipo de dato |
|---|---|
| ID_CATEGORY | INT PRIMARY KEY |
| CATEGORY | TEXT |

#### 5.3.2. DIM_CALENDAR

Almacena la información relacionada con el calendario y los periodos.

| Campo | Tipo de dato |
|---|---|
| WEEK | TEXT |
| YEAR | INTEGER |
| MONTH | INTEGER |
| WEEK_NUMBER | INTEGER |
| DATE | TEXT |

#### 5.3.3. FACT_SALES

Contiene los registros de ventas.

| Campo | Tipo de dato |
|---|---|
| WEEK | TEXT |
| ITEM_CODE | TEXT |
| TOTAL_UNIT_SALES | REAL |
| TOTAL_VALUE_SALES | REAL |
| TOTAL_UNIT_AVG_WEEKLY_SALES | REAL |
| REGION | TEXT |

#### 5.3.4. DIM_PRODUCT

Almacena la información descriptiva de los productos.

| Campo | Tipo de dato |
|---|---|
| MANUFACTURER | TEXT |
| BRAND | TEXT |
| ITEM | TEXT |
| ITEM_DESCRIPTION | TEXT |
| CATEGORY | TEXT |
| FORMAT | TEXT |
| ATTR1 | TEXT |
| ATTR2 | TEXT |
| ATTR3 | TEXT |

#### 5.3.5. DIM_SEGMENT

Contiene la información relacionada con la clasificación de segmentos.

| Campo | Tipo de dato |
|---|---|
| CATEGORY | INTEGER |
| ATTR1 | TEXT |
| ATTR2 | TEXT |
| ATTR3 | TEXT |
| FORMAT | TEXT |
| SEGMENT | TEXT |

### 5.4. Inserción de los datos

Una vez creadas las tablas, se recorren los registros de cada archivo para insertar la información mediante instrucciones SQL `INSERT INTO`.

Se utilizan parámetros (`?`) para enviar los valores a las consultas de inserción.

Para la tabla `DIM_CALENDAR`, se convierten los valores de fecha que se encuentran en formato `datetime` o `date` a texto con el formato `dd/mm/yyyy`.

Después de insertar los registros en cada tabla, se ejecuta `conn.commit()` para confirmar y guardar los cambios en la base de datos.

### 5.5. Consultas de verificación

Después de realizar las inserciones, se ejecutan consultas generales para comprobar la información almacenada.

Se utilizan instrucciones `SELECT *` para consultar los registros de:

- `DIM_CALENDAR`
- `DIM_SEGMENT`
- `FACT_SALES`

Estas consultas permiten revisar que la información se encuentre disponible dentro de las tablas creadas.

### 5.6. Consulta de ventas mediante la unión de tablas

Se realiza una consulta que relaciona las tablas `DIM_PRODUCT`, `FACT_SALES` y `DIM_CALENDAR`.

Las relaciones se realizan mediante los campos:

- `DIM_PRODUCT.ITEM` con `FACT_SALES.ITEM_CODE`.
- `FACT_SALES.WEEK` con `DIM_CALENDAR.WEEK`.

La consulta obtiene la categoría del producto, la región, el periodo —construido a partir del año y el mes— y el total de ventas en unidades.

Se utiliza `SUM()` para calcular las ventas totales y `GROUP BY` para agrupar los resultados por categoría, región y periodo. Finalmente, se ordenan los resultados de mayor a menor mediante `ORDER BY TotalVentas DESC`.

### 5.7. Consulta de categorías, regiones y segmentos

Se ejecuta una segunda consulta para relacionar las tablas `DIM_PRODUCT`, `FACT_SALES`, `DIM_CALENDAR` y `DIM_SEGMENT`.

La relación con `DIM_SEGMENT` se realiza mediante los campos `CATEGORY`, `FORMAT`, `ATTR1`, `ATTR2` y `ATTR3`.

Se utiliza `SELECT DISTINCT` para obtener combinaciones únicas de categoría, región y segmento. Además, se ordenan los resultados por región en orden descendente y se limita la consulta a 15 registros mediante `LIMIT 15`.

### 5.8. Cierre de la conexión

Al finalizar las operaciones, se utiliza `conn.close()` para cerrar la conexión con la base de datos SQLite.

## 6. Resultado del proyecto

Como resultado de este entregable se obtiene una base de datos llamada `PRODUCTOS.db`.

Esta base contiene las cinco tablas creadas y los registros cargados desde los archivos de origen. También se cuenta con consultas SQL que permiten verificar la información y relacionar las tablas para obtener resultados agrupados por categoría, región, periodo y segmento.

- La base de datos se crea utilizando SQLite y se almacena en el archivo `PRODUCTOS.db`.
- Los datos se cargan desde los archivos CSV y Excel proporcionados para el proyecto.
- Las tablas se crean mediante instrucciones `CREATE TABLE`.
- La inserción de registros se realiza mediante `INSERT INTO`.
- Se utiliza `conn.commit()` para guardar los cambios realizados.
- Las consultas SQL permiten revisar los registros y realizar relaciones entre las tablas.
- El código compartido no incluye instrucciones para definir llaves foráneas en las tablas ni para generar índices adicionales.

## 7. Conclusión

En este entregable se construye una base de datos relacional utilizando SQLite, a partir de la información contenida en los archivos.

Se crean cinco tablas para organizar la información de categorías, calendario, ventas, productos y segmentos. Posteriormente, se realiza la carga de los registros y se ejecutan consultas SQL para verificar los datos y obtener información mediante la unión de diferentes tablas.

Con este proceso, la información queda almacenada en una base de datos que permite realizar consultas y facilita su utilización en las siguientes etapas del proyecto.

## 8. Estructura del proyecto

```text
Proyecto-empresa-aliada-entregable-4/
│
├── DIM_CATEGORY.csv
├── DIM_CALENDAR.xlsx
├── FACT_SALES.csv
├── DIM_PRODUCT.xlsx
├── DIM_SEGMENT.xlsx
│
├── Codigo.ipynb
│
└── PRODUCTOS.db
```

