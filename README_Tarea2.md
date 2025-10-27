# Tarea 2:  Limpieza y preparación de datos con awk

## Blanca Azucena Orduña López


1. Descargar y descomprimir la carpeta **tarea2-awk-adult**.


2. Desde ubuntu, ubicarse en la carpeta descomprimida tarea2-awk-adult, ejemplo:

```bash
cd /mnt/c/Users/azuce/Documents/Maestria_ciencia_datos/estadistica_computacional/data/tarea2-awk-adult/

```

2. Conectarse a Docker datasciencetoolbox/dsatcl2e y ubicarnos en la carpeta.

En esta carpeta se encuentra el archivo ejecutable solucion.sh y aquí mismo se creará la carpeta out.

```bash
sudo docker start eb6cf9d4986d
sudo docker attach eb6cf9d4986d

cd /data

cd tarea2-awk-adult

```
Verificar ubicación

```bash

pwd

```
Debemos ver una salida similar a:

/data/tarea2-awk-adult


3. Verificar formato del script (dado que fue creado en Windows)


```bash

dos2unix solucion.sh

```

4. Ejecutar solución.sh

```bash

./solucion.sh

```

5. Si hay algún problema con la ejecución, dar permisos y ejecutar nuevamente.

```bash

chmod +x solucion.sh

./solucion.sh

```



