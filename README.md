
# Jueves de Quack 🦆 con 🤖`cagent`🤖 

<div align="center">

[![YouTube Channel Subscribers](https://img.shields.io/youtube/channel/subscribers/UC140iBrEZbOtvxWsJ-Tb0lQ?style=for-the-badge&logo=youtube&logoColor=white&color=red)](https://www.youtube.com/c/GiselaTorres?sub_confirmation=1)
[![GitHub followers](https://img.shields.io/github/followers/0GiS0?style=for-the-badge&logo=github&logoColor=white)](https://github.com/0GiS0)
[![LinkedIn Follow](https://img.shields.io/badge/LinkedIn-Sígueme-blue?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/giselatorresbuitrago/)
[![X Follow](https://img.shields.io/badge/X-Sígueme-black?style=for-the-badge&logo=x&logoColor=white)](https://twitter.com/0GiS0)

</div>

Hola developer 👋🏻! Este repositorio contiene las demos que mostré durante el evento Jueves de Quack 🦆 con 🤖`cagent`🤖.

 <a href="https://www.youtube.com/watch?v=epBiyhp57bw" target="_blank">
                <img src="https://img.youtube.com/vi/epBiyhp57bw/maxresdefault.jpg" alt="Construyendo chats con IA 🤖 OpenAI SDK vs LangChain explicado fácil 🎯 | Cap. 11" width="100%" />
            <br/>


## ¿Pero qué es `cagent`?

`cagent` es una herramienta desarrollada por Docker que permite, usando un único archivo YAML, crear agentes conversacionales que pueden usar múltiples modelos de lenguaje, herramientas y flujos de trabajo para automatizar tareas complejas.

Lo chulo es que esta herramienta a día de hoy ya permite usar diferentes modelos de diferentes proveedores, integración con MCP Servers e incluso la posibilidad de crear multiples agentes que colaboran entre sí para resolver tareas complejas.


## 🚀 Configuración Inicial

### 1. Descargar `cagent`

```bash
# Cambia a "linux-amd64" o "windows-amd64" según tu SO
PLATFORM="darwin-amd64"

# Recuperar la última versión de cagent
URL_TO_DOWNLOAD=$(curl -fsSL https://api.github.com/repos/docker/cagent/releases/latest \
  | jq -r ".assets[] | select(.name | test(\"cagent-$PLATFORM\")) | .browser_download_url")

# Descargar
curl -fL "$URL_TO_DOWNLOAD" -o cagent

# Hacer el archivo ejecutable
chmod +x cagent

# Verificar instalación
./cagent version
```

### 2. Configurar Variables de Entorno

```bash
cp .env-sample .env
```

Para GitHub Models, crea un Personal Access Token en: https://github.com/settings/tokens

Permisos requeridos: `models:read`

```dotenv
OPENAI_API_KEY=ghp_tu_token_aqui
```

## 📋 Ejemplos Disponibles



Para probar, puedes usar esta descripción de ejemplo:

```text
Esta es la descripción de mi vídeo: En este vídeo te voy a mostrar cómo usar cagent, una herramienta desarrollada por Docker, que te permite usando un único YAML crear agentes conversacionales que pueden usar múltiples modelos de lenguaje, herramientas y flujos de trabajo para automatizar tareas complejas. Veremos cómo instalar cagent, cómo crear un agente básico, utilizarlo con GitHub Models, Ollama y MCP Servers, y finalmente cómo crear un sistema multi-agente colaborativo.
```

###  📁 0) Agente desde cero con `new`

Puedes crear un nuevo agente básico usando el comando `new` de `cagent`:

```bash
./cagent new --model openai/gpt-4.1 --max-tokens=32768
```

>[!NOTE]
>Si por defecto no especificas el proveedor intentará usar Docker Model Runner.


A continuación cada ejemplo incluye su comando de ejecución directo (copiar/pegar). Todos asumen que ya creaste tu archivo `.env` y que `./cagent` es ejecutable.

### 📁 1) `00-basic.yml` – Agente Básico

**Qué hace**: Un único agente que genera 5 títulos de YouTube con emojis (≤70 caracteres) usando OpenAI.
Ideal para: Primer contacto con la sintaxis de `cagent`.

Para ejecutarlo: 

```bash
./cagent run 00-basic.yml --env-from-file .env --debug --log-file basic.log
```

---

### 📁 2) `01-free-demo.yml` – Agente con Modelo Local (Ollama/GitHub Models)

**Qué hace**: Igual que el básico pero usando un modelo local (`gpt-oss`) vía Ollama (sin coste) o GitHub Models si no tienes un ordenador potente.
Ideal para: Desarrollo gratuito sin depender de la nube o si no tienes hardware potente.

Para ejecutarlo:

```bash
./cagent run 01-free-demo.yml --env-from-file .env --debug --log-file free.log
```

---

### 📁 3) `02-mcp-demo.yml` – Integración con MCP Servers

**Qué hace**: Un agente que puede usar servidores MCP (GitHub, Notion, Fetch, Time) + herramientas locales (filesystem, shell). Demuestra cómo orquestar capacidades externas.
Requisitos: Tener Docker y, si usas gateway MCP, que esté disponible. Configura variables como `YOUTUBE_API_KEY` si usas el server de YouTube.
Ideal para: Integrar APIs/servicios y probar toolsets.


Para ejecutarlo:

```bash
./cagent run 02-mcp-demo.yml --env-from-file .env --debug --log-file mcp.log
```

Para probar los diferentes MCP servers puedes hacer preguntas como:

```bash
./cagent run 02-mcp-demo.yml --env-from-file .env --log-file mcp.log "Quiero buscar vídeos sobre n8n en YouTube en español de este mes" 
```

```bash
./cagent run 02-mcp-demo.yml --env-from-file .env --debug --log-file mcp.log "¿puedes contarme algo de mi repo 0gis0/cagent-demos?"
```

Usar el MCP Server de GitHub para poder buscar en tu repo (en este caso todavía estaba privado):

```bash
./cagent run 02-mcp-demo.yml --env-from-file .env --debug --log-file mcp.log --tui=false "¿puedes contarme algo de mi repo 0gis0/cagent-demos?"
```

Para usar la tool `filesystem` y `shell` (asegúrate de tener archivos en el directorio actual):

```bash
./cagent run 02-mcp-demo.yml --env-from-file .env --debug --log-file mcp.log "¿Qué puedes ver en mi directorio actual?"
```

Para usar la tool fetch (asegúrate de tener conexión a internet):

```bash
./cagent run 02-mcp-demo.yml --env-from-file .env --debug --log-file mcp.log "¿Puedes buscar echar un vistazo a esta URL: https://www.returngis.net/2025/09/como-usar-los-modelos-de-ollama-con-cagent/ y darme un resumen?"
```

Para usar la tool `shell` y eliminar archivos `.log` generados:

```bash
./cagent run 02-mcp-demo.yml --env-from-file .env --debug --log-file mcp.log "Puedes eliminar los .log que tengo generados en el directorio actual?"
```

---

### 📁 4) `03-multi-agent-youtube.yml` – Sistema Multi‑Agente (Generador de Títulos YouTube)

**Qué hace**: 3 agentes (coordinador → researcher → title_generator) colaboran para producir un título recomendado basado en términos clave + investigación de YouTube (vía MCP `youtube-mcp-server`).
Ideal para: Ver paso a paso delegación y transferencia entre agentes.

>[!NOTE]
>Necesitas `YOUTUBE_API_KEY` en el entorno si usas el server de YouTube.

Para ejecutarlo (flujo completo):
```bash
./cagent run 03-multi-agent-youtube.yml --env-from-file .env --debug --log-file multi-agent.log --yolo
```

Ejecutar solo un agente (depuración del investigador):

```bash
./cagent run 03-multi-agent-youtube.yml --env-from-file .env --debug --log-file researcher.log --agent researcher
```


---

### 📁 5) `04-multi-agent-github.yml` – Multi‑Agente para Naming de Repositorios

**Qué hace**: 3 agentes generan 5 nombres de repositorio (en inglés, kebab-case) a partir de una descripción y repos similares obtenidos vía MCP GitHub.
Ideal para: Inspirarte en naming y aprender a integrar búsqueda en GitHub como parte del flujo.

Para ejecutarlo (flujo completo):
```bash
./cagent run 04-multi-agent-github.yml --env-from-file .env --debug --log-file repo-names.log --yolo
```

Ejecutar SOLO el investigador (GitHub search):
```bash
./cagent run 04-multi-agent-github.yml --env-from-file .env --debug --log-file researcher.log --agent researcher
```

### 🎯 ¿Te ha resultado útil este contenido?

**¡La mejor forma de agradecerlo es con una suscripción!** 

Cada nuevo suscriptor me motiva a seguir creando contenido de calidad y mantener estos repositorios actualizados. 

[![Suscríbete Ahora](https://img.shields.io/badge/🔔%20SUSCRÍBETE%20AHORA-red?style=for-the-badge&logo=youtube&logoColor=white)](https://www.youtube.com/c/GiselaTorres?sub_confirmation=1)



¡Nos vemos 👋🏻!