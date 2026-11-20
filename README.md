# Chesscraping

Estudio exploratorio de jugadores y partidas de ajedrez, con recopilación de datos, almacenamiento en MySQL, análisis de aperturas y un modelo de predicción de resultados.

Los datos del estudio fueron recopilados en noviembre y diciembre de 2025. El proyecto utiliza datos públicos de Chess.com, incluida su API pública, junto con otras fuentes para códigos de países y aperturas. Una nueva recopilación puede dar resultados distintos.

- `data/csv/`: seis archivos con listas de usuarios, países y clasificaciones para el estudio.
- `datos_chesscraping.zip`: volcados de las tablas MySQL utilizadas en el estudio.
- `requirements.txt`: dependencias de Python.
- [Guía de uso](USO_NOTEBOOK.md): contenido de los datos, preparación de MySQL, checkpoints y opciones de ejecución del notebook.

El notebook `chesscraping.ipynb` incluye una celda inicial para configurar rutas, conexión MySQL y acceso a la API. Copia `.env.example` como `.env` y configura tus datos locales; Git ignora `.env`. Consulta la guía para crear el entorno e instalar las dependencias.

## Cita

Si utilizas este proyecto, cita el repositorio como **García Navarro, Martín José. [Chesscraping](https://github.com/MartinAI100/Chesscraping)**. Los metadatos de cita también están disponibles en `CITATION.cff`.

## Licencia

El código propio del proyecto se distribuye con la [licencia MIT](LICENSE), que permite también su uso comercial. Al distribuir copias o partes sustanciales del código se deben conservar el aviso de autoría y el texto de la licencia. La cita del repositorio indicada arriba es una petición adicional. Las condiciones de los datos y contenidos de terceros son independientes de esta licencia.

## Fuentes y redistribución

El archivo de datos incluye identificadores de usuarios, datos de perfiles y partidas. Que los datos sean públicamente accesibles no implica por sí solo permiso para redistribuir la recopilación: consulta las [condiciones de Chess.com](https://www.chess.com/legal/user-agreement), la [documentación de su API](https://support.chess.com/en/articles/9650547-what-is-the-pubapi-and-how-do-i-use-it) y la normativa de privacidad aplicable antes de publicar o redistribuir estos archivos. El proyecto no afirma disponer de una licencia o autorización especial para redistribuirlos.
