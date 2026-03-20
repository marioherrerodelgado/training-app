# GUÍA DE CONFIGURACIÓN — TRAIN App

## Archivos del proyecto

```
tu-repo/
├── index.html      ← App pública (lo que ven tus amigos)
├── admin.html      ← Panel admin (solo tú)
└── SETUP.md        ← Esta guía
```

---

## PASO 1 — Crear proyecto Firebase (5 minutos)

1. Ve a https://console.firebase.google.com
2. Click "Agregar proyecto"
3. Nombre: `train-app` (o el que quieras)
4. Desactiva Google Analytics (no lo necesitas)
5. Click "Crear proyecto"

---

## PASO 2 — Crear la base de datos Firestore

1. En el menú izquierdo → "Firestore Database"
2. Click "Crear base de datos"
3. Selecciona "Comenzar en modo de prueba" (después lo securizamos)
4. Elige la ubicación más cercana: `europe-west1` (Bélgica)
5. Click "Listo"

---

## PASO 3 — Obtener tu configuración Firebase

1. En Firebase Console → ⚙️ Configuración del proyecto (engranaje arriba izquierda)
2. Scroll hasta "Tus apps" → Click "</>" (añadir app web)
3. Nombre: `train-web`
4. NO actives Firebase Hosting (usamos GitHub Pages)
5. Click "Registrar app"
6. Verás un bloque así:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "train-app-xxxxx.firebaseapp.com",
  projectId: "train-app-xxxxx",
  storageBucket: "train-app-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

7. **Copia esos 6 valores**

---

## PASO 4 — Pegar la configuración en los archivos HTML

Abre `index.html` y busca este bloque (línea ~150 aprox):

```javascript
const firebaseConfig = {
  apiKey: "TU_API_KEY",          // ← reemplaza
  authDomain: "TU_PROJECT...",   // ← reemplaza
  projectId: "TU_PROJECT_ID",    // ← reemplaza
  storageBucket: "TU_PROJECT...",// ← reemplaza
  messagingSenderId: "TU_...",   // ← reemplaza
  appId: "TU_APP_ID"             // ← reemplaza
};
```

Haz lo mismo en `admin.html` — tiene el **mismo bloque** en las mismas posiciones.

---

## PASO 5 — Cambiar las contraseñas

### En `index.html` (contraseña para tus amigos):
```javascript
const ACCESS_PASSWORD = "entrenamiento2026";  // ← cambia esto
```

### En `admin.html` (contraseña solo para ti):
```javascript
const ADMIN_PASSWORD = "admin_train_2026";  // ← cambia esto (diferente)
```

**Regla de oro:** la contraseña admin debe ser diferente a la de usuarios.

---

## PASO 6 — Reglas de seguridad Firestore

En Firebase Console → Firestore → Pestaña "Reglas":

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /wods/{document} {
      allow read: if true;         // Cualquiera puede leer
      allow write: if false;       // Nadie puede escribir directamente
    }
  }
}
```

> **Nota:** La escritura la gestiona el SDK de Firebase desde admin.html con las claves de tu proyecto. Las reglas `write: false` protegen contra escrituras directas no autorizadas desde fuera de tu app.

---

## PASO 7 — Subir a GitHub Pages

### Si no tienes GitHub:
1. Crea cuenta en https://github.com (gratis)
2. Instala GitHub Desktop: https://desktop.github.com

### Crear repositorio y publicar:
1. GitHub → "New repository"
2. Nombre: `train-app` (o similar)
3. Marca "Public"
4. Click "Create repository"
5. Sube los 2 archivos: `index.html` y `admin.html`
6. Ve a Settings → Pages → Source: "Deploy from branch" → `main` → `/root`
7. Guarda → espera 2-3 minutos

Tu app estará en: `https://TU_USUARIO.github.io/train-app/`

---

## PASO 8 — Primera prueba

1. Abre `https://TU_USUARIO.github.io/train-app/admin.html`
2. Entra con tu contraseña admin
3. Sube un WOD de prueba
4. Abre `https://TU_USUARIO.github.io/train-app/`
5. Entra con la contraseña de usuario
6. El WOD debe aparecer en el calendario

---

## Cómo usarlo cada mes

1. Abre `tu-url/admin.html`
2. Entra con contraseña admin
3. Rellena el formulario para cada día de entrenamiento
4. Click "Guardar en Firebase"
5. Tus amigos ven los cambios al instante en `tu-url/`

No necesitas tocar el código nunca más.

---

## Personalizar el nombre de la app

En `index.html`, busca:
```html
<h1 id="appTitle">TRAIN<span>.</span></h1>
```
Cambia `TRAIN` por tu nombre o el del grupo.

---

## Estructura de un WOD en Firebase

Cada WOD se guarda con estos campos:

| Campo | Ejemplo | Requerido |
|-------|---------|-----------|
| date | 2026-03-15 | ✅ |
| month | 2026-03 | ✅ (auto) |
| sport | running / hyrox / deka / crossfit | ✅ |
| title | "Rodaje Z2 — 45 min" | ✅ |
| intensity | baja / media / alta / maxima | ✅ |
| duration | "60'" | opcional |
| volume | "Alto" | opcional |
| type | "Aeróbico" | opcional |
| notes | "Mantén FC en Z2..." | opcional |
| warmup | "10 min suave\nTécnica..." | opcional |
| main | "6×1000m @ ritmo 10K\n..." | opcional |
| metcon | "100 wall balls..." | opcional |
| cooldown | "Estiramientos 5 min" | opcional |

---

## Dudas frecuentes

**¿Cuánto cuesta?**
Firebase plan Spark = gratis. Para tu uso (personal, pocos usuarios) nunca llegarás al límite.

**¿Pueden ver el admin mis amigos?**
Técnicamente pueden abrir `/admin.html`, pero necesitan la contraseña admin para entrar.

**¿Puedo cambiar el nombre de la app después?**
Sí, edita el HTML y vuelve a subir el archivo a GitHub.

**¿Puedo añadir Crossfit después?**
Sí, en `index.html` añade el botón de filtro y el tag CSS. En el admin ya está incluido.
