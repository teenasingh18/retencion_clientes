# Sistema de Retención de Clientes con Machine Learning e Inteligencia Artificial Generativa
# Teena Singh

## Descripción del Proyecto

Este proyecto fue una prueba técnica para una posición de Ciencia de Datos en **Cochez**.

El objetivo es construir un sistema que permita mejorar la retención de clientes mediante mensajes de fidelización. La solución tenía dos etapas principales:

* **Segmentar clientes** según su comportamiento de compra utilizando Machine Learning.
* **Generar mensajes personalizados de fidelización** utilizando un Modelo de Lenguaje (LLM), adaptando el contenido según el perfil de cada cliente.

La solución integra **SQL, Python, Machine Learning e Inteligencia Artificial Generativa** 

---

# Arquitectura de la Solución

1. Base de Datos Relacional -> Consulta SQL para extracción de las métricas RFM y categoría favorita
2. Generación de Dataset sintético utilizando Python       
3. Preprocesamiento de datos (exploración, visualización, análisis)
4. Segmentación de clientes en 3 clusters utilizando K-Means
5. Análisis de Clusters para clasificar cada cluster acorde al perfil del cliente
6. Creación de perfil de cliente
7. Prompt Engineering (Role Prompting + Few-Shot Prompting)
8. LLM / Respuesta Mock
9. Mensaje personalizado para cada cliente

---

# Tecnologías Utilizadas

* Python
* Pandas
* NumPy
* Scikit-learn
* Matplotlib
* SQL
* OpenAI SDK
* python-dotenv
* Jupyter Notebook

---

# Estructura del Proyecto

retencion_clientes/
│
├── notebooks
│   └── retencion_clientes.ipynb
├── README.md
├── .gitignore
├── .env (no incluido en el repositorio)

---

# Tarea 1 – Extracción y Procesamiento de Datos

En la primera etapa se hizo una consulta de SQL para tener, por cada cliente:

* ID del cliente.
* Recencia (días desde la última compra).
* Frecuencia (cantidad total de compras).
* Monto total gastado.
* Categoría de producto más comprada.

Como no se contaba con una base de datos real, se generó un **dataset sintético de 1,000 clientes** utilizando Python.

Para simular un comportamiento de compra más realista se utilizaron diferentes distribuciones estadísticas:

* **Recencia:** Distribución Exponencial.
* **Frecuencia:** Distribución de Poisson.
* **Monto Total:** Distribución Lognormal.

---

# Tarea 2 – Segmentación de Clientes

Se utilizó el algoritmo **K-Means** para agrupar clientes con comportamientos similares.

Antes de entrenar el modelo, las variables numéricas fueron estandarizadas mediante **StandardScaler**, con el objetivo de evitar que variables con escalas mayores (como monto) dominaran el proceso de agrupamiento.

Como resultado, se identificaron tres segmentos:

* Alto Valor
* Estándar
* En Riesgo

El modelo fue evaluado utilizando el **Silhouette Score**, obteniendo un valor de **0.367**, lo que indica una separación moderada entre los grupos. Este resultado es esperado considerando que los datos fueron generados de forma sintética y representan comportamientos continuos de los clientes.

Utilizé K-Means porque:

* No existen etiquetas reales de clientes.
* Es un algoritmo muy usado para segmentación.
* Permite identificar grupos con características similares.

---

# Tarea 3 – Personalización con Inteligencia Artificial Generativa

Una vez identificado el segmento de cada cliente, se genera un perfil para cada cliente con estas características:

* Segmento asignado por el modelo
* Categoría favorita
* Monto total gastado

Este perfil es utilizado por un Modelo de Lenguaje (LLM) para generar un mensaje de fidelización personalizado.

Utilizé **Prompt Engineering**, específicamente:

* **Role Prompting**, asignando al modelo el rol de especialista en marketing de Cochez.
* **Few-Shot Prompting**, proporcionando un ejemplo de entrada y salida para guiar el formato esperado.

La respuesta se genera siguiendo un formato JSON que tenga:

```json
{
  "asunto": "...",
  "cuerpo_mensaje": "...",
  "cupon_descuento_sugerido": "..."
}
```
---


# Implementación del Mock

Para evitar el consumo de créditos de una API de pago, se implementó un **mock**.

Si no existe una API Key configurada, el sistema crea automáticamente una respuesta que mantiene exactamente la estructura JSON que produciría un Modelo de Lenguaje real.

---

# Cómo Ejecutar el Proyecto

## 1. Clonar el repositorio

```bash
git clone https://github.com/teenasingh18/retencion_clientes.git
```

## 2. Instalar las dependencias

```bash
pip install pandas numpy matplotlib scikit-learn openai python-dotenv jupyter
```

## 3. (Opcional) Configurar una API Key

Crear un archivo `.env` en la raíz del proyecto:

OPENAI_API_KEY=tu_api_key

Si la variable no está configurada, el proyecto utilizará la respuesta mock

## 4. Ejecutar el Notebook

Abrir el notebook en Jupyter o Visual Studio Code y ejecutar las celdas en orden.

---

# Decisiones Técnicas

* Se utilizó un dataset sintético debido a la ausencia de datos reales.
* Se seleccionó K-Means por tratarse de un problema sin etiquetas.
* Se estandarizaron las variables antes de entrenar el modelo para disminuir la diferencia de escala.
* Se hizo un mock para permitir la ejecución completa del proyecto sin consumir créditos de una API externa.

---

# Escalabilidad en Producción

Este sistema podría evolucionar de la siguiente forma:

* Conectar  a una base de datos relacional real.
* Entrenar un modelo supervisado de predicción de churn utilizando datos históricos reales.
* Utilizar Structured Outputs mediante la API de OpenAI para garantizar la generación de JSON válidos.
* Programar ejecuciones periódicas para actualizar cada cierto tiempo la segmentación de clientes y generar nuevas campañas de fidelización.