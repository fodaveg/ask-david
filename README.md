# ask-david

Skill para consultar los criterios técnicos de David en el espacio **dualhand** de
Hebra, citando las notas de origen. Si no hay una posición escrita, pregunta a David.

Se instala en dos partes: el conector MCP de Hebra da acceso a las notas y la skill
dice cómo consultarlas. Revisado contra el código el 4 de noviembre de 2026. El
recorrido con una cuenta nueva todavía no se ha probado de principio a fin: si algo
no coincide con lo que ves, avisa a David.

> **En Claude Code, usa el conector de claude.ai, que Claude Code hereda.** Es la vía
> comprobada. Desde el 4 de noviembre de 2026, el servidor de Hebra ya admite a Claude
> Code al autorizar, pero `claude mcp add` no se ha probado hasta el final.

## Antes de empezar

- **Cuenta de Sumego.** El conector no la crea. Si no tienes, pídesela a David.
- **Biblioteca en Hebra.** Hebra tiene que estar conectada a Lumbre y sincronizando,
  con al menos una nota subida, en un dispositivo vinculado antes de empezar. Si creas
  la biblioteca a mitad de la conexión, se rechaza y hay que empezar otra vez.
- **Miembro de dualhand.** Si no lo eres, pídeselo a David.
- **Plan de Claude que deje añadir el conector.** En Free solo cabe un conector
  personalizado. En Team y Enterprise lo añade un Owner o administrador, y después
  cada persona conecta con su cuenta.
- **En Claude Code, login con tu suscripción de Claude** (`/login`). Con una API key,
  Claude Code no carga los conectores de claude.ai: hoy no hay otra vía, avisa a David.
- **Node.js con `npm` y `npx`** para instalar la skill en Claude Code o Codex.

## Pídeselo a Claude

Copia este prompt en Claude Code:

```text
Lee https://github.com/fodaveg/ask-david y sigue su README para instalar lo mínimo:
el conector de Hebra y la skill ask-david. Reutiliza lo que ya funcione. No uses
claude mcp add para Hebra. Si claude mcp list muestra una entrada con la URL
https://mcp.hebra.pro/mcp que no funciona, pídeme permiso para quitarla. Comprueba
los requisitos, instala la skill y guíame paso a paso en lo que tengo que hacer yo:
añadir el conector en claude.ai, el login, aprobar el dispositivo, confirmar los
títulos y encender el acceso de agentes en dualhand. Comprueba el acceso real al
índice de ask-david y responde a una consulta citando su fuente. No des la
instalación por terminada si solo has escrito la configuración.
```

## 1. Conectar Hebra

Hazlo desde el navegador del ordenador, en una sola pestaña y sin parar a medias:
la conexión caduca a los 10 minutos.

1. Si antes probaste `claude mcp add`, quita esa entrada. Si se queda, oculta el
   conector en Claude Code:

   ```bash
   claude mcp list
   claude mcp remove hebra
   ```

2. Comprueba que Lumbre y Sumego están abiertos con **tu cuenta** en ese navegador, o
   cierra la sesión que haya. Si Lumbre tiene abierta otra cuenta, la página te lo dice
   antes de aprobar nada.
3. Abre Hebra en tu otro dispositivo, en **Ajustes > Sincronización**.
4. Añade el conector en [claude.ai/customize/connectors](https://claude.ai/customize/connectors):
   **+ Add > Add custom connector**, con la URL `https://mcp.hebra.pro/mcp`. En Team y
   Enterprise, el Owner lo añade en **Organization settings > Connectors**.
5. Pulsa conectar y entra en Sumego. No pegues contraseñas, tokens ni claves en el chat.
6. Lee **Conecta tu biblioteca**: dice qué aplicación pide la conexión, qué cuenta se
   conecta y qué necesitas, incluido el código de recuperación exportado en el
   dispositivo con el que vas a aprobar. Si no lo has pedido tú, cancela. Si todo está
   bien, pulsa **Continuar en Lumbre** y después **Autorizar y volver a Hebra**.
7. En tu Hebra, pulsa **Buscar solicitudes** y aprueba la solicitud. Lee el aviso: el
   servidor guardará la clave de tu biblioteca y quien tenga acceso root podrá leer
   sus notas.
8. Vuelve al navegador y pulsa **Ya la he aprobado**. La página no se actualiza sola.
   Si tardaste más de 4 minutos, te pedirá identificarte otra vez: entra con la misma
   cuenta y pulsa el botón de nuevo.
9. Comprueba los títulos, hasta tres. Si son de tu biblioteca, pulsa
   **Sí, conectar esta biblioteca**. Si no, cancela.
10. Pulsa **Continuar** antes de 60 segundos. Si se pasa el tiempo, vuelve a conectar:
    tu biblioteca ya queda conectada y solo tendrás que pulsar **Permitir**.

Las siguientes conexiones solo piden login y **Permitir**.

## 2. Encender el acceso de agentes en dualhand

En Hebra, abre el panel del espacio dualhand y enciende tu interruptor en
**Acceso de agentes**. Solo se puede después de conectar: antes, Hebra dice que no
tienes agentes registrados. Compartir dualhand no da acceso a la biblioteca personal
de David.

## 3. Instalar la skill

**Claude Code:**

```bash
npx --yes skills add fodaveg/ask-david -g -y --skill ask-david --agent claude-code
```

**claude.ai:** descarga este repo, comprime la carpeta
[`skills/ask-david`](skills/ask-david) con su `SKILL.md` dentro y carga el ZIP en
**Customize > Skills**. Instalarla con `npx` en un ordenador no la instala en claude.ai.

## 4. Comprobar

Abre una conversación nueva y pide:

```text
Usa ask-david: ¿qué considera David trabajo terminado?
```

Debe responder citando una nota de dualhand. Tu propia biblioteca tarda en bajar la
primera vez: mientras tanto, las búsquedas pueden salir vacías y `hebra_status` dice
`lastSyncAt: null`.

## Si falla

| Qué ves | Qué significa | Qué hacer |
|---|---|---|
| Claude Code esperando sin fin, o un error de autorización en el navegador | Usaste `claude mcp add`, que no se ha probado hasta el final | Quita la entrada y sigue el apartado 1 |
| Conectado en claude.ai, pero en Claude Code sale oculto o pide autenticación | Hay una entrada local con la misma URL | `claude mcp remove hebra` |
| Claude Code no muestra el conector | Entraste con API key, o no has abierto una sesión nueva | `/login` con tu suscripción y conversación nueva |
| "Aprueba en Hebra" no avanza | Hebra no ve la solicitud: sin sincronización o sin dispositivo vinculado | Conecta y sincroniza Hebra, pulsa **Buscar solicitudes** y, si caducó, empieza otra vez |
| "Conexión rechazada" | Biblioteca vacía o recién creada, página recargada, o la vuelta se abrió en otro navegador o en la app de Lumbre | Conecta otra vez desde Claude, en el navegador del ordenador |
| "Lumbre tiene abierta otra cuenta" | La sesión de Lumbre de ese navegador es de otra persona. No se ha conectado nada | Cierra esa sesión, entra con tu cuenta y continúa |
| "Cuenta incorrecta" | La biblioteca no es de la cuenta que inició la conexión | Comprueba las sesiones y conecta otra vez |
| "Autorización no válida" | La autorización caducó o viene de otro enlace. No se ha conectado nada | Pulsa **Continuar en Lumbre** otra vez |
| "Inténtalo en un momento" | Lumbre está atendiendo muchas solicitudes | Espera los segundos que indica y continúa |
| "Demasiados intentos" | Lumbre rechazó varias autorizaciones seguidas y la conexión se cerró | Espera un rato y conecta otra vez |
| "Lumbre rechaza esta conexión" | La credencial está revocada o el agente está desactivado en Hebra | Comprueba en Hebra que el agente está activado y sigue lo que dice la página |
| "Solicitud caducada" o "No disponible" | Pasaron los 10 minutos, o falló un servicio | Conecta otra vez |
| "Acceso denegado" una vez | Conexión antigua en otra pestaña | Conecta otra vez en una sola pestaña |
| "Acceso denegado" cada vez | Un intento anterior ya no debería bloquear, así que es otra causa | No sigas reintentando: avisa a David con el texto exacto |
| JSON con `rate_limited` | Demasiados intentos desde tu IP | Espera 10 minutos |
| "Conexión con la biblioteca cortada", o `hebra_status` con `revoked: true` | Cerraste sesión en todas partes o cambiaste la contraseña | Vuelve a conectar Hebra desde tu aplicación: al iniciar sesión sale **Renueva la conexión** y no pierdes notas. No uses `hebra-mcp pair`. Si vuelve a salir, avisa a David |
| `hebra_list_spaces` vacío, o dualhand con `granted: false` | Falta el acceso de agentes o la membresía en dualhand | Apartado 2, o pídeselo a David |
| `space_unavailable` con `rate_limited` | Lumbre limita la frecuencia. No es falta de acceso | Espera los `retryAfterSeconds` que trae y repite |

## Codex

Completa después los pasos 5 a 10, el acceso de agentes y la comprobación:

```bash
codex mcp add hebra --url https://mcp.hebra.pro/mcp
codex mcp login hebra
npx --yes skills add fodaveg/ask-david -g -y --skill ask-david --agent codex
```

Las instrucciones ampliadas del equipo están en dualhand y se consultan después
de conectar Hebra. Las posiciones de David se actualizan allí; la skill se
actualiza repitiendo su comando de instalación, conservando cambios locales.

## Licencia

MIT.
