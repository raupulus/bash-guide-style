# Bash_Style_Guide
- Mi guía de estilos para Bash (Español)

## Decisiones en general
- Cuando sea necesario ejecutar una consola/terminal/intérprete nunca se usará enlaces como **"sh"** ya que puede apuntar a otro intérprete de órdenes distinto a bash (*Esta guía de estilos es para bash*)

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
```bash
    # Variable Global
    i=foo

    # Variable local
    local i=foo
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
