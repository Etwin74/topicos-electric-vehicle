# Electric Vehicle · Data Lake y analítica

**Procesamiento de datos de vehículos eléctricos con PySpark, Hive, MongoDB y Power BI.**

Proyecto académico que organiza un flujo de datos por capas: carga inicial, normalización, limpieza y agregación. El repositorio incluye scripts, datos de ejemplo, un informe y un archivo de Power BI.

## Flujo implementado

```text
CSV de vehículos
      ↓
Workload · carga inicial
      ↓
Landing · almacenamiento Avro
      ↓
Curated · limpieza y Parquet particionado por año
      ↓
Functional · resumen por estado, tipo y año
      ↓
Exportaciones CSV / MongoDB · análisis en Power BI
```

La capa de limpieza normaliza textos y tipos, filtra años y rangos eléctricos no válidos. La capa funcional calcula el número de vehículos y la autonomía media por grupo.

## Evidencias y entregables

- [Procesos PySpark](electriv-vehicle/datalake/procesos).
- [Esquema Avro](electriv-vehicle/datalake/schema/electric_vehicle.avsc).
- [Informe del proyecto](electriv-vehicle/documentation/informe.pdf).
- [Archivo de Power BI](electriv-vehicle/reports/reporte_proyecto_final.pbix).
- [Salida CSV incluida](datalake/gold.csv).
- [Notebook exploratorio](electriv-vehicle/datalakehouse/pyspark.ipynb).

## Entorno necesario

- Python y Java compatibles con el entorno Spark.
- Spark con Hive, Hadoop HDFS y YARN.
- Soporte Avro y conector Spark–MongoDB compatibles con la distribución instalada.
- MongoDB local para la exportación.
- Power BI Desktop para abrir el archivo `.pbix`.

El archivo `electriv-vehicle/requqerements.txt` es un marcador, no una lista completa de dependencias. La infraestructura distribuida no se instala automáticamente.

## Reproducir el flujo

Usa [instrucciones.txt](instrucciones.txt) como referencia del laboratorio original y revisa los argumentos de cada script antes de ejecutarlo. La secuencia es:

1. Preparar HDFS, Hive, YARN y las rutas del dataset.
2. Ejecutar `poblar_capa_workload.py`.
3. Publicar el esquema Avro y ejecutar `poblar_capa_landing.py`.
4. Ejecutar `poblar_capa_curated.py`.
5. Ejecutar `poblar_capa_functional.py`.
6. Revisar las rutas de `export_gold_csv.py` y `export_gold_to_mongodb.py`.
7. Abrir el archivo de Power BI y ajustar sus orígenes de datos.

**Precaución:** varios procesos recrean tablas o bases de Hive y escriben con modo `overwrite`. Ejecútalos solo sobre un entorno de laboratorio aislado.

### Ajustes conocidos del laboratorio

- Algunas rutas mezclan `TopicosB`, `TOPICOSB` y `topicosb`; HDFS distingue mayúsculas.
- El instructivo menciona `export_gold_to_csv.py`, pero el archivo disponible es `export_gold_csv.py`.
- Las rutas locales y HDFS, así como las versiones de conectores, deben ajustarse al equipo de ejecución.
- El repositorio no incluye una suite automatizada ni una receta de despliegue reproducible de principio a fin.

## Estructura

```text
electriv-vehicle/
  datalake/          Datos, esquema y procesos por capas
  datalakehouse/     Notebook exploratorio
  documentation/    Informe
  reports/          Archivo de Power BI
  scripts/          Utilidades MongoDB
datalake/            Exportaciones guardadas
instrucciones.txt    Recorrido del laboratorio original
```

**Alcance:** trabajo académico de ingeniería de datos y visualización; no un servicio de producción.
