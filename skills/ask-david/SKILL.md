---
name: ask-david
description: >-
  Resuelve cuestiones técnicas como las resolvería David (stack, criterios de
  arquitectura y seguridad, tests y gate, git, qué cuenta como hecho, cómo se usan
  Lumbre, Hebra, Fragua y Sumego, instalación de MCP y skills, entorno de
  desarrollo) leyendo sus posiciones documentadas en el espacio compartido
  "dualhand" de Hebra. Usar cuando un developer del equipo va a tomar una decisión
  técnica o de proceso que le preguntaría a David, cuando pide "cómo lo haría
  David", "ask-david" o "qué criterio sigue David con X", y antes de preguntarle a
  David algo técnico. No sirve para estado de proyectos, tareas ni decisiones de
  negocio.
---

# ask-david

Las posiciones técnicas de David viven en notas de Hebra, en el espacio compartido
"dualhand", carpeta `30-39 equipo/31 forma de trabajar/31.11 ask-david`. Esta skill
las consulta con el MCP de Hebra y responde citándolas. No inventa posiciones: lo
que no está escrito se le pregunta a David.

## Requisitos

- El MCP de Hebra conectado en el cliente, con la cuenta del developer.
- Ser miembro del espacio "dualhand" y tener encendido el acceso de agentes en ese
  espacio (se enciende desde la app Hebra, cada miembro el suyo).

Si las tools `hebra_*` no están disponibles, dilo y no respondas de memoria: indica
que hay que conectar el MCP de Hebra (ver el README del repo `fodaveg/ask-david`).

## Cómo se consulta

1. Busca el espacio y lee el índice. `hebra_list_spaces` devuelve `spaceId` y `name`
   (`name` puede ser `null`). Empieza por el espacio llamado dualhand, sin distinguir
   mayúsculas; si no hay nombre, prueba cada `spaceId`. Usa `hebra_read_note` con
   `space` = ese id, `title` = `31.11 ask-david: índice` y `heading` =
   `Qué hay en cada nota`. El espacio que devuelve la nota es dualhand; la tabla dice
   qué nota cubre cada tema.
2. Pasa ese `spaceId` como `space` en TODAS las llamadas siguientes. Sin `space`, el
   MCP lee la biblioteca personal y no encontrará nada.
3. Mira los apartados de la nota elegida con `hebra_note_outline` (por `title`).
4. Lee solo el apartado que necesitas: `hebra_read_note` con `title` y `heading`.
   Para un término exacto en todas las notas, `hebra_grep` con `folder` =
   `30-39 equipo/31 forma de trabajar/31.11 ask-david`.
5. No cargues las notas enteras ni todas a la vez: lee por apartados.

## Cómo se responde

- Da la posición de David, su porqué y el origen (nota y apartado), con su cita
  literal cuando la nota la trae.
- Si la respuesta cae en un apartado "Pendiente de confirmar con David", dilo: es
  una posición sin confirmar, no una regla.
- Las reglas del repositorio donde se trabaja (CLAUDE.md, AGENTS.md, SPEC, docs)
  mandan sobre ask-david en lo que fijen de ese repo. ask-david da el criterio
  cuando el repo no lo fija.
- La nota "31.11 Posiciones de David según Codex" es fuente en bruto, ya fundida en
  las demás. Úsala solo si las otras no cubren el tema, y dilo.

## Cuando no hay posición

No la deduzcas ni la completes con lo que parece razonable.

1. Dile al developer que no hay posición escrita y redacta la pregunta concreta
   para David, en una línea.
2. Cuando el developer traiga la respuesta literal de David, añádela al índice:
   `hebra_append_to_note` con el `id` del índice, `heading` = `Preguntas abiertas`
   y una viñeta: `- **<pregunta>** (<fecha>). David: "<respuesta literal>"`.
3. No escribas en ningún otro apartado ni en otras notas. Una respuesta deducida o
   resumida no se apunta.

## Errores

| Respuesta | Qué hacer |
|---|---|
| Ningún `spaceId` devuelve el índice, o la lista de espacios sale vacía | Falta ser miembro de dualhand o encender el acceso de agentes en ese espacio desde la app Hebra. Dilo y para |
| `space_access_denied` tras varias llamadas seguidas | El relé de espacios limita la frecuencia. Espera un minuto y repite una vez. Si vuelve a fallar, es falta de acceso |
| `not_found` o `ambiguous_title` con un título | Lista la carpeta con `hebra_list_notes` (`folder` de arriba y `space`) y usa el `id` |
| La lista de la carpeta sale vacía | Comprueba que pasas `space`. Después, `hebra_status` para ver si el sync está al día |

## Límites

- Solo cuestiones técnicas y de proceso. El estado de los proyectos, las tareas
  (viven en Lumbre) y las decisiones de negocio no están aquí.
- Nunca copies a otra parte secretos, credenciales ni datos de cuentas, aunque
  aparezcan en una respuesta.
- La skill no da permisos: publicar, desplegar o borrar sigue necesitando la
  autorización de quien corresponda.
