# 🧬 Sesión 9: Introducción a la Programación en Shell
## Principios de Programación en Bioinformática - Unidad 02

**Docentes:** Frank Guzmán Escudero, Manuel Ramírez Sáenz  
**Universidad:** UPC - Facultad de Ciencias de la Salud - Programa de Biología  
**Horario:** 1:00 pm - 5:00 pm (Descanso: 2:45 pm - 3:00 pm)

---

## 🎯 Logro de la Sesión

Al finalizar esta sesión práctica, el estudiante:
- Comprenderá la utilidad y beneficios de la programación en shell a través de scripts
- Reconocerá los diferentes tipos de shell y su relación con el sistema
- Será capaz de crear, editar y ejecutar scripts básicos en Bash
- Utilizará editores de texto en terminal (VIM y Nano) para manipular archivos

**Palabras clave:** Programación en Shell, Scripts, Automatización, Eficiencia, Tipos de shell

---

## 📋 Pre-requisitos

- Acceso a Google Cloud Shell o cualquier terminal Linux
- Conocimientos básicos de navegación en terminal (cd, ls, mkdir)
- Cuenta de GitHub para subir evidencias

---

## 🧠 1. Recordando la Arquitectura de Linux

Antes de programar, recordemos dónde vive el shell:

```
┌─────────────────────────────────────────┐
│  User Space                             │
│  ┌─────┐ ┌─────┐ ┌─────┐ ┌──────────┐  │
│  │Shell│ │ sshd│ │ grep│ │Display   │  │
│  │     │ │     │ │     │ │server    │  │
│  └──┬──┘ └─────┘ └─────┘ └──────────┘  │
│     │                                   │
│  ┌──┴──┐                              │
│  │Terminal│                             │
│  └──────┘                              │
├─────────────────────────────────────────┤
│  syscalls                               │
├─────────────────────────────────────────┤
│  Kernel                                 │
│  ┌────────┐┌────────┐┌────────┐        │
│  │Procesos││ Memory ││Filesystems│       │
│  └────────┘└────────┘└────────┘        │
├─────────────────────────────────────────┤
│  Hardware                               │
│     CPU      RAM      HDD/SSD           │
└─────────────────────────────────────────┘
```

**El Shell** es el intérprete de comandos que provee una interfaz entre el usuario y el kernel del S.O. Interpreta los comandos que escribes para que puedan ser ejecutados.

**Flujo de datos en el shell:**
```
Keyboard → stdin → [Shell] → stdout → Monitor
                          → stderr → Monitor
```

---

## 🚀 2. ¿Qué es un Script y Por Qué Usarlo?

### Definición
Un **script** es un archivo que contiene una secuencia de comandos escritos en un lenguaje de shell específico (como Bash). Se utilizan para automatizar tareas o realizar una serie de acciones en el sistema operativo.

**Shell scripting:** El acto de escribir y desarrollar scripts utilizando un lenguaje de shell.

### Ventajas del Uso de Scripts

| Ventaja | Descripción | Ejemplo Bioinformático |
|---------|-------------|----------------------|
| **🔄 Automatización** | Automatiza tareas repetitivas. "Construye una vez y ejecuta muchas veces" | Procesar 100 archivos FASTQ automáticamente |
| **📱 Accesibilidad** | Fáciles de escribir, no requieren software especial. Editables con cualquier editor de texto | Editar un pipeline en la terminal de un servidor remoto |
| **🌐 Portabilidad** | Ejecutables en varias plataformas a través de máquinas virtuales o contenedores | Replicar análisis en servidor local y en la nube |
| **🧩 Flexibilidad** | Altamente personalizables, combinables con otros lenguajes | Integrar Python + Bash en un pipeline de análisis |
| **🔗 Integración** | Se integran con bases de datos, servidores web y servicios en la nube | Subir resultados a AWS S3 automáticamente |
| **🐛 Depuración** | La mayoría de shells incluyen herramientas para identificar problemas rápidamente | Usar `set -x` para debuggear un pipeline |

---

## 📝 3. Editores de Texto: La Herramienta del Bioinformático

Los editores de texto son programas que te permiten crear, modificar y guardar archivos de texto. Son herramientas básicas esenciales en sistemas Unix/Linux.

### 3.1 VIM - Vi IMproved

**Historia:**
- **Vi** creado por Bill Joy en 1976 (Berkeley)
- **Vim** lanzado por Bram Moolenaar en 1991
- Multiplataforma, gratuito y de código abierto

**Instalación:**
```bash
sudo apt install vim
```

**Iniciar Vim:**
```bash
vim
# o
vim nombre_archivo.txt
```

#### Los Cuatro Modos de Vim

| Modo | Tecla de acceso | Función |
|------|-----------------|---------|
| **Normal** | Por defecto | Navegar, copiar, cortar, pegar |
| **Insertar** | `i` | Escribir texto |
| **Línea de Comandos** | `:` | Guardar, salir, buscar, reemplazar |
| **Visual** | `v` | Seleccionar texto |

#### Comandos Esenciales de Vim

**Navegación y edición básica:**
```bash
# Crear o abrir archivo
vim prueba.txt

# Modo Insertar: presionar 'i'
# Escribir texto...
# "Vamos a crear un script in bash en esta segunda parte del curso"

# Guardar y salir
# 1. Presionar ESC (volver a modo normal)
# 2. Escribir :wq y presionar ENTER

# Salir sin guardar
:q!

# Guardar sin salir
:w
```

**Copiar, Cortar y Pegar:**
```bash
# 1. Colocar cursor al inicio del texto a copiar
# 2. Presionar 'v' (modo Visual)
# 3. Mover cursor al final del texto (se resaltará)
# 4. Presionar 'y' para copiar (yank) o 'd' para cortar (delete)
# 5. Mover cursor a donde deseas pegar
# 6. Presionar 'p' para pegar después del cursor
#    Presionar 'P' para pegar antes del cursor
```

> 💡 **Tip:** Practica Vim con [Vim Adventures](https://vim-adventures.com/) - ¡aprende jugando!

---

### 3.2 NANO - Editor Amigable para Principiantes

Nano es un editor de texto básico y fácil de usar que se ejecuta en la línea de comandos. Ideal para ediciones rápidas y para quienes están menos familiarizados con Vim.

**Instalación:**
```bash
sudo apt-get install nano
```

**Interfaz de Nano:**
```
┌─────────────────────────────────────────────┐
│ GNU nano 6.2           New Buffer           │  ← Barra de título
│                                             │
│ ventana principal del editor que muestra    │  ← Área de edición
│ lo que está siendo editado.                 │
│                                             │
│                                             │
│ [ Welcome to nano. For basic help, type     │  ← Línea de estado
│   Ctrl+G. ]                                 │
│ ^G Help    ^O Write Out  ^W Where Is  ^K Cut│  ← Atajos (Ctrl+letra)
│ ^X Exit    ^R Read File  ^\ Replace   ^U Paste│
└─────────────────────────────────────────────┘
```

**Símbolos importantes:**
- `^` = Tecla **Ctrl**
- `M-` = Tecla **Alt** (Meta)

#### Comandos Más Comunes de Nano

| Atajo | Función |
|-------|---------|
| `Ctrl+G` | Ayuda |
| `Ctrl+O` | Guardar archivo (Write Out) |
| `Ctrl+X` | Salir |
| `Ctrl+K` | Cortar línea actual |
| `Ctrl+U` | Pegar |
| `Ctrl+W` | Buscar |
| `Ctrl+R` | Insertar otro archivo |
| `Alt+U` | Deshacer (Undo) |
| `Alt+E` | Rehacer (Redo) |
| `Ctrl+A` | Ir al inicio de la línea |
| `Ctrl+E` | Ir al final de la línea |

**Ejercicio práctico con Nano:**
```bash
# Crear archivo de bacterias patógenas
nano file.txt

# Escribir:
Staphylococcus aureus
Enterococcus faecium
Streptococcus pneumoniae
Escherichia coli
Pseudomonas aeruginosa

# Practicar:
# - Ctrl+A (inicio de línea)
# - Ctrl+E (final de línea)
# - Ctrl+K (cortar línea)
# - Ctrl+U (pegar)
# - Alt+U (deshacer)
# - Ctrl+O, ENTER (guardar)
# - Ctrl+X (salir)
```

---

### 3.3 Visual Studio Code (Opcional)

Para desarrollo más avanzado, VS Code ofrece:
- Terminal integrada
- Resaltado de sintaxis para shell scripts
- Extensiones para Bash
- Integración con Git

**Descarga:** [code.visualstudio.com](https://code.visualstudio.com)

---

## 🐚 4. Tipos de Shell

### 4.1 Familias Principales

| Shell | Creador | Características | Prompt |
|-------|---------|-----------------|--------|
| **Bourne Shell (sh)** | Steven Bourne | Primera shell tradicional UNIX, base de compatibilidad | `$` |
| **Bourne Again Shell (bash)** | GNU Project | Shell moderno más utilizado. Edición de línea, historial ilimitado, control de trabajos, alias, aritmética | `$` |
| **Korn Shell (ksh)** | David Korn | Historial de órdenes, edición en línea, características ampliadas de programación | `$` |
| **Z Shell (zsh)** | - | Completado avanzado, temas, plugins, expansión poderosa | `%` |
| **C Shell (csh)** | Bill Joy (Berkeley) | Popular en BSD, características para programadores en C | `%` |
| **Tenex C Shell (tcsh)** | - | Superconjunto de csh, más rápido, edición de línea y historial | `%` |

### 4.2 Verificando tu Shell Actual

```bash
# ¿Qué shell estoy usando?
echo $SHELL
# Output esperado: /bin/bash

# ¿Cuál es la versión?
echo $BASH_VERSION
# Output esperado: 5.2.21(1)-release

# Ver shells disponibles en el sistema
cat /etc/shells
```

### 4.3 Tabla Comparativa

| Característica | sh | ksh | bash | zsh | csh | tcsh |
|----------------|----|-----|------|-----|-----|------|
| Sintaxis mejorada | No | Sí | Sí | Sí | Sí | Sí |
| Características avanzadas | Limitadas | Sí | Sí | Sí | Limitadas | Sí |
| Predeterminado en Linux | No | No | **Sí** | No | No | No |
| Popularidad | Menos | Menos | **Muy popular** | Popular (avanzados) | Menos | Popular (avanzados) |

---

## 💻 5. Creando tu Primer Script en Bash

### 5.1 El Shebang (`#!`)

La primera línea de un script indica al sistema qué intérprete usar:

```bash
#!/bin/bash
```

> 🔑 **Importante:** Siempre comienza tus scripts con el shebang apropiado.

### 5.2 Estructura Básica de un Script

```bash
#!/bin/bash
# Fecha de escritura: 15/05/2024
# Autor: [Tu nombre]
# Descripción: Script de ejemplo para bioinformática

# Este comando te informa en qué directorio te encuentras
lugar=$(pwd)

# Imprime mensaje informativo
echo "En este momento te encuentras trabajando en: $lugar"
echo "¡No vayas a perderte!"
```

### 5.3 Ejecutando el Script

```bash
# Paso 1: Crear el archivo
nano mi_primer_script.sh

# Paso 2: Escribir el contenido (usar ejemplo anterior)

# Paso 3: Guardar y salir (Ctrl+O, Enter, Ctrl+X)

# Paso 4: Dar permisos de ejecución
chmod +x mi_primer_script.sh

# Paso 5: Ejecutar
./mi_primer_script.sh

# Alternativa: ejecutar con bash explícitamente
bash mi_primer_script.sh
```

---

## 🧬 6. Actividad Práctica: Pipeline Bioinformático Básico

### Objetivo
Crear un script que automatice tareas comunes en bioinformática: manejo de archivos de secuencias, directorios de trabajo y reportes.

### Ejercicio 1: Script de Bienvenida y Navegación
```bash
#!/bin/bash
# ejercicio1_bienvenida.sh

echo "=========================================="
echo "  BIENVENIDO AL PIPELINE BIOINFORMÁTICO  "
echo "=========================================="
echo ""
echo "Usuario: $(whoami)"
echo "Fecha: $(date)"
echo "Directorio actual: $(pwd)"
echo ""
echo "Contenido del directorio:"
ls -lh
echo ""
echo "Espacio disponible en disco:"
df -h .
```

### Ejercicio 2: Creador de Estructura de Proyecto
```bash
#!/bin/bash
# ejercicio2_proyecto.sh

# Crear estructura de directorios para un proyecto de metagenómica
PROYECTO="mi_proyecto_bioinfo"

echo "Creando estructura de proyecto: $PROYECTO"

mkdir -p $PROYECTO/{datos_raw,datos_procesados,resultados,scripts,logs,referencias}

echo "Estructura creada:"
find $PROYECTO -type d | sort

echo ""
echo "¡Listo para comenzar tu análisis!"
```

### Ejercicio 3: Analizador de Archivos FASTA (Simulado)
```bash
#!/bin/bash
# ejercicio3_fasta.sh

# Crear archivo FASTA de ejemplo
cat > secuencias.fasta << 'EOF'
>seq1|Staphylococcus_aureus
ATCGATCGATCGATCGATCGATCGATCGATCGATCGATCGATCG
>seq2|Escherichia_coli
GCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTAGCTA
>seq3|Pseudomonas_aeruginosa
TTACGTTACGTTACGTTACGTTACGTTACGTTACGTTACGTTACG
EOF

echo "Archivo FASTA creado: secuencias.fasta"
echo ""
echo "Número de secuencias:"
grep -c ">" secuencias.fasta

echo ""
echo "Identificadores de secuencias:"
grep ">" secuencias.fasta

echo ""
echo "Longitud total del archivo:"
wc -c secuencias.fasta
```

---

## 📸 7. Actividad Asincrónica

### Entregables Obligatorios

Subir a tu repositorio de GitHub (`principios-programacion-bioinfo`) los siguientes **2 o 3 scripts funcionales**:

#### Script 1: Informe del Sistema (Obligatorio)
Crear un script llamado `informe_sistema.sh` que muestre:
- Nombre del usuario actual
- Fecha y hora del sistema
- Directorio de trabajo actual
- Uso de disco del directorio actual
- Lista de archivos con detalles

#### Script 2: Organizador de Datos (Obligatorio)
Crear un script llamado `organizador_datos.sh` que:
- Cree una estructura de carpetas para un proyecto bioinformático (mínimo 5 directorios)
- Cree archivos de ejemplo dentro de al menos 2 directorios
- Liste la estructura completa creada

#### Script 3: Procesador de Secuencias (Opcional - Bonus)
Crear un script llamado `procesador_fasta.sh` que:
- Genere un archivo FASTA con al menos 3 secuencias de diferentes organismos.
- Cuente el número de secuencias.
- Extraiga los identificadores (líneas que comienzan con `>`).
- Calcule la longitud aproximada de cada secuencia.

### Checklist de Verificación

- [ ] Verifiqué mi shell con `echo $SHELL`
- [ ] Verifiqué la versión de bash con `echo $BASH_VERSION`
- [ ] Todos los scripts incluyen shebang `#!/bin/bash`
- [ ] Todos los scripts tienen permisos de ejecución (`chmod +x`)
- [ ] Los scripts se ejecutan sin errores
- [ ] Incluí screenshots de la ejecución exitosa
- [ ] Subí todo a GitHub con mensajes de commit descriptivos

---

## 📚 Referencias Bibliográficas

1. Hausenblas, M. (2022). *Learning Modern Linux*. O'Reilly Media, Inc. Chapter 1.  
   [https://learning.oreilly.com/library/view/learning-modern-linux/9781098108939/](https://learning.oreilly.com/library/view/learning-modern-linux/9781098108939/)

2. Blum, R., & Bresnahan, C. (2021). *Linux Command Line and Shell Scripting Bible* (4th ed.). Wiley.

3. Documentación oficial de Bash: [https://www.gnu.org/software/bash/manual/](https://www.gnu.org/software/bash/manual/)

4. Vim Adventures - Aprende Vim jugando: [https://vim-adventures.com/](https://vim-adventures.com/)

---

> 🎓 **Nota para estudiantes:** *"La programación en shell es el puente entre la biología y la computación. Dominar estos conceptos te permitirá automatizar análisis de secuencias, procesar datos ómicos y escalar tu investigación desde una muestra hasta miles."*

---

*Documento preparado para la Sesión 09 - Semana 9 del curso Principios de Programación en Bioinformática. UPC exígete, innova.*
