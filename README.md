# 🎯 Sistema de Scouting Automatizado — CS2

Proyecto Final — Carrera Desarrollador Full Stack (Coderhouse)
**Entrega Final: Ecosistema de Automatización IA Autónomo para Negocios**

---

## 📋 Descripción

Plataforma de scouting automatizada para reclutar jugadores de CS2/FACEIT en un equipo semi-profesional. El sistema evalúa candidatos combinando filtros numéricos y una evaluación cualitativa con IA (RAG), con un **punto de pausa humano (HITL)** obligatorio antes de cualquier acción crítica — ningún candidato es aprobado sin la validación manual de un scout.

El proyecto integra las 4 categorías obligatorias de la consigna:

| Categoría | Herramienta |
|---|---|
| 🔧 Orquestador | [n8n](https://n8n.io/) (local) |
| 🗄️ Base de datos | [Airtable](https://airtable.com/) |
| 🤖 Procesamiento IA | Google Gemini (`gemini-3.6-flash`) + RAG |
| 📲 Canal de salida | WhatsApp vía [CallMeBot](https://www.callmebot.com/) — validado en producción |

---

## 🧠 Cómo funciona

El sistema se compone de **dos workflows** en n8n que trabajan en cadena:

### 1️⃣ Workflow de Evaluación
Busca candidatos nuevos, valida sus datos, aplica un filtro numérico (KD y winrate mínimos), y evalúa cualitativamente con Gemini a los que pasan el filtro —contrastándolos contra reglas de reclutamiento (RAG). Los aprobados por la IA quedan en estado **"Para revisión"**, esperando aprobación manual del scout (HITL).

### 2️⃣ Workflow de Asignación y Notificación
Corre después de que el scout aprueba manualmente. Busca a los candidatos aprobados, los ordena por mérito, verifica si hay cupo disponible en el equipo, y si lo hay, los aprueba definitivamente y **notifica por WhatsApp** en tiempo real.

📄 La arquitectura completa, con diagramas, código de cada nodo y prompts de IA, está documentada en el PDF de este repo.

---

## 📁 Contenido de este repositorio

| Archivo | Descripción |
|---|---|
| `sistema-scouting-cs2-documentacion-final.pdf` | Documentación técnica completa: arquitectura, diagramas, estructura de datos, optimización de costos, seguridad y resiliencia, capturas de ejecución. |
| `Sistema_de_Scouting_Automatizado_CS2_EvaluacionWF1.json` | Blueprint exportado del Workflow de Evaluación (importable en n8n). |
| `Sistema_de_Scouting_Automatizado_CS2_Asignacion_NotificacionWF2.json` | Blueprint exportado del Workflow de Asignación y Notificación (importable en n8n). |

---

## 🔗 Dashboard en vivo (solo lectura)

Base de Airtable con el estado real del sistema, vía Airtable Interfaces:

- 📊 **Candidatos** (vista Kanban por estado): https://airtable.com/appQg0p4WXjLI8y3m/shrnHHZz1NeKpp6ho
- ⚙️ **Config** (cupos del equipo): https://airtable.com/appQg0p4WXjLI8y3m/shrGZe733ZXeirE5r
- 🕒 **Actividad reciente** (log por fecha de modificación): https://airtable.com/appQg0p4WXjLI8y3m/shrsAiSSZnAgFXw99

---

## ✅ Evidencia de funcionamiento

El sistema fue probado en corridas reales de punta a punta. Como evidencia más concluyente, un candidato aprobado (Wimbo) generó una notificación real recibida por WhatsApp:

> *"Nuevo recluta: Wimbo (KD 0.96, Winrate 0.53) fue APROBADO."*

Esta captura, junto con el resto de la evidencia (canvas de ambos workflows ejecutados exitosamente, estados en Airtable, justificaciones generadas por la IA), está incluida en el PDF de documentación. En la corrida final —ya con Retry On Fail activo en los nodos críticos—, **TopPlayer** fue el último candidato validado de punta a punta, completando exitosamente todo el pipeline del Workflow 2.

---

## 🛡️ Seguridad y resiliencia

Los nodos que dependen de servicios externos (Google Gemini, Airtable, CallMeBot) tienen **reintentos automáticos** configurados (Retry On Fail) para tolerar fallos transitorios de red, deteniendo el workflow de forma segura si el problema persiste tras varios intentos — evitando estados inconsistentes en los datos.

---

## 👤 Autor

Proyecto desarrollado como entrega final de la carrera Desarrollador Full Stack de Coderhouse.
