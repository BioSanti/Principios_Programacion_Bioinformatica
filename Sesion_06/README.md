# Sesión 06 — Comandos Básicos II

> **Curso:** Principios de Programación en Bioinformática  
> **Programa:** Biología · UPC · Facultad de Ciencias de la Salud  
> **Docentes:** Frank Guzmán Escudero · Manuel Ramírez Sáenz  
> **Semana:** 6 de 16 · **Unidad I:** Introducción a GNU/Linux

---

## 🎯 Logro de la sesión

Al finalizar la sesión, el estudiante utiliza la sintaxis básica de los comandos para **gestionar y manipular archivos y directorios** en la terminal Linux, aplicando herramientas de procesamiento de texto con expresiones regulares.

---

## 📋 Agenda

1. Comando `grep` — Búsqueda con expresiones regulares
2. Comando `sort` — Ordenamiento de líneas
3. Comando `uniq` — Eliminación de duplicados
4. Comando `tr` — Traducción de caracteres
5. Comando `wc` — Conteo de palabras, líneas y caracteres
6. Comando `rev` — Reversión de texto
7. Comando `fold` — División de líneas
8. Comando `sed` — Editor de flujo
9. Comando `awk` — Procesador de texto avanzado

---

## 🔍 1. Comando `grep`

`grep` (**g**lobally search for a **r**egular **e**xpression and **p**rint matching lines) es una poderosa herramienta de búsqueda de patrones en archivos de texto.

### Sintaxis básica

```bash
grep "PATRÓN" archivo
```

> Los patrones se ponen entre comillas dobles.

### Opciones principales

| Opción | Función |
|--------|---------|
| `-v` | Invierte la coincidencia (líneas que NO contienen el patrón) |
| `--color` | Colorea el texto que coincide |
| `-F` | Interpreta el patrón como cadena literal |
| `-H`, `-h` | Imprime o no el nombre del archivo |
| `-i` | Ignora mayúsculas/minúsculas |
| `-l` | Lista solo los nombres de archivo que contienen el patrón |
| `-n` | Imprime el número de línea |
| `-c` | Cuenta el número de coincidencias |
| `-o` | Solo imprime el patrón que coincide |
| `-w` | Fuerza coincidencia con palabra entera |
| `-x` | Fuerza coincidencia con toda la línea |
| `-f` | Obtiene patrones desde un archivo |
| `-E` | Interpreta el patrón como expresión regular extendida (ERE) |

### Actividad 1 — Búsqueda básica con `grep`

```bash
# Crear archivo de referencias
nano references.txt
```

Contenido de `references.txt`:
```
seq1chr1exp-AC02
seq1CHR1exp-CC33
seq2chr1exp-HT33
seq3chr2exp-FG90
seq3chr2exp-FG90
seq3chr2exp-FG90
seq3chr2exp-FG90
seq5chr12exp-0011
seq5chr12exp-0011
seq5chr12exp-0011
seq7chr8exp-0TT1
seq7chr8exp-0TT1
seq7chr8exp-0TT1
```

```bash
# Buscar líneas que contengan "seq1"
grep "seq1" references.txt

# Buscar con número de línea
grep -n "seq1" references.txt

# Solo mostrar las coincidencias (no la línea completa)
grep -no "seq1" references.txt

# Mostrar contexto (líneas alrededor de la coincidencia)
grep -C 2 "seq1" references.txt

# Buscar "chr1" con contexto
grep -C 2 "chr1" references.txt

# Buscar el número "1" (coincidencias parciales)
grep "1" references.txt

# Contar coincidencias de líneas que empiecen con "1"
grep -c "^1" references.txt

# Mostrar líneas que NO contengan "seq1"
grep -v "seq1" references.txt
```

### Actividad 2 — Comparación de listas con `grep`

```bash
# Crear lista1.txt
nano lista1.txt
```

Contenido:
```
SBP1
SIX5
PRDM1
SOX13
BAF1
TCF4
PAX5
CTCF
SOX133
```

```bash
# Crear lista2.txt
nano lista2.txt
```

Contenido:
```
SOX13
BAF1
TCF4
PAX5
```

```bash
# Buscar patrones de lista2 en lista1
grep -f lista2.txt lista1.txt

# Coincidencia de palabras completas
grep -wf lista2.txt lista1.txt
```

### Actividad 3 — Expresiones regulares con `grep -E`

```bash
# Crear archivo palabras.txt
nano palabras.txt
```

Contenido:
```
Esta es una prueba
para practicar expresiones regulares en Linux
Algunos ejemplos
aaaaaaaabbc
caabc
abc
abbc
abbcc
1abc
abcde
2abc
```

```bash
# Líneas que empiezan con 'a'
grep -E '^a' palabras.txt

# Una o más 'b' entre 'a' y 'c'
grep -E 'ab+c' palabras.txt

# Cero o una 'b' entre 'a' y 'c'
grep -E 'ab?c' palabras.txt

# Cualquier carácter seguido de 'r'
grep -E '.r' palabras.txt

# Una o más repeticiones de 'bb' entre 'a' y 'c'
grep -E 'a(bb)+c' palabras.txt

# Entre 2 y 3 'b' entre 'a' y 'c'
grep -E 'ab{2,3}c' palabras.txt

# Entre 2 y 3 'a' seguidos de 'bc'
grep -E 'a{2,3}bc' palabras.txt

# Líneas con mayúsculas
grep -E '[[:upper:]]' palabras.txt
```

### Actividad 4 — Búsqueda de sitios de restricción enzimática

La enzima **HincII** presenta una secuencia de reconocimiento "degenerada":
- **Y** puede ser **C** o **T**
- **R** puede ser **G** o **A**

Secuencia: `GTYRAC` → Posibilidades: `GTCGAC`, `GTCAAC`, `GTTGAC`, `GTTAAC`

```bash
# Opción 1: Listar todas las combinaciones posibles
grep -E "GTCAAC|GTCGAC|GTTAAC|GTTGAC" *.fasta

# Opción 2: Usar clases de caracteres (más elegante)
grep -E "GT[CT][AG]AC" *.fasta
```

> 💡 **Tip bioinformático:** Las expresiones regulares son fundamentales para buscar motivos de secuencia, sitios de unión de factores de transcripción, y patrones degenerados en genómica.

---

## 📊 2. Comando `sort`

Permite ordenar líneas de archivos utilizando diferentes criterios.

### Sintaxis

```bash
sort <opciones> <fichero>
```

### Opciones principales

| Opción | Función |
|--------|---------|
| `-c` | Verificar si los datos están ordenados |
| `-r` | Orden inverso (descendente) |
| `-n` | Ordenar numéricamente |
| `-u` | Eliminar duplicados (únicos) |
| `-k` | Ordenar por campo específico |

### Actividad 5 — Ordenamiento básico

```bash
# Crear archivo genes.txt
nano genes.txt
```

Contenido:
```
pax4
sox13
sox13
Tcf4
azz2
```

```bash
# Verificar si está ordenado
sort -c genes.txt

# Eliminar duplicados y ordenar
sort -u genes.txt

# Guardar resultado en archivo
sort -u genes.txt -o genes_unicos.txt

# Ver contenido
cat genes_unicos.txt

# Orden inverso
sort -r genes.txt
```

### Actividad 6 — Ordenamiento numérico y por campo

```bash
# Generar números aleatorios y ordenar
shuf -i 1-10 -n 10 > numeros.txt
sort -n numeros.txt

# Ordenar por tamaño de archivos (segundo campo)
ls -lh | head -n 5 > archivos.txt
sort -n -k 2 archivos.txt
```

---

## 🔄 3. Comando `uniq`

Informa u omite líneas repetidas. **Nota:** requiere que el archivo esté ordenado primero.

### Sintaxis

```bash
uniq <opciones> <fichero>
```

### Opciones principales

| Opción | Función |
|--------|---------|
| `-c` | Cuenta repeticiones de cada línea |
| `-d` | Muestra solo líneas duplicadas |
| `-u` | Muestra solo líneas únicas (no repetidas) |
| `-i` | Ignora mayúsculas/minúsculas |

### Actividad 7 — Uso de `uniq`

```bash
# Crear archivo uniq.txt
nano uniq.txt
```

Contenido:
```
prueba1
prueba1
prueba1
Prueba1
Prueba2
prueba2
test1
test1
Test1
```

```bash
# Ver contenido
cat uniq.txt

# Eliminar duplicados adyacentes
uniq uniq.txt

# Ignorar mayúsculas/minúsculas
uniq -i uniq.txt

# Contar repeticiones
uniq -c uniq.txt

# Mostrar solo líneas no repetidas
uniq -u uniq.txt

# Mostrar solo líneas repetidas
uniq -d uniq.txt
```

---

## 🔤 4. Comando `tr` (translate)

Se usa para:
1. **Cambiar caracteres** (mayúsculas ↔ minúsculas, reemplazo)
2. **Borrar caracteres**

### Sintaxis

```bash
tr <opciones> <conjunto_1> <conjunto_2>
```

### Actividad 8 — Traducción de caracteres

```bash
# Reemplazar 'a' por 'A'
echo "Esto es un ejemplo de tr para la clase de hoy" | tr 'a' 'A'

# Reemplazar vocales minúsculas por mayúsculas
echo "Esto es un ejemplo de tr para la clase de hoy" | tr 'aeiou' 'AEIOU'

# Aplicar a /etc/passwd (primeras líneas)
cat /etc/passwd | tr "aeiou" "AEIOU" | head

# Equivalente con redirección
tr "aeiou" "AEIOU" < /etc/passwd | head

# Convertir todo a mayúsculas
echo "Esto es un ejemplo de tr para la clase de hoy" | tr 'a-z' 'A-Z'

# Borrar el carácter 'e'
echo "Esto es un ejemplo de tr para la clase de hoy" | tr -d "e"

# Borrar 'E' y 'e'
echo "Esto es un ejemplo de tr para la clase de hoy" | tr -d "Ee"
```

### Actividad 9 — Complemento y borrado en bioinformática

```bash
# Secuencia de ADN
echo "AAA TTTTT GGGG AAA TTTT NNN"

# Borrar espacios
echo "AAA TTTTT GGGG AAA TTTT NNN" | tr -d " "

# Complemento: reemplazar todo lo que NO sea ATGN por '-'
echo "AAA TTTTT GGGG AAA TTTT NNN" | tr -c "ATGN" "-"
```

---

## #️⃣ 5. Comando `wc` (word count)

Cuenta líneas, palabras y caracteres.

### Sintaxis

```bash
wc <opciones> <fichero(s)>
```

### Opciones

| Opción | Función |
|--------|---------|
| `-c` | Cuenta bytes |
| `-m` | Cuenta caracteres |
| `-l` | Cuenta líneas |
| `-w` | Cuenta palabras |
| `-L` | Longitud de la línea más larga |

### Actividad 10 — Conteo en archivos

```bash
# Crear archivo secuencias.txt
nano secuencias.txt
```

Contenido:
```
1ACTG
2ACTG
3TTTGACA
```

```bash
# Sin opciones: líneas, palabras, bytes
wc secuencias.txt

# Solo bytes
wc -c secuencias.txt

# Solo caracteres
wc -m secuencias.txt

# Solo palabras
wc -w secuencias.txt
```

---

## 🔄 6. Comando `rev`

Invierte las líneas de texto carácter por carácter.

### Sintaxis

```bash
rev <opciones> <fichero>
```

### Actividad 11 — Reversión de secuencias

```bash
# Crear archivo secuencias2.txt
nano secuencias2.txt
```

Contenido:
```
AAATTT
GGGCC
CATG
```

```bash
# Reversión de una cadena directamente
echo "ATGC" | rev

# Reversión de archivo
rev secuencias2.txt
```

> 💡 **Aplicación bioinformática:** `rev` se combina con `tr` para obtener la **hebra complementaria inversa** de una secuencia de ADN.

---

## 📏 7. Comando `fold`

Divide líneas en un ancho especificado.

### Sintaxis

```bash
fold <opciones> <fichero>
```

### Actividad 12 — Formato FASTA

```bash
# Descargar genoma del Ébola (ejemplo)
curl -s "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi?id=AF086833.2&db=nuccore&rettype=fasta" > Ebola_genome.fasta

# Formatear a 50 caracteres por línea (estándar FASTA)
grep -v ">" Ebola_genome.fasta | fold -w 50

# Formatear a 30 caracteres por línea
grep -v ">" Ebola_genome.fasta | fold -w 30
```

---

## ✏️ 8. Comando `sed` (stream editor)

Editor de flujo para transformaciones de texto.

### Características
- No realiza cambios directamente en el archivo (a menos que use `-i`)
- Opera línea a línea
- Soporta búsqueda y sustitución con expresiones regulares

### Sintaxis

```bash
sed [opciones] ['órdenes'] [archivo(s)]
```

### Opciones principales

| Opción | Función |
|--------|---------|
| `-n` | Suprime impresión automática |
| `-i` | Edita el archivo en lugar (in-place) |
| `-e` | Permite múltiples órdenes |
| `-E` | Usa expresiones regulares extendidas |

### Órdenes principales

| Orden | Función |
|-------|---------|
| `p` | Imprime línea |
| `d` | Elimina línea |
| `s` | Sustituye texto |
| `i` | Inserta antes de la línea |
| `a` | Inserta después de la línea |
| `c` | Cambia (reemplaza) la línea |

### Actividad 13 — Impresión duplicada

```bash
# Crear archivo dias.txt
nano dias.txt
```

Contenido:
```
Lunes
Martes
Miércoles
Jueves
Viernes
Sábado
Domingo
```

```bash
# Imprime cada línea dos veces (por defecto sed imprime + orden p imprime)
sed 'p' dias.txt
```

### Actividad 14 — Impresión controlada con `-n`

```bash
# -n suprime impresión automática; 'p' imprime solo las seleccionadas
sed -n 'p' dias.txt
```

> **Anatomía del comando:** `sed` → comando principal | `-n` → suprimir impresión automática | `'p'` → imprimir líneas seleccionadas | `dias.txt` → archivo editable

### Actividad 15 — Selección de líneas específicas

```bash
# Imprimir solo la primera línea
sed -n '1p' dias.txt

# Imprimir las primeras 5 líneas
sed -n '1,5p' dias.txt

# Imprimir desde línea 1 y 4 más adelante
sed -n '1,+4p' dias.txt

# Imprimir cada 2 líneas (líneas impares)
sed -n '1~2p' dias.txt
```

### Actividad 16 — Eliminación de líneas

```bash
# Eliminar la primera línea
sed '1d' dias.txt

# Eliminar las primeras 5 líneas
sed '1,5d' dias.txt

# Eliminar desde línea 1 y 4 más adelante
sed '1,+4d' dias.txt

# Eliminar cada 2 líneas (líneas impares)
sed '1~2d' dias.txt
```

### Actividad 17 — Inserción y modificación

```bash
# Insertar antes de la línea 7
sed '7i\Sábado_again' dias.txt

# Insertar después de la línea 7
sed '7a\Sábado_again' dias.txt

# Cambiar (reemplazar) la línea 7
sed '7c\Sábado_again' dias.txt
```

### Actividad 18 — Sustitución básica

```bash
# Reemplazar primera ocurrencia de "Lunes" por "Inicio de semana"
sed 's/Lunes/Inicio de semana/' dias.txt

# Suprimir impresión y mostrar solo si hubo sustitución
sed -n 's/Lunes/Inicio de semana/' dias.txt

# Mostrar solo líneas donde se realizó sustitución
sed -n 's/Lunes/Inicio de semana/p' dias.txt

# Reemplazar "Domingo" por "Fin de semana"
sed -n 's/Domingo/Fin de semana/p' dias.txt
```

### Actividad 19 — Sustitución global

```bash
# Crear archivo dias2.txt
nano dias2.txt
```

Contenido:
```
Lunes Lunes
Lunes
Martes
Miércoles
Jueves
Viernes
Sábado
Domingo
```

```bash
# Reemplazar solo primera ocurrencia por línea
sed 's/Lunes/Inicio de semana/' dias2.txt

# Reemplazar TODAS las ocurrencias (global)
sed 's/Lunes/Inicio de semana/g' dias2.txt
```

### Actividad 20 — Sustitución global en texto

```bash
# Crear archivo song.txt
nano song.txt
```

Contenido:
```
this is the song that never ends
yes, it goes on and on, my friend
some people started saying it
not knowing what it was
and they'll continue saying it forever just
```

```bash
# Reemplazar primera ocurrencia de "on" por "ON" por línea
sed 's/on/ON/' song.txt

# Reemplazar TODAS las ocurrencias
sed 's/on/ON/g' song.txt
```

### Actividad 21 — Sustitución de la n-ésima ocurrencia

```bash
# Reemplazar la SEGUNDA ocurrencia de "on" en cada línea
sed 's/on/forward/2' song.txt
```

### Actividad 22 — Edición in-place con backup

```bash
# Modificar archivo directamente creando backup (.back)
sed -i.back '1~2d' dias.txt

# Ver archivos
cat dias.txt
cat dias.txt.back
```

### Actividad 23 — Expresiones regulares extendidas

```bash
# Usar -E para regex extendidas
sed -E 's/(that|what) (never|it)/REPLACED \2/' song.txt
```

### Actividad 24 — Sustitución en rango de líneas

```bash
# Reemplazar "it" por "IT" solo en líneas 3 a 5
sed -E '3,5s/it/IT/' song.txt
```

### Actividad 25 — Ejercicio integrador

```bash
# Crear archivo data.txt
nano data.txt
```

Contenido:
```
seq1 chr1 19
seq3 chr1 34
seq2 chr2 182
seq1 chr1 055
seq2 chr1 133
seq4 chr3 22
```

```bash
# 1. Reemplazar cada ocurrencia de 'chr' por 'CHR'
sed 's/chr/CHR/g' data.txt

# 2. Imprimir líneas 3 y 5
sed -n -e '3p' -e '5p' data.txt
# o equivalente:
sed -n '3p;5p' data.txt

# 3. Insertar "seq5 chr5 33" antes de la línea 3
sed '3i\seq5 chr5 33' data.txt

# 4. Eliminar líneas vacías
sed -E '/^$/d' data.txt
```

---

## 🧮 9. Comando `awk`

`awk` es un **lenguaje de programación** y procesador de texto para manipular datos estructurados.

### Características
- Excelente para archivos de datos tabulares
- Opera línea por línea
- Procesa campos (columnas) automáticamente

### Sintaxis

```bash
awk '/patrón_búsqueda/ { acción_a_realizar; otra_acción }' archivo
```

- La búsqueda se especifica entre barras `/ /`
- La acción se indica entre llaves `{ }`
- Cada acción se separa con punto y coma `;`

### Actividad 27 — Imprimir todo el archivo

```bash
# Sin patrón: ejecuta acción en todas las líneas
awk '{print}' song.txt
```

### Actividad 28 — Filtrar por patrón

```bash
# Sin acción: imprime líneas que coinciden con el patrón
awk '/people/' song.txt
```

### Actividad 29 — Imprimir campos específicos

```bash
# Líneas que terminan en 's', imprimir primer campo
awk '/s$/ {print $1}' song.txt

# Imprimir segundo campo
awk '/s$/ {print $2}' song.txt

# Imprimir tercer campo
awk '/s$/ {print $3}' song.txt

# Imprimir línea completa ($0)
awk '/s$/ {print $0}' song.txt
```

### Actividad 30 — Separador de campos personalizado

```bash
# Crear archivo word_guion.txt
nano word_guion.txt
```

Contenido:
```
amarillo-3
azul-7
verde-4
negro-6
```

```bash
# Por defecto usa espacios como separador (no funciona bien aquí)
awk '/^a/ {print $1}' word_guion.txt

# Definir '-' como separador de campos
awk -F '-' '/^a/ {print $1}' word_guion.txt

# Imprimir segundo campo (número)
awk -F '-' '/^a/ {print $2}' word_guion.txt
```

### Actividad 31 — Variables internas de `awk`

```bash
# Usando -F
awk -F '-' '/^a/ {print $1}' word_guion.txt

# Usando variable interna FS (Field Separator)
awk -v FS='-' '/^a/ {print $2}' word_guion.txt
```

### Variables internas importantes

| Variable | Descripción |
|----------|-------------|
| `FS` | Separador de campos de entrada |
| `OFS` | Separador de campos de salida |
| `RS` | Separador de registros (líneas) de entrada |
| `ORS` | Separador de registros de salida |
| `NF` | Número de campos en la línea actual |
| `NR` | Número de registro (línea) actual |
| `FILENAME` | Nombre del archivo en proceso |
| `ARGC` / `ARGV` | Argumentos de línea de comandos |

---

## 📁 Archivos de la sesión

| Archivo | Descripción |
|---------|-------------|
| `references.txt` | Secuencias de referencia para grep |
| `lista1.txt` / `lista2.txt` | Listas de genes para comparación |
| `palabras.txt` | Texto para práctica de regex |
| `genes.txt` | Lista de genes para sort |
| `uniq.txt` | Texto con duplicados |
| `secuencias.txt` / `secuencias2.txt` | Secuencias de ADN |
| `dias.txt` / `dias2.txt` | Días de la semana |
| `song.txt` | Texto para práctica de sed |
| `data.txt` | Datos tabulares para sed y awk |
| `word_guion.txt` | Datos con separador guión |
| `Ebola_genome.fasta` | Genoma de referencia (descarga NCBI) |

---

## 🔗 Comandos relacionados entre sí

```
┌─────────────────────────────────────────────────────────────┐
│                    PIPELINE TÍPICO EN BIOINFORMÁTICA         │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│   cat archivo.fasta                                         │
│       │                                                     │
│       ▼                                                     │
│   grep ">"        →  Filtra headers                         │
│       │                                                     │
│       ▼                                                     │
│   grep -v ">"     →  Filtra secuencia (sin headers)          │
│       │                                                     │
│       ▼                                                     │
│   tr -d "\n"     →  Elimina saltos de línea                 │
│       │                                                     │
│       ▼                                                     │
│   grep -E "PATRÓN" →  Busca motivos de secuencia             │
│       │                                                     │
│       ▼                                                     │
│   sed 's/ATG/START/g' →  Sustituye codón de inicio           │
│       │                                                     │
│       ▼                                                     │
│   awk -F ',' '{print $2}' →  Extrae columnas                 │
│       │                                                     │
│       ▼                                                     │
│   sort | uniq -c  →  Ordena y cuenta duplicados            │
│       │                                                     │
│       ▼                                                     │
│   wc -l           →  Cuenta líneas finales                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## 📚 Referencias de la sesión

- Sobell & Helmke (2017). *A Practical Guide to Linux Commands* (4a ed.). Pearson — **Cap. 4-5**
- Shotts (2019). *The Linux Command Line* (2a ed.). No Starch Press — **Cap. 19-20**
- Hausenblas (2022). *Learning Modern Linux*. O'Reilly — **Cap. 3**

---

> **Siguiente sesión:** [Sesión 07 — Introducción a Shell Scripting](../sesion_07/)

---

_Repositorio del curso 1ABL0014 · UPC · Semestre 2026-1_
