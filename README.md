# Quiz

Este repositorio contiene diferentes consultas SQL utilizadas para el análisis de datos de los periodos **2022** y **2023**.

---

## PUNTO1

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

## 📈 PUNTO2

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

## PUNTO3
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

## PUNTO4
```sql
SELECT 
    SUBSTR(sede_codigo, 1, 2) AS departamento, 
    COUNT(DISTINCT sede_codigo) AS conteo_sedes_unicas
FROM TIC2023
WHERE actividad_codigo = '05'
GROUP BY SUBSTR(sede_codigo, 1, 2)
ORDER BY conteo_sedes_unicas;
```

---
## PUNTO5
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
