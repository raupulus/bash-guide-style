# Bash_Style_Guide
Con esta guía de estilos personal pretendo estandarizar la creación de mis propios scripts para que vayan todos uniformes siguiendo unas reglas y un patrón.

![Guía de estilos Bash Logo](/Logo-Guía-de-estilos-bash.png)

El motivo de realizar esta guía es que no encontré una que me gustara lo suficiente, pero sobre todo al no encontrar ninguna estándar y bien aceptada por la comunidad como tal.

Posiblemente hayan guías mejores, soluciones mejores o partes que no gusten demasiado pero esta ha sido mi decisión.

En cualquier caso puedes sugerir cambios mediante **Issues** y serán debatidos o directamente aceptados si es alguna corrección demostrable.

Todo esto es orientativo

## Estructura de los scripts
Directrices comunes para todos los scripts en el orden de aparición. Puede apreciarse mejor aún con la plantilla de ejemplo de este repositorio https://github.com/fryntiz/Bash_Style_Guide/blob/master/plantilla.sh.
- Declaración de intérprete de comandos
- Codificación UTF-8 en un comentario
- Nombre de autor/es
- Contacto de autor/es
- Licencia del script (Recomiendo GPLv3)
- Instrucciones
- Importaciones de scripts o recursos mediante **"source"**
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
- El código para deshabilitarlo tendrá solo 1 símbolo **"#"**
- Los comentarios como tal tendrán dos símbolos **"##"** y estarán separados por un espacio desde carácter para comentar **"\#"**.
- Los comentarios en línea, es decir, detrás de código en la misma línea tendrá dos espacios de separación desde el código, luego dos **"##"** otro espacio y el comentario.
- Los comentarios en línea de código o variables relacionados pueden tener más de dos espacios para quedar igualados en altura.
```bash
    ## Este comentario describe lo que realiza la siguiente variable
    #variable='Variable Comentada'  ## Comentario en la misma línea
    #variable2='Comentario'         ## Otro comentario relacionado
    variable3='Comentario'          ## Este también
```

## Limitación de columnas
- No exceder de los 80 carácteres cada línea.

## Espacios entre filas
- No introducir 2 líneas vacías seguidas, es decir, no más de 1 línea en blanco entre bloque, instrucción, declaración o cualquier otro elemento
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
```

## Inicialización de variables
- Para iniciar variables sin saber aún que valor van a tomar usaremos los valores más pequeños (vacío, 0)
```bash
    my_input=''   # Cadena vacía, tipo String
    my_array=()   # Array vacío
    my_number=0   # Número vacío, tipo Integer
    my_float=0.0  # Número vacío, tipo Float
```

## Constantes
- Las constantes se declaran en mayúsculas
- Siempre deberán ser declaradas al principio del script, justo antes que las variables
- La sintaxis de una constante será la misma que para una variable y su diferencia será el uso

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
- El nombre de la función será camelCase, el cual empieza en minúsculas y cada palabra será capitalizada.
- El nombre de la función tiene que ser lo más descriptivo posible.
- El nombre de la función no puede ser igual que una variable, constante o comando.
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
## Declarar bloques con múltiples condiciones o rutas extensas
- Cuando declaremos un bloque el cual tendrá muchas condiciones, rutas extensas incluso si son muchos parámetros se insertarán en la siguiente línea.
- Un bucle while llevará cada condición alineada comenzando debajo de la declaración, tendrá un booleano por línea.
- Un bucle for llevará cada condición en una línea distinta de forma alineada pero terminando con una barra invertida.
- Un if con múltiples condiciones tendrá alineadas cada una de estas en otra línea terminando en el operador de concatenación como final de línea.
```bash
    # Bucle while con diversas condiciones
    while
        condición1
        condición2
        condición3
    do
        ...
    done

    # Bucle for con múltiples rutas → Debe llevar barra invertida al final
    for x in ruta/a/dir/ \
             ruta/a/dir1/ \
             ruta/a/dir2/ \
             ruta/a/dir3/
    do
        ...
    done

    # Condicional if con múltiples condiciones
    if  ( false |
          true )  &&
       [[ 2 > 1 ]]
    then
        echo 'Todo se ha cumplido'
    else
        echo 'No se ha cumplido algo'
    fi
```
