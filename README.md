# 🐟 Proyecto **OrestiasGen**: Análisis genómico de adaptación en *Orestias* del Altiplano

## 🧠 Hipótesis  

La hipótesis principal es que las especies de *Orestias* del Altiplano presentan **adaptaciones genéticas** a las condiciones ambientales extremas (alta radiación UV, altitud, salinidad) que se reflejan en señales de **selección positiva** en sus genomas 🧬✨.  

En particular, se anticipa encontrar:

- Selección positiva en genes relacionados con **estabilidad genómica** y **reparación del ADN**, como se observó en *Orestias ascotanensis* (con cientos de genes bajo selección positiva implicados en reparación de ADN).  
- Diferencias de hábitat (p. ej. aguas salobres vs. dulce, distintas altitudes) que generan patrones de **estructura poblacional** detectables entre especies, evidentes en análisis de componentes principales (PCA) y en gráficos de agrupamiento genético (STRUCTURE/ADMIXTURE).  

👉 En una primera fase **exploratoria**, estas ideas se probarán usando **datos públicos** (GenBank/SRA), mientras se obtienen y secuencian los datos propios de la tesis.

---

## 🎯 Objetivos  

### 🎯 Objetivo general  

Explorar y caracterizar la **adaptación genómica** y la **estructura genética** en especies altoandinas de *Orestias*, utilizando datos genómicos públicos como aproximación preliminar alineada con la tesis de maestría.

### 📌 Objetivos específicos  

- 🔍 **Selección de datos:**  
  Recolectar y procesar datos de secuenciación genómica de varias especies de *Orestias* a partir de repositorios públicos (GenBank, SRA, etc.).  
  Dado que en la cuenca del lago Titicaca y otros sistemas altoandinos se reconocen numerosas especies del género, se seleccionarán al menos **4 especies representativas** con múltiples individuos por especie.

- 🧬 **Análisis poblacional:**  
  Describir la **estructura genética** de las poblaciones mediante PCA y clustering.  
  - Calcular estadísticos como \(F_{ST}\).  
  - Realizar **PCA** para visualizar variación genética.  
  - Ejecutar **STRUCTURE/ADMIXTURE** para inferir agrupamientos genéticos y proporciones de ancestría.  

- 🔥 **Detección de selección positiva:**  
  Identificar genes o regiones del genoma bajo **selección positiva**.  
  - Aplicar pruebas filogenéticas basadas en modelos codón (PAML, HyPhy).  
  - Detectar loci con razones dN/dS elevadas y señales de adaptación.

- 🧩 **Interpretación funcional:**  
  Relacionar los genes candidatos con procesos biológicos de adaptación (respuesta a UV, hipoxia, salinidad, metabolismo, etc.).

- 📚 **Reproducibilidad y documentación:**  
  Implementar todo el flujo de trabajo en **Jupyter Notebooks** (Python 3), con código claro y reproducible.  
  Documentar scripts y, de ser necesario, clonar repositorios útiles (*fastSTRUCTURE*, *CLUMPAK*, etc.), manteniendo coherencia con los objetivos de la tesis.

---

## 🐟 Especies y muestras  

- **Especies objetivo:**  
  Se considerarán al menos **cuatro especies representativas** de *Orestias* del Altiplano (idealmente miembros de la radiación del Titicaca y especies altoandinas relacionadas).  
  Ejemplos de criterios de selección:
  - Especies de **ambientes salobres** vs. **agua dulce**.  
  - Especies de diferentes **cuencas altoandinas** (Titicaca, Junín, lagunas/lagos endorreicos).  
  - Especies con buena representación de secuencias en GenBank/SRA.

- **Muestras por especie:**  
  Se incluirán **todos los individuos disponibles** por especie en los datos públicos (objetivo: ≥10 individuos por especie, si es posible), para asegurar potencia estadística en los análisis poblacionales.  

⚠️ **Nota importante (fase exploratoria):**  
En esta etapa, el número real de individuos y especies estará limitado por los datos disponibles públicamente. Más adelante, los análisis se integrarán con los **datos genómicos propios de la tesis**, una vez completadas las campañas de muestreo y secuenciación.

---

## 🔬 Métodos  

El flujo de trabajo combinará análisis de genómica de poblaciones y comparativa, utilizando datos públicos y herramientas estándar en genética evolutiva.

### 🧼 1. Preprocesamiento de datos  

- Descarga de datos (FASTQ/BAM) desde **GenBank/SRA**.  
- Control de calidad:  
  - Eliminación de adaptadores y recorte de bases de baja calidad (p. ej. con Trimmomatic/Cutadapt).  
  - Evaluación con herramientas como **FastQC**.  

### 🧲 2. Mapeo al genoma de referencia  

- Alineamiento de las lecturas contra un **genoma de referencia** de *Orestias* (o especie cercana) usando:  
  - **BWA-MEM** o **Bowtie2**.  
- Generación de archivos BAM ordenados e indexados.  

### 🧬 3. Llamado y filtrado de variantes  

- Llamado de variantes (SNPs/indels) con:  
  - **GATK** (HaplotypeCaller) o **bcftools**.  
- Filtrado de SNPs de alta calidad usando:  
  - **VCFtools** / **PLINK** (filtros por profundidad, calidad, missing data, MAF, etc.).  

### 👥 4. Análisis de estructura poblacional  

- Cálculo de:  
  - **PCA** sobre la matriz de SNPs para visualizar agrupación de individuos.  
  - Estadísticos de diferenciación como **\(F_{ST}\)** entre especies/poblaciones.  
- Análisis de clustering:  
  - Ejecución de **STRUCTURE** / **fastSTRUCTURE** / **ADMIXTURE** para inferir grupos genéticos y proporciones de ancestría.  
- Visualización:  
  - Gráficos de barras de ancestría.  
  - Biplots de PCA con individuos coloreados por especie/población.

### 🔥 5. Detección de selección positiva  

- **En genes codificantes (nivel filogenético):**  
  - Alineamiento de secuencias de genes ortólogos entre especies de *Orestias*.  
  - Estimación de **dN/dS** (ω) usando **PAML** o **HyPhy**.  
  - Aplicación de modelos de selección positiva (p. ej. branch-site) para identificar genes con evidencia de selección adaptativa.

- **A nivel genómico poblacional:**  
  - Cálculo de **F_ST por locus** o por ventanas para detectar regiones con diferenciación extrema (outliers).  
  - Cálculo de **Tajima’s D** para evidenciar barridos selectivos recientes (valores muy negativos).  
  - Si la densidad de SNP lo permite, análisis de haplotipos extendidos (**iHS**, **XP-EHH**) para detectar selección reciente dentro y entre poblaciones.

### 🧮 6. Herramientas y scripts  

- **Lenguaje principal:** Python 3 🐍  
- **Entorno de trabajo:** Jupyter Notebooks 📓  
- Bibliotecas de Python:  
  - `pandas`, `numpy`, `matplotlib`, `seaborn`  
  - `scikit-allel` (genómica de poblaciones)  
  - `Biopython` (manejo de secuencias)  
- Software externo:  
  - **FastQC**, **Trimmomatic/Cutadapt**  
  - **BWA** / **Bowtie2**  
  - **GATK**, **bcftools**, **VCFtools**, **PLINK**  
  - **STRUCTURE / fastSTRUCTURE / ADMIXTURE**  
  - **PAML**, **HyPhy**  

Todos los scripts propios (Python/Bash) se mantendrán versionados en el repositorio, asegurando **reproducibilidad** y alineamiento con el proyecto de tesis.

---

## 📈 Resultados esperados  

Aunque esta fase es **exploratoria** y depende de la calidad y cantidad de datos públicos, se esperan los siguientes patrones generales:

- 🧩 **Estructura genética detectable:**  
  - Agrupación de individuos por **especie** y, en algunos casos, por **origen geográfico/ambiental** en el PCA.  
  - Valores de \(F_{ST}\) que indiquen **diferenciación moderada a alta** entre especies y/o poblaciones de distintos sistemas altoandinos.  

- 🌐 **Patrones de mezcla genética (admixture):**  
  - Gráficos de STRUCTURE/ADMIXTURE mostrando asignaciones claras a clusters genéticos en la mayoría de individuos.  
  - Posibles señales de **ancestría compartida** o introgresión entre algunas especies o poblaciones.

- 🔥 **Loci candidatos bajo selección positiva:**  
  - Identificación de **genes o regiones genómicas outlier** con F_ST elevado o Tajima’s D extremo.  
  - En genes codificantes, detección de **ω (dN/dS) > 1** en ciertos linajes, sugiriendo selección positiva histórica.  
  - En particular, se esperan candidatos relacionados con:  
    - **Reparación de ADN** (frente a radiación UV).  
    - **Metabolismo y uso de oxígeno** (adaptación a hipoxia de altura).  
    - **Respuesta a estrés osmótico** (salinidad).  

- 🧠 **Marco conceptual para la tesis:**  
  - Los resultados exploratorios servirán como **prueba de concepto** de los análisis que se aplicarán posteriormente a los datos propios de la tesis.  
  - Permitirá ajustar parámetros de filtrado, decidir qué métodos de selección son más informativos y planificar el tamaño muestral y cobertura genómica necesarios para la fase principal del proyecto.  

⚠️ Si los resultados son poco claros (por baja calidad de datos o número limitado de muestras), eso también será informativo, ya que ayudará a definir mejor los requerimientos de la futura fase con datos propios (profundidad de secuenciación, número de individuos, tipo de marcadores, etc.).

---

## 📚 Notas finales  

Este README resume la fase **exploratoria genómica** del proyecto de tesis de maestría, enfocada en la radiación de *Orestias* en sistemas altoandinos (incluyendo la radiación Titicaca).  

- A corto plazo, el objetivo es **probar y pulir el pipeline** con datos públicos.  
- A mediano plazo, el mismo pipeline se aplicará a los **datos genómicos propios** (SNPs de baja cobertura, RADseq u otro enfoque que se defina en la tesis), permitiendo una caracterización profunda de la **estructura genómica** del complejo de radiación Titicaca y la identificación robusta de **huellas de selección**.

🐟🧬✨ Este proyecto se sitúa en la intersección entre **genómica evolutiva**, **ecología** y **conservación**, y servirá como base metodológica directa para varias actividades
