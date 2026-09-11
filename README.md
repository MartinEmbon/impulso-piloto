# Impulso · Piloto funcional

Plataforma móvil de habilidades laborales. Nombre provisional. UI en español argentino. Código portable para GitHub y Render, sin dependencias npm externas. Requiere Node 24 (usa node:sqlite).

## Incluido
- Cuatro misiones completas de Atención al Cliente, con dos preguntas cada una y práctica escrita opcional.
- Catálogo ampliable; portugués gastronómico, inglés gastronómico y administración están explícitamente en preparación.
- Modo de prueba sin registro con progreso local. Cuentas por invitación con sesiones HttpOnly, contraseñas scrypt y progreso SQLite.
- Acceso de coordinación con métricas reales y exportación CSV. Una institución por instalación.
- Corrección de preguntas en servidor. Mejor resultado por misión, intentos y desbloqueo secuencial. 50 XP por misión aprobada sin duplicados.
- Tutor opcional vía OpenAI Responses API, clave solo en servidor, máximo 20 llamadas/día por cuenta y 5/minuto. Sus devoluciones no asignan notas.
- PWA con iconos y caché de interfaz. Necesita conexión para API, registro, corrección, IA y métricas. No promete funcionamiento offline integral.
- Personaje original generado con IA e introducción MP4 de 15 segundos con movimiento de cámara y subtítulos. Sin voz ni animación labial, sin avatar en tiempo real. El mismo video introductorio se reutiliza.

## Ejecutar
1. Instalá Node 24.
2. Copiá `.env.example` a `.env` y reemplazá los códigos con valores distintos y privados.
3. `node --env-file=.env server.mjs`
4. Abrí `http://localhost:3000`.

No hay que instalar paquetes. `npm run build` verifica sintaxis; `npm test` ejecuta pruebas de integración. El servidor sirve `dist/` y la API.

## Cuentas de piloto
Usá «Tengo una invitación». El código INSTITUTION_CODE crea alumnos; ADMIN_CODE crea coordinadores. El código de coordinador no se comparte con alumnos ni se introduce en el frontend. No hay cuentas ni passwords preinstaladas. El avance anónimo no se migra al registrarse. Los datos de pruebas no se incluyen en el repositorio.

## Render
Conectá este repositorio en Render como Blueprint (`render.yaml`). El archivo solicita un servicio Node y disco persistente de 1 GB: son recursos de pago; revisá el precio que muestre Render antes de crearlos. Los datos se guardan en `/var/data`. Sin disco persistente, los cambios pueden perderse al desplegar. Configurá INSTITUTION_CODE y ADMIN_CODE al crear el Blueprint; deben ser diferentes. Agregá opcionalmente INSTITUTION_NAME.

El Blueprint no publica nada por sí solo. Arranque: `npm start`, build: `npm run build`, health: `/api/health`. La publicación HTTPS permite cookies Secure y PWA. Usar una sola instancia con SQLite; para múltiples instancias o instituciones, migrar a Postgres y tenancy explícita.

## Activar tutor IA
En variables de entorno de Render configurá OPENAI_API_KEY y OPENAI_MODEL con un modelo Responses al que tu proyecto tenga acceso. Nunca pongas la clave en el repositorio ni en el chat. Sin configuración la app explica que el tutor no está habilitado. Fijá además presupuesto/alertas en el proyecto proveedor. El límite por alumno no reemplaza un presupuesto global. La llamada usa `store:false`; revisá las condiciones de tratamiento del proveedor antes de incorporar alumnos reales. La disponibilidad del modelo no se infiere de la conversación en ChatGPT.

## Alcance y pendientes antes de un lanzamiento comercial
Es un piloto funcional, no un SaaS comercial completo. Faltan recuperación de contraseña, verificación de email, borrado/exportación de datos personales, política de privacidad y términos propios, backups automatizados, gestión de cohortes e invitaciones individuales, cobros recurrentes, editor de rutas, aislamiento multiinstitución, audio conversacional, contenido pedagógico revisado y seguimiento de inserción laboral. Las métricas son de ejercicios, no certifican competencias ni empleo. El umbral inicial de 50% es de demostración y debe revisarse pedagógicamente.

## Arquitectura
`curriculum.mjs`: contenido y claves de corrección en servidor. `server.mjs`: HTTP/API, autenticación, SQLite y puente OpenAI. `dist/app.js`: experiencia móvil y dashboards. `dist/style.css`: sistema visual. `dist/assets/`: personaje y video. `render.yaml`: despliegue.

## Referencias verificadas
- https://render.com/docs/blueprint-spec
- https://developers.openai.com/api/docs/guides/text

La integración OpenAI requiere prueba real con credenciales del propietario; no se ejecutó una llamada facturable durante esta entrega.
