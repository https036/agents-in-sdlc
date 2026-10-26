---
title: "Lección 2 - Añadir puntuaciones: una mejora rápida"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revísalo en un navegador reenviado y fusiónalo como tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ya has instalado Copilot CLI y probado una conversación; ahora harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una puntuación con estrellas, pero las tarjetas de la página de inicio no la muestran. Pedirás al agente que la añada, revisarás el cambio y lo fusionarás como tu primera pull request.

En esta lección:

- iniciarás una conversación centrada en una rama de funcionalidad;
- pedirás al agente que haga un cambio pequeño;
- revisarás el cambio con `/diff`;
- ejecutarás la aplicación y comprobarás el cambio en un navegador reenviado;
- crearás y fusionarás tu primera pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una puntuación con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de juegos de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como ejercicio inicial, pedirás al agente que muestre la puntuación existente en cada tarjeta: un cambio pequeño y autocontenido, perfecto para tu primera sesión.

## Anatomía de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de CLI usa el repositorio y la rama que estén activos en el terminal, en lugar de crear un árbol de trabajo dedicado. Las conversaciones guardadas te permiten retomar el mismo diálogo, mientras que los archivos y la rama siguen siendo el estado normal de Git en el disco.

En una conversación verás tres elementos: tus prompts y las respuestas del agente, la actividad de sus herramientas mientras explora y edita archivos, y los cambios que puedes inspeccionar con `/diff`.

## Iniciar una conversación y solicitar el cambio

1. Vuelve a tu Codespace.
2. Si el terminal no sigue abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si Copilot aún no está iniciado, ejecuta:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Envía este prompt:

   ```plaintext
   Muestra el starRating de cada juego sobre 5 en las tarjetas de la página de listado. Si la puntuación es null, muestra "No rating yet". Mantén el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. Ya has añadido una funcionalidad con Copilot CLI.

## Revisar las diferencias

Las modificaciones generadas por IA deben revisarse antes de fusionarlas, aunque sean pequeñas.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que cada tarjeta muestra la puntuación numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones que ejecutó Copilot y pídele que corrija los fallos.
5. Cuando termines, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico y no determinista, así que el código exacto puede variar. Revisa el comportamiento, no esperes una implementación idéntica.

## Comprobar los cambios

No basta con leer el código y suponer que funciona. Pide a Copilot que inicie el sitio web para que puedas examinar la interfaz en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda revisar en el navegador el cambio de las puntuaciones con estrellas. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas muestran las puntuaciones sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que inició:

   ```plaintext
   Detén el servidor de desarrollo que has iniciado.
   ```

## Crear y fusionar tu primera pull request

Ya has creado la funcionalidad. Ahora crearás una pull request (PR) para integrar el código en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Confirma los cambios revisados de las puntuaciones con estrellas con un mensaje de commit adecuado.
   ```

2. Escribe `/pr create`. Copilot CLI enviará el commit existente al crear la PR y mostrará su URL.
3. Abre la PR; para ello, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu Codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has publicado tu primer cambio con GitHub Copilot CLI. En concreto:

- iniciaste una conversación de Copilot centrada en una rama de funcionalidad;
- indicaste al agente que hiciera un pequeño cambio en las tarjetas;
- revisaste el cambio con `/diff`;
- ejecutaste la aplicación y comprobaste la puntuación en un navegador reenviado;
- creaste y fusionaste tu primera pull request.

A continuación, partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot para [crear una funcionalidad más amplia][next-lesson].

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
