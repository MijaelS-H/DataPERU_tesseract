# Plataforma de datos ITP: Tesseract

## Descripción

El presente repositorio tiene como objetivo recopilar la estructura de Tesseract, el cual ofrece una API desglosar, cortar, filtrar y examinar un cubo de datos.
Tesseract opera con base en esquemas desarrollados a partir de datos ingestados y referentes a múltiples bases de datos, que permiten la visualización y comprension de los datos de una manera dinámica y comprensible al usuario.

## Estructura de Repositorio

El repositorio actual se encuentra estructurado de la siguiente forma:

### frags/

Recopilación de esquemas de múltiples fuentes (ej: MINAGRI, MEF, ITP, entre otros). 
Su estructura responde a carpetas de archivos organizados según fuente de datos o temáticas similares (ej: Indicadores de Censos o Encuestas).
Cada archivo ```.xml``` corresponde al esquema de la fuente de datos correspondiente (ej: /frags/minagri/dinamica_agricola.xml hace referencia el esquema correspondiente al set de datos de dinámica agrícola otorgado por MINAGRI).
Adicionalmente, es posible encontrar el archivo `shared.xml`, el cual corresponde a la recopilación de variables compartidas dentro de un esquema, las cuales corresponden a variables de uso común en más de un fragmento de esquema (ej: Tiempo, Ubigeo, CIIU, entre otros).

### justfile

Archivo de comando requerido para la contanenación de esquemas, proceso necesario para la generación del esquema general compuesto de cada fragmento de esquema disponible en la carpeta frags/

### logic-layer-config.json

Archivo lógico que permite a Tesseract diferenciar dimensiones similares en un mismo cubo. Este archivo se hace necesario si una dimensión es utilizada reiteradas veces en un mismo esquema (ej: CIIU Primario, CIIU Secundario y CIIU Terciario hacen referencia a la misma dimensión compartida CIIU).

### requirements.txt

Archivo de requisitos necesarios para la ejecución de múltiples comandos intermedios.
Se recomienda, para su utilización, la creación de un entorno virtual para evitar el conflicto de versiones preexistentes de los requisitos especificados.
Un ejemplo de creación de entorno vitual es realizado mediante los siguientes comandos:

```
sudo apt-get install python3-pip
sudo pip3 install virtualenv

virtualenv -p python3.7 venv
source venv/bin/activate
```

Una vez generado el entorno virtual, es necesaria la instalacción de las dependencias requeridas:

```
pip install -r requirements.txt
```

### schema.xml

Corresponde al esquema general utilizado por Tesseract. Su estructura corresponde a la concatenación de los fragmentos de esquemas definidos en `frags/` y las dimensiones compartidas definidas en `shared.xml`.

## Actualización de esquemas

Para agregar un nuevo cubo al esquema, es necesario agregar un nuevo archivo `.xml` en la carpeta `frags/`. Es necesario asegurarse que el fragmento insertado, corresponda al mismo esquema definido en el archivo `schema.xml` (`<Schema name="dataperu" />`).

## Concatenación de esquemas

La generación del esquema general es un proceso manual que debe realizarse a medida que se realicen cambios en cada fragmento de esquema. Con el objetivo de evitar problemas de transferencias entre archivos, esta tarea es posible de automatizar mediante la utilización de `moncat`. El proceso necesario es detallado a continuación:

### 1. Instalar moncat

Para realizar este proceso es necesario instalar `moncat`, proceso detallado en el repositorio oficial de [moncat](https://github.com/hwchen/mondrian-schema-cat#installation)

### 2. Ejecutar comando de concatenación

Una vez instalado `moncat`, es necesario ejecutar el siguiente comando, el cual concatena cada fragmento definido en el directorio `frags/` en el esquema general `schema.xml`

```
./moncat -d frags/ -o schema.xml
```

### 3. Agregar contenido de `shared.xml`

Actualmente, `moncat` no permite la agregación automática de dimensiones compartidas bajo el objeto `<SharedDimension />`, como lo son las dimensiones definidas en el archivo `frags/shared.xml`. Dado lo anterior, es necesario de manera manual al inicio del archivo `schema.xml` generado por moncat.

### 4. Reiniciar servicio de Tesseract

Cada modificación del esquema utilizado por Tesseract, requiere que el servicio sea reiniciado para acceder a los nuevos cambios definidos. Favor contactar al administrador del servidor de backend para reiniciar el servicio correspondiente.
