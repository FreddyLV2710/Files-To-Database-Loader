# Data Loader Script

Este proyecto es un script de Python que carga datos desde archivos CSV en una base de datos PostgreSQL, utilizando esquemas predefinidos para organizar las columnas de los datasets. El script permite procesar uno o más datasets y cargarlos en la base de datos en fragmentos, lo que lo hace ideal para manejar archivos grandes.

## Características

- **Lectura en fragmentos**: El script procesa archivos CSV en bloques de 10,000 registros para evitar problemas de memoria.
- **Esquemas de columnas**: Utiliza un archivo `schemas.json` para definir el nombre y el orden de las columnas de cada dataset.
- **Inserción en base de datos**: Inserta los datos en la base de datos utilizando `pandas` y el método `to_sql` para PostgreSQL.
- **Variables de entorno**: Configura las rutas de origen y las credenciales de la base de datos a través de un archivo `.env`.

## Requisitos previos

1. **Python 3.x**: Este script utiliza Python y necesita tener instalado Python 3.x.
2. **PostgreSQL**: Debe haber una base de datos PostgreSQL configurada y accesible desde el script.
3. **Paquetes de Python**: Se necesitan las siguientes bibliotecas para ejecutar el script:
    - `pandas`
    - `psycopg2`
    - `dotenv`

## Instalación

1. Clona este repositorio.
2. Crea un entorno virtual (opcional, pero recomendado):
   ```bash
   python -m venv env
   source env/bin/activate  # En Linux/Mac
   .\env\Scripts\activate  # En Windows
3. pip install -r requirements.txt
4. SRC_BASE_DIR=path_to_datasets_directory
    DB_HOST=localhost
    DB_PORT=5432
    DB_NAME=my_database
    DB_USER=my_user
    DB_PASS=my_password
5. Crea un archivo `schemas.json` en el directorio de origen que contenga los esquemas de los datasets. Un ejemplo de este archivo es:

   ```json
   {
     "dataset1": [
       {"column_name": "id", "column_position": 1},
       {"column_name": "name", "column_position": 2},
       {"column_name": "age", "column_position": 3}
     ],
     "dataset2": [
       {"column_name": "product_id", "column_position": 1},
       {"column_name": "price", "column_position": 2}
     ]
   }

## Uso

### Ejecución estándar

Para ejecutar el script y procesar todos los datasets definidos en `schemas.json`, simplemente ejecuta el siguiente comando:

```bash
python script.py
```

### Procesar datasets específicos

Si deseas procesar datasets específicos, puedes proporcionar una lista de nombres de datasets en formato JSON como argumento:

```bash
python script.py '[\"dataset1\", \"dataset2\"]'
```

### Funciones principales

- **`get_column_names(schemas, ds_name, sorting_key)`**: Obtiene y ordena los nombres de las columnas de un dataset dado.
- **`read_csv(file, schemas)`**: Lee un archivo CSV en fragmentos de 10,000 registros.
- **`to_sql(df, db_conn_uri, ds_name)`**: Inserta un DataFrame en la base de datos PostgreSQL.
- **`db_loader(src_base_dir, db_conn_uri, ds_name)`**: Carga los archivos CSV correspondientes a un dataset en la base de datos.
- **`process_files(ds_names)`**: Procesa y carga uno o más datasets en la base de datos, leyendo los esquemas desde el archivo `schemas.json`.

### Ejemplo

Si tienes un dataset llamado `dataset1` en el directorio de datasets con archivos llamados `part-*`, el script leerá esos archivos, procesará los datos en fragmentos y los cargará en una tabla en la base de datos PostgreSQL.

```bash
python script.py '[\"dataset1\"]'
```

## Manejo de Errores

El script maneja las siguientes excepciones:

- `NameError`: Si no se encuentran archivos para el dataset especificado.
- `Exception`: Para cualquier otro error que pueda ocurrir durante la carga de los datos, el script continuará procesando los siguientes datasets.

## Contribuciones

Si deseas contribuir a este proyecto, crea un fork del repositorio y envía un pull request con tus cambios.