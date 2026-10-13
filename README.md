# L-SEA · Lunar Site Evaluation Algorithm

## Créditos y Código Previo

**Este repositorio es la base de partida para el NASA Space Apps Challenge 2026 (Málaga).**

El código base entregado consiste en un **motor de evaluación de sitios lunares (base técnica mínima)** con:

### Motor (`src/core/`) — 8 módulos, lógica pura, sin dependencias de UI
| Módulo | Función |
|--------|---------|
| `geo.js` | Conversión píxel ↔ lat/lon (estereográfica polar), orientación mapa, curvatura lunar |
| `astro.js` | Posición Sol, Tierra, rotación lunar IAU (series Meeus truncadas) |
| `engine.js` | Pendiente (gradiente central), horizonte 360° con ray-tracing + curvatura, sombras, regla de visibilidad, clasificación de pendiente |
| `illumination.js` | Fracción de iluminación solar a 30 días en malla gruesa (1.28 km) |
| `windows.js` | Visibilidad diaria Sol/Tierra por sitio, búsqueda de ventanas |
| `power.js` | Modelo EPS (panel vertical/horizontal) + elevación Luna vista desde DSN (Madrid, Goldstone, Canberra) |
| `nasa_suite.js` | Temperatura radiativa, corredor glideslope 12°, TRN, margen línea de vista |
| `autopilot.worker.js` | Auditoría seguridad + iluminación en Web Worker (multihilo) |

### Rendering 3D (`src/3d/`) — Three.js
| Módulo | Función |
|--------|---------|
| `scene.js` | Escena, cámara, luces |
| `terrain.js` | Malla DEM con LOD opcional (decimate) |
| `loader.js` | Carga DEM binario (cabecera rows/cols/mpc + Float32Array) |
| `layers.js` | Generación texturas GIS: elevación, pendiente, iluminación |

### UI actual (`src/App.vue`)
- **Canvas azul** (sin controles, sin paneles, sin features de misión)

### Datos
- `descargar_lola.py` — Descarga DEM LRO/LOLA polo sur (640 km × 640 km, 160 m/celda, 4000×4000) → `public/data/lola_shackleton.bin`

### Validación
- `node tests/verify.mjs` — Checks físicos vs Skyfield/JPL DE421 (Sun ≤ 0.003°, Earth ≤ 0.06°, DSN ≤ 0.07°)

---

## Licencia
MIT.