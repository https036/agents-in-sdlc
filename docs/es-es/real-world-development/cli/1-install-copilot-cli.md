---
title: "Lección 1 - Instalar GitHub Copilot CLI"
description: "Instala y autentica Copilot CLI en tu Codespace, conoce sus funciones y localiza la incidencia de filtrado del backlog."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] es un asistente de programación con agentes que se ejecuta en el terminal y permite explorar bases de código, generar código, ejecutar comandos e interactuar con herramientas externas desde la línea de comandos. Puedes delegar tareas, solicitar cambios y mantener la concentración. Primero tendrás que instalarlo, algo que puedes hacer con herramientas que ya conoces.

En esta lección:

- instalarás GitHub Copilot CLI con npm;
- te autenticarás con tu cuenta de GitHub;
- confiarás en el repositorio del taller y probarás una conversación rápida;
- buscarás la incidencia de filtrado con el servidor GitHub MCP integrado.

## Escenario

Tu equipo empieza a utilizar agentes de IA para abordar un backlog cada vez mayor. Copilot CLI lleva esas capacidades al terminal, donde muchos desarrolladores trabajan habitualmente. En esta lección lo instalarás y autenticarás para usarlo durante el resto del taller.

## Instalar Copilot CLI

Puedes instalar Copilot CLI con [npm][install-cli], WinGet o Homebrew. Como GitHub Codespaces ya incluye Node.js, usarás npm.

1. Vuelve a tu Codespace y abre un terminal.
2. Comprueba que Node.js está instalado y cumple el requisito de versión:

   ```bash
   node --version
   ```

   Deberías ver la versión 24 o posterior.

3. Instala Copilot CLI globalmente:

   ```bash
   npm install -g @github/copilot
   ```

4. Comprueba la instalación:

   ```bash
   copilot --version
   ```

   Debería aparecer el número de versión.

## Autenticarse con GitHub

La primera vez que lo inicies, Copilot CLI te pedirá que te autentiques con tu cuenta de GitHub.

1. Inicia Copilot CLI:

   ```bash
   copilot
   ```

2. Si se solicita, sigue las instrucciones de autenticación con código de dispositivo y autoriza Copilot CLI.
3. Copilot CLI mostrará este prompt:

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. Confirma que la ruta corresponde al repositorio Tailspin Toys y selecciona **Yes, and remember this folder for future sessions**.

> [!NOTE]
> En Codespaces quizá ya estés autenticado mediante tu sesión de GitHub. Si Copilot CLI se inicia sin pedir autenticación, ya puedes continuar.

## Familiarizarse con la herramienta

Los comandos que escribes en el prompt habitual del shell se ejecutan directamente en Codespaces. Una vez iniciado Copilot CLI, el lenguaje natural se envía al agente y los comandos de barra controlan la conversación.

1. Escribe `/model`, usa las flechas para seleccionar **Auto**, pulsa <kbd>Enter</kbd> y vuelve a pulsar <kbd>Enter</kbd> para confirmar.
2. Escribe `/help` para consultar los comandos de tu versión y pulsa <kbd>Esc</kbd> para cerrar la ayuda.
3. Haz una pregunta sencilla para comprobar que funciona:

   ```plaintext
   ¿Cuáles son los archivos principales de este proyecto?
   ```

4. Lee la respuesta y observa cómo Copilot explora el repositorio antes de contestar.
5. Escribe `/mcp list` y confirma que el servidor GitHub MCP integrado está disponible.
6. Pide a Copilot que encuentre la incidencia de filtrado:

   ```plaintext
   Usa GitHub MCP para buscar en este repositorio la incidencia titulada "Allow users to filter games by category and publisher". Dame su URL y un resumen breve. No hagas cambios.
   ```

7. Abre la URL y lee la incidencia. La usarás después de completar un primer cambio rápido.

> [!TIP]
> Una sesión normal de Copilot CLI trabaja en la rama activa del terminal; no crea automáticamente un worktree. Antes de cada cambio crearás una rama de funcionalidad.

## Usar el atajo del taller

Por lo general, Copilot CLI pide permiso antes de usar herramientas fuera de los permisos establecidos. Para este taller, volverás a iniciarlo con `--yolo`, un atajo aprobado por el usuario que elimina las solicitudes de aprobación dentro del Codespace y permite centrarse en los ejercicios.

> [!CAUTION]
> `--yolo` habilita permisos automáticos completos (`--allow-all-tools`, `--allow-all-paths` y `--allow-all-urls`). Úsalo solo en un entorno aislado, como Codespaces o una máquina virtual, y no lo configures nunca como alias predeterminado para el desarrollo diario. Consulta [Permitir y denegar el uso de herramientas][allow-all-warning].

En este taller, `--enable-all-github-mcp-tools` habilita las herramientas GitHub MCP de lectura y escritura que se usarán para trabajar con incidencias y pull requests. El Codespace limita el acceso al equipo local, pero los recursos de GitHub autenticados siguen siendo reales. Revisa los cambios antes de publicarlos o fusionarlos.

1. Sal de Copilot CLI con `/exit`.
2. Vuelve a iniciarlo desde la raíz del repositorio:

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. Haz otra pregunta breve sobre el proyecto para confirmar que la conversación funciona y, después, sal con `/exit`.

Copilot guarda las conversaciones automáticamente. Más adelante, después de cambiar las instrucciones o añadir un agente, usarás `copilot --resume` para volver a la misma conversación y rama de funcionalidad.

## Resumen y siguientes pasos

En esta lección:

- instalaste Copilot CLI con npm;
- te autenticaste con tu cuenta de GitHub;
- confiaste en el repositorio del taller y probaste una conversación;
- encontraste la incidencia de filtrado con el servidor GitHub MCP integrado.

A continuación, [harás tu primer cambio acotado][next-lesson] y usarás Copilot CLI para mostrar las puntuaciones en las tarjetas.

## Recursos

- [Instalar GitHub Copilot CLI][install-cli]
- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
