# Documentación

[![Captura-de-pantalla-2024-11-06-195154.png](https://i.postimg.cc/KYCJrH0f/Captura-de-pantalla-2024-11-06-195154.png)](https://postimg.cc/cKMRdXyt)

*Nombre del autor*: Ignacio Cardetti  
*Email*: nachocardetti1@gmail.com  
*Cohorte*: DA-FT 05  
*Fecha de entrega*: 08/08/2024  

---

## ÍNDICE

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

Adventure Works es una gran empresa multinacional de fabricación que produce y distribuye bicicletas, piezas y accesorios. Quiere comprender su rendimiento de ventas para comprender los factores que afectan a las ventas, los costos y la rentabilidad, facilitando la toma de decisiones estratégicas basadas en datos.

## Fuente de Origen y Carga de los Datos

- Se descargó la base de datos desde el servidor original.
- La base de datos fue restaurada en SQL Server para su posterior manipulación y análisis.
- Se importaron las tablas específicas desde SQL Server a Power BI:
  - FactInternetSales
  - DimProduct
  - DimProductSubcategory
  - DimProductCategory
  - DimDate
  - DimPromotion
  - DimGeography
- Se importó la tabla `DimCustomer` desde Google Sheets por razones de seguridad y disponibilidad.

## Transformación de Datos

### General

- Verificación de encabezados para asegurar consistencia.
- Eliminación de filas en blanco en todas las tablas.
- Identificación de claves primarias y claves foráneas.
- Decisión de estandarización del lenguaje: Se ha decidido conservar solo las columnas en el idioma inglés y descartar las demás para evitar redundancia en los datos y optimizar así la base de datos.
- Fueron deshabilitadas la carga para las tablas de ProductCategory, ProductSubcategory y Geography en Power Query.

### DimCustomer

- **Descripción**: Tabla que hace referencia a los clientes.
- **Acciones Realizadas**:
  - Eliminación de columnas debido a datos nulos o irrelevantes:
    - Title
    - MiddleName
    - Suffix
    - Column18
    - AddressLine2
    - Column31
  - Eliminación de columnas redundantes gracias a la tabla `DimGeography`:
    - CountryRegionCode
    - CountryRegionCode_1
    - CountryRegionCode_2
    - CountryRegionCode_3
    - CountryRegionCode_4
    - CountryRegionCode_5
  - Combinación con `DimGeography` mediante la clave `GeographyKey` para traer las columnas:
    - City
    - StateProvinceCode
    - StateProvinceName

### FactInternetSales

- **Acciones Realizadas**:
  - Creación de una clave primaria (PK) llamada `ID_Ventas` con un índice inicial de 2480, incrementado de a 1.
  - Eliminación de columnas no esenciales para el análisis:
    - CarrierTrackingNumber
    - CustomerPONumber
  - Cambio del tipo de datos a decimal fijo para la columna `UnitPriceDiscountPct`.

### DimProduct

- **Acciones Realizadas**:
  - Eliminación de columnas por redundancia de datos:
    - FrenchDescription
    - ChineseDescription
    - ArabicDescription
    - HebrewDescription
    - ThaiDescription
    - GermanDescription
    - JapaneseDescription
    - TurkishDescription
    - SpanishProductName
    - FrenchProductName
  - Combinación con `DimProductSubcategory` mediante la columna `ProductSubcategoryKey` para traer la columna `ProductCategoryKey`.
  - Combinación con `DimProductCategory` mediante la columna `ProductCategoryKey` para traer la columna `EnglishProductCategoryName`.

### DimProductSubcategory

- **Acciones Realizadas**:
  - Eliminación de columnas redundantes:
    - SpanishProductSubcategoryName
    - FrenchProductSubcategoryName

### DimProductCategory

- **Acciones Realizadas**:
  - Eliminación de columnas redundantes:
    - SpanishProductCategoryName
    - FrenchProductCategoryName

### DimDate

- **Acciones Realizadas**:
  - Fue marcada como tabla calendario.
  - Eliminación de columnas redundantes:
    - SpanishDayNameOfWeek
    - FrenchDayNameOfWeek
    - SpanishMonthName
    - FrenchMonthName
  - Creación de Columnas:
    - Columna personalizada con el nombre del mes en formato corto (primeras 3 letras de nombre del mes).
    - Columna calculada indicando el trimestre.

### DimPromotion

- **Acciones Realizadas**:
  - Eliminación de columnas redundantes:
    - SpanishPromotionName
    - FrenchPromotionName
    - SpanishPromotionType
    - FrenchPromotionType
    - SpanishPromotionCategory
    - FrenchPromotionCategory

### DimGeography

- **Acciones Realizadas**:
  - Eliminación de columnas redundantes:
    - SpanishCountryRegionName
    - FrenchCountryRegionName

> Por pedido del cliente se han deshabilitado las tablas: DimGeography, DimProductCategory y DimProductSubcategory

## Medidas Calculadas

### Medidas Bases

- **% Margen Bruto**  
  - Descripción: Calcula el porcentaje del margen bruto.
  - Fórmula: `% Margen Bruto = DIVIDE([Utilidad Bruta], [Ingresos])`

- **EEUU % Margen Neto**  
  - Descripción: Porcentaje del margen de utilidad neta en Estados Unidos.
  - Fórmula: `% MargenUtilidadNeta = DIVIDE([EEUU Utilidad Neta], [EEUU Ingresos])`

- **Costo Envio**  
  - Descripción: Suma del costo de envío.
  - Fórmula: `Costo de Envio = SUM(FactInternetSales[Freight])`

- **EEUU Costos**  
  - Descripción: Suma total de los costos en Estados Unidos.
  - Fórmula: `EEUU Costos = CALCULATE([Costos], ALL(DimGeography[EnglishCountryRegionName]), DimGeography[EnglishCountryRegionName] = "United States")`

- **EEUU Ingresos**  
  - Descripción: Suma de los ingresos generados en Estados Unidos.
  - Fórmula: `EEUU Ingresos = CALCULATE([Ingresos], ALL(DimGeography[EnglishCountryRegionName]), DimGeography[EnglishCountryRegionName] = "United States")`

- **EEUU Ingresos Acumulados**  
  - Descripción: Suma de los ingresos acumulados en Estados Unidos.
  - Fórmula: `EEUU Ingresos Acumulados = CALCULATE([Ingresos Acumulados], ALL(DimGeography[EnglishCountryRegionName]), DimGeography[EnglishCountryRegionName] = "United States")`

- **EEUU Ingresos Acumulados PA**  
  - Descripción: Suma de los ingresos acumulados del período anterior en Estados Unidos.
  - Fórmula: `EEUU Ingresos Acumulados PA = CALCULATE([Ingresos Acumulados PA], ALL(DimGeography[EnglishCountryRegionName]), DimGeography[EnglishCountryRegionName] = "United States")`

- **EEUU Utilidad Bruta**  
  - Descripción: Calcula la utilidad bruta obtenida en Estados Unidos.
  - Fórmula: `EEUU Utilidad Bruta = CALCULATE([Utilidad Bruta], ALL(DimGeography[EnglishCountryRegionName]), DimGeography[EnglishCountryRegionName] = "United States")`

- **EEUU Utilidad Neta**  
  - Descripción: Calcula la utilidad neta obtenida en Estados Unidos.
  - Fórmula: `EEUU Utilidad Neta = CALCULATE([Utilidad Neta], ALL(DimGeography[EnglishCountryRegionName]), DimGeography[EnglishCountryRegionName] = "United States")`
**% COGS PA**:  
Descripción: Porcentaje de COGS del período anterior en relación con los ingresos.  
Fórmula: % COGS PA = DIVIDE([COGS PA], [Ingresos PA])

**% MargenUtilidadBruta PA**:  
Descripción: Porcentaje de margen de utilidad bruta del período anterior.  
Fórmula: % MargenUtilidadBruta PA = DIVIDE([UtilidadBruta PA], [Ingresos PA])

**% MargenUtilidad Neta PA**:  
Descripción: Porcentaje de margen de utilidad neta del período anterior.  
Fórmula: % MargenUtilidad Neta PA = DIVIDE([Utilidad Neta PA], [Ingresos PA])

**COGS PA**:  
Descripción: Costo de bienes vendidos del período anterior.  
Fórmula: COGS PA = CALCULATE([Costos], PREVIOUSYEAR(DimDate[FullDateAlternateKey]))

**Ingresos Acumulados PA**:  
Descripción: Suma de los ingresos acumulados del período anterior.  
Fórmula: Ingresos Acumulados PA = CALCULATE([Ingresos Acumulados], PREVIOUSYEAR(DimDate[FullDateAlternateKey]))

**Ingresos PA**:  
Descripción: Suma de los ingresos del período anterior.  
Fórmula: Ingresos PA = CALCULATE([Ingresos], PREVIOUSYEAR(DimDate[FullDateAlternateKey]))

**UtilidadBruta PA**:  
Descripción: Utilidad bruta del período anterior.  
Fórmula: UtilidadBruta PA = CALCULATE([Utilidad Bruta], PREVIOUSYEAR(DimDate[FullDateAlternateKey]))

**Utilidad Neta PA**:  
Descripción: Utilidad neta del período anterior.  
Fórmula: Utilidad Neta PA = CALCULATE([Utilidad Neta], PREVIOUSYEAR(DimDate[FullDateAlternateKey]))

**% Costos**:  
Descripción: Calcula el porcentaje del costo.  
Fórmula: % Costos = DIVIDE([Costos], [Ingresos])

**% Margen Bruto**:  
Descripción: Porcentaje del margen bruto.  
Fórmula: % Margen Bruto = DIVIDE([Utilidad Bruta], [Ingresos])

**% Margen de Utilidad Neta**:  
Descripción: Porcentaje de margen de utilidad neta.  
Fórmula: % Margen de Utilidad Neta = DIVIDE([Utilidad Neta], [Ingresos])

**% MargenUtilidadBruta**:  
Descripción: Porcentaje del margen de utilidad bruta.  
Fórmula: % MargenUtilidadBruta = DIVIDE([Utilidad Bruta], [Ingresos])

**% Ratio Costo Operacional PA**:  
Descripción: Porcentaje del ratio de costo operacional del período anterior.  
Fórmula: % Ratio Costo Operacional PA = DIVIDE([Costo Total + Envio PA], [Ingresos PA])

**% Ratio de Costo Operacional**:  
Descripción: Porcentaje del ratio de costo operacional.  
Fórmula: % Ratio de Costo Operacional = DIVIDE([Costo Total + Envio], [Ingresos])

**Cantidad Clientes**:  
Descripción: Recuento de la cantidad de clientes.  
Fórmula: Cantidad Clientes = COUNT(DimCustomer[CustomerKey])

**Cantidad Vendida**:  
Descripción: Suma total de las ventas.  
Fórmula: Cantidad Vendida = SUM(FactInternetSales[OrderQuantity])

**Costo de Envio**:  
Descripción: Suma del precio del costo de envío.  
Fórmula: Costo de Envio = SUM(FactInternetSales[Freight])

**Costo Total + Envio**:  
Descripción: Suma total del costo más el costo del envío.  
Fórmula: Costo Total + Envio = SUMX(FactInternetSales, FactInternetSales[TotalProductCost] + FactInternetSales[Freight])

**Impuestos**:  
Descripción: Suma de los impuestos.  
Fórmula: Impuestos = SUM(FactInternetSales[TaxAmt])

**Ingresos**:  
Descripción: Suma de los ingresos generados.  
Fórmula: Ingresos = SUM(FactInternetSales[SalesAmount])

**Ingresos Acumulados**:  
Descripción: Ingresos acumulados por período.  
Fórmula: Ingresos Acumulados = CALCULATE([Ingresos], DATESYTD(DimDate[FullDateAlternateKey]))

**Utilidad Bruta**:  
Descripción: Calcula la utilidad bruta restando costos a ingresos.  
Fórmula: Utilidad Bruta = [Ingresos] - [Costos]

**Utilidad Neta**:  
Descripción: Calcula la utilidad neta obtenida.  
Fórmula: Utilidad Neta = [Ingresos] - [Costos] - [Costo de Envio] - [Impuestos]

**Variación**:  
Descripción: Variación entre el período actual y el período anterior.  
Fórmula: Variación = [Ingresos] - [Ingresos PA]

**% Variacion COGS**:  
Descripción: Porcentaje de variación del costo de bienes vendidos.  
Fórmula: % Variacion COGS = DIVIDE([COGS] - [COGS PA], [COGS PA])

**% Variación Ingresos**:  
Descripción: Porcentaje de variación de los ingresos.  
Fórmula: % Variación Ingresos = DIVIDE([Ingresos] - [Ingresos PA], [Ingresos PA])

**% Variacion Margen Neto**:  
Descripción: Porcentaje de variación del margen de utilidad neta.  
Fórmula: % Variacion Margen Neto = DIVIDE([Utilidad Neta] - [Utilidad Neta PA], [Utilidad Neta PA])

**% Variación Utilidad Bruta**:  
Descripción: Porcentaje de variación de la utilidad bruta.  
Fórmula: % Variación Utilidad Bruta = DIVIDE([Utilidad Bruta] - [Utilidad Bruta PA], [Utilidad Bruta PA])

**Variación Costos**:  
Descripción: Variación en los costos entre períodos.  
Fórmula: Variación Costos = [Costos] - [Costos PA]

**Variacion de Clientes**:  
Descripción: Variación en la cantidad de clientes entre períodos.  
Fórmula: Variacion de Clientes = [Cantidad Clientes] - [Cantidad Clientes PA]

**Variacion de costo operacional**:  
Descripción: Variación en el costo operacional entre períodos.  
Fórmula: Variacion de costo operacional = [Costo Total + Envio] - [Costo Total + Envio PA]

**Variación Ingresos**:  
Descripción: Variación en los ingresos entre períodos.  
Fórmula: Variación Ingresos = [Ingresos] - [Ingresos PA]

**Variacion Utilidad Bruta**:  
Descripción: Variación en la utilidad bruta entre períodos.  
Fórmula: Variacion Utilidad Bruta = [Utilidad Bruta] - [Utilidad Bruta PA]

**Variacion Utilidad Neta**:  
Descripción: Variación en la utilidad neta entre períodos.  
Fórmula: Variacion Utilidad Neta = [Utilidad Neta] - [Utilidad Neta PA]

---

## Modelo de datos relacional
[![Captura-de-pantalla-2024-11-06-195237.png](https://i.postimg.cc/fyfCsTDq/Captura-de-pantalla-2024-11-06-195237.png)](https://postimg.cc/5QjLqJH8)
[![Captura-de-pantalla-2024-11-06-195254.png](https://i.postimg.cc/cC07FywR/Captura-de-pantalla-2024-11-06-195254.png)](https://postimg.cc/KkpgYW9j)

FactInternetSales: Tabla de hechos.
DimCustomer: Datos de clientes.
DimGeography: Ubicaciones geográficas específicas.
DimProduct: Datos de Productos.
DimDate: Tabla calendario.
DimProductCategory: Categoría de los productos.
DimProductSubcategory: Subcategoría de los productos.
DimPromotion: Promociones.
DimSalesTerritory: Regiones o territorios.

  Por petición del cliente se ha cancelado la carga de las tablas:
DimProductCategory
DimProductSubcategory
DimGeography

## Mockup

### Diapositiva 1: Ventas

Responde las siguientes consignas:
- ¿Cuál es el total de ingresos del período actual y del período anterior? ¿Qué porcentaje representa dicha variación?
- ¿Cuál es la cantidad vendida?
- ¿Cuántos clientes hay en cada país? El usuario desea ver esta demografía representada en mapas.
[![Captura-de-pantalla-2024-11-06-195314.png](https://i.postimg.cc/cChRj9BV/Captura-de-pantalla-2024-11-06-195314.png)](https://postimg.cc/2LqLLxJw)

### Diapositiva 1: Costos

Responde las siguientes consignas:
- ¿Cuál es el costo de los bienes vendidos (COGS) del período actual y del período anterior? ¿En qué porcentaje varía?
- ¿Cómo se distribuyen los ingresos, el COGS y la utilidad bruta mensualmente?
[![Captura-de-pantalla-2024-11-06-195321.png](https://i.postimg.cc/RZkHPxzW/Captura-de-pantalla-2024-11-06-195321.png)](https://postimg.cc/VJFv6Tvw)

### Diapositiva 2: Utilidades

Responde las siguientes consignas:
¿Cuál es la utilidad bruta del período actual y del período anterior? ¿Y la utilidad neta? ¿Cuál es el porcentaje de variación de ambas utilidades?
¿Qué utilidad (bruta y neta) tuvo cada segmento (categoría) y subcategoría de producto?

[![Captura-de-pantalla-2024-11-06-195329.png](https://i.postimg.cc/C1YfgC5Y/Captura-de-pantalla-2024-11-06-195329.png)](https://postimg.cc/NyJ08HRP)

### Diapositiva 3: Eficiencia

Responde las siguientes consignas:
Los usuarios desean ver además el Ratio Costo operacional versus LY (COGS + freight / Ingresos), el porcentaje de margen de utilidad bruta y utilidad neta y el porcentaje de COGS mostrado de manera eficiente en medidores (o tacómetros).

[![Captura-de-pantalla-2024-11-06-195335.png](https://i.postimg.cc/gj2XwY3T/Captura-de-pantalla-2024-11-06-195335.png)](https://postimg.cc/xJBThVxv)

### Diapositiva 4: Negocios Usa

Responde las siguientes consignas:
Como adicional, el usuario solicita ver de manera detallada indicadores del negocio de Estados Unidos donde se muestre por cada provincia y ciudad el segmento de producto (categoría), los ingresos, utilidades, COGS, márgenes (bruto y neto), y el costo de envío. Todo lo anterior desean ver resumido en una tabla. Por otro lado se solicitó un gráfico que muestre el COGS y el % de margen bruto (utilidad bruta) por ciudad y otro comparativo que muestre los ingresos acumulados del período actual versus los del período anterior.

[![Captura-de-pantalla-2024-11-06-195340.png](https://i.postimg.cc/GtgBGZQh/Captura-de-pantalla-2024-11-06-195340.png)](https://postimg.cc/fSXThqp6)

---

## Dashboard

### Página 1: Portada

Descripción: La portada del dashboard debe ser simple y amigable, orientada a facilitar la navegación del usuario dentro del informe.

[![Captura-de-pantalla-2024-11-06-195347.png](https://i.postimg.cc/GhN82ntX/Captura-de-pantalla-2024-11-06-195347.png)](https://postimg.cc/xcvTx7Km)

### Página 2: Ventas

Descripción: Esta página está dedicada a proporcionar métricas y gráficos clave que instruyan sobre el rendimiento de las ventas de la empresa.

[![Captura-de-pantalla-2024-11-06-195359.png](https://i.postimg.cc/W1kDqYJT/Captura-de-pantalla-2024-11-06-195359.png)](https://postimg.cc/18yRLHHj)

### Página 3: Costos
Descripción: En esta página se presentan métricas y gráficos relacionados con los costos de la empresa.

[![Captura-de-pantalla-2024-11-06-195411.png](https://i.postimg.cc/rp2D1GSW/Captura-de-pantalla-2024-11-06-195411.png)](https://postimg.cc/Yv82kWDq)

### Página 4: Utilidad
Descripción: Esta sección se enfoca en las utilidades de la empresa, tanto brutas como netas.

[![Captura-de-pantalla-2024-11-06-195416.png](https://i.postimg.cc/GpPHqBQf/Captura-de-pantalla-2024-11-06-195416.png)](https://postimg.cc/DWZvzwRQ)

### Página 5: Eficiencia
Descripción: Enfocada en métricas relacionadas con la eficiencia operativa de la empresa.

[![Captura-de-pantalla-2024-11-06-195422.png](https://i.postimg.cc/RFMq8MpZ/Captura-de-pantalla-2024-11-06-195422.png)](https://postimg.cc/67m94NNk)

### Página 6: Negocios USA
Descripción: Datos específicos del mercado estadounidense, presentando un análisis detallado de las operaciones en esta región.

[![Captura-de-pantalla-2024-11-06-195428.png](https://i.postimg.cc/Px2NnB0z/Captura-de-pantalla-2024-11-06-195428.png)](https://postimg.cc/624Bd1Wy)
---

## Resultados Principales y Líneas Futuras de Análisis

Los análisis realizados han revelado insights clave en las áreas de ventas, costos y eficiencia operativa, destacando un crecimiento sostenido en los ingresos y una mejora en los márgenes de utilidad neta. Las regiones de Estados Unidos han mostrado un desempeño particularmente fuerte, sugiriendo oportunidades de expansión. De cara al futuro, se recomienda profundizar en la segmentación por categoría de producto y explorar el impacto de las fluctuaciones de costos operativos en los márgenes, así como un análisis más granular del comportamiento del cliente para optimizar estrategias comerciales y de marketing.

---
