# Examen Final

### Blanca Azucena Orduña López

**Contenido de la carpeta**

- **Descarga y limpieza de los datos**
    - descarga_limpieza.sh
    - auto-mpg.data (se incluye en caso de problemas con el repositorio UCI)
    - auto-mpg.names
    - data_clean.csv
    - headers.csv
    - auto_mpg_final.csv (resultado final de limpieza con encabezados)
  
- **EDA.R**
     - EDA_codigo.rmd
     - EDA_visibilidad.html (para visibilidad de gráficas)
  
- **API**
    - train_model.py
    - model_mpg.pkl
    - model_columns.pkl
    - main.py
    - requirements
    - Dockerfile

- **Shiny App**
    - shiny_app.r
    - Dockerfile.shiny

- **Generales**

    - auto_mpg_clean.feather
  
  
## Limpieza de datos y preparación de la base

1. Descarga y limpieza de datos


Desde ubuntu, ubicarse en la carpeta de trabajo deseada y crear si no existe la carpeta para el ejercicio, ejemplo:

```bash

cd /mnt/c/Users/azuce/Documents/Maestria_ciencia_datos/

mkdir -p 226094

cd 226094

```

2. Ejecutar descarga_limpieza.sh

```bash

./limpieza.sh

```

Si hay algún problema con la ejecución, dar permisos y ejecutar nuevamente.

```bash

chmod +x descarga_limpieza.sh

./limpieza.sh

```

## EDA MPG

Es posible visualizar el código en el archivo EDA_codigo.rmd o tener la vista directa en EDA_visibilidad.html

## API MPG

Es posible acceder a la API en el link  http://18.217.165.20/docs


**Instrucciones para uso de API**

Para realizar una predicción:

En /Predict seleccionar "Try it out" la API solicitará que se ingresen los valores para realizar la predicción, deberán ingresarse con en el ejemplo.

```bash
{
  "cylinders": 8,
  "displacement": 310,
  "horsepower": 130,
  "weight": 3550,
  "acceleration": 12,
  "model_year": 70,
  "origin": "USA"
}
```

Una vez que se tienen los datos seleccionar "Execute", en la sección details se mostrará la predicción de mpg para este registro.

```bash

Response body
Download
{
  "mpg_prediction": 16.052277705627713,
  "message": "Predicción guardada en base-mpg"
}

```

Para calcular los estadísticos:

En la sección /stats seleccionar "Try it out" y "Execute" en la sección details se mostrarán los cálculos.

```bash
{
  "total_requests": 3,
  "average_mpg": 17.609397252400715,
  "max_mpg": 20.6666,
  "min_mpg": 16.0523
}
```

- El modelo RandomForestRegressor se encuentra en train_model.py 

## Shiny app

Es posible acceder a la shiny app del análisis estadístico en http://18.217.165.20:3838


