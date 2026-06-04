# 🐳 Bot Generador de Entornos Docker

<div align="center">

![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![n8n](https://img.shields.io/badge/n8n-EA4B71?style=for-the-badge&logo=n8n&logoColor=white)
![Telegram](https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white)
![Groq](https://img.shields.io/badge/Groq-F55036?style=for-the-badge&logo=groq&logoColor=white)
![OpenHands](https://img.shields.io/badge/OpenHands-22C55E?style=for-the-badge&logo=openai&logoColor=white)
![Ngrok](https://img.shields.io/badge/Ngrok-1F1E37?style=for-the-badge&logo=ngrok&logoColor=white)

**Bot de Telegram con IA que genera y despliega entornos Docker automáticamente a partir de descripciones en lenguaje natural.**

*Proyecto Final — Sistemas Informáticos · 1º DAM · IES Océano Atlántico · 2025-2026*

*Autores: Sergio Pallarés Tejedor · Izan Asin Mazuque*

</div>

---

## 📋 Índice

- [¿Qué es este proyecto?](#-qué-es-este-proyecto)
- [Arquitectura del sistema](#-arquitectura-del-sistema)
- [Tecnologías utilizadas](#-tecnologías-utilizadas)
- [Estructura del repositorio](#-estructura-del-repositorio)
- [Requisitos previos](#-requisitos-previos)
- [Instrucciones de arranque](#-instrucciones-de-arranque)
- [Uso del bot](#-uso-del-bot)
- [Niveles del proyecto](#-niveles-del-proyecto)
- [Capturas del sistema](#-capturas-del-sistema)

---

## 🤖 ¿Qué es este proyecto?

Este proyecto consiste en un **bot de Telegram inteligente** que permite generar y desplegar entornos Docker de forma completamente automática, simplemente describiendo lo que necesitas en lenguaje natural.

El usuario escribe un mensaje como:

> 💬 *"Quiero un entorno con Nginx y MySQL"*

Y el bot responde automáticamente con un `docker-compose.yml` válido y funcional generado por inteligencia artificial.

### ✨ Funcionalidades principales

- 🧠 **Generación automática** de ficheros `docker-compose.yml` mediante IA (Groq + LLaMA 3.3)
- 🚀 **Despliegue automático** de los entornos generados con OpenHands
- 💬 **Conversación fluida** sobre Docker, contenedores, redes y volúmenes
- 📋 **Comandos de gestión**: `/start`, `/help`, `/list`
- 🌐 **Acceso externo** mediante túnel Ngrok sin abrir puertos en el router
- 📱 **Todo desde el móvil** a través de Telegram

---

## 🏗️ Arquitectura del sistema

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Telegram  │────▶│     n8n     │────▶│   Groq IA   │────▶│  OpenHands  │────▶│   Docker    │
│   (móvil)   │     │  (webhook)  │     │ (LLaMA 3.3) │     │  (deploy)   │     │ (containers)│
└─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘     └─────────────┘
       │                   │                   │                   │                   │
  Usuario envía       Recibe y               Genera           Despliega el        Contenedores
   el mensaje        procesa la            compose.yml         entorno             corriendo
                     petición               con YAML
```

### Flujo detallado

1. **Usuario** → Escribe un mensaje en Telegram al bot `@dockerbot_sergio_bot`
2. **Ngrok** → Expone el webhook de n8n a internet de forma segura
3. **n8n** → Recibe el mensaje, detecta el tipo (comando o petición libre) mediante un nodo Switch
4. **Groq IA** → Procesa la petición y genera un `docker-compose.yml` válido
5. **OpenHands** → Ejecuta el compose generado y levanta los contenedores
6. **Telegram** → El bot responde al usuario con el resultado

---

## 🛠️ Tecnologías utilizadas

| Tecnología | Versión | Uso |
|------------|---------|-----|
| **Docker** | Latest | Contenedores y orquestación |
| **docker-compose** | v3.8 | Definición de servicios |
| **n8n** | Latest | Motor de automatización y flujos |
| **Groq + LLaMA 3.3** | 70B | IA gratuita para generar YAMLs |
| **OpenHands** | Main | Despliegue automático de entornos |
| **Telegram Bot API** | v6+ | Interfaz de usuario móvil |
| **Ngrok** | v3 | Túnel público para webhooks |

---

## 📁 Estructura del repositorio

```
docker-bot-proyecto/
│
├── 📄 docker-compose.yml        # Infraestructura principal (n8n + OpenHands)
├── 📄 generated-compose.yml     # Ejemplo de compose generado por la IA
├── 📄 My workflow.json          # Exportación del flujo de n8n
├── 📄 nginx.conf                # Configuración de Nginx
└── 📄 README.md                 # Este fichero
```

### Descripción de archivos

**`docker-compose.yml`** — Define los servicios principales del proyecto:
- `n8n`: Motor de automatización expuesto en el puerto `5678`
- `openhands`: Agente de despliegue expuesto en el puerto `3000`
- Red bridge `bot_network` para comunicación interna
- Volúmenes para persistencia de datos

**`My workflow.json`** — Exportación completa del flujo de n8n con todos los nodos configurados: Telegram Trigger, Switch, HTTP Request a Groq, Code in JavaScript y Send Message.

---

## ✅ Requisitos previos

Antes de arrancar el proyecto necesitas tener instalado:

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) (Windows/Mac/Linux)
- [Git](https://git-scm.com/)
- [Ngrok](https://ngrok.com/) (cuenta gratuita)
- Una cuenta en [Groq](https://console.groq.com/) para obtener la API key gratuita
- Un bot de Telegram creado con [@BotFather](https://t.me/BotFather)

---

## 🚀 Instrucciones de arranque

### 1. Clonar el repositorio

```bash
git clone https://github.com/sergiopt07/docker-bot-proyecto.git
cd docker-bot-proyecto
```

### 2. Configurar las variables de entorno

Edita el fichero `docker-compose.yml` y actualiza estas variables:

```yaml
environment:
  - N8N_BASIC_AUTH_USER=admin
  - N8N_BASIC_AUTH_PASSWORD=admin123
  - WEBHOOK_URL=https://TU-URL-DE-NGROK.ngrok-free.dev
```

### 3. Levantar los contenedores

```bash
docker compose up -d
```

Verifica que están corriendo:

```bash
docker ps
```

Deberías ver:

```
CONTAINER ID   IMAGE                              STATUS
xxxxxxxxxxxx   n8nio/n8n                          Up X seconds   0.0.0.0:5678->5678/tcp   n8n
xxxxxxxxxxxx   ghcr.io/all-hands-ai/openhands     Up X seconds   0.0.0.0:3000->3000/tcp   openhands
```

### 4. Iniciar el túnel Ngrok

```bash
ngrok http 5678
```

Copia la URL pública generada (ejemplo: `https://deception-tried-partake.ngrok-free.dev`) y actualízala en el `docker-compose.yml`.

### 5. Configurar n8n

1. Abre `http://localhost:5678` en el navegador
2. Importa el fichero `My workflow.json`
3. Configura las credenciales de Telegram con tu token de bot
4. Configura las credenciales de Groq con tu API key
5. Publica el workflow

### 6. ¡Listo!

Abre Telegram, busca tu bot y escribe `/start` para empezar. 🎉

---

## 💬 Uso del bot

### Comandos disponibles

| Comando | Descripción |
|---------|-------------|
| `/start` | Mensaje de bienvenida e introducción al bot |
| `/help` | Lista completa de comandos disponibles |
| `/list` | Ver todos los contenedores Docker activos |

### Ejemplos de peticiones

El bot entiende lenguaje natural. Puedes escribir cosas como:

```
"quiero un entorno con Nginx y MySQL"
"dame un WordPress con base de datos"
"necesito un servidor con Redis y Node.js"
"crea un entorno con Python y PostgreSQL"
"qué es un volumen en Docker?"
"cómo paro un contenedor?"
```

---

## 📊 Niveles del proyecto

### 🟡 Nivel 1 — Aprobado (5-6 puntos)
- ✅ `docker-compose.yml` con n8n y OpenHands en red bridge
- ✅ Conexión a IA gratuita (Groq + LLaMA 3.3)
- ✅ System prompt configurado para generar compose válidos
- ✅ Repositorio en GitHub con README completo

### 🔵 Nivel 2 — Notable (7-8 puntos)
- ✅ Volúmenes configurados en todos los servicios
- ✅ OpenHands despliega automáticamente el entorno generado
- ✅ Contenedores corriendo tras cada petición

### 🟢 Nivel 3 — Sobresaliente (9-10 puntos)
- ✅ Bot de Telegram creado con BotFather e integrado en n8n
- ✅ Túnel Ngrok para exponer el webhook sin abrir puertos
- ✅ Comandos `/start`, `/help` y `/list` implementados
- ✅ Conversación fluida sobre Docker en lenguaje natural

---

## 📸 Capturas del sistema

### Flujo de n8n
El workflow completo con todos los nodos conectados:

```
Telegram Trigger → Switch → /start → Send Message (bienvenida)
                          → /help  → Send Message (ayuda)
                          → /list  → Code JS → Send Message (contenedores)
                          → texto  → HTTP Request (Groq) → Code JS → Send Message
```

### Bot en Telegram
El bot responde desde el móvil en tiempo real, sin necesidad de tocar ningún panel ni terminal.

---

<div align="center">

**Sergio Pallarés Tejedor**

*1º DAM — IES Océano Atlántico — 2025-2026*

</div>
