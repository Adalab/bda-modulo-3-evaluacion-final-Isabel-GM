## 📊 Análisis inicial de .describe().T` del dataset `Customer Flight Activity`

La exploración con `df_flight.describe().T` aporta información muy útil para entender la distribución general de las variables numéricas del dataset.

---------------------

### ✅ Principales insights detectados:

#### 1. Distribuciones generales
- **Flights Booked**: Media = 4.12, Mediana = 1.00 → Distribución asimétrica. La mayoría de clientes reserva pocos vuelos, pero hay algunos con muchos vuelos (máx: 21).
- **Flights with Companions**: Similar patrón, con una media baja (1.03) y máximos altos (hasta 11).
- **Points Redeemed** y **Dollar Cost Points Redeemed**: Tienen medias bajas pero desviaciones estándar muy altas -> algunos clientes redimen muchísimos puntos.

#### 2. Valores cero frecuentes
- Varias columnas como `Flights Booked`, `Distance`, `Points Redeemed` y `Dollar Cost Points Redeemed` tienen **mínimos en 0**, lo que puede reflejar meses sin actividad para algunos clientes.

#### 3. Rango y outliers
- Algunas columnas muestran valores extremos, como:
  - `Points Redeemed`: hasta 876 puntos redimidos.
  - `Dollar Cost Points Redeemed`: máximo de 71$, muy por encima del percentil 75 (que es 0.00).

#### 4. Validación de datos de fecha
- `Year`: media = 2017.5, mínimo = 2017, máximo = 2018 → confirman que los datos corresponden a solo dos años.
- `Month`: de 1 a 12, con media de 6.

---------------------------

## ℹ️ Exploración con `.info()` del dataset `Customer Flight Activity`

La función `df_flight.info()` nos proporciona una visión general de la estructura del dataset. A continuación se detallan los principales puntos observados:

---

### ✅ Tamaño del dataset
- **405.624 filas** y **10 columnas** → un dataset amplio, con un buen volumen de información para analizar.

### ✅ Valores nulos
- Todas las columnas tienen **405.624 valores no nulos**, por lo tanto:
  - **No hay valores nulos** en este dataset.
  - No se requiere tratamiento de nulos en esta fase.

### ✅ Tipos de datos (`dtype`)
- **9 columnas** tienen tipo de dato `int64`.
- **1 columna** (`Points Accumulated`) es `float64`, lo cual es lógico ya que puede tener valores decimales.
- Confirmamos que los decimales están representados correctamente con `punto (.)`, como espera Python.

### ✅ Memoria
- El DataFrame ocupa aproximadamente **30.9 MB en memoria**, lo cual es razonable para su tamaño.

---

### 🧠 Conclusión
Los datos están **completos y correctamente tipados**, lo cual nos permite pasar sin problemas a la siguiente fase: exploración visual, análisis de valores extremos o unión con otros datasets.

----------------------------------

## 📑 Duplicados en el dataset `Customer Flight Activity`

Se detectaron **1864 filas duplicadas**.  
Esto significa que existen **1864 registros que son idénticos en todas las columnas**.

---

### ✅ Opciones para tratar los duplicados

#### 1. Mantener los duplicados
- Puede ser válido si los datos duplicados reflejan situaciones reales repetidas (por ejemplo, vuelos múltiples del mismo tipo).
- Aun así, es recomendable dejar constancia de que los duplicados existen y han sido analizados.

#### 2. Eliminar los duplicados
- Si se asume que estas repeticiones no aportan valor o distorsionan el análisis estadístico, pueden eliminarse fácilmente con:

---

*** Análisis detallado de filas duplicadas ***

Uso  df_flight[df_flight.duplicated()].value_counts() para identificar exactamente cuáles son las filas duplicadas dentro del dataset Customer Flight Activity.

- Resultado observado:

Se identificaron 1848 combinaciones de filas duplicadas, lo que representa un total de 1864 filas repetidas.
Estas duplicaciones afectan a múltiples clientes, no solo a uno.
La mayoría de estas filas contienen valores en cero en todas las columnas de actividad:
Flights Booked = 0
Distance = 0
Points Accumulated = 0
Dollar Cost Points Redeemed = 0
etc.
Esto sugiere que se trata de meses sin actividad que han sido duplicados por error en el proceso de carga de datos.

# Conclusión:

Tras analizar las filas duplicadas en el dataset Customer Flight Activity, se identificaron 1864 registros repetidos exactamente (misma información en todas las columnas). La mayoría de estas filas contienen datos con actividad nula: cero vuelos, cero puntos, cero distancia, etc.

Dado que estos duplicados:

- No aportan información nueva,
- Representan posibles errores de carga en meses sin actividad,
- pueden distorsionar los resultados del análisis (por ejemplo, inflando los promedios o el número total de registros por cliente),

Decido eliminarlos del dataset antes de continuar con la fase de análisis y visualización. Esta limpieza mejora la calidad del dataset y permite obtener resultados más precisos.


-----------------------------------

## 🔎 Diferencia entre `.nunique()` y `.unique()` en pandas

Durante la exploración del DataFrame `df_flight`, hem utilizado el método `.nunique()` para obtener el número de valores únicos por columna.

---

### ✅ `.nunique()`
- Devuelve **cuántos valores únicos hay** en cada columna (o en una columna específica).
- Muy útil para detectar columnas con baja variabilidad o redundantes.

-----------------------

### csv Loyalty:

# .describe()

Análisis del resumen estadístico (.describe().T) del dataset Customer Loyalty History
La función df_loyalty.describe().T proporciona estadísticas básicas de las columnas numéricas. A continuación se detallan los principales hallazgos:

1. Salary

Solo hay datos de salario para 12.499 clientes, lo que indica que faltan más de 4.000 valores → se deberá tratar como valor nulo.
La media salarial es de 79.245 $, pero hay valores negativos (mínimo: -58.486 $), lo cual no tiene sentido y debe considerarse un error.
El máximo es de 407.228 $, lo que podría indicar un outlier o cliente de muy alto ingreso.

Tratamiento de salarios negativos

Durante la revisión de la columna Salary, se detectaron 20 valores negativos.
Aunque no es un número elevado, se considera que estos valores corresponden a errores de carga, ya que el salario no puede ser negativo en un contexto realista.

Como en ejercicios anteriores se han tratado como valores absolutos, se decide mantener esa misma lógica aquí.

Además:

El número de casos es muy bajo (20 de 16.737 registros), por lo que no distorsionará las estadísticas.
Así se evita eliminar información útil de esos clientes.
Decisión: transformar esos valores negativos a positivos utilizando .abs().


2. Cancellation Year y Cancellation Month

Solo 2.067 clientes tienen una fecha de cancelación registrada.
Esto indica que la mayoría siguen activos.
Las fechas y meses son correctos y no presentan errores aparentes.


# .info()

ℹ️ Análisis del .info() del dataset Customer Loyalty History
Al ejecutar df_loyalty.info(), se obtiene una visión general de la estructura y calidad de los datos. A continuación se resumen los puntos clave:

1. Detección de valores nulos

Aunque .info() no muestra explícitamente los valores "NaN", se puede detectar su presencia comparando los valores en la columna Non-Null Count.
Salary: tiene solo 12.499 valores no nulos, por lo tanto hay 4.238 valores nulos.
Cancellation Year y Cancellation Month: tienen 2.067 valores no nulos, lo que indica que la mayoría de los clientes no han cancelado su membresía.

3. Tipos de datos (dtype)

Cancellation Year y Cancellation Month: aparecen como float64 porque contienen valores nulos. Aunque semánticamente son enteros, pandas los convierte a float cuando hay nulos.
Si en algún momento se imputan o eliminan los nulos, se podrían convertir a tipo entero usando Int64 (con soporte para nulos).

Conclusión:
Los datos están en general bien tipados y organizados. Se identifican valores nulos relevantes en Salary y en las columnas de cancelación, lo que deberá ser tratado durante la limpieza. El resto de columnas presentan tipos adecuados para su análisis posterior.

# .nunique() 

Análisis de valores únicos con .nunique()
Se ejecutó df_loyalty.nunique() para conocer cuántos valores únicos existen por columna en el dataset. Este análisis permite identificar columnas con poca variación, posibles identificadores únicos, y variables útiles para segmentar o agrupar.

1. Country → 1 único valor. Todos los clientes pertenecen al mismo país. Esta columna no aporta valor analítico y puede eliminarse.

Conclusión:
La mayoría de columnas muestran una variabilidad adecuada para el análisis.  Eliminar la columna Country (sin variación), y revisar si Postal Code aporta valor adicional respecto a ciudad y provincia.