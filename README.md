# Bash_Style_Guide
Mi guía de estilos para Bash (Español)

## Espacios en vez de tabulaciones
- Usar 4 espacios y nunca usar tabulaciones.

## Limitación de columnas
- No exceder de los 80 carácteres cada línea.

## Finales de línea
- No usar punto y coma en finales de línea.
- El punto y coma **";"** se puede usar para separar órdenes, no para terminar una línea.
'''bash
    # MAL:
    echo 'Hola mundo';

    # BIEN:
    echo 'Hola mundo'
'''

## Funciones
- Las funciones no llevarán la palabra reservada **function**
- En todo momento se ha de procurar usar variables locales.
- Las variables globales se usarán lo menos posible.
- Una función no puede generar efectos colaterales y retornar (Una cosa u otra).
'''bash
    # MAL:
    function foo {
        i=foo # Variable declarada como global
    }

    # BIEN:
    foo() {
        local i=foo # Variable local a la función (preferible)
    }
'''
