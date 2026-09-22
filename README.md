# Para Camilita

> Para Camilita, la Ingeniera más talentosa y libre ❤

Página front-end 100% estática: una escena dibujada íntegramente con **Canvas 2D** — cielo azul con nubes en movimiento, césped, corte vertical del suelo con sus estratos, raíces que se ramifican y flores amarillas (girasoles, tulipanes y lirios) que brotan y luego se mecen con el viento.

## Stack

**Ninguna dependencia.** HTML + CSS + JavaScript vanilla en un solo archivo (`index.html`). No hay build, ni bundler, ni `node_modules`, ni paquetes que instalar — es lo óptimo aquí: la escena es 100% Canvas 2D, así que cualquier framework solo añadiría peso y tiempo de arranque a una página que ya es de un único archivo.

Lo único externo es la tipografía Fraunces de Google Fonts (con fallback a serif si no hay red).

## Cómo verlo

Basta con abrir `index.html` en el navegador (doble clic). Si prefieres servirlo por HTTP:

```bash
# Python (ya lo tienes: 3.11.9)
python -m http.server 5173

# o Node, sin instalar nada permanente
npx serve .
```

Luego abre <http://localhost:5173>.

## Interacción

- **Toca / haz clic** en cualquier punto para sembrar una flor nueva ahí (crece desde la raíz).
- **Resembrar** genera un jardín completamente nuevo.

## Detalles técnicos

- **Mobile first**: el lienzo ocupa el viewport completo (`100%`, no `100vh`), respeta `env(safe-area-inset-*)`, usa `touch-action: none` y adapta la línea del horizonte según la orientación (52 % de la altura en vertical para dejar más tierra visible, 58 % en horizontal).
- **Nítido en pantallas retina**: el canvas se escala por `devicePixelRatio` (limitado a 2 para no castigar el móvil).
- **Rendimiento**: el fondo estático (cielo, sol, colinas, estratos, grava, piedras, tepe) se pinta una sola vez en un canvas fuera de pantalla y cada frame solo se copia con `drawImage`. Cada nube se pre-renderiza también una vez, porque los degradados radiales suaves son caros de repintar.
- **Raíces**: se generan una vez como una lista de segmentos con una ventana de crecimiento `(t0, t1)`; animarlas es solo un `clamp` por segmento. Se dibujan en dos pasadas: un halo oscuro de tierra removida y encima la raíz.
- **Viento**: una función `windAt(x, t)` suma tres senoidales con desfase según la posición X, así la ráfaga recorre la escena en lugar de mover todo a la vez. Alimenta tallos, hojas, briznas de césped y polen.
- **Crecimiento por fases**: raíz → tallo → hojas → floración, con solapes y `easeOutBack` en la apertura de la flor.
- **Accesibilidad**: respeta `prefers-reduced-motion` (la escena se muestra ya crecida y en calma).

## Estructura

```
jardin-canvas/
├── index.html   # todo: markup, estilos y la escena
└── README.md
```
