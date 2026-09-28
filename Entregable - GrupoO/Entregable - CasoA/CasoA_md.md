# Caso A - Predicción de abandono de clientes (*Churn Prediction*)
Parte de: Alessandro Landaverde - Grupo O

## 1-Problema de negocio

### 1.1-Descripción del problema

Una tienda en línea (*e-commerce*) está perdiendo clientes y hoy se da cuenta hasta que el cliente ya dejó de comprar. Cada cliente que se va representa ingresos futuros perdidos por ventas, y conseguir uno nuevo suele costar varias veces más que retener a uno actual.

El reto es ANTICIPARSE: usar el comportamiento de compra de cada cliente (frecuencia, gasto, uso de promociones, quejas, etc) para **estimar su probabilidad de abandono** y actuar antes de que se vaya.

### 1.2-Variable objetivo (Y)

| Elemento | Detalle |
|---|---|
| **Variable objetivo** | `Churn` |
| **Valores** | `1` = el cliente **abandona** · `0` = el cliente **se queda** |
| **Tipo de variable** | Categórica binaria (discreta, no continua) |
| **Tipo de problema** | **Clasificación binaria** con aprendizaje supervisado (conocemos la respuesta histórica de cada cliente) |


### 1.3-Variables independientes (X)

| Variable del dataset | Descripción | Tipo | Uso |
|---|---|---|---|
| `TransactionCount` | Cantidad de transacciones del cliente | Numérica discreta | En modelo |
| `AvgTransactionAmount` | Monto promedio por transacción | Numérica continua | En modelo |
| `DaySinceLastOrder` | Días transcurridos desde la última orden | Numérica discreta | En modelo |
| `PromoInteractionCount` | Número de promociones utilizadas | Numérica discreta | En modelo |
| `FlagComplain` | `1` = con queja hist / `0` = no | Binaria | En modelo |
| `PreferredLoginDevice` | Dispositivo preferido para ingresar (computadora o celular) | Categórica | En modelo |
| `PreferedOrderCat` | Categoría de producto que más compra | Categórica | En modelo |
| `Gender`, `MaritalStatus` | Género y estado civil | Categóricas | No uso |
| `CustomerID`| Identificador único del cliente | Identificador | No uso |


### 1.4-Contexto de negocio y decisiones que apoyará el modelo

Actualmente dentro de la compañia se está creando un área de retención de clientes que de forma proactiva buscará fidelizar los clientes con alto riesgo de fuga. 

La creación de esta área fue requerida por el CEO de la empresa dado que a partir de análisis del área financiera se habían llegado a las siguientes conclusiones:
- La fuga de los clientes está generando un alto impacto negativo en las utilidades.
- Es mejor invertir en un incentivo para retener al cliente y no que se vaya.

En otras palabras, perder a un cliente cuesta más que otorgar un incentivo innecesario

Con la creación del modelo se pretende poder tener un listado de los clientes que poseen una probabilidad de abandono, ordenada desde la probabilidad más alta hacia la más baja. Con este insumo el área de retenciones, segmentará los clientes por probabilidad para poder enfocar una estrategia diferente.

## 2-Mapa de selección de algoritmos

### 2.1-Familia de algoritmos 

* Regresión Logística - Aplica por devolver una probabilidad interpretable
* Árboles de decisión - Aplica por ser fácil de interpretar

### 2.2-Restricciones del contexto

| Restricción | Situación en el caso | Implicación para el modelo |
|---|---|---|
| **Interpretabilidad** | El negocio debe entender por qué un cliente está en riesgo | Priorizar modelos explicables |
| **Velocidad** | Se debe calificar a toda la base de clientes periódicamente | Entrenamiento y predicción en segundos |
| **Desbalance** | Solo ≈ 16% de los clientes abandona | Considerar métodos de balanceos y evaluar con Recall y F1-Score |
| **Ética y privacidad** | Existen variables sensibles (género y estado civil) | Excluirlas del modelo (realmente no es necesario pero solo para simular la ética y privacidad)|
| **Calidad de datos** | Nulos, categorías repetidas y registros duplicados | Limpieza de datos|


### 2.3-Algoritmos candidatos y sus limitaciones 

| | **Regresión Logística** | **Árbol de Decisión** |
|---|---|---|
| **¿Cómo funciona?** | Combina las variables en una ecuación lineal y la transforma en una probabilidad (0 a 1) | Divide a los clientes con preguntas sucesivas  hasta formar grupos parecidos |
| **¿Por qué es candidato?** | Entrega la probabilidad de abandono y coeficientes fáciles de explicar| Genera reglas de negocio visuales|
| **Supuestos** | Variable objetivo binaria, observaciones independientes, sin multicolinealidad| No tiene supuestos estadísticos  |
| **Limitaciones** | No capta relaciones no lineales por sí sola; sensible a outliers y a la multicolinealidad; requiere escalar variables | Tiende al overfitting
