# Proyecto-final
Entrega de Proyecto final master analisis de datos thepower
** 1 Integración de fuentes de datos

Fuentes utilizadas

Para el desarrollo del proyecto se utilizaron dos fuentes de datos diferentes relacionadas con películas y valoraciones de usuarios:

Fuente 1: MovieLens 25M

Dataset académico desarrollado por GroupLens Research (Universidad de
Minnesota), que contiene valoraciones realizadas por usuarios sobre
películas.

Archivos utilizados:

ratings.csv
movies.csv
links.csv

Variables principales:

Identificador de usuario (userId)
Identificador de película (movieId)
Valoración (rating)
Fecha de valoración (timestamp)
Título de la película
Géneros
Identificadores externos (imdbId, tmdbId)

Fuente 2: TMDB (The Movie Database)

Base de datos pública de metadatos cinematográficos utilizada para enriquecer la información de las películas.

Archivos utilizados:

tmdb_5000_movies.csv
tmdb_5000_credits.csv

Debido al tamaño de los archivos originales, estos no se incluyen en el respositorio.

Variables principales:

Presupuesto (budget)
Ingresos (revenue)
Duración (runtime)
Popularidad (popularity)
Valoración media (vote_average)
Número de votos (vote_count)
Idioma original
Fecha de lanzamiento
Actores y equipo técnico



** 2 Proceso de integración de datos

La integración de ambas fuentes se realizó mediante varias etapas:

1. Integración de MovieLens

Paso 1 - Se unen los datos de valoraciones de los usuarios (*ratings*) con la información de las películas (*movies*) mediante *movieId*, conservando todas las valoraciones y añadiendo el título y género correspondiente a cada película.

 

Paso 2- Se incorpora la información de *links* mediante *movieId* para relacionar las películas de MovieLens con sus identificadores externos, especialmente el identificador de TMDB (tmdbId).

Paso 3: Se combinan los datos de las películas de TMDB con los créditos mediante la correspondencia entre id de tmdb_5000_movies y movie_id de tmdb_5000_credits, incorporando información adicional sobre el reparto y equipo de las películas, a combinar con la informacion descriptiva de las peliculas.

 Integración de archivos TMDB -> tmdb_5000_movies.csv + tmdb_5000_credits.csv = mediante id = movie_id


Paso 4 Integración final. Unión entre MovieLens y TMDB utilizando tmdbId como identificador de MovieLens y id como identificador de TMDB, obteniendo un conjunto de datos final que combina las valoraciones de los usuarios con la información detallada de las películas y sus créditos.

De esta forma se obtuvo un único dataset que combina:

Valoraciones de usuarios.
Características de las películas.
Información económica.
Popularidad.
Información de producción.

El resultado fue un dataset enriquecido apto para realizar procesos de limpieza, transformación, análisis exploratorio de datos y
construcción de dashboards.

** 3 Selección de la muestra de trabajo

El dataset integrado generado a partir de MovieLens y TMDB contenía un volumen de información significativamente superior al requerido para el proyecto.

Con el objetivo de optimizar el procesamiento y facilitar el análisis en un entorno local, se decidió trabajar con una muestra de 70.000 registros, manteniendo un tamaño superior al mínimo exigido de 50.000 filas establecido en los requisitos del proyecto.

Durante el desarrollo se evaluó la posibilidad de realizar un muestreo aleatorio sobre el conjunto completo de datos. Sin embargo, debido al elevado tamaño del dataset integrado y a las limitaciones de procesamiento disponibles, esta opción generaba tiempos de ejecución significativamente elevados durante las fases de integración y exportación de datos.

Para garantizar que la muestra utilizada conservaba una diversidad suficiente para el análisis, se realizaron diferentes comprobacionessobre los registros seleccionados.

La muestra final contiene:

70.000 valoraciones.
3.546 películas únicas.
3.544 títulos únicos.
755 usuarios únicos.

Estos resultados indican que la muestra conserva una elevada variedad de películas, títulos y usuarios, permitiendo desarrollar
adecuadamente procesos de limpieza, transformación, análisis exploratorio, análisis estadístico y construcción del dashboard final.


** 4 - Entendimiento de los datos
Antes de iniciar las taresa de limpieza y transformacion se realizó una revision individual de cada variable con el objetivo de comprender su significado, identificar posibles duplicidades derivadas ed la integracion de fuentes y determinar su utilidad para el análisis posterior.
- principales hallazgos:
la mayoria de las varibales presentan un 0% de nulos (solo dos de ellas presentan nulosw: homepage con un 58,7% y tagline con un 3,61%)
Se identificaron variables duplicadas derivadas de la integracion (genres_x, genres_y, title_x, title_y, movie_id, id)


Se revisó la existencia de registros duplicados derivados de la ingregacion de diferentes fuentes, y de valores unicos.

** 5 Modificacion de datos

Se revisó el tipo de dato de cada valor, modificando a tipo fecha y a tipo str los que correspondían por el tipo de dato.
Posteriormente, se crea el archivo 03_limpieza_transformacion para seguir limpiando el archivo, de columnas no necesarias debido a duplicidades por la union de los archivos, creación de nuevas columnas necesarias para el análisis y gestión de valores nulos y/o duplicados.

** 6 Limpieza y Transformación de Datos

Una vez realizado el análisis preliminar del conjunto de datos, se procedió a la fase de limpieza y transformación con el objetivo de mejorar la calidad de la información, eliminar redundancias y generar nuevas variables que facilitasen el análisis exploratorio posterior.

6.1. Revisión y corrección de tipos de datos

Durante el análisis preliminar se identificaron variables cuyo tipo de dato no era el más adecuado para su interpretación.

Las columnas: *timestamp* y *release_date*

fueron convertidas a formato fecha para permitir posteriores análisis temporales y la creación de nuevas variables relacionadas con los años "rating_year" y "main_actor".

Variables identificadoras - este tipo de modificación lo hemos hecho en cada archivo ya que se modifica al guardarlo

Las columnas:*userId*, *movieId*, *imdbId*, *tmdbId*, *Id*
se mofificaron sus valores de tipo int o float a tipo str ya que su función es identificar registros y no representar magnitudes cuantitativas.

- Eliminación de variables redundantes

Durante la integración de MovieLens y TMDB se generaron varias columnas duplicadas o equivalentes.

Tras su revisión, se eliminaron aquellas variables que aportaban información repetida y otras que carecían de valor analítico para los objetivos del proyecto.

Entre ellas se encuentran:

genres_y
title_y
homepage
tagline

La eliminación de estas variables permitió reducir la complejidad del conjunto de datos y facilitar su posterior análisis.


- Simplificación de variables complejas

Varias columnas incorporaban información en formato lista o diccionario, dificultando su utilización en análisis estadísticos y
visualizaciones.

Con el objetivo de simplificar el conjunto de datos se extrajo la información más relevante de cada estructura, creandose columnas adicionales con la información más relevantes: cast, crew, production_companies, production_countries, obteniendo en el mismo orden: actor principal, director, productoera principal y principal pais. Algunos de estos datos devolvian estructuras vacías ([]).

Dado que estos casos representaban un porcentaje muy reducido del conjunto de datos, se decidió mantener los registros y asignar el valor "Unknown" a las variables derivadas correspondientes.

Esta decisión permitió conservar todas las observaciones del dataset sin perder información relevante para el análisis posterior.

en el archivo 02_analisis_preliminar se crearon dos columnas Año de estreno procedentes de timestamp y released_date cuando se modificaron a dato tipo fecha se extrajo el año de cada valor, obteniendo las columnas  rating_year  y release_year, respectivamente.


- Tratamiento de valores anómalos
Presupuesto (budget)

Se observó que un 3,29% de los registros presentaban un valor igual a 0.

Dado que este valor probablemente representa presupuestos no informados y puede generar distorsiones en métricas derivadas, dichos registros fueron transformados en valores nulos (NaN).

Ingresos (revenue)

Se observó igualmente que un 5,24% de los registros presentaban ingresos iguales a 0.

Al igual que en la variable budget, estos registros fueron tratados como valores faltantes y transformados en valores nulos (NaN) para evitar interpretaciones erróneas de la rentabilidad de las películas.

- Nuevas variables

una vez las variables previas budget y revenue se trataron se generaron dos variables numericas "profit" y "roi"

Beneficio estimado = profit = revenue - budget

Retorno sobre la inversión = roi = profit / budget

Resultado de la fase de limpieza y transformación

Tras la ejecución de todas las tareas descritas:

Se eliminaron variables redundantes o sin valor analítico.
Se corrigieron tipos de datos.
Se trataron valores nulos y anómalos.
Se simplificaron estructuras complejas.
Se generaron variables derivadas con valor analítico.
Se obtuvo un dataset más limpio, consistente y preparado para la fase de análisis exploratorio y construcción del dashboard final.

** Visualizacion de variables numéricas
se realizó una exploración de las variables numéricas mediante histogramas y diagramas de caja (boxplots) para analizar su distribución e identificar posibles valores extremos. Esta exploración permitió observar que algunas variables presentaban distribuciones asimétricas y un número elevado de valores extremos.

Para identificar los outliers se realizaron diferentes comprobaciones utilizando los percentiles 75, 90 y 99. Se decidió utilizar el percentil 90 como referencia, ya que el conjunto de datos contiene valores extremos que pueden corresponder a situaciones reales, especialmente en variables como presupuesto, ingresos o popularidad. El percentil 99 no permitió identificar outliers relevantes.

Los valores extremos detectados fueron revisados individualmente para comprobar si eran coherentes con la variable correspondiente. En aquellos casos en los que se identificaron valores que no resultaban razonables, como ocurrió con determinados valores de runtime, estos fueron modificados.

Finalmente, se realizó el tratamiento de los valores nulos presentes en las variables seleccionadas - *budget* y *revenue* y las variables creadas a partir de ellas *roi* y *profit*. En estas variables se realizó una sustitución de los NaN por la mediana de la variable, al considerarse una medida más robusta frente a la presencia de valores extremos.

Posteriormente se ha llevado a cabo la limpieza y transformación de los valores en el notebook 03_limpieza_transformaciones

Principalmente se han identificado aquellas columnas con valores únicos para mantenerlas y eliminar aquellas que no aportaban valor o columnas que tenían mismos valores. Por lo que se han eliminado varias variables como "title", "title_x", "title_y","genres_y" que tenian mismos valores que otras. Adicionalmente, se han limpiado los valores de otras columnas como "crew" y "cast" eran variables cuyo valor mostraba un diccionario con diferentes valores, y nos hemos quedado con el más relevante para posteriormente usarlos en la estadística y en el dashboard. Al igual que production_countries y production_companies, se ha sacado el valor principal haciendo otra columna/variable para el resultado, eliminando del df la original con ruido.

Se ha visualizado mediante boxplot las columnas numericas - interpretándose cada variable, viendo los fallos - date, los valores nulos y los outliers. Hemos revisado los outliers, eliminando algunos y modificando por otros cuando se conocia el valor real. 
En las variables "budget", "revenue" había valores nulos que han sido sustituidos por la mediana y posteriormente hemos sustituido tambien las variables creadas a partir de estas "profit" y "roi".

Se ha visualizado tambien las columnas categoricas dado que en la mayoria de las variables había mas de 10 categorías hemos sacado el top 10 de estas, ya que era complicado de procesar en un gráfico tantos valores. una de las variables tenia valores nulos "homepage" que han sido modificados por "unknown".

Por último se ha realizado un análisis estadístico: (notebook 04_analisis_estadistico) para identificar relaciones entre variables, comprender los principales patrones y extraer principales insights.

Para comenzar se ha realizado una matriz de correlaciones utilizando las principales variables numéricas, con lo que se identificaron relaciones significativas entre diferentes variables.

Seguido de gráficos de dispersión entre diferentes variables, análisis de la variable extraida ROI y modificación de sus valores mediante la transformación de la variable "budget" veiamos que tenia valores que no podían ser reales modificando estos valores muy bajos a la mediana. Reescribiendo las variables *profit* y *ROI*. adicionalmente se creaba otra variable basada en categorias de nivel de presupuesto ya que aun con la modificación habia valores que eran demasiado bajos.

Tambien se han analizado otras relaciones entre variables como género e dioma con el rating y revenue, la identificacion de los top 10 directores con lo que veiamos que si teniamos en cuenta solo directores que habian hecho más de 3 películas, el top cambiaba. Igualmente ocurría con main_country, vimos que había países muy bien valorados pero que si agrupábamos y filtrabamos por paises con mas de 20 peliculas, el top 3 se modificaba por completo. 

las conclusiones obtenidas de este análisis han sido principalmente:
 - Relación positiva entre presupuesto e ingresos
 - Popularidad y el numero de valoraciones muestran tambien una fuerte relación
 - las películas de menos prespuesto presentan una rentabilidad relativa superior
 - Existen diferencias relevantes entre géneros, idiomas y directores en terminos de valoración y desempeño económico.
 
