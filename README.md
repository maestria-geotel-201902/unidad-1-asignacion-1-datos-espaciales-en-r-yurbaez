
# Unidad 1, asignación 1: datos espaciales en R

Por lo pronto, ayudaré con `...`. Donde quiera que los veas, deberás
sustituirlos por lo que indique el correspondiente mandato. “No te
aco’tumbre”.

Dentro de las opciones de `knitr`, en el encabezado de este archivo, es
probable que encuentres el argumento `eval = F`. Antes de tejer debes
cambiarlo a `eval = T`, para que evalúe los bloques de código según tus
cambios.

## Mis primeros mapas en R con *simple features*

### Provincia asignada

Toma nota del código de tu provincia asignada aleatoriamente.

``` r
 # abreviatura provaleatoria
 #       acade            11
 #       agrie            09
 #       aleir            10
 #       arqco            16
 #       cindy            12
 #       franc            23
 #       geora            08
 #       hoyod            22
 #       ingan            07
 #       ingdi            19
 #       itac9            15
 #       ivanv            25
 #       lbine            30
 #       leona            24
 #       magda            20
 #       maryj            02
 #       masue            26
 #       mmvol            13
 #       naui2            29
 #       rober            27
 #       wilne            06
 #       yoenn            21
```

### Paquete `sf`

  - Carga el paquete `sf`, con el que podrás importar y manipular
    objetos *simple features*.

<!-- end list -->

``` r
library(...)
```

### Todas las provincias

  - Carga a la memoria la capa de provincias del archivo GeoPackage.
      - Primero determina cómo se denomina la capa de provincias usando
        la función `st_layers`. En esta función, escribe la ruta del
        archivo (el único GeoPackage dentro de la carpeta `data`. La
        ruta será algo tal que `data/...`). La función devolverá los
        nombres de las capas disponibles. Toma nota del nombre de la
        capa de provincias (es intuitivo).
      - Con dicha información, carga la capa de provincias con la
        función `st_read`, asignando al objeto `prov`. En el argumento
        `dsn` debes escribir la ruta del GeoPackage, y en `layer` el
        nombre de la capa.
  - Genera un panel de mapas mediante `plot`, donde se muestre el objeto
    representado según todas sus columnas disponibles (se deberían
    generar 4 mapas en un panel, uno por cada campo).
  - Genera un mapa mediante `plot` donde se muestre el objeto
    representado según la columna `PROV`.

<!-- end list -->

``` r
st_layers(...)
... <- st_read(dsn = '...', layer = '...')
plot(...)
plot(...['...'])
```

### Mi provincia asignada

  - Genera un mapa de tu provincia asignada representando sólo el campo
    `PROV`. Nota que para elegir tu provincia asignada, necesitarás
    colocar el código de la misma dentro de los apóstrofos del índice de
    filas `prov$PROV %in% ''`.
  - Genera un objeto `sf` que sólo contenga tu provincia asignada
    mediante un subconjunto usando el índice de filas. Denomínalo
    `miprov`. El nuevo objeto podrás usarlo posteriormente.
  - Imprime el objeto `miprov`. “Imprimir” en este contexto consiste en
    mostrar el objeto (normalmente un resumen de éste) de acuerdo a su
    clase. Para ello basta con escribir su nombre y presionar
    `<entrer>`.
  - un panel de mapas mediante `plot`, donde se muestre `miprov`
    representado según todas sus columnas disponibles.

<!-- end list -->

``` r
plot(...[prov$PROV %in% '', '...'])
... <- ...[prov$PROV %in% '...', ]
...
plot(...)
```

## Cálculos, operaciones geométricas

Nota que hasta este punto sólo realizaste mapas. En esta sección,
realizarás cálculos y operaciones geométricas básicas. Además del
paquete `sf` ya cargado, necesitarás la colección `tidyverse`.

### Paquetes

``` r
library(...)
```

### Área provincial

  - Calcula el área de cada provincia y asígnala a una nueva columna
    denominada `areaenm2` dentro del propio objeto `prov`.
  - Imprime la tabla `prov` mostrando sólo las columnas `TOPONIMIA` y
    `areaenm2` recién creada. Se mostrará también la columna geométrica.

<!-- end list -->

``` r
prov$... <- st_area(...)
as.data.frame(prov[,c('...','...')])
```

### Ordenar provincias

  - Genera una tabla de provincias sólo con las columnas `TOPONIMIA` y
    `areaenm2`, y ordénalas de mayor a menor usando el campo `areaenm2`.
    Aunque puedes ejecutar una tubería de manera fluida y en una sola
    línea, te recomiendo hacerlo paso a paso, para que explores el
    efecto de cada verbo y la funcionalidad del operador `pipe` (`%>%`).
      - Primero asegúrate de que el objeto `prov` está disponible
        imprimiéndolo. El encabezado del resumen debería mostrar algo
        tal que `Simple feature collection with 32 features and 5
        fields`.
      - Deshazte de la columna geométrica, mediante la función
        `st_drop_geometry()`. Esto devolverá el objeto como `data.frame`
        (el objeto `prov` se mantendrá como `sf`, sólo estarás generando
        un `sf` que no reservará espacio de la RAM).
      - Selecciona las columnas `TOPONIMIA` y `areaenm2`.
      - Organiza el resultado mediante la función `arrange`. Verás que
        la tabla se organizará ascendentemente.
      - Organiza descendentemente mediante la función `desc`.

<!-- end list -->

``` r
...
... %>% st_drop_geometry()
... %>% st_drop_geometry() %>% dplyr::select(..., ...)
... %>% st_drop_geometry() %>% dplyr::select(..., ...) %>% arrange(...)
... %>% st_drop_geometry() %>% dplyr::select(..., ...) %>% arrange(desc(...))
```

### Filtra tu provincia con `tidyverse`

  - Usa la tubería de `tidyverse` para filtrar tu provincia mediante la
    función `dplyr::filter`.
  - Dentro de la misma tubería, haz un panel de mapas mediante `plot`
    usando la tubería de `tidyverse`, donde se muestre tu provincia
    representada según la columna `areaenm2`

<!-- end list -->

``` r
... %>% dplyr::filter(PROV %in% '...')
... %>% dplyr::filter(PROV %in% '...') %>% dplyr::select(...) %>% plot
```

### Filtra tres provincias

Haz un `plot` de tres provincias usando sólo la columna `areaenm2`, una
de ellas que sea la tuya e incluye las de dos compañeras más que elijas
libremente.

``` r
... %>% dplyr::filter(PROV %in% c('...', '...', '...')) %>% dplyr::select(...) %>% plot
```

### Municipios

  - Carga la capa de municipios y asígnala al objeto `mun`.
  - Imprime el objeto `mun`. Observa qué columnas componen sus
    atributos. Nota que existe una columna heredada `REG` y otra `PROV`.
  - Genera un panel de mapas mediante `plot` usando la tubería de
    `tidyverse`, donde se muestre `mun` representado según todas sus
    columnas disponibles.

<!-- end list -->

``` r
... <- st_read(dsn = '...', layer = '...')
...
... %>% plot
```

### Municipios de mi provincia

  - Genera el objeto `mimun`, que contenga los municipios de tu
    provincia asignada usando la tubería de `tidyverse`.
  - Haz un panel de mapas mediante `plot` usando la tubería de
    `tidyverse`, donde se muestre `mimun` representado según todas sus
    columnas disponibles.

<!-- end list -->

``` r
... <- ... %>% dplyr::filter(PROV %in% '...')
... %>% plot
```
