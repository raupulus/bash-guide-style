# Bash_Style_Guide
- Mi guía de estilos para Bash (Español)

## Estructura de los script
Directrices comunes para todos los scripts en el orden de aparición. Puede apreciarse mejor aún con la plantilla de ejemplo de este repositorio.
- Declaración de intérprete de comandos
- Codificación UTF-8 en un comentario
- Nombre de autor/es
- Contacto de autor/es
- Licencia del script (Recomiendo GPLv3)
- Instrucciones
- Constantes agrupando las que se relacionan deberán existir las siguientes:
    - WORKSCRIPT → Su valor será el directorio principal del script.
    - USER → Usuario
- Variables agrupando las que se relacionan
- Funciones
- Órdenes y funcionamiento del programa
- Salida correcta

## Decisiones en general
- Cuando sea necesario ejecutar una consola/terminal/intérprete nunca se usará enlaces como **"sh"** ya que puede apuntar a otro intérprete de órdenes distinto a bash (*Esta guía de estilos es para bash*).
- Evitar el uso de herramientas externas a bash siempre que sea posible.
- Usar comillas dobles solo cuando se necesite expandir una variable.
- Usar comillas simples en todo momento que sea posible y no haya que expandir variables.
- Nunca usar **eval** en el propio script ya que existen otras formas en el lenguaje
- En comandos/órdenes que puedan fallar controlar errores como por ejemplo **cd /some/path || exit**
- La codificación tiene que ser UTF-8

## Espacios en vez de tabulaciones
- Usar 4 espacios y nunca usar tabulaciones.

## Comentarios

## Limitación de columnas
- No exceder de los 80 carácteres cada línea.

## Espacios entre filas
- No introducir 2 líneas vacías seguidas, es decir, no más de 1 línea en blanco por fila
- Detrás de cada bloque habrá una línea en blanco
- Si se declaran variables o constantes globales solo habrá nuevas líneas cuando se vaya a separar variables que no se agrupen en relación con las anteriores, es decir, separar bloques de variables que no tengan relación con una línea en blanco de por medio pero solo en el caso que haya muchas variables

## Finales de línea
- No usar punto y coma en finales de línea.
- El punto y coma **";"** se puede usar para separar órdenes, no para terminar una línea.
```bash
    # MAL:
    echo 'Hola mundo';

    # BIEN:
    echo 'Hola mundo'
```

## Variables
- Las variables deberán siempre que sea posible pertenecer a un ámbito local
- Usar la cantidad justa de variables globales
- Declarar variables globales al principio del script
- Declarar las constantes globales antes de las varibles, al principio del script
- No usar **let** ni **readonly** para declarar variables
- Todas las variables deberán ir encerradas entre comillas dobles cuando se llamen incorporadas a un comando.
- Variables dentro de una condición a comprobar **[[ -d $variable ]]** no se encierran entre comillas
```bash
    # Variable Global
    i='foo'

    # Variable local
    local i='foo'

    # Llamar a la variable
    echo "La variable vale $i"
    echo "$i"

    # Variable como condición a comprobar
    if [[ -n $foo ]]; then   # No es necesario encerar entre comillas

    echo "$foo" # Es necesario encerrarlas entre comillas

    # Asignación a otra variable
    j=$foo # Tampoco es necesario encerrarla entre comillas
fi
```

## Condiciones y comprobaciones que devuelven boolean
- Se han de encerrar siempre en dobles corchentes "[[ test ]]"
- Debe existir un espacio entre la comprobación y el corchete "[[ -d dir ]]"
- No usar en ningún momento solo 1 corchete "[ test ]" ← MAL
```bash
    # MAL :
    if [ -d directorio ]; then
        ...
    fi

    # BIEN:
    if [[ -d directorio ]]; then
        ...
    fi
```

## Funciones
- Las funciones no llevarán la palabra reservada **function**
- En todo momento se ha de procurar usar variables locales.
- Las variables globales se usarán lo menos posible.
- Una función no puede generar efectos colaterales y retornar (Una cosa u otra).
```bash
    # MAL:
    function foo {
        i=foo # Variable declarada como global
    }

    # BIEN:
    foo() {
        local i=foo # Variable local a la función (preferible)
    }
```

## Declaraciones de bloque
- Las declaraciones de bloque separarán mediante **";"** el delimitador de bloque correspondiente (then, do...)
```bash
    # MAL :
    if true
    then
        ...
    fi

    true && {
        ...
    }

    # BIEN:
    # Condicional if
    if true; then
        ...
    fi

    # Bucle while
    while true; do
        ...
    done
```

## Secuencias
- Las secuencias se deben generar usando métodos propios de bash y no con aplicaciones externas o métodos que puedan ser específicos de un ámbito, distribución o requieran la instalación o uso de herramientas de terceros lo cual podría generar conflictos y disminuirá la compatibilidad.
```bash
    # MAL:
    for f in $(seq 1 10); do
        ...
    done

    # BIEN:
    for f in {1..10}; do
        ...
    done

    for ((i = 0; i < 10; i++)); do
        ...
    done
```

## Listar sin usar el comando **"ls"**
- No se debe usar el comando ls en bucles (loop) ya que es peligroso, en su lugar usar el selector asterisco "\*" en los ejemplos vemos como obtener el mismo resultado.
```bash
    # MAL:
    for f in $(ls); do
        ...
    done

    # BIEN:
    for f in *; do
        ...
    done
```

## Evitar usar el comando **"cat"** cuando no sea necesario
```bash
    # MAL:
    cat archivo | grep foo

    # BIEN:
    grep foo < archivo

    # Mucho mejor
    grep foo archivo
```

## Resultado de comando en variable
- Para asignar el resultado de un comando a una variable usar siempre **$(comando)**
```bash
    # MAL:
    foo=`whoami`

    # BIEN:
    foo=$(whoami)
```

## Arrays
- Usar arrays en vez de listas de cadenas separadas por espacios, líneas en blanco, tabulaciones...
- En muchas ocasiones trabajaremos incluso mejor con arrays y no necesitaremos bucles for si el comando admite múltiples argumentos aunque también podemos usarlos de esta forma.
```bash
    # MAL:
    modulos='modulo1 modulo2 modulo3'
    for modulo in $modulos; do
        install "$modulo"
    done

    # BIEN:
    modulos=(modulo1 modulo2 modulo3)
    for modulo in "${modulos[@]}"; do
        install "$modulo"
    done

    # AÚN MEJOR:
    modulos=(modulo1 modulo2 modulo3)
    install "${modulos[@]}" # Pasa todos los argumentos del array de una vez
```
