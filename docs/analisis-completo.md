# Análisis Completo: Linkwarden

> Ejercicio retrospectivo de simulación Scrum+Kanban sobre un proyecto de software existente.

---

## 1. Análisis del Proyecto

### 1.1 Objetivo del Sistema

Linkwarden es un gestor de marcadores (bookmarks) colaborativo, auto-hospedado y de código abierto. Su propósito es recolectar, organizar, leer, anotar y preservar páginas web en un solo lugar, evitando la pérdida de contenido (*link rot*) mediante la generación de copias en formato screenshot, PDF y HTML legible.

### 1.2 Usuarios Involucrados

| Rol | Descripción |
|-----|-------------|
| **Visitante** | Usuario no registrado que puede ver colecciones y links públicos |
| **Usuario Registrado** | Usuario autenticado que puede crear y gestionar sus marcadores |
| **Miembro de Colección** | Usuario con permisos específicos (crear, editar, eliminar) sobre una colección compartida |
| **Administrador** | Usuario con acceso al panel de administración (gestión de usuarios, estadísticas del worker) |
| **Suscriptor** | Usuario con suscripción activa (Stripe) que puede invitar usuarios adicionales |

### 1.3 Funcionalidades Reales Identificadas

| # | Funcionalidad | Módulo | Evidencia en Código |
|---|---|---|---|
| 1 | Registro de usuario | Autenticación | `pages/register.tsx`, `POST /api/v1/auth/[...nextauth]` |
| 2 | Inicio de sesión (40+ SSO + email) | Autenticación | `pages/login.tsx`, `pages/api/v1/auth/[...nextauth].ts` (1513 líneas) |
| 3 | Recuperación de contraseña | Autenticación | `pages/forgot.tsx`, `POST /api/v1/auth/forgot-password` |
| 4 | Verificación de email | Autenticación | `pages/auth/verify-email.tsx`, `POST /api/v1/auth/verify-email` |
| 5 | Configuración de perfil | Cuenta | `pages/settings/account.tsx` (nombre, username, email, avatar) |
| 6 | Cambio de contraseña | Cuenta | `pages/settings/password.tsx` |
| 7 | Preferencias (tema, archival, AI) | Cuenta | `pages/settings/preference.tsx` |
| 8 | Crear marcador (URL) | Marcadores | `POST /api/v1/links`, `LinkCreator.tsx` |
| 9 | Listar y ver marcadores | Marcadores | `GET /api/v1/links`, `pages/links/[id].tsx` |
| 10 | Editar marcador | Marcadores | `PUT /api/v1/links/[id]` |
| 11 | Eliminar marcador (individual/masivo) | Marcadores | `DELETE /api/v1/links/[id]`, `DELETE /api/v1/links` |
| 12 | Fijar/desfijar marcadores | Marcadores | `pinLink.ts`, `LinkPin.tsx` |
| 13 | Vistas de marcadores (card, list, masonry) | Marcadores | `components/LinkViews/` |
| 14 | Crear colección | Colecciones | `POST /api/v1/collections` |
| 15 | Listar colecciones | Colecciones | `GET /api/v1/collections` |
| 16 | Editar colección (nombre, ícono, color) | Colecciones | `PUT /api/v1/collections/[id]` |
| 17 | Eliminar colección | Colecciones | `DELETE /api/v1/collections/[id]` |
| 18 | Sub-colecciones (anidadas) | Colecciones | Modelo `Collection.parentId` |
| 19 | Crear etiquetas | Etiquetas | `POST /api/v1/tags` |
| 20 | Asignar etiquetas a marcadores | Etiquetas | Relación `LinkTag` en schema |
| 21 | Fusionar etiquetas | Etiquetas | `POST /api/v1/tags/merge` |
| 22 | Eliminar etiquetas (individual/masivo) | Etiquetas | `DELETE /api/v1/tags/[id]`, `DELETE /api/v1/tags` |
| 23 | Auto-etiquetado con AI | Etiquetas | `worker/workers/autoTagPreservedLinks.ts` |
| 24 | Buscar marcadores (full-text) | Búsqueda | `GET /api/v1/search`, Meilisearch |
| 25 | Filtrar por colección, etiqueta, texto | Búsqueda | `LinkRequestQuery` |
| 26 | Archivar URL (screenshot, PDF, HTML, readable) | Preservación | `worker/lib/archiveHandler.ts`, Playwright |
| 27 | Ver versión preservada (4 formatos) | Preservación | `components/Preservation/`, `GET /api/v1/preserved/view` |
| 28 | Subir archivo como marcador | Preservación | `POST /api/v1/archives` (PDF, imagen, HTML) |
| 29 | Eliminar archivos preservados | Preservación | `DELETE /api/v1/links/archive` |
| 30 | Envío a Wayback Machine | Preservación | `schema.prisma: archiveAsWaybackMachine` |
| 31 | Compartir colección con miembros | Colaboración | `EditCollectionSharingModal.tsx` |
| 32 | Gestionar permisos por miembro | Colaboración | Modelo `UsersAndCollections` (canCreate/Update/Delete) |
| 33 | Colección pública | Colaboración | `Collection.isPublic`, `pages/public/collections/[id]` |
| 34 | Feed RSS de colección pública | Colaboración | `pages/public/collections/[id]/rss.tsx` |
| 35 | Suscripción a RSS | RSS | `POST /api/v1/rss`, `worker/workers/rssPolling.ts` |
| 36 | Crear API token | Avanzado | `POST /api/v1/tokens` |
| 37 | Gestionar API tokens | Avanzado | `pages/settings/access-tokens.tsx` |
| 38 | Importar marcadores (HTML, Pocket, Wallabag, Omnivore) | Migración | `controllers/migration/`, `ImportDropdown.tsx` |
| 39 | Exportar datos (JSON) | Migración | `controllers/migration/exportData.ts` |
| 40 | Panel admin: gestionar usuarios | Administración | `pages/admin/user-administration.tsx`, `CRUD /api/v1/users` |
| 41 | Ver estadísticas del worker | Administración | `GET /api/v1/worker` |
| 42 | Suscripción Stripe (checkout, billing) | Facturación | `pages/settings/billing.tsx`, `lib/api/stripe/` |
| 43 | Dashboard personalizable | Dashboard | `pages/dashboard.tsx`, modelo `DashboardSection` |
| 44 | Anotaciones/highlights en links preservados | Anotaciones | `POST /api/v1/highlights`, modelo `Highlight` |
| 45 | Drag & drop para organizar | UI | `components/DragNDrop.tsx`, `@dnd-kit` |
| 46 | Modo oscuro/claro/auto | UI | `ToggleDarkMode.tsx`, `User.theme` |
| 47 | Multi-idioma (i18n) | UI | `next-i18next`, `crowdin.yml` |

### 1.4 Stack Tecnológico Real

| Capa | Tecnologías |
|---|---|
| Frontend | Next.js 15, React 19, TypeScript, Tailwind CSS, daisyUI, shadcn/ui |
| Estado | TanStack Query, Zustand |
| Backend | Next.js API Routes (Pages Router), Prisma 6, PostgreSQL |
| Auth | NextAuth.js (40+ proveedores SSO) |
| Búsqueda | Meilisearch |
| Worker | Playwright, Mozilla Readability, AI SDK (OpenAI, Anthropic, Ollama, etc.) |
| Pagos | Stripe |
| Infra | Docker, Docker Compose, Yarn 4 (monorepo) |
| Storage | Local filesystem / AWS S3 compatible |
| Testing | Playwright (E2E), Vitest |
| CI/i18n | ESLint, Prettier, Crowdin |

### 1.5 Restricciones del Negocio

- Autenticación obligatoria para operaciones de escritura
- Límite de 30,000 links por usuario (configurable)
- Tamaño máximo de archivo subido: 10 MB
- Límite de 20 suscripciones RSS por usuario
- Periodo de prueba de 14 días (configurable)
- Demo mode deshabilita escritura
- Registro deshabilitable vía variable de entorno
- Paginación por cursor en listas

---

## 2. Épicas

| Épica | ID | Descripción |
|---|---|---|
| **Autenticación y Cuenta** | E01 | Gestión de acceso al sistema: registro, inicio de sesión, recuperación de contraseña y configuración de perfil |
| **Gestión de Marcadores** | E02 | Operaciones CRUD sobre marcadores: crear, organizar en colecciones, editar, eliminar y fijar |
| **Etiquetado y Búsqueda** | E03 | Sistema de etiquetado y búsqueda full-text para localizar marcadores |
| **Preservación de Contenido** | E04 | Archivado automático de páginas web en múltiples formatos y visualización de versiones preservadas |
| **Colaboración** | E05 | Compartición de colecciones, gestión de permisos y publicación de contenido público |
| **Administración y Avanzado** | E06 | Funcionalidades avanzadas: API tokens, importación/exportación, panel de administración y suscripciones |

---

## 3. Product Backlog — Historias de Usuario

### Épica E01: Autenticación y Cuenta

---

#### US01 — Registrarse en la plataforma

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero registrarme en la plataforma con mi correo electrónico y una contraseña, para acceder a las funcionalidades de gestión de marcadores |
| **Prioridad** | Alta |
| **Estimación** | 3 SP |
| **Dependencias** | — |

**Criterios de Aceptación:**
1. El sistema debe mostrar un formulario de registro con campos de nombre, correo electrónico, contraseña y confirmación de contraseña.
2. El sistema debe validar que el correo electrónico no esté registrado previamente.
3. El sistema debe exigir una contraseña de al menos 8 caracteres con al menos una letra mayúscula y un número.
4. El sistema debe enviar un correo de verificación al correo registrado.
5. El sistema debe redirigir al usuario al dashboard después de verificar su correo.
6. El sistema debe mostrar mensajes de error claros si el registro falla (correo duplicado, contraseña débil, etc.).
7. El sistema debe permitir deshabilitar el registro mediante una variable de entorno.

**Tareas Técnicas:**
- Crear endpoint `POST /api/v1/auth/register`
- Implementar formulario de registro en `pages/register.tsx`
- Configurar envío de email de verificación con Nodemailer
- Implementar validación de datos con Zod
- Agregar variable `NEXT_PUBLIC_DISABLE_REGISTRATION`

**Comentario de Negocio:** Funcionalidad crítica para la adquisición de usuarios. Debe ser intuitiva y segura.

---

#### US02 — Iniciar sesión con proveedores SSO

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero iniciar sesión usando mi cuenta de Google, GitHub u otro proveedor SSO, para acceder sin necesidad de recordar una contraseña adicional |
| **Prioridad** | Alta |
| **Estimación** | 5 SP |
| **Dependencias** | US01 |

**Criterios de Aceptación:**
1. El sistema debe mostrar al menos 5 proveedores SSO en la página de inicio de sesión.
2. El sistema debe redirigir al proveedor SSO seleccionado para autenticación.
3. El sistema debe crear automáticamente una cuenta local si es la primera vez que el usuario inicia sesión con SSO.
4. El sistema debe vincular la cuenta SSO con la cuenta local si el correo ya está registrado.
5. El sistema debe redirigir al dashboard después de una autenticación exitosa.
6. El sistema debe mostrar un mensaje de error si la autenticación con el proveedor falla.

**Tareas Técnicas:**
- Configurar NextAuth.js con múltiples proveedores
- Implementar página de login en `pages/login.tsx`
- Configurar lógica de vinculación de cuentas
- Implementar botones de SSO con iconos de cada proveedor

**Comentario de Negocio:** El SSO reduce la fricción en el registro. Google y GitHub cubren >80% de los usuarios técnicos.

---

#### US03 — Recuperar contraseña

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero recuperar mi contraseña mediante un enlace enviado a mi correo, para restablecer el acceso a mi cuenta si olvido mi contraseña |
| **Prioridad** | Media |
| **Estimación** | 3 SP |
| **Dependencias** | US01 |

**Criterios de Aceptación:**
1. El sistema debe mostrar un formulario para ingresar el correo electrónico.
2. El sistema debe enviar un correo con un enlace de restablecimiento válido por 1 hora.
3. El sistema debe permitir al usuario ingresar una nueva contraseña después de hacer clic en el enlace.
4. El sistema debe invalidar el token después de usarlo o al expirar.
5. El sistema debe notificar al usuario que el correo fue enviado (incluso si el correo no existe, por seguridad).
6. El sistema debe redirigir al login después de restablecer la contraseña exitosamente.

**Tareas Técnicas:**
- Crear endpoint `POST /api/v1/auth/forgot-password`
- Crear endpoint `POST /api/v1/auth/reset-password`
- Implementar plantilla de email con Handlebars
- Implementar página `pages/forgot.tsx` y `pages/auth/reset-password.tsx`
- Configurar expiración de tokens

**Comentario de Negocio:** Funcionalidad de seguridad esencial para retención de usuarios.

---

#### US04 — Configurar perfil y preferencias

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero configurar mi perfil (nombre, foto, idioma) y preferencias (tema visual, defaults de archivado), para personalizar la experiencia según mis necesidades |
| **Prioridad** | Media |
| **Estimación** | 5 SP |
| **Dependencias** | US01 |

**Criterios de Aceptación:**
1. El sistema debe permitir cambiar nombre, username y foto de perfil.
2. El sistema debe permitir seleccionar entre tema oscuro, claro o automático.
3. El sistema debe permitir configurar el idioma de la interfaz.
4. El sistema debe permitir configurar opciones por defecto de archivado (screenshot, PDF, readable, HTML).
5. El sistema debe permitir cambiar la contraseña desde la configuración.
6. El sistema debe persistir todas las preferencias en la base de datos.
7. El sistema debe aplicar los cambios inmediatamente sin requerir reinicio de sesión.

**Tareas Técnicas:**
- Implementar páginas de settings (`account.tsx`, `password.tsx`, `preference.tsx`)
- Crear endpoint `PUT /api/v1/users/[id]/preference`
- Implementar selector de tema con persistencia en BD
- Configurar i18n con next-i18next

**Comentario de Negocio:** La personalización incrementa la satisfacción del usuario. El modo oscuro es una de las características más solicitadas.

---

### Épica E02: Gestión de Marcadores

---

#### US05 — Guardar un marcador (URL)

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero guardar un marcador ingresando una URL, para acceder rápidamente a páginas web importantes desde un solo lugar |
| **Prioridad** | Alta |
| **Estimación** | 8 SP |
| **Dependencias** | US01 |

**Criterios de Aceptación:**
1. El sistema debe permitir ingresar una URL y automáticamente obtener el título, descripción y favicon de la página.
2. El sistema debe permitir seleccionar o crear una colección al guardar el marcador.
3. El sistema debe permitir asignar etiquetas al marcador.
4. El sistema debe permitir agregar una descripción personalizada.
5. El sistema debe iniciar el archivado automático (screenshot, PDF, readable) en segundo plano.
6. El sistema debe validar que la URL sea accesible antes de guardar.
7. El sistema debe impedir duplicados si la opción está activada.
8. El sistema debe mostrar el nuevo marcador en la colección correspondiente inmediatamente.

**Tareas Técnicas:**
- Implementar endpoint `POST /api/v1/links` con controlador `postLink`
- Crear componente `LinkCreator.tsx` con formulario
- Implementar extracción de metadatos (Open Graph, favicon)
- Implementar cola de archivado en el worker
- Implementar lógica anti-duplicados

**Comentario de Negocio:** Funcionalidad principal del producto. La experiencia debe ser rápida y sin fricción.

---

#### US06 — Organizar marcadores en colecciones

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero crear y gestionar colecciones de marcadores (incluyendo sub-colecciones anidadas), para organizar mis enlaces por temas o proyectos |
| **Prioridad** | Alta |
| **Estimación** | 8 SP |
| **Dependencias** | US05 |

**Criterios de Aceptación:**
1. El sistema debe permitir crear colecciones con nombre, descripción, ícono y color personalizable.
2. El sistema debe permitir crear sub-colecciones dentro de una colección existente.
3. El sistema debe mostrar la estructura jerárquica de colecciones en el panel lateral.
4. El sistema debe permitir mover marcadores entre colecciones (drag & drop).
5. El sistema debe permitir renombrar y reordenar colecciones.
6. El sistema debe permitir eliminar una colección (con confirmación) manteniendo los marcadores huérfanos accesibles.

**Tareas Técnicas:**
- Implementar endpoints CRUD para colecciones (`/api/v1/collections`)
- Implementar modelo con `parentId` para jerarquía
- Crear componentes de árbol de colecciones
- Implementar drag & drop con `@dnd-kit`
- Implementar selector de íconos y colores

**Comentario de Negocio:** La organización jerárquica es clave para usuarios con muchos marcadores. Diferenciador frente a competidores.

---

#### US07 — Editar y eliminar marcadores

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero editar y eliminar marcadores (individualmente o en lote), para mantener mi colección actualizada y libre de enlaces rotos o irrelevantes |
| **Prioridad** | Alta |
| **Estimación** | 5 SP |
| **Dependencias** | US05 |

**Criterios de Aceptación:**
1. El sistema debe permitir editar el nombre, URL, descripción, colección y etiquetas de un marcador.
2. El sistema debe permitir seleccionar múltiples marcadores y aplicar acciones masivas (mover a colección, asignar etiqueta, eliminar).
3. El sistema debe solicitar confirmación antes de eliminar (individual o masivo).
4. El sistema debe permitir deshacer la eliminación dentro de un período de tiempo.
5. El sistema debe actualizar la vista inmediatamente después de cualquier modificación.
6. El sistema debe verificar permisos si el marcador está en una colección compartida.

**Tareas Técnicas:**
- Implementar endpoint `PUT /api/v1/links/[id]` y `DELETE /api/v1/links/[id]`
- Implementar acciones masivas (`PUT /api/v1/links` bulk, `DELETE /api/v1/links` bulk)
- Implementar selección múltiple en UI
- Implementar confirmación y toast de deshacer

**Comentario de Negocio:** La edición masiva es una funcionalidad muy solicitada por usuarios avanzados.

---

#### US08 — Fijar marcadores

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero fijar marcadores importantes al inicio de mi lista, para acceder rápidamente a los enlaces que uso con más frecuencia |
| **Prioridad** | Baja |
| **Estimación** | 3 SP |
| **Dependencias** | US05 |

**Criterios de Aceptación:**
1. El sistema debe permitir fijar y desafijar un marcador con un solo clic.
2. El sistema debe mostrar los marcadores fijados antes que los no fijados en la lista.
3. El sistema debe mostrar los marcadores fijados en una sección separada del dashboard.
4. El sistema debe persistir el estado de fijado en la base de datos.
5. El sistema debe permitir fijar múltiples marcadores simultáneamente.

**Tareas Técnicas:**
- Implementar endpoint `POST /api/v1/links/[id]/pin`
- Implementar modelo `Link.pinnedBy` (relación muchos a muchos)
- Crear componente `LinkPin.tsx`
- Agregar sección "Pinned Links" al dashboard

**Comentario de Negocio:** Funcionalidad simple pero de alto valor para la experiencia diaria del usuario.

---

### Épica E03: Etiquetado y Búsqueda

---

#### US09 — Crear y asignar etiquetas a marcadores

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero crear etiquetas y asignarlas a mis marcadores, para clasificarlos por temas transversales independientemente de la colección |
| **Prioridad** | Alta |
| **Estimación** | 5 SP |
| **Dependencias** | US05 |

**Criterios de Aceptación:**
1. El sistema debe permitir crear etiquetas con nombre único por usuario.
2. El sistema debe permitir asignar una o varias etiquetas a un marcador.
3. El sistema debe mostrar las etiquetas como chips/badges en la vista de marcadores.
4. El sistema debe autocompletar etiquetas existentes mientras el usuario escribe.
5. El sistema debe permitir filtrar marcadores por etiqueta desde la interfaz.
6. El sistema debe permitir definir opciones de archivado por etiqueta (screenshot, PDF, etc.).

**Tareas Técnicas:**
- Implementar endpoints CRUD para tags (`/api/v1/tags`)
- Implementar componente `TagSelector.tsx` con autocompletado
- Implementar filtro por etiqueta en queries
- Agregar opciones de archivado por etiqueta en el modelo `Tag`

**Comentario de Negocio:** Las etiquetas son el mecanismo principal de organización transversal. Deben ser rápidas de asignar.

---

#### US10 — Buscar marcadores

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero buscar entre mis marcadores por texto libre y filtrar por colección y etiquetas, para encontrar rápidamente enlaces específicos sin navegar manualmente |
| **Prioridad** | Alta |
| **Estimación** | 8 SP |
| **Dependencias** | US05, US09 |

**Criterios de Aceptación:**
1. El sistema debe permitir buscar por texto libre en título, URL y descripción.
2. El sistema debe permitir filtrar por colección y etiqueta simultáneamente.
3. El sistema debe mostrar resultados en tiempo real mientras el usuario escribe.
4. El sistema debe ordenar resultados por relevancia.
5. El sistema debe incluir el contenido textual preservado en los resultados de búsqueda.
6. El sistema debe mostrar resultados paginados con carga rápida.

**Tareas Técnicas:**
- Configurar Meilisearch con el worker de indexación
- Implementar endpoint `GET /api/v1/search`
- Implementar página `pages/search.tsx`
- Implementar `SearchBar.tsx` con búsqueda en tiempo real
- Sincronizar índice Meilisearch con el worker

**Comentario de Negocio:** La búsqueda rápida es crítica para usuarios con +1000 marcadores. Meilisearch ofrece resultados en milisegundos.

---

#### US11 — Fusionar y eliminar etiquetas masivamente

| Campo | Valor |
|---|---|
| **Rol** | investigador |
| **Descripción** | Como investigador, quiero fusionar etiquetas duplicadas y eliminar etiquetas en lote, para mantener la taxonomía de etiquetas limpia y consistente |
| **Prioridad** | Baja |
| **Estimación** | 3 SP |
| **Dependencias** | US09 |

**Criterios de Aceptación:**
1. El sistema debe permitir seleccionar dos etiquetas y fusionarlas en una sola.
2. El sistema debe reasignar todos los marcadores de la etiqueta fusionada a la etiqueta destino.
3. El sistema debe eliminar la etiqueta origen después de la fusión.
4. El sistema debe permitir seleccionar múltiples etiquetas y eliminarlas simultáneamente.
5. El sistema debe solicitar confirmación antes de eliminar etiquetas.

**Tareas Técnicas:**
- Implementar endpoint `POST /api/v1/tags/merge`
- Implementar endpoint `DELETE /api/v1/tags` (bulk)
- Implementar interfaz de selección múltiple de etiquetas

**Comentario de Negocio:** Funcionalidad de limpieza para usuarios avanzados que gestionan muchas etiquetas.

---

### Épica E04: Preservación de Contenido

---

#### US12 — Archivar páginas web automáticamente

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero que el sistema archive automáticamente las páginas que guardo (screenshot, PDF, HTML legible), para preservar el contenido aunque la página original deje de estar disponible |
| **Prioridad** | Alta |
| **Estimación** | 13 SP |
| **Dependencias** | US05 |

**Criterios de Aceptación:**
1. El sistema debe capturar automáticamente un screenshot de la página al guardar un marcador.
2. El sistema debe generar un PDF de la página.
3. El sistema debe extraer el contenido en formato legible (Readable View).
4. El sistema debe generar un archivo HTML completo (monolito) de la página.
5. El sistema debe procesar el archivado en segundo plano sin bloquear al usuario.
6. El sistema debe permitir configurar qué formatos archivar por defecto.
7. El sistema debe permitir re-archivar una página manualmente.
8. El sistema debe enviar la página a Wayback Machine si está configurado.

**Tareas Técnicas:**
- Implementar worker de archivado con Playwright
- Implementar `archiveHandler.ts` (screenshot, PDF, readable, monolith)
- Configurar cola de procesamiento con lotes de 5 links
- Integrar Mozilla Readability para vista legible
- Integrar envío a Wayback Machine

**Comentario de Negocio:** Funcionalidad estrella del producto. El archivado automático es el principal diferenciador frente a otros gestores de marcadores.

---

#### US13 — Visualizar versiones preservadas

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero ver las versiones preservadas de un marcador (screenshot, PDF, readable, HTML), para acceder al contenido aunque la página original haya cambiado o caído |
| **Prioridad** | Alta |
| **Estimación** | 5 SP |
| **Dependencias** | US12 |

**Criterios de Aceptación:**
1. El sistema debe mostrar una vista previa del screenshot al seleccionar un marcador.
2. El sistema debe permitir cambiar entre los formatos preservados (screenshot, PDF, readable, HTML).
3. El sistema debe abrir la vista preservada sin salir de la aplicación.
4. El sistema debe mostrar el contenido legible con formato limpio y sin publicidad.
5. El sistema debe permitir descargar el PDF o screenshot.
6. El sistema debe mostrar indicadores de qué formatos están disponibles.

**Tareas Técnicas:**
- Crear componentes `PreservationContent.tsx`, `PreservationNavbar.tsx`
- Implementar endpoint `GET /api/v1/preserved/view`
- Implementar `ReadableView.tsx` con Mozilla Readability
- Implementar visualización de PDF incrustado

**Comentario de Negocio:** La visualización integrada evita que el usuario tenga que salir de la aplicación, mejorando la retención.

---

#### US14 — Subir archivos como marcadores

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero subir archivos (PDF, imágenes, HTML) como marcadores, para preservar documentos locales junto con mis marcadores web |
| **Prioridad** | Media |
| **Estimación** | 5 SP |
| **Dependencias** | US05, US12 |

**Criterios de Aceptación:**
1. El sistema debe permitir seleccionar y subir archivos PDF, PNG, JPG y HTML.
2. El sistema debe limitar el tamaño del archivo a 10 MB.
3. El sistema debe mostrar una vista previa del archivo subido.
4. El sistema debe almacenar el archivo en el sistema de archivos (local o S3).
5. El sistema debe tratar el archivo subido como un marcador más (etiquetable, organizable en colecciones).

**Tareas Técnicas:**
- Implementar endpoint `POST /api/v1/archives`
- Configurar almacenamiento con `packages/filesystem/` (local/S3)
- Implementar límite de tamaño con `NEXT_PUBLIC_MAX_FILE_BUFFER`
- Mostrar vista previa según tipo de archivo

**Comentario de Negocio:** Útil para investigadores que quieren adjuntar PDFs de artículos académicos a sus colecciones.

---

### Épica E05: Colaboración

---

#### US15 — Compartir colecciones con otros usuarios

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero compartir una colección con otros usuarios de la plataforma, para colaborar en la curaduría de enlaces sobre un tema común |
| **Prioridad** | Alta |
| **Estimación** | 5 SP |
| **Dependencias** | US06 |

**Criterios de Aceptación:**
1. El sistema debe permitir invitar a otros usuarios a una colección por correo o username.
2. El sistema debe enviar una notificación al usuario invitado.
3. El sistema debe mostrar la colección compartida en el panel del miembro invitado.
4. El sistema debe permitir al propietario remover miembros de la colección.
5. El sistema debe permitir al miembro abandonar la colección compartida.

**Tareas Técnicas:**
- Implementar modelo `UsersAndCollections`
- Crear componente `EditCollectionSharingModal.tsx`
- Implementar endpoints para gestionar miembros
- Enviar notificaciones de invitación

**Comentario de Negocio:** La colaboración es clave para equipos de investigación y grupos de estudio.

---

#### US16 — Gestionar permisos de miembros en colecciones

| Campo | Valor |
|---|---|
| **Rol** | docente |
| **Descripción** | Como docente, quiero asignar permisos específicos (crear, editar, eliminar) a cada miembro de una colección compartida, para controlar quién puede modificar el contenido |
| **Prioridad** | Media |
| **Estimación** | 5 SP |
| **Dependencias** | US15 |

**Criterios de Aceptación:**
1. El sistema debe mostrar los permisos actuales de cada miembro.
2. El sistema debe permitir activar/desactivar permisos de crear, editar y eliminar por miembro.
3. El sistema debe aplicar los permisos en todas las operaciones de la colección.
4. El sistema debe impedir que un miembro sin permisos realice acciones no autorizadas.
5. El sistema debe notificar al miembro si sus permisos cambian.

**Tareas Técnicas:**
- Implementar verificación de permisos en endpoints de links y colecciones
- Crear UI de gestión de permisos en el modal de compartir
- Implementar middleware de autorización por colección

**Comentario de Negocio:** Esencial para entornos educativos donde el docente debe supervisar las contribuciones de los estudiantes.

---

#### US17 — Publicar colección como pública con RSS

| Campo | Valor |
|---|---|
| **Rol** | docente |
| **Descripción** | Como docente, quiero publicar una colección como pública y generar un feed RSS, para compartir mis recursos seleccionados con personas externas a la plataforma |
| **Prioridad** | Media |
| **Estimación** | 5 SP |
| **Dependencias** | US06 |

**Criterios de Aceptación:**
1. El sistema debe permitir marcar una colección como pública.
2. El sistema debe generar una URL pública accesible sin autenticación.
3. El sistema debe generar un feed RSS de la colección pública.
4. El sistema debe permitir despublicar la colección en cualquier momento.
5. El sistema debe mostrar solo los links que el propietario decida incluir.

**Tareas Técnicas:**
- Implementar endpoint `GET /api/v1/public/collections/[id]`
- Implementar feed RSS en `pages/public/collections/[id]/rss.tsx`
- Implementar toggle de visibilidad en la interfaz de colección
- Configurar cabeceras RSS y caching

**Comentario de Negocio:** Ideal para docentes que mantienen listas de lecturas y bibliografías para sus estudiantes.

---

### Épica E06: Administración y Avanzado

---

#### US18 — Gestionar API tokens

| Campo | Valor |
|---|---|
| **Rol** | investigador |
| **Descripción** | Como investigador, quiero crear y gestionar tokens de acceso a la API, para integrar Linkwarden con herramientas externas y automatizar flujos de trabajo |
| **Prioridad** | Media |
| **Estimación** | 3 SP |
| **Dependencias** | US01 |

**Criterios de Aceptación:**
1. El sistema debe permitir crear un token con un nombre descriptivo.
2. El sistema debe mostrar el token completo una sola vez al crearlo.
3. El sistema debe permitir revocar tokens individualmente.
4. El sistema debe listar todos los tokens activos del usuario.
5. El sistema debe autenticar peticiones API mediante el token en el header.
6. El sistema debe registrar la última fecha de uso de cada token.

**Tareas Técnicas:**
- Implementar endpoints CRUD para tokens (`/api/v1/tokens`)
- Implementar middleware de autenticación por token
- Crear página `pages/settings/access-tokens.tsx`
- Implementar hash seguro de tokens en BD

**Comentario de Negocio:** Los API tokens permiten integraciones y automatización, ampliando el ecosistema de la herramienta.

---

#### US19 — Importar marcadores desde otros servicios

| Campo | Valor |
|---|---|
| **Rol** | usuario promedio |
| **Descripción** | Como usuario promedio, quiero importar mis marcadores desde otros servicios (HTML, Pocket, Wallabag, Omnivore), para migrar todo mi historial a Linkwarden sin perder datos |
| **Prioridad** | Media |
| **Estimación** | 8 SP |
| **Dependencias** | US05 |

**Criterios de Aceptación:**
1. El sistema debe permitir importar desde archivo HTML de marcadores (formato estándar del navegador).
2. El sistema debe permitir importar desde Pocket (archivo HTML exportado).
3. El sistema debe permitir importar desde Wallabag (JSON).
4. El sistema debe permitir importar desde Omnivore (JSON).
5. El sistema debe procesar la importación en segundo plano para archivos grandes.
6. El sistema debe mostrar un resumen de la importación (marcadores importados, errores, duplicados).
7. El sistema debe respetar el límite máximo de archivo de 10 MB.

**Tareas Técnicas:**
- Implementar controladores de importación para cada formato
- Implementar worker de migración (`migrationWorker.ts`)
- Crear componente `ImportDropdown.tsx`
- Validar y sanitizar datos importados

**Comentario de Negocio:** La importación sin fricción desde competidores es clave para la adopción. Pocket y Wallabag son los orígenes más comunes.

---

#### US20 — Administrar usuarios (panel admin)

| Campo | Valor |
|---|---|
| **Rol** | docente |
| **Descripción** | Como docente, quiero gestionar los usuarios del sistema desde un panel de administración, para controlar quién tiene acceso y mantener el orden en la plataforma |
| **Prioridad** | Alta |
| **Estimación** | 5 SP |
| **Dependencias** | US01 |

**Criterios de Aceptación:**
1. El sistema debe listar todos los usuarios registrados con su información básica.
2. El sistema debe permitir crear nuevos usuarios manualmente.
3. El sistema debe permitir editar datos de usuarios (nombre, email, rol).
4. El sistema debe permitir eliminar cuentas de usuario.
5. El sistema debe mostrar estadísticas del worker (links procesados, errores, estado).
6. El sistema debe restringir el acceso al panel solo al administrador.

**Tareas Técnicas:**
- Implementar endpoints CRUD de usuarios para admin (`/api/v1/users`)
- Crear página `pages/admin/user-administration.tsx`
- Implementar verificación de rol admin
- Mostrar estadísticas del worker (`GET /api/v1/worker`)

**Comentario de Negocio:** El panel de administración es necesario para mantener el control sobre la instancia, especialmente en despliegues institucionales.

---

#### US21 — Gestionar suscripción y facturación

| Campo | Valor |
|---|---|
| **Rol** | docente |
| **Descripción** | Como docente, quiero gestionar la suscripción y facturación del equipo (planes, asientos, período de prueba), para decidir qué funcionalidades premium están disponibles |
| **Prioridad** | Media |
| **Estimación** | 8 SP |
| **Dependencias** | US01 |

**Criterios de Aceptación:**
1. El sistema debe mostrar el plan actual y el estado de la suscripción.
2. El sistema debe permitir iniciar el proceso de pago mediante Stripe.
3. El sistema debe manejar el período de prueba gratuito (14 días por defecto).
4. El sistema debe permitir gestionar el número de asientos (usuarios adicionales).
5. El sistema debe procesar eventos de Stripe vía webhook (creación, actualización, cancelación).
6. El sistema debe enviar un correo recordatorio antes de que termine el período de prueba.
7. El sistema debe mostrar el historial de facturación.

**Tareas Técnicas:**
- Integrar Stripe Checkout (`lib/api/paymentCheckout.ts`)
- Implementar webhook Stripe (`POST /api/v1/webhook`)
- Implementar modelo `Subscription` en Prisma
- Crear página `pages/settings/billing.tsx`
- Implementar worker de email de fin de trial

**Comentario de Negocio:** El modelo de suscripción permite sostener el desarrollo del proyecto. La gestión de asientos es clave para equipos.

---

## 4. Sprint Planning

### Criterios de Planificación

| Parámetro | Valor |
|---|---|
| Duración del Sprint | 2 semanas |
| Velocidad del equipo | ~35-40 Story Points |
| Días hábiles por Sprint | 10 |
| Equipo estimado | 4-5 desarrolladores |

---

### Sprint 1: Fundación del Sistema

**Objetivo:** Establecer la base del sistema con autenticación, registro de usuarios y la funcionalidad principal de guardar y organizar marcadores en colecciones.

| US | Título | SP | Dependencias |
|---|---|---|---|
| US01 | Registrarse en la plataforma | 3 | — |
| US02 | Iniciar sesión con SSO | 5 | US01 |
| US03 | Recuperar contraseña | 3 | US01 |
| US04 | Configurar perfil y preferencias | 5 | US01 |
| US05 | Guardar un marcador | 8 | US01 |
| US06 | Organizar marcadores en colecciones | 8 | US05 |
| **Total** | | **32** | |

**Justificación:** Este Sprint entrega el MVP completo: el usuario se registra, configura su cuenta, guarda su primer marcador y lo organiza en colecciones. Es la base funcional mínima del producto.

---

### Sprint 2: Descubrimiento y Preservación

**Objetivo:** Implementar la edición/eliminación de marcadores, etiquetado, búsqueda full-text y el archivado automático de páginas web.

| US | Título | SP | Dependencias |
|---|---|---|---|
| US07 | Editar y eliminar marcadores | 5 | US05 |
| US08 | Fijar marcadores | 3 | US05 |
| US09 | Crear y asignar etiquetas | 5 | US05 |
| US10 | Buscar marcadores | 8 | US05, US09 |
| US12 | Archivar páginas web automáticamente | 13 | US05 |
| US13 | Visualizar versiones preservadas | 5 | US12 |
| **Total** | | **39** | |

**Justificación:** Este Sprint completa la gestión de marcadores (editar, eliminar, fijar, etiquetar) e implementa el core diferencial: archivado automático con preservación en múltiples formatos.

---

### Sprint 3: Colaboración y Madurez (En curso)

**Objetivo:** Implementar funcionalidades colaborativas, importación desde otros servicios, API tokens, administración de usuarios y suscripciones.

| US | Título | SP | Dependencias |
|---|---|---|---|
| US11 | Fusionar y eliminar etiquetas | 3 | US09 |
| US14 | Subir archivos como marcadores | 5 | US05, US12 |
| US15 | Compartir colecciones con otros usuarios | 5 | US06 |
| US16 | Gestionar permisos de miembros | 5 | US15 |
| US17 | Publicar colección pública con RSS | 5 | US06 |
| US18 | Gestionar API tokens | 3 | US01 |
| US19 | Importar marcadores desde otros servicios | 8 | US05 |
| US20 | Administrar usuarios (panel admin) | 5 | US01 |
| US21 | Gestionar suscripción y facturación | 8 | US01 |
| **Total** | | **47** | |

**Justificación:** Sprint final que cierra el ciclo con colaboración entre usuarios, integración vía API, migración desde competidores, control administrativo y monetización. Las 9 US se distribuyen en equipos paralelos.

---

## 5. Sprint Backlog (Desglose en Tareas Técnicas)

### Sprint 1 — Desglose

| US | Tarea | Horas Est. |
|---|---|---|
| US01 | Diseñar esquema de base de datos para User | 4 |
| US01 | Implementar endpoint POST /api/v1/auth/register | 8 |
| US01 | Crear formulario de registro en pages/register.tsx | 6 |
| US01 | Configurar envío de email de verificación | 6 |
| US01 | Implementar validación Zod para registro | 4 |
| US01 | Agregar variable NEXT_PUBLIC_DISABLE_REGISTRATION | 2 |
| US02 | Configurar NextAuth.js con Google y GitHub providers | 8 |
| US02 | Implementar página de login con botones SSO | 6 |
| US02 | Implementar lógica de vinculación de cuentas SSO | 8 |
| US02 | Configurar proveedores adicionales (Discord, Apple, etc.) | 4 |
| US03 | Implementar endpoint forgot-password | 6 |
| US03 | Implementar endpoint reset-password | 6 |
| US03 | Crear plantilla de email con Handlebars | 4 |
| US03 | Implementar páginas forgot.tsx y reset-password.tsx | 6 |
| US04 | Implementar página de settings/account.tsx | 8 |
| US04 | Implementar página de settings/preference.tsx | 8 |
| US04 | Implementar página de settings/password.tsx | 6 |
| US04 | Implementar endpoint PUT /api/v1/users/[id]/preference | 6 |
| US04 | Implementar selector de tema con persistencia | 4 |
| US05 | Implementar endpoint POST /api/v1/links | 8 |
| US05 | Crear componente LinkCreator.tsx | 10 |
| US05 | Implementar extracción de metadatos (OG, favicon) | 6 |
| US05 | Implementar lógica anti-duplicados | 4 |
| US05 | Integrar cola de archivado con worker | 6 |
| US05 | Implementar vistas de marcadores (card, list) | 8 |
| US06 | Implementar CRUD de colecciones (endpoints) | 8 |
| US06 | Implementar modelo jerárquico con parentId | 4 |
| US06 | Crear panel lateral de navegación por colecciones | 8 |
| US06 | Implementar drag & drop para mover marcadores | 10 |
| US06 | Implementar selector de íconos y colores | 6 |
| US06 | Crear sub-colecciones (UI) | 6 |
| **Total Sprint 1** | | **192** |

### Sprint 2 — Desglose

| US | Tarea | Horas Est. |
|---|---|---|
| US07 | Implementar endpoint PUT /api/v1/links/[id] | 6 |
| US07 | Implementar endpoint DELETE (individual y bulk) | 8 |
| US07 | Implementar selección múltiple en UI | 6 |
| US07 | Implementar confirmación y deshacer eliminación | 6 |
| US08 | Implementar endpoint pin/unpin | 4 |
| US08 | Crear componente LinkPin.tsx | 4 |
| US08 | Agregar sección "Pinned Links" al dashboard | 4 |
| US09 | Implementar CRUD de tags (endpoints) | 8 |
| US09 | Crear componente TagSelector con autocompletado | 8 |
| US09 | Implementar filtro por etiqueta en queries | 6 |
| US09 | Agregar opciones de archivado por etiqueta | 4 |
| US10 | Configurar Meilisearch e indexación | 10 |
| US10 | Implementar endpoint GET /api/v1/search | 8 |
| US10 | Implementar SearchBar con búsqueda en tiempo real | 8 |
| US10 | Implementar página pages/search.tsx | 8 |
| US12 | Configurar worker con Playwright | 10 |
| US12 | Implementar captura de screenshot | 10 |
| US12 | Implementar generación de PDF | 8 |
| US12 | Implementar extracción Readable (Mozilla Readability) | 8 |
| US12 | Implementar archivo HTML monolito (SingleFile) | 10 |
| US12 | Configurar cola de procesamiento (lotes de 5) | 6 |
| US12 | Integrar envío a Wayback Machine | 6 |
| US13 | Crear visualización de screenshot | 6 |
| US13 | Crear visualización de PDF | 6 |
| US13 | Crear ReadableView.tsx | 8 |
| US13 | Crear PreservationNavbar con cambio de formatos | 6 |
| **Total Sprint 2** | | **192** |

### Sprint 3 — Desglose (En curso)

| US | Tarea | Horas Est. |
|---|---|---|
| US11 | Implementar endpoint POST /api/v1/tags/merge | 6 |
| US11 | Implementar UI de fusión de tags | 6 |
| US14 | Implementar endpoint POST /api/v1/archives | 8 |
| US14 | Configurar almacenamiento local/S3 | 6 |
| US14 | Mostrar vista previa de archivos subidos | 6 |
| US15 | Implementar modelo UsersAndCollections | 4 |
| US15 | Crear modal de invitación de miembros | 8 |
| US15 | Implementar endpoints de gestión de miembros | 8 |
| US15 | Enviar notificaciones de invitación | 4 |
| US16 | Implementar middleware de permisos | 8 |
| US16 | Crear UI de asignación de permisos | 8 |
| US16 | Aplicar permisos en endpoints de links | 6 |
| US17 | Implementar toggle de visibilidad pública | 4 |
| US17 | Implementar vista de colección pública | 8 |
| US17 | Implementar feed RSS público | 8 |
| US18 | Implementar CRUD de tokens (endpoints) | 8 |
| US18 | Implementar middleware de autenticación por token | 6 |
| US18 | Crear página settings/access-tokens.tsx | 6 |
| US19 | Implementar importación desde HTML | 10 |
| US19 | Implementar importación desde Pocket | 8 |
| US19 | Implementar importación desde Wallabag | 8 |
| US19 | Implementar importación desde Omnivore | 8 |
| US19 | Implementar worker de migración | 6 |
| US19 | Crear componente ImportDropdown.tsx | 6 |
| US20 | Implementar endpoints CRUD de usuarios (admin) | 8 |
| US20 | Crear página admin/user-administration.tsx | 10 |
| US20 | Mostrar estadísticas del worker | 6 |
| US21 | Integrar Stripe Checkout | 10 |
| US21 | Implementar webhook Stripe | 10 |
| US21 | Crear página settings/billing.tsx | 8 |
| US21 | Implementar worker de email de fin de trial | 6 |
| **Total Sprint 3** | | **236** |

---

## 6. Tableros Kanban

### Sprint 1 — COMPLETADO ✅ (32/32 SP)

| US | Estado |
|---|---|
| US01 — Registrarse en la plataforma | ✅ Done |
| US02 — Iniciar sesión con SSO | ✅ Done |
| US03 — Recuperar contraseña | ✅ Done |
| US04 — Configurar perfil y preferencias | ✅ Done |
| US05 — Guardar un marcador | ✅ Done |
| US06 — Organizar marcadores en colecciones | ✅ Done |

### Sprint 2 — COMPLETADO ✅ (39/39 SP)

| US | Estado |
|---|---|
| US07 — Editar y eliminar marcadores | ✅ Done |
| US08 — Fijar marcadores | ✅ Done |
| US09 — Crear y asignar etiquetas | ✅ Done |
| US10 — Buscar marcadores | ✅ Done |
| US12 — Archivar páginas web automáticamente | ✅ Done |
| US13 — Visualizar versiones preservadas | ✅ Done |

### Sprint 3 — EN CURSO 🚧 (0/47 SP)

| US | SP | Estado |
|---|---|---|
| US11 — Fusionar y eliminar etiquetas | 3 | 📋 To Do |
| US14 — Subir archivos como marcadores | 5 | 📋 To Do |
| US15 — Compartir colecciones con otros usuarios | 5 | 📋 To Do |
| US16 — Gestionar permisos de miembros | 5 | 📋 To Do |
| US17 — Publicar colección pública con RSS | 5 | 📋 To Do |
| US18 — Gestionar API tokens | 3 | 📋 To Do |
| US19 — Importar marcadores desde otros servicios | 8 | 📋 To Do |
| US20 — Administrar usuarios (panel admin) | 5 | 📋 To Do |
| US21 — Gestionar suscripción y facturación | 8 | 📋 To Do |

---

## 7. Simulación de Daily Scrums

### Sprint 1 — Días 1 al 10

**Día 1 — Lunes (Inicio Sprint 1)**
> **Ayer:** Sprint Planning. Definimos el objetivo del Sprint 1 y seleccionamos US01-US06 (32 SP).
> **Hoy:** Comenzar con el modelo User y endpoint de registro (US01). Configurar entorno Docker.
> **Bloqueos:** Ninguno.

**Día 2 — Martes**
> **Ayer:** Completé el modelo User en Prisma y el endpoint POST /api/v1/auth/register.
> **Hoy:** Terminar validación Zod y empezar formulario de registro.
> **Bloqueos:** Pendientes estilos del formulario con el diseñador.

**Día 3 — Miércoles**
> **Ayer:** Formulario de registro completo. Empecé NextAuth.js (US02).
> **Hoy:** Configurar providers Google y GitHub. Integrar página de login.
> **Bloqueos:** Ninguno.

**Día 4 — Jueves**
> **Ayer:** Google y GitHub como providers SSO funcionando.
> **Hoy:** Vincular cuentas SSO con locales. Empezar US03 (forgot password).
> **Bloqueos:** Callback de GitHub requiere URL pública. Usar ngrok.

**Día 5 — Viernes**
> **Ayer:** Vinculación SSO completa. Endpoints forgot/reset password listos.
> **Hoy:** Plantillas de email con Handlebars y páginas forgot/reset.
> **Bloqueos:** Nodemailer necesita SMTP. Usar Mailhog.

**Día 6 — Lunes (Semana 2)**
> **Ayer:** Completé US03. Empecé US04 (Configurar perfil).
> **Hoy:** Implementar settings/account.tsx y settings/preference.tsx.
> **Bloqueos:** Ninguno.

**Día 7 — Martes**
> **Ayer:** Avancé account.tsx (nombre, username, avatar).
> **Hoy:** Terminar preference.tsx (tema, idioma, defaults archivado).
> **Bloqueos:** i18n requiere configurar traducciones base.

**Día 8 — Miércoles**
> **Ayer:** Completé US04. Empecé US05 (Guardar marcador).
> **Hoy:** Endpoint POST /api/v1/links y componente LinkCreator.
> **Bloqueos:** Ninguno.

**Día 9 — Jueves**
> **Ayer:** Endpoint de creación y LinkCreator listos.
> **Hoy:** Extracción de metadatos (OG, favicon) y anti-duplicados.
> **Bloqueos:** Timeout para URLs lentas en extracción OG.

**Día 10 — Viernes (Fin Sprint 1)**
> **Ayer:** Metadatos y anti-duplicados listos. Empecé vistas de marcadores.
> **Hoy:** Terminar vistas. Empezar US06 (Colecciones). Preparar demo.
> **Bloqueos:** Playwright requiere instalar dependencias del navegador en Docker.

---

### Sprint 2 — Días 11 al 20

**Día 11 — Lunes (Inicio Sprint 2)**
> **Ayer:** Sprint Review y Retrospective del Sprint 1. 32/32 SP completados.
> **Hoy:** Sprint Planning Sprint 2. Seleccionamos US07-US10 + US12-US13 (39 SP).
> **Bloqueos:** Ninguno.

**Día 12 — Martes**
> **Ayer:** Sprint Planning completado.
> **Hoy:** Implementar PUT/DELETE de links (US07). Arrancar CRUD de tags (US09).
> **Bloqueos:** Ninguno.

**Día 13 — Miércoles**
> **Ayer:** Endpoints de edición/eliminación listos. CRUD de tags avanzando.
> **Hoy:** Terminar tags y empezar US08 (Fijar marcadores).
> **Bloqueos:** Ninguno.

**Día 14 — Jueves**
> **Ayer:** Tags completado. Pin/unpin funcionando (US08).
> **Hoy:** Configurar Meilisearch y empezar búsqueda (US10).
> **Bloqueos:** Meilisearch necesita configuración en docker-compose.

**Día 15 — Viernes**
> **Ayer:** Meilisearch configurado e indexando.
> **Hoy:** Implementar endpoint GET /api/v1/search y SearchBar.
> **Bloqueos:** Índice de búsqueda debe sincronizarse con worker.

**Día 16 — Lunes (Semana 2)**
> **Ayer:** Búsqueda funcionando con Meilisearch. Filtros por colección y tags.
> **Hoy:** Empezar US12 (Archivar páginas con Playwright).
> **Bloqueos:** Playwright necesita chromium instalado.

**Día 17 — Martes**
> **Ayer:** Worker de archivado con Playwright configurado.
> **Hoy:** Implementar captura de screenshot y generación de PDF.
> **Bloqueos:** Consumo de memoria de Playwright en Docker.

**Día 18 — Miércoles**
> **Ayer:** Screenshot y PDF funcionando. Readable View con Mozilla Readability.
> **Hoy:** HTML monolito y cola de procesamiento en lotes.
> **Bloqueos:** Ninguno.

**Día 19 — Jueves**
> **Ayer:** Archivado completo con los 4 formatos. Cola de lotes funcionando.
> **Hoy:** US13 (Visualizar preservados). Crear Preservation components.
> **Bloqueos:** Ninguno.

**Día 20 — Viernes (Fin Sprint 2)**
> **Ayer:** Visualización de preservados completa. Sprint Review preparado.
> **Hoy:** Sprint Review Sprint 2. 39/39 SP completados.
> **Bloqueos:** Ninguno.

---

### Sprint 3 — Día 21 (Hoy — Inicio)

**Día 21 — Lunes (Inicio Sprint 3)**
> **Ayer:** Sprint Review y Retrospective del Sprint 2. 39/39 SP completados. Demostramos archivado automático funcionando.
> **Hoy:** Sprint Planning Sprint 3. Seleccionamos US11 + US14-US21 (47 SP).
> **Bloqueos:** Ninguno. El equipo está alineado para arrancar el sprint final.

---

## 8. Sprint Review — Sprint 1

### Historias Completadas

| US | Título | SP | Estado |
|---|---|---|---|
| US01 | Registrarse en la plataforma | 3 | ✅ Completado |
| US02 | Iniciar sesión con SSO | 5 | ✅ Completado |
| US03 | Recuperar contraseña | 3 | ✅ Completado |
| US04 | Configurar perfil y preferencias | 5 | ✅ Completado |
| US05 | Guardar un marcador | 8 | ✅ Completado |
| US06 | Organizar marcadores en colecciones | 8 | ✅ Completado |
| **Total** | | **32/32** | **100%** |

### Incremento Entregado

- Sistema de autenticación completo: registro con email, login con SSO (Google, GitHub, Discord, Apple), recuperación de contraseña y verificación de email.
- Configuración de perfil: nombre, avatar, tema (oscuro/claro/auto), idioma, defaults de archivado.
- Funcionalidad principal de guardar marcadores: creación con extracción automática de metadatos, selección de colección, asignación de etiquetas y archivado automático en background.
- Colecciones con jerarquía (sub-colecciones), íconos y colores personalizables.
- Vistas de marcadores en formato card y list.

### Velocidad del Equipo

- Story Points planificados: 32
- Story Points completados: 32
- Velocidad: **32 SP** (16 SP/semana)

### Lecciones Técnicas

- Playwright requiere recursos significativos de memoria en Docker. Aumentar el límite a 2 GB.
- El enlace de verificación de email debe tener una ventana de expiración más amplia (2 horas en lugar de 1).
- La extracción de metadatos necesita timeouts configurables para URLs lentas.

---

## 9. Sprint Review — Sprint 2

### Historias Completadas

| US | Título | SP | Estado |
|---|---|---|---|
| US07 | Editar y eliminar marcadores | 5 | ✅ Completado |
| US08 | Fijar marcadores | 3 | ✅ Completado |
| US09 | Crear y asignar etiquetas | 5 | ✅ Completado |
| US10 | Buscar marcadores | 8 | ✅ Completado |
| US12 | Archivar páginas web automáticamente | 13 | ✅ Completado |
| US13 | Visualizar versiones preservadas | 5 | ✅ Completado |
| **Total** | | **39/39** | **100%** |

### Incremento Entregado

- Edición y eliminación de marcadores (individual y masivo) con confirmación y deshacer.
- Sistema de fijación de marcadores con sección dedicada en el dashboard.
- Etiquetado completo: creación, asignación, autocompletado y filtro por etiqueta.
- Búsqueda full-text con Meilisearch: resultados en tiempo real con filtros por colección y etiqueta.
- Archivado automático en 4 formatos: screenshot, PDF, readable (Mozilla Readability) y HTML monolito.
- Cola de procesamiento en background con lotes de 5 links.
- Visualización de versiones preservadas con cambio entre formatos.
- Integración con Wayback Machine.

### Velocidad del Equipo

- Story Points planificados: 39
- Story Points completados: 39
- Velocidad: **39 SP** (~19.5 SP/semana)

---

## 10. Sprint Retrospective — Sprint 1 y 2

### Qué salió bien 🌟

1. **Planificación realista:** 100% de completitud en ambos Sprints. Las estimaciones fueron precisas.
2. **Arquitectura desacoplada:** Separar worker de archivado del frontend permitió desarrollo paralelo.
3. **Sistema de búsqueda:** Meilisearch ofreció resultados en milisegundos desde la primera integración.
4. **Preservación completa:** Los 4 formatos de archivado funcionan correctamente con cola de procesamiento.
5. **MVP sólido:** El producto ya cubre el ciclo completo: guardar → organizar → buscar → preservar.

### Qué se puede mejorar 🔧

1. **Cobertura de pruebas:** Faltan tests unitarios en los controladores. Se agregaron al DoD pero no se cumplió completamente.
2. **Documentación de API:** Los endpoints no tienen documentación OpenAPI. Difiero a Sprint 3.
3. **Monitoreo del worker:** La cola de archivado carece de métricas y logs estructurados.
4. **Rendimiento en listas grandes:** La carga de colecciones con +1000 links muestra latencia. Optimizar con paginación por cursor.

### Acciones de Mejora 📋

| Acción | Responsable | Sprint |
|---|---|---|
| Agregar tests unitarios a controladores | Equipo | Sprint 3 |
| Documentar endpoints con OpenAPI | Dev 1 | Sprint 3 |
| Implementar logging estructurado en worker | Dev 2 | Sprint 3 |
| Optimizar paginación por cursor en listas | Dev 3 | Sprint 3 |

---

## 11. Resumen Final

| Métrica | Valor |
|---|---|
| Total de Épicas | 6 (E01–E06) |
| Total de Historias de Usuario | 21 (US01–US21) |
| Total de Story Points | 118 SP |
| Sprints planificados | 3 (× 2 semanas) |
| SP completados (Sprints 1 y 2) | 71 |
| SP planificados (Sprint 3, en curso) | 47 |
| Velocidad promedio | ~35.5 SP/Sprint |
| Duración total estimada | 6 semanas (~1.5 meses) |
| Funcionalidades reales cubiertas | 47 |
| Modelos de datos analizados | 14 |
| Endpoints API identificados | 45+ |
| Roles de usuario | 5 (Visitante, Registrado, Miembro, Admin, Suscriptor) |
