
### 🧾 Informe completo del Análisis - Evaluación Final Módulo 3

## 🌟 Objetivo del proyecto

El objetivo de este análisis es comprender el comportamiento y las características de los clientes de una aerolínea a partir de dos fuentes de datos: actividad de vuelos mensuales y datos demográficos/fidelización. El análisis busca aportar información valiosa para mejorar la retención de clientes, personalizar promociones y optimizar la estrategia de fidelización.

---

### 🔎 Exploración y limpieza de datos

## Archivos utilizados:

- `Customer Flight Activity.csv`
- `Customer Loyalty History.csv`

## Principales acciones realizadas:

- Revisión de estructura de datos (`.info()`, `.describe()`, `.shape()`).
- Eliminación de **duplicados completos** en `Customer Flight Activity` (1864 filas).
- Comprobación de nulos:
  - En `Salary` y en `Cancellation Year/Month`.
        - Conversión de **valores negativos en `Salary`** a su valor absoluto.
        - Normalización de textos en variables categóricas (uso de `.str.title()`).
        - Creación de columna `active_customer` (True/False) para identificar si un cliente sigue activo en base a los nulos en cancelaciones.

---

### 🔗 Integración de los datos

- Se realizó un **merge** entre los dos datasets a través de la columna `Loyalty Number`.
- Se usó un `left join` para conservar toda la información de actividad de vuelo, aunque no todos los clientes estuvieran activos.
- El DataFrame final `df_unido` contiene **403.760 registros** y **25 columnas**.

---

### 📊 Visualizaciones y análisis

## 1. 🌐 Vuelos por mes
Se observan picos de actividad en **julio, agosto y diciembre**, lo que sugiere patrones estacionales. Enero y febrero son los meses más bajos.

## 2. 🔹 Relación entre distancia y puntos acumulados
Existe una correlación positiva clara: **a mayor distancia recorrida, mayor cantidad de puntos acumulados**.

## 3. 📅 Clientes por provincia
Ontario y British Columbia concentran la mayor cantidad de clientes. Se utilizó `drop_duplicates()` para asegurar que cada cliente solo cuente una vez.

## 4. 🏫 Salario promedio por nivel educativo
Se observó que a mayor nivel educativo, **mayor salario promedio** (Doctorado y Máster en los niveles superiores).

## 5. 🍓 Distribución de tarjetas de fidelidad
La mayoría de clientes tienen tarjeta **Star (45.6%)**, seguida de **Nova (33.9%)** y **Aurora (20.5%)**.

## 6. 🫋 Estado civil y género
Distribución muy equilibrada entre hombres y mujeres en todos los estados civiles. El grupo mayoritario es "Married".

---

### 🧰 Conclusiones generales

- Existen **picos claros de actividad** en los meses de verano y navidad, que pueden aprovecharse para ofertas estacionales.
- El **nivel educativo se relaciona fuertemente con el salario**, lo que puede influir en estrategias de segmentación.
- La mayoría de clientes usan tarjetas **Star** y **Nova**, por lo que estas deberían recibir atención prioritaria en promociones.
- La nueva variable `active_customer` permite distinguir clientes actuales de inactivos, clave para estrategias de retención.

---

### ⏳ Next steps / Futuras acciones

1. **Usar la variable `active_customer`** para:
   - Analizar las diferencias entre clientes activos e inactivos.
   - Estudiar por qué se cancelan algunas membresías.
   - Crear un modelo de predicción de cancelación o abandono.

2. **Segmentar clientes** según salario, educación o tipo de tarjeta.

3. **Diseñar promociones personalizadas** para:
   - Meses de baja actividad (enero, febrero).
   - Clientes con tarjeta Aurora (menos frecuentes).

4. **Ampliar el análisis**:
   - Incorporar variables temporales para análisis de tendencias.
   - Analizar clientes por antigüedad y fidelidad (usando Enrollment Year).

---

### 📄 Informe creado por

**Isabel García** – [@Isabel-GM](https://github.com/Isabel-GM)

Proyecto final para el Módulo 3 de Adalab

