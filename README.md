# Sistema web escolar

Sistema web para el control de horarios y calificaciones de una preparatoria. Permite al personal administrativo elaborar horarios sin empalmes de docente, aula y grupo, a los docentes capturar calificaciones y consultar su carga, y a los alumnos consultar su horario, calificaciones y boleta.

Proyecto de la materia **Programación web** — Instituto Tecnológico Superior de Xalapa.
Profesor (Product Owner): Felipe Martín Sánchez Cortés.

## Equipo

| Integrante                      | Rol en Scrum |
| Axel Mauritzio Rodríguez Flores | Scrum Master y Developer |
| Gustavo Ramírez Peredo          | Developer |
| Marcos Francisco Zamora Reyes   | Developer |
| Gloria Dominguez Diaz           | Developer |

## Tecnologías

- Front-end: HTML5, CSS3 y JavaScript (vistas Blade de Laravel)
- Back-end: Laravel (PHP), con arquitectura MVC
- Base de datos: MySQL
- Nube: AWS 
- Control de versiones: Git y GitHub
- Gestión de tareas: Jira

## Perfiles de usuario

Superusuario, administrador, docente y alumno. Cada perfil ve únicamente las pantallas y funciones que le corresponden.

## Cómo trabajamos

La documentación Scrum completa del proyecto (visión, Product Backlog, Definition of Done y Sprint Planning) está en la carpeta `docs/`.

### Ramas

- `main`: rama estable. **Nadie sube código directamente a `main`.**
- Cada funcionalidad o tarea se trabaja en su propia rama, creada desde `main`:

| Tipo | Formato | Ejemplo |
| --- | --- | --- |
| Funcionalidad | `feature/<clave-Jira>-descripcion-corta` | `feature/SWE-14-inicio-de-sesion` |
| Corrección | `fix/<clave-Jira>-descripcion-corta` | `fix/SWE-14-mensaje-de-error` |
| Documentación o diseño | `docs/<clave-Jira>-descripcion-corta` | `docs/SWE-11-wireframes` |

Si el nombre de la rama incluye la clave de Jira, Jira enlaza automáticamente la rama y los commits con la tarjeta.

### Flujo de una tarea

1. Tomar la tarjeta en Jira y moverla a **En progreso**.
2. Crear la rama desde `main` con el formato de arriba.
3. Hacer commits pequeños y frecuentes, con mensajes claros en español (por ejemplo: `Agrega migración de la tabla aulas`).
4. Abrir un **pull request** hacia `main` y llenar la plantilla.
5. Otro integrante revisa y aprueba el pull request (mínimo 1 aprobación).
6. Unir a `main`, mover la tarjeta a **Hecho** y borrar la rama.

Una historia está terminada cuando cumple la Definition of Done del documento Scrum.

## Estructura del repositorio

```
docs/            Documentación Scrum, wireframes, guía de estilo y diagrama entidad-relación
.github/         Plantilla de pull request
(proyecto Laravel)   Se agrega en la tarea T-11 del Sprint 1
```
