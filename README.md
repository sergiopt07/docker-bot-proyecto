# 🤖 Bot Telegram — Google Calendar

Bot personal para gestionar tu Google Calendar desde Telegram,
con alertas automáticas 7 días antes de cada evento.

---

## ⚡ Comandos disponibles

| Comando | Acción |
|---------|--------|
| `/hoy` | Eventos de hoy |
| `/semana` | Eventos de los próximos 7 días |
| `/mes` | Eventos de los próximos 30 días |
| `/crear` | Instrucciones para crear un evento |
| `/borrar` | Muestra botones para borrar eventos |
| `/editar` | Muestra botones para editar eventos |
| `/alertas` | Lista de próximos eventos con días restantes |

**Crear evento** (escribir directamente):
```
CREAR | Título | DD/MM/YYYY HH:MM | Lugar | Descripción
```

**Editar evento** (tras seleccionar con /editar):
```
EDITAR | ID_EVENTO | título=Nuevo título, lugar=Nuevo lugar
```

---

## 🛠 Instalación paso a paso

### PASO 1 — Crear el bot en Telegram

1. Abre Telegram y busca **@BotFather**
2. Escríbele `/newbot`
3. Pon un nombre y un username (ej: `MiCalendarioBot`)
4. Guarda el **TOKEN** que te da (algo como `123456:ABCdef...`)

5. Para saber tu Chat ID: busca **@userinfobot** en Telegram
   y escríbele cualquier cosa. Te dirá tu ID numérico.

---

### PASO 2 — Activar Google Calendar API

1. Ve a [console.cloud.google.com](https://console.cloud.google.com)
2. Crea un proyecto nuevo (ej: "Mi Bot Calendar")
3. Ve a **APIs y servicios → Biblioteca**
4. Busca "Google Calendar API" y actívala
5. Ve a **APIs y servicios → Credenciales**
6. Haz clic en **"+ Crear credenciales" → ID de cliente OAuth**
7. Tipo de aplicación: **Aplicación de escritorio**
8. Descarga el fichero JSON y renómbralo `credentials.json`

9. Ve a **"Pantalla de consentimiento OAuth"**
   - Tipo: **Externo**
   - Añade tu correo de Google en "Usuarios de prueba"

---

### PASO 3 — Generar el token de acceso (en tu ordenador)

```bash
# Instala dependencias
pip install -r requirements.txt

# Pon el credentials.json en esta carpeta y ejecuta:
python generar_token.py
```

Se abrirá el navegador para que autorices el acceso.
Al terminar, se genera `token.json`. **Guarda su contenido**, lo necesitarás en el Paso 5.

---

### PASO 4 — Subir el código a Railway

1. Ve a [railway.app](https://railway.app) y regístrate con GitHub
2. Haz clic en **"New Project" → "Deploy from GitHub repo"**
   - O crea un repo en GitHub, sube estos ficheros y conéctalo
3. Railway detectará automáticamente el `requirements.txt`

---

### PASO 5 — Configurar las variables de entorno en Railway

En Railway, ve a tu proyecto → **Variables** y añade:

| Variable | Valor |
|----------|-------|
| `TELEGRAM_TOKEN` | El token de @BotFather |
| `YOUR_CHAT_ID` | Tu Chat ID numérico |
| `TIMEZONE` | `Europe/Madrid` |
| `TOKEN_JSON` | El contenido del `token.json` (todo el JSON) |

⚠️ El `token.json` necesita un pequeño ajuste: en el `bot.py` ya está
preparado para leerlo desde la variable de entorno. Solo copia el
contenido completo del fichero como valor de `TOKEN_JSON`.

---

### PASO 6 — ¡A funcionar!

Railway desplegará el bot automáticamente.
Busca tu bot en Telegram y escríbele `/start`.

---

## 🔔 Alertas automáticas

El bot revisa tu calendario cada día a las **9:00 AM** (hora de Madrid).
Si encuentra algún evento exactamente **7 días después**, te manda una alerta:

```
🔔 Recordatorio — 7 días

📅 Concierto en el Teatro
🕐 Sáb 15/06  20:00
📍 Teatro Principal

Quedan 7 días para este evento.
```

---

## 📁 Estructura del proyecto

```
telegram-calendar-bot/
├── bot.py              # Bot principal
├── google_calendar.py  # Conexión con Google Calendar API
├── generar_token.py    # Script de autorización (solo 1 vez)
├── requirements.txt    # Dependencias
├── railway.toml        # Config para Railway
└── README.md
```
