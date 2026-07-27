# Phishi_Detector 🎣
![Gemini_Generated_Image_canou4canou4cano](https://github.com/user-attachments/assets/51e99fad-1961-4e6c-b5eb-d4b3523363e2)


**Una herramienta basada en Python para escanear correos electrónicos y URLs con el fin de detectar posibles amenazas de phishing.**

Phishi_Detector analiza varios componentes de correos electrónicos y URLs, incluyendo la reputación del remitente, el contenido, los archivos adjuntos, y aprovecha múltiples servicios de inteligencia de amenazas para proporcionar una evaluación exhaustiva de phishing.


## 🌟 Características Principales

* **Análisis de Encabezado de Correo:**
    * Verifica los estados de autenticación SPF, DKIM y DMARC.
    * Comprueba la alineación del encabezado para detectar spoofing.
* **Reputación del Remitente:**
    * Utiliza búsquedas WHOIS para obtener detalles del registro del dominio.
    * Se integra con VirusTotal y AbuseIPDB para conocer la reputación del dominio y la IP.
* **Escaneo y Análisis de URLs:**
    * Escanea URLs encontradas en el cuerpo del correo utilizando VirusTotal, Google Safe Browse e IPQualityScore (IPQS).
    * Realiza análisis léxicos en busca de patrones sospechosos (ej. longitud excesiva, caracteres especiales, palabras clave relacionadas con phishing, typosquatting).
* **Inspección de Contenido:**
    * Analiza el cuerpo del correo (texto y HTML) en busca de palabras y frases comunes de phishing.
    * Detecta formularios HTML sospechosos y técnicas de ofuscación (ej. elementos ocultos, trucos de JavaScript).
* **Verificación de Adjuntos:**
    * Inspecciona los archivos adjuntos del correo en busca de tipos de archivos potencialmente riesgosos y extensiones maliciosas comunes.
* **Puntuación y Veredicto:**
    * Proporciona una puntuación general de probabilidad de phishing.
    * Entrega un veredicto cualitativo (ej. Seguro, Sospechoso, Phishing).

## ⚙️ Cómo Funciona (Nivel General)

1.  **Entrada:** Recibe un archivo de correo electrónico (ej. formato `.eml`) o una URL directa como entrada.
2.  **Extracción y Análisis (Parsing):** Extrae la información relevante: encabezados, cuerpo y adjuntos de los correos; componentes de las URLs.
3.  **Módulos de Análisis:** Cada característica (verificación de encabezado, escaneo de URL, análisis de contenido, etc.) procesa los datos extraídos.
4.  **Integración de API:** Consulta servicios externos de inteligencia de amenazas (VirusTotal, AbuseIPDB, etc.) para obtener datos de amenazas actualizados.
5.  **Algoritmo de Puntuación:** Agrega los hallazgos de todos los módulos utilizando un sistema de puntuación ponderado.
6.  **Salida:** Presenta un informe resumido que incluye las verificaciones individuales, la puntuación general y el veredicto final.

## 🔧 Prerrequisitos

* Python 3.7 o superior
* Librerías de Python requeridas (ver `requirements.txt`):
    * `requests`
    * `beautifulsoup4`
    * `dnspython`
    * `python-whois`
    * `tabulate` (para salida potencialmente formateada, considere si lo usará para reportes de CLI)
    * `html5lib` (analizador para BeautifulSoup)

## 🚀 Instrucciones de Instalación

1.  **Clonar el Repositorio:**
    ```bash
    git clone [https://github.com/0xka3rim/Phishi_Detector.git](https://github.com/0xka3rim/Phishi_Detector.git)
    cd Phishi_Detector
    ```

2.  **Crear y Activar un Entorno Virtual (Recomendado):**
    * **Linux/macOS:**
        ```bash
        python3 -m venv venv
        source venv/bin/activate
        ```
    * **Windows:**
        ```bash
        python -m venv venv
        venv\Scripts\activate
        ```

3.  **Instalar Dependencias:**
    Asegúrese de tener un archivo `requirements.txt` en su repositorio. De lo contrario, puede crear uno después de la instalación manual (para desarrollo):
    ```bash
    # Configuración inicial o si falta requirements.txt
    pip install requests beautifulsoup4 dnspython python-whois tabulate html5lib
    pip freeze > requirements.txt # Para generar el archivo para otros

    # Si requirements.txt ya está presente
    pip install -r requirements.txt
    ```

4.  **🔑 Configuración de Claves API (CRÍTICO):**
    Este script requiere claves API para la funcionalidad completa con servicios externos. **NO escriba las claves API directamente en el script.**
    **DEBE** configurarlas como variables de entorno. El script intentará cargar estas claves utilizando `os.environ.get('API_KEY_NAME')`.

    * `VIRUSTOTAL_API_KEY`: Su clave API de VirusTotal.
    * `ABUSEIPDB_API_KEY`: Su clave API de AbuseIPDB.
    * `GOOGLE_SAFE_Browse_API_KEY`: Su clave API de Google Safe Browse API (asegúrese de usar la API correcta, ej. Web Risk API).
    * `IPQS_API_KEY`: Su clave API de IPQualityScore.

    **Configuración de Variables de Entorno:**

    * **Linux/macOS (bash/zsh):**
        Abra su archivo de configuración de shell (ej. `~/.bashrc`, `~/.zshrc`) y agregue:
        ```bash
        export VIRUSTOTAL_API_KEY="your_actual_virustotal_key"
        export ABUSEIPDB_API_KEY="your_actual_abuseipdb_key"
        export GOOGLE_SAFE_Browse_API_KEY="your_actual_google_key"
        export IPQS_API_KEY="your_actual_ipqs_key"
        ```
        Luego, cargue el archivo (ej. `source ~/.bashrc`) o abra una nueva terminal.

    * **Windows (Símbolo del Sistema - Temporal):**
        ```cmd
        set VIRUSTOTAL_API_KEY="your_actual_virustotal_key"
        set ABUSEIPDB_API_KEY="your_actual_abuseipdb_key"
        # ... y así sucesivamente para otras claves
        ```
        *(Nota: Estas se configuran solo para la sesión actual).*

    * **Windows (PowerShell - Temporal):**
        ```powershell
        $Env:VIRUSTOTAL_API_KEY="your_actual_virustotal_key"
        $Env:ABUSEIPDB_API_KEY="your_actual_abuseipdb_key"
        # ... y así sucesivamente para otras claves
        ```
        *(Nota: Estas se configuran solo para la sesión actual).*

    * **Windows (Persistente):**
        Busque "variables de entorno" en el Menú de Inicio, haga clic en "Editar las variables de entorno del sistema", luego haga clic en el botón "Variables de entorno...". Agregue nuevas variables de usuario con los nombres y sus claves.

    Si falta alguna clave API, es posible que se omitan las verificaciones correspondientes o que la funcionalidad sea limitada. El script idealmente debería manejar estas claves faltantes con elegancia (ej. imprimiendo una advertencia).

## 🎮 Uso

**(¡Por favor, actualice esta sección basándose en cómo recibe la entrada su script!)**

Navegue al directorio del proyecto en su terminal. Aquí hay algunos *ejemplos* de cómo podría ejecutarse. Deberá definir cómo los usuarios pasan los correos o las URLs.

**Opción 1: Usando la GUI**
![{4763B483-6CE6-420E-89E3-87E6D512CCA2}](https://github.com/user-attachments/assets/684451a2-b071-474d-bfdc-553eb8fc5075)
![{A52DBC29-717D-494F-8233-3E32FE3BF203}](https://github.com/user-attachments/assets/e4630903-d2a6-4639-8e09-6b27bacd1b85)


**Opción 2: Usando Argumentos de Línea de Comandos (Recomendado para Scripting)**

```bash
# Ejemplo: Analizar un archivo de correo
python Phishi_Detector_Project.py --email path/to/your/email.eml

# Ejemplo: Analizar una sola URL
python Phishi_Detector_Project.py --url "[https://suspicious-example.com/login](https://suspicious-example.com/login)"

# Ejemplo: Analizar una URL y exportar a un archivo
python Phishi_Detector_Project.py --url "[https://example.com](https://example.com)" --output report.txt

# Mostrar mensaje de ayuda (si implementa argparse)
python Phishi_Detector_Project.py --help
```
