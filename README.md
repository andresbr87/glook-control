# GLOOK · Caribe Vivo — Control de producción, stock y ventas

Aplicativo web de una sola página (HTML+JS puro, sin build). Guarda los datos en
Firebase Firestore, así que persisten fuera de esta conversación y se sincronizan
en tiempo real entre todas las personas que lo usen.

## 1. Crear el proyecto de Firebase (gratis)

1. Ve a https://console.firebase.google.com y crea un proyecto (ej. `glook-caribe-vivo`).
2. En el menú lateral, entra a **Firestore Database** → **Crear base de datos** →
   elige **modo producción** → elige la región más cercana (ej. `southamerica-east1`).
3. En **Reglas** de Firestore, pega el contenido del archivo `firestore.rules`
   que te dejé adjunto, y publica.
4. En **Authentication** → **Sign-in method**, activa el proveedor **Anónimo**
   (esto es lo que permite que la app entre a Firestore sin pedirle cuenta a
   cada persona del equipo).
5. Ve a **Configuración del proyecto** (ícono de engranaje) → en "Tus apps",
   crea una app web (ícono `</>`), ponle un nombre y copia el objeto
   `firebaseConfig` que te muestra (apiKey, authDomain, projectId, etc.).

## 2. Configurar el archivo

Abre `glook-control.html` y busca este bloque cerca del inicio del `<script>`:

```js
const firebaseConfig = {
  apiKey: "TU_API_KEY",
  authDomain: "TU_PROYECTO.firebaseapp.com",
  projectId: "TU_PROYECTO",
  storageBucket: "TU_PROYECTO.appspot.com",
  messagingSenderId: "000000000000",
  appId: "TU_APP_ID"
};

const PIN_HASH_EQUIPO = "..."; // hash SHA-256 del PIN, no el PIN en texto plano
```

Reemplaza `firebaseConfig` por los valores que copiaste de Firebase. Para el PIN,
no lo escribas directo en el archivo — pídele a Claude que te genere el hash
SHA-256 del PIN que quieras usar (así nunca queda visible el PIN real en el
código fuente, ni siquiera en el repositorio público de GitHub).

⚠️ **Sobre el PIN**: solo bloquea la pantalla de la aplicación (una barrera
básica para que no cualquiera que encuentre la URL entre a mirar). Guardarlo
como hash evita que alguien vea el PIN real con solo mirar el código del
repositorio, pero no es seguridad de nivel bancario. Los datos en sí están
protegidos por las reglas de Firestore (que exigen autenticación anónima), no
por el PIN. Para control de acceso real por persona, el siguiente paso sería
agregar login individual (Firebase Auth con email/contraseña), pero eso ya es
una mejora posterior.

## 3. Subir a GitHub

```bash
git init
git add glook-control.html firestore.rules README.md
git commit -m "GLOOK Caribe Vivo - control de producción y ventas"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/glook-caribe-vivo.git
git push -u origin main
```

(Si no usas git desde la terminal, también puedes crear el repo directo en
github.com y arrastrar los archivos desde "Add file → Upload files".)

## 4. Activar GitHub Pages

1. En tu repositorio de GitHub, ve a **Settings → Pages**.
2. En "Source", elige la rama `main` y la carpeta `/ (root)`.
3. Guarda. En un par de minutos tu app va a quedar disponible en:
   `https://TU_USUARIO.github.io/glook-caribe-vivo/glook-control.html`

Comparte esa URL + el PIN con el equipo de GLOOK.

## 5. Migrar tus datos actuales

Usa la carga masiva por archivo (.txt) en las pestañas Producción, Entrega de
producto y Venta directa, tal como ya lo veníamos haciendo en el prototipo.

## Notas

- No hay build ni dependencias que instalar: es un solo archivo HTML.
- Cualquier cambio futuro que quieras (nuevas columnas, nuevos reportes) se
  edita directamente en `glook-control.html` y se vuelve a subir a GitHub.
- Los datos viven en Firestore, no en el navegador — puedes borrar el archivo
  local o cambiar de computador sin perder nada.
- El plan gratuito (Spark) de Firebase alcanza sobradamente para este volumen
  de datos.
