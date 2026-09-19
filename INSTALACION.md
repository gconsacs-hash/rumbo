# Rumbo Financiero — cómo usarla

**Tu app:** https://gconsacs-hash.github.io/rumbo/

No depende de Claude ni de ningún servicio pagado. Los archivos están publicados en tu cuenta de GitHub (`gconsacs-hash/rumbo`) y los datos se guardan en tu proyecto de Firebase (`rumbo-3ba8c`, plan gratuito, cuenta Google). Funciona sin conexión y se sincroniza sola entre celular y PC.

## Conectar el PC (una vez)

1. Abre la URL → **Ajustes** → tarjeta **Sincronización** → **Conectar**.
2. El punto de arriba a la derecha se pone verde: *Sincronizado*.
3. Copia la **Clave de sincronización** y guárdala (es la "cuenta" que comparten tus dispositivos).

## Conectar el celular (Android)

1. Abre la misma URL en **Chrome**.
2. Menú ⋮ → **Agregar a pantalla principal** / **Instalar app**.
3. Ábrela → **Ajustes** → **Sincronización** → **Usar otra clave** → escribe la clave del PC → **Conectar**.

Listo: lo que registres en un dispositivo aparece en el otro en segundos.

## Respaldos

**Ajustes → Respaldo**: JSON completo (importable en otro dispositivo) y CSV por mes para Excel.

## Actualizar la app

Los archivos viven en `C:\Users\gcont\rumbo\`. Tras cambiar `index.html`, subir el número de `VERSION` en `sw.js` (v2 → v3…) y volver a subirlos al repositorio (Claude puede hacerlo con `gh`, que ya está instalado y autenticado).

## Costos y seguridad

- GitHub Pages: gratis. Firebase plan Spark: gratis (50.000 lecturas y 20.000 escrituras diarias; una persona usa una fracción mínima). No pide tarjeta.
- Quien tenga la URL **y tu clave de sincronización** podría ver tus datos. No compartas la clave.
- El `apiKey` de Firebase incorporado en la app es un identificador público, no un secreto; la protección real son las reglas de Firestore.
