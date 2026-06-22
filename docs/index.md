# LINKWARDEN

Plataforma open source para guardar, organizar y preservar enlaces, artículos y recursos digitales de manera colaborativa.

---

## Grupo A6

| # | Integrante |
|---|---|
| 1 | BRAVO ARREDONDO CRISTHIAN MATÍAS |
| 2 | CASTILLO LAZO RODRIGO ZARÚN |
| 3 | CHOQUE QUISPE EDUARDO JHOSUE |
| 4 | LUQUE GUEVARA FERNANDO GERSON |
| 5 | SUBIA HUAICANE EDSON FABRICIO |

---

## Roles del Equipo

Distribución de responsabilidades dentro del equipo Scrum para el desarrollo y documentación del proyecto Linkwarden.

- **Scrum Master:** Coordina las actividades del equipo, gestiona el tablero Kanban en GitHub Projects y asegura la comunicación efectiva entre los miembros.
- **Product Owner:** Encargado de representar los intereses de los stakeholders y gestionar el valor del producto durante el desarrollo.
- **Ingeniero DevOps:** Responsable de la configuración del pipeline CI/CD usando GitHub Actions, la gestión de Docker Compose y el despliegue automático hacia entornos de staging.
- **Arquitecto de Software / Analista:** Responsable de comprender la arquitectura del sistema, analizar técnicamente el código y documentar decisiones técnicas en los Issues del proyecto.
- **Desarrollador / QA:** Encargado de aplicar mejoras o refactorizaciones, ejecutar pruebas y asegurar la calidad y estabilidad del producto.
- **Documentador Técnico:** Responsable de mantener GitHub Pages, garantizar el cumplimiento del formato IEEE y publicar los avances técnicos del proyecto.

---

## Objetivo de Linkwarden

Linkwarden permite almacenar, categorizar y preservar enlaces web importantes mediante colecciones compartidas y archivado automático. Su objetivo es evitar la pérdida de información digital y facilitar la gestión colaborativa de recursos.

[Ver Repositorio Oficial](https://github.com/linkwarden/linkwarden)

---

## Stack Tecnológico

| Capa | Tecnologías |
|---|---|
| **Frontend Web** | Next.js 15 + React 19 + TailwindCSS 3 + DaisyUI |
| **Frontend Mobile** | React Native 0.81 + Expo SDK 54 + NativeWind |
| **Backend / API** | Next.js API Routes + NextAuth.js + Stripe |
| **Base de Datos** | PostgreSQL 16 + Prisma 6 ORM |
| **Búsqueda** | MeiliSearch + fuse.js |
| **Estado** | TanStack React Query + Zustand |
| **Worker / Archiving** | tsx + Playwright + Mozilla Readability |
| **Infraestructura** | Docker + Docker Compose |
| **AI / ML** | Vercel AI SDK + Ollama + OpenAI/Anthropic |

---

## Características de Linkwarden

Linkwarden proporciona herramientas modernas para gestionar y preservar contenido web de forma segura y organizada.

- **Gestión de Enlaces:** Guardado y organización de enlaces mediante etiquetas, colecciones y filtros personalizados.
- **Archivado Automático:** Conservación de páginas web mediante snapshots para evitar pérdida de contenido online.
- **Colaboración:** Compartición de colecciones y trabajo colaborativo entre equipos y usuarios.
- **Importación:** Importación de marcadores desde navegadores y otras plataformas de gestión de enlaces.
- **Búsqueda Avanzada:** Motor de búsqueda integrado para encontrar enlaces rápidamente dentro de grandes colecciones.
- **Autenticación:** Soporte para autenticación segura y acceso multiusuario en entornos self-hosted.

---

## Historias de Usuario

### Sprint 1

**OBJETIVO:** Establecer las bases técnicas y organizativas mediante el mapeo arquitectónico, la configuración del entorno local reproducible con Docker y el ordenamiento del backlog y la documentación en GitHub.

---

#### US01: Mapear la arquitectura de Linkwarden

| Campo | Valor |
|---|---|
| **Prioridad** | Alta |
| **Estimación** | 8 |
| **Dependencias** | Ninguna |

**Descripción:**
> Como investigador,
> quiero mapear la arquitectura de Linkwarden,
> para comprender el flujo de datos y documentar la estructura del sistema.

**Criterios de Aceptación:**
1. El sistema debe documentar la arquitectura general (frontend, backend, base de datos, worker).
2. El sistema debe describir el flujo de datos entre los componentes principales.
3. El sistema debe identificar las dependencias externas y los servicios requeridos.
4. El sistema debe incluir un diagrama arquitectónico que represente visualmente los componentes.
5. El sistema debe especificar los protocolos de comunicación entre módulos.

**Tareas Técnicas:**
1. Revisar la documentación técnica existente del proyecto.
2. Identificar los componentes principales y sus responsabilidades.
3. Elaborar un diagrama de arquitectura con las relaciones entre componentes.
4. Documentar las decisiones arquitectónicas clave.
5. Validar el diagrama con el equipo de desarrollo.

**Comentario de Negocio:** Esta funcionalidad permite al equipo de operaciones comprender la estructura del sistema, facilitando el mantenimiento, la resolución de incidentes y la planificación de mejoras futuras.

---

#### US02: Configurar el entorno Docker

| Campo | Valor |
|---|---|
| **Prioridad** | Alta |
| **Estimación** | 5 |
| **Dependencias** | US01 - Mapear la arquitectura de Linkwarden |

**Descripción:**
> Como investigador,
> quiero configurar el entorno Docker de Linkwarden,
> para garantizar un ambiente de estudio reproducible y consistente.

**Criterios de Aceptación:**
1. El sistema debe proporcionar un archivo docker-compose.yml con todos los servicios necesarios.
2. Los contenedores deben iniciar correctamente sin errores críticos.
3. El sistema debe ser accesible desde el navegador en localhost después de iniciar los contenedores.
4. El sistema debe incluir las variables de entorno documentadas en un archivo .env.sample.
5. El sistema debe persistir los datos de la base de datos mediante volúmenes de Docker.

**Tareas Técnicas:**
1. Revisar y actualizar el archivo docker-compose.yml del proyecto.
2. Configurar los volúmenes necesarios para PostgreSQL y MeiliSearch.
3. Verificar que las variables de entorno requeridas estén documentadas.
4. Probar el inicio completo del stack con docker compose up.
5. Documentar el proceso de instalación y configuración paso a paso.

**Comentario de Negocio:** Un entorno Docker correctamente configurado reduce el tiempo de incorporación de nuevos miembros al equipo y elimina los problemas de inconsistencia entre entornos de desarrollo.

---

#### US03: Registrar el Product Backlog

| Campo | Valor |
|---|---|
| **Prioridad** | Alta |
| **Estimación** | 3 |
| **Dependencias** | Ninguna |

**Descripción:**
> Como docente,
> quiero registrar el Product Backlog en GitHub Projects,
> para organizar y priorizar el trabajo del equipo de desarrollo.

**Criterios de Aceptación:**
1. El sistema debe mostrar el tablero con todas las historias de usuario registradas.
2. Cada historia debe tener un identificador único, título y descripción.
3. El sistema debe permitir asignar prioridades a cada historia.
4. El sistema debe permitir asignar responsables a cada tarea.
5. El tablero debe organizar las tareas en Sprints con fechas definidas.

**Tareas Técnicas:**
1. Crear el GitHub Project en el repositorio del proyecto.
2. Registrar cada historia de usuario siguiendo el formato establecido.
3. Asignar prioridades y responsables a cada tarea.
4. Configurar los Sprints con fechas de inicio y fin.
5. Validar que el tablero refleje correctamente el plan de trabajo.

**Comentario de Negocio:** Un backlog bien estructurado permite al equipo tener visibilidad completa del trabajo pendiente, facilita la toma de decisiones sobre prioridades y asegura que todos los miembros del equipo conozcan sus responsabilidades.

---

#### US04: Validar la ejecución local de Linkwarden

| Campo | Valor |
|---|---|
| **Prioridad** | Alta |
| **Estimación** | 3 |
| **Dependencias** | US02 - Configurar el entorno Docker |

**Descripción:**
> Como usuario promedio,
> quiero validar la ejecución local de Linkwarden,
> para asegurarme de que la aplicación funciona correctamente en mi equipo.

**Criterios de Aceptación:**
1. El sistema debe cargar la interfaz web sin errores en el navegador.
2. El sistema debe permitir iniciar sesión con credenciales válidas.
3. El sistema debe permitir crear una nueva colección correctamente.
4. El sistema debe permitir agregar un enlace a una colección.
5. El sistema debe mostrar los logs de los contenedores sin errores críticos.

**Tareas Técnicas:**
1. Iniciar los contenedores con docker compose up.
2. Acceder a la aplicación en http://localhost:3000.
3. Crear un usuario de prueba e iniciar sesión.
4. Probar las funcionalidades básicas: crear colección, agregar link.
5. Verificar los logs de los contenedores en busca de errores.

**Comentario de Negocio:** Esta validación garantiza que el entorno de desarrollo está correctamente configurado y que la aplicación base funciona antes de realizar cualquier modificación.

---

#### US05: Publicar documentación técnica en GitHub Pages

| Campo | Valor |
|---|---|
| **Prioridad** | Media |
| **Estimación** | 5 |
| **Dependencias** | US03 - Registrar el Product Backlog |

**Descripción:**
> Como usuario promedio,
> quiero acceder a la documentación técnica publicada en GitHub Pages,
> para conocer el funcionamiento, la arquitectura y la configuración del sistema.

**Criterios de Aceptación:**
1. El sistema debe habilitar GitHub Pages en el repositorio del proyecto.
2. El sitio debe cargar correctamente una página principal con la introducción del proyecto.
3. El sitio debe incluir secciones de navegación entre los distintos temas.
4. El sistema debe mostrar el stack tecnológico utilizado.
5. El sitio debe ser accesible públicamente mediante la URL de GitHub Pages.

**Tareas Técnicas:**
1. Habilitar GitHub Pages en la configuración del repositorio.
2. Crear la página index.html con la estructura de documentación.
3. Incluir secciones de integrantes, roles, stack tecnológico y cronograma.
4. Verificar que los enlaces de navegación funcionen correctamente.
5. Publicar la página y validar el acceso público.

**Comentario de Negocio:** La documentación técnica accesible permite a cualquier miembro del equipo o stakeholder externo comprender el proyecto rápidamente, reduciendo las barreras de entrada.

---

### Sprint 2

**OBJETIVO:** Asegurar la automatización y calidad del desarrollo implementando un pipeline CI, estandarizando las revisiones de código y desplegando una versión demo funcional en el entorno de Staging.

---

#### US06: Implementar pipeline de integración continua

| Campo | Valor |
|---|---|
| **Prioridad** | Alta |
| **Estimación** | 8 |
| **Dependencias** | US02 - Configurar el entorno Docker |

**Descripción:**
> Como investigador,
> quiero implementar un pipeline de integración continua (CI),
> para automatizar la compilación y ejecución de pruebas del proyecto.

**Criterios de Aceptación:**
1. El pipeline debe ejecutarse automáticamente al hacer push a la rama principal.
2. El sistema debe compilar el proyecto sin errores durante la ejecución del pipeline.
3. El sistema debe ejecutar las pruebas unitarias definidas en el proyecto.
4. El pipeline debe notificar el resultado (éxito o fallo) al equipo.
5. El sistema debe generar un artefacto del build al finalizar correctamente.

**Tareas Técnicas:**
1. Crear el archivo YAML de GitHub Actions en .github/workflows.
2. Configurar los pasos de checkout, instalación de dependencias y build.
3. Agregar la ejecución de pruebas unitarias con Vitest.
4. Configurar notificaciones de resultado del pipeline.
5. Probar el pipeline con un push y verificar su correcto funcionamiento.

**Comentario de Negocio:** Un pipeline CI automatizado reduce el riesgo de errores en producción al validar cada cambio de código y acelera el ciclo de retroalimentación para los desarrolladores.

---

#### US07: Desplegar demo funcional en Staging

| Campo | Valor |
|---|---|
| **Prioridad** | Alta |
| **Estimación** | 5 |
| **Dependencias** | US06 - Implementar pipeline CI |

**Descripción:**
> Como docente,
> quiero acceder a una demo funcional en un entorno de staging,
> para validar el producto antes de su entrega final.

**Criterios de Aceptación:**
1. El sistema debe estar desplegado en un entorno de staging accesible desde internet.
2. El sistema debe permitir navegar por las funcionalidades principales sin errores.
3. El sistema debe contener datos de prueba representativos para la demostración.
4. El sistema debe responder con tiempos de carga aceptables.
5. El sistema debe mostrar un health check operativo al acceder a la URL.

**Tareas Técnicas:**
1. Configurar un entorno de staging con Docker Compose.
2. Desplegar la versión más reciente del código en el entorno de staging.
3. Poblar la base de datos con datos de prueba.
4. Verificar que todas las funcionalidades core operen correctamente.
5. Compartir la URL con el equipo para la revisión.

**Comentario de Negocio:** Una demo en staging permite a los stakeholders validar el producto en un entorno realista antes del lanzamiento, reduciendo el riesgo de defectos en producción.

---

#### US08: Registrar decisiones técnicas en Issues

| Campo | Valor |
|---|---|
| **Prioridad** | Media |
| **Estimación** | 5 |
| **Dependencias** | US03 - Registrar el Product Backlog |

**Descripción:**
> Como investigador,
> quiero consultar las decisiones técnicas registradas en Issues,
> para mantener la trazabilidad del proyecto y justificar las elecciones tecnológicas.

**Criterios de Aceptación:**
1. Cada decisión técnica importante debe tener un Issue asociado en el repositorio.
2. El Issue debe incluir el contexto y la justificación de la decisión.
3. El sistema debe permitir etiquetar los Issues con categorías descriptivas.
4. Los Issues deben poder ser referenciados desde la documentación del proyecto.
5. El sistema debe mantener un registro histórico de las decisiones tomadas.

**Tareas Técnicas:**
1. Crear Issues en GitHub para cada decisión técnica relevante.
2. Documentar el contexto, opciones consideradas y justificación.
3. Asignar etiquetas descriptivas a cada Issue.
4. Vincular los Issues con la documentación del proyecto.
5. Revisar y actualizar los Issues según sea necesario.

**Comentario de Negocio:** La trazabilidad de decisiones técnicas es fundamental para la gobernanza del proyecto, permitiendo a futuros integrantes comprender el porqué de cada elección tecnológica.

---

#### US09: Establecer revisión de código colaborativa

| Campo | Valor |
|---|---|
| **Prioridad** | Alta |
| **Estimación** | 5 |
| **Dependencias** | US06 - Implementar pipeline CI |

**Descripción:**
> Como investigador,
> quiero establecer un proceso de revisión de código colaborativa,
> para asegurar la calidad del software y el cumplimiento de estándares.

**Criterios de Aceptación:**
1. El sistema debe contar con una guía de estilo de código documentada.
2. Cada Pull Request debe ser revisado por al menos un miembro del equipo.
3. El sistema debe verificar automáticamente que no hay errores de linting.
4. El sistema debe asegurar que el código sigue las convenciones del proyecto.
5. El proceso de revisión debe estar documentado y accesible al equipo.

**Tareas Técnicas:**
1. Documentar la guía de estilo y estándares de codificación.
2. Configurar ESLint y Prettier para validación automática.
3. Definir el flujo de revisión de Pull Requests.
4. Realizar revisiones de código a los Pull Requests existentes.
5. Documentar el proceso de revisión acordado.

**Comentario de Negocio:** Las revisiones de código colaborativas mejoran la calidad del software, facilitan la transferencia de conocimiento y reducen la probabilidad de defectos en producción.

---

#### US10: Actualizar documentación técnica en GitHub Pages

| Campo | Valor |
|---|---|
| **Prioridad** | Media |
| **Estimación** | 3 |
| **Dependencias** | US05 - Publicar documentación técnica en GitHub Pages |

**Descripción:**
> Como usuario promedio,
> quiero consultar la documentación técnica actualizada en GitHub Pages,
> para mantenerme informado sobre los avances del proyecto.

**Criterios de Aceptación:**
1. El sitio debe incluir la documentación generada en los Sprints 1 y 2.
2. El sistema debe mantener la estructura y navegación existente del sitio.
3. Los nuevos contenidos deben estar correctamente enlazados desde la navegación.
4. El sistema debe verificar que no hay enlaces rotos en la documentación.
5. La documentación debe estar disponible en formato Markdown y HTML.

**Tareas Técnicas:**
1. Recopilar la documentación generada durante los Sprints 1 y 2.
2. Actualizar el archivo index.html con los nuevos contenidos.
3. Crear la versión en formato Markdown de la documentación.
4. Verificar la correcta navegación y enlaces del sitio.
5. Publicar los cambios en la rama de documentación del repositorio.

**Comentario de Negocio:** Mantener la documentación actualizada es esencial para que todos los involucrados en el proyecto tengan acceso a la información más reciente.

---

### Sprint 3 (Próximo)

**OBJETIVO:** Implementar mejoras finales, automatizar el despliegue continuo (CD) y redactar el artículo técnico final.

---

#### US11: Implementar mejoras finales

| Campo | Valor |
|---|---|
| **Prioridad** | Alta |
| **Estimación** | 8 |
| **Dependencias** | US09 - Establecer revisión de código colaborativa |

**Descripción:**
> Como usuario promedio,
> quiero que se implementen mejoras finales en la aplicación,
> para disfrutar de un producto estable, completo y listo para su uso.

**Criterios de Aceptación:**
1. El sistema debe resolver los bugs reportados durante los Sprints anteriores.
2. El sistema debe pasar todas las pruebas establecidas sin errores.
3. El sistema debe mantener la estabilidad después de las modificaciones.
4. El sistema debe contar con las funcionalidades core operativas.
5. El sistema debe ser revisado antes del cierre del Sprint.

**Tareas Técnicas:**
1. Revisar el backlog y priorizar los issues pendientes.
2. Corregir los bugs reportados por el equipo.
3. Ejecutar las pruebas unitarias y de integración.
4. Realizar una última revisión de código antes del cierre.
5. Documentar los cambios realizados.

**Comentario de Negocio:** Las mejoras finales aseguran que el producto entregado cumple con los estándares de calidad esperados, corrigiendo los últimos defectos.

---

#### US12: Automatizar despliegue continuo

| Campo | Valor |
|---|---|
| **Prioridad** | Alta |
| **Estimación** | 8 |
| **Dependencias** | US06 - Implementar pipeline CI |

**Descripción:**
> Como investigador,
> quiero automatizar el despliegue continuo (CD) del proyecto,
> para garantizar entregas frecuentes y confiables del producto.

**Criterios de Aceptación:**
1. El pipeline de CD debe ejecutarse después de que el pipeline de CI pase exitosamente.
2. El sistema debe desplegar automáticamente en el entorno de producción o staging.
3. El sistema debe ejecutar un health check para verificar el despliegue.
4. El sistema debe notificar al equipo en caso de fallo del despliegue.
5. El proceso de despliegue debe estar documentado con las variables necesarias.

**Tareas Técnicas:**
1. Extender el workflow de GitHub Actions para incluir despliegue.
2. Configurar las credenciales y variables de entorno para el entorno destino.
3. Implementar un health check post-despliegue.
4. Configurar notificaciones de éxito o fallo del despliegue.
5. Documentar el proceso de despliegue y las variables requeridas.

**Comentario de Negocio:** La automatización del despliegue continuo reduce el tiempo entre el desarrollo y la producción, minimiza errores humanos y permite entregar valor al usuario final más rápido.

---

#### US13: Redactar artículo técnico final en GitHub Pages

| Campo | Valor |
|---|---|
| **Prioridad** | Media |
| **Estimación** | 13 |
| **Dependencias** | US10 - Actualizar documentación técnica en GitHub Pages |

**Descripción:**
> Como usuario promedio,
> quiero acceder al artículo técnico final en GitHub Pages,
> para conocer el proceso completo del proyecto y las decisiones tomadas.

**Criterios de Aceptación:**
1. El sitio debe incluir una introducción al proyecto y sus objetivos.
2. El sitio debe documentar el stack tecnológico y su justificación.
3. El sitio debe describir la arquitectura del sistema.
4. El sitio debe incluir una guía de instalación y configuración paso a paso.
5. El sitio debe presentar las conclusiones y lecciones aprendidas del proyecto.

**Tareas Técnicas:**
1. Redactar la introducción y objetivos del proyecto.
2. Documentar el stack tecnológico con la justificación de cada herramienta.
3. Describir la arquitectura del sistema con diagramas.
4. Elaborar la guía de instalación y configuración.
5. Publicar el artículo completo en GitHub Pages y validar su acceso.

**Comentario de Negocio:** El artículo técnico final constituye la principal entrega de documentación del proyecto, permitiendo a la comunidad y futuros mantenedores comprender el trabajo realizado.

---

## Project Board

[Ver Project Board en GitHub](https://github.com/users/luqueGG/projects/7)

---

*Linkwarden - Documentación Sprints 1 y 2 | Grupo A6*
