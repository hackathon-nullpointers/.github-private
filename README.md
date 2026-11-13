# .github-private

## Confluence FAQ en VS Code

El agente `agents/confluence-faq.agent.md` responde únicamente con documentación
consultada en el Confluence de `https://bcphackaton.atlassian.net/wiki/home`.
Cita sus fuentes y crea tickets en el proyecto Jira `KAN` cuando se lo pides.
No habilita herramientas de web, archivos, GitHub ni lectura de tickets.

### Abrir y conectar

1. En VS Code, abre `confluence-faq.code-workspace` mediante **File → Open Workspace from File**,
   o abre directamente la carpeta `.github-private`. Abrir solo la carpeta padre
   no carga esta configuración de carpeta.
2. Usa una versión actual de VS Code con GitHub Copilot y el harness **Copilot**.
   El servidor está en `.mcp.json`; `.vscode/settings.json` registra la carpeta `agents`.
3. Abre **Chat: Open Customizations → MCP Servers**, localiza `atlassian` y
   completa la autorización OAuth en el navegador con tu cuenta de Atlassian.
4. Selecciona **confluence faq** en el selector de agentes del chat.
5. Prueba: «Busca en Confluence cómo solicitar acceso a la VPN y cita las páginas».
   Para crear: «Con base en esas páginas, crea un ticket en KAN para mi solicitud».

No se ha iniciado sesión ni probado el acceso real. Comprueba las herramientas
que muestra el agente después de conectar; validar los archivos no confirma la conexión.

### ¿Dónde pongo el token?

**La configuración incluida utiliza OAuth: no necesitas pegar un API token.**
Autorizas tu cuenta en el navegador y el cliente administra la sesión.
No necesitas secretos de GitHub para ejecutarlo en VS Code.

Si tu organización exige un API token, el MCP admite el encabezado
`Authorization: Basic <base64(email:api_token)>`. Esa variante requiere configurar
credenciales locales por separado; no pegues ese valor en archivos compartidos
ni en el chat. La configuración incluida no contiene encabezados de autenticación.

### Alcance

Confluence: solo `bcphackaton.atlassian.net`. `/wiki/home` es la portada: el agente
puede leer las páginas accesibles del sitio, no solamente un espacio.
Jira: creación de tickets en `KAN` y consulta de metadatos necesarios para crearlos.
El tablero 1 es una vista del proyecto, no el destino técnico de creación.

Las listas de herramientas del agente y de `.mcp.json` restringen las operaciones.
El endpoint `?tools=all` permite enumerar herramientas individuales sin habilitar
los ejecutores genéricos. El límite de sitio/proyecto está en las instrucciones;
para imponerlo también mediante permisos, utiliza una cuenta dedicada con acceso
solo a ese Confluence y creación de tickets solo en `KAN`.

### Versiones anteriores de VS Code

Si utilizas el harness **Local** y no detecta `.mcp.json`, añade el servidor con
**MCP: Add Server → HTTP**, nombre `atlassian`, URL
`https://mcp.atlassian.com/v2/mcp?tools=all` y ámbito del workspace. Completa OAuth
y verifica las herramientas del agente; no habilites todas las del servidor.

### Referencias

- [Configuración MCP en VS Code](https://code.visualstudio.com/docs/agents/reference/mcp-configuration)
- [Agentes personalizados y ubicaciones](https://code.visualstudio.com/docs/agents/reference/custom-agent-file-reference)
- [MCP oficial de Atlassian](https://developer.atlassian.com/cloud/rovo-mcp/guides/getting-started/)
