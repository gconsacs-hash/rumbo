# Rumbo Financiero — guía de instalación

La app no depende de Claude ni de ningún servicio pagado. Son 5 archivos estáticos que se publican en GitHub Pages (gratis) y guardan los datos en tu propio proyecto de Firebase (gratis, cuenta Google). Funciona sin conexión y se sincroniza sola entre celular y PC.

Archivos de esta carpeta:

| Archivo | Para qué |
|---|---|
| `index.html` | La app completa |
| `sw.js` | Hace que funcione sin internet |
| `manifest.webmanifest` | Permite "instalarla" en Android |
| `icon-192.png`, `icon-512.png` | Ícono de la app |

---

## Paso 1 — Publicar en GitHub Pages (10 minutos, una sola vez)

1. Crea una cuenta en **github.com** (si no tienes) e inicia sesión.
2. Arriba a la derecha, **+ → New repository**.
   - Repository name: `rumbo`
   - Public
   - **Create repository**
3. En la página del repo nuevo, haz clic en **uploading an existing file**.
4. Arrastra los 5 archivos de esta carpeta (`index.html`, `sw.js`, `manifest.webmanifest`, `icon-192.png`, `icon-512.png`) a la zona de carga. Abajo pulsa **Commit changes**.
5. Ve a **Settings** (pestaña del repo) → menú izquierdo **Pages**.
   - Source: **Deploy from a branch**
   - Branch: **main** / **(root)** → **Save**
6. Espera 1-2 minutos y recarga la página de Pages. Aparecerá la URL:
   `https://TUUSUARIO.github.io/rumbo/`

Esa es tu app. Ábrela en el PC y en el celular.

**Para actualizar la app más adelante:** en el repo, abre el archivo, clic en el lápiz (o vuelve a subir con *Add file → Upload files*) y Commit. En `sw.js` sube el número de `VERSION` (`rumbo-v2`, `v3`…) para que los dispositivos tomen la versión nueva.

---

## Paso 2 — Crear el proyecto Firebase para sincronizar (10 minutos, una sola vez)

1. Entra a **console.firebase.google.com** con tu Gmail → **Crear un proyecto**.
   - Nombre: `rumbo`
   - Desactiva Google Analytics → **Crear proyecto**.
2. Menú izquierdo **Compilación → Authentication → Comenzar**.
   - Pestaña *Sign-in method* → **Anónimo** → **Habilitar** → Guardar.
3. Menú izquierdo **Compilación → Firestore Database → Crear base de datos**.
   - Ubicación: `southamerica-east1` (São Paulo) o la que ofrezca.
   - Modo: **producción** → Crear.
4. En Firestore, pestaña **Reglas**. Borra todo y pega:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /spaces/{space}/docs/{doc} {
      allow read, write: if request.auth != null;
    }
  }
}
```

   Pulsa **Publicar**.

5. Ícono de engranaje (arriba a la izquierda) → **Configuración del proyecto** → baja hasta **Tus apps** → ícono **Web `</>`**.
   - Apodo: `rumbo` → **Registrar app** (no marques Hosting).
   - Copia el bloque que empieza con `const firebaseConfig = {` hasta `};`.

---

## Paso 3 — Conectar la app (en el PC)

1. Abre `https://TUUSUARIO.github.io/rumbo/` → **Ajustes** → tarjeta **Sincronización**.
2. Pega el bloque `firebaseConfig` en el cuadro.
3. Pulsa **Conectar**. El punto de arriba a la derecha se pone verde: *Sincronizado*.
4. Copia la **Clave de sincronización** (botón *Copiar clave*). Es la "cuenta" que comparten tus dispositivos: guárdala en un lugar seguro (por ejemplo, en tus notas).

## Paso 4 — Conectar el celular (Android)

1. Abre la misma URL en **Chrome** del celular.
2. Menú ⋮ → **Agregar a pantalla principal** (o *Instalar app*). Queda como app con ícono propio.
3. Ábrela → **Ajustes** → **Sincronización**:
   - **Usar otra clave** → pega la clave del PC → Aceptar.
   - Pega el mismo bloque `firebaseConfig`.
   - **Conectar**.
4. Listo: lo que registres en el celular aparece en el PC y viceversa, en segundos. Sin internet la app sigue funcionando y sincroniza cuando vuelve la señal.

---

## Respaldos

En **Ajustes → Respaldo** puedes descargar un JSON con todo y volver a importarlo en cualquier dispositivo (útil si cambias de celular). También un CSV por mes para Excel.

## Costos

GitHub Pages: gratis. Firebase plan Spark: gratis hasta 50.000 lecturas y 20.000 escrituras diarias; una persona registrando gastos usa una fracción mínima de eso. No pide tarjeta.

## Seguridad

- Los datos viven en TU proyecto de Firebase, solo tú tienes acceso a la consola.
- Cualquiera que tenga tu URL y **tu clave de sincronización** y **tu firebaseConfig** podría leer los datos. No compartas la clave.
- El `apiKey` de Firebase no es un secreto (es un identificador público); la protección real son las reglas de Firestore + la clave.
