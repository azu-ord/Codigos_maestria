# Examen Parcial — Blanca Azucena Orduña López

A continuación se muestran los pasos para ejecutar los ejercicios correspondientes al examen parcial.

## Preparación del entorno

Ubicarse en el subdirectorio donde se creará la carpeta "examen" y conectarse a datasciencetoolbox/dsatcl2e, ejemplo:

```bash

# Directorio
 cd /mnt/c/Users/azuce/Documents/Maestria_ciencia_datos/estadistica_computacional/data/

# Conectarse a docker

sudo docker start eb6cf9d4986d
sudo docker attach eb6cf9d4986d

#Ubicarse en el directorio 

cd /data

# Crear carpeta "examen" y ubicarse en el directorio

mkdir -p examen

cd examen

```

## Pregunta 1: Descarga y Preparación de Archivos

```bash
# Descarga de archivos

  # Instalar wget
  sudo apt update
  sudo apt install -y wget

  wget http://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.data
  wget http://archive.ics.uci.edu/ml/machine-learning-databases/adult/adult.names

```

## Pregunta 2: Creación de Encabezado

- Generar CSV de encabezados
- Incluir columna income
- Generar archivo header.txt con todas las columnas

```bash


awk -F: '/^[A-Za-z][A-Za-z_-]*:/{print $1}' adult.names | paste -sd, - > encabezados.csv

sed -i 's/$/,income/' encabezados.csv

cp encabezados.csv header.txt


```
## Pregunta 3: Limpieza de Datos

- Sustituir todas las ocurrencias del carácter ? en el archivo adult.data por la cadena "Unknown".
- Normaliza los delimitadores para que no queden espacios después de las comas (el archivo original tiene ", ").
- Elimina las líneas vacías del archivo resultante.
- Generar archivo adult_cleaned.data

```bash

sed -i 's/?/Unknown/g' adult.data

sed -i 's/, /,/g' adult.data

sed -i '/^$/d' adult.data 

cp adult.data adult_cleaned.data


```
## Pregunta 4: Creación de Catálogos

- Crea un catálogo para cada una de las siguientes columnas en el archivo adult_cleaned.data (índices 1-basados)

2: workclass
4: education
6: marital-status
7: occupation
8: relationship
9: race
10: sex
14: native-country
15: income

```bash
cut -d',' -f2  adult_cleaned.data | sort | uniq | awk -F'\n' 'BEGIN{OFS=","} NF{print NR,$0}' > workclass.txt
cut -d',' -f4  adult_cleaned.data | sort | uniq | awk -F'\n' 'BEGIN{OFS=","} NF{print NR,$0}' > education.txt
cut -d',' -f6  adult_cleaned.data | sort | uniq | awk -F'\n' 'BEGIN{OFS=","} NF{print NR,$0}' > marital-status.txt
cut -d',' -f7  adult_cleaned.data | sort | uniq | awk -F'\n' 'BEGIN{OFS=","} NF{print NR,$0}' > occupation.txt
cut -d',' -f8  adult_cleaned.data | sort | uniq | awk -F'\n' 'BEGIN{OFS=","} NF{print NR,$0}' > relationship.txt
cut -d',' -f9  adult_cleaned.data | sort | uniq | awk -F'\n' 'BEGIN{OFS=","} NF{print NR,$0}' > race.txt
cut -d',' -f10 adult_cleaned.data | sort | uniq | awk -F'\n' 'BEGIN{OFS=","} NF{print NR,$0}' > sex.txt
cut -d',' -f14 adult_cleaned.data | sort | uniq | awk -F'\n' 'BEGIN{OFS=","} NF{print NR,$0}' > native-country.txt
cut -d',' -f15 adult_cleaned.data | sort | uniq | awk -F'\n' 'BEGIN{OFS=","} NF{print NR,$0}' > income.txt

# Normalizar catálogos y datos

sudo apt update
sudo apt install --only-upgrade dos2unix

dos2unix adult_cleaned.data *.txt 2>/dev/null || true

# quitar espacios alrededor de comas 
sed -i 's/ *, */,/g' adult_cleaned.data
for f in workclass.txt education.txt marital-status.txt occupation.txt relationship.txt race.txt sex.txt native-country.txt income.txt; do
  sed -i 's/ *, */,/g' "$f"
done


```
## Pregunta 5: Sustitución de Valores por Índices Numéricos

-Utilizando sort, join y cut, reemplaza cada una de las categorías textuales de las columnas 2, 4, 6, 7, 8, 9, 10, 14 y 15 
en el archivo adult_cleaned.data por el índice numérico de cada categoría en su respectivo catálogo

```bash

# Tratamiento de catálogos previo a join

cut -d',' -f2 workclass.txt       > tmp_v.txt; cut -d',' -f1 workclass.txt       > tmp_i.txt; paste -d',' tmp_v.txt tmp_i.txt | sort -t',' -k1,1 > mapa_workclass.txt
cut -d',' -f2 education.txt       > tmp_v.txt; cut -d',' -f1 education.txt       > tmp_i.txt; paste -d',' tmp_v.txt tmp_i.txt | sort -t',' -k1,1 > mapa_education.txt
cut -d',' -f2 marital-status.txt  > tmp_v.txt; cut -d',' -f1 marital-status.txt  > tmp_i.txt; paste -d',' tmp_v.txt tmp_i.txt | sort -t',' -k1,1 > mapa_marital-status.txt
cut -d',' -f2 occupation.txt      > tmp_v.txt; cut -d',' -f1 occupation.txt      > tmp_i.txt; paste -d',' tmp_v.txt tmp_i.txt | sort -t',' -k1,1 > mapa_occupation.txt
cut -d',' -f2 relationship.txt    > tmp_v.txt; cut -d',' -f1 relationship.txt    > tmp_i.txt; paste -d',' tmp_v.txt tmp_i.txt | sort -t',' -k1,1 > mapa_relationship.txt
cut -d',' -f2 race.txt            > tmp_v.txt; cut -d',' -f1 race.txt            > tmp_i.txt; paste -d',' tmp_v.txt tmp_i.txt | sort -t',' -k1,1 > mapa_race.txt
cut -d',' -f2 sex.txt             > tmp_v.txt; cut -d',' -f1 sex.txt             > tmp_i.txt; paste -d',' tmp_v.txt tmp_i.txt | sort -t',' -k1,1 > mapa_sex.txt
cut -d',' -f2 native-country.txt  > tmp_v.txt; cut -d',' -f1 native-country.txt  > tmp_i.txt; paste -d',' tmp_v.txt tmp_i.txt | sort -t',' -k1,1 > mapa_native-country.txt
cut -d',' -f2 income.txt          > tmp_v.txt; cut -d',' -f1 income.txt          > tmp_i.txt; paste -d',' tmp_v.txt tmp_i.txt | sort -t',' -k1,1 > mapa_income.txt
rm -f tmp_v.txt tmp_i.txt


# JOIN

nl -w1 -s',' adult_cleaned.data | cut -d',' -f1,3 | sort -t',' -k2,2 > col2_linea_valor_ordenado.txt
join -t',' -1 2 -2 1 -a1 -o 1.1,2.2 col2_linea_valor_ordenado.txt mapa_workclass.txt | sort -t',' -k1,1n > col2_linea_indice.txt
cut -d',' -f1 adult_cleaned.data    > izq2.txt
cut -d',' -f3-15 adult_cleaned.data > der2.txt
cut -d',' -f2 col2_linea_indice.txt > idx2.txt
paste -d',' izq2.txt idx2.txt der2.txt > paso2.data
rm -f col2_linea_valor_ordenado.txt col2_linea_indice.txt izq2.txt der2.txt idx2.txt

nl -w1 -s',' paso2.data | cut -d',' -f1,5 | sort -t',' -k2,2 > col4_linea_valor_ordenado.txt
join -t',' -1 2 -2 1 -a1 -o 1.1,2.2 col4_linea_valor_ordenado.txt mapa_education.txt | sort -t',' -k1,1n > col4_linea_indice.txt
cut -d',' -f1-3  paso2.data > izq4.txt
cut -d',' -f5-15 paso2.data > der4.txt
cut -d',' -f2 col4_linea_indice.txt > idx4.txt
paste -d',' izq4.txt idx4.txt der4.txt > paso4.data
rm -f col4_linea_valor_ordenado.txt col4_linea_indice.txt izq4.txt der4.txt idx4.txt paso2.data

nl -w1 -s',' paso4.data | cut -d',' -f1,7 | sort -t',' -k2,2 > col6_linea_valor_ordenado.txt
join -t',' -1 2 -2 1 -a1 -o 1.1,2.2 col6_linea_valor_ordenado.txt mapa_marital-status.txt | sort -t',' -k1,1n > col6_linea_indice.txt
cut -d',' -f1-5  paso4.data > izq6.txt
cut -d',' -f7-15 paso4.data > der6.txt
cut -d',' -f2 col6_linea_indice.txt > idx6.txt
paste -d',' izq6.txt idx6.txt der6.txt > paso6.data
rm -f col6_linea_valor_ordenado.txt col6_linea_indice.txt izq6.txt der6.txt idx6.txt paso4.data

nl -w1 -s',' paso6.data | cut -d',' -f1,8 | sort -t',' -k2,2 > col7_linea_valor_ordenado.txt
join -t',' -1 2 -2 1 -a1 -o 1.1,2.2 col7_linea_valor_ordenado.txt mapa_occupation.txt | sort -t',' -k1,1n > col7_linea_indice.txt
cut -d',' -f1-6  paso6.data > izq7.txt
cut -d',' -f8-15 paso6.data > der7.txt
cut -d',' -f2 col7_linea_indice.txt > idx7.txt
paste -d',' izq7.txt idx7.txt der7.txt > paso7.data
rm -f col7_linea_valor_ordenado.txt col7_linea_indice.txt izq7.txt der7.txt idx7.txt paso6.data

nl -w1 -s',' paso7.data | cut -d',' -f1,9 | sort -t',' -k2,2 > col8_linea_valor_ordenado.txt
join -t',' -1 2 -2 1 -a1 -o 1.1,2.2 col8_linea_valor_ordenado.txt mapa_relationship.txt | sort -t',' -k1,1n > col8_linea_indice.txt
cut -d',' -f1-7  paso7.data > izq8.txt
cut -d',' -f9-15 paso7.data > der8.txt
cut -d',' -f2 col8_linea_indice.txt > idx8.txt
paste -d',' izq8.txt idx8.txt der8.txt > paso8.data
rm -f col8_linea_valor_ordenado.txt col8_linea_indice.txt izq8.txt der8.txt idx8.txt paso7.data

nl -w1 -s',' paso8.data | cut -d',' -f1,10 | sort -t',' -k2,2 > col9_linea_valor_ordenado.txt
join -t',' -1 2 -2 1 -a1 -o 1.1,2.2 col9_linea_valor_ordenado.txt mapa_race.txt | sort -t',' -k1,1n > col9_linea_indice.txt
cut -d',' -f1-8   paso8.data > izq9.txt
cut -d',' -f10-15 paso8.data > der9.txt
cut -d',' -f2 col9_linea_indice.txt > idx9.txt
paste -d',' izq9.txt idx9.txt der9.txt > paso9.data
rm -f col9_linea_valor_ordenado.txt col9_linea_indice.txt izq9.txt der9.txt idx9.txt paso8.data

nl -w1 -s',' paso9.data | cut -d',' -f1,11 | sort -t',' -k2,2 > col10_linea_valor_ordenado.txt
join -t',' -1 2 -2 1 -a1 -o 1.1,2.2 col10_linea_valor_ordenado.txt mapa_sex.txt | sort -t',' -k1,1n > col10_linea_indice.txt
cut -d',' -f1-9   paso9.data > izq10.txt
cut -d',' -f11-15 paso9.data > der10.txt
cut -d',' -f2 col10_linea_indice.txt > idx10.txt
paste -d',' izq10.txt idx10.txt der10.txt > paso10.data
rm -f col10_linea_valor_ordenado.txt col10_linea_indice.txt izq10.txt der10.txt idx10.txt paso9.data

nl -w1 -s',' paso10.data | cut -d',' -f1,15 | sort -t',' -k2,2 > col14_linea_valor_ordenado.txt
join -t',' -1 2 -2 1 -a1 -o 1.1,2.2 col14_linea_valor_ordenado.txt mapa_native-country.txt | sort -t',' -k1,1n > col14_linea_indice.txt
cut -d',' -f1-13 paso10.data > izq14.txt
cut -d',' -f15   paso10.data > der14.txt
cut -d',' -f2 col14_linea_indice.txt > idx14.txt
paste -d',' izq14.txt idx14.txt der14.txt > paso14.data
rm -f col14_linea_valor_ordenado.txt col14_linea_indice.txt izq14.txt der14.txt idx14.txt paso10.data

nl -w1 -s',' paso14.data | cut -d',' -f1,16 | sort -t',' -k2,2 > col15_linea_valor_ordenado.txt
join -t',' -1 2 -2 1 -a1 -o 1.1,2.2 col15_linea_valor_ordenado.txt mapa_income.txt | sort -t',' -k1,1n > col15_linea_indice.txt
cut -d',' -f1-14 paso14.data > izq15.txt
cut -d',' -f2 col15_linea_indice.txt > idx15.txt
paste -d',' izq15.txt idx15.txt > adult_numeric.data
rm -f col15_linea_valor_ordenado.txt col15_linea_indice.txt izq15.txt idx15.txt paso14.data

```
- Realizar el mismo procedimiento anterior, pero ahora utilizando únicamente awk


```bash

cat > map_adult_simple.awk <<'EOF'
BEGIN {
    FS=","; OFS=","
}
function load_catalog(file, arr, line, parts) {
    while ((getline line < file) > 0) {
        split(line, parts, ",")
        if (length(parts[1]) && length(parts[2])) {
            arr[parts[2]] = parts[1]
        }
    }
    close(file)
}
BEGIN {
    load_catalog("workclass.txt",      wc)
    load_catalog("education.txt",      edu)
    load_catalog("marital-status.txt", ms)
    load_catalog("occupation.txt",     occ)
    load_catalog("relationship.txt",   rel)
    load_catalog("race.txt",           rc)
    load_catalog("sex.txt",            sx)
    load_catalog("native-country.txt", nc)
    load_catalog("income.txt",         inc)
}
{
    if (NF != 15) next
    if ($2  in wc)  $2  = wc[$2]
    if ($4  in edu) $4  = edu[$4]
    if ($6  in ms)  $6  = ms[$6]
    if ($7  in occ) $7  = occ[$7]
    if ($8  in rel) $8  = rel[$8]
    if ($9  in rc)  $9  = rc[$9]
    if ($10 in sx)  $10 = sx[$10]
    if ($14 in nc)  $14 = nc[$14]
    if ($15 in inc) $15 = inc[$15]
    print $0
}
EOF

awk -f map_adult_simple.awk adult_cleaned.data > adult_numeric_awk.data


```

## Pregunta 6: Creación y Manejo de Base de Datos con SQLite

- Crea una base de datos SQLite llamada adult.db.
- Genera una versión con encabezado del archivo numérico para facilitar la importación
- Crea una tabla adult_numeric para almacenar el contenido de adult_numeric_with_header.csv

```bash

cat header.txt > adult_numeric_with_header.csv
cat adult_numeric.data >> adult_numeric_with_header.csv

# Instalar sqlite3 y crear base de datos

sudo apt install sqlite3

sqlite3 adult.db


# Crear tabla

DROP TABLE IF EXISTS adult_numeric;
CREATE TABLE adult_numeric (
  age              INTEGER,
  workclass        INTEGER,
  fnlwgt           INTEGER,
  education        INTEGER,
  education_num    INTEGER,
  marital_status   INTEGER,
  occupation       INTEGER,
  relationship     INTEGER,
  race             INTEGER,
  sex              INTEGER,
  capital_gain     INTEGER,
  capital_loss     INTEGER,
  hours_per_week   INTEGER,
  native_country   INTEGER,
  income           INTEGER
);

.mode csv
.import --skip 1 adult_numeric_with_header.csv adult_numeric

# Tablas para catálogos

sqlite3 adult.db <<'SQL'
.mode csv

DROP TABLE IF EXISTS workclass_catalog;
CREATE TABLE workclass_catalog (id INTEGER PRIMARY KEY, value TEXT);
.import workclass.txt workclass_catalog

DROP TABLE IF EXISTS education_catalog;
CREATE TABLE education_catalog (id INTEGER PRIMARY KEY, value TEXT);
.import education.txt education_catalog

DROP TABLE IF EXISTS marital_status_catalog;
CREATE TABLE marital_status_catalog (id INTEGER PRIMARY KEY, value TEXT);
.import "marital-status.txt" marital_status_catalog

DROP TABLE IF EXISTS occupation_catalog;
CREATE TABLE occupation_catalog (id INTEGER PRIMARY KEY, value TEXT);
.import occupation.txt occupation_catalog

DROP TABLE IF EXISTS relationship_catalog;
CREATE TABLE relationship_catalog (id INTEGER PRIMARY KEY, value TEXT);
.import relationship.txt relationship_catalog

DROP TABLE IF EXISTS race_catalog;
CREATE TABLE race_catalog (id INTEGER PRIMARY KEY, value TEXT);
.import race.txt race_catalog

DROP TABLE IF EXISTS sex_catalog;
CREATE TABLE sex_catalog (id INTEGER PRIMARY KEY, value TEXT);
.import sex.txt sex_catalog

DROP TABLE IF EXISTS native_country_catalog;
CREATE TABLE native_country_catalog (id INTEGER PRIMARY KEY, value TEXT);
.import "native-country.txt" native_country_catalog

DROP TABLE IF EXISTS income_catalog;
CREATE TABLE income_catalog (id INTEGER PRIMARY KEY, value TEXT);
.import income.txt income_catalog

.quit
SQL


```

## Pregunta 7: Consultas SQL

- En la base adult.db, escribe una consulta SQL que reemplace los valores numéricos de las columnas 2, 4, 6, 7, 8, 9, 10, 14 y 15 por sus correspondientes valores textuales utilizando JOIN con las tablas de catálogos creadas en la Pregunta 6.
- El resultado debe ser un conjunto de filas idéntico a adult_cleaned.data (con ? ya sustituido por Unknown, delimitado por comas y sin espacios tras las comas).
- Exporta las filas a un archivo llamado adult_restored.data (puedes usar .mode csv y .once adult_restored.data).


```bash

sqlite3 adult.db <<'SQL'
.headers off
.mode csv
.once adult_restored.data

SELECT
  n.age,
  wc.value,
  n.fnlwgt,
  edu.value,
  n.education_num,
  ms.value,
  occ.value,
  rel.value,
  rc.value,
  sx.value,
  n.capital_gain,
  n.capital_loss,
  n.hours_per_week,
  nc.value,
  inc.value
FROM adult_numeric AS n
LEFT JOIN workclass_catalog      AS wc  ON wc.id  = n.workclass
LEFT JOIN education_catalog      AS edu ON edu.id = n.education
LEFT JOIN marital_status_catalog AS ms  ON ms.id  = n.marital_status
LEFT JOIN occupation_catalog     AS occ ON occ.id = n.occupation
LEFT JOIN relationship_catalog   AS rel ON rel.id = n.relationship
LEFT JOIN race_catalog           AS rc  ON rc.id  = n.race
LEFT JOIN sex_catalog            AS sx  ON sx.id  = n.sex
LEFT JOIN native_country_catalog AS nc  ON nc.id  = n.native_country
LEFT JOIN income_catalog         AS inc ON inc.id = n.income
WHERE n.age != 'age';

.quit
SQL


```
- Escribe y ejecuta una consulta SQL para encontrar el número de individuos con age > 40 que ganan >50K
- Exporta las filas en query_results.txt .

```bash


sqlite3 adult.db <<'SQL'
.mode list
.once query_results.txt

SELECT COUNT(*)
FROM adult_numeric AS n
JOIN income_catalog AS inc ON inc.id = n.income
WHERE n.age > 40 AND inc.value = '>50K';

.quit
SQL


```
Escribe una consulta para encontrar los países (native-country) que tienen individuos trabajando en todas las ocupaciones disponibles (occupation).


```bash

sqlite3 adult.db <<'SQL'
.headers off
.mode csv
.once ocupaciones.txt

SELECT nc.value
FROM adult_numeric AS n
JOIN native_country_catalog AS nc ON nc.id = n.native_country
GROUP BY nc.value
HAVING COUNT(DISTINCT n.occupation) = (SELECT COUNT(*) FROM occupation_catalog);

.quit
SQL

```


## Pregunta 8: Análisis en R y Resumen Estadístico

- Aplicar un script en R al archivo procesado para generar:

Estadísticos descriptivos de variables numéricas (conteo, faltantes, media, desviación estándar, min, Q1, mediana, Q3, max).


```bash
cat header.txt > adult_cleaned_with_header.csv
cat adult_cleaned.data >> adult_cleaned_with_header.csv

$ Rscript analyze_adult.R adult_cleaned_with_header.csv resultados_r


```

