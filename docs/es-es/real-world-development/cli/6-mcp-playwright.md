---
title: "Lección 6 - Validar la funcionalidad con Playwright MCP"
description: "Configura Playwright MCP en Copilot CLI si aún no está disponible y úsalo para explorar el filtrado en un navegador."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Escribir código no es más que una parte del trabajo. También hay que trabajar con datos y servicios externos, y ofrecer automatizaciones adicionales a Copilot. Para eso sirven los servidores MCP: amplían las herramientas y los servicios disponibles en Copilot CLI.

En esta lección:

- aprenderás qué es Model Context Protocol (MCP) y cómo lo usa Copilot CLI;
- añadirás el servidor MCP de Playwright si aún no está disponible;
- pedirás al agente que controle un navegador y explore la funcionalidad de filtrado.

## Escenario

Las pruebas unitarias y end-to-end son importantes, pero para validar los cambios de interfaz hay que interactuar con ella. Usarás el sitio como lo haría una persona para comprobar el resultado y automatizar una parte del proceso de desarrollo.

## ¿Qué es Model Context Protocol (MCP)?

[Model Context Protocol (MCP)][mcp-blog-post] permite a los agentes de IA comunicarse con herramientas y servicios externos en tiempo real, acceder a información actualizada y realizar acciones en tu nombre.

Se accede a esas herramientas y recursos a través de un servidor MCP, que hace de puente entre el agente de IA y las herramientas y servicios externos. Cada servidor MCP representa un conjunto de herramientas y recursos a los que puede acceder el agente.

Algunos servidores MCP populares son:

- **[GitHub MCP Server][github-mcp]**: proporciona acceso a API para gestionar repositorios, incidencias y pull requests de GitHub.
- **[Playwright MCP Server][playwright-mcp-server]**: ofrece automatización del navegador mediante Playwright.

GitHub aloja un [registro MCP][mcp-registry] para facilitar el descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad.

## Añadir el servidor MCP de Playwright

Si el servidor MCP de Playwright aún no está disponible, añádelo para que Copilot pueda interactuar con el sitio como lo haría una persona.

> [!NOTE]
> Si Playwright MCP ya está configurado en Copilot CLI, omite estos pasos y continúa con la sección siguiente.

1. Vuelve a tu Codespace.
2. En Copilot CLI, ejecuta:

   ```plaintext
   /mcp add
   ```

3. Escribe `playwright` como nombre y pulsa <kbd>Tab</kbd>.
4. Confirma que el tipo de servidor es **STDIO** pulsando <kbd>Enter</kbd> y, después, pulsa <kbd>Tab</kbd>.
5. En el cuadro **Command**, pega:

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. Pulsa <kbd>Control</kbd>+<kbd>S</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) para guardar el servidor MCP.
7. Pulsa <kbd>Esc</kbd> para salir del cuadro de configuración.

## Pedir a Copilot que explore la funcionalidad con Playwright

Ya has comprobado manualmente que la funcionalidad se comporta como esperas. Ahora pedirás a Copilot que haga lo mismo con el servidor MCP de Playwright.

1. Pide a Copilot que use Playwright MCP para validar la funcionalidad:

   ```plaintext
   Inicia la aplicación y usa Playwright MCP para comprobar el filtrado según la incidencia y nuestro plan. Dime qué funciona y qué no, sin hacer cambios. Cuando termines, detén el servidor que has iniciado.
   ```

> [!NOTE]
> No es imprescindible indicar que use MCP; Copilot suele elegirlo por su cuenta. Aun así, mencionarlo puede ayudarle a elegir la herramienta adecuada.

2. Observa los pasos que realiza Copilot en el navegador.
3. Lee el informe y confirma que todo funciona como esperas.

Copilot iniciará el servidor, utilizará Playwright, detendrá el servidor y presentará un informe.

## Resumen y siguientes pasos

Has usado Playwright MCP para explorar la funcionalidad en un navegador real. En esta lección:

- aprendiste qué es MCP y cómo lo usa Copilot CLI;
- añadiste el servidor MCP de Playwright si aún no estaba disponible;
- pediste al agente que controlara un navegador y explorara la funcionalidad.

A continuación, crearás un [agente personalizado de control de calidad][next-lesson] que combine las habilidades y las herramientas del navegador.

## Recursos

- [¿Qué demonios es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Añadir servidores MCP a Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
