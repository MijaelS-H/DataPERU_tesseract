# Entrenamiento Tesseract

## Descripción

El presente directorio tiene como objetivo desarrollar un ejemplo de generación de esquemas de datos que permita conectarse con una base de datos ya ingestada en Clickhouse.

Su objetivo es comprender de mejor forma la estructura de un esquema de Tesseract y comprender su estructura.

## Comandos a ejecutar

### Concatenación de esquemas
`./moncat -d training -o schema-training.xml`

### Establecer variables de entorno
`source .env`

### Ejecución de Tesseract
`tesseract-olap`

### Ejecución de Tesseract-UI (cd ../dataperu-ui)
`npm run dev`
