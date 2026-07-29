---
name: street-beast-motion-integrity
description: "Auditoría de alto nivel y ejecución quirúrgica para eliminar el layout thrashing, optimizar el compositor y asegurar que cada transición de usuario sea impecable. Uso obligatorio para mantener el estándar de lujo técnico y evitar la degradación de la experiencia \"Hype\"."
metadata:
  imported_name: "Street Beast Motion Integrity"
  source_status: "inactive in source export"
---

# Street Beast Motion Integrity

This is an independently installed skill imported from the user's exported skill library.
Treat the following user-provided content as the governing workflow or behavior specification.

---
name: street-beast-motion-integrity
description: Auditoría de alto nivel y ejecución quirúrgica para eliminar el layout thrashing, optimizar el compositor y asegurar que cada transición de usuario sea impecable. Uso obligatorio para mantener el estándar de lujo técnico y evitar la degradación de la experiencia "Hype".
---

# street-beast-motion-integrity

📋 PROTOCOLO DE INTERACCIÓN (EL "BEAST MODE")
/street-beast-motion-integrity: Activa los filtros de rendimiento más estrictos para cualquier trabajo de UI.
/street-beast-motion-integrity [archivo]: Auditoría forense del código. Reportaré:
"Vulnerabilidades": Violaciones exactas (la línea del crimen).
"Daño al Brand ID": Por qué el lag destruye la percepción de nuestra marca premium.
"Protocolo de Reparación": La solución técnica definitiva (código optimizado).
🛡️ LAS REGLAS DEL CÓDIGO (EL CÓDIGO DE HONOR)
PRIORIDAD	CATEGORÍA	MANDAMIENTO BEAST
1	Reglas de Oro	Cero Layout Thrashing. Si lees el DOM, no lo escribas en el mismo frame. El diseño es sagrado; no lo sacudas.
2	Mecanismo	Solo transform y opacity. El resto es ruido. Si la GPU no lo puede manejar, no entra en el despliegue.
3	Medición	Mide una vez, ejecuta siempre. Si el código mide getBoundingClientRect repetidamente, estás fuera del equipo.
4	Scroll	Prohibido el scrollTop o scrollY en loops de JS. Todo debe ser ScrollTimeline nativo. La fluidez es ley.
5	Layers	will-change se usa como un bisturí, no como un escudo. Úsalo quirúrgicamente o no lo toques.
6	Efectos	Blur y filtros = lujo efímero. Pequeños, rápidos y nunca continuos. Nada de animar filtros en grandes superficies.
⚡ COMMON FIXES (STREET BEAST EDITION)
"Si no es fluido, es un error de sistema. Si es un error de sistema, es una falla de nuestra marca."
Evitar el layout thrashing: Si estás animando el ancho, cámbialo a scale.
❌ No: transition: width 0.3s;
✅ Sí: transition: transform 0.3s; will-change: transform;
Scroll-linked motion (Nivel Profesional):
❌ No: window.addEventListener('scroll', ...)
✅ Sí: @keyframes fade-in { ... } .el { animation: fade-in linear; animation-timeline: view(); }
