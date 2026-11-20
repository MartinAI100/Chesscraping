# Guía de uso

## Datos y procedencia

El estudio emplea datos recopilados en noviembre y diciembre de 2025. El notebook combina datos públicos de Chess.com, incluida su API, con información de otras fuentes para países y aperturas. Las fechas corresponden a la recopilación del estudio; los datos recuperados en una ejecución nueva pueden cambiar.

Los seis CSV se encuentran directamente en `data/csv/`. El archivo `datos_chesscraping.zip` contiene los volcados SQL en la carpeta `sql/`:

| Carpeta | Contenido | Uso |
|---|---|---|
| `data/csv/` | `1.3mill.csv`, `players_pool.csv`, `Fide_max.csv`, `streamers_players.csv`, `titled_players.csv` y `titled_players_original.csv`. | Listas de usuarios y países, titulados, streamers y jugadores de clasificación. El notebook utiliza varios de estos archivos durante la recopilación. |
| `sql/` | Volcados `chesscraping_country.sql`, `chesscraping_game.sql`, `chesscraping_gamelist.sql`, `chesscraping_mode.sql`, `chesscraping_opening.sql`, `chesscraping_player.sql`, `chesscraping_stats.sql`, `chesscraping_status.sql` y `chesscraping_title.sql`. | Tablas y registros para restaurar una copia de la base de datos del estudio. |

`1.3mill.csv` contiene aproximadamente 1,3 millones de registros de nombres de usuario y códigos de país. No aparece referenciado directamente por su nombre en el código del notebook.

Los archivos pueden contener datos que identifiquen usuarios. Antes de redistribuirlos, revisa las condiciones de Chess.com y las obligaciones de privacidad. No se garantiza que la recopilación pueda redistribuirse sin autorización.

## Preparación del notebook

El notebook principal es `chesscraping.ipynb`. Sus metadatos indican que se utilizó el entorno base de Anaconda con Python 3.12.7. Necesita Jupyter y las bibliotecas de `requirements.txt`, además de MySQL para las etapas que consultan o guardan datos.

Para crear un entorno de Anaconda independiente desde Anaconda Prompt, situado en la carpeta del proyecto:

```powershell
conda create -n chesscraping python=3.12
conda activate chesscraping
python -m pip install -r requirements.txt
python -m ipykernel install --user --name chesscraping --display-name "Python (chesscraping)"
jupyter lab
```

Selecciona el kernel `Python (chesscraping)` en el notebook. Las dependencias no están fijadas a las versiones históricas; pueden variar los resultados o aparecer incompatibilidades. Instálalas antes de ejecutar la primera celda de configuración.

La primera celda configura `DATA_DIR`, la carpeta con los CSV; `CHECKPOINT_DIR`, la carpeta donde se guardan los checkpoints; y la conexión MySQL. Por defecto, `DATA_DIR` apunta a `./data/csv`, relativo al directorio de trabajo del kernel, y los checkpoints se guardan en ese directorio de trabajo. Puedes cambiar ambas rutas por rutas absolutas.

Copia `.env.example` como `.env` en la raíz del proyecto y edita la copia con tus valores. `.env` está ignorado por Git y no debe compartirse. La celda carga ese archivo; las variables de entorno ya definidas tienen prioridad. Si `CHESS_MYSQL_PASSWORD` no está definida, la contraseña se solicita de forma oculta; si está definida vacía se utiliza una contraseña vacía. También puedes configurar `CHESS_MYSQL_HOST` y `CHESS_MYSQL_USER`. `CHESS_USER_AGENT` permite indicar un identificador y contacto para la API. `CHESS_CHROMEDRIVER_PATH` es opcional: si está vacío, Selenium intenta gestionar el controlador; si no, usa la ruta indicada. No guardes contraseñas ni valores privados en el notebook.

## Reutilizar los datos o recopilar de nuevo

1. Si trabajas desde la raíz del proyecto, `data/csv/` ya contiene los CSV. Si utilizas otra carpeta, ajusta `DATA_DIR` a su ruta. Descomprime `datos_chesscraping.zip` en una carpeta de trabajo cuando necesites los volcados de `sql/`.
2. Para usar los CSV, conserva sus nombres: el notebook construye `POOL_CSV`, `TITLED_CSV`, `STREAMERS_CSV` y `Leaderboard_CSV` a partir de `DATA_DIR`.
3. Para reutilizar los resultados de la base, importa los nueve volcados de `sql/` en una base MySQL de trabajo `chesscraping`, con una herramienta como MySQL Workbench. Los volcados incluyen `DROP TABLE`: usa una base nueva o de prueba para no borrar tablas existentes. Comprueba el orden de importación y las claves foráneas en tu instalación.
4. Configura la conexión del notebook a esa base y ejecuta en orden las celdas necesarias para inicializar imports, conexión y consultas antes de las secciones de análisis. Las consultas leen MySQL; disponer únicamente de los CSV no reemplaza esa base.

Alternativamente, el notebook contiene la creación de tablas y el proceso de recopilación. Volver a obtener los datos puede requerir mucho tiempo y producir una muestra diferente. Antes de ejecutar solicitudes automatizadas o entrenar modelos, consulta las condiciones vigentes de las fuentes.

## Checkpoints

El notebook usa `playersrap_checkpoint.txt` para registrar el avance de usuarios y `checkpoints_rapid_games.csv` para el avance de partidas. Si no existen, las funciones de lectura empiezan sin progreso anterior y las de guardado los crean en `CHECKPOINT_DIR`. La carpeta debe existir. Conserva estos archivos junto a la base correspondiente si quieres reanudar una ejecución interrumpida; por sí solos no contienen la base de datos.

## Opciones de análisis e informes

La celda titulada «VARS IMPORTANTES BOOLS CAMBIAR XA EJEC O NO» define estas variables:

| Variable | Función observada |
|---|---|
| `ejecallmodes` | Participa en la condición de carga de datos agregados de todos los modos. |
| `ejecmodex` | Participa en las condiciones de carga de partidas y jugadores del modo seleccionado. |
| `noejecna` | Interviene en esas condiciones; no garantiza que se omitan todas las demás celdas. |
| `primeravez` | También interviene en las condiciones de carga y cambia durante algunas llamadas; incluso con otras flags desactivadas puede activar alguna carga. |
| `ejec` | Se define en esta celda; no se ha identificado una condición posterior que la utilice. |
| `ejec_allhtml` | Con valor `1` activa la generación de todos los informes de profiling de la sección correspondiente. |
| `html_gmr`, `html_allmodes`, `html_dfres`, `html_dfeconum`, `html_bullet`, `html_blitz`, `html_rapid`, `html_daily`, `html_tactics`, `html_puzzle_rush` | Con valor `1` activa el informe correspondiente. |

Los indicadores de HTML están inicialmente a `0`; los informes se generan cuando se activan. El análisis, los modelos y el scraping pueden consumir tiempo aunque los informes permanezcan desactivados. Ejecuta las secciones en orden y comprueba que sus variables y tablas previas estén disponibles.
