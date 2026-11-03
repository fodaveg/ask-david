# ask-david

Skill para agentes (Claude Code, Codex, claude.ai) que resuelve cuestiones técnicas
como las resolvería David: stack, criterios de arquitectura y seguridad, tests y gate,
git, qué cuenta como hecho, cómo se usan Lumbre, Hebra, Fragua y Sumego, instalación
de MCP y skills, y entorno de desarrollo.

La skill no lleva el contenido. Lo lee con el MCP de Hebra de las notas del espacio
compartido "dualhand", carpeta `30-39 equipo/31 forma de trabajar/31.11 ask-david`.
Cuando no hay posición escrita, redacta la pregunta para David y, con su respuesta
literal, la apunta en el índice.

## Requisitos

1. Node.js (para `npx`): `brew install node` en macOS, `sudo dnf install nodejs` en
   Fedora.
2. El MCP de Hebra conectado con tu cuenta (paso 1 de la instalación).
3. Ser miembro del espacio "dualhand" en Hebra y encender en él el acceso de agentes,
   desde la app Hebra.

## Instalación

### 1. Conectar el MCP de Hebra

Claude Code:

```bash
claude mcp add --transport http hebra https://mcp.hebra.pro/mcp
```

Después abre `/mcp` en Claude Code y autoriza la conexión en el navegador.

Codex:

```bash
codex mcp add hebra --url https://mcp.hebra.pro/mcp
codex mcp login hebra
```

claude.ai (web, escritorio o móvil): añade un conector personalizado con la URL
`https://mcp.hebra.pro/mcp`. No añadas un token en la URL.

### 2. Instalar la skill

Claude Code y Codex, con [`skills`](https://skills.sh):

```bash
npx --yes skills add fodaveg/ask-david -g -y --skill ask-david --agent codex claude-code
```

Comprueba que quedó instalada:

```bash
test -f ~/.agents/skills/ask-david/SKILL.md && echo ok
test -f ~/.claude/skills/ask-david/SKILL.md && echo ok
```

claude.ai: comprime la carpeta `skills/ask-david` (con su `SKILL.md` dentro) en un
ZIP y súbelo como skill personalizada desde los ajustes de claude.ai.

### 3. Probarla

Abre una conversación nueva y pide:

```
usa ask-david: ¿qué versión de Node uso en los proyectos web?
```

Tiene que responder citando la nota y el apartado de donde lo saca.

## Actualizar

```bash
npx --yes skills update ask-david -g -y
```

La skill sigue la rama `main` de este repositorio. Las posiciones se actualizan en
Hebra y no necesitan actualizar la skill.

## Licencia

MIT.
