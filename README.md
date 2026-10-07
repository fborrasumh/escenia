# EscenIA

Charlas como beats, no como diapositivas. Aplicación web de **un solo fichero** (`index.html`), sin servidor.

**Usar la app:** https://fborrasumh.github.io/escenia/

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23223200.svg)](https://doi.org/10.5281/zenodo.23223200)

**Idiomas:** español (por defecto), inglés y portugués; selector en la barra superior (o `?lang=en` / `?lang=pt` en la URL).

## Qué hace

- **Guion en texto → charla por beats.** Un clic es un beat sobre un escenario fijo de 1920×1080 escalado con bandas negras. El estado de la charla es solo la pareja `{escena, beat}`: `#3.4` en la dirección es siempre el mismo fotograma, vengas de donde vengas.
- **Bloques:** afirmación, texto, tachado, texto que se escribe, contador animado, terminal (con salida resaltada y aparición automática), diagramas que crecen nodo a nodo, gráfico de barras, código QR (calculado en tu navegador) y notas del ponente. Temas oscuro y claro y color de acento.
- **Escenario** con teclado y clic (→ ↓ PgDn Espacio · ← ↑ PgUp · 1-9 escena · Inicio/Fin), **vista general** (O), **vista de presentador** (P) con beat actual y siguiente, notas y cronómetro sincronizados entre ventanas, **apagón** (B) y **pantalla completa** (F). Movimiento reducido respetado; `?debug=1` y `?capture=1` disponibles.
- **IA opcional (clave propia: OpenAI, Gemini o Claude):** a partir de un texto, Word, PDF o PowerPoint propone el guion.
- **Exporta** la charla como un único HTML autónomo con el reproductor dentro (y una política que bloquea la red), notas en Word, proyecto `.json` y guion `.txt`.

## Qué comprueba el código y qué hace la IA

La IA solo **propone**; el código **comprueba**:

- Su respuesta se trata como dato no fiable: solo pasan los tipos de bloque conocidos, con límites de longitud y cantidad; el HTML que traiga se muestra como texto.
- **Auditoría de contenido** (si hay material): cada beat propuesto lleva una cita literal; el código verifica que aparece en tu material (✓/⚠), señala las **cifras que no están en el material** y lista las **frases del material que ningún beat cubre**.
- **Comprobar la charla:** recorre todos los beats hacia delante, hacia atrás, saltando directamente y recargando desde su dirección, y compara los fotogramas (estructura del DOM, **no píxeles**); detecta texto que se sale del escenario y referencias a la red en la exportación.
- El guion se lee con errores por línea; la charla nunca se obtiene de otra cosa que del guion de texto.

## Privacidad

Tu material, guion y charla se guardan solo en este navegador (IndexedDB). Hacia tu proveedor de IA sale **únicamente el texto del material, y solo si pides un guion**, con correos, DNI/NIE, teléfonos enmascarados y tras un aviso previo con muestra. La clave se guarda solo en tu navegador. La charla exportada no contiene claves ni tu material.

## Límites

- No se probó con claves reales de OpenAI, Gemini ni Claude: las pruebas usan IA simulada (que miente a propósito) para los tres proveedores.
- La verificación de determinismo compara **estructura**, no píxeles; el aspecto depende de las fuentes del sistema (la charla exportada no incluye fuentes web).
- La vista de presentador **abierta desde la app** se sincroniza por BroadcastChannel; necesita servirse por `http(s)` (GitHub Pages lo cumple). Abierta desde `file://` la ventana tiene origen opaco y no sincroniza. La charla **exportada** sí sincroniza con su presentador entre ventanas `file://` (probado en Chromium; otros navegadores no se probaron).
- Los códigos QR admiten hasta 213 bytes (versión 10, corrección M).
- El diagrama admite hasta 12 nodos; el gráfico, barras agrupadas de hasta 12 categorías.
- Es una herramienta de apoyo: la IA puede equivocarse y la decisión final es de quien presenta. No sustituye revisar el guion.

## Autoría

Fernando Borrás Rocher (Universidad Miguel Hernández de Elche).

ORCID: Fernando Borrás Rocher [0000-0002-5519-4573](https://orcid.org/0000-0002-5519-4573)

**Fuente inspiradora:** [beatdeck](https://github.com/borjaperfra/beatdeck) de borjaperfra (MIT), que propone las charlas como beats sobre un escenario fijo. EscenIA reproduce ese comportamiento de forma independiente en un único HTML, **con código propio**: no copia ni incluye código de beatdeck.

## Cómo citar

Borrás Rocher, F. (2026). *EscenIA* (v1.0.0) [Software]. DOI: [10.5281/zenodo.23223200](https://doi.org/10.5281/zenodo.23223200)

## Licencia

MIT. Véase [LICENSE](LICENSE).
