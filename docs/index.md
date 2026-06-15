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

## Cronograma

### Sprint 1

**OBJETIVO:** Establecer las bases técnicas y organizativas mediante el mapeo arquitectónico, la configuración del entorno local reproducible con Docker y el ordenamiento del backlog y la documentación en GitHub.

| # | Issue | Etiqueta |
|---|---|---|
| **US01** | **Mapear la arquitectura de Linkwarden** - Yo como analista, quiero mapear la arquitectura de Linkwarden para comprender el flujo de datos y dependencias. | Analysis |
| **US02** | **Configurar el entorno Docker** - Yo como desarrollador, quiero configurar el entorno Docker para garantizar la reproducibilidad del entorno. | DevOps |
| **US03** | **Validar la ejecución local** - Yo como desarrollador, quiero validar la ejecución local para asegurar que el sistema funcione en mis herramientas. | Testing |
| **US04** | **Registrar el Product Backlog en GitHub Issues** - Yo como líder de gestión, quiero registrar el Product Backlog en GitHub Issues para organizar el trabajo. | Management |
| **US05** | **Configurar GitHub Pages** - Yo como documentador, quiero configurar GitHub Pages para la documentación técnica. | Documentation |

### Sprint 2

**OBJETIVO:** Asegurar la automatización y calidad del desarrollo implementando un pipeline CI, estandarizando las revisiones de código y desplegando una versión demo funcional en el entorno de Staging.

| # | Issue | Etiqueta |
|---|---|---|
| **US06** | **Implementar un pipeline CI** - Yo como DevOps, quiero implementar un pipeline CI para automatizar la compilación y ejecución de pruebas del proyecto. | DevOps |
| **US07** | **Desplegar una demo funcional en Staging** - Yo como QA, quiero acceder a una demo funcional en staging para validar el producto antes de su entrega final. | Testing |
| **US08** | **Realizar revisión de código** - Yo como desarrollador, quiero realizar revisión de código colaborativa para asegurar la calidad y el cumplimiento de estándares. | Development |
| **US09** | **Registrar decisiones técnicas en Issues** - Yo como documentador, quiero registrar decisiones técnicas en Issues para mantener la trazabilidad del proyecto. | Documentation |
| **US10** | **Configurar GitHub Pages #2** - Yo como documentador, quiero actualizar GitHub Pages con los avances de documentación para mantenerla actualizada. | Documentation |

### Sprint 3 (Próximo)

**OBJETIVO:** Implementar mejoras finales, automatizar el despliegue continuo (CD) y redactar el artículo técnico final.

| # | Issue | Etiqueta |
|---|---|---|
| **US11** | **Mejoras finales** - Implementar mejoras finales y corregir issues pendientes para completar el alcance del proyecto. | Development |
| **US12** | **Mejoras Pipeline CI/CD** - Automatizar el despliegue continuo (CD) para garantizar entregas frecuentes y confiables. | DevOps |
| **US13** | **Configurar GitHub Pages #3** - Redactar el artículo técnico final documentando todo el proceso del proyecto. | Documentation |

---

[Ver Project Board](https://github.com/users/luqueGG/projects/7)

---

*Linkwarden - Documentación Sprints 1 y 2 | Grupo A6*
