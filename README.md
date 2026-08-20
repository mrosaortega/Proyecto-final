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