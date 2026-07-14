REPFORA E.P. - Seguimiento de Etapas Productivas SENA
📖 Descripción del Proyecto
REPFORA E.P. es una plataforma web desarrollada específicamente para el SENA con el objetivo de resolver la dispersión de información durante las etapas productivas. El sistema centraliza, digitaliza y optimiza el diligenciamiento, control y seguimiento de las bitácoras y evidencias de los aprendices.

Antes de este sistema, no existía una plataforma dedicada para este fin. Ahora, REPFORA establece un flujo de trabajo claro y directo entre la administración, los instructores y los estudiantes.

🚀 Roles y Funcionalidades Principales
El sistema está diseñado para atender a tres tipos de usuarios, cada uno con permisos y herramientas específicas:

👨‍🎓 Aprendices:

Carga de archivos y evidencias (bitácoras) de su etapa productiva.

Seguimiento del estado de sus revisiones.

Recepción de notificaciones sobre aprobaciones o correcciones.

👨‍🏫 Instructores de Seguimiento:

Panel de control con la lista de aprendices asignados.

Descarga y revisión de los archivos subidos por los estudiantes.

Aprobación de bitácoras y envío de retroalimentación.

⚙️ Administrador:

Gestión global de la plataforma y de los usuarios.

Asignación estratégica de instructores a los respectivos aprendices.

Supervisión general del progreso y cumplimiento de las etapas productivas.

🛠️ Tecnologías y Arquitectura
Este proyecto está construido íntegramente sobre el ecosistema JavaScript, garantizando un rendimiento fluido y escalabilidad:

Frontend: Vue.js potenciado con Quasar Framework para una interfaz reactiva y optimizada.

Base de Datos: MongoDB Atlas para un almacenamiento NoSQL seguro en la nube.

Automatización: Tareas programadas mediante Crons para ejecutar validaciones periódicas y disparar recordatorios del sistema.

Integraciones y APIs:

Google Drive API: Utilizado como sistema de almacenamiento en la nube para gestionar de forma segura todos los archivos y evidencias pesadas subidas por los aprendices.

Brevo API: Sistema de mensajería para el envío automatizado de correos electrónicos transaccionales (alertas de revisión, asignación de instructores, etc.).

⚙️ Requisitos Previos
Para desplegar o contribuir a este proyecto en un entorno local, asegúrate de contar con:

Node.js (versión recomendada LTS)

Git

Además, se requiere configurar las credenciales para los servicios de terceros:

Cadena de conexión de MongoDB Atlas.

Credenciales de Google Cloud Console (Drive API).

API Key de Brevo.
