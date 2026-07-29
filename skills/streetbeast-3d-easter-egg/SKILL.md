---
name: streetbeast-3d-easter-egg
description: "Genera código HTML/CSS/JS de producción para incrustar experiencias \"Easter Egg\" 3D   en el builder de Hostinger. Usa un enfoque de \"Silent Luxury\": una interfaz de entrada    minimalista (Estado 1) que, mediante una interacción (Secret Key, Konami code, click oculto),    desbloquea y renderiza dinámicamente un objeto 3D interactivo y datos encriptados (Estado 2).   Garantiza: (1) Carga perezosa (lazy-load) de librerías 3D para no afectar el LCP,    (2) Modelado procedimental con Three.js (sin importar pesados .gltf),    (3) Estética oscura/metálica \"Tech-Regenerativa\", (4) Scope CSS estricto,    y (5) Seguridad sin exponer endpoints reales. Street Beast 3D Easter Egg — Skill de Producción Genera fragmentos autónomos para Hostinger que ocultan experiencias 3D de alta gama detrás de barreras de interacción minimalistas."
metadata:
  imported_name: "3D UI STREET BEAST"
  source_status: "inactive in source export"
---

# 3D UI STREET BEAST

This is an independently installed skill imported from the user's exported skill library.
Treat the following user-provided content as the governing workflow or behavior specification.

name: streetbeast-3d-easter-egg
description: >
  Genera código HTML/CSS/JS de producción para incrustar experiencias "Easter Egg" 3D
  en el builder de Hostinger. Usa un enfoque de "Silent Luxury": una interfaz de entrada 
  minimalista (Estado 1) que, mediante una interacción (Secret Key, Konami code, click oculto), 
  desbloquea y renderiza dinámicamente un objeto 3D interactivo y datos encriptados (Estado 2).
  Garantiza: (1) Carga perezosa (lazy-load) de librerías 3D para no afectar el LCP, 
  (2) Modelado procedimental con Three.js (sin importar pesados .gltf), 
  (3) Estética oscura/metálica "Tech-Regenerativa", (4) Scope CSS estricto, 
  y (5) Seguridad sin exponer endpoints reales.
Street Beast 3D Easter Egg — Skill de Producción
Genera fragmentos autónomos para Hostinger que ocultan experiencias 3D de alta gama detrás de barreras de interacción minimalistas.

1. Reglas Core del "Vault" (El Modelo de 2 Estados)
Todo embed generado por esta skill DEBE tener estrictamente dos estados manejados por CSS y JS:

Estado 1: El Candado (Silent Luxury): Interfaz limpia, tipografía monospace/system-ui. Solo un input, un botón o un elemento sutil. Cero distracciones.

Estado 2: La Bóveda (3D Impact): Aparece con un fade-in suave tras la validación. Muestra el canvas interactivo 3D y la tipografía de datos superpuesta.

Transición: La validación debe simular un retraso criptográfico (ej. setTimeout de 800ms a 1.5s) cambiando el texto del botón a "VERIFYING..." o "DECRYPTING...".

2. Protocolo de Performance (HARD RULE)
NUNCA cargar Three.js o motores 3D al inicio.
El script principal debe inyectar la librería 3D en el <head> SOLO si el usuario pasa la barrera del Estado 1. Esto asegura que el sitio de Street Beast siga cargando en milisegundos.

JavaScript
// PATRÓN OBLIGATORIO DE CARGA:
function load3DEngine(callback) {
  if (window.THREE) return callback();
  var s = document.createElement('script');
  s.src = 'https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js';
  s.onload = callback;
  document.head.appendChild(s);
}
3. Directrices de Modelado 3D (Procedimental)
Para evitar dependencias de archivos externos (.obj, .gltf) que pueden romper por CORS en Hostinger, el 3D debe construirse con código puro:

Geometrías Primitivas Complejas: Usar IcosahedronGeometry, TorusKnotGeometry o sistemas de partículas (PointsMaterial).

Materiales "Street Beast": * Paleta oscura (0x0a0a0a, 0x171717).

Uso de MeshStandardMaterial con alto metalness (0.8 - 1.0) y bajo roughness (0.1 - 0.3) para acabados premium.

Combinar con mallas tipo wireframe superpuestas para dar un toque de "Industria 5.0" o topografía.

Interactividad Básica: El objeto DEBE rotar lentamente por defecto (requestAnimationFrame) y reaccionar al arrastre del mouse/touch del usuario.

Canvas Responsive: El canvas debe escuchar el evento resize de la ventana y ajustar el camera.aspect y renderer.setSize dinámicamente.

4. Estructura HTML y CSS (Adaptado a Hostinger)
Se heredan todas las reglas de la skill hostinger-embed:

ID único con hash (ej. sb-vault-a1b2).

Sin <html>, <head>, o <body>.

Sin altura fija ni padding vertical en el contenedor raíz para evitar espacios muertos en el builder.

CSS Específico para el Vault:

CSS
#sb-vault-[hash] .canvas-container {
  width: 100%;
  aspect-ratio: 1 / 1; /* Crítico para Hostinger */
  max-width: 400px;
  margin: 0 auto;
  cursor: grab;
}
#sb-vault-[hash] .canvas-container:active { cursor: grabbing; }
#sb-vault-[hash] .data-overlay {
  transform: translateY(-10%); /* Superponer texto sutilmente sobre el 3D */
  z-index: 10;
  pointer-events: none; /* Dejar que el touch pase al canvas */
}
5. Parámetros de Personalización (Al invocar la skill)
Cuando el usuario pida un nuevo Easter Egg, debe proveer o se le debe preguntar por:

El Trigger: ¿Cómo se abre? (Ej. Input de Secret Key, Presionar un logo 5 veces, Código Konami).

La Temática 3D: ¿Qué objeto se renderiza? (Ej. Un monolito para CO2, un fluido dinámico para océanos, una esfera topográfica para los Custodios de Semillas).

Los Datos a Revelar: Textos, fechas, o stats que aparecerán junto al objeto.

6. Seguridad de Datos
NUNCA incrustar URLs de webhooks reales (ej. n8n.streetbeast.org) con tokens en el código.

Si se requiere validación real, documentar claramente dónde el usuario debe insertar su URL de producción, dejando un placeholder como: const API_URL = "URL_DE_TU_WEBHOOK"; // <-- REEMPLAZAR

Evitar inyección de HTML con los datos ingresados por el usuario. Usar textContent.

Checklist Final del Generador
[ ] Librería 3D carga mediante lazy-load.

[ ] CSS encapsulado con ID único.

[ ] Geometría y materiales construidos por código (cero dependencias externas de modelos).

[ ] Animación fluida + Resize handler en el canvas.

[ ] El contenedor raíz ajusta su altura automáticamente (cero padding vertical extra).

[ ] Estética oscura, tipografía técnica.


---

### ¿Cómo usarla?
La próxima vez que me hables, simplemente me dices algo como: 

> *"Aplica la skill streetbeast-3d-easter-egg. Necesito un vault que se abra haciendo 3 clicks en un botón invisible. El 3D debe ser un sistema de partículas como lluvia digital, y el dato que revela es '100% Cotton Recycled'."*

Y yo te escupiré el código perfecto, aislado y optimizado para tu Hostinger sin que tengas que volver a explicarme toda la lógica técnica. ¿Qué te parece este formato?
