---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Comprueba o configura Playwright MCP en Copilot CLI y úsalo para explorar la funcionalidad de filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Para desarrollar software también hay que trabajar con datos y servicios externos, y proporcionar a Copilot más herramientas. Los servidores Model Context Protocol (MCP) amplían las funcionalidades integradas en la CLI con herramientas y servicios adicionales.

En esta lección, aprenderás a:

- entender qué es Model Context Protocol (MCP) y cómo lo utiliza Copilot CLI.
- añadir el servidor Playwright MCP si aún no está disponible.
- pedir al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y de extremo a extremo son importantes, pero también es necesario interactuar con la interfaz para validar los cambios. Permitirás que Copilot use el sitio web como lo haría una persona y automatice más comprobaciones.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real, consulten información actualizada y realicen acciones en tu nombre.

Los agentes acceden a estas herramientas y recursos a través de un servidor MCP, que actúa como puente. Cada servidor ofrece un conjunto distinto de herramientas y recursos. Algunos servidores MCP conocidos son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a las API para gestionar repositorios, incidencias y solicitudes de incorporación de cambios de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece automatización del navegador mediante Playwright.

Hay muchos más servidores MCP. GitHub aloja un [registro de MCP][mcp-registry] que facilita descubrirlos y contribuir a ellos.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia. Antes de utilizar uno, revisa su código fuente, comprueba quién lo publica y evalúa las implicaciones de seguridad.

## Añade el servidor Playwright MCP

Si Copilot CLI aún no tiene disponible el servidor Playwright MCP, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu codespace.
2. En Copilot CLI, escribe este comando para abrir el cuadro de diálogo para añadir un servidor MCP:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma el tipo de servidor **STDIO** con <kbd>Enter</kbd> y pulsa <kbd>Tab</kbd>.
5. Introduce lo siguiente en el cuadro de diálogo **Command**:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de diálogo de MCP.

## Pide a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad se comporta como esperas. Ahora pídele a Copilot que haga lo mismo con el servidor Playwright MCP.

1. Usa este prompt para pedir a Copilot que valide la funcionalidad mediante Playwright MCP:

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> No es imprescindible mencionar el servidor MCP: Copilot suele identificarlo por sí mismo. Indicarle qué herramienta debe usar puede hacer que los resultados sean más coherentes.

2. Observa los pasos que Copilot realiza en el navegador para comprobar que la funcionalidad funciona.
3. Lee el informe y confirma que todo se comporta como se esperaba.

Copilot iniciará el servidor, interactuará con el sitio mediante Playwright, detendrá el servidor y te dará un informe.

## Resumen y pasos siguientes

Has utilizado el servidor Playwright MCP para explorar la funcionalidad en un navegador real desde Copilot CLI. En esta lección:

- has aprendido qué es MCP y cómo lo utiliza Copilot CLI.
- has añadido el servidor Playwright MCP si aún no estaba disponible.
- has pedido al agente que controlara un navegador y explorara el filtrado.

A continuación, [crearás un agente personalizado de QA][next-lesson] que combinará la habilidad y las herramientas del navegador.

## Recursos

- [Qué es MCP y por qué se habla tanto de ello][mcp-blog-post]
- [Servidor Playwright MCP de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
