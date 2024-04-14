# Orientación Proyecto AC

## Materiales

Para la realización del proyecto se entregan junto con este informe los siguientes materiales:

* Este documento (`README.md`)
* `smips.pdf`
* Scripts de Python para el testeo automático(`asambler.py`, `test.py` y `price.py`)
* Dos archivos punto circ `s-mips-template.circ` y `s-mips.circ`.
* Carpeta `tests` conteniendo diferentes archivos `.asm`

## Orientación

El objetivo de este proyecto es que puedan implementar usando `Logisim` una arquitectura para interpretar las instrucciones de S-MIPS. Los detalles relacionados a esta implementación estan descritos en el archivo [`s-mips.pdf`](./s-mips.pdf).

## Estructura de la plantilla

La plantilla consta de 3 componentes principales:

* CPU
* RAM
* RAM Dispatcher

La RAM es la componente que simulará la memoria RAM de un computadora. Esta no requiere de ningún cambio para que el microprocesador funcione. La interfaz a travéz de la cuál se utilizá estará descrita en el [`s-mips.pdf`](./s-mips.pdf). El RAM Dispatcher está estrechamente relacionado con la RAM y es necesario para la ejecución automática de los tests.

La CPU es la componente la cuál usted deberá modificar para que interprete la instrucciones que cargue de la RAM y realice las operaciones correspondientes. Cualquier cambio fuera de esta componente será ignorado a la hora de hacer las revisiones por tanto limite cualquier cambio que considere importante a su interior.

## Tests Automáticos

El proyecto también incluye un conjunto de scripts en Python que permitirán realizar pruebas automaticas para conocer el estado actual del microprocesador y verificar que este funcione correctamente.

### Requisitos para utilizar los tests automáticos

Para poder ejecutar estos tests automáticos es necesario contar con los siguientes requisitos:

* Sistema Operativo Unix
* Python 3 instalado y accesible desde el comando `python3`
* Logisim instalado y accesible desde el comando `logisim`

### Pasos para ejecución

Una vez cumplidos los requisitos necesarios los tests pueden ser ejecutados mediante el siguiente comando:

```bash
./test.py tests s-mips.circ -o ./tests-out -t s-mips-template.circ
```

Este script dado un directorio escanea todos los ficheros `.asm` recursivamente dentro de dicho directorio y subdirectorios ensamblando el código de cada uno y generando los test correspondientes. Se espera encontrar dentro del fichero .asm una línea con un comentario de la siguiente forma:
**#prints** [:space:] *salida esperada*

Así cada test se ejecuta imprimiendo **OK** o **FAIL** en dependencia de si se obtuvo el resultado esperado o no. El script toma además varios niveles de verbosidad en el que brinda información más detallada de la ejecución.

### Agregar nuevos casos de prueba

Para crear nuevos casos de prueba se deberá crear un nuevo archivo `<test>.asm`. Es archivo contendrá el código que ejecutará el microprocesador. Estas instrucciones serán tomadas de las descritas en el [`s-mips.pdf`](./s-mips.pdf). Para definir cuál es el resultado correcto a mostrar por este código deberá estar definido una línea con el siguiente formato: `#prints <salida>`. Para mejor visualización de esto ver los casos de prueba existentes.

### Ejecución Manual

Para aquellos casos en los que se desee hacer un ejecución manual de uno de los casos de prueba se deben seguir los siguientes pasos:

1- Realizar el ensamblado de los casos de pruebas a utilizar. Para ello se debe usar el script `asambler.py`. Este recibe como parámetros el archivo con el código ensamblador y el directorio de salida. Ver la ayuda.

2- Una vez ejecutado el script buscar en la carpeta que existan 4 archivos llamados `Bank0`, `Bank1`, `Bank2` y `Bank3`.

3- Abrir en `logisim` el circuito `s-mips.circ`.

4- Activar la entrada nombrada `Deshabilitar Dispatcher`.

5- Luego se procede a cargar los archivos `Bank` dentro de la componente `RAM`. Para ello buscar en esta componente las etiquetas con nombres similares. Por cada componente `Bank`(llamada también `RAM` en logisim) hacer click derecho sobre ella. Luego hacer click en cargar imagen. Finalmente seleccionar el archivo `Bank` correspondiente del caso de prueba a utilizar. Importante asegurarse de haber utilizado los bancos correctos. La búsqueda al cargar imagen recuerda direcciones separadas del resto.

6- Conmutar el reloj. Si todos los pasos fueron seguidos de forma correcta el microprocesador deberá empezar a ejecutar las instrucciones almacenadas ahora en la RAM.

## Detalles de la evaluación

La entrega de dicho proyecto se realizará del 1ro al 5 de abril de 2024. La entrega consistirá de un archivo `zip` con el siguiente formato de nombre `<Nombre-de-Estudiante>-C<Grupo>.zip`. El archivo debe contener el `s-mips.circ` y el resto de los archivos entregados. Esta carpeta consta de un repositorio git local el cuál el estudiante debe usar para llevar constancia del trabajo realizado. Debe realizarce al menos un commit por cada componente implementada. Evitar por todos los medios crear un único commit al final con todos los cambios del proyecto.

### Requisitos

Para considerar un microprocesador como válido se deberán cumplir dos requisitos de precio, eficiencia y correctitud.

La correctitud se tomará en base a un conjunto de casos de pruebas que incluye los entregados con esta plantilla. En cada caso de prueba la salida de su microproseador por pantalla deberá coincidir con la salida establecida por la línea `#prints <salida>` en el caso de prueba. La salida de su microprocesador es aquella resultante de la ejecución de la instrucción `tty` en el código.

Junto al proyecto se entrega otro script de python `price.py` que permite dado un archivo `circ` calcular un precio del microprocesador. Dicho precio se calcula en base a las componentes utilizadas para la creación del mismo. El precio de un microprocesador para ser aceptado tendrá que tener un precio menor a las `100` unidades.

La eficiencia será medida en base a la cantidad de ciclos del reloj que toma completar un caso de prueba determinado. Este límite estará dado por `x` ciclos del reloj. Esto será establecido para cada caso de prueba. El número exacto para un caso de prueba estará dado por la línea `#limit <cant-iterciones>`. El número exacto no está definido aún para todos los tests pero si será tomado en cuenta a la hora de la evaluación.

### Precisiones Adicionales

Para el correcto funcionamiento de los tests automáticos las componentes `RAM` de `logisim` (no la `RAM` implementada en la plantilla) utilizadas en el `s-mips.circ` deben cumplir ciertas condiciones. Para evitar conflictos, y puesto que tampoco es necesario, queda prohibido utilizar dichas componentes como partes de alguna de las componentes a implementar.
