---
title: "Ejercicio 3 - Probar la funcionalidad con el servidor MCP de Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Modo de agente][previous-lesson] · [Siguiente lección: Agentes personalizados →][next-lesson]

Acabas de crear el filtrado con el modo Agent. Antes de abrir una pull request, comprueba que funciona en el navegador. En lugar de probar la aplicación manualmente, configurarás el **servidor MCP de Playwright** para que Copilot controle un navegador real, pruebe la funcionalidad y, después, publique tu rama y abra una PR.

En este ejercicio:

- aprenderás qué es Model Context Protocol (MCP) y cómo sus servidores amplían las herramientas de Copilot;
- añadirás el servidor MCP de Playwright al espacio de trabajo;
- pedirás a Copilot que lo use para probar manualmente el filtrado en un navegador;
- publicarás tu rama y abrirás una pull request con el trabajo.

## ¿Qué es Model Context Protocol (MCP)?

El modo Agent es más potente cuando puede acceder a herramientas fuera del editor. Copilot utiliza Model Context Protocol (MCP), un estándar para comunicarse con herramientas y servicios externos.

![Diagrama del funcionamiento interno del modo Agent y su interacción con el contexto, el LLM y las herramientas, incluidas las que proporcionan los servidores MCP y las extensiones de VS Code](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que los agentes de IA se comuniquen con herramientas y servicios externos en tiempo real, accedan a información actualizada mediante recursos y realicen acciones en tu nombre mediante herramientas.

Se accede a estas herramientas y recursos a través de un servidor MCP, que hace de puente entre el agente de IA y las herramientas externas, como API existentes o herramientas locales como los paquetes NPM. Cada servidor ofrece un conjunto distinto de herramientas y recursos.

Algunos servidores MCP populares son:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: proporciona acceso a API para gestionar repositorios, actualizarlos y administrar incidencias y pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: ofrece automatización del navegador mediante Playwright, incluida la navegación por páginas, la cumplimentación de formularios y la selección de botones.

Hay muchos otros servidores MCP. GitHub aloja un [registro MCP](https://github.com/mcp) para facilitar su descubrimiento y las contribuciones al ecosistema.

> [!CAUTION]
> Trata los servidores MCP como cualquier otra dependencia del proyecto. Antes de usar uno, revisa su código fuente, verifica quién lo publica y valora las implicaciones de seguridad. Usa solo servidores de confianza y ten cuidado al conceder acceso a recursos u operaciones sensibles.

## Revisar la configuración de MCP

El archivo `.vscode/mcp.json` configura los servidores MCP disponibles en este espacio de trabajo de VS Code.

1. Abre `.vscode/mcp.json` en tu Codespace.
2. Deberías ver un servidor `github` ya configurado:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

La plantilla del proyecto incluye esta entrada, por eso Copilot pudo leer el backlog de incidencias en el ejercicio anterior. Usa el [servidor GitHub MCP remoto][remote-github-mcp-server]; no tienes que instalarlo localmente. VS Code se conecta por HTTP y te autentica con GitHub la primera vez que Copilot usa una herramienta.

## Añadir el servidor MCP de Playwright

Añadirás un segundo servidor. El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador que puede controlar para probar la funcionalidad.

1. Añade una entrada `playwright` junto a `github` en `.vscode/mcp.json`:

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. Guarda el archivo.

A diferencia del servidor remoto `github`, `playwright` es local: VS Code lo inicia en tu máquina mediante `npx @playwright/mcp@latest`. La opción `--headless` indica a Playwright que ejecute el navegador sin una ventana visible, algo necesario en Codespaces.

> [!NOTE]
> Tailspin Toys ya usa Playwright para las pruebas end-to-end, así que normalmente el navegador requerido ya estará instalado. Si Copilot informa de que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

## Iniciar y confiar en el servidor

VS Code inicia los servidores MCP **cuando se necesitan**. La primera vez que Copilot solicita sus herramientas, VS Code inicia el servidor y te pide que confirmes que confías en él.

1. En `.vscode/mcp.json`, aparecerá una acción **Start** encima de la entrada `playwright`. Selecciónala para iniciar el servidor y confirmar que la configuración es correcta.
2. Si VS Code pregunta si confías en el servidor, revisa la configuración y confirma que confías en él para que pueda iniciarse.

Iniciarlo manualmente es opcional. Si omites este paso, VS Code lo iniciará y pedirá confirmación la primera vez que Copilot necesite sus herramientas.

## Probar el filtrado

El [servidor MCP de Playwright][playwright-mcp-server] proporciona a Copilot un navegador real. El agente puede abrir una página, navegar, aplicar filtros y comunicarte los resultados sin que tengas que recorrer la aplicación manualmente.

El servidor trabaja con el [árbol de accesibilidad][playwright-mcp-server] de la página, no con capturas de pantalla. El agente analiza elementos estructurados y etiquetados —botones, enlaces y elementos de lista— de la misma forma que la tecnología de asistencia. Así, una comprobación funcional breve también sirve como revisión básica de accesibilidad.

Con el servidor conectado y la aplicación en ejecución, pide a Copilot que pruebe el filtrado:

```text
Usa el servidor MCP de Playwright para abrir un navegador en la aplicación que se ejecuta en http://localhost:4321 y verificar la nueva funcionalidad de filtrado de juegos:

1. Ve a la página de juegos e indica cuántos aparecen.
2. Aplica un filtro de categoría y confirma que la lista solo muestra juegos de esa categoría.
3. Borra el filtro y aplica uno de editorial; confirma que la lista solo muestra juegos de esa editorial.
4. Combina un filtro de categoría y otro de editorial, y confirma que los resultados cumplen ambos.

Indica lo que observas en cada paso y señala cualquier comportamiento inesperado.
```

Copilot iniciará un navegador mediante el servidor MCP de Playwright, seguirá los pasos e informará de los resultados. Compara el informe con los criterios de aceptación de la incidencia. Si algo no es correcto, haz preguntas o pide a Copilot que arregle el código antes de abrir la pull request.

> [!NOTE]
> Para esta prueba, la aplicación debe ejecutarse en `http://localhost:4321`. Si detuviste el servidor de desarrollo, vuelve a iniciarlo antes de enviar el prompt. La primera vez que Copilot use el servidor MCP de Playwright puede que tenga que descargar un navegador; si indica que falta, pídele que ejecute `npx playwright install chromium` y vuelve a intentarlo.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar la rama y crear una pull request

Ahora que has confirmado que la funcionalidad funciona, puedes abrir una pull request (PR) para que el equipo la revise. Primero publicarás la rama `filtering-vscode`.

1. Abre el panel **Source Control** del Codespace y revisa los cambios de Copilot.
2. Prepara los cambios seleccionando el icono **+**.
3. Genera un mensaje de commit con el botón **Sparkle**.

   ![Panel Source Control con los cambios realizados](../../../_images/ex3-source-control-changes.png)

4. Selecciona **Publish** para enviar la rama al repositorio.

## Crear la pull request

Hay varias formas de crear una pull request, como github.com o la interfaz de línea de comandos (CLI) de GitHub. En este taller, Copilot buscará la incidencia correspondiente y creará una PR asociada a ella.

1. Abre Copilot Chat y selecciona **New Chat** para iniciar una sesión nueva.
2. Confirma que **Agent** está seleccionado para que Copilot pueda usar las herramientas de GitHub.
3. Pide a Copilot que cree una PR:

   ```plaintext
   Busca en el repositorio la incidencia sobre el filtrado por categoría y editorial. Crea una pull request para la rama actual y relaciónala con la incidencia correcta.
   ```

4. Si es necesario, selecciona **Continue** para permitir que Copilot recopile información y realice las operaciones. La primera vez que use una herramienta de GitHub quizá tengas que autenticarte.
5. Observa cómo Copilot busca las incidencias, encuentra la correcta y crea la PR.
6. Selecciona el enlace que genera Copilot para revisar la pull request, pero **no la fusiones todavía**.

## Resumen y siguientes pasos

En este ejercicio:

- aprendiste qué es MCP y cómo sus servidores amplían las herramientas de Copilot;
- añadiste el servidor MCP de Playwright al espacio de trabajo;
- lo usaste para probar manualmente el filtrado en un navegador antes de publicarlo;
- publicaste la rama y abriste una pull request.

Has usado un servidor MCP para probar una funcionalidad. MCP es solo una forma de ampliar Copilot; a continuación, crearás un [agente personalizado][next-lesson] para agilizar tareas concretas como las revisiones de accesibilidad.

## Recursos

- [¿Qué demonios es MCP y por qué todo el mundo habla de ello?][mcp-blog-post]
- [Servidor MCP de Playwright de Microsoft][playwright-mcp-server]
- [Servidor MCP de GitHub][github-mcp-server]
- [Registro MCP de GitHub][mcp-registry]
- [Servidores MCP en VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
