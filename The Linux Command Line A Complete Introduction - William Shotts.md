# Notas del Libro sobre la Shell de Linux

## PARTE I: APRENDIENDO LA SHELL

### Capítulo 1: ¿QUÉ ES LA SHELL?

*   **Shell:** Programa que toma comandos del teclado y los pasa al sistema operativo. `bash` (Bourne-Again SHell) es la shell más común en Linux, un reemplazo mejorado de `sh`.
*   **Emulador de Terminal:** Programa gráfico (GUI) para interactuar con la shell (ej: `konsole`, `gnome-terminal`).
*   **Prompt de la Shell:** Indicador de que la shell está lista (ej: `[usuario@máquina directorio]$`).
    *   `$`: Usuario normal.
    *   `#`: Superusuario (root).
*   **Comandos:**
    *   Introducir texto y presionar ENTER.
    *   Si el comando no es válido, la shell muestra un error.
*   **Historial de Comandos:**
    *   Flecha arriba/abajo: Navegar por comandos anteriores/siguientes.
    *   Recuerda ~1000 comandos por defecto.
*   **Movimiento del Cursor:**
    *   Flechas izquierda/derecha: Mover el cursor en la línea de comandos para editar.
*   **Copiar y Pegar (Ratón):**
    *   Seleccionar texto con el botón izquierdo.
    *   Pegar con el botón central.
    *   **Importante:** `CTRL-C` y `CTRL-V` **no** funcionan como copiar/pegar en la terminal; tienen otros significados.
*   **Comandos Simples Introducidos:**
    *   `date`: Muestra fecha y hora actual.
    *   `cal`: Muestra el calendario del mes actual.
    *   `df`: Muestra el espacio libre en los discos.
    *   `free`: Muestra la memoria libre.
*   **Terminar Sesión:**
    *   Cerrar la ventana del emulador.
    *   Comando `exit`.
    *   Presionar `CTRL-D`.
*   **Consolas Virtuales:**
    *   Sesiones de terminal detrás del escritorio gráfico.
    *   Acceso: `CTRL-ALT-F1` a `CTRL-ALT-F6`.
    *   Volver al escritorio gráfico: `ALT-F7` (puede variar).

### Capítulo 2: NAVEGACIÓN

*   **Comandos Introducidos:**
    *   `pwd`: (Print Working Directory) Muestra el directorio de trabajo actual.
    *   `cd`: (Change Directory) Cambia el directorio actual.
    *   `ls`: (List) Lista el contenido de un directorio.
*   **Árbol del Sistema de Archivos:**
    *   Estructura jerárquica de directorios (carpetas).
    *   Comienza en el directorio raíz (`/`).
    *   Linux tiene un único árbol, a diferencia de Windows (C:, D:). Los dispositivos se *montan* en puntos del árbol.
*   **Directorio de Trabajo Actual:** El directorio donde "estás" en un momento dado. Se muestra con `pwd`. Al iniciar sesión, suele ser el *directorio home* del usuario (`/home/usuario`).
*   **Listar Contenidos:** `ls` lista archivos y subdirectorios del directorio actual.
*   **Cambiar Directorio (`cd`):**
    *   Se usa seguido de la *ruta* (pathname) del directorio deseado.
    *   **Rutas Absolutas:** Empiezan desde la raíz (`/`). Ejemplo: `cd /usr/bin`.
    *   **Rutas Relativas:** Empiezan desde el directorio actual.
        *   `.` (punto): Se refiere al directorio actual.
        *   `..` (punto punto): Se refiere al directorio padre (el que está un nivel arriba).
        *   Ejemplos: `cd ..` (subir un nivel), `cd bin` (entrar al subdirectorio `bin`), `cd ./bin` (igual que el anterior, `./` es implícito).
*   **Hechos Importantes sobre Nombres de Archivo:**
    *   Los archivos que empiezan con `.` están ocultos (`ls -a` para verlos).
    *   Nombres y comandos son **sensibles a mayúsculas/minúsculas** (`Archivo1` != `archivo1`).
    *   Evitar espacios y caracteres especiales (excepto punto `.`, guion `-`, guion bajo `_`). Usar `_` en lugar de espacios.
    *   Linux **no** tiene concepto de "extensión de archivo" obligatoria para determinar el tipo, aunque muchas aplicaciones sí las usan.
*   **Atajos Útiles con `cd`:**
    *   `cd`: Va al directorio *home* del usuario.
    *   `cd -`: Va al directorio de trabajo *anterior*.
    *   `cd ~nombre_usuario`: Va al directorio *home* del usuario especificado.

### Capítulo 3: EXPLORANDO EL SISTEMA

*   **Comandos Introducidos:**
    *   `ls` (con más opciones): Listar contenido de directorios.
    *   `file`: Determinar el tipo de un archivo.
    *   `less`: Ver el contenido de archivos de texto (paginador).
*   **Más sobre `ls`:**
    *   `ls [directorio(s)]`: Lista el contenido del directorio(s) especificado(s).
    *   `ls -l`: Formato largo, muestra detalles (permisos, propietario, tamaño, fecha).
    *   **Opciones y Argumentos:** `comando -opciones argumentos`.
        *   Opciones cortas: `-l`, `-t`. Se pueden agrupar: `ls -lt`.
        *   Opciones largas: `--reverse`.
        *   **Sensible a mayúsculas/minúsculas.**
    *   **Opciones Comunes de `ls`:**
        *   `-a`, `--all`: Listar todos los archivos (incluidos los ocultos).
        *   `-A`, `--almost-all`: Como `-a` pero sin `.` y `..`.
        *   `-d`, `--directory`: Listar directorios en sí, no su contenido (útil con `-l`).
        *   `-F`, `--classify`: Añadir indicador al final (`/` para directorios).
        *   `-h`, `--human-readable`: Mostrar tamaños legibles (KB, MB, GB) con `-l`.
        *   `-l`: Formato largo.
        *   `-r`, `--reverse`: Orden inverso.
        *   `-S`: Ordenar por tamaño.
        *   `-t`: Ordenar por fecha de modificación.
    *   **Formato Largo (`ls -l`):** `-rw-r--r-- 1 root root 32059 2017-04-03 11:05 archivo.txt`
        *   `-`: Tipo de archivo (`-`=regular, `d`=directorio, `l`=enlace simbólico).
        *   `rw-r--r--`: Permisos (owner, group, world).
        *   `1`: Número de enlaces duros.
        *   `root`: Propietario.
        *   `root`: Grupo propietario.
        *   `32059`: Tamaño en bytes.
        *   `2017-04-03 11:05`: Fecha/hora de última modificación.
        *   `archivo.txt`: Nombre del archivo.
*   **Comando `file`:**
    *   `file nombre_archivo`: Determina el tipo de archivo (ej: `JPEG image data`, `ASCII text`).
    *   Útil porque las extensiones no son obligatorias ni garantizan el tipo en Linux.
*   **Comando `less`:**
    *   `less nombre_archivo`: Visor de archivos de texto. Permite scroll adelante/atrás.
    *   Archivos de configuración y scripts suelen ser texto plano (ASCII).
    *   ASCII: Codificación simple de caracteres a números. No es un formato de procesador de texto (Word, Writer).
    *   **Comandos de `less`:**
        *   `PAGE UP` o `b`: Página atrás.
        *   `PAGE DOWN` o `espacio`: Página adelante.
        *   Flechas arriba/abajo: Línea arriba/abajo.
        *   `G`: Ir al final del archivo.
        *   `1G` o `g`: Ir al inicio del archivo.
        *   `/patron`: Buscar `patron` hacia adelante.
        *   `n`: Buscar siguiente ocurrencia.
        *   `h`: Ayuda.
        *   `q`: Salir.
    *   `less` es un *paginador*, mejor que `more`.
*   **Recorrido Guiado por el Sistema:**
    *   Estándar de Jerarquía del Sistema de Archivos (FHS).
    *   Usar `cd`, `ls -l`, `file`, `less` para explorar.
    *   Comando `reset`: Recupera la terminal si se desconfigura al ver un archivo no-texto.
    *   **Directorios Comunes:**
        *   `/`: Raíz.
        *   `/bin`: Binarios esenciales del sistema.
        *   `/boot`: Kernel de Linux, gestor de arranque (GRUB).
        *   `/dev`: Archivos de dispositivo (discos, terminales).
        *   `/etc`: Archivos de configuración de todo el sistema, scripts de inicio.
        *   `/home`: Directorios home de los usuarios.
        *   `/lib`: Bibliotecas compartidas esenciales.
        *   `/lost+found`: Archivos recuperados tras corrupción del sistema de archivos.
        *   `/media`: Puntos de montaje para medios extraíbles (USB, CD).
        *   `/mnt`: Punto de montaje tradicional para sistemas montados manualmente.
        *   `/opt`: Software opcional (a menudo comercial).
        *   `/proc`: Sistema de archivos virtual del kernel (información del sistema en tiempo real).
        *   `/root`: Directorio home del superusuario.
        *   `/sbin`: Binarios del sistema (administración).
        *   `/tmp`: Archivos temporales.
        *   `/usr`: Programas y datos de usuario (el más grande).
        *   `/usr/bin`: Programas principales instalados por la distribución.
        *   `/usr/lib`: Bibliotecas compartidas para `/usr/bin`.
        *   `/usr/local`: Software instalado localmente (no por la distro).
        *   `/usr/sbin`: Más programas de administración.
        *   `/usr/share`: Datos compartidos (iconos, fondos, docs).
        *   `/usr/share/doc`: Documentación de paquetes.
        *   `/var`: Datos variables (logs, correo, bases de datos).
        *   `/var/log`: Archivos de log del sistema (ej: `messages`, `syslog`).
*   **Enlaces Simbólicos (Symbolic Links / Soft Links / Symlinks):**
    *   Un archivo que apunta a otro archivo o directorio.
    *   Permiten referenciar archivos con múltiples nombres o versiones.
    *   `ls -l` los muestra con `l` al inicio y `->` apuntando al destino.
    *   Si el destino se borra, el enlace queda *roto* (broken).
    *   Se crean con `ln -s destino enlace`.
*   **Enlaces Duros (Hard Links):**
    *   Permiten múltiples nombres para el *mismo* archivo (mismo inodo).
    *   Limitaciones: No pueden apuntar a directorios, no pueden cruzar sistemas de archivos.
    *   Indistinguibles del archivo original en `ls -l` (excepto por el contador de enlaces).
    *   El archivo real se borra solo cuando se elimina el último enlace duro.

### Capítulo 4: MANIPULANDO ARCHIVOS Y DIRECTORIOS

*   **Comandos Introducidos:**
    *   `cp`: Copiar archivos y directorios.
    *   `mv`: Mover/Renombrar archivos y directorios.
    *   `mkdir`: Crear directorios.
    *   `rm`: Eliminar archivos y directorios.
    *   `ln`: Crear enlaces duros y simbólicos.
*   **Ventaja de la Línea de Comandos:** Poder y flexibilidad para tareas complejas (ej: copiar solo archivos `.html` más nuevos).
*   **Comodines (Wildcards / Globbing):** Caracteres especiales para especificar grupos de nombres de archivo.
    *   `*`: Coincide con cualquier secuencia de caracteres.
    *   `?`: Coincide con cualquier carácter individual.
    *   `[caracteres]`: Coincide con cualquier carácter que sea miembro del conjunto `caracteres`. (Ej: `[aeiou]`)
    *   `[!caracteres]`: Coincide con cualquier carácter que **no** sea miembro del conjunto `caracteres`.
    *   `[[:clase:]]`: Coincide con cualquier carácter miembro de la clase especificada.
        *   `[:alnum:]`: Alfanuméricos.
        *   `[:alpha:]`: Alfabéticos.
        *   `[:digit:]`: Numéricos.
        *   `[:lower:]`: Letras minúsculas.
        *   `[:upper:]`: Letras mayúsculas.
    *   **Ejemplos:**
        *   `*`: Todos los archivos.
        *   `g*`: Empiezan con `g`.
        *   `b*.txt`: Empiezan con `b`, terminan con `.txt`.
        *   `Data???`: Empiezan con `Data` seguido de 3 caracteres.
        *   `[abc]*`: Empiezan con `a`, `b` o `c`.
        *   `[[:upper:]]*`: Empiezan con mayúscula.
    *   **Importante:** Usar con precaución con `rm`. Probar primero con `ls`.
*   **Comando `mkdir`:**
    *   `mkdir directorio...`: Crea uno o más directorios.
*   **Comando `cp`:**
    *   `cp origen destino`: Copia un archivo/directorio `origen` a `destino`.
    *   `cp item... directorio`: Copia múltiples ítems a un `directorio`.
    *   **Opciones Comunes:**
        *   `-a`, `--archive`: Copia recursiva preservando atributos (permisos, propietario, fecha).
        *   `-i`, `--interactive`: Pregunta antes de sobrescribir.
        *   `-r`, `--recursive`: Copia directorios y su contenido recursivamente (necesario para copiar directorios).
        *   `-u`, `--update`: Copia solo si el origen es más nuevo que el destino o si el destino no existe.
        *   `-v`, `--verbose`: Muestra mensajes informativos.
*   **Comando `mv`:**
    *   Mueve o renombra archivos/directorios. El nombre original deja de existir.
    *   `mv origen destino`: Mueve/renombra `origen` a `destino`.
    *   `mv item... directorio`: Mueve ítems a `directorio`.
    *   **Opciones Comunes:** `-i`, `-u`, `-v` (similares a `cp`).
*   **Comando `rm`:**
    *   `rm item...`: Elimina (borra) archivos o directorios.
    *   **¡¡¡PELIGRO!!!:** En Linux/Unix **no hay comando "undelete"**. Una vez borrado con `rm`, se pierde. Usar con extrema precaución, especialmente con comodines.
    *   **Opciones Comunes:**
        *   `-i`, `--interactive`: Pregunta antes de borrar.
        *   `-r`, `--recursive`: Borra directorios y su contenido recursivamente (necesario para borrar directorios).
        *   `-f`, `--force`: Ignora archivos no existentes y no pregunta (sobrescribe `-i`). ¡Muy peligroso!
        *   `-v`, `--verbose`: Muestra mensajes.
*   **Comando `ln`:**
    *   `ln archivo enlace`: Crea un enlace **duro**.
    *   `ln -s item enlace`: Crea un enlace **simbólico** (`item` puede ser archivo o directorio).
    *   **Enlaces Duros:** Múltiples nombres para el mismo archivo (mismo inodo). Limitado al mismo sistema de archivos, no para directorios.
    *   **Enlaces Simbólicos:** Un archivo especial que contiene un puntero de texto al destino. Pueden cruzar sistemas de archivos y apuntar a directorios. Son preferidos en la práctica moderna.
*   **Playground (Ejercicios):** Se recomienda crear un directorio `playground` para practicar estos comandos de forma segura.

### Capítulo 5: TRABAJANDO CON COMANDOS

*   **Comandos Introducidos:**
    *   `type`: Indica cómo se interpreta un nombre de comando.
    *   `which`: Muestra qué programa ejecutable se ejecutará.
    *   `help`: Obtiene ayuda para *builtins* de la shell.
    *   `man`: Muestra la página del manual de un comando.
    *   `apropos`: Muestra una lista de comandos apropiados (búsqueda en man).
    *   `info`: Muestra la entrada *info* de un comando (alternativa a man).
    *   `whatis`: Muestra descripciones de una línea de páginas man.
    *   `alias`: Crea un alias para un comando.
*   **Tipos de Comandos:** Un comando puede ser:
    1.  **Un programa ejecutable:** Binarios compilados (C, C++) o scripts (shell, Python, etc.). Ej: `/usr/bin/ls`.
    2.  **Un comando incorporado (builtin) de la shell:** Ejecutado directamente por `bash`. Ej: `cd`, `type`, `help`.
    3.  **Una función de la shell:** Mini-scripts definidos en el entorno.
    4.  **Un alias:** Nombre personalizado para otro comando (con opciones).
*   **Identificar Comandos:**
    *   `type comando`: Muestra si es builtin, alias, función o archivo ejecutable (y su ruta si es alias o ejecutable).
    *   `which comando`: Muestra la ruta completa del *ejecutable* que se usaría. No funciona para builtins o funciones puras.
*   **Obtener Documentación:**
    *   `help comando_builtin`: Ayuda específica para comandos incorporados de `bash`.
    *   `comando --help`: Muchos ejecutables muestran información de uso con esta opción.
    *   `man comando`: Muestra la página del manual (referencia detallada).
        *   Secciones del manual (1=comandos usuario, 5=formatos archivo, 8=comandos admin, etc.).
        *   `man sección término_búsqueda`: Ver sección específica (ej: `man 5 passwd`).
        *   Navegación usualmente con `less`.
    *   `apropos palabra_clave` (o `man -k palabra_clave`): Busca páginas man relacionadas con `palabra_clave`.
    *   `whatis comando`: Descripción breve (una línea) de la página man.
    *   `info comando`: Sistema de documentación alternativo (GNU), hiperenlazado, navegado con `info`. Teclas: `n`(next), `p`(previous), `u`(up), `Enter`(seguir enlace), `q`(quit).
    *   Archivos `README` y otros en `/usr/share/doc/paquete`: Documentación adicional, a menudo archivos de texto (`less`) o HTML. `zless` para ver archivos `.gz`.
*   **Crear Comandos Propios con `alias`:**
    *   Permite crear atajos para comandos largos o con opciones frecuentes.
    *   Sintaxis: `alias nombre='comando'` (sin espacios alrededor del `=`).
    *   Ejemplo: `alias ll='ls -l --color=auto'`.
    *   `alias` (sin argumentos): Muestra todos los alias definidos.
    *   `unalias nombre`: Elimina un alias.
    *   Los alias definidos en la línea de comandos desaparecen al cerrar la sesión. Para hacerlos permanentes, se añaden a archivos de inicio como `.bashrc` (Capítulo 11).
    *   Se pueden poner varios comandos en un alias separándolos por `;`. Ej: `alias test='cd /usr; ls; cd -'`.

### Capítulo 6: REDIRECCIÓN

*   **Comandos Introducidos:**
    *   `cat`: Concatenar archivos y mostrar en salida estándar.
    *   `sort`: Ordenar líneas de texto.
    *   `uniq`: Reportar u omitir líneas repetidas (requiere entrada ordenada).
    *   `grep`: Imprimir líneas que coincidan con un patrón.
    *   `wc`: (Word Count) Contar líneas, palabras y bytes.
    *   `head`: Mostrar las primeras líneas de un archivo.
    *   `tail`: Mostrar las últimas líneas de un archivo.
    *   `tee`: Leer de entrada estándar y escribir a salida estándar y archivos.
*   **Entrada/Salida Estándar y Error Estándar:**
    *   **Salida Estándar (stdout, fd 1):** Donde los programas envían sus resultados normales (por defecto, la pantalla).
    *   **Error Estándar (stderr, fd 2):** Donde los programas envían mensajes de estado y error (por defecto, la pantalla).
    *   **Entrada Estándar (stdin, fd 0):** De donde los programas leen datos (por defecto, el teclado).
*   **Redirección de Salida Estándar (stdout):**
    *   `comando > archivo`: Redirige stdout a `archivo`. **Sobrescribe** el archivo si existe.
    *   `comando >> archivo`: Redirige stdout a `archivo`. **Añade** al final del archivo si existe.
    *   Ejemplo: `ls -l /usr/bin > lista_bin.txt`.
    *   `> archivo_vacio`: Trunca un archivo existente o crea uno nuevo vacío.
*   **Redirección de Error Estándar (stderr):**
    *   `comando 2> archivo_error`: Redirige stderr (fd 2) a `archivo_error`.
*   **Redirección de stdout y stderr al Mismo Archivo:**
    *   Forma tradicional: `comando > archivo 2>&1` (Redirige stdout a `archivo`, luego redirige stderr a donde apunta stdout (fd 1)). **El orden importa.**
    *   Forma moderna (bash): `comando &> archivo`.
    *   Para añadir a un archivo: `comando &>> archivo`.
*   **Descartar Salida:**
    *   Redirigir a `/dev/null` (el "bit bucket").
    *   Ejemplo (descartar errores): `comando 2> /dev/null`.
*   **Redirección de Entrada Estándar (stdin):**
    *   `comando < archivo`: Usa `archivo` como fuente de stdin en lugar del teclado.
    *   **Comando `cat`:**
        *   `cat archivo...`: Muestra el contenido de los archivos en stdout.
        *   `cat`: Sin argumentos, lee de stdin (teclado) y lo repite en stdout hasta `CTRL-D`.
        *   `cat > archivo`: Crea un archivo corto con lo tecleado hasta `CTRL-D`.
        *   `cat < archivo`: Equivalente a `cat archivo`.
        *   `cat archivo1 archivo2 > archivo_combinado`: Concatena archivos.
*   **Tuberías (Pipelines):**
    *   `comando1 | comando2`: Conecta la stdout de `comando1` a la stdin de `comando2`.
    *   Permite encadenar comandos para procesar datos.
    *   Ejemplo: `ls -l /usr/bin | less` (ver listado largo página a página).
*   **Filtros:** Comandos usados en pipelines que modifican datos.
    *   `sort`: Ordena líneas. `ls /bin /usr/bin | sort | less`.
    *   `uniq`: Elimina líneas duplicadas *adyacentes* (usar después de `sort`).
        *   `ls /bin /usr/bin | sort | uniq | less`.
        *   `uniq -d`: Muestra solo las líneas duplicadas.
    *   `wc`: Cuenta líneas (`-l`), palabras (`-w`), bytes (`-c`).
        *   `ls /bin /usr/bin | sort | uniq | wc -l` (cuenta archivos únicos).
    *   `grep`: Busca patrones.
        *   `ls /bin /usr/bin | sort | uniq | grep zip`.
        *   Opciones: `-i` (ignorar may/min), `-v` (invertir, mostrar líneas *sin* el patrón).
    *   `head`: Muestra las N primeras líneas (defecto 10). `head -n 5 archivo`.
    *   `tail`: Muestra las N últimas líneas (defecto 10). `tail -n 5 archivo`.
        *   `tail -f archivo`: Sigue mostrando líneas nuevas a medida que se añaden (útil para logs). `CTRL-C` para parar.
*   **Comando `tee`:**
    *   `comando | tee archivo | otro_comando`: Bifurca la tubería. Lee de stdin, escribe a stdout **y** al `archivo` especificado.
    *   Útil para guardar resultados intermedios en una pipeline.
    *   Ej: `ls /usr/bin | tee lista_completa.txt | grep zip`.

### Capítulo 7: VIENDO EL MUNDO COMO LO VE LA SHELL

*   **Comando Introducido:**
    *   `echo`: Muestra una línea de texto.
*   **Expansión:** Proceso por el cual la shell sustituye ciertos caracteres o secuencias por otros valores *antes* de ejecutar el comando.
    *   Ej: `echo *` no imprime `*`, sino los nombres de los archivos en el directorio actual. `echo` nunca ve el `*`.
*   **Tipos de Expansión:**
    *   **Expansión de Nombres de Ruta (Pathname Expansion):** Comodines (`*`, `?`, `[]`).
        *   Respeta archivos ocultos (`*` no los muestra).
        *   `echo .*` muestra ocultos pero también `.` y `..` (¡cuidado!).
        *   `echo .[!.]*` es más seguro para listar archivos ocultos sin `.` y `..`.
    *   **Expansión de Tilde (Tilde Expansion):**
        *   `~` se expande al directorio home del usuario actual. `echo ~` -> `/home/usuario`.
        *   `~nombre_usuario` se expande al home de ese usuario. `echo ~otro` -> `/home/otro`.
    *   **Expansión Aritmética (Arithmetic Expansion):**
        *   Sintaxis: `$((expresión))`.
        *   Realiza cálculos con **enteros**.
        *   Operadores: `+`, `-`, `*`, `/` (división entera), `%` (módulo/resto), `**` (exponenciación).
        *   Ej: `echo $(( (5**2) * 3 ))` -> `75`.
    *   **Expansión de Llaves (Brace Expansion):**
        *   Crea múltiples cadenas a partir de un patrón con llaves `{}`. No puede contener espacios sin comillas.
        *   `echo Front-{A,B,C}-Back` -> `Front-A-Back Front-B-Back Front-C-Back`.
        *   Rangos: `echo Number_{1..5}` -> `Number_1 Number_2 ... Number_5`.
        *   Rangos con relleno de ceros (bash >= 4.0): `echo {01..05}` -> `01 02 03 04 05`.
        *   Rangos de letras: `echo {Z..A}` -> `Z Y X ... A`.
        *   Anidación: `echo a{A{1,2},B{3,4}}b` -> `aA1b aA2b aB3b aB4b`.
        *   Muy útil para crear listas de archivos/directorios: `mkdir fotos-{2023..2024}-{01..12}`.
    *   **Expansión de Parámetros (Parameter Expansion):**
        *   Sustituye el nombre de una variable por su valor.
        *   Sintaxis: `$VARIABLE` o `${VARIABLE}` (las llaves son necesarias para desambiguar).
        *   Ej: `echo $USER`.
        *   Si la variable no existe o está vacía, se expande a una cadena vacía.
        *   Ver variables: `printenv | less` o `set | less`.
    *   **Sustitución de Comandos (Command Substitution):**
        *   Permite usar la salida (stdout) de un comando como parte de otro comando.
        *   Sintaxis moderna: `$(comando)`.
        *   Sintaxis antigua: `` `comando` `` (backticks).
        *   Ej: `echo "Hoy es $(date)"`.
        *   Ej: `ls -l $(which cp)`.
        *   Se puede usar con pipelines: `file $(ls -d /usr/bin/* | grep zip)`.
*   **Entrecomillado (Quoting):** Mecanismo para suprimir expansiones no deseadas.
    *   **Comillas Dobles (`"`):**
        *   Suprimen la mayoría de los caracteres especiales y expansiones (separación de palabras, pathname, tilde, llaves).
        *   **PERMITEN** expansión de parámetros (`$VAR`), aritmética (`$(( ))`) y sustitución de comandos (`$( )`).
        *   Preservan los espacios en blanco y saltos de línea dentro de la cadena.
        *   Esencial para nombres de archivo con espacios: `mv "archivo con espacios.txt" archivo_sin_espacios.txt`.
        *   Comparar `echo $(cal)` vs `echo "$(cal)"`.
    *   **Comillas Simples (`'`):**
        *   Suprimen **TODAS** las expansiones. Todo dentro de las comillas se trata literalmente.
        *   Ej: `echo 'Mi directorio es $HOME'` -> `Mi directorio es $HOME`.
    *   **Escape de Caracteres (`\`):**
        *   Suprime el significado especial del carácter *inmediatamente siguiente*.
        *   Útil dentro de comillas dobles para evitar una expansión específica: `echo "El precio es \$5.00"`.
        *   Útil para nombres de archivo con caracteres especiales: `mv archivo\&viejo archivo_nuevo`.
        *   Para incluir una barra invertida literal, usar `\\`.
    *   **Secuencias de Escape con Barra Invertida:**
        *   Notación para representar caracteres especiales (códigos de control ASCII).
        *   Usar con `echo -e` o dentro de `$''`.
        *   Comunes: `\a` (bell), `\b` (retroceso), `\n` (nueva línea), `\r` (retorno de carro), `\t` (tabulador).
        *   Ej: `echo -e "Linea 1\nLinea 2"` o `echo $'Linea 1\nLinea 2'`.

### Capítulo 8: TRUCOS AVANZADOS DE TECLADO

*   **Comandos Introducidos:**
    *   `clear`: Limpiar la pantalla de la terminal.
    *   `history`: Mostrar o manipular la lista del historial.
*   **Edición de Línea de Comandos (Readline):** Biblioteca usada por `bash` para editar la línea de comandos.
    *   **Movimiento del Cursor:**
        *   `CTRL-A`: Inicio de línea.
        *   `CTRL-E`: Fin de línea.
        *   `CTRL-F` / Flecha Der: Carácter adelante.
        *   `CTRL-B` / Flecha Izq: Carácter atrás.
        *   `ALT-F`: Palabra adelante.
        *   `ALT-B`: Palabra atrás.
        *   `CTRL-L`: Limpiar pantalla (como `clear`).
    *   **Modificación de Texto:**
        *   `CTRL-D`: Borrar carácter bajo el cursor.
        *   `CTRL-T`: Transponer (intercambiar) carácter bajo el cursor con el anterior.
        *   `ALT-T`: Transponer palabra actual con la anterior.
        *   `ALT-L`: Convertir a minúsculas desde el cursor hasta el fin de palabra.
        *   `ALT-U`: Convertir a mayúsculas desde el cursor hasta el fin de palabra.
    *   **Cortar y Pegar (Killing y Yanking):**
        *   `CTRL-K`: Cortar (kill) desde el cursor hasta el fin de línea.
        *   `CTRL-U`: Cortar (kill) desde el cursor hasta el inicio de línea.
        *   `ALT-D`: Cortar (kill) desde el cursor hasta el fin de palabra.
        *   `ALT-BACKSPACE`: Cortar (kill) desde el cursor hasta el inicio de palabra.
        *   `CTRL-Y`: Pegar (yank) lo último cortado.
    *   **Meta Key:** En teclados modernos, es la tecla `ALT`. Se puede simular presionando y soltando `ESC`.
*   **Completado (Completion):**
    *   Presionar `TAB`: Intenta completar el texto tecleado.
    *   Funciona para nombres de ruta (más común), variables (`$VAR<TAB>`), nombres de usuario (`~user<TAB>`), comandos (al inicio de línea), nombres de host (`@host<TAB>`).
    *   Si hay múltiples coincidencias, no completa o (en muchas configuraciones) muestra las opciones al presionar `TAB` de nuevo.
    *   `ALT-?`: Mostrar lista de posibles completados (a menudo igual que doble `TAB`).
    *   `ALT-*`: Insertar todos los posibles completados.
    *   **Completado Programable:** Característica avanzada que permite definir reglas de completado específicas para comandos y opciones (común en distros modernas).
*   **Uso del Historial (`history`):**
    *   `bash` guarda los comandos en `~/.bash_history`.
    *   `history`: Muestra la lista numerada de comandos recientes.
    *   `history | less`: Ver historial largo.
    *   `history | grep comando`: Buscar comandos específicos en el historial.
    *   **Expansión del Historial:**
        *   `!número`: Ejecuta el comando con ese número del historial. (Ej: `!88`).
        *   `!!`: Repite el último comando.
        *   `!cadena`: Repite el último comando que empieza con `cadena`. (¡Precaución!).
        *   `!?cadena`: Repite el último comando que contiene `cadena`. (¡Precaución!).
    *   **Búsqueda en el Historial:**
        *   `CTRL-R`: Búsqueda incremental inversa. Escribir texto para buscar hacia atrás. `ENTER` para ejecutar, `CTRL-J` para copiar a la línea actual, `CTRL-R` de nuevo para buscar la siguiente coincidencia anterior, `CTRL-G` o `CTRL-C` para cancelar.
        *   `CTRL-P` / Flecha Arriba: Comando anterior.
        *   `CTRL-N` / Flecha Abajo: Comando siguiente.
        *   `ALT-<`: Ir al inicio del historial.
        *   `ALT->`: Ir al final del historial (línea actual).
        *   `CTRL-O`: Ejecutar comando actual del historial y pasar al siguiente.
*   **Comando `script`:**
    *   `script [archivo]`: Graba toda la sesión de la terminal en un archivo (por defecto `typescript`).

### Capítulo 9: PERMISOS

*   **Multiusuario:** Linux permite que múltiples usuarios usen el sistema simultáneamente, protegiendo los archivos de cada uno.
*   **Comandos Introducidos:**
    *   `id`: Mostrar identidad del usuario (UID, GID, grupos).
    *   `chmod`: Cambiar modo (permisos) de un archivo.
    *   `umask`: Establecer permisos por defecto para archivos nuevos.
    *   `su`: Ejecutar una shell como otro usuario (substitute user).
    *   `sudo`: Ejecutar un comando como otro usuario (superuser do).
    *   `chown`: Cambiar propietario de un archivo.
    *   `chgrp`: Cambiar grupo propietario de un archivo.
    *   `passwd`: Cambiar contraseña de usuario.
*   **Propietarios, Grupos, Otros:**
    *   Cada archivo/directorio tiene un **propietario** (usuario) y un **grupo** propietario.
    *   Los permisos se definen para: **Propietario (u)**, **Grupo (g)**, **Otros (o, world)**.
    *   `id`: Muestra `uid` (User ID), `gid` (Group ID), `groups` (grupos a los que pertenece).
    *   Información de usuarios en `/etc/passwd`, grupos en `/etc/group`, contraseñas (hash) en `/etc/shadow`.
*   **Leer, Escribir, Ejecutar (rwx):**
    *   Permisos básicos aplicados a propietario, grupo y otros.
    *   Salida de `ls -l`: `drwxr-xr-x ...`
        *   Primer carácter: Tipo de archivo (`-` regular, `d` directorio, `l` enlace simbólico, `c` disp. carácter, `b` disp. bloque).
        *   Nueve caracteres siguientes: Modo (permisos `rwx` para u, g, o).
    *   **Significado de Permisos:**
        *   **Archivo:**
            *   `r`: Permite leer el contenido.
            *   `w`: Permite modificar o truncar. (Borrar/renombrar depende del directorio).
            *   `x`: Permite ejecutar (si es programa/script).
        *   **Directorio:**
            *   `r`: Permite listar el contenido (si `x` también está).
            *   `w`: Permite crear, borrar, renombrar archivos *dentro* del directorio (si `x` también está).
            *   `x`: Permite entrar al directorio (`cd`) y acceder a sus archivos/subdirectorios.
*   **Comando `chmod`:**
    *   Cambia los permisos. Solo el propietario o root pueden hacerlo.
    *   **Notación Octal:** Usa números de 3 dígitos (0-7) para u, g, o. Cada dígito representa `rwx`.
        *   `r`=4, `w`=2, `x`=1. Se suman.
        *   `7` = `rwx` (4+2+1)
        *   `6` = `rw-` (4+2+0)
        *   `5` = `r-x` (4+0+1)
        *   `4` = `r--` (4+0+0)
        *   `0` = `---` (0+0+0)
        *   Ej: `chmod 600 archivo` -> `-rw-------` (solo propietario tiene rw).
        *   Ej: `chmod 755 archivo` -> `-rwxr-xr-x` (propietario rwx, grupo y otros r-x).
    *   **Notación Simbólica:** `[ugoa...][+-=][rwx...]`
        *   Quién: `u` (usuario/owner), `g` (grupo), `o` (otros), `a` (todos - por defecto).
        *   Operación: `+` (añadir), `-` (quitar), `=` (establecer exactamente).
        *   Qué: `r`, `w`, `x`.
        *   Ej: `chmod u+x archivo` (añade permiso de ejecución al propietario).
        *   Ej: `chmod go=r archivo` (establece grupo y otros a solo lectura).
        *   Ej: `chmod a-w archivo` (quita permiso de escritura a todos).
        *   Se pueden combinar con comas: `chmod u+x,go=rx archivo`.
*   **Comando `umask`:**
    *   Establece la máscara de permisos por defecto para *nuevos* archivos y directorios.
    *   Se especifica en octal como los permisos que se *quitarán* de los permisos base (666 para archivos, 777 para directorios).
    *   `umask` (sin argumentos): Muestra la máscara actual (ej: `0002` o `0022`).
    *   `umask 0022`: Permisos por defecto serán 644 (`rw-r--r--`) para archivos y 755 (`rwxr-xr-x`) para directorios.
    *   `umask 0002`: Permisos por defecto serán 664 (`rw-rw-r--`) para archivos y 775 (`rwxrwxr-x`) para directorios (permite escribir al grupo).
*   **Permisos Especiales (Octal de 4 dígitos):**
    *   **setuid (4000):** Si está en un ejecutable, se ejecuta con los permisos del *propietario* del archivo (a menudo root), no del usuario que lo lanza. Peligroso, usar con moderación. `ls` muestra `s` en lugar de `x` del propietario (`-rwsr-xr-x`).
    *   **setgid (2000):** Similar a setuid pero para el grupo. Si está en un *directorio*, los nuevos archivos creados dentro heredan el grupo del directorio, no el grupo primario del creador. `ls` muestra `s` en lugar de `x` del grupo (`-rwxrwsr-x`).
    *   **Sticky Bit (1000):** En archivos, ignorado por Linux. En *directorios*, solo el propietario del archivo, el propietario del directorio o root pueden borrar/renombrar archivos dentro, aunque otros tengan permiso de escritura en el directorio. Útil para directorios compartidos como `/tmp`. `ls` muestra `t` en lugar de `x` de otros (`drwxrwxrwt`).
    *   `chmod` simbólico: `u+s` (setuid), `g+s` (setgid), `+t` (sticky).
*   **Cambiar Identidad:**
    *   `su [-l] [usuario]`: Cambia al `usuario` especificado (o root si se omite). `-l` (o `-`) inicia una *shell de login* (carga entorno, va al home). Requiere la contraseña del usuario destino. `exit` para volver.
    *   `su -c 'comando'`: Ejecuta un solo `comando` como otro usuario.
    *   `sudo comando`: Ejecuta `comando` como otro usuario (usualmente root). Configurado en `/etc/sudoers`. Requiere la contraseña del *propio* usuario. No inicia nueva shell por defecto (`-i` sí).
*   **Cambiar Propiedad:**
    *   `chown [propietario][:[grupo]] archivo...`: Cambia propietario y/o grupo. Requiere privilegios root.
        *   `chown bob archivo`: Cambia propietario a `bob`.
        *   `chown bob:users archivo`: Cambia propietario a `bob`, grupo a `users`.
        *   `chown :admins archivo`: Cambia solo el grupo a `admins`.
        *   `chown bob: archivo`: Cambia propietario a `bob`, grupo al grupo de login de `bob`.
    *   `chgrp grupo archivo...`: Cambia solo el grupo propietario.
*   **Comando `passwd`:**
    *   `passwd`: Cambia la contraseña del usuario actual (pide la vieja, luego la nueva dos veces).
    *   `sudo passwd usuario`: (Como root) Cambia la contraseña de otro usuario.

### Capítulo 10: PROCESOS

*   **Multitarea:** El SO crea la ilusión de hacer varias cosas a la vez cambiando rápidamente entre programas en ejecución.
*   **Proceso:** Una instancia de un programa en ejecución. El kernel gestiona los procesos.
*   **Comandos Introducidos:**
    *   `ps`: Reporta una instantánea de los procesos actuales.
    *   `top`: Muestra tareas del sistema de forma dinámica.
    *   `jobs`: Lista trabajos activos en la shell actual.
    *   `bg`: Pone un trabajo en segundo plano (background).
    *   `fg`: Pone un trabajo en primer plano (foreground).
    *   `kill`: Envía una señal a un proceso.
    *   `killall`: Mata procesos por nombre.
    *   `shutdown`: Apaga o reinicia el sistema.
*   **Funcionamiento de un Proceso:**
    *   Al arrancar, el kernel inicia `init` (PID 1).
    *   `init` ejecuta scripts de inicio (`/etc/init.d` o similar) que lanzan servicios del sistema.
    *   Muchos servicios son *demonios* (daemons): programas que corren en segundo plano sin interfaz.
    *   Un proceso puede lanzar otros (padre -> hijo).
    *   Cada proceso tiene un **PID** (Process ID) único.
    *   El kernel rastrea memoria, estado, propietario, etc. de cada proceso.
*   **Comando `ps`:**
    *   `ps` (sin opciones): Muestra procesos asociados a la terminal actual.
        *   Columnas: `PID`, `TTY` (terminal), `TIME` (tiempo CPU), `CMD` (comando).
    *   `ps x`: Muestra *todos* los procesos propiedad del usuario actual (incluso sin TTY). `?` en TTY significa sin terminal. Introduce columna `STAT`.
    *   `ps aux`: Muestra procesos de *todos* los usuarios (estilo BSD).
        *   Columnas adicionales: `USER`, `%CPU`, `%MEM`, `VSZ` (mem virtual), `RSS` (mem física/RAM), `START` (hora inicio).
    *   **Estados de Proceso (`STAT`):**
        *   `R`: Running (en ejecución o listo).
        *   `S`: Sleeping (esperando evento, ej: E/S, señal).
        *   `D`: Uninterruptible Sleep (esperando E/S disco, no puede ser matado fácilmente).
        *   `T`: Stopped (detenido por señal, ej: `CTRL-Z`).
        *   `Z`: Zombie/Defunct (terminado pero no limpiado por el padre).
        *   `+`: Proceso en foreground (en algunas versiones).
        *   `<`: Alta prioridad (menos "nice").
        *   `N`: Baja prioridad ("nice").
*   **Comando `top`:**
    *   Vista dinámica y en tiempo real de los procesos, ordenada por uso de CPU por defecto. Se actualiza cada pocos segundos.
    *   **Cabecera:**
        *   Hora, uptime, usuarios logueados, load average (carga media: 1, 5, 15 min).
        *   Tareas: total, running, sleeping, stopped, zombie.
        *   CPU: %us (usuario), %sy (sistema/kernel), %ni (nice), %id (idle/inactivo), %wa (esperando E/S).
        *   Memoria (Mem): total, usada, libre, buffers/cache.
        *   Swap: total, usada, libre.
    *   **Tabla de Procesos:** Similar a `ps aux`.
    *   **Comandos de `top`:** `h` (ayuda), `q` (salir).
*   **Control de Procesos:**
    *   **Ejecutar en Segundo Plano (Background):** Añadir `&` al final del comando.
        *   Ej: `xlogo &`. La shell devuelve el prompt y muestra `[job_num] PID`.
    *   **Comando `jobs`:** Muestra los trabajos (procesos lanzados desde la shell actual) que están en segundo plano o detenidos.
    *   **Traer a Primer Plano (Foreground):** `fg %jobspec`
        *   `%jobspec` es `%` seguido del número de trabajo (ej: `%1`). Si solo hay un trabajo, `%jobspec` es opcional.
        *   El proceso vuelve a primer plano y puede recibir entrada del teclado.
    *   **Enviar a Segundo Plano (un trabajo detenido):** `bg %jobspec`
        *   Reanuda la ejecución de un trabajo detenido, pero en segundo plano.
    *   **Detener (Pausar) un Proceso en Primer Plano:** `CTRL-Z`.
        *   Envía señal `TSTP`. El proceso se detiene, `jobs` lo muestra como "Stopped".
        *   Se puede reanudar con `fg` (a primer plano) o `bg` (a segundo plano).
    *   **Interrumpir un Proceso en Primer Plano:** `CTRL-C`.
        *   Envía señal `INT`. Pide al programa que termine amablemente. Muchos programas lo hacen.
*   **Señales (Signals):** Mecanismo de comunicación del SO con los procesos.
    *   **Comando `kill`:** `kill [-signal] PID...` (o `%jobspec`). Envía una señal a un proceso.
        *   Por defecto envía `TERM` (15).
        *   Señales comunes (se pueden especificar por número o nombre, ej: `-9` o `-KILL`):
            *   `1` (`HUP`): Hangup. A menudo usado para re-leer configuración por demonios.
            *   `2` (`INT`): Interrupt (como `CTRL-C`).
            *   `9` (`KILL`): Mata el proceso incondicionalmente (el kernel lo termina, no el proceso). **Último recurso**, no permite limpieza ni guardar.
            *   `15` (`TERM`): Terminate. Petición amable para terminar.
            *   `18` (`CONT`): Continue. Reanuda un proceso detenido (`STOP`/`TSTP`). Usado por `fg`/`bg`.
            *   `19` (`STOP`): Detiene el proceso incondicionalmente (no puede ser ignorado).
            *   `20` (`TSTP`): Terminal Stop (como `CTRL-Z`). Puede ser ignorado.
    *   **Comando `killall`:** `killall [-u usuario] [-señal] nombre_programa...`
        *   Envía señal a *todos* los procesos con ese nombre de programa.
        *   Útil para matar múltiples instancias.
*   **Apagar el Sistema:**
    *   Requiere privilegios root.
    *   `shutdown [opciones] tiempo [mensaje]`: Forma preferida.
        *   `-h`: Halt (detener).
        *   `-r`: Reboot (reiniciar).
        *   `-P`: Poweroff (apagar completamente, si el hardware lo soporta).
        *   `tiempo`: `now` (ahora), `+minutos` (en X minutos), `hh:mm` (a hora específica).
        *   Ej: `sudo shutdown -h now`.
        *   Ej: `sudo shutdown -r +10 "Reinicio en 10 min"`.
    *   `halt`, `poweroff`, `reboot`: Comandos más directos, a menudo enlaces a `shutdown`.
*   **Otros Comandos Relacionados con Procesos:**
    *   `pstree`: Muestra procesos en formato de árbol (relaciones padre-hijo).
    *   `vmstat`: Muestra estadísticas de memoria virtual, E/S, etc. `vmstat 5` actualiza cada 5 seg.
    *   `xload`: Programa gráfico que muestra la carga del sistema.
    *   `tload`: Similar a `xload` pero en la terminal.

## PARTE II: CONFIGURACIÓN Y EL ENTORNO

### Capítulo 11: EL ENTORNO

*   **Entorno (Environment):** Colección de datos que la shell mantiene durante una sesión. Los programas lo usan para obtener información sobre la configuración del sistema.
*   **Comandos Introducidos:**
    *   `printenv`: Imprime todo o parte del entorno (variables de entorno).
    *   `set`: Establece opciones de la shell, muestra variables de shell y entorno, y funciones.
    *   `export`: Exporta variables al entorno para que los programas hijos las hereden.
    *   `alias`: Crear alias (repaso).
*   **Contenido del Entorno:**
    *   **Variables de Entorno:** Datos disponibles para la shell y programas hijos.
    *   **Variables de Shell:** Datos usados internamente por la shell.
    *   **Alias:** Atajos para comandos.
    *   **Funciones de Shell:** Mini-scripts definidos en la shell.
*   **Examinar el Entorno:**
    *   `printenv`: Muestra variables de entorno.
    *   `printenv NOMBRE_VAR`: Muestra el valor de una variable específica.
    *   `set`: Muestra variables de shell y entorno, y funciones (ordenado).
    *   `echo $NOMBRE_VAR`: Muestra el valor de una variable (requiere `$` para expansión).
    *   `alias`: Muestra los alias definidos.
*   **Variables Interesantes Comunes:**
    *   `DISPLAY`: Nombre de la pantalla para GUI (ej: `:0`).
    *   `EDITOR`: Editor de texto por defecto.
    *   `SHELL`: Ruta a la shell actual.
    *   `HOME`: Ruta al directorio home del usuario.
    *   `LANG`: Configuración regional (idioma, conjunto de caracteres).
    *   `OLDPWD`: Directorio de trabajo anterior.
    *   `PAGER`: Programa paginador por defecto (ej: `less`).
    *   `PATH`: Lista de directorios separados por `:` donde la shell busca ejecutables.
    *   `PS1`: Define el prompt primario de la shell.
    *   `PWD`: Directorio de trabajo actual.
    *   `TERM`: Tipo de terminal emulado.
    *   `USER`: Nombre de usuario actual.
*   **Establecimiento del Entorno:**
    *   Al iniciar sesión, `bash` lee *archivos de inicio* (startup files).
    *   **Shell de Login:** Sesión donde se pide usuario/contraseña (ej: consola virtual, ssh inicial).
        *   Lee `/etc/profile` (global).
        *   Luego lee *uno* de: `~/.bash_profile`, `~/.bash_login`, `~/.profile` (el primero que encuentre).
    *   **Shell No-Login:** Sesión iniciada sin login explícito (ej: abrir terminal en GUI).
        *   Lee `/etc/bash.bashrc` (global, si existe).
        *   Lee `~/.bashrc` (personal).
        *   Hereda el entorno de la shell padre (usualmente una shell de login).
    *   `~/.bashrc` es el archivo más importante para personalizaciones del usuario, ya que suele ser leído directa o indirectamente por ambos tipos de shells.
*   **Contenido de un Archivo de Inicio (Ej: `.bash_profile`):**
    *   Comentarios (`#`).
    *   Comandos `if` para comprobar y leer otros archivos (ej: leer `.bashrc` desde `.bash_profile`).
    *   Definición/modificación de variables (ej: `PATH=$PATH:$HOME/bin`).
    *   Comando `export`: Hace que una variable esté disponible para programas hijos. `export PATH`.
*   **Modificar el Entorno:**
    *   Editar los archivos de inicio personales (`~/.bash_profile`, `~/.profile`, `~/.bashrc`).
    *   **Regla general:** Cambios en `PATH` y variables de entorno en `.bash_profile` (o equivalente); alias y funciones en `.bashrc`.
    *   Usar un **editor de texto** (`nano`, `vim`, `gedit`, `kate`, etc.).
    *   **¡Hacer copia de seguridad antes de editar!** `cp .bashrc .bashrc.bak`.
    *   **Editor `nano`:** Simple, comandos en la parte inferior (`^X` salir, `^O` guardar).
    *   **Aplicar Cambios:** Los cambios surten efecto al iniciar una *nueva* sesión de shell, o forzando la re-lectura del archivo con `source ~/.bashrc` (o `. ~/.bashrc`).
*   **Comentarios:** Usar `#` para añadir explicaciones a los archivos de configuración. Ayuda al mantenimiento.

### Capítulo 12: UNA INTRODUCCIÓN AMABLE A VI

*   **`vi` (VIsual editor):** Editor de texto clásico de Unix, potente pero con curva de aprendizaje.
*   **`vim` (Vi IMproved):** Reemplazo mejorado de `vi`, común en Linux. A menudo `vi` es un enlace a `vim`.
*   **¿Por qué aprender `vi`?**
    *   Casi siempre disponible (estándar POSIX), incluso en sistemas mínimos o remotos.
    *   Ligero y rápido.
    *   Eficiente para mecanógrafos (no requiere ratón).
*   **Modos de Edición:** `vi` es un editor *modal*.
    *   **Modo Comando (o Modo Normal):** Estado inicial. Las teclas son comandos (mover cursor, borrar, copiar, etc.). **Presionar `ESC`** para volver a este modo.
    *   **Modo Inserción:** Para escribir texto. Se entra con comandos como `i`, `a`, `o`. Se sale con `ESC`.
    *   **Modo Comando `ex` (o Línea de Comandos):** Se entra desde modo comando presionando `:`. Permite comandos más complejos (guardar, salir, buscar/reemplazar).
*   **Iniciar y Salir:**
    *   `vi [archivo]`: Abre `vi`. Si `archivo` no existe, crea uno nuevo.
    *   Comandos `ex` para salir (desde modo comando):
        *   `:q`: Salir (Quit). Falla si hay cambios sin guardar.
        *   `:q!`: Salir forzadamente, descartando cambios.
        *   `:w`: Guardar (Write) cambios.
        *   `:wq`: Guardar y salir.
        *   `ZZ` (en modo comando, sin `:`): Equivalente a `:wq`.
*   **Movimiento del Cursor (Modo Comando):**
    *   `h`, `j`, `k`, `l` (o flechas): Izquierda, abajo, arriba, derecha.
    *   `0`: Inicio de línea (columna 0).
    *   `^`: Primer carácter no blanco de la línea.
    *   `$`: Fin de línea.
    *   `w`: Inicio de siguiente palabra/puntuación.
    *   `W`: Inicio de siguiente palabra (ignora puntuación).
    *   `b`: Inicio de palabra/puntuación anterior.
    *   `B`: Inicio de palabra anterior (ignora puntuación).
    *   `CTRL-F` / `PageDown`: Página abajo.
    *   `CTRL-B` / `PageUp`: Página arriba.
    *   `nG`: Ir a la línea `n` (ej: `1G` = primera línea).
    *   `G`: Ir a la última línea.
    *   **Repetición:** `número` + comando de movimiento (ej: `5j` baja 5 líneas).
*   **Edición Básica (Modo Comando):**
    *   **Insertar Texto:**
        *   `i`: Insertar *antes* del cursor.
        *   `a`: Insertar (append) *después* del cursor.
        *   `o`: Abrir una nueva línea *debajo* de la actual e insertar.
        *   `O`: Abrir una nueva línea *encima* de la actual e insertar.
        *   `A`: Insertar (append) al *final* de la línea actual.
    *   **Borrar Texto (también corta al portapapeles):**
        *   `x`: Borrar carácter bajo el cursor. `nx` borra `n` caracteres.
        *   `d` + movimiento: Borrar texto cubierto por el movimiento.
            *   `dd`: Borrar línea actual. `ndd` borra `n` líneas.
            *   `dw`: Borrar desde cursor hasta inicio de siguiente palabra.
            *   `d$`: Borrar desde cursor hasta fin de línea.
            *   `d0`: Borrar desde cursor hasta inicio de línea.
            *   `dG`: Borrar desde línea actual hasta fin de archivo.
    *   **Copiar Texto (Yank):**
        *   `y` + movimiento: Copia (yank) texto cubierto por el movimiento.
            *   `yy`: Copiar línea actual. `nyy` copia `n` líneas.
            *   `yw`: Copiar desde cursor hasta inicio de siguiente palabra.
            *   `y$`: Copiar desde cursor hasta fin de línea.
    *   **Pegar Texto (Paste):**
        *   `p`: Pegar *después* del cursor (si se copió línea, pega en línea siguiente).
        *   `P`: Pegar *antes* del cursor (si se copió línea, pega en línea anterior).
    *   **Deshacer:**
        *   `u`: Deshacer último cambio (`vim` permite múltiples `u`).
    *   **Unir Líneas:**
        *   `J`: Une la línea actual con la siguiente.
*   **Búsqueda y Reemplazo:**
    *   **Buscar dentro de línea:** `f` + carácter (busca siguiente ocurrencia), `;` (repetir búsqueda).
    *   **Buscar en todo el archivo:** `/` + patrón + `ENTER` (busca hacia adelante), `n` (buscar siguiente). Usa expresiones regulares básicas.
    *   **Reemplazo Global (`ex`):** `:range s/patrón/reemplazo/flags`
        *   `range`: Rango de líneas. `%` = todo el archivo. `n,m` = línea n a m. Omitido = línea actual.
        *   `s`: Comando de sustitución.
        *   `patrón`: Expresión regular (básica) a buscar.
        *   `reemplazo`: Texto con el que sustituir.
        *   `flags`: Modificadores. Comunes:
            *   `g`: Global (reemplaza *todas* las ocurrencias en la línea, no solo la primera).
            *   `c`: Confirmar cada reemplazo (y/n/a/q/l...).
        *   Ej: `:%s/viejo/nuevo/g` (reemplaza todas las 'viejo' por 'nuevo' en el archivo).
*   **Editar Múltiples Archivos:**
    *   `vi archivo1 archivo2 ...`: Abre varios archivos.
    *   Comandos `ex`:
        *   `:n` o `:bn` (buffer next): Ir al siguiente archivo/buffer.
        *   `:bp` (buffer previous): Ir al archivo/buffer anterior.
        *   `:buffers`: Listar los archivos abiertos (buffers).
        *   `:buffer n`: Ir al buffer número `n`.
        *   `:e archivo`: Abrir `archivo` adicional para editar.
    *   Copiar/pegar entre archivos: Usar `y` en un archivo, cambiar de buffer (`:bn`, `:bp`, `:buffer n`), usar `p` o `P` en el otro.
    *   Insertar archivo completo: `:r nombre_archivo` (inserta contenido de `nombre_archivo` debajo de la línea actual).

### Capítulo 13: PERSONALIZANDO EL PROMPT

*   **Prompt de la Shell:** Indicador que muestra la shell (ej: `[usuario@máquina directorio]$`).
*   **Variable `PS1`:** Variable de entorno que define el contenido del prompt primario.
*   **Ver el Prompt Actual:** `echo $PS1`.
*   **Códigos de Escape Especiales (Backslash-escaped):** Usados en `PS1` para mostrar información dinámica.
    *   `\u`: Nombre de usuario.
    *   `\h`: Nombre de host (hasta el primer punto).
    *   `\H`: Nombre de host completo.
    *   `\w`: Directorio de trabajo actual (ruta completa, `~` para home).
    *   `\W`: Nombre base del directorio de trabajo actual (último componente).
    *   `\$`: Muestra `#` si es root, `$` si es usuario normal.
    *   `\d`: Fecha (ej: "Mon May 26").
    *   `\t`: Hora (HH:MM:SS, 24h).
    *   `\T`: Hora (HH:MM:SS, 12h).
    *   `\@`: Hora (HH:MM AM/PM, 12h).
    *   `\A`: Hora (HH:MM, 24h).
    *   `\n`: Nueva línea.
    *   `\s`: Nombre de la shell.
    *   `\v`: Versión de la shell (ej: 4.4).
    *   `\V`: Versión y release de la shell.
    *   `\!`: Número de comando en el historial.
    *   `\#`: Número de comando en la sesión actual.
    *   `\a`: Bell ASCII (beep).
    *   `\[ ... \]`: Encapsula secuencias de caracteres **no imprimibles** (como códigos de color o movimiento de cursor) para que `bash` calcule correctamente la longitud del prompt visible.
*   **Modificar el Prompt:**
    *   Asignar un nuevo valor a `PS1`. Ej: `PS1="\u@\h:\w\$ "`
    *   Guardar el prompt original: `ps1_old="$PS1"`. Restaurar: `PS1="$ps1_old"`.
    *   Añadir un espacio al final del prompt para separar del comando.
*   **Añadir Color:**
    *   Usar **códigos de escape ANSI** dentro de `\[ ... \]`.
    *   Formato: `\033[Attribute;ForegroundColor;BackgroundColor`m (la `m` es importante). `\033` es el código octal para ESC.
    *   **Atributos:** `0` (normal), `1` (negrita/brillante).
    *   **Colores Texto (Foreground):** 30 (negro), 31 (rojo), 32 (verde), 33 (marrón/amarillo), 34 (azul), 35 (púrpura), 36 (cyan), 37 (gris claro). Añadir `1;` para versión brillante (ej: `1;31` rojo brillante).
    *   **Colores Fondo (Background):** 40-47 (mismos colores que texto).
    *   **Resetear color/atributos:** `\033[0m`. **Importante** añadirlo al final del prompt para que el texto tecleado no herede el color.
    *   Ejemplo (prompt verde brillante): `PS1='\[\033[1;32m\]\u@\h:\w\$ \[\033[0m\]'`
*   **Mover el Cursor:**
    *   Usar códigos de escape ANSI dentro de `\[ ... \]`.
    *   `\033[l;cH`: Mover a línea `l`, columna `c`. (ej: `\033[1;1H` -> esquina superior izquierda).
    *   `\033[nA`: Subir `n` líneas.
    *   `\033[nB`: Bajar `n` líneas.
    *   `\033[nC`: Mover `n` caracteres a la derecha.
    *   `\033[nD`: Mover `n` caracteres a la izquierda.
    *   `\033[s`: Guardar posición actual del cursor.
    *   `\033[u`: Restaurar posición guardada del cursor.
    *   `\033[2J`: Limpiar pantalla y mover a esquina superior izquierda.
    *   `\033[K`: Limpiar desde cursor hasta fin de línea.
    *   Útil para crear prompts complejos con información en diferentes partes de la pantalla (ej: un reloj en la esquina).
*   **Guardar el Prompt:**
    *   Para hacerlo permanente, añadir la asignación de `PS1` (y `export PS1`) al archivo `~/.bashrc`.

## PARTE III: TAREAS COMUNES Y HERRAMIENTAS ESENCIALES

### Capítulo 14: GESTIÓN DE PAQUETES

*   **Gestión de Paquetes:** Método para instalar, mantener y eliminar software en el sistema. Crucial en Linux debido a la naturaleza dinámica del software.
*   **Sistemas de Paquetes:**
    *   La mayoría de las distribuciones usan uno de dos sistemas principales:
        *   **Debian (`.deb`):** Usado por Debian, Ubuntu, Mint, etc. Herramientas: `dpkg` (bajo nivel), `apt-get`/`apt` (alto nivel).
        *   **Red Hat (`.rpm`):** Usado por Fedora, CentOS, RHEL, openSUSE. Herramientas: `rpm` (bajo nivel), `yum`/`dnf` (alto nivel).
    *   Paquetes de una familia no son directamente compatibles con la otra.
*   **Cómo Funciona:**
    *   **Archivo de Paquete:** Colección comprimida de archivos del software, más metadatos (descripción, versión, dependencias) y scripts de pre/post instalación. Creado por un *mantenedor*.
    *   **Repositorio:** Servidor centralizado que almacena miles de paquetes construidos para una distribución específica. Puede haber repositorios de prueba, desarrollo, de terceros (para software no incluido por razones legales, ej: codecs).
    *   **Dependencias:** Un paquete requiere otros componentes (ej: bibliotecas compartidas) para funcionar. El sistema de gestión resuelve e instala las dependencias automáticamente (con herramientas de alto nivel).
    *   **Herramientas de Bajo Nivel:** Instalan/eliminan archivos de paquete individuales (ej: `dpkg`, `rpm`). No resuelven dependencias.
    *   **Herramientas de Alto Nivel:** Buscan en repositorios, resuelven dependencias, descargan e instalan paquetes (ej: `apt-get`, `yum`, `apt`).
*   **Tareas Comunes (Ejemplos):**
    *   **Buscar Paquete (en repositorio):**
        *   Debian: `apt-cache search término` (después de `apt-get update`)
        *   Red Hat: `yum search término`
    *   **Instalar Paquete (desde repositorio):**
        *   Debian: `apt-get update; apt-get install nombre_paquete` (o `apt install nombre_paquete`)
        *   Red Hat: `yum install nombre_paquete` (o `dnf install nombre_paquete`)
    *   **Instalar Paquete (desde archivo local):**
        *   Debian: `sudo dpkg -i archivo_paquete.deb` (**no** resuelve dependencias)
        *   Red Hat: `sudo rpm -i archivo_paquete.rpm` (**no** resuelve dependencias)
    *   **Eliminar Paquete:**
        *   Debian: `sudo apt-get remove nombre_paquete` (o `apt remove nombre_paquete`)
        *   Red Hat: `sudo yum erase nombre_paquete` (o `dnf remove nombre_paquete`)
    *   **Actualizar Todos los Paquetes (desde repositorio):**
        *   Debian: `sudo apt-get update; sudo apt-get upgrade` (o `apt update; apt upgrade`)
        *   Red Hat: `sudo yum update` (o `dnf upgrade`)
    *   **Actualizar Paquete (desde archivo local, si ya está instalado):**
        *   Debian: `sudo dpkg -i archivo_paquete_nuevo.deb`
        *   Red Hat: `sudo rpm -U archivo_paquete_nuevo.rpm` (`-U` = Upgrade)
    *   **Listar Paquetes Instalados:**
        *   Debian: `dpkg -l`
        *   Red Hat: `rpm -qa` (`q`=query, `a`=all)
    *   **Verificar si un Paquete está Instalado:**
        *   Debian: `dpkg -s nombre_paquete`
        *   Red Hat: `rpm -q nombre_paquete`
    *   **Mostrar Información de Paquete (instalado o en repo):**
        *   Debian: `apt-cache show nombre_paquete`
        *   Red Hat: `yum info nombre_paquete` (o `dnf info nombre_paquete`)
    *   **Averiguar qué Paquete Instaló un Archivo:**
        *   Debian: `dpkg -S /ruta/al/archivo`
        *   Red Hat: `rpm -qf /ruta/al/archivo`

### Capítulo 15: MEDIOS DE ALMACENAMIENTO

*   Gestión de dispositivos de almacenamiento (discos duros, USB, CD/DVD).
*   **Comandos Introducidos:**
    *   `mount`: Montar un sistema de archivos.
    *   `umount`: Desmontar un sistema de archivos.
    *   `fsck`: Comprobar y reparar un sistema de archivos.
    *   `fdisk`: Manipular tabla de particiones de disco (para MBR).
    *   `mkfs`: Crear (formatear) un sistema de archivos.
    *   `dd`: Convertir y copiar un archivo (copia a bajo nivel, bloque a bloque).
    *   `genisoimage` (o `mkisofs`): Crear un archivo de imagen ISO 9660 (CD/DVD).
    *   `wodim` (o `cdrecord`): Escribir datos en medios ópticos (CD/DVD).
    *   `md5sum`: Calcular checksum MD5.
*   **Montar y Desmontar:**
    *   **Montar:** Proceso de adjuntar un dispositivo al árbol del sistema de archivos para acceder a él.
    *   `/etc/fstab` (file system table): Archivo que lista los dispositivos a montar al arrancar.
        *   Campos: Dispositivo (ej: `/dev/sda1`, `LABEL=xxx`, `UUID=yyy`), Punto de montaje (directorio), Tipo de sistema de archivos (`ext4`, `vfat`, `ntfs`, `iso9660`), Opciones (`defaults`, `ro`, `rw`), Freq (para dump), Orden (para fsck).
    *   **Comando `mount`:**
        *   `mount` (sin argumentos): Muestra los sistemas de archivos actualmente montados.
        *   `sudo mount -t tipo /dev/dispositivo /punto/de/montaje`: Monta manualmente un dispositivo. `-t tipo` puede ser opcional si `mount` lo detecta.
    *   **Comando `umount`:** (¡Sin 'n'!)
        *   `sudo umount /dev/dispositivo` o `sudo umount /punto/de/montaje`: Desmonta un dispositivo.
        *   **Importante:** Desmontar es crucial antes de extraer un medio extraíble para asegurar que todos los datos en *buffer* (caché de escritura) se escriban físicamente. No desmontar puede llevar a **corrupción de datos**.
        *   No se puede desmontar si el dispositivo está en uso (ej: si tu directorio actual está en él).
*   **Nombres de Dispositivos (`/dev`):**
    *   `/dev/sda`, `/dev/sdb`, ...: Discos SCSI, SATA, USB (tratados como SCSI). `a`=primer disco, `b`=segundo, etc.
    *   `/dev/sda1`, `/dev/sda2`, ...: Particiones en el primer disco.
    *   `/dev/hda`, `/dev/hdb`, ...: Discos IDE/PATA antiguos.
    *   `/dev/sr0`, `/dev/sr1`, ...: Unidades ópticas (CD/DVD).
    *   `/dev/fd0`, ...: Disqueteras.
    *   Enlaces simbólicos convenientes: `/dev/cdrom`, `/dev/dvd`.
    *   **Identificar dispositivos extraíbles:** Mirar salida de `dmesg` o `tail -f /var/log/syslog` (o `messages`) al conectar el dispositivo.
*   **Crear Nuevos Sistemas de Archivos:**
    *   **Paso 1 (Opcional): Particionar con `fdisk` (para MBR) o `gdisk` (para GPT).**
        *   `sudo fdisk /dev/dispositivo_entero` (ej: `/dev/sdb`, **no** `/dev/sdb1`).
        *   Comandos comunes de `fdisk`: `m` (menú), `p` (print table), `n` (new partition), `d` (delete), `t` (change type, `L` para listar tipos, `83`=Linux, `b`=W95 FAT32), `w` (write changes & exit), `q` (quit without saving).
        *   **¡¡PELIGRO!!:** `fdisk` manipula el disco a bajo nivel. Un error puede borrar datos. Asegúrate del dispositivo correcto.
    *   **Paso 2: Formatear (Crear sistema de archivos) con `mkfs`.**
        *   `sudo mkfs -t tipo /dev/particion` (ej: `/dev/sdb1`).
        *   `tipo`: `ext4` (Linux nativo), `vfat` (FAT32), `ntfs`, etc.
        *   Formatear **borra** todo el contenido de la partición.
*   **Comprobar y Reparar Sistemas de Archivos (`fsck`):**
    *   `sudo fsck /dev/particion` (La partición debe estar **desmontada**).
    *   Comprueba la integridad y puede reparar errores (con éxito variable).
    *   Se ejecuta automáticamente al arranque en particiones marcadas en `/etc/fstab`.
*   **Comando `dd`:**
    *   Copia datos bloque a bloque. Muy potente y **peligroso** si se usa mal ("destroy disk").
    *   `dd if=archivo_entrada of=archivo_salida [bs=tamaño_bloque] [count=num_bloques]`
        *   `if`: Input file (puede ser un dispositivo).
        *   `of`: Output file (puede ser un dispositivo).
        *   `bs`: Block size (tamaño de bloque a leer/escribir).
        *   `count`: Número de bloques a copiar.
    *   **Ejemplos:**
        *   Clonar un pendrive a otro: `sudo dd if=/dev/sdb of=/dev/sdc bs=4M`.
        *   Crear imagen de un pendrive: `sudo dd if=/dev/sdb of=imagen_pendrive.img bs=4M`.
        *   Crear imagen ISO de un CD/DVD: `dd if=/dev/cdrom of=imagen.iso`.
*   **Crear y Grabar Imágenes de CD/DVD:**
    *   **Crear Imagen ISO desde Archivos:** `genisoimage -o imagen.iso -R -J /directorio/con/archivos` (`-R`: Rock Ridge para Linux, `-J`: Joliet para Windows).
    *   **Montar Imagen ISO (sin grabar):** `sudo mount -t iso9660 -o loop imagen.iso /punto/montaje`.
    *   **Borrar CD/DVD RW:** `wodim dev=/dev/cdrw blank=fast` (borrado rápido).
    *   **Grabar Imagen ISO:** `wodim dev=/dev/cdrw imagen.iso`. `-v` (verbose), `-dao` (disc-at-once).
*   **Verificar Integridad (Checksum):**
    *   `md5sum archivo`: Calcula el hash MD5.
    *   Comparar el hash calculado de un archivo descargado (ej: ISO) con el proporcionado por el publicador para verificar que no haya corrupción.
    *   Se puede usar `md5sum /dev/cdrom` para verificar un CD/DVD grabado (comparar con `md5sum imagen.iso`).

### Capítulo 16: REDES

*   Exploración de herramientas de red básicas.
*   **Comandos Introducidos:**
    *   `ping`: Enviar paquetes ICMP ECHO_REQUEST para verificar conectividad.
    *   `traceroute`: Mostrar la ruta (saltos/routers) que toman los paquetes.
    *   `ip`: Herramienta moderna para mostrar/manipular rutas, dispositivos, etc. (reemplaza `ifconfig`).
    *   `netstat`: Mostrar conexiones de red, tablas de ruta, estadísticas (en parte reemplazado por `ip` y `ss`).
    *   `ftp`: Programa de transferencia de archivos por Internet (protocolo FTP).
    *   `wget`: Descargador de red no interactivo (HTTP, FTP).
    *   `ssh`: Cliente OpenSSH (programa de login remoto seguro).
    *   `scp`: Copia segura de archivos (usa SSH).
    *   `sftp`: Cliente FTP seguro (usa SSH).
*   **Conceptos de Red:** IP Address, Hostname, Domain Name, URI.
*   **Examinar y Monitorizar:**
    *   `ping hostname_o_ip`: Envía pings hasta ser interrumpido (`CTRL-C`). Muestra tiempo de respuesta y pérdida de paquetes. Útil para diagnóstico básico. (Puede ser bloqueado por firewalls).
    *   `traceroute hostname_o_ip`: Muestra los routers intermedios hasta el destino. Muestra latencia a cada salto. `* * *` indica que un router no responde. Opciones `-T` (TCP) o `-I` (ICMP) pueden ayudar a pasar firewalls. (`tracepath` es similar).
    *   `ip addr` (o `ip a`): Muestra información de las interfaces de red (IP, MAC, estado). Buscar `UP` y una IP válida. `lo` es la interfaz loopback. `eth0`, `wlan0`, etc., son interfaces físicas.
    *   `netstat -ie`: Similar a `ip addr`, muestra interfaces y estadísticas.
    *   `netstat -r`: Muestra la tabla de enrutamiento del kernel. `ip route show` hace lo mismo.
*   **Transferencia de Archivos:**
    *   **`ftp hostname_o_ip`:** Cliente interactivo FTP.
        *   A menudo se usa con servidores *anónimos* (usuario `anonymous`, cualquier email como contraseña).
        *   **Inseguro:** Envía usuario/contraseña en texto plano. Evitar para cuentas reales.
        *   Comandos comunes dentro de `ftp`: `cd`, `ls`, `lcd` (local cd), `get archivo`, `put archivo`, `mget` (multiple get), `mput`, `binary` (modo binario, para no-texto), `ascii`, `help`, `bye`/`quit`.
    *   **`lftp`:** Cliente FTP mejorado (más características, soporta otros protocolos).
    *   **`wget URL`:** Descarga archivos de forma no interactiva desde HTTP o FTP.
        *   Muy útil para scripting.
        *   Muchas opciones: recursivo (`-r`), seguir enlaces, continuar descargas parciales (`-c`), etc.
*   **Comunicación Segura con Hosts Remotos (SSH):**
    *   **SSH (Secure Shell):** Protocolo para comunicación remota segura. Autentica el host remoto y cifra toda la comunicación. Usa el puerto TCP 22 por defecto.
    *   **Componentes:** Servidor SSH (en host remoto, `sshd`) y Cliente SSH (en host local, `ssh`).
    *   **Cliente `ssh`:** `ssh [usuario@]hostname_o_ip`
        *   La primera vez que conecta a un host, pregunta si confías en su clave (fingerprint). La guarda en `~/.ssh/known_hosts`.
        *   Si la clave del host cambia (ej: reinstalación), `ssh` dará una advertencia ¡GRANDE Y AMENAZANTE! (posible ataque "man-in-the-middle"). Verificar con el admin antes de borrar la clave vieja de `known_hosts`.
        *   Se puede ejecutar un solo comando remoto: `ssh host 'comando'`. La salida aparece localmente.
    *   **Túneles SSH:** SSH crea un túnel cifrado. Se puede usar para redirigir otro tráfico.
        *   **X11 Forwarding:** `ssh -X host` (o `-Y` si `-X` falla). Permite ejecutar aplicaciones gráficas (X client) en el host remoto y verlas en la pantalla local (X server). Ej: `ssh -X remoto` y luego `xclock &`.
    *   **Copia Segura de Archivos:**
        *   **`scp` (Secure Copy):** `scp [usuario@]host:ruta_remota ruta_local` o `scp ruta_local [usuario@]host:ruta_remota`. Sintaxis similar a `cp`. Usa SSH.
        *   **`sftp` (Secure FTP):** Cliente interactivo similar a `ftp` pero usa SSH. No requiere servidor FTP en el remoto, solo `sshd`. Comandos similares a `ftp`.

### Capítulo 17: BUSCANDO ARCHIVOS

*   **Comandos Introducidos:**
    *   `locate`: Encuentra archivos por nombre usando una base de datos pre-construida.
    *   `find`: Busca archivos en una jerarquía de directorios basada en criterios.
    *   `xargs`: Construye y ejecuta líneas de comando desde la entrada estándar.
    *   `touch`: Cambia fechas de archivos (o crea archivos vacíos si no existen).
    *   `stat`: Muestra estado detallado de un archivo o sistema de archivos.
*   **Comando `locate`:**
    *   `locate patrón`: Búsqueda rápida de nombres de archivo/ruta que contengan `patrón`.
    *   Usa una base de datos (ej: `/var/lib/mlocate/mlocate.db`).
    *   La base de datos se actualiza periódicamente (usualmente diaria) por el comando `updatedb` (ejecutado por `cron`).
    *   No encontrará archivos muy recientes creados después de la última actualización.
    *   Se puede forzar la actualización con `sudo updatedb`.
    *   Puede combinarse con `grep` para refinar: `locate zip | grep bin`.
    *   Variantes comunes: `slocate`, `mlocate`. Opciones pueden variar (`--regexp`, `--regex`).
*   **Comando `find`:**
    *   `find [ruta...] [expresión]`
    *   Busca recursivamente en las `ruta`s especificadas. Si no se da ruta, busca en directorio actual (`.`).
    *   `expresión` consiste en *opciones*, *tests* (pruebas) y *acciones*.
    *   **Tests Comunes:**
        *   `-type t`: Busca por tipo de archivo (`f`=regular, `d`=directorio, `l`=enlace simbólico). Ej: `find . -type d`.
        *   `-name patrón`: Busca por nombre usando comodines (patrón debe ir entre comillas). Ej: `find . -name "*.txt"`.
        *   `-iname patrón`: Igual que `-name` pero ignora mayúsculas/minúsculas.
        *   `-path patrón`: Busca en la ruta completa (incluyendo directorios).
        *   `-size n[cwbkMG]`: Busca por tamaño. `n`=exacto, `+n`=mayor que, `-n`=menor que. Unidades: `c`(bytes), `k`(KiB), `M`(MiB), `G`(GiB). Ej: `find . -type f -size +10M`.
        *   `-user nombre`: Busca archivos del usuario `nombre`.
        *   `-group nombre`: Busca archivos del grupo `nombre`.
        *   `-perm modo`: Busca archivos con permisos exactos `modo` (octal o simbólico). `-perm -modo` busca si *todos* los bits de modo están presentes.
        *   `-mtime n`: Modificado hace `n` * 24 horas. `+n`=hace más de n días, `-n`=hace menos de n días.
        *   `-atime n`: Accedido hace `n` * 24 horas.
        *   `-ctime n`: Estado cambiado hace `n` * 24 horas.
        *   `-mmin n`, `-amin n`, `-cmin n`: Igual pero en minutos.
        *   `-newer archivo`: Busca archivos modificados más recientemente que `archivo`.
    *   **Operadores Lógicos (para combinar tests):**
        *   `-and` (o `-a`, implícito por defecto): Ambos tests deben ser verdaderos.
        *   `-or` (o `-o`): Al menos un test debe ser verdadero.
        *   `-not` (o `!`): Niega el resultado del test siguiente.
        *   `(` `)`: Agrupa tests y operadores (deben escaparse en la línea de comando: `\( ... \)`).
        *   Evaluación de cortocircuito: Si el resultado se sabe tras el primer test (ej: Falso en un AND, Verdadero en un OR), el segundo no se evalúa.
        *   Ej: `find . \( -type f -not -perm 0600 \) -or \( -type d -not -perm 0700 \)`
    *   **Acciones (qué hacer con los archivos encontrados):**
        *   `-print`: Imprime el nombre del archivo/ruta (acción por defecto si no se especifica otra).
        *   `-ls`: Ejecuta `ls -dils` en el archivo encontrado.
        *   `-delete`: Borra el archivo encontrado (**¡MUY PELIGROSO!** Probar con `-print` primero).
        *   `-exec comando {} \;`: Ejecuta `comando` para cada archivo. `{}` se reemplaza por el nombre del archivo. El comando debe terminar con `\;` (escapado). Lanza un proceso por cada archivo.
        *   `-exec comando {} +`: Similar a `-exec ... \;`, pero agrupa los nombres de archivo y ejecuta `comando` menos veces (más eficiente).
        *   `-ok comando {} \;`: Igual que `-exec ... \;` pero pide confirmación antes de cada ejecución.
    *   **Opciones (controlan el comportamiento de `find`):**
        *   `-maxdepth n`: Profundidad máxima a descender.
        *   `-mindepth n`: Profundidad mínima antes de aplicar tests/acciones.
        *   `-mount` (o `-xdev`): No cruzar a otros sistemas de archivos.
        *   `-depth`: Procesar contenido del directorio antes que el directorio mismo.
*   **Comando `xargs`:**
    *   Lee ítems de stdin (separados por espacio/newline por defecto) y los usa como argumentos para ejecutar otro `comando`.
    *   `find . -type f -name "*.tmp" -print | xargs rm`
    *   Útil para procesar listas de archivos generadas por `find`.
    *   Manejo de nombres con espacios/caracteres raros: Usar `find ... -print0 | xargs -0 comando`. (`-print0` separa con NUL, `-0` lee NUL).
*   **Comandos `touch` y `stat`:**
    *   `touch archivo`: Actualiza fecha de modificación/acceso de `archivo`. Si no existe, crea archivo vacío.
    *   `stat archivo`: Muestra información detallada (inodo, permisos, tamaño, fechas, etc.).

### Capítulo 18: ARCHIVADO Y RESPALDO

*   Compresión y archivado de archivos.
*   **Comandos Introducidos:**
    *   `gzip`, `gunzip`: Compresor/descompresor de archivos (formato `.gz`).
    *   `bzip2`, `bunzip2`: Compresor/descompresor alternativo (formato `.bz2`, a menudo mejor compresión pero más lento).
    *   `tar`: Utilidad de archivado en cinta (Tape ARchiver). Agrupa múltiples archivos/directorios en un solo archivo (`.tar`).
    *   `zip`, `unzip`: Empaqueta y comprime archivos (formato `.zip`, común en Windows).
    *   `rsync`: Sincronización remota (y local) de archivos y directorios.
*   **Compresión de Archivos:**
    *   Reduce el tamaño eliminando redundancia.
    *   **Sin Pérdida (Lossless):** Preserva todos los datos originales (`gzip`, `bzip2`, `zip`). Se puede restaurar el archivo exacto.
    *   **Con Pérdida (Lossy):** Elimina datos para mayor compresión (`JPEG`, `MP3`). La restauración es una aproximación.
    *   No intentar comprimir archivos ya comprimidos (JPEG, MP3, .gz, .zip), usualmente resultará en un archivo más grande.
    *   **`gzip [opciones] archivo...`:**
        *   Reemplaza `archivo` con `archivo.gz`.
        *   Opciones: `-c` (a stdout, no borra original), `-d` (descomprimir, como `gunzip`), `-r` (recursivo), `-t` (testear), `-v` (verbose), `-N` (nivel 1-9).
        *   `zcat archivo.gz` (o `gunzip -c archivo.gz`): Ver contenido sin descomprimir. `zless` es como `less` para `.gz`.
    *   **`bzip2 [opciones] archivo...`:**
        *   Similar a `gzip`, crea `archivo.bz2`.
        *   Generalmente mejor compresión que `gzip`.
        *   `bunzip2` descomprime, `bzcat` muestra contenido.
*   **Archivado de Archivos:**
    *   Agrupar muchos archivos en uno solo (archivo `tar`). Facilita transferencia y almacenamiento.
    *   **`tar modo[opciones] [archivo_tar] [ruta...]`:**
        *   **Modos Principales:**
            *   `c`: Crear un nuevo archivo tar.
            *   `x`: Extraer archivos de un tar.
            *   `t`: Listar contenido de un tar.
            *   `r`: Añadir archivos al final de un tar existente.
            *   `u`: Añadir archivos solo si son más nuevos que los del tar.
        *   **Opciones Comunes (sin `-` inicial es tradicional, pero `-` también funciona):**
            *   `f archivo`: Especifica el nombre del archivo tar (sino usa cinta magnética!). **Casi siempre necesaria.**
            *   `v`: Verbose (muestra nombres de archivos procesados).
            *   `z`: Filtrar a través de `gzip` (crear/leer `.tar.gz` o `.tgz`).
            *   `j`: Filtrar a través de `bzip2` (crear/leer `.tar.bz2` o `.tbz`).
            *   `p`: Preservar permisos.
            *   `C directorio`: Cambiar a `directorio` antes de operar.
        *   **Ejemplos:**
            *   Crear: `tar cvf mi_archivo.tar /directorio/a/guardar`
            *   Crear comprimido con gzip: `tar cvzf mi_archivo.tgz /directorio/a/guardar`
            *   Listar: `tar tvf mi_archivo.tar`
            *   Extraer: `tar xvf mi_archivo.tar` (extrae en directorio actual)
            *   Extraer comprimido: `tar xvzf mi_archivo.tgz`
            *   Extraer a otro directorio: `tar xvf mi_archivo.tar -C /otro/directorio`
        *   `tar` guarda rutas relativas por defecto (quitando `/` inicial) para poder extraer en cualquier lugar.
        *   Se puede usar con `find`: `find . -name "*.txt" -print0 | tar czf textos.tgz --null -T -` (`-T -` lee lista de archivos de stdin, `--null` maneja nombres raros).
        *   Copiar directorios entre máquinas: `ssh host_remoto 'tar cf - /ruta/remota' | tar xf -`
    *   **`zip archivo.zip archivo...` / `unzip archivo.zip`:**
        *   Herramienta de compresión y archivado (formato `.zip`).
        *   `zip -r archivo.zip directorio`: Crea zip recursivo de un directorio.
        *   `unzip archivo.zip`: Extrae.
        *   `unzip -l archivo.zip`: Lista contenido.
        *   Útil para intercambiar con Windows.
*   **Sincronización de Archivos (`rsync`):**
    *   `rsync [opciones] origen destino`: Sincroniza `origen` con `destino`.
    *   Muy eficiente: Transfiere solo las diferencias usando un protocolo especial.
    *   `origen`/`destino` pueden ser locales o remotos (`[usuario@]host:ruta`).
    *   **Opciones Comunes:**
        *   `-a`: Modo archivo (recursivo, preserva enlaces, permisos, fechas, propietario, grupo). Equivale a `-rlptgoD`.
        *   `-v`: Verbose.
        *   `-z`: Comprimir datos durante transferencia.
        *   `--delete`: Borra archivos en `destino` que no existen en `origen`. **¡Usar con cuidado!**
        *   `--exclude patrón`: Excluye archivos que coincidan con `patrón`.
        *   `-n` o `--dry-run`: Simula la operación, muestra qué haría sin hacerlo. **¡Muy útil para probar!**
        *   `-P`: Equivale a `--partial --progress` (muestra progreso y permite reanudar).
        *   `--rsh=comando`: Especifica la shell remota (ej: `--rsh=ssh`).
    *   **Importancia de `/` al final del origen:**
        *   `rsync -av origen destino`: Copia el directorio `origen` *dentro* de `destino`.
        *   `rsync -av origen/ destino`: Copia el *contenido* de `origen` a `destino`.
    *   Útil para backups incrementales y mirroring.

### Capítulo 19: EXPRESIONES REGULARES

*   **Expresiones Regulares (Regex):** Notaciones simbólicas para identificar patrones en texto. Más potentes que los comodines de la shell.
*   Usadas por muchas herramientas (`grep`, `sed`, `awk`, `less`, `vim`) y lenguajes de programación.
*   Existen ligeras variaciones (POSIX BRE, POSIX ERE, Perl, etc.). Nos centraremos en POSIX.
*   **Comando `grep` (repaso):**
    *   `grep [opciones] regex [archivo...]`: Busca `regex` en archivos (o stdin). Imprime líneas coincidentes.
    *   **Opciones Comunes:** `-i` (ignorar caso), `-v` (invertir), `-c` (contar), `-l` (listar archivos), `-n` (número línea), `-h` (sin nombre archivo).
*   **Literales y Metacaracteres:**
    *   **Literales:** Caracteres que coinciden consigo mismos (ej: `a`, `b`, `1`, `2`).
    *   **Metacaracteres:** Caracteres con significado especial en regex. En POSIX BRE/ERE: `^ $ . [ ] { } - ? * + ( ) | \`. Otros caracteres son literales.
    *   **Escape:** `\` antes de un metacaracter lo trata como literal (ej: `\.` busca un punto literal).
    *   **Importante:** Al usar regex en la línea de comandos, **entrecomillarlas** (usualmente comillas simples `'`) para evitar expansión por la shell.
*   **Metacaracteres Básicos (POSIX BRE y ERE):**
    *   `.` (Punto): Coincide con **cualquier carácter individual** (excepto newline). Ej: `'a.c'` coincide con 'abc', 'a_c', 'a5c'.
    *   `^` (Caret): Ancla. Coincide con el **inicio de la línea**. Ej: `'^Inicio'` coincide solo si la línea empieza con "Inicio".
    *   `$` (Dólar): Ancla. Coincide con el **fin de la línea**. Ej: `'fin$'` coincide solo si la línea termina con "fin".
    *   `^$`: Coincide con una línea vacía.
    *   `[]` (Corchetes): Expresión de corchetes. Coincide con **un solo carácter** que esté *dentro* del conjunto.
        *   `[aeiou]`: Coincide con una vocal minúscula.
        *   `[0-9]`: Coincide con un dígito (rango tradicional).
        *   `[a-zA-Z]`: Coincide con una letra (rango tradicional).
        *   **Negación:** Si el primer carácter es `^`, coincide con cualquier carácter *excepto* los del conjunto. Ej: `[^0-9]` coincide con cualquier carácter no numérico.
        *   `-` (guion) es literal si es el primero o último dentro de `[]`. `^` es literal si no es el primero. `]` es literal si es el primero.
    *   **Clases de Caracteres POSIX:** Alternativa a rangos, más portable entre locales. Se usan *dentro* de `[]`.
        *   `[[:alnum:]]`, `[[:alpha:]]`, `[[:digit:]]`, `[[:lower:]]`, `[[:upper:]]`, `[[:space:]]` (espacio, tab, nl, etc.), `[[:punct:]]`, etc.
        *   Ej: `'^[[:upper:]]'` coincide con línea que empieza con mayúscula.
*   **Expresiones Regulares Extendidas (ERE):** Usar `grep -E` o `egrep`. Añaden más metacaracteres.
    *   `?`: Cuantificador. El elemento anterior es opcional (coincide **0 ó 1** vez). Ej: `colou?r` coincide con 'color' y 'colour'.
    *   `*`: Cuantificador. El elemento anterior coincide **0 ó más** veces. Ej: `'a*b'` coincide con 'b', 'ab', 'aab', etc.
    *   `+`: Cuantificador. El elemento anterior coincide **1 ó más** veces. Ej: `'a+b'` coincide con 'ab', 'aab', pero no 'b'.
    *   `{n}`: Cuantificador. El elemento anterior coincide exactamente **n** veces. Ej: `[0-9]{3}` coincide con 3 dígitos.
    *   `{n,}`: Cuantificador. El elemento anterior coincide **n ó más** veces.
    *   `{n,m}`: Cuantificador. El elemento anterior coincide entre **n y m** veces (inclusive).
    *   `|`: Alternancia (OR). Coincide con la expresión a la izquierda **O** la expresión a la derecha. Ej: `gato|perro` coincide con 'gato' o 'perro'.
    *   `()`: Agrupación. Agrupa sub-expresiones para aplicar cuantificadores o alternancia. Ej: `^(bz|gz|zip)` coincide con líneas que empiezan con 'bz' o 'gz' o 'zip'.
*   **BRE vs ERE:**
    *   BRE (Basic, `grep` por defecto): Metacaracteres `^ $ . [ ] *`. Los metacaracteres `( ) { }` necesitan `\` para ser especiales (ej: `\{n\}`). `| ? +` no existen o necesitan `\`.
    *   ERE (Extended, `grep -E`): Metacaracteres adicionales `? * + { } ( ) |`. Aquí, `\` *quita* el significado especial.
*   **Aplicaciones:**
    *   `grep` para validar formatos (ej: números teléfono, emails).
    *   `find -regex patrón`: Busca archivos cuya ruta *completa* coincida con `patrón` (ERE por defecto en GNU find).
    *   `locate --regex patrón`: Busca en la base de datos usando ERE.
    *   `less` y `vim`: Usan `/patrón` para buscar (usualmente BRE, `vim` tiene opciones para ERE).

### Capítulo 20: PROCESAMIENTO DE TEXTO

*   Herramientas para manipular contenido de archivos de texto.
*   **Comandos Introducidos/Revisitados:**
    *   `cat`: Concatenar/mostrar archivos (con opciones nuevas).
    *   `sort`: Ordenar líneas (con opciones nuevas).
    *   `uniq`: Eliminar/reportar líneas duplicadas adyacentes.
    *   `cut`: Extraer secciones (columnas/caracteres) de cada línea.
    *   `paste`: Unir líneas de archivos (pegar columnas).
    *   `join`: Unir líneas de dos archivos basadas en un campo común.
    *   `comm`: Comparar dos archivos ordenados línea por línea.
    *   `diff`: Comparar archivos línea por línea, mostrando diferencias.
    *   `patch`: Aplicar un archivo `diff` (parche) a un archivo original.
    *   `tr`: Traducir o borrar caracteres.
    *   `sed`: Editor de flujo (stream editor) para filtrar y transformar texto.
    *   `aspell`: Corrector ortográfico interactivo.
*   **Usos del Texto en Linux:** Documentos, páginas web (HTML/XML), email, salida de impresora (PostScript), código fuente.
*   **Más sobre `cat`:**
    *   `cat -A`: Muestra caracteres no imprimibles (`^I` para tab, `$` al final de línea). Útil para detectar tabs vs espacios, espacios finales, finales de línea DOS (`^M$`).
    *   `cat -n`: Numera líneas de salida.
    *   `cat -s`: Suprime líneas en blanco repetidas.
*   **Más sobre `sort`:**
    *   Ordena líneas de archivos o stdin.
    *   **Opciones Clave:**
        *   `-n`: Orden numérico.
        *   `-r`: Orden inverso (descendente).
        *   `-f`: Ignorar mayúsculas/minúsculas.
        *   `-k campo`: Ordenar basado en el `campo` especificado (columna). Los campos se separan por defecto por espacios/tabs.
        *   `-t char`: Usar `char` como separador de campos (ej: `-t:` para `/etc/passwd`).
        *   Se pueden especificar múltiples claves `-k`. Ej: `sort -k 1,1 -k 2n` (ordena por campo 1 alfabético, luego por campo 2 numérico).
        *   Se pueden especificar sub-campos: `sort -k 3.7` (ordena por el 7mo carácter del 3er campo).
*   **Más sobre `uniq`:**
    *   Requiere **entrada ordenada**. Elimina líneas duplicadas *adyacentes*.
    *   `sort archivo | uniq`. (O `sort -u archivo`).
    *   Opciones: `-c` (contar ocurrencias), `-d` (mostrar solo duplicadas), `-u` (mostrar solo únicas).
*   **Comando `cut`:**
    *   Extrae partes de líneas.
    *   `cut -c lista`: Extrae caracteres especificados en `lista` (ej: `-c 1-5`, `-c 1,3,5`).
    *   `cut -f lista`: Extrae campos (columnas) especificados en `lista`. Por defecto, el delimitador es TAB.
    *   `cut -d delimitador`: Especifica el `delimitador` de campos para `-f`.
    *   Ej: `cut -d ':' -f 1 /etc/passwd` (extrae nombres de usuario).
*   **Comando `paste`:**
    *   Combina líneas de múltiples archivos, columna por columna.
    *   `paste archivo1 archivo2`: Pega las líneas correspondientes de `archivo1` y `archivo2` separadas por TAB.
*   **Comando `join`:**
    *   Une líneas de dos archivos que tienen un campo (columna) **común y ordenado**. Similar a JOIN en SQL.
    *   `join archivo1 archivo2`: Une basado en el primer campo de cada archivo.
    *   Opciones para especificar campos de unión y delimitadores.
*   **Comparar Texto:**
    *   **`comm archivo1 archivo2`:** Compara dos archivos **ordenados**. Produce 3 columnas: líneas solo en arch1, líneas solo en arch2, líneas en ambos. Opciones `-1`, `-2`, `-3` suprimen columnas.
    *   **`diff archivo1 archivo2`:** Muestra las diferencias entre dos archivos.
        *   Formato por defecto: Edición tipo `ed` (críptico).
        *   Formato Contexto (`-c`): Muestra diferencias con líneas de contexto alrededor. `***` archivo1, `---` archivo2. Indicadores: `-` (borrado), `+` (añadido), `!` (cambiado).
        *   Formato Unificado (`-u`): Más conciso que contexto. Indicadores: `-` (línea solo en arch1), `+` (línea solo en arch2), ` ` (contexto). Preferido para parches.
        *   Útil para comparar versiones de archivos (configuración, código).
    *   **`patch < archivo_diff`:** Aplica un archivo de diferencias (creado con `diff -u` o similar) a un archivo original para convertirlo en la nueva versión. Fundamental en desarrollo de software colaborativo (ej: parches del kernel).
*   **Edición en Flujo:**
    *   **`tr set1 [set2]`:** Translitera (reemplaza) caracteres. Lee de stdin, escribe a stdout.
        *   `echo "hola" | tr 'a-z' 'A-Z'` -> `HOLA` (convierte a mayúsculas).
        *   `tr -d 'caracteres'`: Borra los `caracteres` especificados. Ej: `tr -d '\r' < dos.txt > unix.txt` (quita CR de archivos DOS).
        *   `tr -s 'caracteres'`: Comprime secuencias repetidas de `caracteres` a uno solo. Ej: `echo "a   b" | tr -s ' '` -> `a b`.
    *   **`sed [opciones] 'comando' [archivo...]`:** Stream Editor. Procesa texto línea por línea aplicando `comando`. Muy potente.
        *   `sed 's/regex/reemplazo/flags'`: Comando de sustitución (search & replace).
            *   `regex`: Expresión regular básica (BRE).
            *   `reemplazo`: Texto de reemplazo. `&` representa el texto coincidente. `\1`, `\2`... refieren a subexpresiones `\(...\)` en `regex` (back-references).
            *   `flags`: `g` (global, todas las ocurrencias en la línea), `i` (ignorar caso), etc.
        *   `sed 'comando'` se aplica a todas las líneas. Se puede prefijar con una *dirección* para aplicarlo solo a ciertas líneas:
            *   `n`: Número de línea.
            *   `$` : Última línea.
            *   `/regex/`: Líneas que coincidan con `regex`.
            *   `addr1,addr2`: Rango de líneas.
        *   Otros comandos: `d` (delete line), `p` (print line, usar con `-n`), `a` (append text after), `i` (insert text before), `y/set1/set2/` (transliterate).
        *   `sed -f script.sed archivo`: Lee comandos desde `script.sed`.
        *   `sed -i ... archivo`: Edita el archivo "in-place" (modifica el original). **¡Cuidado!**
*   **Corrector Ortográfico:**
    *   **`aspell check archivo.txt`:** Corrector interactivo en terminal.
    *   Opciones para diferentes modos (ej: `-H` para HTML, ignora tags).

### Capítulo 21: FORMATEANDO SALIDA

*   Herramientas para dar formato a texto, a menudo para impresión.
*   **Comandos Introducidos:**
    *   `nl`: Numerar líneas.
    *   `fold`: Cortar líneas largas a una longitud específica.
    *   `fmt`: Formateador de texto simple (ajusta párrafos).
    *   `pr`: Preparar texto para impresión (paginación, cabeceras).
    *   `printf`: Formatear e imprimir datos (builtin de bash).
    *   `groff`: Sistema de formateo de documentos (GNU Troff).
*   **Comando `nl`:**
    *   Numera líneas de archivos o stdin.
    *   `nl archivo.txt`. Similar a `cat -n`.
    *   Soporta "páginas lógicas" con marcas `\:\:\:`, `\:\:`, `\:` para cabecera/cuerpo/pie, permitiendo reiniciar numeración o cambiar estilo.
    *   Opciones para controlar estilo de numeración (`-b`), formato (`-n`), separador (`-s`), ancho (`-w`), etc.
*   **Comando `fold`:**
    *   Corta líneas largas.
    *   `fold -w ancho archivo.txt`: Corta líneas a `ancho` caracteres (por defecto 80).
    *   Por defecto, corta sin importar palabras.
    *   `fold -s`: Intenta cortar en espacios en blanco antes del ancho límite.
*   **Comando `fmt`:**
    *   Formateador de párrafos más inteligente que `fold`.
    *   Intenta unir líneas cortas y cortar largas para ajustar a un ancho, preservando párrafos (líneas en blanco) e indentación inicial por defecto.
    *   `fmt -w ancho archivo.txt`: Formatea a `ancho` columnas (defecto 75).
    *   Opciones: `-c` (crown margin), `-s` (solo cortar, no unir), `-u` (espaciado uniforme: 1 entre palabras, 2 entre frases).
*   **Comando `pr`:**
    *   Pagina texto para impresión. Añade cabeceras, pies de página, márgenes.
    *   `pr [opciones] archivo.txt`.
    *   Opciones: `-l longitud` (líneas por pág, defecto 66), `-w ancho` (caracteres por línea, defecto 72), `-h "Texto Cabecera"`, `-Ncolumna` (formato multicolumna).
    *   Ej: `ls /usr/bin | pr -3 -w 65` (lista en 3 columnas, ancho 65).
*   **Comando `printf` (Builtin):**
    *   Formatea e imprime datos según una cadena de formato. No lee stdin. Muy usado en scripts.
    *   `printf "formato" argumento1 argumento2 ...`
    *   `formato`: Cadena con texto literal, secuencias de escape (`\n`, `\t`) y **especificadores de conversión** (`%`).
    *   **Especificadores Comunes:** `%s` (string), `%d` (entero decimal), `%f` (punto flotante), `%c` (carácter), `%%` (`%` literal).
    *   **Modificadores (entre `%` y letra):** `printf "%[flags][width][.precision]letra"`
        *   `flags`: `-` (izq.), `+` (signo), ` ` (espacio si +), `0` (relleno ceros), `#` (forma alt.).
        *   `width`: Ancho mínimo del campo (rellena con espacios o ceros).
        *   `.precision`: Para `%f`, decimales; para `%s`, máx. caracteres.
    *   Ej: `printf "Nombre: %-10s Edad: %03d\n" "Ana" 25`.
*   **Sistemas de Formateo de Documentos:**
    *   Para documentos complejos (libros, artículos). Usan lenguajes de marcado en texto plano.
    *   **Familia `roff` (`nroff`, `troff`, `groff`):** Clásico de Unix.
        *   `groff`: Implementación GNU.
        *   Procesa texto con marcas embebidas.
        *   Paquetes de macros (`-man`, `-ms`, `-me`) simplifican el marcado.
        *   Puede generar salida para terminales (`-Tascii`), PostScript (`-Tps`, por defecto), PDF (`-Tpdf`), HTML (`-Thtml`).
        *   `tbl` (tablas), `eqn` (ecuaciones), `pic` (diagramas) son preprocesadores.
        *   Ej: `groff -ms -T ps documento.ms > documento.ps`.
        *   Las páginas `man` usan `groff -mandoc`.
    *   **TEX (`LaTeX`):** Otro sistema potente, popular en academia. `texlive` es el paquete común.

### Capítulo 22: IMPRESIÓN

*   Herramientas de línea de comandos para imprimir y gestionar trabajos de impresión.
*   **Comandos Introducidos:**
    *   `pr`: Preparar texto para imprimir (repaso).
    *   `lpr`: Enviar archivos a la impresora (estilo Berkeley/LPD).
    *   `lp`: Enviar archivos a la impresora (estilo System V).
    *   `a2ps`: Formatear archivos para imprimir en impresora PostScript (o compatible).
    *   `lpstat`: Mostrar estado del sistema de impresión.
    *   `lpq`: Mostrar estado de la cola de impresión.
    *   `lprm`, `cancel`: Cancelar trabajos de impresión.
*   **Sistema de Impresión en Linux (CUPS):**
    *   **CUPS (Common Unix Printing System):** Sistema moderno estándar en Linux. Gestiona drivers, colas, trabajos.
    *   **Ghostscript:** Intérprete PostScript, actúa como RIP (Raster Image Processor) para convertir PostScript/PDF a formato de puntos que la impresora entiende.
    *   **Colas de Impresión (Print Queues):** Cada impresora tiene una cola donde los trabajos esperan a ser enviados a la impresora física.
*   **Preparar Archivos para Imprimir:**
    *   `pr`: Formatea texto en páginas con cabeceras/márgenes. Útil para texto plano. Opciones (`-l`, `-w`, `-h`, `-Ncolumna`, etc.).
*   **Enviar Trabajo a Impresora:**
    *   **`lpr [opciones] [archivo...]`:** Estilo Berkeley.
        *   Lee de stdin si no se dan archivos.
        *   `lpr archivo.txt`.
        *   `-P impresora`: Especifica la impresora destino (si no, usa la por defecto).
        *   `-# num`: Número de copias.
        *   `-p`: "Pretty print" (añade cabecera sombreada).
        *   Ej: `ls | pr -2 | lpr -P mi_laser`.
    *   **`lp [opciones] [archivo...]`:** Estilo System V.
        *   Lee de stdin si no se dan archivos.
        *   `lp archivo.txt`.
        *   `-d impresora`: Especifica la impresora destino.
        *   `-n num`: Número de copias.
        *   `-o opcion`: Opciones específicas del driver/CUPS (muy versátil).
            *   `-o landscape`: Orientación horizontal.
            *   `-o fitplot`: Escalar imagen para ajustar a página.
            *   `-o scaling=N`: Escalar a N% (100=lleno).
            *   `-o cpi=N`, `-o lpi=N`: Caracteres/Líneas por pulgada (texto).
            *   `-o page-left=N`, `-o page-right=N`, etc.: Márgenes en puntos (72 pts = 1 pulgada).
        *   Ej: `lp -d color -n 2 -o landscape informe.pdf`.
    *   **`a2ps [opciones] [archivo...]`:** "Anything to PostScript".
        *   Formatea elegantemente (a menudo "2-up", 2 páginas por hoja) y envía a impresora o archivo PostScript.
        *   Detecta tipo de archivo y aplica formato (syntax highlighting para código).
        *   `-o archivo.ps`: Guarda salida en archivo PostScript.
        *   `-P impresora`: Especifica impresora.
        *   `--columns=N`: N páginas por hoja.
        *   `-R` (portrait), `-r` (landscape).
        *   `-B` (sin cabeceras), `-b "Texto"` (cabecera).
        *   Ej: `a2ps --columns=1 -P laser *.c`.
*   **Monitorizar y Controlar Trabajos:**
    *   **`lpstat`:** Muestra estado.
        *   `lpstat -a`: Muestra estado de aceptación de trabajos de las colas.
        *   `lpstat -p`: Muestra estado de las impresoras físicas.
        *   `lpstat -d`: Muestra la impresora por defecto.
        *   `lpstat -s`: Resumen del sistema (impresora def, dispositivos).
        *   `lpstat -t`: Estado completo.
    *   **`lpq [-P impresora]`:** Muestra el contenido de la cola de impresión (trabajos en espera o imprimiendo). Muestra `job ID`.
    *   **`lprm job_id`:** Cancela un trabajo de impresión (estilo Berkeley).
    *   **`cancel job_id`:** Cancela un trabajo de impresión (estilo System V).
    *   Se puede requerir `sudo` para cancelar trabajos de otros usuarios.

### Capítulo 23: COMPILANDO PROGRAMAS

*   Proceso de construir programas ejecutables a partir de su código fuente.
*   **¿Por qué compilar?**
    *   Software no disponible en repositorios de la distribución.
    *   Obtener la versión más reciente de un programa.
    *   Personalizar opciones de compilación.
*   **Comando Introducido:**
    *   `make`: Utilidad para mantener programas (automatiza la compilación).
*   **¿Qué es Compilar?**
    *   Traducir **código fuente** (legible por humanos, ej: C, C++) a **código máquina** (instrucciones numéricas para la CPU).
    *   **Lenguajes Compilados (ej: C, C++):**
        *   **Compilador (ej: `gcc`):** Programa que traduce fuente a código objeto (o ensamblador).
        *   **Ensamblador (ej: `as`):** Traduce lenguaje ensamblador a código máquina.
        *   **Enlazador (Linker, ej: `ld`):** Combina código objeto compilado con **bibliotecas** (código compartido para tareas comunes, ej: `libc`) para crear el **archivo ejecutable** final.
    *   **Lenguajes Interpretados (ej: Shell, Python, Perl):**
        *   No se compilan a código máquina de antemano.
        *   Un **intérprete** lee el código fuente y lo ejecuta instrucción por instrucción.
        *   Generalmente más lentos en ejecución pero más rápidos en desarrollo (sin paso de compilación).
*   **Proceso Típico de Compilación (GNU Autotools):** La mayoría del software FOSS sigue este patrón.
    1.  **Obtener Código Fuente:**
        *   Usualmente un archivo comprimido (`.tar.gz`, `.tar.bz2`) llamado *tarball*.
        *   Descargar desde sitio web del proyecto, FTP, etc. (usar `wget` o `ftp`).
        *   Crear un directorio para fuentes (ej: `~/src`).
    2.  **Desempaquetar:**
        *   `tar xzf archivo.tar.gz` o `tar xjf archivo.tar.bz2`.
        *   Crea un directorio con el nombre del proyecto y versión (ej: `proyecto-1.2.3`).
        *   **Consejo:** Listar contenido antes con `tar tzvf ... | head` para ver si crea directorio o no.
    3.  **Explorar Árbol de Fuentes:**
        *   `cd directorio_proyecto`.
        *   Buscar archivos `README` e `INSTALL`. **Leerlos** para instrucciones específicas.
        *   Archivos `.c` (código C), `.h` (cabeceras C), `Makefile.in`, `configure`, etc.
    4.  **Configurar (`./configure`):**
        *   Ejecutar el script `configure` (está en el directorio actual, por eso `./`).
        *   Analiza el entorno de compilación (sistema, bibliotecas instaladas, herramientas).
        *   Prepara el proceso de compilación para el sistema específico.
        *   Genera el archivo `Makefile` a partir de `Makefile.in` y otros archivos (ej: `config.h`).
        *   Revisar la salida por **errores**. Si falla, usualmente faltan dependencias (bibliotecas de desarrollo, `-dev` o `-devel` en nombres de paquete).
    5.  **Compilar (`make`):**
        *   Ejecuta el comando `make`.
        *   `make` lee el `Makefile`.
        *   El `Makefile` contiene *reglas* que definen *objetivos* (ej: archivos ejecutables, archivos objeto `.o`) y sus *dependencias* (ej: archivos fuente `.c`, cabeceras `.h`).
        *   `make` determina qué necesita ser compilado/recompilado basado en las fechas de modificación de archivos. Si un `.c` es más nuevo que su `.o`, recompila el `.o`. Si un `.o` es más nuevo que el ejecutable, vuelve a enlazar.
        *   Ejecuta los comandos necesarios (compilar, enlazar) para construir los objetivos.
        *   Si se ejecuta `make` de nuevo sin cambios, dirá "Nothing to be done".
    6.  **Instalar (`sudo make install`):**
        *   Si `make` fue exitoso, este comando (si el `Makefile` lo soporta) copia los archivos compilados (ejecutables, bibliotecas, páginas man, etc.) a sus ubicaciones estándar en el sistema.
        *   Ubicación común para software compilado localmente: `/usr/local/bin`, `/usr/local/lib`, etc.
        *   Requiere `sudo` porque escribe fuera del directorio home.
    7.  **(Opcional) Limpiar (`make clean`):**
        *   Elimina los archivos generados durante la compilación (ej: `.o`, ejecutables) dejando solo los fuentes. Útil si algo salió mal o para ahorrar espacio.
*   **Herramientas Necesarias:**
    *   Compilador C (`gcc`).
    *   Utilidad `make`.
    *   Bibliotecas de desarrollo necesarias para el programa a compilar (paquetes `*-dev` o `*-devel`).
    *   Muchas distros ofrecen un meta-paquete (ej: `build-essential` en Debian/Ubuntu) que instala las herramientas básicas.

## PARTE IV: ESCRIBIENDO SHELL SCRIPTS

### Capítulo 24: ESCRIBIENDO TU PRIMER SCRIPT

*   **Shell Scripts:** Archivos de texto que contienen una serie de comandos de la shell. La shell los lee y ejecuta como si se teclearan en la línea de comandos.
*   Permiten automatizar tareas complejas combinando herramientas.
*   **Cómo Escribir un Script:**
    1.  **Escribir el Script:** Usar un editor de texto (preferiblemente con *resaltado de sintaxis* para detectar errores, ej: `vim`, `gedit`, `kate`).
    2.  **Hacerlo Ejecutable:** Dar permisos de ejecución al archivo. `chmod 755 mi_script.sh` (para todos) o `chmod 700 mi_script.sh` (solo propietario).
    3.  **Ubicarlo:** Poner el script en un directorio que esté en la variable de entorno `PATH` para poder ejecutarlo solo con su nombre.
*   **Formato del Archivo de Script:**
    *   **Shebang (`#!`):** **Obligatorio** en la *primera línea*. Indica al kernel qué intérprete usar.
        *   Para scripts bash: `#!/bin/bash` (o `#!/usr/bin/env bash` para más portabilidad).
    *   **Comentarios (`#`):** Líneas que empiezan con `#` son ignoradas por la shell. Usar para explicar el código. Pueden ir al final de una línea de comando también.
    *   **Comandos:** Comandos de la shell, uno o más por línea (separados por `;` si en la misma línea).
*   **Ejemplo "Hola Mundo":**
    ```bash
    #!/bin/bash
    # Mi primer script

    echo '¡Hola Mundo!'
    ```
    Guardar como `hola_mundo`, luego `chmod 755 hola_mundo`.
*   **Ejecutar un Script:**
    *   Si **no** está en el `PATH`: `ruta/al/script` o `./script` (si está en directorio actual).
    *   Si **está** en el `PATH`: `nombre_script` (sin ruta).
*   **Variable `PATH`:**
    *   Lista de directorios separados por `:` donde la shell busca ejecutables.
    *   Ver con `echo $PATH`.
    *   Convención común: Crear directorio `~/bin` y añadirlo al `PATH`.
    *   Añadir a `PATH` (en `~/.bashrc` o `~/.profile`): `export PATH="$HOME/bin:$PATH"` (añade `~/bin` al *inicio* de la búsqueda).
    *   Aplicar cambio en sesión actual: `source ~/.bashrc` (o `. ~/.bashrc`).
*   **Ubicaciones Comunes para Scripts:**
    *   `~/bin`: Scripts personales del usuario.
    *   `/usr/local/bin`: Scripts/programas instalados localmente para todos los usuarios.
    *   `/usr/local/sbin`: Scripts/programas de administración instalados localmente.
    *   Evitar poner scripts propios en `/bin`, `/usr/bin`, `/sbin`, `/usr/sbin` (reservados para la distribución).
*   **Trucos de Formato para Legibilidad:**
    *   Usar **nombres largos de opciones** (ej: `--recursive` en vez de `-r`) en scripts mejora la claridad.
    *   Usar **indentación** (espacios o tabs) para mostrar la estructura lógica (dentro de `if`, `while`, `for`, funciones).
    *   Usar **continuación de línea** (`\`) al final de una línea para dividir comandos largos en varias líneas.

### Capítulo 25: INICIANDO UN PROYECTO

*   Se inicia un proyecto ejemplo: Generador de informes del sistema en formato HTML.
*   **Etapa 1: Documento Mínimo:**
    *   Estructura básica HTML: `<html><head><title>...</title></head><body>...</body></html>`.
    *   Script inicial usando `echo` para cada línea HTML.
    *   Redirigir salida del script a un archivo `.html`: `mi_script > informe.html`.
    *   Mejora: Usar un solo `echo` con una cadena multilínea entrecomillada (`"..."`). La shell maneja los saltos de línea dentro de las comillas.
*   **Variables y Constantes:**
    *   Usar variables para almacenar valores repetidos o que pueden cambiar, mejorando mantenibilidad.
    *   **Creación:** Simplemente usarla (ej: `mivariable="valor"`). `bash` la crea automáticamente.
    *   **Asignación:** `variable=valor` (**sin espacios** alrededor de `=`).
    *   **Expansión:** `$variable` o `${variable}` (llaves necesarias para desambiguar, ej: `${var}texto`).
    *   **Tipos:** `bash` trata todo como cadenas por defecto. `declare -i` para forzar entero.
    *   **Constantes:** Variables cuyo valor no debería cambiar. Convención: usar NOMBRES_EN_MAYUSCULAS. `declare -r CONSTANTE="valor"` la hace de solo lectura.
    *   **Peligro:** Variables vacías o mal escritas se expanden a nada, lo que puede romper comandos. **Siempre entrecomillar las expansiones (`"$variable"`)** a menos que se necesite separación de palabras.
*   **Documentos Incrustados (Here Documents):**
    *   Alternativa a `echo` para bloques grandes de texto.
    *   Sintaxis:
        ```bash
        comando << DELIMITADOR
        Texto línea 1 $variable $(comando)
        Texto línea 2 'esto' "eso"
        ...
        DELIMITADOR
        ```
    *   `comando` recibe el texto entre `<< DELIMITADOR` y `DELIMITADOR` como su entrada estándar (stdin).
    *   `DELIMITADOR` es una cadena elegida por el programador (convención: `_EOF_`). Debe estar **solo** en la línea de cierre, sin espacios antes ni después.
    *   Dentro del "here document", las comillas (`'` y `"`) pierden su significado especial y se tratan literalmente.
    *   La expansión de parámetros (`$var`), sustitución de comandos (`$(cmd)`) y aritmética (`$(( ))`) **sí** ocurren por defecto. (Se puede evitar si el `DELIMITADOR` inicial va entre comillas: `<< 'DELIMITADOR'`).
    *   `<<- DELIMITADOR`: Permite indentar el "here document" con **tabs** (no espacios) iniciales, que serán eliminados. Mejora legibilidad.

### Capítulo 26: DISEÑO DESCENDENTE (TOP-DOWN DESIGN)

*   **Diseño Descendente:** Técnica para abordar problemas complejos dividiéndolos en tareas más pequeñas y manejables.
    *   Identificar pasos principales de alto nivel.
    *   Descomponer cada paso en sub-pasos más detallados.
    *   Continuar hasta que cada paso sea simple de implementar.
*   **Funciones de Shell:**
    *   Mecanismo para agrupar una serie de comandos bajo un nombre, creando "mini-scripts" dentro de un script más grande. Encapsulan tareas.
    *   Perfectas para implementar los sub-pasos del diseño descendente.
    *   **Sintaxis (dos formas equivalentes):**
        1.  `function nombre_funcion { comandos; return; }`
        2.  `nombre_funcion () { comandos; return; }` (Forma preferida)
    *   **Llamada:** Simplemente usar `nombre_funcion` como si fuera un comando.
    *   **Definición:** La definición de la función debe aparecer en el script **antes** de su primera llamada.
    *   `return [n]`: Termina la función. Opcionalmente devuelve un *estado de salida* `n` (0=éxito, >0=fallo). Si se omite, el estado de salida es el del último comando ejecutado en la función.
*   **Stubs (Esqueletos de Funciones):**
    *   Crear funciones vacías o con un simple `echo` que indique que se ejecutó.
    *   Permite verificar el flujo lógico general del programa antes de implementar los detalles de cada función.
    *   Ayuda a mantener el script **ejecutable** durante el desarrollo y a probarlo frecuentemente.
*   **Variables Locales:**
    *   Variables definidas *dentro* de una función usando la palabra clave `local`.
    *   `local nombre_variable[=valor]`
    *   Solo son visibles y accesibles **dentro** de la función donde se declararon.
    *   Dejan de existir cuando la función termina.
    *   **Ventajas:**
        *   Evitan conflictos de nombres con variables globales u otras funciones.
        *   Promueven la encapsulación y hacen las funciones más reutilizables e independientes.
*   **Aplicación al Proyecto `sys_info_page`:**
    *   Se definen funciones stub para `report_uptime`, `report_disk_space`, `report_home_space`.
    *   Se implementa el contenido de cada función usando comandos (`uptime`, `df -h`, `du -sh`) y `cat` con "here documents" para formatear la salida HTML.

### Capítulo 27: CONTROL DE FLUJO: BIFURCACIÓN CON IF

*   **Bifurcación (Branching):** Ejecutar diferentes bloques de código basados en una condición.
*   **Estado de Salida (Exit Status):**
    *   Todo comando, al terminar, devuelve un valor numérico (0-255) al sistema.
    *   `0` significa **éxito**.
    *   Cualquier valor distinto de `0` significa **fallo**.
    *   La variable especial `$?` contiene el estado de salida del *último* comando ejecutado. `echo $?`.
    *   Comandos `true` (siempre éxito, sale con 0) y `false` (siempre fallo, sale con 1).
*   **Comando `if`:**
    *   Ejecuta bloques de código condicionalmente basado en el *estado de salida* de un comando.
    *   **Sintaxis:**
        ```bash
        if comando(s)_prueba; then
            comandos_si_exito
        elif comando(s)_prueba2; then
            comandos_si_exito2
        else
            comandos_si_fallo
        fi
        ```
    *   `if` ejecuta `comando(s)_prueba`. Si el *último* comando de esa lista sale con `0` (éxito), ejecuta el bloque `then`.
    *   `elif` (else if) es opcional, permite probar condiciones adicionales si la anterior falló.
    *   `else` es opcional, se ejecuta si todas las pruebas `if`/`elif` fallaron.
    *   `fi` marca el final del bloque `if`.
*   **Comando `test` (y `[`):**
    *   Comando usado comúnmente con `if` para evaluar expresiones y devolver un estado de salida.
    *   Sintaxis: `test expresión` o `[ expresión ]` (¡Espacios alrededor de `[` y `]` son **obligatorios**!).
    *   Devuelve 0 (éxito) si la expresión es verdadera, 1 (fallo) si es falsa.
    *   **Expresiones de Archivo:**
        *   `-e archivo`: Existe.
        *   `-f archivo`: Es un archivo regular.
        *   `-d archivo`: Es un directorio.
        *   `-h` o `-L archivo`: Es un enlace simbólico.
        *   `-r archivo`: Es legible.
        *   `-w archivo`: Es escribible.
        *   `-x archivo`: Es ejecutable (o se puede buscar en él si es directorio).
        *   `-s archivo`: Existe y tamaño > 0.
        *   `archivo1 -nt archivo2`: arch1 más nuevo que arch2.
        *   `archivo1 -ot archivo2`: arch1 más viejo que arch2.
    *   **Expresiones de Cadena:**
        *   `-z cadena`: Longitud cero (vacía).
        *   `-n cadena`: Longitud no cero (no vacía).
        *   `cadena1 = cadena2`: Son iguales (o `==`).
        *   `cadena1 != cadena2`: No son iguales.
        *   `cadena1 < cadena2`: cadena1 ordena antes que cadena2 (¡Escapar `<`!).
        *   `cadena1 > cadena2`: cadena1 ordena después que cadena2 (¡Escapar `>`!).
    *   **Expresiones Enteras:**
        *   `int1 -eq int2`: Equal (igual).
        *   `int1 -ne int2`: Not Equal (no igual).
        *   `int1 -lt int2`: Less Than (menor que).
        *   `int1 -le int2`: Less or Equal (menor o igual que).
        *   `int1 -gt int2`: Greater Than (mayor que).
        *   `int1 -ge int2`: Greater or Equal (mayor o igual que).
    *   **Importante:** Siempre entrecomillar variables dentro de `test`/`[` (`"$VAR"`) para evitar errores si están vacías o contienen espacios.
*   **Versión Moderna de `test`: `[[ ]]`**
    *   Comando compuesto de `bash` (más robusto y con más características que `test`).
    *   Sintaxis: `[[ expresión ]]` (Espacios después de `[[` y antes de `]]` requeridos).
    *   Soporta todas las expresiones de `test`.
    *   **Ventajas:**
        *   No requiere comillas para variables (aunque sigue siendo buena práctica).
        *   No realiza separación de palabras ni expansión de nombres de ruta en las variables.
        *   Operadores `<` y `>` no necesitan escape.
        *   Usa `==` para comparación de cadenas, que soporta **coincidencia de patrones** (como comodines). Ej: `[[ $VAR == *.txt ]]`.
        *   Nuevo operador `=~`: Coincidencia con **expresión regular extendida (ERE)**. Ej: `[[ $VAR =~ ^[0-9]+$ ]]`.
        *   Operadores lógicos `&&` (AND), `||` (OR), `!` (NOT) se usan *dentro* de `[[ ]]`.
*   **Aritmética Entera: `(( ))`**
    *   Comando compuesto para evaluación aritmética.
    *   Sintaxis: `(( expresión_aritmética ))`.
    *   Devuelve 0 (éxito) si la expresión es **no-cero** (verdadera aritméticamente).
    *   Devuelve 1 (fallo) si la expresión es **cero** (falsa aritméticamente).
    *   Usa sintaxis similar a C: `<`, `>`, `<=`, `>=`, `==`, `!=`, `&&`, `||`, `!`.
    *   No requiere `$` para las variables dentro. Ej: `(( var1 < var2 ))`.
*   **Combinar Expresiones:**
    *   `test`: `-a` (AND), `-o` (OR), `!` (NOT), `\( ... \)` (agrupar, escapar paréntesis).
    *   `[[ ]]` y `(( ))`: `&&` (AND), `||` (OR), `!` (NOT), `( ... )` (agrupar, sin escapar).
*   **Operadores de Control (`&&` y `||` fuera de `[[/((`):**
    *   `comando1 && comando2`: Ejecuta `comando2` **solo si** `comando1` tuvo éxito (exit status 0).
    *   `comando1 || comando2`: Ejecuta `comando2` **solo si** `comando1` falló (exit status != 0).
    *   Útil para encadenamiento condicional simple y manejo de errores. Ej: `mkdir dir && cd dir` o `[ -f file ] || echo "Falta archivo"`.

### Capítulo 28: LEYENDO ENTRADA DEL TECLADO

*   Hacer scripts interactivos.
*   **Comando `read` (Builtin):**
    *   Lee una línea de la entrada estándar (stdin) y la asigna a una o más variables.
    *   Sintaxis: `read [opciones] [variable1 variable2 ...]`
    *   Si no se especifican variables, la línea entera se guarda en la variable `$REPLY`.
    *   Si se especifican variables, `read` divide la línea de entrada en campos (usando espacios/tabs/newlines del `IFS` como delimitadores) y asigna cada campo a una variable consecutiva. Si hay más campos que variables, el resto de la línea va a la última variable. Si hay menos campos, las variables sobrantes quedan vacías.
*   **Opciones de `read`:**
    *   `-p "prompt"`: Muestra el texto `prompt` antes de esperar la entrada (sin newline al final). Más limpio que usar `echo -n`.
    *   `-n num`: Lee solo `num` caracteres.
    *   `-t segundos`: Establece un tiempo límite de `segundos`. Si se excede, `read` falla (exit status > 0).
    *   `-s`: Modo silencioso (no muestra los caracteres tecleados). Útil para contraseñas.
    *   `-a array`: Lee los campos en un array (Cap 35).
    *   `-d delimitador`: Termina de leer al encontrar `delimitador` en lugar de newline.
    *   `-e`: Usa Readline para permitir edición en la línea de entrada (como en el prompt).
    *   `-i "texto"`: (Usar con `-e`). Ofrece `texto` como entrada por defecto si el usuario solo presiona Enter.
    *   `-r`: Modo Raw (no interpreta `\` como escape).
*   **Variable `IFS` (Internal Field Separator):**
    *   Variable especial que contiene los caracteres que `bash` usa para dividir líneas en campos (por defecto: espacio, tabulador, nueva línea).
    *   Se puede cambiar temporalmente para `read` para procesar datos con otros delimitadores.
    *   Ejemplo para leer campos separados por `:` de `/etc/passwd`:
        ```bash
        IFS=":" read user pw uid gid name home shell <<< "$linea_passwd"
        ```
    *   Forma `IFS=":" comando ...`: Cambia `IFS` solo para la ejecución de `comando`.
*   **Limitación de `read` en Tuberías:**
    *   Un comando como `echo "valor" | read variable` **no funciona** como se espera. La variable *fuera* de la tubería no se asigna.
    *   Esto ocurre porque cada parte de una tubería se ejecuta en una *subshell* (una copia del entorno de la shell). La variable se asigna en la subshell, pero esta se destruye al terminar, perdiéndose la asignación.
    *   **Soluciones:**
        *   Usar "Here Strings": `read variable <<< "valor"`.
        *   Usar Sustitución de Procesos: `read variable < <(echo "valor")`.
        *   Agrupar comandos (ver Cap 36).
*   **Validación de Entrada:**
    *   ¡Esencial para scripts robustos! Comprobar siempre la entrada del usuario.
    *   Usar `if`, `[[ ]]`, expresiones regulares (`=~`), etc., para verificar que la entrada sea del tipo y formato esperados (ej: numérico, no vacío, longitud correcta, patrón específico).
    *   Mostrar mensajes de error claros (a stderr: `>&2`) y salir con estado de error (`exit 1`) si la entrada es inválida.
*   **Menús:**
    *   Presentar opciones numeradas o con letras al usuario.
    *   Usar `read` para obtener la selección.
    *   Usar `if`/`elif`/`else` (o `case`, Cap 31) para actuar según la selección.

### Capítulo 29: CONTROL DE FLUJO: BUCLES CON WHILE/UNTIL

*   **Bucles (Looping):** Ejecutar repetidamente un bloque de código.
*   **Comando `while`:**
    *   Ejecuta un bloque de comandos *mientras* un comando de prueba tenga éxito (exit status 0).
    *   **Sintaxis:**
        ```bash
        while comando(s)_prueba; do
            comandos_del_bucle
        done
        ```
    *   El bucle termina cuando `comando(s)_prueba` falla (exit status != 0).
    *   Ejemplo contador:
        ```bash
        count=1
        while [[ "$count" -le 5 ]]; do
            echo "$count"
            count=$((count + 1)) # O ((count++))
        done
        ```
*   **Comando `until`:**
    *   Ejecuta un bloque de comandos *hasta que* un comando de prueba tenga éxito (exit status 0). Es lo contrario de `while`.
    *   **Sintaxis:**
        ```bash
        until comando(s)_prueba; do
            comandos_del_bucle
        done
        ```
    *   El bucle termina cuando `comando(s)_prueba` tiene éxito (exit status 0).
    *   Ejemplo contador (equivalente al `while` anterior):
        ```bash
        count=1
        until [[ "$count" -gt 5 ]]; do
            echo "$count"
            ((count++))
        done
        ```
*   **Comandos `break` y `continue`:**
    *   Se usan *dentro* de bucles (`while`, `until`, `for`).
    *   `break`: Termina **inmediatamente** el bucle actual. La ejecución continúa con el comando *después* de `done`.
    *   `continue`: Salta el resto de los comandos de la iteración *actual* del bucle y comienza la **siguiente** iteración.
*   **Bucles Infinitos:**
    *   Un bucle cuya condición de prueba siempre es verdadera (`while true`) o siempre falsa (`until false`).
    *   Requieren un `break` interno para poder salir.
    *   Común para menús que se repiten hasta que el usuario elige "salir".
*   **Leer Archivos con Bucles:**
    *   Se puede redirigir la entrada de un archivo a un bucle `while` o `until`.
    *   Usar `read` dentro del bucle para procesar el archivo línea por línea.
    *   Sintaxis:
        ```bash
        while read var1 var2 ...; do
            # Procesar variables
        done < archivo.txt
        ```
    *   `read` devolverá éxito (0) mientras lea líneas, y fallo (>0) al llegar al final del archivo (EOF), terminando así el bucle `while`.
    *   También se puede usar con tuberías: `comando | while read ...; do ... done`. **Recordar** que esto ejecuta el bucle `while` en una subshell, las variables asignadas dentro se pierden al salir.

### Capítulo 30: RESOLUCIÓN DE PROBLEMAS (TROUBLESHOOTING)

*   Diagnóstico y corrección de errores en scripts.
*   **Errores Sintácticos:**
    *   Errores en la estructura del lenguaje de la shell (palabras clave mal escritas, comandos incompletos, comillas sin cerrar).
    *   La shell detecta estos errores y detiene la ejecución, a menudo mostrando un mensaje de error.
    *   **Causas Comunes:**
        *   **Comillas Faltantes:** Ej: `echo "Hola` (sin comilla de cierre). El error puede reportarse mucho después, al encontrar la siguiente comilla o el fin del script. Usar editor con resaltado de sintaxis ayuda.
        *   **Tokens Faltantes o Inesperados:** Ej: Olvidar `;` o `then` en `if`, `do` o `done` en bucles, `fi` al final de `if`, `esac` al final de `case`. El error puede indicar una línea posterior al problema real.
        *   **Expansiones Inesperadas:** Variables vacías o con espacios dentro de `test`/`[` sin comillas. Ej: `if [ $var = val ]` falla si `$var` está vacía. **Solución:** Usar comillas `if [ "$var" = val ]`. **Regla:** Casi siempre entrecomillar variables y sustituciones de comandos. Usar `[[ ]]` es más robusto contra esto.
*   **Errores Lógicos:**
    *   El script se ejecuta sin errores de sintaxis, pero no hace lo que se esperaba (produce resultados incorrectos).
    *   **Causas Comunes:**
        *   **Expresiones Condicionales Incorrectas:** Lógica `if`/`elif`/`else` mal planteada (condiciones erróneas, invertidas, incompletas).
        *   **Errores "Off-by-One":** Bucles que iteran una vez de más o de menos, a menudo por empezar/terminar el contador incorrectamente (ej: empezar en 1 vs 0).
        *   **Situaciones Imprevistas:** El script no maneja todos los posibles casos de entrada o condiciones del sistema (ej: archivo no existe, permisos incorrectos, nombre de archivo con caracteres raros).
*   **Programación Defensiva:**
    *   Anticipar problemas y escribir código para manejarlos.
    *   **Verificar Estados de Salida:** Comprobar `$?` después de comandos críticos. Usar `&&` y `||` para encadenamiento condicional.
    *   **Validar Supuestos:** No asumir que un comando tuvo éxito o que una variable contiene lo esperado. Comprobar existencia de archivos/directorios (`-e`, `-d`), permisos (`-r`, `-w`), etc.
    *   **Manejo de Errores:** Si una condición necesaria falla, mostrar un mensaje de error descriptivo (a `stderr`: `>&2`) y terminar el script con un estado de salida de error (`exit 1`).
    *   **Cuidado con Nombres de Archivo:** Nombres que empiezan con `-` pueden ser interpretados como opciones por comandos como `rm`. Usar `./*` en lugar de `*` para forzar que se traten como nombres de ruta. Considerar usar solo caracteres portables (POSIX Portable Filename Character Set: A-Z, a-z, 0-9, ., -, _).
    *   **Validar Entradas:** Comprobar rigurosamente cualquier entrada del usuario o datos externos (ver Cap 28).
*   **Pruebas (Testing):**
    *   Ejecutar el script en diversas condiciones para encontrar errores.
    *   **Casos de Prueba:** Diseñar entradas y escenarios que cubran:
        *   Funcionamiento normal.
        *   Valores límite (mínimos, máximos).
        *   Casos extremos o raros (edge/corner cases).
        *   Entradas inválidas.
    *   **Pruebas Seguras:** Al probar código destructivo (ej: `rm`), reemplazar el comando peligroso con `echo` para ver qué *haría* sin ejecutarlo. Añadir comentarios (`# TESTING`) para marcar el código de prueba. Usar `exit` para detener la ejecución después del fragmento probado.
*   **Depuración (Debugging):** Proceso de encontrar y corregir errores.
    *   **Aislar el Problema:** "Comentar" bloques de código (`#` al inicio de cada línea) para desactivarlos temporalmente y ver si el error persiste, ayudando a localizar la sección problemática.
    *   **Trazado (Tracing):** Ver el flujo de ejecución del script.
        *   **Mensajes Manuales:** Insertar comandos `echo "Mensaje debug..." >&2` en puntos clave para ver si se alcanzan y qué valores tienen las variables (`echo "VAR=$VAR" >&2`).
        *   **Trazado de Bash:**
            *   Ejecutar el script con `bash -x mi_script.sh`.
            *   Añadir `-x` al shebang: `#!/bin/bash -x`.
            *   Activar/desactivar trazado dentro del script: `set -x` (activar), `set +x` (desactivar).
            *   Muestra cada comando *después* de las expansiones, precedido por el valor de `$PS4` (por defecto `+`).
            *   Personalizar `PS4` para más info: `export PS4='$LINENO + '` (muestra número de línea).

### Capítulo 31: CONTROL DE FLUJO: BIFURCACIÓN CON CASE

*   **Comando `case`:** Alternativa a `if`/`elif`/`else` para manejar múltiples opciones basadas en el valor de una palabra (variable).
*   **Sintaxis:**
    ```bash
    case palabra in
        patrón1)
            comandos1
            ;;
        patrón2 | patrón3) # Patrones múltiples con | (OR)
            comandos2
            ;;
        patrónN)
            comandosN
            ;;
        *) # Patrón comodín (opcional, captura todo lo demás)
            comandos_por_defecto
            ;;
    esac
    ```
*   **Funcionamiento:**
    1.  `palabra` (usualmente una variable expandida, ej: `"$REPLY"`) se evalúa.
    2.  Se compara con `patrón1`. Si coincide, se ejecutan `comandos1` hasta `;;`.
    3.  Si no coincide con `patrón1`, se compara con `patrón2` (o `patrón3`). Si coincide, se ejecutan `comandos2` hasta `;;`.
    4.  Continúa hasta encontrar una coincidencia.
    5.  `;;` es crucial: termina la ejecución de los comandos para ese caso y **sale** del `case`. Sin `;;`, caería al siguiente bloque de comandos (raramente deseado en shell).
    6.  `*)` es un patrón especial que coincide con cualquier cosa. Se usa como último caso para manejar valores inesperados o como opción por defecto.
    7.  `esac` (case al revés) finaliza el comando `case`.
*   **Patrones:**
    *   Usan la misma sintaxis que la **expansión de nombres de ruta** (comodines), **no** expresiones regulares.
    *   `*`, `?`, `[...]`, `[!...]`, `[[:clase:]]`.
    *   `patrón1 | patrón2`: Coincide si `palabra` casa con `patrón1` **O** `patrón2`. Útil para opciones equivalentes (ej: `y|Y` para sí/Yes).
*   **Ventajas sobre `if`/`elif`:** Más legible y a menudo más eficiente cuando se comparan múltiples valores exactos o patrones simples de una sola variable.
*   **Bash 4.0+ Nuevas Terminaciones:**
    *   `;;&`: Después de ejecutar los comandos, **continúa** probando los siguientes patrones (permite múltiples coincidencias).
    *   `;;&`: Después de ejecutar los comandos, **salta** al siguiente patrón *después* de uno que use `|` (más complejo, ver `man bash`).

### Capítulo 32: PARÁMETROS POSICIONALES

*   Variables especiales que contienen los argumentos pasados a un script (o función) en la línea de comandos.
*   **Acceso a Parámetros:**
    *   `$0`: Contiene el nombre (ruta) del script mismo.
    *   `$1`, `$2`, `$3`, ... `$9`: Contienen el primer, segundo, tercer... noveno argumento.
    *   `${10}`, `${11}`, ...: Para acceder a argumentos más allá del noveno, usar llaves `{}`.
*   **Variables Especiales Relacionadas:**
    *   `$#`: Contiene el **número** total de argumentos posicionales (sin contar `$0`).
*   **Comando `shift`:**
    *   Descarta `$1` y mueve todos los demás parámetros una posición hacia abajo (`$2` se convierte en `$1`, `$3` en `$2`, etc.).
    *   Decrementa `$#` en uno.
    *   Permite procesar un número arbitrario de argumentos iterando mientras `$#` sea mayor que cero y procesando `$1` en cada paso, seguido de `shift`.
*   **Aplicaciones Simples:** Scripts que toman uno o dos argumentos fijos (ej: nombre de archivo).
*   **Uso en Funciones de Shell:** Las funciones también reciben sus propios parámetros posicionales (`$1`, `$2`, etc.) cuando se les pasan argumentos al llamarlas. `$0` dentro de una función sigue siendo el nombre del script principal. La variable `$FUNCNAME` contiene el nombre de la función actual.
*   **Manejo de Todos los Parámetros a la Vez:**
    *   `$*`: Se expande a una **única palabra** que contiene todos los parámetros posicionales (desde `$1`), separados por el *primer* carácter de `$IFS` (usualmente espacio).
    *   `$@`: Se expande a **palabras separadas**, donde cada palabra es un parámetro posicional (desde `$1`).
    *   **Diferencia Clave (al entrecomillar):**
        *   `"$*"`: Se expande a **una única cadena** `"arg1 IFS arg2 IFS arg3..."`. Pierde la separación original si los argumentos tenían espacios.
        *   `"$@"`: Se expande a **múltiples cadenas separadas**, donde cada cadena es un parámetro posicional original, preservando los espacios dentro de cada argumento: `"arg1" "arg2 con espacios" "arg3" ...`.
    *   **Regla General:** Usar `"$@"` casi siempre es lo correcto para pasar argumentos a otros comandos o funciones, ya que preserva la integridad de cada argumento individual.
*   **Aplicación al Proyecto `sys_info_page`:** Se añade procesamiento de opciones de línea de comandos (`-f archivo`, `-i`, `-h`) usando un bucle `while [[ -n "$1" ]]`, `case "$1" in ... esac`, y `shift`.

### Capítulo 33: CONTROL DE FLUJO: BUCLES CON FOR

*   Otro tipo de bucle, diseñado para iterar sobre una secuencia de elementos (palabras).
*   **Forma Tradicional de `for`:**
    *   **Sintaxis:**
        ```bash
        for variable [in lista_de_palabras]; do
            comandos
        done
        ```
    *   El bucle itera una vez por cada palabra en `lista_de_palabras`.
    *   En cada iteración, la palabra actual se asigna a `variable`.
    *   `lista_de_palabras` puede ser:
        *   Una lista literal: `for i in A B C; do ...`
        *   Una expansión de llaves: `for i in {1..5}; do ...`
        *   Una expansión de nombres de ruta (comodín): `for i in *.txt; do ...` (**¡Cuidado!** si no hay coincidencias, `i` tomará el valor literal del patrón, ej: `*.txt`. Comprobar existencia con `[[ -e "$i" ]]` dentro).
        *   Una sustitución de comandos: `for i in $(comando); do ...` (**¡Precaución!** La salida de `comando` sufrirá separación de palabras por `IFS`. Si la salida contiene espacios/newlines no deseados, puede no funcionar como se espera. Usar `while read` es a menudo más seguro para procesar salida línea por línea).
        *   `$*` o `$@` (ver Cap 32).
    *   **Si `in lista_de_palabras` se omite:** El bucle itera sobre los **parámetros posicionales** (`"$@"` implícito). `for variable; do ...`.
*   **Forma Estilo C de `for`:**
    *   Introducida en versiones más recientes de `bash`. Similar a `for` en C/Java/etc.
    *   **Sintaxis:**
        ```bash
        for (( expr_inicial; expr_condicion; expr_incremento )); do
            comandos
        done
        ```
    *   Usa **expresiones aritméticas** (como `(( ))`). No requiere `$` para variables.
    *   `expr_inicial`: Se ejecuta una vez al principio (ej: `i=0`).
    *   `expr_condicion`: Se evalúa antes de cada iteración. Si es no-cero (verdadera), el bucle continúa. Si es cero (falsa), el bucle termina (ej: `i<10`).
    *   `expr_incremento`: Se ejecuta al final de cada iteración (ej: `i++` o `i=i+1`).
    *   Muy útil para bucles con contadores numéricos.
*   **Aplicación al Proyecto `sys_info_page`:** Se reescribe la función `report_home_space` usando `for` para iterar sobre los directorios home (obtenidos de una variable que contiene un patrón o el `$HOME`) y usar `find`, `du`, `printf` para generar un informe más detallado por usuario.

### Capítulo 34: CADENAS Y NÚMEROS

*   Manipulación de datos a nivel de cadenas (strings) y números enteros.
*   **Expansión de Parámetros (Avanzada):**
    *   **Formas Básicas (repaso):** `$param`, `${param}` (llaves para desambiguar).
    *   **Manejo de Variables Vacías/No Definidas:**
        *   `${param:-palabra}`: Usa `palabra` si `param` está vacío/no definido, sino usa `param`. **No** cambia `param`.
        *   `${param:=palabra}`: Usa `palabra` si `param` está vacío/no definido, **y asigna** `palabra` a `param`. Sino usa `param`.
        *   `${param:?palabra}`: Si `param` está vacío/no definido, muestra `palabra` como error en stderr y sale del script. Sino usa `param`.
        *   `${param:+palabra}`: Si `param` **no** está vacío/no definido, usa `palabra`. Si está vacío/no definido, usa nada (cadena vacía).
    *   **Longitud:** `${#param}`: Devuelve la longitud de la cadena en `param`. Si `param` es `@` o `*`, devuelve el número de parámetros posicionales.
    *   **Extracción de Subcadenas:**
        *   `${param:offset}`: Extrae desde `offset` hasta el final.
        *   `${param:offset:longitud}`: Extrae `longitud` caracteres desde `offset`.
        *   `offset` negativo (requiere espacio antes: `${param: -5}`) cuenta desde el final.
    *   **Eliminación de Patrones (Prefijo):**
        *   `${param#patrón}`: Elimina la coincidencia más **corta** de `patrón` (comodín) desde el **principio**.
        *   `${param##patrón}`: Elimina la coincidencia más **larga** de `patrón` desde el **principio**. (Útil para quitar rutas: `"${ruta##*/}"` -> nombre base).
    *   **Eliminación de Patrones (Sufijo):**
        *   `${param%patrón}`: Elimina la coincidencia más **corta** de `patrón` desde el **final**. (Útil para quitar extensiones: `"${archivo%.*}"`).
        *   `${param%%patrón}`: Elimina la coincidencia más **larga** de `patrón` desde el **final**.
    *   **Sustitución de Patrones:**
        *   `${param/patrón/cadena}`: Reemplaza la **primera** coincidencia de `patrón` con `cadena`.
        *   `${param//patrón/cadena}`: Reemplaza **todas** las coincidencias de `patrón` con `cadena`.
        *   `${param/#patrón/cadena}`: Reemplaza solo si `patrón` coincide al **principio**.
        *   `${param/%patrón/cadena}`: Reemplaza solo si `patrón` coincide al **final**.
        *   Si `/cadena` se omite, se borra la coincidencia.
    *   **Conversión de Mayúsculas/Minúsculas (Bash 4.0+):**
        *   `${param,,}`: Todo a minúsculas.
        *   `${param,}`: Primera letra a minúsculas.
        *   `${param^^}`: Todo a mayúsculas.
        *   `${param^}`: Primera letra a mayúsculas (Capitalize).
        *   Se puede añadir `patrón` opcional para limitar qué caracteres convertir.
    *   **Expansiones de Nombres de Variable:**
        *   `${!prefijo*}` o `${!prefijo@}`: Expande a los *nombres* de las variables que empiezan con `prefijo`.
*   **Evaluación y Expansión Aritmética (Repaso):**
    *   `$((expresión))`: Expansión aritmética (devuelve el resultado del cálculo).
    *   `((expresión))`: Comando compuesto para evaluación aritmética (devuelve estado de salida 0 si no-cero, 1 si cero).
    *   **Bases Numéricas:** `N` (decimal), `0N` (octal), `0xN` (hex), `BASE#N`.
    *   **Operadores:** `+`, `-`, `*`, `/`, `%`, `**`. Unarios `+`, `-`.
    *   **Asignación:** `=`, `+=`, `-=`, `*=`, `/=`, `%=`.
    *   **Incremento/Decremento:** `var++` (post), `++var` (pre), `var--` (post), `--var` (pre). Pre-incremento es usualmente más intuitivo.
    *   **Operadores a Nivel de Bit:** `~` (NOT), `<<` (left shift), `>>` (right shift), `&` (AND), `|` (OR), `^` (XOR).
    *   **Operadores Lógicos/Comparación (dentro de `(( ))`):** `<=`, `>=`, `<`, `>`, `==`, `!=`, `&&`, `||`, `!`. Ternario `cond ? val_si : val_no`.
*   **`bc` - Calculadora de Precisión Arbitraria:**
    *   Herramienta externa para cálculos con **punto flotante** o alta precisión.
    *   Lenguaje propio similar a C.
    *   `bc [opciones] [archivo_script...]`. `-q` (quiet, sin banner).
    *   Lee de stdin si no se dan archivos. Útil con `<<<` (here strings) o `<<` (here documents).
    *   Variable especial `scale=N`: Define el número de decimales para los resultados.
    *   Ej: `resultado=$(echo "scale=4; 10 / 3" | bc)` o `resultado=$(bc <<< "scale=4; 10 / 3")`.

### Capítulo 35: ARRAYS

*   **Array:** Variable que puede almacenar múltiples valores.
*   En `bash`, los arrays son **unidimensionales**.
*   **Arrays Indexados (Numéricos):**
    *   Índices son enteros no negativos (empiezan en `0`).
    *   **Creación/Asignación:**
        *   Elemento individual: `nombre_array[indice]=valor` (ej: `mi_array[0]="A"`, `mi_array[1]="B"`).
        *   Múltiples elementos: `nombre_array=(valor0 valor1 valor2 ...)` (los índices se asignan secuencialmente desde 0).
        *   Múltiples con índice explícito: `nombre_array=([0]=val0 [2]=val2 [5]=val5 ...)`. Permite índices dispersos.
    *   **Acceso a Elementos:**
        *   Elemento individual: `${nombre_array[indice]}` (¡Las llaves `{}` son **obligatorias**!).
        *   Todos los elementos: `${nombre_array[*]}` (como una sola palabra) o `${nombre_array[@]}` (como palabras separadas). **Usar `"${nombre_array[@]}"`** es lo más seguro para iterar, preserva elementos con espacios.
    *   **Obtener Índices:** `${!nombre_array[*]}` o `${!nombre_array[@]}`. Devuelve la lista de índices *en uso*. `"${!nombre_array[@]}"` es la forma segura.
    *   **Obtener Número de Elementos:** `${#nombre_array[*]}` o `${#nombre_array[@]}`.
    *   **Obtener Longitud de un Elemento:** `${#nombre_array[indice]}`.
    *   **Añadir Elementos al Final:** `nombre_array+=(valorN valorN+1 ...)`.
    *   **Eliminar Elemento:** `unset 'nombre_array[indice]'` (¡Comillas necesarias!).
    *   **Eliminar Array Completo:** `unset nombre_array`.
    *   **Referencia sin Índice:** `echo $nombre_array` equivale a `echo ${nombre_array[0]}`.
*   **Arrays Asociativos (Bash 4.0+):**
    *   Índices son **cadenas** (strings) en lugar de números. Similar a diccionarios/hashes en otros lenguajes.
    *   **Declaración Obligatoria:** `declare -A nombre_array_asoc`.
    *   **Asignación:** `nombre_array_asoc[indice_cadena]=valor`.
    *   **Acceso:** `${nombre_array_asoc[indice_cadena]}`.
    *   Las operaciones como `${array[@]}`, `${!array[@]}`, `${#array[@]}` funcionan igual.
    *   Muy útil para contadores basados en nombres, como en el script `array-2`.
*   **Operaciones Comunes (Implementadas con código):**
    *   **Ordenar Array:** No hay comando directo. Técnica común:
        ```bash
        array_ordenado=($(for i in "${mi_array[@]}"; do echo "$i"; done | sort))
        ```
        Expande cada elemento en una línea, ordena las líneas, y captura la salida ordenada en un nuevo array.

### Capítulo 36: EXÓTICA

*   Características menos comunes pero útiles de `bash`.
*   **Comandos Agrupados (Group Command):**
    *   Sintaxis: `{ comando1; comando2; ...; }` (¡Espacio después de `{` y antes de `}`!, ¡`;` o newline antes de `}`!).
    *   Ejecuta los comandos en la **shell actual**.
    *   Útil para aplicar **redirección** a un bloque de comandos.
    *   Ej: `{ comando1; comando2; } > archivo_salida` o `{ comando1; comando2; } | otro_comando`.
*   **Subshells:**
    *   Sintaxis: `(comando1; comando2; ...)`
    *   Ejecuta los comandos en una **copia (hija) de la shell actual**.
    *   También útil para redirección de bloques.
    *   **Diferencia clave con Group Command:** Cambios en el entorno (variables, directorio actual) hechos *dentro* de la subshell **no** afectan a la shell padre. Se pierden al terminar la subshell.
    *   Generalmente, preferir Group Commands `{}` si no se necesita el aislamiento de la subshell (son más eficientes).
*   **Sustitución de Procesos (Process Substitution):**
    *   Permite tratar la salida o entrada de un proceso (o lista de comandos) como si fuera un archivo temporal.
    *   Formas:
        *   `<(lista_comandos)`: La salida de `lista_comandos` puede ser leída como un archivo.
        *   `>(lista_comandos)`: La salida redirigida aquí se envía como entrada a `lista_comandos`.
    *   Expande a un nombre de archivo especial (ej: `/dev/fd/63`).
    *   **Solución al problema `echo "val" | read var`:**
        ```bash
        read var < <(echo "val")
        ```
        Aquí, `read` no está en una subshell directa de tubería, por lo que la asignación a `var` persiste.
    *   Útil para comparar salidas de comandos con `diff <(cmd1) <(cmd2)` o para alimentar bucles `while read` sin subshells: `while read ...; do ... done < <(comando)`.
*   **Trampas (Traps):**
    *   Permiten ejecutar comandos cuando la shell recibe una **señal** específica.
    *   Útil para limpieza (ej: borrar archivos temporales) si el script es interrumpido (`CTRL-C`), terminado, etc.
    *   Sintaxis: `trap 'comando(s)' SEÑAL1 SEÑAL2 ...`
    *   `comando(s)`: Cadena que se interpreta como comandos, o el nombre de una función de shell.
    *   `SEÑAL`: Nombre de la señal (ej: `SIGINT` para `CTRL-C`, `SIGTERM`, `SIGQUIT`) o número, o `EXIT` (se ejecuta al salir del script, normal o por señal).
    *   Ejemplo simple: `trap "echo 'Interrumpido!'" SIGINT`
    *   Ejemplo con función de limpieza:
        ```bash
        cleanup() {
            echo "Limpiando archivos temporales..."
            rm -f /tmp/mi_script.*
        }
        trap cleanup EXIT # Ejecuta cleanup al salir
        ```
    *   Para ignorar una señal: `trap '' SEÑAL` (ej: `trap '' SIGINT`).
    *   Para restaurar manejo por defecto: `trap - SEÑAL`.
*   **Archivos Temporales Seguros:**
    *   No usar nombres predecibles en `/tmp` (vulnerable a ataques "temp race").
    *   Mala idea: `/tmp/mi_script.tmp`.
    *   Mejor: `/tmp/$(basename "$0").$$.$RANDOM` (usa nombre script, PID `$$`, número aleatorio). `$RANDOM` tiene rango limitado.
    *   **Mejor:** Usar `mktemp`. Crea un archivo (o directorio con `-d`) con nombre único y seguro, y devuelve el nombre.
        *   `mktemp /tmp/mi_prefijo.XXXXXXXXXX` (Las `X` se reemplazan por caracteres aleatorios).
        *   Guardar el nombre: `temp_file=$(mktemp /tmp/mi_script.XXXXXXXX)`
        *   Usar con `trap` para borrarlo al salir:
            ```bash
            temp_file=$(mktemp ...)
            trap 'rm -f "$temp_file"' EXIT
            # ... usar $temp_file ...
            ```
*   **Ejecución Asíncrona y `wait`:**
    *   Lanzar procesos en segundo plano (`comando &`).
    *   `$!`: Variable especial que contiene el PID del último proceso puesto en segundo plano.
    *   **Comando `wait [PID]`:** Pausa el script padre hasta que el proceso con `PID` especificado (usualmente un hijo lanzado en background) termine. Si no se da PID, espera a *todos* los hijos en background.
    *   Permite coordinar tareas paralelas.
*   **Tuberías Nombradas (Named Pipes / FIFO):**
    *   Tipo especial de archivo (`p` en `ls -l`) que actúa como una tubería persistente en el sistema de archivos.
    *   Creada con `mkfifo nombre_pipe`.
    *   Un proceso escribe en ella (`comando > nombre_pipe`), otro lee de ella (`comando < nombre_pipe`).
    *   Se bloquean si no hay lector/escritor en el otro extremo.
    *   Permiten comunicación FIFO entre procesos no relacionados directamente por tuberías `|`. Menos común hoy en día.
