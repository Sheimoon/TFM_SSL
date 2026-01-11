# Influencia de la Disponibilidad de Nutrientes en la Metástasis del Melanoma  
**Pipelines empleados en el análisis de DNA-seq y scRNA-seq**

## Descripción general
Este repositorio contiene todas las libretas de código y scripts utilizados para analizar el impacto de la disponibilidad de aminoácidos esenciales en la capacidad metastásica de células de melanoma murino. El estudio combina **seguimiento clonal mediante códigos de barras (DNA-seq)** y **análisis transcriptómico a nivel unicelular (scRNA-seq)** en condiciones *in vitro* e *in vivo*. El código incluye los pasos de preprocesado de datos, control de calidad, análisis de abundancia clonal, expresión diferencial, análisis funcional, diversidad intraclonal y estimación de la actividad de rutas metabólicas.

---

## 📁 Estructura del repositorio y libretas de código

### 🔹 Análisis de DNA-seq

#### `DNA_seq.ipynb`
Libreta principal para el análisis de la secuenciación de códigos de barras. Incluye:
- Carga y combinación de archivos CSV generados por el pipeline Larryseq  
- Asignación de barcodes a muestras, incluyendo corrección mediante distancia de Hamming  
- Filtrado de clústeres de baja calidad  
- Cálculo del número total de lecturas por muestra  
- Análisis de correlación de Pearson entre muestras  
- Visualización de la abundancia relativa de barcodes mediante mapas de calor  
- Análisis estadístico de cambios de abundancia clonal entre etapas experimentales (Pool_before, Mouse y Pool_after), empleando pruebas de Kruskal–Wallis y Dunn con corrección FDR  

---

### 🔹 Preprocesado de scRNA-seq

#### `Seurat_a_Scanpy.Rmd`
Script en R para la conversión de objetos Seurat (generados con CloneRanger) a formatos compatibles con Scanpy. Exporta:
- Matriz de conteos en formato MTX  
- Identificadores de genes y células  
- Metadatos de las células  

Este paso permite realizar el análisis posterior en Python.

#### `Preprocesado_muestras.ipynb`
Pipeline de preprocesado de datos de scRNA-seq en Scanpy, que incluye:
- Creación de objetos AnnData  
- Detección y eliminación de dobletes mediante SOLO (scVI)  
- Identificación de genes mitocondriales y ribosomales  
- Asignación de barcode y condición experimental a cada célula  
- Anotación del ciclo celular  
- Cálculo de métricas de calidad y filtrado de células  

---

### 🔹 Análisis de expresión diferencial y funcional

#### `Analisis_funcional.ipynb`
Libreta dedicada a:
- Normalización y transformación logarítmica de los datos  
- Regresión de covariables técnicas y biológicas  
- Reducción de dimensionalidad (PCA), construcción del grafo de vecinos y clusterización mediante Leiden  
- Análisis de expresión diferencial entre clústeres y condiciones experimentales  
- Visualización de genes marcadores mediante mapas de calor  
- Análisis funcional utilizando GSEApy (Enrichr) con bases de datos GO, KEGG, Reactome y MSigDB
  
#### `Integracion.ipynb`
Libreta destinada a la **integración conjunta de las dos muestras de ratón** (Mouse_1 y Mouse_3). El flujo de trabajo seguido en esta libreta es equivalente al descrito en `Analisis_funcional.ipynb`, incluyendo los pasos de normalización, regresión de covariables, reducción de dimensionalidad, construcción del grafo de vecinos, clusterización y análisis de expresión diferencial.

La principal diferencia es que, en este caso, ambas muestras se procesan de manera conjunta con el objetivo de:
- Integrar los datos en un único espacio transcriptómico compartido  
- Reducir posibles efectos de batch entre muestras  
- Facilitar la comparación directa de poblaciones celulares entre ratones  
- Identificar clústeres y patrones de expresión conservados entre ambas muestras *in vivo*  

---

### 🔹 Análisis de diversidad intraclonal

#### `Diversidad_Intra_Clonal.ipynb`
Libreta orientada a evaluar la heterogeneidad transcriptómica dentro de cada clon. Incluye:
- Clusterización local de células dentro de cada clon y condición  
- Cálculo de métricas de diversidad (entropía de Shannon, índice de Simpson y equidad de Pielou)  
- Procedimientos de bootstrap y rarefacción para evaluar la robustez de las métricas  
- Cálculo de la dispersión celular en el espacio UMAP  
- Visualización de resultados mediante gráficos de violín y dispersión  

---

### 🔹 Análisis de actividad de rutas (GSVA)

#### `GSVA.rmd`
Flujo de trabajo en R para el análisis de variación de la actividad génica mediante GSVA. Este script:
- Carga los datos de expresión preprocesados  
- Emplea conjuntos de genes de MSigDB  
- Calcula puntuaciones de actividad de rutas metabólicas y biológicas por muestra  
- Genera visualizaciones para explorar patrones funcionales globales  

---

### 🔹 Identificación y seguimiento de sub-clústeres

#### `busqueda_clusters.ipynb`
Libreta diseñada para:
- Identificar sub-clústeres transcriptómicos en células condicionadas *in vitro*  
- Definir firmas génicas representativas de cada subpoblación  
- Proyectar dichas firmas en muestras *in vivo*  
- Asignar a cada célula el sub-clúster más probable  
- Visualizar la contribución relativa de cada subpoblación celular en las muestras metastásicas  

---

## 📄 Notas
Las libretas están pensadas para ejecutarse siguiendo el flujo experimental del trabajo de fin de máster. Es posible que sea necesario adaptar rutas de archivos o nombres de muestras, todo esto aparece indicado en los diferentes scripts.
