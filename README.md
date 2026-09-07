# Kart do Pedro, version standalone

Misma GUI del trick `com.pedro.kart`, pero sin servidor: el ranking y los pilotos
viven en `localStorage` del aparato.

## Como regenerar despues de tocar el juego

Se edita **siempre** el original del trick:
`/home/mano/tricks/com.pedro.kart/gui/index.html`

Y despues:

```bash
bash /home/mano/work/kart-standalone/build.sh
```

Sale `dist/index.html`, archivo unico, listo para mandar o subir.

## Que hace el build

1. Convierte el `<script type="module" data-trick-id=...>` en script plano
   (los modulos no cargan desde `file://`, y queremos doble clic sin servidor).
2. Reemplaza el import del SDK del trick por `shim.js`, que implementa
   `trick.query` y `trick.sendEvent` con las mismas queries y eventos
   (`list_racers`, `list_ranking`, `list_races`, `racer_added`,
   `race_finished`, `ranking_reset`) contra `localStorage`.
3. Cambia el boot de `await` top-level a `.then()`.
4. Agrega los metas de "Agregar a pantalla de inicio" (iOS y Android).

Si el GUI del trick cambia de forma y alguno de esos bloques no aparece,
el build falla en voz alta en vez de generar un archivo roto.

## Estado

- 2026-09-07: build funcionando, verificado en Chromium desde `file://`.
  Persistencia confirmada tras recargar.
- Pendiente: URL fija (Cloudflare Pages o Netlify) para que las
  actualizaciones lleguen solas al iPhone. Falta cuenta o token de Alex.
