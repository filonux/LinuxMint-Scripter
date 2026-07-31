# LinuxMint Scripter

**Generador y revisor de scripts (Bash / Python) asistido por IA, hecho a medida para Linux Mint 22.3 «Zena»**

![Licencia](https://img.shields.io/badge/licencia-GPLv3-blue.svg)
![Versión](https://img.shields.io/badge/versi%C3%B3n-2.1-brightgreen.svg)
![Hecho para](https://img.shields.io/badge/hecho%20para-Linux%20Mint%2022.3-green.svg)
![Arquitectura](https://img.shields.io/badge/arquitectura-100%25%20cliente-lightgrey.svg)

LinuxMint Scripter es una aplicación web de **un único archivo HTML** (HTML + CSS + JavaScript vanilla, sin frameworks ni paso de compilación) que traduce peticiones en lenguaje natural a scripts de **Bash o Python 3** listos para ejecutar en Linux Mint 22.3, o a comandos de terminal sueltos. A diferencia de pedirle lo mismo a un chatbot genérico, lleva integrado el conocimiento específico de Mint (Cinnamon, Nemo, `snapd` bloqueado de fábrica, Flatpak/Flathub, `mintupdate-cli`, Timeshift…) y está explícitamente instruida para no disfrazar de "solución para Mint" un script pensado para Ubuntu/GNOME genérico.

Habla con la **API Local (LM Studio, Ollama)** o con **cualquier servidor/API compatible con OpenAI o Anthropic** — o en la nube (OpenRouter, Groq, Mistral, Google, Hugging Face)— usando tu propia clave. No hay servidor intermedio: la conexión, el almacenamiento y el cifrado ocurren enteramente dentro de tu navegador.

<img width="1909" height="852" alt="Inicio" src="https://github.com/user-attachments/assets/e02274cb-1da0-4dc5-81f4-bde2f158a415" />
<img width="1901" height="852" alt="Revision-opciones" src="https://github.com/user-attachments/assets/95925ab2-d1c5-4bd7-a41a-e70702ce927f" />
<img width="1907" height="852" alt="Revision-Iterativa" src="https://github.com/user-attachments/assets/83449d0b-fb4a-4899-bf19-1ea6ce37820e" />
<img width="1912" height="853" alt="Historial-menu" src="https://github.com/user-attachments/assets/c2e9369a-db5f-4d0e-ab8a-bafdc1ec3c32" />


## Índice

- [¿Por qué esta herramienta?](#por-qué-esta-herramienta)
- [Características](#características)
- [Requisitos](#requisitos)
- [Puesta en marcha](#puesta-en-marcha)
- [Flujo de uso típico](#flujo-de-uso-típico)
- [Proveedores de modelos compatibles](#proveedores-de-modelos-compatibles)
- [Privacidad y seguridad](#privacidad-y-seguridad)
- [Limitaciones conocidas](#limitaciones-conocidas)
- [Contribuir](#contribuir)
- [Licencia](#licencia)

## ¿Por qué esta herramienta?

- **Contexto real de Mint 22.3, no genérico.** El *system prompt* incorpora de serie los hechos que distinguen a Mint de Ubuntu/GNOME: gestor de archivos Nemo, snap bloqueado, Flatpak listo, particularidades de `mintupdate-cli`, Timeshift, Python 3.12 preinstalado…
- **No solo genera, también audita.** La revisión iterativa encadena varios pases de auditoría/corrección sobre el script completo, inspirados en los bucles «planificar → aplicar → verificar» de la codificación agéntica.
- **Cero instalación.** Es un `.html` suelto: se abre con doble clic y ya está.
- **Multi-proveedor y con tu propia clave (BYOK).** Modelo local o cualquier API en la nube compatible con OpenAI o Claude.
- **Nada sale de tu navegador** salvo las llamadas al proveedor de IA que elijas: sin cuentas, sin telemetría, sin backend propio que mantener.

## Características

### Generación asistida por IA
- Chat en lenguaje natural para pedir scripts o comandos.
- Selector de lenguaje: Auto (decide el modelo), 🐚 forzar Bash o 🐍 forzar Python 3.
- **Modo verificación**: pide una explicación línea a línea del script.
- **Plantilla Terminal**: limita la respuesta a comandos sueltos, sin generar un script completo.
- Plantillas rápidas para tareas habituales (instalar paquetes, cron, backup con `rsync`, unidad `systemd`, red con `nmcli`, limpieza del sistema, monitorización con alertas).
- Adjunta imágenes (Ctrl+V o arrastrar) para modelos con visión — útil para pegar una captura de un error de terminal.
- Sube un script existente (`.sh`, `.py`, `.service`, `.yaml`…) para que el modelo lo revise.

### Revisión y edición de código
- Editor con resaltado de sintaxis (CodeMirror) para Bash, Python, YAML y TOML, con buscar/reemplazar integrado.
- Selecciona cualquier fragmento del script (o haz clic en el número de línea) para preguntar, corregir o pedir una explicación **solo de esa parte**, con vista de diff antes/después al aplicar un cambio.
- **Revisión iterativa**: de 1 a 3 pases automáticos de auditoría y corrección del script completo, con puntuación por pase y resumen de cambios.
- Detección local (heurística, basada en patrones) de comandos potencialmente destructivos (`rm -rf`, `dd`, `mkfs`, `shutil.rmtree`…) y de posibles problemas de sintaxis, resaltados directamente en el editor.
- Acceso directo a **shellcheck.net** con el script ya copiado, para un análisis real.

### Despliegue de scripts
- Genera un paquete de despliegue a partir del script del editor: ruta de instalación, ámbito (solo tu usuario o todo el sistema) y permisos.
- Programación opcional: tarea **cron** (con atajos: cada hora, diario, semanal, al arrancar), **servicio systemd** o **servicio + temporizador** (con `OnCalendar` y `Persistent=true`).
- Pasos listos para copiar y pegar en la terminal, o descarga directa de un instalador `.sh`.

### ⌨️ Comandos rápidos (sin pasar por el modelo)
- Chuleta local y buscable con una docena de categorías: APT, Flatpak, actualizaciones de Mint, archivos, permisos, procesos, servicios systemd, red, discos/backups, usuarios, compresión y ajustes de Cinnamon.

### Comparador y utilidades
- Compara dos proveedores/modelos en paralelo con el mismo prompt.
- Historial local de scripts generados, con búsqueda.
- Exporta la conversación completa a Markdown; guarda el script con nombre propio (la extensión se ajusta al lenguaje activo).
- Indicador de uso de contexto (tokens) frente al límite configurado, con recorte automático del historial más antiguo antes de fallar por exceso de contexto.

## Requisitos

- Un navegador moderno (Chrome, Firefox, Edge…).
- Conexión a internet para cargar las librerías de la interfaz (CodeMirror, tipografías) y para las llamadas al proveedor de IA elegido, salvo que sirvas esos recursos tú mismo.
- **Una clave de API**, de alguno de estos dos tipos:
  - Un servidor local (**LM Studio**, **Ollama**) o una API en la nube compatible con OpenAI (**OpenRouter, Groq, Mistral, Google, Hugging Face**).
  - Clave de la **API de Anthropic**
    
## Puesta en marcha

1. Clona o descarga este repositorio.
2. Abre `LinuxMint_Scripter.html` directamente en el navegador (doble clic, o `xdg-open LinuxMint_Scripter.html`).
3. Pulsa **⚙ Ajustes** y elige tu proveedor:
   - **Local / compatible OpenAI**: pulsa uno de los chips (LM Studio, Ollama…) o escribe tu propia URL base, y añade la clave si el servidor la exige.
   - **Anthropic (Claude)**: pega tu clave `sk-ant-…`.
4. Elige un modelo en el selector de la barra superior (se autocompleta con los modelos disponibles del proveedor conectado).
5. Escribe tu petición en el chat y pulsa **Enviar**.

## Flujo de uso típico

1. Describe lo que necesitas en el chat (o usa una plantilla rápida).
2. El script aparece en el editor de la derecha, con resaltado de sintaxis.
3. Revisa el aviso de comandos peligrosos o posibles errores si aparece alguno.
4. Ajusta el script a mano, pide una revisión de un fragmento concreto, o lanza la revisión iterativa completa.
5. Copia el script, guárdalo, o pulsa **📦 Instalar** para generar los pasos (o el `.sh`) de despliegue, con o sin tarea programada.

## Proveedores de modelos compatibles

| Proveedor | Tipo | Clave requerida |
|---|---|---|
| LM Studio | Local | No |
| Ollama | Local | No |
| OpenAI | Nube | Sí |
| Anthropic (Claude) | Directo, oficial | Sí |
| OpenRouter | Nube | Sí |
| Groq | Nube | Sí |
| Mistral | Nube | Sí |
| Google (Gemini / Gemma) | Nube | Sí |
| Hugging Face | Nube | Sí |
| Cualquier otro endpoint `/v1/chat/completions` | Local o nube | Depende |

## Privacidad y seguridad

- **Todo ocurre en tu navegador.** No hay servidor propio de por medio: las peticiones van directas de tu navegador al proveedor que elijas.
- Los ajustes, las claves de API y el historial de scripts se guardan solo en el `localStorage` de tu navegador.
- Puedes exportar tus claves a un `.json` (para restaurarlas en otro navegador o equipo) y protegerlo opcionalmente con contraseña — cifrado **AES-256-GCM** con derivación **PBKDF2-SHA256** (600.000 iteraciones), usando la Web Crypto API nativa del navegador, sin librerías externas.
- Si exportas las claves **sin** cifrar, trata ese archivo como una contraseña: no lo subas a un repositorio ni lo compartas.
- Sin cuentas, sin analítica, sin telemetría.

## Limitaciones conocidas

- La detección de comandos peligrosos y de errores de sintaxis es **local y heurística** (basada en patrones), no un parser completo: puede tener falsos positivos y falsos negativos. Es un apoyo, no un sustituto de revisar el script.
- El botón de ShellCheck abre **shellcheck.net** en una pestaña nueva; no hay un analizador ShellCheck real incrustado en la propia app.
- La calidad y corrección de los scripts generados depende del modelo elegido: revisa siempre el resultado antes de ejecutarlo, especialmente si requiere privilegios de administrador.

## Contribuir

Las *issues* y *pull requests* son bienvenidas. Si propones un cambio grande, abre antes una *issue* para comentar el enfoque.

## Licencia

Software libre distribuido bajo los términos de la **GNU General Public License v3.0 (GPLv3)**. Consulta el archivo [`LICENSE`](LICENSE) para el texto completo.

## Autor

Desarrollado por **Filonux**.
