# 🎯 Taller OpenCode: De 0 a Agentes con Skills

**Duración:** 3h | **Modalidad:** Virtual | **Modelo:** Gratuito (Zen Free / DeepSeek / Groq)

**Repo de skills del instructor:** https://github.com/gabalzate/skills-cheveres-gabriel

---

## ⏱️ Bloque 1 — Introducción (25 min)

### 1.1 ¿Qué es OpenCode?
- Agente de código abierto (160K ⭐ GitHub)
- Gratuito: modelos gratis incluidos (Zen Free)
- Multi-provider: DeepSeek, Groq, Gemini, OpenAI, etc.

### 1.2 Instalación exprés
```bash
curl -fsSL https://opencode.ai/install | bash
```

### 1.3 ¿Qué vamos a construir hoy?
- HTMLs visuales con Three.js que abrimos en el navegador
- Scraping con Playwright
- Skills que encapsulan conocimiento reusable

### 1.4 Demo rápida
```bash
opencode run 'Responde solo: Hola Taller OpenCode!'
```

### 1.5 Instalar skills del repo del instructor
```bash
# Crear estructura de skills
mkdir -p .opencode/skills

# Clonar skills del repo
git clone https://github.com/gabalzate/skills-cheveres-gabriel /tmp/skills-cheveres

# Copiar skills al proyecto
cp -r /tmp/skills-cheveres/skills/* .opencode/skills/

# Verificar instalación
ls .opencode/skills/
# Debería mostrar: ds-brainstorm ds-implement ds-plan ds-review ds-verify
```

---

## ⏱️ Bloque 2 — Proceso Normal (40 min)

### 2.1 El agente sin instrucciones
- Le pedimos algo a OpenCode sin contexto
- El agente improvisa: decide estructura, librerías, todo desde 0

### 🖐️ Ejercicio 1 (15 min)
```bash
opencode run 'Crea un HTML con Three.js que muestre un sistema solar interactivo. Un solo archivo que pueda abrir en el navegador.'
```
- Cada asistente ejecuta y **abre el HTML en su navegador**
- Observan: ¿qué tan bonito quedó? ¿tiene controles? ¿animación?

### 2.2 Lo que pasó por dentro
- El agente tomó decisiones sin contexto
- No sabía nuestro estándar de calidad
- No sabía qué librerías preferimos
- No sabía qué nivel de detalle queremos

### 2.3 Problemas del proceso "crudo"
- ❌ Sin plan: el agente adivina el camino
- ❌ Sin contexto: ignora convenciones
- ❌ Sin verificación: no sabe cuándo está "bien"
- ❌ Sin reuso: mañana empezamos de nuevo

---

## ⏱️ Bloque 3 — Skills (45 min)

### 3.1 ¿Qué es un Skill?
- Una **receta reusable** que le dice al agente **cómo** hacer algo bien
- No es código — es **conocimiento procedural**
- Se guarda en `.opencode/skills/<nombre>/SKILL.md`

### 3.2 Anatomía de un Skill
```markdown
---
name: visual-prototype
description: "Crear prototipos visuales en HTML con Three.js. Un solo archivo, CDN, listo para abrir en navegador."
---

# visual-prototype

## Reglas
- Un solo archivo HTML (todo inline)
- Three.js vía CDN
- OrbitControls para interacción
- Fondo oscuro, diseño responsivo
```

### 3.3 Nuestro primer Skill: `visual-prototype`

Lo creamos en `.opencode/skills/visual-prototype/SKILL.md`:

```markdown
---
name: visual-prototype
description: "Crear prototipos visuales en HTML con Three.js. Un solo archivo autónomo que se abre en el navegador."
---

# visual-prototype

## Reglas
- Un solo archivo HTML con CSS y JS inline
- Three.js desde CDN: https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js
- OrbitControls desde CDN
- Fondo oscuro (#0a0a0a), diseño responsivo
- Rotación automática + interacción mouse
- Sin servidores — solo abrir el archivo .html
```

### 🖐️ Ejercicio 2 (20 min)
1. Crean la carpeta `.opencode/skills/visual-prototype/` con el SKILL.md
2. Ejecutan:
```bash
opencode run --skill visual-prototype \
  'Crea un sistema solar con planetas orbitando. Cada planeta de diferente color, tamaño y velocidad orbital.'
```
3. Abren el HTML en el navegador
4. **Comparan** con el resultado del Ejercicio 1 — ¿mejor?

---

## ⏱️ Bloque 4 — Más Skills (40 min)

### 4.1 Skill de scraping: `web-scraper`

```markdown
---
name: web-scraper
description: "Extraer datos de sitios web usando Playwright. Un solo script .mjs que exporta JSON."
---

# web-scraper

## Reglas
- Usar Playwright (npx playwright)
- Un solo script .mjs
- Manejo de errores y timeouts
- Guardar resultado en data/output.json
```

### 🖐️ Ejercicio 3 (10 min)
```bash
opencode run --skill web-scraper \
  'Extrae los 20 titulares de Hacker News con su puntuación y guarda en data/hackernews.json. Incluye: título, puntos, URL, autor.'
```
- **Output:** `data/hackernews.json` — lo usaremos en el siguiente ejercicio

---

## ⏱️ Bloque 5 — Pipeline de Datos con Skills (25 min)

### 5.1 El flujo ds-* (5 fases)

Pipeline completo de data science con skills:

```
ds-brainstorm → ds-plan → ds-implement → ds-review → ds-verify
```

| Fase | Skill | Qué hace |
|------|-------|----------|
| 1 | `ds-brainstorm` | Clarifica objetivos con preguntas |
| 2 | `ds-plan` | Perfilea datos y crea plan |
| 3 | `ds-implement` | Ejecuta análisis con verificación |
| 4 | `ds-review` | Revisa metodología y calidad |
| 5 | `ds-verify` | Confirma reproducibilidad |

### 🖐️ Ejercicio 4 (20 min): Pipeline de datos completo

**Paso 1:** Ejecutar ds-brainstorm
```bash
opencode run --skill ds-brainstorm \
  'Quiero analizar los titulares de Hacker News que acabamos de scrapear para entender qué temas son más populares. Los datos están en data/hackernews.json'
```
- El agente hará preguntas sobre objetivos
- Se creará `docs/SPEC.md`

**Paso 2:** Ejecutar las 5 fases
```bash
opencode run --skill ds-brainstorm --skill ds-plan --skill ds-implement --skill ds-review --skill ds-verify \
  'Analiza data/hackernews.json. Quiero saber qué temas son más populares y visualizar los datos en un HTML interactivo.'
```

- **Output esperado:**
  - `docs/SPEC.md` — especificación del análisis
  - `docs/PLAN.md` — plan de tareas
  - `docs/LEARNINGS.md` — aprendizajes del análisis
  - `docs/VERIFICATION_REPORT.md` — reporte de verificación
  - HTML con visualización interactiva de los datos

### 5.2 Comparativa final

| Aspecto | Sin Skills | Con Skills |
|---|---|---|
| Consistencia | Cada vez es diferente | Misma calidad siempre |
| Velocidad | Improvisa | Sigue la receta |
| Reusabilidad | Empieza de 0 | Skills acumulan conocimiento |
| Colaboración | Individual | Skills en git, compartibles |
| Curva de aprendizaje | Cada sesión es nueva | Skills mejoran con el tiempo |

---

## ⏱️ Bloque 6 — Cierre (10 min)

### 6.1 Buenas prácticas
- Skills **atómicos**: una responsabilidad cada uno
- Skills en `.opencode/skills/` dentro del repo del proyecto
- Versionar skills con git
- Compartir skills entre el equipo

### 6.2 Para seguir explorando
- [OpenCode Docs](https://opencode.ai/docs)
- [OpenCode GitHub](https://github.com/opencode-ai)
- Skills de ejemplo: https://github.com/gabalzate/skills-cheveres-gabriel
- Playwright: `npx playwright`

### 6.3 Q&A