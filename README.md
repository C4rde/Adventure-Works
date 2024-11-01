# Documentación

**Autor**: Ignacio Cardetti  
**Email**: nachocardetti@gmail.com  
**Cohorte**: DA-FT 05  
**Fecha de entrega**: 08/08/2024  

---

## Índice
1. [Introducción](#introducción)
2. [Fuente de Origen y Carga de los Datos](#fuente-de-origen-y-carga-de-los-datos)
3. [Transformación de Datos](#transformación-de-datos)
   - [DimCustomer](#dimcustomer)
   - [FactInternetSales](#factinternetsales)
   - [DimProduct](#dimproduct)
   - [DimProductSubcategory](#dimproductsubcategory)
   - [DimProductCategory](#dimproductcategory)
   - [DimDate](#dimdate)
   - [DimPromotion](#dimpromotion)
   - [DimGeography](#dimgeography)
4. [Medidas Calculadas](#medidas-calculadas)
5. [Mockup](#mockup)
   - [Diapositiva 1: Ventas](#diapositiva-1-ventas)
   - [Diapositiva 1: Costos](#diapositiva-1-costos)
   - [Diapositiva 2: Utilidades](#diapositiva-2-utilidades)
   - [Diapositiva 3: Eficiencia](#diapositiva-3-eficiencia)
   - [Diapositiva 4: Negocios Usa](#diapositiva-4-negocios-usa)
6. [Dashboard](#dashboard)
   - [Página 1: Portada](#página-1-portada)
   - [Página 2: Ventas](#página-2-ventas)
   - [Página 3: Costos](#página-3-costos)
   - [Página 4: Utilidad](#página-4-utilidad)
   - [Página 5: Eficiencia](#página-5-eficiencia)
   - [Página 6: Negocios USA](#página-6-negocios-usa)
7. [Resultados Principales y Líneas Futuras de Análisis](#resultados-principales-y-líneas-futuras-de-análisis)

---

## Introducción

Adventure Works es una gran empresa multinacional de fabricación que produce y distribuye bicicletas, piezas y accesorios. Quiere comprender su rendimiento de ventas para identificar los factores que afectan a las ventas, los costos y la rentabilidad, facilitando la toma de decisiones estratégicas basadas en datos.

## Fuente de Origen y Carga de los Datos

- La base de datos fue descargada y restaurada en SQL Server para su análisis.
- Se importaron tablas específicas desde SQL Server a Power BI:
  - FactInternetSales
  - DimProduct
  - DimProductSubcategory
  - DimProductCategory
  - DimDate
  - DimPromotion
  - DimGeography
- La tabla `DimCustomer` fue importada desde Google Sheets por razones de seguridad.

## Transformación de Datos

### General
- Verificación de encabezados para asegurar consistencia.
- Eliminación de filas en blanco en todas las tablas.
- Identificación de claves primarias y foráneas.
- Estandarización del idioma a inglés, eliminando otras columnas redundantes.

### DimCustomer
- **Descripción**: Tabla de clientes.
- **Acciones**:
  - Eliminación de columnas con datos nulos o irrelevantes.
  - Combinación con `DimGeography` para incorporar datos geográficos.

### FactInternetSales
- **Acciones**:
  - Creación de clave primaria `ID_Ventas`.
  - Eliminación de columnas no esenciales.

### DimProduct
- **Acciones**:
  - Eliminación de descripciones en otros idiomas.
  - Combinación con tablas `DimProductSubcategory` y `DimProductCategory`.

### DimProductSubcategory
- Eliminación de columnas redundantes.

### DimProductCategory
- Eliminación de columnas redundantes.

### DimDate
- Marcada como tabla calendario.
- Creación de columnas para el nombre corto del mes y el trimestre.

### DimPromotion
- Eliminación de nombres y tipos de promoción en otros idiomas.

### DimGeography
- Eliminación de columnas redundantes con nombres de país en otros idiomas.

---

## Medidas Calculadas

- **% Margen Bruto**: `DIVIDE([Utilidad Bruta], [Ingresos])`
- **Costo Envío**: `SUM(FactInternetSales[Freight])`
- **Ingresos USA**: `CALCULATE([Ingresos], DimGeography[EnglishCountryRegionName] = "United States")`
- **Ingresos Acumulados PA**: `CALCULATE([Ingresos Acumulados], PREVIOUSYEAR(DimDate[FullDateAlternateKey]))`
- **Utilidad Neta PA**: `CALCULATE([Utilidad Neta], PREVIOUSYEAR(DimDate[FullDateAlternateKey]))`
- **% Variación Ingresos**: `DIVIDE([Ingresos] - [Ingresos PA], [Ingresos PA])`

(Ver documento para lista completa de medidas calculadas)

## Mockup

### Diapositiva 1: Ventas
- Total de ingresos y su variación porcentual.
- Cantidad vendida y demografía de clientes por país representada en un mapa.

### Diapositiva 1: Costos
- Costos de bienes vendidos (COGS) y su variación.

### Diapositiva 2: Utilidades
- Utilidad bruta y neta con su variación porcentual.
- Detalle de utilidades por segmento y subcategoría de producto.

### Diapositiva 3: Eficiencia
- Ratio Costo Operacional (COGS + flete / ingresos).
- Medidores para margen bruto y neto.

### Diapositiva 4: Negocios USA
- Indicadores detallados para Estados Unidos, por provincia y ciudad.

## Dashboard

### Página 1: Portada
Portada amigable y orientada a facilitar la navegación del usuario.

### Página 2: Ventas
Métricas clave y gráficos sobre el rendimiento de ventas.

### Página 3: Costos
Análisis detallado de los costos.

### Página 4: Utilidad
Enfoque en la utilidad bruta y neta.

### Página 5: Eficiencia
Métricas de eficiencia operativa de la empresa.

### Página 6: Negocios USA
Análisis específico del mercado estadounidense.

## Resultados Principales y Líneas Futuras de Análisis

Los análisis revelan insights en ventas, costos y eficiencia operativa. Los ingresos muestran un crecimiento y mejora en los márgenes de utilidad neta. Las regiones de Estados Unidos presentan oportunidades de expansión. Se recomienda profundizar en la segmentación por categoría de producto y el análisis de clientes.

---

## Instrucciones para añadir imágenes en Markdown

Si deseas insertar imágenes, puedes usar la siguiente sintaxis:

```markdown
![Texto alternativo](ruta_de_la_imagen)
