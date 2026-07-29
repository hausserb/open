---
name: hostinger-embed
description: " Genera código HTML/CSS/JS de producción para el elemento \"Incrustar código\" del Creador de Sitios Web de Hostinger.   Usar SIEMPRE que el usuario pida un widget, catálogo, galería, carrusel, formulario, botón, CTA, tabla, mapa,   contador, countdown, popup, testimonios, FAQ, acordeón, calculadora, configurador, embed de terceros, o cualquier   componente visual/interactivo. Activar también con frases como \"embed de Hostinger\", \"incrustar en Hostinger\",   \"widget para Hostinger\", La skill garantiza: (1) entrevista de color para ajustar el embed al fondo real del sitio, (2) centrado horizontal   correcto y altura ajustada al contenido sin espacio vacío, (3) CSS con scope sin colisiones, (4) accesibilidad   WCAG AA, (5) responsive mobile-first, (6) revisión de seguridad/privacidad — sin fugas de API keys,   secretos, emails personales, endpoints internos, ni datos sensibles."
metadata:
  imported_name: "Hostinger Embed"
  source_status: "inactive in source export"
---

# Hostinger Embed

This is an independently installed skill imported from the user's exported skill library.
Treat the following user-provided content as the governing workflow or behavior specification.

# Hostinger Embed — Skill de Producción
 
Genera fragmentos HTML/CSS/JS autónomos, accesibles, seguros y centrados para el elemento **Incrustar código** del Creador de Sitios Web de Hostinger.
 
---
 
## Tabla de contenidos
 
1. Modelo mental del entorno
2. Protocolo de seguridad y privacidad (OBLIGATORIO)
3. Patrón base y sistema de diseño
4. Centrado robusto — 4 técnicas según caso
5. Biblioteca de plantillas
6. Accesibilidad WCAG AA
7. Performance
8. Checklist final de entrega
9. Instrucciones para el usuario
10. Contexto de negocio del usuario
---
 
## 1. Modelo mental del entorno
 
El elemento "Incrustar código" de Hostinger inyecta HTML **dentro del `<body>` de la página**, sin iframe aislado. Implicaciones:
 
| Restricción | Razón |
|---|---|
| NO incluir `<html>`, `<head>`, `<body>`, `<!DOCTYPE>` | El builder ya los provee; duplicarlos rompe el DOM |
| NO usar `document.write()` | Corrompe el DOM después de parseo inicial |
| CSS obligatoriamente con scope por ID único | El CSS global del tema colisiona con clases genéricas (`.card`, `.btn`, `.container`) |
| URLs en HTTPS sin espacios | HTTP es bloqueado por navegadores modernos; espacios rompen iframes |
| Sin `<link rel="stylesheet">` a CSS externos pesados | El builder ya carga su stack; añadir Bootstrap/Tailwind full genera FOUC y conflictos |
| JS encapsulado en IIFE | Evita contaminar `window` y permite reutilizar el mismo embed varias veces |
| Elementos `position: fixed` con cuidado | Pueden quedar debajo del header/footer del builder |
| Sin `alert()`, `confirm()`, `prompt()` | Experiencia amateur y bloqueante |
| Sin `eval()` ni `new Function()` | Vector de ataque y bloqueado por CSP agresivas |
 
**El builder soporta:** HTML5 moderno, CSS3 (grid, flexbox, custom properties, container queries), JS ES6+ (fetch, async/await), Web Components básicos.
 
---
 
## 2. Protocolo de seguridad y privacidad (OBLIGATORIO)
 
**Antes de entregar CUALQUIER código, ejecutar este escaneo sobre el fragmento completo.** Si cualquiera de los puntos siguientes dispara, detenerse, avisar al usuario y corregir.
 
### 2.1 Datos que JAMÁS pueden aparecer en el código del embed
 
El embed es **público**. Cualquiera con "Ver código fuente" lo verá. Por tanto, nunca incluir:
 
- **API keys, tokens, secrets** de cualquier servicio (Teemill API key, OpenAI, SendGrid, Stripe secret, JWT, webhook secrets, credenciales de Chatwoot/n8n/Evolution API/Postiz, contraseñas de PostgreSQL, etc.).
- **URLs internas de Tailscale** (`*.tail*.ts.net`), IPs privadas (`10.*`, `192.168.*`, `100.*`), o el hostname `internal-host` / `internal-host`.
- **Endpoints de servicios self-hosted que no deban ser públicos**: `n8n.example.com/webhook-test/*`, rutas admin de Chatwoot, Coolify, OpenWebUI, Postiz.
- **Emails personales del dueño** (usar solo emails corporativos públicos tipo `hola@example.com`).
- **Números de teléfono personales** (solo líneas de negocio publicadas).
- **Direcciones físicas privadas** (solo direcciones de negocio publicadas).
- **Nombres reales de empleados/familia** que no estén públicos.
- **IDs de clientes, pedidos, transacciones** reales.
- **Comentarios con notas internas** ("TODO: pedir key a Brian", "server en 203.0.113.10", etc.).
### 2.2 Patrones a usar en su lugar
 
- **Claves públicas de terceros OK** (Google Maps API key con restricción de dominio, reCAPTCHA site key, Stripe **publishable** key — nunca secret).
- **Proxies vía n8n**: si el embed necesita datos autenticados, crear webhook público en n8n que haga la autenticación server-side. El embed llama al webhook, no al servicio directamente.
- **Placeholders explícitos**: si se necesita que el usuario rellene una key pública, usar `{{API_KEY_PUBLICA}}` y documentar en comentario.
- **Formularios**: enviar a webhook público con rate limiting, nunca exponer credenciales SMTP/email.
### 2.3 Protecciones defensivas en el código generado
 
Todo embed que maneje input o datos dinámicos debe incluir:
 
- **Sanitización al renderizar contenido dinámico**: usar `textContent` en lugar de `innerHTML`, o función `escapeHTML` al construir markup desde datos.
- **`rel="noopener noreferrer"`** en todo `<a target="_blank">` para evitar tabnabbing.
- **`loading="lazy"`** en imágenes no críticas y en iframes.
- **Rate-limit visual**: deshabilitar botón tras submit para evitar dobles envíos.
- **Sin scripts de dominios no confiables**: preferir código inline sobre CDNs desconocidos.
- **Honeypot** en formularios (campo oculto que solo bots rellenan).
### 2.4 Pregunta de verificación
 
Antes de entregar, preguntar al usuario si alguna parte del código contiene datos que deberían ser confidenciales. Frase sugerida: *"Revisé el código y no veo secretos expuestos. ¿Confirmas que la URL X, el email Y y el dominio Z son públicos y está bien que los visitantes los vean?"*
 
---
 
## 2.5 Entrevista de color — PASO OBLIGATORIO ANTES DE GENERAR CÓDIGO
 
Antes de escribir una sola línea de CSS, preguntar al usuario:
 
> **"¿Cuál es el color de fondo de la sección donde va a ir el embed?"**
> Si puede compartir el hex exacto, mejor. Si no, describir con palabras ("fondo blanco puro", "negro carbón", "beige crema", "azul marino oscuro").
 
Con esa respuesta, determinar automáticamente la paleta de contraste:
 
### Tabla de decisión de paleta
 
| Fondo del sitio | Luminosidad | Paleta a aplicar en tokens |
|---|---|---|
| Blanco / crema / gris muy claro (`#fff`, `#f5f5f5`, `#fafaf8`) | Claro | `--hb-bg: transparent; --hb-fg: #0a0a0a; --hb-accent: #0a0a0a; --hb-accent-fg: #fff` |
| Gris medio (`#888`–`#bbb`) | Medio | Evaluar contraste con ambos extremos; preferir fg oscuro con borde visible |
| Negro / carbón / gris oscuro (`#0a0a0a`, `#111`, `#1a1a1a`) | Oscuro | `--hb-bg: transparent; --hb-fg: #f5f5f5; --hb-accent: #f5f5f5; --hb-accent-fg: #0a0a0a` |
| Color saturado (azul, verde, rojo…) | Variable | Derivar: fg en blanco `#fff` si la luminosidad relativa del fondo es < 0.179 (WCAG), negro si > 0.179 |
| Imagen de fondo / gradiente | Incierto | Añadir `backdrop-filter: none` y usar el color dominante aproximado; advertir al usuario |
 
### Regla de contraste mínimo (WCAG AA)
- Texto normal: relación ≥ **4.5:1**
- Texto grande (≥18pt / ≥24px, o bold ≥14pt / ≥18.67px): relación ≥ **3:1**
- Componentes UI (bordes de input, iconos funcionales): ≥ **3:1**
**Fórmula rápida mental**: sobre fondo oscuro puro `#0a0a0a`, el blanco `#fff` da ~19:1. Sobre blanco, el negro da ~21:1. Cualquier color medio debe verificarse.
 
### Fondo transparente por defecto
 
Siempre usar `background: transparent` en el wrapper raíz (no `#fff` ni `#000`) para heredar el fondo real del builder:
 
```css
#hb-[id].hb-root {
  background: transparent; /* hereda el fondo del builder */
}
```
 
Solo añadir `background` explícito si el widget necesita una "tarjeta" con fondo propio diferenciado (ej. card con `background: var(--hb-card-bg)`). En ese caso, ese fondo interno es el que entra en el cálculo de contraste contra el texto interno de la card.
 
### Neutralizar el `prefers-color-scheme` si el usuario confirma el fondo
Si el usuario dice que su sitio tiene fondo oscuro fijo (no responde a dark mode del sistema), **no incluir** el bloque `@media (prefers-color-scheme)` — los tokens ya estarán correctos y el media query los sobreescribiría innecesariamente.
 
---
 
## 3. Patrón base y sistema de diseño
 
### 3.1 Estructura canónica
 
Todo embed sigue tres capas:
 
```html
<div id="hb-[tipo]-[hash]" class="hb-root">
  <style>
    #hb-[tipo]-[hash].hb-root { /* reset + centrado */ }
    #hb-[tipo]-[hash] .inner { /* contenido */ }
  </style>
  <div class="inner">...</div>
</div>
<script>
(function(){
  'use strict';
  var root = document.getElementById('hb-[tipo]-[hash]');
  if (!root) return;
  /* lógica */
})();
</script>
```
 
**Hash**: 6 caracteres alfanuméricos aleatorios (`hb-catalog-a3f7k2`). Permite múltiples embeds del mismo tipo en una página sin colisiones.
 
### 3.2 Design tokens (CSS custom properties)
 
```css
#hb-[id].hb-root {
  --hb-bg:#fff; --hb-fg:#0a0a0a; --hb-muted:#666; --hb-border:#e5e5e5;
  --hb-accent:#0a0a0a; --hb-accent-fg:#fff;
  --hb-danger:#dc2626; --hb-success:#16a34a;
  --hb-font: system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  --hb-size-sm:.875rem; --hb-size-base:1rem; --hb-size-lg:1.25rem; --hb-size-xl:1.875rem;
  --hb-sp-1:4px; --hb-sp-2:8px; --hb-sp-3:12px; --hb-sp-4:16px; --hb-sp-6:24px; --hb-sp-8:32px;
  --hb-radius:10px; --hb-radius-sm:6px;
  --hb-shadow:0 2px 12px rgba(0,0,0,.08); --hb-shadow-lg:0 8px 32px rgba(0,0,0,.12);
  --hb-trans:200ms ease;
  all: revert;
  font-family: var(--hb-font);
  color: var(--hb-fg);
  line-height: 1.5;
  box-sizing: border-box;
}
#hb-[id].hb-root *,
#hb-[id].hb-root *::before,
#hb-[id].hb-root *::after { box-sizing: border-box; }
```
 
### 3.3 Dark mode automático
 
```css
@media (prefers-color-scheme: dark) {
  #hb-[id].hb-root {
    --hb-bg:#0a0a0a; --hb-fg:#f5f5f5; --hb-muted:#a3a3a3; --hb-border:#262626;
    --hb-accent:#f5f5f5; --hb-accent-fg:#0a0a0a;
  }
}
```
 
### 3.4 Responsive mobile-first
 
Desarrollar primero para 320px; escalar con `@media (min-width: 640px)` y `(min-width: 1024px)`. Tipografía fluida con `clamp()`:
 
```css
font-size: clamp(1rem, 2.5vw, 1.25rem);
```
 
---
 
## 4. Centrado y altura — reglas críticas para Hostinger
 
### El problema real de altura en Hostinger
 
El builder de Hostinger permite ajustar el **ancho** del contenedor del embed arrastrando sus bordes, pero **no el alto** — la altura del elemento se ajusta automáticamente al contenido HTML. Esto genera dos problemas si el código no está bien escrito:
 
1. **Espacio en blanco excesivo arriba/abajo** — causado por `padding` generoso, `min-height`, o `height` fijos en el wrapper raíz.
2. **Contenido cortado o desbordado** — causado por `height` fijo que no refleja el contenido real.
**Solución**: el wrapper raíz nunca lleva `height`, `min-height`, ni `padding-top`/`padding-bottom` grandes. La altura la dicta el contenido. El padding vertical va en el elemento interno (`.inner`), no en el root.
 
```css
/* MAL — crea espacio en blanco vacío que el usuario no puede quitar */
#hb-[id].hb-root {
  min-height: 300px;
  padding: 40px 16px;
}
 
/* BIEN — la altura se ajusta exactamente al contenido */
#hb-[id].hb-root {
  width: 100%;
  /* sin height, sin min-height, sin padding vertical en el root */
}
#hb-[id] .inner {
  padding: 16px; /* solo el necesario */
  /* el padding vertical aquí sí es controlable */
}
```
 
### Centrado horizontal (siempre requerido)
 
Usar según el tipo de contenido:
 
**Técnica A — Flex center** (botón CTA, card única, elemento aislado):
```css
#hb-[id].hb-root {
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: flex-start;
}
```
 
**Técnica B — Grid place-items** (catálogos, galerías, layouts multi-columna):
```css
#hb-[id].hb-root {
  width: 100%;
  display: grid;
  place-items: start center;
}
#hb-[id] .inner {
  width: 100%;
  max-width: 1100px;
  padding: 16px;
}
```
 
**Técnica C — Overlay fixed** (popups, modals — caso especial):
```css
#hb-[id] .overlay {
  position: fixed;
  inset: 0;
  display: grid;
  place-items: center;
  background: rgba(0,0,0,.6);
  z-index: 9999;
  padding: 16px;
}
```
 
**Técnica D — Aspect-ratio** (iframes, videos — sin padding-bottom vertical en root):
```css
#hb-[id].hb-root { width: 100%; }
#hb-[id] .inner {
  width: 100%;
  max-width: 800px;
  aspect-ratio: 16 / 9;
  margin-inline: auto;
}
#hb-[id] .inner iframe { width: 100%; height: 100%; border: 0; }
```
 
### Centrado vertical — cuándo y cuándo NO
 
**No centrar verticalmente el root**: el root ocupa exactamente lo que ocupa su contenido. Centrar verticalmente (`align-items: center`) en el root crea espacio vacío en la zona que el builder no puede recortar.
 
**Sí centrar verticalmente** dentro de componentes hijos que tienen altura propia definida:
```css
/* Dentro de una card de altura fija, centrar su contenido */
#hb-[id] .card {
  height: 200px; /* altura justificada por diseño, no por el root */
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
```
 
### Resumen de altura mínima por tipo de widget
 
| Tipo de widget | Padding vertical recomendado | Notas |
|---|---|---|
| Botón CTA simple | `8px 0` en `.inner` | Solo el botón, sin espacio extra |
| Card producto | `0` en root; card define su propia altura | `aspect-ratio: 1` en imagen |
| Formulario | `16px` en `.inner` | Sin min-height en root |
| Galería / grid | `16px` en `.inner` | Altura dictada por las imágenes |
| FAQ / acordeón | `8px` en `.inner` | Se expande con el contenido |
| Iframe / mapa | `0` en root; `aspect-ratio` en `.inner` | Ver Técnica D |
| Countdown | `16px` en `.inner` | Solo los dígitos + labels |
| Testimonios scroll-snap | `16px` en `.inner` | Altura fija en las cards individuales, no en el root |
 
 
 
## 5. Biblioteca de plantillas
 
Cada plantilla es un punto de partida. Adaptar tokens, contenido y estructura al caso concreto.
 
### 5.1 Comercio
 
#### Catálogo de productos (grid con filtros)
 
```html
<div id="hb-catalog-{HASH}" class="hb-root" role="region" aria-label="Catálogo de productos">
  <style>
    #hb-catalog-{HASH}.hb-root {
      --hb-bg:#fff; --hb-fg:#0a0a0a; --hb-muted:#666; --hb-border:#e5e5e5;
      --hb-accent:#0a0a0a; --hb-accent-fg:#fff; --hb-radius:10px;
      --hb-shadow:0 2px 12px rgba(0,0,0,.08); --hb-trans:200ms ease;
      --hb-font:system-ui,-apple-system,"Segoe UI",Roboto,sans-serif;
      all:revert; font-family:var(--hb-font); color:var(--hb-fg); line-height:1.5;
      display:grid; place-items:start center; width:100%; padding:16px; box-sizing:border-box;
    }
    #hb-catalog-{HASH} *,#hb-catalog-{HASH} *::before,#hb-catalog-{HASH} *::after{box-sizing:border-box}
    #hb-catalog-{HASH} .inner{width:100%;max-width:1100px}
    #hb-catalog-{HASH} .filters{display:flex;flex-wrap:wrap;gap:8px;justify-content:center;margin-bottom:24px}
    #hb-catalog-{HASH} .filter-btn{
      padding:8px 16px;border:1px solid var(--hb-border);background:transparent;
      border-radius:999px;font-size:.875rem;cursor:pointer;transition:var(--hb-trans);color:inherit;
    }
    #hb-catalog-{HASH} .filter-btn:hover{background:var(--hb-fg);color:var(--hb-bg)}
    #hb-catalog-{HASH} .filter-btn[aria-pressed="true"]{background:var(--hb-fg);color:var(--hb-bg)}
    #hb-catalog-{HASH} .filter-btn:focus-visible{outline:2px solid var(--hb-accent);outline-offset:2px}
    #hb-catalog-{HASH} .grid{
      display:grid;grid-template-columns:repeat(auto-fill,minmax(220px,1fr));gap:20px;
    }
    #hb-catalog-{HASH} .card{
      background:var(--hb-bg);border:1px solid var(--hb-border);border-radius:var(--hb-radius);
      overflow:hidden;display:flex;flex-direction:column;transition:var(--hb-trans);
    }
    #hb-catalog-{HASH} .card:hover{box-shadow:var(--hb-shadow);transform:translateY(-2px)}
    #hb-catalog-{HASH} .card img{width:100%;aspect-ratio:1;object-fit:cover;display:block;background:#f5f5f5}
    #hb-catalog-{HASH} .card-body{padding:12px;display:flex;flex-direction:column;gap:4px;flex:1;text-align:center}
    #hb-catalog-{HASH} .card-title{font-size:.9rem;font-weight:600;margin:0}
    #hb-catalog-{HASH} .card-price{font-size:.95rem;color:var(--hb-muted);margin:0 0 8px}
    #hb-catalog-{HASH} .card-cta{
      margin-top:auto;padding:10px 16px;background:var(--hb-accent);color:var(--hb-accent-fg);
      text-decoration:none;border-radius:6px;font-size:.85rem;font-weight:600;
      display:inline-block;transition:var(--hb-trans);
    }
    #hb-catalog-{HASH} .card-cta:hover{opacity:.85}
    #hb-catalog-{HASH} .card-cta:focus-visible{outline:2px solid var(--hb-accent);outline-offset:2px}
    @media (prefers-color-scheme: dark){
      #hb-catalog-{HASH}.hb-root{--hb-bg:#0a0a0a;--hb-fg:#f5f5f5;--hb-muted:#a3a3a3;--hb-border:#262626;--hb-accent:#f5f5f5;--hb-accent-fg:#0a0a0a}
      #hb-catalog-{HASH} .card img{background:#171717}
    }
    @media (prefers-reduced-motion: reduce){
      #hb-catalog-{HASH} *{animation-duration:.01ms!important;transition-duration:.01ms!important}
    }
  </style>
  <div class="inner">
    <div class="filters" role="group" aria-label="Filtros de categoría">
      <button class="filter-btn" aria-pressed="true" data-cat="all">Todos</button>
      <button class="filter-btn" aria-pressed="false" data-cat="camisetas">Camisetas</button>
      <button class="filter-btn" aria-pressed="false" data-cat="sudaderas">Sudaderas</button>
    </div>
    <div class="grid" id="hb-catalog-{HASH}-grid"></div>
  </div>
</div>
<script>
(function(){
  'use strict';
  var root = document.getElementById('hb-catalog-{HASH}');
  if (!root) return;
  var grid = root.querySelector('#hb-catalog-{HASH}-grid');
 
  // Fuente de datos inline. Para catálogo dinámico, reemplazar por fetch a webhook PÚBLICO.
  // NUNCA poner API keys aquí — usar proxy n8n si se necesita autenticación.
  var products = [
    // {id:'RNA1', cat:'camisetas', title:'Nombre', price:'$25.00', img:'https://...', url:'https://shop.example.com/product/...'},
  ];
 
  function esc(s){return String(s).replace(/[&<>"']/g,function(c){return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]})}
 
  function render(filter){
    grid.innerHTML = '';
    var frag = document.createDocumentFragment();
    products.filter(function(p){return filter==='all'||p.cat===filter}).forEach(function(p){
      var card = document.createElement('article');
      card.className='card';
      card.innerHTML =
        '<img src="'+esc(p.img)+'" alt="'+esc(p.title)+'" loading="lazy" decoding="async">'+
        '<div class="card-body">'+
          '<h3 class="card-title">'+esc(p.title)+'</h3>'+
          '<p class="card-price">'+esc(p.price)+'</p>'+
          '<a class="card-cta" href="'+esc(p.url)+'" target="_blank" rel="noopener noreferrer" aria-label="Comprar '+esc(p.title)+'">Comprar</a>'+
        '</div>';
      frag.appendChild(card);
    });
    grid.appendChild(frag);
  }
 
  root.querySelectorAll('.filter-btn').forEach(function(btn){
    btn.addEventListener('click', function(){
      root.querySelectorAll('.filter-btn').forEach(function(b){b.setAttribute('aria-pressed','false')});
      btn.setAttribute('aria-pressed','true');
      render(btn.dataset.cat);
    });
  });
 
  render('all');
})();
</script>
```
 
#### Tarjeta producto individual (hero)
Versión simplificada: Técnica A + `max-width: 500px`, imagen grande, descripción, botón CTA.
 
#### Comparador de planes (tabla)
Usar `<table>` semántica con `<th scope="col">`. En mobile, colapsar a una columna por plan con `@media (max-width: 640px)`.
 
### 5.2 Marketing
 
#### CTA centrado con múltiples opciones
 
```html
<div id="hb-cta-{HASH}" class="hb-root">
  <style>
    #hb-cta-{HASH}.hb-root{
      --hb-accent:#0a0a0a;--hb-accent-fg:#fff;--hb-radius:8px;--hb-trans:200ms ease;
      --hb-font:system-ui,-apple-system,sans-serif;
      all:revert;font-family:var(--hb-font);
      display:flex;flex-direction:column;align-items:center;gap:16px;
      width:100%;padding:32px 16px;box-sizing:border-box;text-align:center;
    }
    #hb-cta-{HASH} h2{margin:0;font-size:clamp(1.5rem,4vw,2.25rem);font-weight:700;line-height:1.2}
    #hb-cta-{HASH} p{margin:0;max-width:560px;color:#666;font-size:1rem}
    #hb-cta-{HASH} .btns{display:flex;flex-wrap:wrap;gap:12px;justify-content:center;margin-top:8px}
    #hb-cta-{HASH} .btn{
      padding:14px 32px;border-radius:var(--hb-radius);font-size:1rem;font-weight:600;
      text-decoration:none;transition:var(--hb-trans);border:2px solid var(--hb-accent);
      cursor:pointer;display:inline-block;
    }
    #hb-cta-{HASH} .btn-primary{background:var(--hb-accent);color:var(--hb-accent-fg)}
    #hb-cta-{HASH} .btn-secondary{background:transparent;color:var(--hb-accent)}
    #hb-cta-{HASH} .btn:hover{opacity:.85}
    #hb-cta-{HASH} .btn:focus-visible{outline:2px solid var(--hb-accent);outline-offset:3px}
  </style>
  <h2>Titular que engancha</h2>
  <p>Subtítulo descriptivo con la propuesta de valor en una frase.</p>
  <div class="btns">
    <a class="btn btn-primary" href="https://..." target="_blank" rel="noopener noreferrer">Acción principal</a>
    <a class="btn btn-secondary" href="https://..." target="_blank" rel="noopener noreferrer">Acción secundaria</a>
  </div>
</div>
```
 
#### Countdown timer
`setInterval` + `Date.now()`. Fecha objetivo en `data-target="2026-12-31T23:59:59Z"`. Render días/horas/minutos/segundos. Al expirar: mostrar "Finalizado" y `clearInterval`. **Seguridad**: validar que la fecha objetivo no revele información privada (ej. fecha de un evento interno).
 
#### Popup newsletter con consentimiento GDPR
- Solo una aparición por sesión (`sessionStorage` con try/catch para modo privado).
- Casilla consentimiento explícita (no pre-marcada).
- Link a política de privacidad obligatorio.
- Cerrar con botón, overlay click, y tecla ESC.
- Usar `<dialog>` nativo o `role="dialog" aria-modal="true"` con focus trap.
- Submit a webhook público de n8n.
#### Banner aviso (cookies, promo)
`position: sticky` > `position: fixed` cuando sea posible. Botón cerrar con `aria-label="Cerrar aviso"`. Persistir cierre en `localStorage` con clave versionada.
 
### 5.3 Contenido
 
#### Galería con lightbox
Grid de miniaturas; click abre overlay. Navegación flechas + teclas ← →. ESC cierra. `aria-live="polite"` en contador "3 de 12". Focus trap mientras el lightbox esté abierto.
 
#### Testimonios
- **Grid estático** (simple, <6 testimonios)
- **Scroll horizontal con CSS snap** (`scroll-snap-type: x mandatory`) — preferido
- **Carrusel JS con dots** (si autoplay es necesario)
Respetar `prefers-reduced-motion` desactivando autoplay.
 
#### FAQ (acordeón accesible)
 
```html
<details class="faq-item">
  <summary>¿Pregunta?</summary>
  <div class="faq-answer">Respuesta…</div>
</details>
```
 
`<details>/<summary>` nativo: accesible por defecto, animable con CSS, sin JS. Ocultar marker nativo (`summary::-webkit-details-marker{display:none}`) y añadir chevron con `::after` que rote en `[open]`.
 
#### Equipo / Staff cards
Foto circular, nombre, rol, enlaces sociales. **Seguridad**: solo incluir personas con consentimiento explícito para aparición pública. Usar fotos profesionales, no casuales/personales.
 
#### Timeline / Historia
Línea vertical con hitos. Grid de dos columnas (fecha | evento) en desktop, una columna en mobile. Indicadores con `aria-label="Hito"`.
 
### 5.4 Interactivos
 
#### Calculadora
Inputs `type="number"` con `step`, `min`, `max`. Cálculo en tiempo real con evento `input`. Resultado con `aria-live="polite"`. Validar rangos.
 
#### Configurador de producto
Selectores de opciones (color, talla) actualizan preview y URL de compra. Estado en objeto JS; re-renderizar en cambios. Sincronizar con query string si se quiere enlazable.
 
#### Mapa interactivo
 
**Opción preferida — Google Maps embed (sin key):**
```html
<iframe src="https://www.google.com/maps/embed?pb=..." loading="lazy" title="Ubicación del hotel" style="border:0;width:100%;height:100%"></iframe>
```
Generar URL desde Google Maps → Compartir → Insertar mapa.
 
**Opción 2 — Leaflet + OpenStreetMap** (sin key, peso mayor):
```html
<link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css">
<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
```
 
**Opción 3 — Google Maps JS API:** evitar salvo necesidad real. La key queda expuesta; **restringir por dominio en Google Cloud Console** y documentar al usuario. Limitar a APIs estrictamente necesarias.
 
#### Filtros de contenido
Botones de categoría que muestran/ocultan items con `data-cat`. CSS con atributos `[data-cat="x"]:not([data-visible])`.
 
### 5.5 Formularios
 
**Regla de oro**: el `action` apunta a webhook público del backend del usuario (n8n), nunca a servicio con credenciales expuestas.
 
```html
<form id="hb-form-{HASH}" novalidate>
  <!-- campos visibles -->
  <label>Nombre<input type="text" name="nombre" required minlength="2"></label>
  <label>Email<input type="email" name="email" required></label>
  <label>Mensaje<textarea name="mensaje" required maxlength="1000"></textarea></label>
  <label><input type="checkbox" name="gdpr" required> Acepto la <a href="/privacidad">política de privacidad</a></label>
  <!-- honeypot anti-spam -->
  <input type="text" name="website" tabindex="-1" autocomplete="off" aria-hidden="true" style="position:absolute;left:-9999px">
  <button type="submit">Enviar</button>
</form>
<script>
(function(){
  var form = document.getElementById('hb-form-{HASH}');
  if (!form) return;
  form.addEventListener('submit', async function(e){
    e.preventDefault();
    if (form.website.value) return; // bot detectado, fallar silenciosamente
    var btn = form.querySelector('[type="submit"]');
    btn.disabled = true;
    btn.textContent = 'Enviando...';
    var data = Object.fromEntries(new FormData(form));
    delete data.website;
    try {
      var r = await fetch('https://n8n.example.com/webhook/PATH_PUBLICO_RANDOM', {
        method:'POST',
        headers:{'Content-Type':'application/json'},
        body: JSON.stringify(data)
      });
      if (!r.ok) throw new Error();
      form.innerHTML = '<p role="status">¡Gracias! Te contactaremos pronto.</p>';
    } catch(err) {
      btn.disabled = false;
      btn.textContent = 'Reintentar';
      var msg = form.querySelector('.form-error') || (function(){
        var d = document.createElement('p');
        d.className='form-error'; d.setAttribute('role','alert');
        d.style.color='#dc2626'; d.style.marginTop='8px';
        form.appendChild(d); return d;
      })();
      msg.textContent = 'Error al enviar. Intenta de nuevo.';
    }
  });
})();
</script>
```
 
**Para reservas de hotel**: añadir `type="date"` entrada/salida, `type="number" min="1"` huéspedes. Validar salida > entrada antes de submit. El webhook n8n puede crear el registro y enviar confirmación.
 
### 5.6 Embeds externos
 
#### YouTube / Vimeo con carga diferida
Mostrar thumbnail estático primero; reemplazar por iframe solo al click. Ahorra 1-2 MB de JS y mejora LCP.
 
Usar dominio `youtube-nocookie.com` para no setear cookies hasta reproducción.
 
#### Instagram / TikTok / Twitter embed
Scripts oficiales de la plataforma. **Privacidad**: estos scripts trackean al visitante. Avisar en banner de cookies.
 
#### Typeform / Google Forms
iframe directo con Técnica D.
 
---
 
## 6. Accesibilidad WCAG AA
 
Obligatorio en todos los embeds:
 
- **Contraste 4.5:1** mínimo texto normal, 3:1 texto grande (≥18pt) y componentes UI.
- **Focus visible**: `:focus-visible` con outline ≥2px. Nunca `outline:none` sin reemplazo.
- **Labels**: todo input con `<label for>`, `aria-label` o `aria-labelledby`. Placeholder NO es label.
- **Roles ARIA** solo cuando HTML semántico no basta. Preferir `<button>` sobre `<div role="button">`.
- **Anuncios dinámicos**: `aria-live="polite"` para cambios de contenido, `role="status"` feedback no urgente, `role="alert"` errores.
- **Orden teclado lógico**: probar con solo Tab. Evitar `tabindex` > 0.
- **Imágenes**: `alt` descriptivo para contenido, `alt=""` decorativas.
- **Movimiento**: respetar `prefers-reduced-motion`.
- **Idioma**: `lang="en"` en fragmentos en otro idioma dentro de página en español.
```css
@media (prefers-reduced-motion: reduce) {
  #hb-[id] *, #hb-[id] *::before, #hb-[id] *::after {
    animation-duration: .01ms !important;
    transition-duration: .01ms !important;
  }
}
```
 
---
 
## 7. Performance
 
- **Inline todo**: CSS y JS dentro del fragmento. Excepción justificada: librerías necesarias (Leaflet, Chart.js).
- **`loading="lazy"`** imágenes no above-the-fold, iframes.
- **`decoding="async"`** imágenes no críticas.
- **Tamaños optimizados**: CDN con `?w=400&q=80` o `srcset`.
- **No duplicar librerías**: consolidar si varios embeds usan la misma.
- **Debounce** en `input`/`scroll`/`resize` con `requestAnimationFrame`.
- **Event delegation** sobre el root en lugar de N listeners.
Presupuesto por embed: **<50 KB total** (HTML+CSS+JS inline, sin imágenes). Justificar si se excede.
 
---
 
## 8. Checklist final de entrega
 
Ejecutar antes de entregar al usuario:
 
**Estructura**
- [ ] Sin `<html>`, `<head>`, `<body>`, `<!DOCTYPE>`
- [ ] Wrapper con ID único `hb-[tipo]-[hash6]`
- [ ] CSS 100% con scope vía `#hb-[id]`
- [ ] JS en IIFE con `'use strict'`
- [ ] `if (!root) return` al inicio del script
**Color y contraste (nuevo — sección 2.5)**
- [ ] Se preguntó el color de fondo del sitio antes de generar código
- [ ] Tokens `--hb-bg`, `--hb-fg`, `--hb-accent` ajustados al fondo real
- [ ] Contraste verificado ≥4.5:1 texto normal, ≥3:1 texto grande y componentes UI
- [ ] Wrapper raíz con `background: transparent` (no fondo fijo salvo cards internas justificadas)
- [ ] `prefers-color-scheme` incluido solo si el sitio responde a dark mode del sistema
**Altura y espacio en blanco**
- [ ] Sin `height`, `min-height` ni `padding` vertical en el wrapper raíz
- [ ] Padding vertical solo en `.inner` y en el valor mínimo necesario
- [ ] Ningún widget tiene espacio vacío arriba/abajo no justificado por diseño
**Centrado**
- [ ] Técnica apropiada (A/B/C/D) para el tipo de contenido
- [ ] Centrado horizontal con flex/grid en el root
- [ ] Centrado vertical SOLO dentro de componentes hijos con altura propia; nunca `align-items:center` en el root
- [ ] `max-width` definido en `.inner`, no en el root
- [ ] `box-sizing: border-box` en root y descendientes
**Responsive**
- [ ] Probado mentalmente en 320px, 768px, 1440px
- [ ] Tipografía fluida con `clamp()` en titulares
- [ ] Sin scroll horizontal en ningún breakpoint
**Accesibilidad**
- [ ] Contraste ≥4.5:1 (verificar, no asumir)
- [ ] `:focus-visible` en todo elemento interactivo
- [ ] Labels en inputs, `alt` en imágenes
- [ ] ARIA solo donde hace falta
- [ ] `prefers-reduced-motion` respetado
- [ ] Navegable 100% con teclado
**Seguridad y privacidad (crítico — sección 2)**
- [ ] Sin API keys, tokens, secrets, credenciales
- [ ] Sin URLs internas, IPs privadas, hostnames Tailscale
- [ ] Sin emails/teléfonos/direcciones personales
- [ ] Sin comentarios con notas internas
- [ ] `rel="noopener noreferrer"` en `target="_blank"`
- [ ] Formularios → webhook público, no servicio con key
- [ ] Honeypot anti-spam en formularios
- [ ] Consentimiento GDPR donde aplique
- [ ] Sanitización (`escapeHTML`/`textContent`) al renderizar dinámico
**URLs**
- [ ] Todas HTTPS
- [ ] Sin espacios
- [ ] Dominios verificados (sin typos)
**Confirmación con usuario**
- [ ] Preguntar si algún dato del código es confidencial
- [ ] Confirmar destino de los enlaces
- [ ] Mencionar keys públicas usadas (Maps, reCAPTCHA) y recordar restringirlas por dominio
---
 
## 9. Instrucciones para el usuario
 
Al entregar, incluir estas instrucciones al final de la respuesta:
 
**Cómo incrustar el código en Hostinger:**
 
1. Editor del sitio → panel izquierdo → **Agregar elementos** → **Incrustar código**
2. Arrastrar el elemento a la posición deseada
3. Click en el elemento → **Introducir código**
4. Pegar el fragmento completo (desde `<div id="hb-...` hasta el `</script>` final)
5. Click en **Incrustar código**
6. Ajustar el tamaño del contenedor arrastrando bordes si es necesario
7. Click en **Actualizar** arriba a la derecha para publicar
**Si el widget no aparece online:**
- Esperar 30 segundos, refrescar con Ctrl+Shift+R (cache duro)
- Verificar en ventana incógnita
- Consola del navegador (F12) por errores
- Confirmar que todas las URLs son HTTPS
**Si se ve mal en el editor pero bien online:** normal. El editor no renderiza `<script>` ni ciertos estilos; la vista real es la del sitio publicado.
 
---
 
