# Proyecto Análisis de Datos de Steam

## Descripción

Este proyecto realiza un análisis exhaustivo de datos de juegos de Steam mediante la extracción de información de la API de Steam. Utiliza Python para procesar datos de bibliotecas de usuarios, logros, contenido para adultos y metadatos de juegos. Además, emplea el modelo de IA Ollama para clasificar países de desarrolladores y géneros de juegos. Los datos procesados se visualizan en PowerBI para obtener insights sobre patrones de juego, preferencias de usuarios y tendencias en la industria de videojuegos.

El proyecto genera varios datasets clave y permite una exploración profunda de la plataforma Steam.

## Requisitos

- **Python 3.x**: Versión recomendada 3.8 o superior.
- **Librerías de Python**:
  - `pandas`
  - `numpy`
  - `requests`
  - `matplotlib`
  - `seaborn`
  - `ollama`
  - `mysql.connector`
  - `python-dotenv`
  - `tqdm`
  - `concurrent.futures`
- **API Key de Steam**: Necesaria para acceder a la API de Steam. Debe configurarse en un archivo `.env`.
- **Ollama**: Instalado y configurado con el modelo `gemma3:4b` para procesamiento de IA.
- **PowerBI**: Para visualizar los datos generados (archivo `juegos_steam.pbix`).

## Instalación

1. **Clona el repositorio**:
   ```bash
   git clone <url-del-repositorio>
   cd project-DA-promo-59-modulo-4-Patricia-Anaya
   ```

2. **Instala las dependencias de Python**:
   ```bash
   pip install pandas numpy requests matplotlib seaborn ollama mysql-connector-python python-dotenv tqdm
   ```

3. **Configura el archivo `.env`**:
   - Crea un archivo `.env` en la raíz del proyecto.
   - Agrega tu clave de API de Steam:
     ```
     steam_key=TU_CLAVE_DE_API_DE_STEAM
     ```

4. **Instala y configura Ollama**:
   - Descarga e instala Ollama desde [ollama.ai](https://ollama.ai).
   - Instala el modelo `gemma3:4b`:
     ```bash
     ollama pull gemma3:4b
     ```

5. **Instala PowerBI** (opcional para visualización):
   - Descarga PowerBI Desktop desde el sitio oficial de Microsoft.

## Uso

1. **Ejecuta el notebook principal**:
   - Abre `steam.ipynb` en Jupyter Notebook o VS Code.
   - Ejecuta las celdas en orden para extraer y procesar los datos.
   - El notebook realiza los siguientes pasos:
     - Carga de configuraciones y librerías.
     - Extracción de bibliotecas de juegos de usuarios de Steam.
     - Obtención de logros y estadísticas.
     - Procesamiento de contenido para adultos.
     - Creación del dataset maestro con metadatos de juegos, países de desarrolladores y géneros clasificados por IA.

2. **Datasets generados**:
   - `datasets/dataset_steam.csv`: Datos de juegos por usuario, incluyendo tiempo de juego y logros.
   - `datasets/mapeo_contenido.csv`: Mapeo de descriptores de contenido para adultos.
   - `datasets/juegos_maestro.csv`: Dataset maestro con información detallada de juegos (precio, género, desarrollador, país, etc.).

3. **Visualización en PowerBI**:
   - Abre el archivo `juegos_steam.pbix` en PowerBI Desktop.
   - Explora los dashboards preconfigurados para analizar tendencias, distribuciones y correlaciones en los datos de Steam.

## Estructura del Proyecto

```
project-DA-promo-59-modulo-4-Patricia-Anaya/
├── steam.ipynb                    # Notebook principal con el código de extracción y procesamiento
├── juegos_steam.pbix              # Archivo de PowerBI para visualización
├── datasets/                      # Carpeta con datasets generados
│   ├── dataset_steam.csv
│   ├── mapeo_contenido.csv
│   └── juegos_maestro.csv
├── .env                           # Archivo de variables de entorno (no incluido en el repo)
└── README.md                      # Este archivo
```

## Notas Importantes

- **Límites de la API de Steam**: La API tiene restricciones de rate limiting. El código incluye pausas y manejo de errores para evitar bloqueos.
- **Procesamiento con IA**: El uso de Ollama requiere recursos computacionales. Ajusta `MAX_THREADS_OLLAMA` según tu hardware.
- **Privacidad**: No compartas tu clave de API de Steam ni el archivo `.env`.
- **Actualizaciones**: Los datos de Steam cambian con el tiempo; ejecuta el notebook periódicamente para datos frescos.
