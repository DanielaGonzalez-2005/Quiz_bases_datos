# Quiz_bases_datos

# 📊 Consultas SQL - Análisis TIC

Este repositorio contiene diferentes consultas SQL utilizadas para el análisis de datos de los periodos **2022** y **2023**.

---

## 🔎 1. Consulta general y valores distintos

Esta consulta permite:

* Obtener todos los registros de la tabla `TIC2023`
* Identificar los valores únicos de la columna `ACTIVIDAD_NOMBRE`

```sql
-- Consulta de todos los registros
WITH CONSULTA AS (
    SELECT * 
    FROM TIC2023
)

-- Valores distintos en la columna ACTIVIDAD_NOMBRE
SELECT DISTINCT ACTIVIDAD_NOMBRE
FROM TIC2023;
```

---

## 📈 2. Consulta de actividades (TIC 2022)

Esta consulta selecciona información clave de actividades y la organiza por sede.

```sql
SELECT 
    sede_codigo AS codigo_sede, 
    periodo_anio AS anio_reporte, 
    actividad_codigo AS cod_actividad, 
    actividad_nombre AS nombre_actividad
FROM TIC2022
ORDER BY codigo_sede
LIMIT 50;
```

---

## 🏗️ 3. Creación de tabla resumen

Se crea una tabla para almacenar un resumen de las sedes con restricciones de unicidad.

```sql
CREATE TABLE tic_sedes_resumen (
    resumen_id INT PRIMARY KEY,
    sede_codigo INT,
    Total_actividades INT,
    Tiene_internet BOOLEAN,
    Fecha_carga DATETIME,
    UNIQUE (sede_codigo, anio)
);
```

---

## 📊 4. Comparación de actividades entre 2022 y 2023

Esta consulta compara la cantidad de actividades por sede entre ambos años y calcula la tendencia.

```sql
SELECT  
    t2.sede_codigo AS codigo_sede, 
    COUNT(t2.sede_codigo) AS total_act_2022, 
    COUNT(t3.sede_codigo) AS total_act_2023, 
    (COUNT(t3.sede_codigo) - COUNT(t2.sede_codigo)) AS diferencia, 
    CASE 
        WHEN (COUNT(t3.sede_codigo) - COUNT(t2.sede_codigo)) > 0 THEN 'CRECIÓ' 
        WHEN (COUNT(t3.sede_codigo) - COUNT(t2.sede_codigo)) < 0 THEN 'DECRECIÓ' 
        ELSE 'SIN CAMBIO'
    END AS tendencia
FROM TIC2022 AS t2 
INNER JOIN TIC2023 AS t3 
    ON t2.sede_codigo = t3.sede_codigo 
GROUP BY t2.sede_codigo 
HAVING COUNT(t2.sede_codigo) >= 2 
ORDER BY diferencia DESC
LIMIT 30;
```

---

## ✨ Notas

* Las consultas están diseñadas para análisis exploratorio y comparativo.
* Se utilizan funciones agregadas como `COUNT` y estructuras como `CASE` para generar métricas.
* Se recomienda validar los nombres de columnas según el motor de base de datos utilizado.

---
