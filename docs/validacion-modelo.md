# T-10: Validación del modelo de datos contra las historias de usuario

Sprint 1 · Responsable: Axel Mauritzio Rodríguez Flores · 20/10/2026
Base: diagrama entidad-relación `diagrama-er.png` y diccionario de datos (12 tablas).

Método: por cada historia se escribe la pregunta que el sistema le hará a la base de datos y qué tablas y columnas la responden. Si alguna no se puede responder, el modelo se corrige.

## Resultado

**16 de 16 historias con datos quedan respaldadas por el modelo.** HU-17 a HU-22 (estilo, menú, celular, mensajes, AWS y respaldos) no dependen de tablas.

Durante la validación se agregó una columna: `grupos.horario_completo` (HU-11).

## Tabla de validación

| Historia | Pregunta a la base de datos | Tablas y columnas que la responden | ¿Se puede? |
|---|---|---|---|
| HU-01 Iniciar sesión | ¿Existe este email con esta contraseña, está activo y qué perfil tiene? | `usuarios.email` (UNICA), `password` (hash), `activo`, `rol` | Sí |
| HU-02 Alta de administradores | ¿Ya existe ese email antes de crear la cuenta? | `usuarios` con `rol = 'administrador'`; `email` UNICA | Sí |
| HU-03 Materias | ¿Ya existe esa clave? ¿Qué materias están activas? | `materias.clave` (UNICA), `activo` | Sí |
| HU-04 Aulas | ¿Ya existe esa clave de aula? ¿Cuáles están activas? | `aulas.clave` (UNICA), `activa` | Sí |
| HU-05 Docentes | ¿Qué materias puede dar este docente y en qué horas está disponible? | `docentes` → `docente_materia` (N:M con `materias`) y `disponibilidades` (`dia`, `hora_inicio`, `hora_fin`) | Sí |
| HU-06 Grupos | ¿Ya existe un grupo con ese nombre en este semestre? | `grupos`: UNICA (`nombre`, `semestre_id`), `turno` | Sí |
| HU-07 Semestres y franjas | ¿Cuál es el semestre activo? ¿Qué franjas puedo elegir? ¿Siguen visibles los semestres pasados? | `semestres.activo`, fechas; `franjas_horarias`; `clases` y `calificaciones` guardan `semestre_id`, así que el historial no se pierde | Sí* |
| HU-08 Alumnos | ¿Ya existe esta matrícula? ¿En qué grupo está el alumno? | `alumnos.matricula` (UNICA), `alumnos.grupo_id`; un alumno es un usuario (1:1, `usuario_id` UNICA) | Sí |
| HU-09 Asignar clases | Guardar grupo, materia, docente, aula y franja de una clase | `clases` con sus 5 FK y `semestre_id` | Sí* |
| HU-10 Bloquear empalmes | Al guardar una clase, ¿ya hay otra con el mismo grupo, docente o aula en esa franja y semestre? | `clases`: UNICA (`grupo_id`, `franja_id`), UNICA (`semestre_id`, `docente_id`, `franja_id`), UNICA (`semestre_id`, `aula_id`, `franja_id`). La regla que falla indica la causa del empalme | Sí |
| HU-11 Horario completo | ¿Este grupo ya tiene su horario marcado como completo? | `grupos.horario_completo` (agregada en esta validación) | Sí |
| HU-12 Horario del alumno | ¿Qué clases tiene el grupo de este alumno, a qué hora y en qué aula? | `usuarios` → `alumnos.grupo_id` → `clases` → `materias`, `aulas`, `franjas_horarias`, `docentes` | Sí |
| HU-13 Carga del docente | ¿Qué clases da este docente, a qué hora y en qué aula? | `usuarios` → `docentes` → `clases` (`docente_id`) → `grupos`, `materias`, `aulas`, `franjas_horarias` | Sí |
| HU-14 Capturar calificaciones | ¿Qué grupos y materias le tocan a este docente? Guardar la calificación de cada alumno | `clases` donde `docente_id` es el suyo; `calificaciones` (`alumno_id`, `materia_id`, `semestre_id`, `docente_id`, `calificacion DECIMAL(4,2)`) | Sí* |
| HU-15 Consultar calificaciones | ¿Qué calificaciones tiene este alumno por materia? | `calificaciones` por `alumno_id`, con `materias` | Sí |
| HU-16 Boleta | ¿Cuál es la boleta de este alumno en un semestre, con su promedio? | `calificaciones` por `alumno_id` y `semestre_id`; el promedio se calcula (`AVG`). No se guarda tabla de boleta | Sí |

## \* Reglas que van en la lógica de Laravel (no en la base de datos)

El modelo guarda los datos para estas reglas, pero la base de datos no puede imponerlas sola. Las debe programar el back-end:

1. **Un solo semestre activo (HU-07):** al marcar uno como activo, desactivar los demás.
2. **Docente apto para la clase (HU-09):** antes de guardar una clase, comprobar en `docente_materia` que el docente imparte esa materia, y en `disponibilidades` que está libre en esa franja.
3. **Calificación entre 0 y 10 (HU-14):** `DECIMAL(4,2)` acepta hasta 99.99, así que el formulario debe validar el rango.
4. **El docente solo califica sus grupos (HU-14):** solo mostrar y aceptar alumnos de grupos donde el docente tiene una clase de esa materia.

## Criterios de aceptación de HT-02

- [x] Aparecen usuarios y roles, materias, aulas, docentes con su disponibilidad, semestres, franjas horarias, grupos, alumnos, clases y calificaciones, con PK y FK.
- [x] Las relaciones y cardinalidades permiten consultar los empalmes y la boleta por semestre.
- [ ] El diagrama queda guardado en el repositorio (`docs/`), pendiente del PR.
