# ask-david

Skill para consultar los criterios técnicos de David en el espacio **dualhand** de
Hebra, citando las notas de origen. Si no hay una posición escrita, pregunta a David.

Estas instrucciones son públicas: puedes empezar sin tener Hebra conectado.
Solo necesitas **el MCP remoto de Hebra y la skill `ask-david`**. Tu cuenta debe
ser miembro de **dualhand** y tener habilitado allí el acceso de agentes desde Hebra.

## Pídeselo a Claude

Copia este prompt en Claude Code:

```text
Lee https://github.com/fodaveg/ask-david y sigue su README para instalar lo mínimo:
el MCP remoto de Hebra y la skill ask-david. Reutiliza lo que ya esté configurado.
Ejecuta la instalación y guíame en el login, el emparejado y el permiso de dualhand
que tenga que aprobar personalmente. Comprueba el acceso real al índice de
ask-david y responde a una consulta citando su fuente. No des la instalación por
terminada si solo has escrito la configuración.
```

## Claude Code: instalación mínima

Necesitas Node.js con `npm`/`npx` para instalar la skill. Si Hebra ya está conectado
y funciona en tu sesión, conserva esa conexión y omite el primer comando.

```bash
claude mcp add --transport http --scope user hebra https://mcp.hebra.pro/mcp
npx --yes skills add fodaveg/ask-david -g -y --skill ask-david --agent claude-code
```

1. Abre **`/mcp` dentro de Claude Code**, selecciona Hebra y completa el login con
   **tu propia cuenta**. No pegues contraseñas, tokens ni claves en el chat.
2. Si aún no tienes biblioteca conectada, sigue el emparejado web y aprueba el
   dispositivo desde tu Hebra. Lee el aviso: el servidor guardará la clave de tu
   biblioteca y quien tenga acceso root podrá leer sus notas. Comprueba los títulos
   mostrados antes de confirmar. Habilita también el acceso de tu agente en dualhand.
3. Abre una conversación nueva y pide:

   ```text
   Usa ask-david: ¿qué considera David trabajo terminado?
   ```

Debe responder citando una nota de dualhand. La skill local sin acceso a ese
espacio todavía no funciona. Compartir dualhand no da acceso a la biblioteca
personal de David.

## Otros clientes

**claude.ai:** añade o reutiliza el conector `https://mcp.hebra.pro/mcp` y completa
el login y emparejado anteriores. Descarga este repo, comprime la carpeta
[`skills/ask-david`](skills/ask-david) con su `SKILL.md` dentro y carga el ZIP en
**Customize > Skills**. Activa la skill y prueba la consulta anterior. Instalarla
con `npx` en un ordenador no la instala en claude.ai.

**Codex:** usa estos comandos y completa el mismo emparejado y permiso de dualhand:

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
