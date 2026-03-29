# Content Coach — Instrucciones Completas de Instalacion

**Sistema de IA Local para Creadores de Contenido**

---

## Paso 1 — Descargar el Archivo

Descarga el archivo `content-coach-project.tar.gz` que se te entrego en el chat de Claude.

---

## Paso 2 — Descomprimir en tu Computadora

Abre tu terminal normal (no Claude Code todavia) y escribe:

```bash
mkdir ~/content-coach
tar -xzf ~/Downloads/content-coach-project.tar.gz -C ~/content-coach
```

Si lo descargaste en otra carpeta, cambia `~/Downloads/` por la ruta donde este el archivo.

Para verificar que se descomprimio bien:

```bash
ls ~/content-coach
```

Deberias ver: `CLAUDE.md`, `setup.sh`, `docker-compose.yml`, `.env`, y carpetas como `prompts/`, `scripts/`, `sql/`, `docs/`, `shared/`, etc.

---

## Paso 3 — Obtener las API Keys Gratuitas

Necesitas dos API keys. Ambas son 100% gratis y **NO piden tarjeta de credito**.

### Gemini (Google AI Studio)

1. Abrir [Google AI Studio](https://aistudio.google.com)
2. Iniciar sesion con tu cuenta de Google
3. Click en **"Get API Key"**
4. Click en **"Create API Key"**
5. Copiar la key (empieza con `AIza...`)
6. Guardarla en algun lado temporalmente

### Groq

1. Abrir [Groq Console](https://console.groq.com)
2. Registrarte con Google o GitHub
3. Ir a la seccion **"API Keys"**
4. Click en **"Create API Key"**
5. Copiar la key (empieza con `gsk_...`)
6. Guardarla en algun lado temporalmente

---

## Paso 4 — Configurar tus Datos

Abrir el archivo `.env` con cualquier editor de texto.

**En Mac/Linux:**

```bash
nano ~/content-coach/.env
```

**En Windows (si usas Notepad):**

```
notepad C:\Users\TU_USUARIO\content-coach\.env
```

Buscar y cambiar estas lineas con tus datos **REALES**:

```env
GEMINI_API_KEY=pegar_tu_key_de_gemini_aqui
GROQ_API_KEY=pegar_tu_key_de_groq_aqui
CREATOR_NICHE=tu_nicho_aqui
CREATOR_AUDIENCE=tu_audiencia_aqui
CREATOR_TONE=tu_tono_aqui
TZ=America/Mexico_City
```

### Ejemplos de nicho

marketing digital, fitness, tecnologia, cocina, finanzas personales, desarrollo personal, etc.

### Ejemplos de audiencia

emprendedores 25-40 anos, estudiantes universitarios, madres jovenes, etc.

### Ejemplos de tono

cercano y directo, profesional pero accesible, humoristico y casual, motivacional, etc.

### Ejemplos de timezone

| Region | Timezone |
|---|---|
| Mexico | `America/Mexico_City` |
| Colombia | `America/Bogota` |
| Peru | `America/Lima` |
| Argentina | `America/Buenos_Aires` |
| Chile | `America/Santiago` |
| Espana | `Europe/Madrid` |
| Venezuela | `America/Caracas` |
| Ecuador | `America/Guayaquil` |

Si usas nano, guardar con `Ctrl+O`, presionar Enter, y salir con `Ctrl+X`.

---

## Paso 5 — Instalar Docker Desktop

> Si ya tienes Docker instalado, salta al Paso 6.

1. Ir a [Docker Desktop](https://docker.com/products/docker-desktop)
2. Descargar para tu sistema operativo (Mac, Windows o Linux)
3. Instalar siguiendo el asistente
4. Abrir Docker Desktop
5. Esperar hasta que diga **"Docker is running"** o muestre el icono verde en la barra de tareas

**IMPORTANTE:** Docker Desktop debe estar corriendo ANTES de continuar con el siguiente paso.

---

## Paso 6 — Levantar Todo el Sistema

En tu terminal:

```bash
cd ~/content-coach
docker compose up -d
```

Esto descarga e inicia tres servicios automaticamente:

- **n8n** — el orquestador de workflows
- **PostgreSQL** — la base de datos
- **Ollama** — la IA local gratuita e ilimitada

La primera vez tarda entre 3 y 10 minutos porque descarga las imagenes de Docker. Las siguientes veces arranca en segundos.

Para verificar que los tres servicios estan corriendo:

```bash
docker compose ps
```

Deberias ver tres contenedores con estado "Up":

- `n8n_coach`
- `postgres_coach`
- `ollama_coach`

---

## Paso 7 — Descargar el Modelo Local de IA

Ejecutar una sola vez:

```bash
docker exec ollama_coach ollama pull llama3.1:8b
```

Esto descarga el modelo de IA local (Llama 3.1 de 8B). Pesa aproximadamente 4.7 GB. Solo se hace una vez. Sirve como respaldo ilimitado cuando las APIs gratis lleguen a su limite diario.

---

## Paso 8 — Verificar que Todo Funciona

```bash
bash ~/content-coach/scripts/health_check.sh
```

Deberias ver algo asi:

```
✅ n8n:        http://localhost:5678 — ACTIVO
✅ PostgreSQL:  localhost:5432 — ACTIVO
   └─ Tablas: 8
✅ Ollama:      localhost:11434 — ACTIVO
   └─ Modelos: llama3.1:8b
✅ Gemini:      Configurada
✅ Groq:        Configurada
```

Si algo sale con ❌, revisar:

- Docker Desktop esta corriendo?
- Las API keys en `.env` estan bien pegadas?
- Ejecutaste `docker compose up -d`?

---

## Paso 9 — Abrir n8n

Abrir tu navegador web y ir a:

```
http://localhost:5678
```

Te pedira credenciales:

- **Usuario:** `admin`
- **Password:** `ContentCoach2026`

Desde aqui puedes importar los workflows del proyecto. Ir a **Settings → Import Workflow** → seleccionar los archivos JSON de la carpeta `workflows/`.

---

## Paso 10 — Abrir con Claude Code

Abrir tu terminal y ejecutar:

```bash
cd ~/content-coach
claude
```

Claude Code leera el archivo `CLAUDE.md` automaticamente y entendera todo el proyecto: que es, como funciona, que workflows hay, que prompts usar, que APIs tiene, etc.

Ahora puedes hablarle naturalmente. Ejemplos:

- *"Crea el workflow del cazador de tendencias"*
- *"Escribeme un guion sobre como ganar seguidores en TikTok"*
- *"Audita estas metricas de mi contenido"*
- *"Genera 10 hooks para un video sobre productividad"*
- *"Dame ideas de contenido para esta semana"*
- *"Exporta el plan semanal a Markdown"*

---

## Uso Diario — Rutina Recomendada

### Por la manana

Revisar la carpeta `shared/ideas/` donde aparecen las tendencias y sugerencias del dia.

### Cuando quieras crear contenido

Decirle a Claude Code *"escribeme un guion sobre [tema]"* y el usa los prompts profesionales que ya tiene. El guion se exporta listo en `shared/guiones/`.

### Antes de grabar

Pedirle *"audita este guion"* y te da feedback detallado con score por categoria (hook, retencion, valor, CTA, etc.) y te dice si esta listo para grabar o que mejorar.

### Despues de publicar

Registrar tu contenido y metricas. Puedes llenar la plantilla CSV en `templates/metricas_template.csv` y ejecutar:

```bash
python3 scripts/import_metrics.py shared/metricas-csv/mi_archivo.csv
```

### Fines de semana

Pedirle que ejecute la auditoria semanal. Genera un reporte completo con score general, que funciono, que no, patrones detectados y plan de accion para la proxima semana. Se guarda en `shared/auditorias/`.

---

## Comandos Utiles

| Accion | Comando |
|---|---|
| Levantar el sistema | `cd ~/content-coach && docker compose up -d` |
| Apagar el sistema | `cd ~/content-coach && docker compose down` |
| Ver si todo corre bien | `bash scripts/health_check.sh` |
| Hacer backup de la BD | `bash scripts/backup_db.sh` |
| Exportar plan semanal | `python3 scripts/export_plan.py --semana actual` |
| Importar metricas | `python3 scripts/import_metrics.py shared/metricas-csv/archivo.csv` |
| Ver logs de n8n | `docker compose logs -f n8n` |
| Actualizar n8n | `docker compose pull n8n && docker compose up -d n8n` |
| Abrir n8n | `http://localhost:5678` (admin / ContentCoach2026) |

---

## Que Hace Cada Carpeta

| Carpeta | Descripcion |
|---|---|
| `prompts/` | Los 8 prompts de IA profesionales (cazador, ideas, guiones, auditor, etc.) |
| `scripts/` | Herramientas: health check, backup, importar metricas, exportar plan |
| `sql/` | Esquema de la base de datos (se carga automatico al crear PostgreSQL) |
| `templates/` | Plantillas CSV para registrar metricas y contenido publicado |
| `shared/guiones/` | Aqui se exportan los guiones escritos |
| `shared/ideas/` | Aqui aparecen las ideas y tendencias |
| `shared/auditorias/` | Aqui se guardan las auditorias |
| `shared/exportados/` | Planes semanales y otros exports |
| `shared/metricas-csv/` | Dejar aqui tus CSV de metricas |
| `docs/` | Guia de inicio rapido, como usar workflows y preguntas frecuentes |
| `workflows/` | Archivos JSON para importar en n8n |
| `backups/` | Backups automaticos de la base de datos |

---

## Solucion de Problemas

### "Docker no arranca"

Asegurate de que Docker Desktop esta abierto y corriendo. En Windows puede requerir reiniciar la computadora despues de instalar.

### "n8n no responde en localhost:5678"

Ejecutar: `docker compose logs n8n`
Verificar que el puerto 5678 no este ocupado por otra app.

### "Error de API key"

Verificar que las keys en `.env` no tienen espacios extra ni comillas. Deben estar exactamente asi:

```
GEMINI_API_KEY=AIzaSyD...
```

(sin comillas, sin espacios)

### "Ollama muy lento"

Es normal en CPU. Si tienes GPU NVIDIA, editar `docker-compose.yml` y descomentar las lineas de GPU en el servicio ollama.

### "Se me acabaron los requests gratis"

Gemini da 250 por dia, Groq da 1,000 por dia. Se reinician a medianoche hora del Pacifico. Mientras tanto, Ollama funciona como respaldo ilimitado.

### "Quiero usar esto sin n8n"

Los prompts en la carpeta `prompts/` funcionan en cualquier chat de IA. Puedes copiar y pegar los prompts directamente en Claude, ChatGPT, Gemini o cualquier otro.

---

## Como Retomar en Otra Cuenta de Claude

Si pierdes acceso a este chat:

1. Abre una nueva conversacion en Claude (cualquier cuenta)
2. Copia y pega el contenido del archivo `CLAUDE.md`
3. Dile: *"Continua desde donde quedamos, estoy en Fase X"*
4. Claude tendra todo el contexto del proyecto

El archivo `CLAUDE.md` es tu memoria persistente. Funciona en Claude chat, Claude Code, o cualquier otro LLM.

---

**Stack:** n8n + PostgreSQL + Ollama + Gemini + Groq
**Costo:** $0 (todo gratuito)
