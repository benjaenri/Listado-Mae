# Registro de Seguridad Corporativa MAE

## 1. Crear el proyecto
1. Entrá a Firebase Console.
2. Crear proyecto.
3. Agregá una aplicación Web (`</>`).
4. Copiá el objeto `firebaseConfig`.
5. Pegalo en `index.html`, reemplazando los valores `PEGAR_...`.

## 2. Authentication
Firebase Console > Authentication > Sign-in method > Email/Password > Activar.

Después creá el usuario que va a administrar las planillas desde Authentication > Users.

## 3. Firestore
Firebase Console > Firestore Database > Create database.

Usá estas reglas como punto de partida. Permiten leer/escribir solamente a usuarios autenticados:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /companies/{companyId} {
      allow read, write: if request.auth != null;
      match /records/{recordId} {
        allow read, write: if request.auth != null;
      }
    }
  }
}
```

## 4. Publicar
Podés usar Firebase Hosting, GitHub Pages, Netlify o abrir `index.html` localmente para probar. Para producción, Firebase Hosting es una buena opción.

## 5. Qué guarda
Cada empresa es una carpeta virtual en Firestore y cada planilla queda dentro de `companies/{empresa}/records`.

Al tocar **Imprimir**, primero se guarda la planilla en Firestore y después se abre la impresión A4.

Nota: esta versión guarda los datos de la planilla, no un PDF físico. Si después querés conservar además un PDF exacto de cada impresión, se puede agregar Firebase Storage.
