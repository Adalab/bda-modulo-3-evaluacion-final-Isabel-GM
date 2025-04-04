## 📊 Análisis inicial de .describe().T` del dataset `Customer Flight Activity`

La exploración con `df_flight.describe().T` aporta información muy útil para entender la distribución general de las variables numéricas del dataset.

---------------------

## ✅ Principales insights detectados:

# 1. Distribuciones generales
- Flights Booked: Media = 4.12, Mediana = 1.00 → Distribución asimétrica. La mayoría de clientes reserva pocos vuelos, pero hay algunos con muchos vuelos (máx: 21).
- Flights with Companions: Similar patrón, con una media baja (1.03) y máximos altos (hasta 11).
- Points Redeemed y Dollar Cost Points Redeemed: Tienen medias bajas pero desviaciones estándar muy altas -> algunos clientes redimen muchísimos puntos.

# 2. Valores cero frecuentes
- Varias columnas como `Flights Booked`, `Distance`, `Points Redeemed` y `Dollar Cost Points Redeemed` tienen **mínimos en 0**, lo que puede reflejar meses sin actividad para algunos clientes.

# 3. Rango y outliers
- Algunas columnas muestran valores extremos, como:
  - `Points Redeemed`: hasta 876 puntos redimidos.
  - `Dollar Cost Points Redeemed`: máximo de 71$, muy por encima del percentil 75 (que es 0.00).

# 4. Validación de datos de fecha
- `Year`: media = 2017.5, mínimo = 2017, máximo = 2018 → confirman que los datos corresponden a solo dos años.
- `Month`: de 1 a 12, con media de 6.

---------------------------

### ℹ️ Exploración con `.info()` del dataset `Customer Flight Activity`

La función `df_flight.info()` nos proporciona una visión general de la estructura del dataset. A continuación se detallan los principales puntos observados:

---

# ✅ Tamaño del dataset
- **405.624 filas** y **10 columnas** → un dataset amplio, con un buen volumen de información para analizar.

# ✅ Valores nulos
- Todas las columnas tienen **405.624 valores no nulos**, por lo tanto:
  - **No hay valores nulos** en este dataset.
  - No se requiere tratamiento de nulos en esta fase.

# ✅ Tipos de datos (`dtype`)
- **9 columnas** tienen tipo de dato `int64`.
- **1 columna** (`Points Accumulated`) es `float64`, lo cual es lógico ya que puede tener valores decimales.
- Confirmamos que los decimales están representados correctamente con `punto (.)`, como espera Python.

# ✅ Memoria
- El DataFrame ocupa aproximadamente **30.9 MB en memoria**, lo cual es razonable para su tamaño.

---

## 🧠 Conclusión
Los datos están **completos y correctamente tipados**, lo cual nos permite pasar sin problemas a la siguiente fase: exploración visual, análisis de valores extremos o unión con otros datasets.

----------------------------------

## 📑 Duplicados en el dataset `Customer Flight Activity`

Se detectaron **1864 filas duplicadas**.  
Esto significa que existen **1864 registros que son idénticos en todas las columnas**.

---

## ✅ Opciones para tratar los duplicados

# 1. Mantener los duplicados
- Puede ser válido si los datos duplicados reflejan situaciones reales repetidas (por ejemplo, vuelos múltiples del mismo tipo).
- Aun así, es recomendable dejar constancia de que los duplicados existen y han sido analizados.

# 2. Eliminar los duplicados
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

***  Tratamiento de salarios negativos ***

Durante la revisión de la columna Salary, se detectaron 20 valores negativos.
Aunque no es un número elevado, se considera que estos valores corresponden a errores de carga, ya que el salario no puede ser negativo en un contexto realista.

Como en ejercicios anteriores se han tratado como valores absolutos, se decide mantener esa misma lógica aquí.

Además:

El número de casos es muy bajo (20 de 16.737 registros), por lo que no distorsionará las estadísticas.
Así se evita eliminar información útil de esos clientes.
Decisión: transformar esos valores negativos a positivos utilizando .abs().

*** Gestion de nulos de Salary ***

🧼Imputación de valores nulos en la columna Salary
Tras analizar la distribución de la columna Salary, observo que:

La media es 79.359 € y la mediana es 73.455 €.
Hay cierta dispersión (desviación típica de 34.750 €).
El valor máximo es muy elevado (407.228 €), lo que sugiere posibles outliers.

Decido rellenar los valores nulos con la mediana, ya que:
- Es una medida robusta frente a valores extremos.
- Refleja mejor el comportamiento típico del salario en este contexto.


2. Cancellation Year y Cancellation Month

Solo 2.067 clientes tienen una fecha de cancelación registrada.
Esto indica que la mayoría siguen activos.
Las fechas y meses son correctos y no presentan errores aparentes.

# Gestión de nulos de las columnas Cancellation Year y Cancellation Month: 

Estas olumnas indican cuando el cliente dejó su membresía, por lo que si un cliente sigue activo, no tiene fecha de cancelacion y aparece como NaN

- ¿Qué hacer con estos nulos? En este caso, no se deben imputar ni eliminar, porque:
    1. Su ausencia tiene un significado: el cliente sigue siendo parte del programa.
    2. Eliminarlos sería perder información útil.
    3. Imputarlos sería inventar algo que no ocurrió

# 🧾 Nueva columna: `Active Customer`

Se ha creado una nueva columna llamada `Active Customer` para indicar si un cliente sigue activo o no en el programa de fidelización.

- Se basa en la columna `Cancellation Year`:
  - Si el valor es `NaN`, significa que el cliente **sigue activo**.
  - Si hay un año registrado, indica que **el cliente ha cancelado** su membresía.

- Se han utilizado los valores `"Yes"` para clientes activos y `"No"` para clientes inactivos.









# .info()

*** ℹ️ Análisis del .info() del dataset Customer Loyalty History ***
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


 *** Tipo de dato en columnas de cancelación ***

Durante la exploración inicial las columnas `Cancellation Year` y `Cancellation Month` aparecen como tipo `float64`, a pesar de que representan años y meses (valores enteros).


 ¿Por qué son `float64` y no `int64`?

Esto ocurre porque ambas columnas contienen valores `NaN`, los cuales pandas solo puede almacenar en columnas de tipo `float` (en su comportamiento por defecto).  
Los tipos `int` normales (`int64`) **no pueden contener `NaN`**, por lo que se convierte todo a `float64` automáticamente.


Decido **mantener ambas columnas como `float64`**, ya que:

- Los `NaN` tienen un significado importante (clientes activos).
- No es necesario cambiar el tipo para el análisis actual.
- Podria usar un tipo especial llamado "Int64" (con mayúscula), que sí acepta NaN pero forzar la conversión a `int` podría provocar errores o pérdida de información.

Si en un futuro se desea trabajar con enteros y permitir `NaN`, podría considerarse convertirlas a `Int64` (entero "nullable" de pandas).


# .nunique() 

Análisis de valores únicos con .nunique()
Se ejecutó df_loyalty.nunique() para conocer cuántos valores únicos existen por columna en el dataset. Este análisis permite identificar columnas con poca variación, posibles identificadores únicos, y variables útiles para segmentar o agrupar.

1. Country → 1 único valor. Todos los clientes pertenecen al mismo país. Esta columna no aporta valor analítico y puede eliminarse.

Conclusión:
La mayoría de columnas muestran una variabilidad adecuada para el análisis.  Eliminar la columna Country (sin variación), y revisar si Postal Code aporta valor adicional respecto a ciudad y provincia.


------------------------------

### 🔗 Unión de los datasets: vuelos + información de cliente

Decidido limpiar primero cada dataset por separado (df_loyalty y df_flight) antes de unirlos, con el objetivo de:
- Corregir valores erróneos (como salarios negativos).
- Imputar nulos con la lógica adecuada según cada contexto.
- Eliminar duplicados exactos que no aportaban información.
Así aseguro que el merge se hace sobre datos limpios y consistentes, evitando arrastrar errores al DataFrame final.

Para combinar la información de comportamiento de vuelo (`df_flight`) con los datos personales y demográficos (`df_loyalty`), se realiza una unión mediante la columna común `Loyalty Number`.


### 🔗 Unión eficiente de los datasets

Se realiza la unión de los dos conjuntos de datos `df_flight` (actividad de vuelo) y `df_loyalty` (información del cliente), utilizando la clave común `Loyalty Number`.

Se emplea un **inner join**, ya que se considera la forma más eficiente para este análisis:
- Solo conserva las filas que están presentes en ambos datasets.
- Elimina datos incompletos o no vinculados.
- Optimiza el tamaño del DataFrame resultante.

df_merged = pd.merge(df_flight, df_loyalty, on='Loyalty Number', how='inner')

-------------------------

### VISUALIZACION: 

# 1. ¿Como se distribuye la cantidad de vuelos reservados por mes durante el año?

# 📊 Distribución de vuelos reservados por mes

El objetivo de esta visualización es analizar **cómo varía la cantidad de vuelos reservados (`Flights Booked`) a lo largo del año**, agrupando los datos por mes.

Selecciono el gráfico de barras por los siguientes motivos:

- Permite **comparar cantidades entre categorías** discretas (en este caso, los meses del año).
- Muestra con claridad las diferencias entre los meses.

### 📊 Análisis: Distribución de vuelos reservados por mes

Tras representar gráficamente la cantidad total de vuelos reservados por mes, se observan los siguientes patrones:

---

# 🔼 Meses con mayor número de vuelos reservados:

En la visualización se aprecia claramente una estacionalidad en el comportamiento de los usuarios.

Los meses de verano, especialmente julio, seguido por junio y agosto, destacan como los períodos con mayor volumen de vuelos reservados. Este patrón es coherente con las vacaciones estivales, tanto escolares como laborales, en las que muchas personas aprovechan para viajar, lo que provoca un aumento significativo de la demanda.

Otro pico relevante se observa en diciembre, mes asociado a las celebraciones navideñas y de fin de año, durante el cual también es habitual que se realicen desplazamientos familiares o de ocio.

Por otro lado, los meses de enero y febrero presentan los niveles más bajos de reservas. Esto puede deberse a la conocida "cuesta de enero", un periodo posterior a las fiestas en el que muchas personas ajustan su presupuesto. Además, el clima invernal podría influir negativamente en las decisiones de viaje, reduciendo así la demanda.

En conjunto, el gráfico refleja cómo los hábitos de viaje varían a lo largo del año, estando fuertemente condicionados por factores como las vacaciones, las festividades y el contexto económico


## 2. ¿Existe una relación entre la distancia de los vuelos y los puntos acumulados por los cliente?


# ✅ Interpretación:

En la visualización se observa una **relación lineal positiva muy clara y marcada**.  
Los puntos siguen trayectorias diagonales bien definidas, lo cual indica que a **mayor distancia recorrida**, los clientes **acumulan más puntos**.

Esta relación es lógica dentro del contexto de un programa de fidelización, donde los puntos suelen asignarse en función de la distancia volada.

Existe una **correlación directa, sistemática y consistente** entre ambas variables. Esto sugiere que la asignación de puntos por vuelo se realiza siguiendo una fórmula clara y estable en función de la distancia.

## 3. ¿Cuál es la distribución de los clientes por provincia o estado?

He utilizado un gráfico de barras horizontales para visualizar la distribución de clientes únicos por provincia.
El gráfico muestra una **distribución claramente desigual** entre las diferentes regiones. Las provincias con mayor número de clientes son:

- **Ontario**, con diferencia, es la provincia con más clientes registrados.
- Le siguen **British Columbia** y **Quebec**, también con una alta concentración de clientes.

Por otro lado, provincias como **Yukon**, **Newfoundland** y **Prince Edward Island** tienen una representación significativamente menor.

Este tipo de distribución puede deberse a varios factores como la densidad de población, la infraestructura aérea disponible o el enfoque comercial de la compañía en determinadas regiones.

La mayor parte de los clientes se concentra en unas pocas provincias, lo cual puede ser clave para orientar decisiones de negocio, campañas de fidelización o expansión de servicios.


## 4.  ¿Cómo se compara el salario promedio entre los diferentes niveles educativos de los clientes?

El gráfico muestra cómo varía el salario promedio de los clientes en función de su nivel educativo. Se observa una **tendencia clara**: a mayor nivel educativo, mayor salario promedio.

- Los clientes con **Doctorado** son los que presentan el salario promedio más alto.
- Les siguen aquellos con **Master**, también con ingresos elevados.
- **Bachelor** y **College** muestran salarios similares, en un rango medio.
- El grupo **High School or Below** tiene el salario promedio más bajo.

Esta distribución es coherente con lo esperado, reflejando que **la formación académica tiene un impacto positivo en los ingresos** de los clientes.

# 5.  ¿Cuál es la proporción de clientes con diferentes tipos de tarjetas de fidelidad?

Para resolver este ejercicio, parto de la premisa de que cada cliente puede aparecer múltiples veces en el DataFrame original (`df_unido`) debido al registro mensual de su actividad, sin embargo, el tipo de tarjeta de fidelidad (`Loyalty Card`) **no cambia** para un mismo cliente, por lo que **es fundamental contar cada cliente una sola vez**.


