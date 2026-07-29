---
name: sb-design-lab
description: "Ejecuta auditorías de diseño, genera 5 variaciones de UI en un entorno temporal, recopila feedback y produce planes de implementación técnicos. Diseñado estrictamente para el ecosistema STREET BEAST y su estética de \"Hype Puro\" y \"System Restore\"."
metadata:
  imported_name: "Sb Design Lab"
  source_status: "inactive in source export"
---

# Sb Design Lab

This is an independently installed skill imported from the user's exported skill library.
Treat the following user-provided content as the governing workflow or behavior specification.

---
name: sb-design-lab
description: Ejecuta auditorías de diseño, genera 5 variaciones de UI en un entorno temporal, recopila feedback y produce planes de implementación técnicos. Diseñado estrictamente para el ecosistema STREET BEAST y su estética de "Hype Puro" y "System Restore".
---

# sb-design-lab

Fase 0: Detección de Integridad (Preflight)
Antes de iniciar, el sistema debe escanear la infraestructura actual:
Gestor de Paquetes: pnpm, yarn, npm, bun.
Framework: Next.js (App/Pages), Vite, etc.
Sistema de Estilos: Extraer variables directamente de tailwind.config.js o globales. Regla de Oro: Nunca usar estilos genéricos. Todo debe emanar del lenguaje visual del proyecto.
Fase 1: Interrogatorio de Marca (Interview)
Usa la herramienta de preguntas al usuario para definir los parámetros operativos.
Pregunta 1: Dominio de Operación
"¿Para qué entorno estamos diseñando?"
Opciones: .co (Hype puro, 100% visual, cero eco-talk), .org (The Vault, transparencia, datos crudos, regeneración), o Interno (Herramientas del equipo).
Pregunta 2: Estética Base
"¿Qué línea visual rige este drop?"
Opciones: Earth Rare (Lujo silencioso, premium hype), System Restore (Brutalismo, Windows 95, errores cínicos), Raw Data (Mapas topográficos, binarios, coordenadas).
Pregunta 3: Objetivo de la UI
"¿Qué problema estamos aniquilando con este rediseño?"
Opciones: (Dejar abierto para texto).
Fase 2: Blueprint (Design Brief)
Genera un archivo JSON en .claude-design/design-brief.json con los datos recopilados, estableciendo las reglas de juego antes de tirar código.
Fase 3: The Grid (Generación de Variantes)
Construye el laboratorio en .claude-design/lab/. Obligatorio: Cada variante debe explorar una de las rutas estratégicas de STREET BEAST:
Variante A (La Ruta Conservadora): Diseño fundamentado en los estándares más altos de la industria del streetwear. Lujo silencioso, UX impecable, jerarquía visual de conversión rápida.
Variante B (La Ruta Innovadora): Funcionalidad emergente. Interacciones micro-animadas, optimización extrema para móvil, flujos de usuario optimizados para escasez artificial (Drops).
Variante C (La Ruta Disruptiva): Rompe el status quo. Estética "System Restore" o "Raw Data". Diseño asimétrico, brutalismo digital, mensajes de error intencionales para generar impacto psicológico.
Variante D (Densidad y Datos): Enfoque para la .org. Visualización de datos duros (20kg de CO2e mitigados, coordenadas de reforestación) en un formato crudo e industrial.
Variante E (Maximum Hype): Prioridad absoluta en la imagen del producto (ropa de 300g) usando componentes expansivos y minimalismo extremo.
Nota Crítica: El componente FeedbackOverlay.tsx debe inyectarse siempre para permitir revisión táctica.
Fase 4 & 5: Feedback Loop
Presenta el laboratorio temporal al usuario. Recibe comentarios interactivos a través del Overlay. Analiza qué elementos de lujo visual, interacción técnica o despliegue de datos funcionales prefiere el usuario.
Fase 6 & 7: Síntesis y Render Final
Combina los elementos ganadores en la Variante F (El Frankestein Perfecto). Crea la ruta /__design_preview para la aprobación final.
Fase 8: Ejecución y Autodestrucción (Cleanup & Manifest)
Protocolo de Autodestrucción: Una vez aprobado (o abortado), elimina absolutamente todo el rastro temporal (.claude-design/, /__design_lab).
El Manifiesto (DESIGN_PLAN.md): Escribe el plan de implementación técnico paso a paso, incluyendo accesibilidad, tokens de diseño y requerimientos de API para el Supply Chain o los bonos de biodiversidad, si aplica.
Memoria del Sistema (DESIGN_MEMORY.md): Actualiza el cerebro de la marca con los nuevos patrones visuales aprobados.
