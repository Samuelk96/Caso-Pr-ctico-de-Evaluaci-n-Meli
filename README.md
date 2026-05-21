# MeLi Set Up - Automated Openings Dashboard MVP 🚀

## 📋 Descripción del Proyecto
Este repositorio contiene el Producto Mínimo Viable (MVP) automatizado desarrollado para el equipo de **Set Up de Mercado Libre** El objetivo principal es centralizar y dar visibilidad semanal al estado de ejecución, fases del Gantt e infraestructura para la apertura de **12 nuevos Service Centers (SC)** distribuidos en 4 estados de México.

La solución elimina una carga operativa de más de 2 horas semanales de preparación manual (derivada de la dispersión de datos en Monday.com, Google Sheets y registros de correo), reduciendo el tiempo de consolidación a **menos de 5 minutos** mediante un pipeline robusto en Python, auditoría predictiva con Inteligencia Artificial (Anthropic API) y un tablero de control ejecutivo interactivo.

---

## 🛠️ Stack Tecnológico
* **Core Data Pipeline:** Python 3.10+ (Pandas, NumPy) [cite: 3, 14]
* **Integración/Conectividad:** API de Google Sheets (`gspread` / `oauth2client`) [cite: 16, 19, 20]
* **AI Risk Management:** Anthropic API (`claude-sonnet-4-20250514`) [cite: 38, 43, 57]
* **Capa de BI & Visualización:** Looker Studio / Tableau [cite: 3, 31]

---

## ⚙️ Arquitectura de la Solución

### 1. Pipeline ETL & Modelado de Datos ()
* **Limpieza y Consolidación:** Ingesta y tratamiento de datos faltantes/atípicos utilizando `pandas`. Centraliza los estatus del Gantt operativo (pre-contrato, habilitación, compras, operación piloto).
* **Cálculo de KPIs Integrados:**
  * `avance total`: Medición estandarizada del progreso de infraestructura calculando el promedio simple (% avance) de las categorías core: Obra Civil, Compras y Licencias.
  * `dias_retraso`: Algoritmo dinámico que mide la desviación en días frente a la fecha estimada de apertura utilizando la fecha actual. Utiliza la función `.clip(lower=0)` para evitar penalizaciones en centros que van adelantados o en tiempo.
* **Automatización del Flujo:** Conexión directa mediante Service Account para actualizar la base de datos en la nube (`Set Up Dashboard`) de forma transparente para el usuario.

### 2. Módulo de IA para Detección de Riesgos ()
Utilizando el modelo especializado `claude-sonnet-4-20250514` de Anthropic, este módulo transforma el DataFrame consolidado a formato JSON y procesa un diagnóstico automatizado bajo un rol de Analista de Operaciones Logísticas. El output genera:
* Un **resumen ejecutivo** de alta densidad informativa (máximo 80 palabras).
* Identificación de los **3 principales riesgos operativos** de la semana.
* Propuesta de **acciones concretas** e inmediatas para mitigar los cuellos de botella en los centros con mayor rezago.

### 3. Capa de Business Intelligence (Looker Studio)
Diseño de tablero interactivo enfocado en ser **autosuficiente para el negocio**:
* **Control Room Ejecutivo:** Tarjetas de resumen con el conteo total de SCs en proceso, % promedio de avance general y un contador crítico para proyectos con retrasos mayores a 14 días.
* **Análisis de Infraestructura:** Gráfica de barras apiladas que desglosa el avance por categoría (Obra Civil, Compras, Licencias) segmentado por estado, permitiendo detectar si el bloqueo es constructivo, de suministro o normativo.
* **Matriz de Riesgo Operativo:** Tabla de detalle ordenada de forma descendente por días de retraso, parametrizada con alertas semafóricas condicionales:
  * 🟢 **Verde:** Retraso < 7 días.
  * 🟡 **Amarillo:** Retraso entre 7 y 14 días.
  * 🔴 **Rojo:** Retraso > 14 días.
* **Filtros Dinámicos:** Segmentación avanzada por Estado, Tipo de SC (Estatus Gantt) y rango de fechas estimadas de apertura.
