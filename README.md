# MeLi Set Up - Automated Openings Dashboard MVP 🚀

## 📋 Descripción del Proyecto
[cite_start]Este repositorio contiene el Producto Mínimo Viable (MVP) automatizado desarrollado para el equipo de **Set Up de Mercado Libre**[cite: 1, 10]. [cite_start]El objetivo principal es centralizar y dar visibilidad semanal al estado de ejecución, fases del Gantt e infraestructura para la apertura de **12 nuevos Service Centers (SC)** distribuidos en 4 estados de México[cite: 5, 6, 10].

[cite_start]La solución elimina una carga operativa de más de 2 horas semanales de preparación manual (derivada de la dispersión de datos en Monday.com, Google Sheets y registros de correo) [cite: 7, 8][cite_start], reduciendo el tiempo de consolidación a **menos de 5 minutos** mediante un pipeline robusto en Python [cite: 3, 10, 12][cite_start], auditoría predictiva con Inteligencia Artificial (Anthropic API) [cite: 3, 37, 38] [cite_start]y un tablero de control ejecutivo interactivo[cite: 3, 10, 31].

---

## 🛠️ Stack Tecnológico
* [cite_start]**Core Data Pipeline:** Python 3.10+ (Pandas, NumPy) [cite: 3, 14]
* [cite_start]**Integración/Conectividad:** API de Google Sheets (`gspread` / `oauth2client`) [cite: 16, 19, 20]
* [cite_start]**AI Risk Management:** Anthropic API (`claude-sonnet-4-20250514`) [cite: 38, 43, 57]
* [cite_start]**Capa de BI & Visualización:** Looker Studio / Tableau [cite: 3, 31]

---

## ⚙️ Arquitectura de la Solución

### 1. Pipeline ETL & Modelado de Datos (`src/etl_pipeline.py`)
* [cite_start]**Limpieza y Consolidación:** Ingesta y tratamiento de datos faltantes/atípicos utilizando `pandas`[cite: 14]. [cite_start]Centraliza los estatus del Gantt operativo (pre-contrato, habilitación, compras, operación piloto)[cite: 6].
* **Cálculo de KPIs Integrados:**
  * [cite_start]`avance total`: Medición estandarizada del progreso de infraestructura calculando el promedio simple (% avance) de las categorías core: Obra Civil, Compras y Licencias[cite: 15, 24].
  * [cite_start]`dias_retraso`: Algoritmo dinámico que mide la desviación en días frente a la fecha estimada de apertura utilizando la fecha actual[cite: 15, 25]. [cite_start]Utiliza la función `.clip(lower=0)` para evitar penalizaciones en centros que van adelantados o en tiempo[cite: 27].
* [cite_start]**Automatización del Flujo:** Conexión directa mediante Service Account para actualizar la base de datos en la nube (`Set Up Dashboard`) de forma transparente para el usuario[cite: 20, 29, 30].

### 2. Módulo de IA para Detección de Riesgos (`src/ai_module.py`)
[cite_start]Utilizando el modelo especializado `claude-sonnet-4-20250514` de Anthropic [cite: 38, 57][cite_start], este módulo transforma el DataFrame consolidado a formato JSON [cite: 39] [cite_start]y procesa un diagnóstico automatizado bajo un rol de Analista de Operaciones Logísticas[cite: 48]. El output genera:
* [cite_start]Un **resumen ejecutivo** de alta densidad informativa (máximo 80 palabras)[cite: 50].
* [cite_start]Identificación de los **3 principales riesgos operativos** de la semana[cite: 51].
* [cite_start]Propuesta de **acciones concretas** e inmediatas para mitigar los cuellos de botella en los centros con mayor rezago[cite: 41, 52].

### 3. Capa de Business Intelligence (Looker Studio)
[cite_start]Diseño de tablero interactivo enfocado en ser **autosuficiente para el negocio**[cite: 31, 61]:
* [cite_start]**Control Room Ejecutivo:** Tarjetas de resumen con el conteo total de SCs en proceso, % promedio de avance general y un contador crítico para proyectos con retrasos mayores a 14 días[cite: 33].
* [cite_start]**Análisis de Infraestructura:** Gráfica de barras apiladas que desglosa el avance por categoría (Obra Civil, Compras, Licencias) segmentado por estado, permitiendo detectar si el bloqueo es constructivo, de suministro o normativo[cite: 34].
* [cite_start]**Matriz de Riesgo Operativo:** Tabla de detalle ordenada de forma descendente por días de retraso, parametrizada con alertas semafóricas condicionales[cite: 35]:
  * [cite_start]🟢 **Verde:** Retraso < 7 días[cite: 35].
  * [cite_start]🟡 **Amarillo:** Retraso entre 7 y 14 días[cite: 35].
  * [cite_start]🔴 **Rojo:** Retraso > 14 días[cite: 35].
* [cite_start]**Filtros Dinámicos:** Segmentación avanzada por Estado, Tipo de SC (Estatus Gantt) y rango de fechas estimadas de apertura[cite: 36].

---

## 🚀 Guía de Ejecución y Reproducibilidad

1. **Clonar el repositorio:**
   ```bash
   git clone [https://github.com/tu-usuario/nombre-de-tu-repo.git](https://github.com/tu-usuario/nombre-de-tu-repo.git)
   cd nombre-de-tu-repo
