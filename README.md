# 📊 Análisis Exploratorio de Datos (EDA) - Dataset de Ventas

## Portada

**Estudiante:** Patricio Gonzales Barboza

**Módulo:** Análisis de Datos con Python y SQL

**Proyecto:** Análisis de Ventas de una Empresa Retail

**Fuente del dataset:** :https://www.kaggle.com/datasets/krakenteach13/ventas-tienda

## Descripción

Este proyecto consiste en la realización de un **Análisis Exploratorio de Datos (EDA)** utilizando Python en Google Colab. El objetivo es comprender la estructura del conjunto de datos, evaluar su calidad, obtener estadísticas descriptivas, visualizar el comportamiento de las ventas y realizar consultas SQL mediante DuckDB.

---

## Objetivos

- Explorar la estructura del dataset.
- Verificar la calidad de los datos.
- Identificar valores nulos y registros duplicados.
- Obtener estadísticas descriptivas de variables numéricas y categóricas.
- Analizar las ventas por tienda, categoría, producto y período.
- Generar visualizaciones interactivas con Plotly.
- Realizar consultas SQL utilizando DuckDB.
- Presentar conclusiones basadas en el análisis realizado.

---

## Dataset

El conjunto de datos contiene **500 registros** y **8 variables**.

| Variable | Tipo |
|----------|------|
| fecha | Fecha |
| tienda | Categórica |
| categoria de producto | Categórica |
| vendedor | Categórica |
| producto | Categórica |
| cantidad | Numérica |
| precio | Numérica |
| total | Numérica |

---

## Herramientas utilizadas

- Python
- Google Colab
- Pandas
- Plotly Express
- DuckDB

---

## Desarrollo del análisis

El proyecto se desarrolló siguiendo las etapas de un EDA.

### 1. Carga del dataset

Se cargó el archivo utilizando Pandas y se verificó su correcta lectura.

```python
import pandas as pd

datos_ventas = pd.read_csv("ventas.csv")
datos_ventas.head()
```

---

### 2. Exploración inicial

Se revisó la estructura del conjunto de datos mediante:

- `info()`
- `shape`
- `head()`
- `tail()`
- `columns`

Con ello fue posible identificar el tipo de datos de cada variable y la dimensión del dataset.

---

### 3. Calidad de datos

Se realizaron las siguientes validaciones:

- Identificación de valores nulos.
- Revisión de registros duplicados.
- Verificación de posibles inconsistencias.
- Validación de la relación entre **Cantidad × Precio = Total**.

Ejemplo:

```python
datos_ventas.duplicated().sum()
```

---

### 4. Estadística descriptiva

Se obtuvieron estadísticas para las variables numéricas mediante:

```python
datos_ventas.describe()
```

Y para las variables categóricas:

```python
datos_ventas.describe(include="object")
```

---

### 5. Visualizaciones

Se desarrollaron distintos gráficos utilizando Plotly para facilitar la interpretación de los datos.

Entre ellos:

- Ventas por tienda.
- Ventas por categoría.
- Productos más vendidos.
- Distribución de precios.
- Boxplot para detección de valores atípicos.
- Tendencia de ventas por mes.

Ejemplo del análisis temporal:

```python
# Crear una columna que combine el año y el mes para la agrupación
datos_ventas['mes_anio'] = datos_ventas['fecha'].dt.to_period('M')

# Agrupar las ventas por mes/año y sumar los totales
ventas_mensuales = datos_ventas.groupby('mes_anio')['total'].sum().reset_index()

# Convertir el período a texto para Plotly
ventas_mensuales['mes_anio'] = ventas_mensuales['mes_anio'].astype(str)

# Crear gráfico de tendencia
fig = px.line(
    ventas_mensuales,
    x="mes_anio",
    y="total",
    title="Tendencia de Ventas Mensual/Anual",
    markers=True
)

fig.update_layout(
    xaxis_title="Mes/Año",
    yaxis_title="Total de Ventas"
)

fig.show()
```
![alt text](image.png)

---

### 6. Consultas SQL con DuckDB

Se realizaron consultas SQL directamente sobre el DataFrame para obtener información como:

- Ventas totales.
- Ventas por tienda.
- Ventas por vendedor.
- Productos más vendidos.
- Precio promedio por categoría.

---

## Principales hallazgos

- El dataset contiene información suficiente para realizar un análisis de ventas.
- No se identificaron valores nulos ni registros duplicados.
- Las variables categóricas permiten segmentar las ventas por tienda, categoría, vendedor y producto.
- Las visualizaciones facilitaron la identificación de tendencias y diferencias entre categorías.
- Las consultas SQL complementaron el análisis exploratorio y permitieron resumir la información de forma eficiente.

---

## Estructura del proyecto

```
Proyecto_EDA/
│
├── EDA_Ventas.ipynb
├── ventas.csv
├── README.md
└── requirements.txt (opcional)
```

### 7. Tecnologias utilizadas

•  Python 
•  pandas 
•  numpy 
•  duckdb 
•  plotly 
•  Google Colab 
•  GitHub para publicación del repositorio 

---

# Consultas SQL realizadas

En esta sección se presentan las principales consultas SQL desarrolladas sobre la tabla **Tabla_ventas**. Se utilizaron las cláusulas `SELECT`, `WHERE`, `GROUP BY` y `ORDER BY` para extraer y analizar la información del conjunto de datos.

---

# 1. Uso de SELECT

La cláusula `SELECT` permite seleccionar columnas específicas de una tabla y obtener información resumida mediante funciones de agregación.

## Consulta 1: Unidades vendidas por producto

```sql
SELECT producto,
       SUM(cantidad) AS unidades_vendidas
FROM Tabla_ventas
GROUP BY producto
ORDER BY unidades_vendidas DESC;
```

**Descripción:**
Obtiene la cantidad total de unidades vendidas por cada producto, ordenando los resultados de mayor a menor.

---

## Consulta 2: Ventas totales por tienda

```sql
SELECT tienda,
       SUM(total) AS ventas_totales
FROM Tabla_ventas
GROUP BY tienda
ORDER BY ventas_totales DESC;
```

**Descripción:**
Calcula el monto total de ventas generado por cada tienda y lo presenta de mayor a menor.

---

# 2. Uso de WHERE

La cláusula `WHERE` permite filtrar registros que cumplen una condición específica.

## Consulta 1: Productos de la categoría Juguetes

```sql
SELECT *
FROM Tabla_ventas
WHERE "categoria de producto" = 'Juguetes';
```

**Descripción:**
Muestra únicamente los registros pertenecientes a la categoría **Juguetes**.

---

## Consulta 2: Ventas de la Tienda E superiores a 3000

```sql
SELECT *
FROM Tabla_ventas
WHERE tienda = 'Tienda E'
AND total > 3000;
```

**Descripción:**
Filtra las ventas realizadas en la **Tienda E** cuyo importe sea superior a **3000**.

---

# 3. Uso de GROUP BY

La cláusula `GROUP BY` agrupa registros con valores iguales para aplicar funciones de agregación.

## Consulta 1: Cantidad de ventas por tienda

```sql
SELECT tienda,
       COUNT(*) AS total_ventas
FROM Tabla_ventas
GROUP BY tienda
ORDER BY total_ventas DESC;
```

**Descripción:**
Cuenta la cantidad de registros de ventas correspondientes a cada tienda.

---

## Consulta 2: Total vendido por tienda

```sql
SELECT tienda,
       SUM(total) AS ventas_totales
FROM Tabla_ventas
GROUP BY tienda
ORDER BY ventas_totales DESC;
```

**Descripción:**
Suma el importe total de las ventas por tienda.

---

# 4. Uso de ORDER BY

La cláusula `ORDER BY` permite ordenar los resultados de una consulta de forma ascendente o descendente.

## Consulta: Productos ordenados por cantidad vendida

```sql
SELECT producto,
       cantidad,
       total
FROM Tabla_ventas
ORDER BY cantidad ASC;
```

**Descripción:**
Ordena los productos según la cantidad vendida, desde la menor hasta la mayor.

---

# Conclusión

Las consultas SQL desarrolladas permitieron analizar el comportamiento de las ventas desde diferentes perspectivas, como el desempeño por tienda, producto y categoría. Asimismo, se aplicaron filtros, agrupaciones y ordenamientos para obtener información relevante que facilita la interpretación de los datos y la toma de decisiones.

