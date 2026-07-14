# REPFORA E.P. — Sistema de Seguimiento de Etapa Productiva

REPFORA es una solución integral diseñada para el SENA, destinada a gestionar, automatizar y auditar el ciclo completo de la Etapa Productiva de los aprendices. El sistema permite el control de bitácoras, seguimientos, documentación de certificación y la bolsa de horas de los instructores.

## 🚀 Tecnologías y Librerías Usadas

### Backend Core
*   **Node.js & Express**: Entorno de ejecución y framework web robusto.
*   **MongoDB & Mongoose**: Base de datos NoSQL y modelado de objetos para una estructura de datos flexible.
*   **ES Modules (ESM)**: Uso de `import/export` nativo para un código moderno y modular.

### Seguridad y Autenticación
*   **jsonwebtoken (JWT)**: Gestión de sesiones seguras mediante tokens.
*   **bcryptjs**: Encriptación de contraseñas con algoritmos de hash de alta seguridad.

### Gestión de Archivos y Documentos
*   **pdfkit**: Generación dinámica y programática de reportes y actas en formato PDF.
*   **multer**: Middleware para la recepción y procesamiento de archivos (multipart/form-data).
*   **xlsx / csv-parser**: Procesamiento de archivos Excel y CSV para la importación masiva de aprendices.

### Automatización y Comunicaciones
*   **nodemailer**: Envío de notificaciones automáticas por correo electrónico.
*   **node-cron**: Programación de tareas en segundo plano (alertas de vencimiento, revisiones pendientes).

---

## 🏗️ Estructura y Flujo de Desarrollo (Módulos 1-12)

El desarrollo se realizó de forma incremental, siguiendo una jerarquía de dependencias estricta:

### 1. Autenticación (`Auth`)
*   **Clave**: Gestión de roles (`ADMIN`, `INSTRUCTOR`, `APPRENTICE`) y primer inicio de sesión obligatorio.
*   **Funciones**: `login()`, `changePasswordFirstLogin()`, `verifyToken` (middleware).

### 2. Configuración del Sistema (`SystemConfig`)
*   **Clave**: Variables globales dinámicas (horas por bitácora, días de alerta).
*   **Variable**: `key` (PK), `value` (Mixed).

### 3. Usuarios (`Users`)
*   **Clave**: Gestión de perfiles y carga masiva.
*   **Funciones**: `createInstructor()`, `importApprenticesFromCSV()`.

### 4. Empresas (`Companies`)
*   **Clave**: Registro de entes coformadores y supervisores.

### 5. Etapas Productivas (`ProductiveStages`)
*   **Clave**: El núcleo del sistema. Vincula aprendiz, empresa e instructores.
*   **Lógica**: `checkAndAdvanceStatus()` — Función que evalúa el progreso para cambiar automáticamente de `ACTIVE` a `IN_FOLLOWUP` o `CERTIFICATION`.

### 6. Bitácoras (`Bitacoras`)
*   **Clave**: Entregas quincenales.
*   **Funciones**: `submitBitacora()`, `approveBitacora()`. Incrementa `completedBitacoras`.

### 7. Seguimientos (`Trackings`)
*   **Clave**: Visitas técnicas (Presenciales/Virtuales) y extraordinarias.
*   **Lógica**: Requiere validación de firmas (`signedByInstructor`, `signedByApprentice`) antes de la ejecución.

### 8. Bolsa de Horas (`Hours`)
*   **Clave**: Registro mensual de la labor del instructor.
*   **Funciones**: `addHours()` (centralizada), `carryOver()` (traslado de excedentes).

### 9. Documentos de Certificación (`Documents`)
*   **Clave**: Flujo final de 3 documentos obligatorios.
*   **Regla**: Solo se activan horas de certificación si los 3 documentos están en estado `APPROVED`.

### 10. Novedades (`Novelties`)
*   **Clave**: Reporte de incidentes críticos (deserción, etc.).
*   **Punto Clave**: Generación inmediata de acta PDF al reportar.

### 11. Notificaciones (`Notifications`)
*   **Clave**: Sistema dual.
*   **Funciones**: `notificationService.send()` — Despacha a la base de datos y por Email simultáneamente sin bloquear el flujo principal.

### 12. Reportes (`Reports`)
*   **Clave**: Inteligencia de datos y exportación.
*   **Funciones**: `getEPSummary()`, `exportToPdf()`.

---

## 🛠️ Puntos Clave de Implementación

### Generación de PDFs (`pdfGenerator.util.js`)
Se implementó un motor genérico basado en **PDFKit** que recibe secciones de datos y genera un buffer listo para descarga o almacenamiento en Drive.
*   **Función**: `generatePdf({ title, sections, summary })`.
*   **Uso**: Reportes administrativos, actas de novedades y certificados de horas.

### Gestión de Archivos (`Files`)
El sistema utiliza **Multer** para recibir archivos. Aunque el backend procesa el archivo, la arquitectura está preparada para integrarse con **Google Drive API**, utilizando un patrón de "Mocks" en desarrollo que simula el almacenamiento permanente y genera URLs de acceso.

### Trazabilidad (`AuditLog`)
Cada acción crítica (borrar un documento, cambiar un estado, pagar horas) queda registrada en la colección `audit_logs`.
*   **Campos**: `action`, `performedBy`, `entityId`, `details`.

### Automatización (`Cron Jobs`)
En `src/jobs/alerts.job.js` residen las tareas que corren diariamente a las 8:00 AM para:
*   Notificar bitácoras no entregadas.
*   Alertar a Admins sobre instructores con revisiones atrasadas (> 7 días).
*   Avisar sobre vencimientos de fichas.

---

## 🚦 Cómo ejecutar el proyecto

1.  **Instalar dependencias**: `npm install`
2.  **Configurar entorno**: Crear un archivo `.env` con:
    *   `MONGODB_URI`, `JWT_SECRET`, `EMAIL_HOST`, `EMAIL_USER`, `EMAIL_PASS`.
3.  **Semilla de datos**: `npm run seed` (crea el Admin inicial y configuraciones).
4.  **Pruebas**: `npm test` (ejecuta los 12 suites de pruebas con Jest).





  🛠️ Puntos Clave de Especificidad Técnica

   1. Archivos (Files): Se gestionan con multer. En el service, las funciones reciben el
      objeto file, extraen originalname y buffer, y lo envían a un helper que simula (mock)
      la subida a Google Drive.
   2. PDFs: La función pdfGenerator.generatePdf es la única que toca la librería pdfkit.
      Recibe un objeto estructurado (sections, summary) para mantener los reportes limpios
      y fáciles de editar.
   3. Metodología de Tests:
       * Usamos Supertest para llamadas HTTP reales a los endpoints.
       * Usamos Mongoose con una base de datos local para asegurar limpieza total entre
         pruebas (beforeAll limpia colecciones).
       * Determinismo: Fijamos fechas (ej. Mayo 2025) para que los reportes de horas den
         resultados exactos sin importar cuándo corras el test.
