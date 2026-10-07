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
- **Una imagen por escena** (la imagen pertenece a la escena y se mantiene en todos sus beats, también en los que borran lo anterior). Tres orígenes, a elección en cada escena:
  - **Generada con IA** con el modelo más económico del proveedor, editable: `gpt-image-1-mini` en calidad baja (OpenAI) o `gemini-3.1-flash-lite-image` (Google), con tu clave. **Claude (Anthropic) no genera imágenes.** En la diapositiva se rotula «Imagen generada con IA · modelo».
  - **Libre**, buscada en **Wikimedia Commons** y filtrada por código a licencias CC0, dominio público, CC BY y CC BY-SA (se descartan NC, ND, GFDL, imágenes pequeñas y las que declaran restricciones). El autor, la licencia y «Wikimedia Commons» se muestran en la propia diapositiva y en el Word de notas.
  - **La tuya**, subida desde tu equipo (los derechos son responsabilidad tuya).
  Dos disposiciones: panel lateral (por defecto) o imagen de fondo con velo. Las imágenes se reducen a ≤1280 px y se **incrustan** en la charla exportada, que sigue funcionando sin conexión.
- **Colores personalizables:** fondo, texto y acento (`@fondo`, `@texto`, `@acento` en el guion), con selector, presets y aviso de contraste calculado (WCAG, mínimo recomendado 4,5:1).
- **Exporta** la charla como un único HTML autónomo con el reproductor dentro (y una política que bloquea la red), notas en Word, proyecto `.json` y guion `.txt`.

## Qué comprueba el código y qué hace la IA

La IA solo **propone**; el código **comprueba**:

- Su respuesta se trata como dato no fiable: solo pasan los tipos de bloque conocidos, con límites de longitud y cantidad; el HTML que traiga se muestra como texto.
- **Auditoría de contenido** (si hay material): cada beat propuesto lleva una cita literal; el código verifica que aparece en tu material (✓/⚠), señala las **cifras que no están en el material** y lista las **frases del material que ningún beat cubre**.
- **Comprobar la charla:** recorre todos los beats hacia delante, hacia atrás, saltando directamente y recargando desde su dirección, y compara los fotogramas (estructura del DOM, **no píxeles**); detecta texto que se sale del escenario y referencias a la red en la exportación.
- El guion se lee con errores por línea; la charla nunca se obtiene de otra cosa que del guion de texto.

## Privacidad

Las imágenes añaden dos envíos opcionales, siempre con aviso previo y confirmación en cada tanda: a tu proveedor de IA va **el título y las ideas de cada diapositiva** (con correos, DNI y teléfonos enmascarados; no tu material completo), y a Wikimedia Commons va **solo la búsqueda**. Las imágenes se guardan en tu navegador y viajan dentro del HTML exportado.

Tu material, guion y charla se guardan solo en este navegador (IndexedDB). Hacia tu proveedor de IA sale **únicamente el texto del material, y solo si pides un guion**, con correos, DNI/NIE, teléfonos enmascarados y tras un aviso previo con muestra. La clave se guarda solo en tu navegador. La charla exportada no contiene claves ni tu material.

## Límites

- **Imágenes de IA no probadas con claves reales:** las llamadas a OpenAI y Gemini se probaron con respuestas simuladas, no con tus cuentas. Los nombres de modelo y los precios cambian (en octubre de 2026 Imagen 4 y `gemini-2.5-flash-image` ya están retirados o cerrados); el precio exacto de `gemini-3.1-flash-lite-image` no se pudo confirmar en la documentación oficial, así que consulta el precio vigente antes de generar. El campo «precio por imagen» es tuyo.
- **Wikimedia Commons no probado en vivo** (el entorno de pruebas no llega a ese dominio): se probó con respuestas simuladas con el formato de su API. El filtro de licencias se basa en lo que declaran quienes suben los ficheros; revisa siempre la imagen y sus condiciones. Con «completar las escenas» se elige el primer resultado: revisa que encaje y que sea adecuada.
- **Contraste sobre imágenes:** en modo fondo el velo (≥ 74 % de opacidad) mantiene legible el texto con los colores por defecto; con colores propios, el aviso de contraste se calcula entre texto y fondo, no sobre la imagen.
- Con imagen lateral, la columna de texto se estrecha: diagramas de hasta 6 nodos y textos largos pueden no caber (la comprobación de la charla lo avisa).

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

Borrás Rocher, F. (2026). *EscenIA* (v1.1.0) [Software]. DOI: [10.5281/zenodo.23223200](https://doi.org/10.5281/zenodo.23223200)

## Licencia

MIT. Véase [LICENSE](LICENSE).
