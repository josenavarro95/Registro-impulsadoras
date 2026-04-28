# ⚡ Electric & Automation — Sistema de Registro de Impulsadoras

Aplicativo móvil PWA (Progressive Web App) para registro de horarios y trabajo de impulsadoras.  
Desarrollado para **Jose Navarro | Electric & Automation**.

---

## 📋 Archivos incluidos

| Archivo | Descripción |
|---|---|
| `index.html` | Aplicación completa (HTML + CSS + JS) |
| `manifest.json` | Configuración PWA para instalación en celular |
| `sw.js` | Service Worker (caché offline) |
| `icon-192.png` | Ícono de la app (192×192) |
| `icon-512.png` | Ícono de la app (512×512) |

---

## 🔥 PASO 1 — Configurar Firebase

### 1.1 Obtener las credenciales de tu proyecto

1. Ve a 👉 [https://console.firebase.google.com](https://console.firebase.google.com)
2. Selecciona tu proyecto **registro-impulsadoras**
3. Click en el ícono ⚙️ (engranaje) → **Configuración del proyecto**
4. Baja hasta **"Tus apps"** → selecciona tu app web (o crea una nueva con el ícono `</>`)
5. Copia el objeto `firebaseConfig` que aparece

### 1.2 Editar index.html

Abre `index.html` en un editor de texto (Bloc de notas, VS Code, Notepad++) y busca esta sección:

```javascript
const firebaseConfig = {
  apiKey:            "REEMPLAZA_TU_API_KEY",
  authDomain:        "registro-impulsadoras.firebaseapp.com",
  projectId:         "registro-impulsadoras",
  storageBucket:     "registro-impulsadoras.appspot.com",
  messagingSenderId: "REEMPLAZA_TU_SENDER_ID",
  appId:             "REEMPLAZA_TU_APP_ID"
};
```

Reemplaza los valores `REEMPLAZA_...` con los de tu proyecto Firebase.

---

## 🔐 PASO 2 — Reglas de Firestore

En la consola de Firebase → **Firestore Database** → **Reglas**, pega esto:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Permitir acceso a config (credenciales)
    match /config/{doc} {
      allow read, write: if true;
    }
    // Permitir acceso a registros
    match /registros/{doc} {
      allow read, write: if true;
    }
  }
}
```

> ⚠️ Estas reglas son abiertas para simplicidad. En producción considera añadir autenticación Firebase Auth.

---

## 🗄️ PASO 3 — Reglas de Firebase Storage

En la consola → **Storage** → **Reglas**, pega esto:

```javascript
rules_version = '2';
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write: if true;
    }
  }
}
```

---

## 🐙 PASO 4 — Subir a GitHub Pages

### 4.1 Crear repositorio en GitHub

1. Ve a [https://github.com](https://github.com) y crea una cuenta (si no tienes)
2. Click en **"New repository"**
3. Nombre: `ea-registro` (o el que prefieras)
4. Marca **"Public"**
5. Click **"Create repository"**

### 4.2 Subir los archivos

**Opción A — Desde la web (más fácil):**
1. Entra al repositorio creado
2. Click en **"uploading an existing file"** (o "Add file" → "Upload files")
3. Arrastra y suelta todos los archivos:
   - `index.html`
   - `manifest.json`
   - `sw.js`
   - `icon-192.png`
   - `icon-512.png`
4. Click **"Commit changes"**

**Opción B — Con Git (si tienes instalado):**
```bash
git init
git add .
git commit -m "Initial: EA Registro app"
git branch -M main
git remote add origin https://github.com/TU_USUARIO/ea-registro.git
git push -u origin main
```

### 4.3 Activar GitHub Pages

1. En tu repositorio → click en **Settings** (⚙️)
2. En el menú izquierdo → **Pages**
3. En **"Branch"** selecciona `main` y carpeta `/` (root)
4. Click **Save**
5. Espera 1-2 minutos y tendrás tu URL:
   ```
   https://TU_USUARIO.github.io/ea-registro/
   ```

---

## 📱 PASO 5 — Instalar en el celular

### Android (Chrome):
1. Abre Chrome y ve a la URL de GitHub Pages
2. Verás un banner **"Añadir a pantalla de inicio"** — toca **Instalar**
3. O toca el menú (3 puntos) → **"Añadir a pantalla de inicio"**

### iPhone / iPad (Safari):
1. Abre Safari y ve a la URL
2. Toca el ícono de **Compartir** (cuadrado con flecha)
3. Selecciona **"Añadir a pantalla de inicio"**
4. Toca **Agregar**

---


## 🕐 Horarios de referencia

| Tipo | Hora objetivo | Tolerancia |
|---|---|---|
| 🌅 Entrada | 9:00 AM | ±10 minutos (8:50 – 9:10) |
| 🌆 Salida | 5:00 PM | ±10 minutos (4:50 – 5:10) |

---

## 📊 Funcionalidades

### Usuario (userall):
- ✅ Registro de **entrada** y **salida** con hora exacta
- ✅ Autocomplete de nombre/apellido por cédula (historial)
- ✅ Subida de hasta **2 fotos** por registro
- ✅ Detección automática de puntualidad vs tardanza
- ✅ Prevención de doble registro en el mismo día

### Administrador (admin):
- ✅ **Búsqueda por cédula** con filtro de fechas
- ✅ Ver historial completo: horarios, descripción, fotos, valor
- ✅ **Descargar fotos** directamente
- ✅ **Exportar Excel** (semanal / quincenal / mensual / personalizado)
- ✅ Cambiar contraseñas de ambos usuarios
- ✅ Limpieza de fotos antiguas (>30 días, más antiguas primero)

---

## 🗑️ Limpieza automática de fotos

- Al ingresar como **Admin**, el sistema revisa automáticamente si hay fotos con más de 30 días
- Las elimina en orden cronológico (del día más antiguo hacia adelante)
- El registro de texto permanece aunque se eliminen las fotos
- También puedes ejecutar la limpieza manual desde ⚙️ Config → Mantenimiento

---

## 🆘 Soporte

Para configuración o soporte: **jota952009@gmail.com**

---

*Electric & Automation — Jose Navarro · Sistema de Registro v1.0*
