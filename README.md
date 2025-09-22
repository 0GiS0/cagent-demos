
# Demos Jueves de Quack: 🤖`cagent`🤖 con GitHub Models 

Hola developer 👋🏻! Este repositorio contiene ejemplos prácticos de configuración de 🤖`cagent`🤖.

## ¿Pero qué es `cagent`?

`cagent` es una herramienta desarrollada por Docker que permite, usando un único archivo YAML, crear agentes conversacionales que pueden usar múltiples modelos de lenguaje, herramientas y flujos de trabajo para automatizar tareas complejas.

Lo chulo es que esta herramienta a día de hoy ya permite usar diferentes modelos de diferentes proveedores, integración con MCP Servers e incluso la posibilidad de crear multiples agentes que colaboran entre sí para resolver tareas complejas.

## 📋 Ejemplos Disponibles

### 📁 `00-basic.yml` - Agente Básico
**Propósito**: Configuración básica de un solo agente especializado en generar títulos de YouTube.

**Características**:
- ✅ Un solo agente (`root`) con instrucciones específicas
- ✅ Usa GitHub Models (GPT-5) como proveedor
- ✅ Optimizado para generar 5 títulos atractivos con emojis
- ✅ Límite de 70 caracteres por título

**Caso de uso**: Ideal para empezar con `cagent` y entender la configuración básica.

---

### 📁 `01-free-demo.yml` - Agente con Ollama
**Propósito**: Demostración usando modelos locales gratuitos con Ollama.

**Características**:
- ✅ Modelo principal: Ollama local (`gpt-oss`)
- ✅ Alternativa configurada: GitHub Models
- ✅ Misma funcionalidad que el ejemplo básico
- ✅ Completamente gratis al usar modelos locales

**Caso de uso**: Cuando quieres usar modelos locales sin depender de APIs externas.

---

### 📁 `02-mcp-demo.yml` - Integración con MCP Servers
**Propósito**: Demostración avanzada de integración con Model Context Protocol (MCP) servers.

**Características**:
- ✅ **MCP Servers remotos**: GitHub, Notion, Fetch, Time
- ✅ **Herramientas built-in**: Sistema de archivos, shell
- ✅ Configuración con Docker MCP Gateway
- ✅ Timezone configurado para Europa/Madrid

**Caso de uso**: Cuando necesitas que tu agente interactúe con servicios externos como GitHub, Notion, etc.

---

### 📁 `03-multi-agent.yml` - Sistema Multi-Agente
**Propósito**: Ejemplo complejo de coordinación entre múltiples agentes especializados.

**Arquitectura**:
```
root (coordinador)
├── investigador → Busca videos relacionados en YouTube
└── generador_titulo → Crea títulos basados en la investigación
```

**Características**:
- ✅ **3 agentes especializados** con roles específicos
- ✅ **Workflow coordinado**: investigación → generación → entrega
- ✅ **Integración YouTube**: Búsqueda automática de videos relacionados
- ✅ **Múltiples proveedores**: GitHub Models, Ollama (comentado)

**Caso de uso**: Tareas complejas que requieren especialización y coordinación entre agentes.

---

## 🚀 Configuración Inicial

### 1. Descargar `cagent`

```bash
URL_TO_DOWNLOAD=$(curl -fsSL https://api.github.com/repos/docker/cagent/releases/latest \
  | jq -r '.assets[] | select(.name | test("cagent-darwin-amd64")) | .browser_download_url')
curl -fL "$URL_TO_DOWNLOAD" -o cagent
chmod +x cagent
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

---

## 🎯 Guía de Ejecución

### Ejemplo Básico (GitHub Models)
```bash
./cagent run 00-basic.yml --env-from-file .env --debug --log-file basic.log
```
**💡 Perfecto para**: Primeros pasos y pruebas rápidas

### Ejemplo con Ollama (Gratis)
```bash
./cagent run 01-free-demo.yml --env-from-file .env --debug --log-file ollama.log
```
**💡 Perfecto para**: Desarrollo local sin costos

### Ejemplo con MCP Servers (Avanzado)
```bash
./cagent run 02-mcp-demo.yml --env-from-file .env --debug --log-file mcp.log
```
**💡 Perfecto para**: Integración con servicios externos

### Ejemplo Multi-Agente (Complejo)
```bash
./cagent run 03-multi-agent.yml --env-from-file .env --debug --log-file multi-agent.log --yolo
```
**💡 Perfecto para**: Workflows complejos y coordinación de tareas

---

## 📊 Comparativa de Ejemplos

| Ejemplo | Complejidad | Agentes | Herramientas | Costo | Caso de Uso |
|---------|-------------|---------|--------------|-------|-------------|
| `00-basic.yml` | ⭐ Básico | 1 | Ninguna | 💰 Bajo | Aprendizaje inicial |
| `01-free-demo.yml` | ⭐ Básico | 1 | Ninguna | 🆓 Gratis | Desarrollo local |
| `02-mcp-demo.yml` | ⭐⭐⭐ Avanzado | 1 | MCP, FS, Shell | 💰 Medio | Integración externa |
| `03-multi-agent.yml` | ⭐⭐⭐⭐ Complejo | 3 | MCP, Think | 💰💰 Alto | Workflows complejos |

---

## 🔧 Opciones de Línea de Comandos

- `--env-from-file .env`: Carga variables de entorno desde archivo
- `--debug`: Muestra información detallada de ejecución
- `--log-file nombre.log`: Guarda logs en archivo específico
- `--yolo`: Modo permisivo para ejecución experimental

### Generador de Títulos de YouTube (Multi-agente en Español)

Nuevo ejemplo: Un flujo de tres agentes que colaboran para generar el mejor título optimizado de YouTube en español a partir de una descripción del vídeo.

Archivo: `03-multi-agent.yml`

Roles:
- `coordinador`: Orquesta el flujo, delega primero a investigación y luego a generación.
- `investigador`: Usa un MCP (`mcp-youtube`) para buscar títulos existentes, detectar patrones, palabras clave y oportunidades.
- `generador_titulo`: Propone entre 6 y 10 títulos optimizados (<=60 caracteres ideal) y selecciona uno recomendado con justificación.

Ejecución:
```bash
./cagent run 03-multi-agent.yml --env-from-file .env --debug --log-file yt-titulos.log --yolo
```

Para probar, puedes usar esta descripción de ejemplo:

```text
Esta es la descripción de mi vídeo: En este vídeo te voy a mostrar cómo usar cagent, una herramienta desarrollada por Docker, que te permite usando un único YAML crear agentes conversacionales que pueden usar múltiples modelos de lenguaje, herramientas y flujos de trabajo para automatizar tareas complejas. Veremos cómo instalar cagent, cómo crear un agente básico, utilizarlo con GitHub Models, Ollama y MCP Servers, y finalmente cómo crear un sistema multi-agente colaborativo.
```


¡Nos vemos 👋🏻!