# .github-private

## Confluence FAQ

El perfil `agents/confluence-faq.agent.md` define un agente para
GitHub Copilot en la nube. Conecta con Atlassian Cloud mediante el MCP oficial:
`https://mcp.atlassian.com/v2/mcp?tools=all`.

Responde exclusivamente con documentación de Confluence y cita las páginas fuente.
Cuando el usuario lo solicita, crea tickets Jira basados en ese conocimiento.
La lista explícita de herramientas permite lectura y búsqueda en Confluence,
consulta de metadatos necesarios de Jira y creación de tickets. No habilita acceso
a archivos, GitHub, web, lectura de tickets ni modificación de contenido existente.
El parámetro `tools=all` permite enumerar herramientas sin habilitar los ejecutores
genéricos del MCP; las dos listas `tools` restringen lo que el agente puede usar.

### Activación

1. Publica el perfil en la rama predeterminada del repositorio `.github-private`
   de tu organización. Este cambio local todavía no lo publica.
2. Un administrador de Atlassian debe habilitar la autenticación mediante API token
   para el servidor MCP.
3. Crea un token personal con los permisos necesarios de Jira y Confluence.
   Utiliza los scopes `read:confluence:agent-interface`,
   `search:confluence:agent-interface`, `read:jira:agent-interface` y
   `write:jira:agent-interface`. No necesita scopes de escritura en Confluence
   ni búsqueda, eliminación o administración en Jira. El scope de lectura de Jira
   permite obtener los metadatos de creación; el perfil no habilita leer tickets.
   Los permisos de tu cuenta también deben permitir las acciones solicitadas.
4. Configura un secreto de **Agents** a nivel de organización o en el repositorio
   donde ejecutarás el agente, con nombre
   `COPILOT_MCP_ATLASSIAN_AUTHORIZATION`.
   Su valor completo debe ser `Basic <base64(email:api_token)>`.
   Si utilizas una API key de cuenta de servicio, el valor es `Bearer <api_key>`.
   No guardes el valor en este repositorio ni lo pegues en conversaciones.
5. Selecciona `confluence faq` en Copilot y proporciona el sitio y espacio.
   Por ejemplo: «En https://miempresa.atlassian.net, busca en el espacio SOPORTE:
   ¿cómo solicito acceso a la VPN?».
   Para crear un ticket: «Con base en esas páginas, crea un ticket en el proyecto
   HELP para solicitar mi acceso a la VPN».

La configuración está preparada, pero no se ha autenticado ni probado contra tu
sitio. No sirve para Jira o Confluence Data Center. En VS Code, el servidor MCP
se configura por separado: el campo `mcp-servers` de este perfil se utiliza en
Copilot en la nube.

### Referencias

- [Configuración de agentes y secretos MCP en GitHub](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Autenticación y scopes del MCP de Atlassian](https://developer.atlassian.com/cloud/rovo-mcp/guides/configuring-authentication-via-api-token/)
- [Catálogo de herramientas MCP de Atlassian](https://developer.atlassian.com/cloud/rovo-mcp/guides/supported-tools/)
