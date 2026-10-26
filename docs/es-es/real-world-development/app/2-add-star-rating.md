---
title: "Lección 2 - Añadir puntuaciones: una mejora rápida"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, realiza un pequeño cambio en las tarjetas de juegos y fusiónalo como tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

En la lección anterior conociste el espacio de trabajo y probaste un chat rápido. Ahora iniciarás una **sesión de agente** para realizar el primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una puntuación con estrellas, pero las tarjetas de la página de inicio aún no la muestran. Pedirás al agente que la añada, revisarás el cambio y lo fusionarás como tu primera pull request.

En esta lección:

- iniciarás una sesión de agente y aprenderás cómo se organiza;
- pedirás al agente un cambio pequeño y acotado;
- revisarás el cambio en la vista de diferencias del espacio de trabajo;
- ejecutarás la aplicación en local para comprobar el cambio en el navegador;
- abrirás y fusionarás tu primera pull request.

## Escenario

Cada juego de Tailspin Toys puede tener una puntuación con estrellas, que ya aparece en su página de detalles. Sin embargo, las tarjetas de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como primer ejercicio, pedirás al agente que muestre la puntuación existente en cada tarjeta: un cambio pequeño y autocontenido, ideal para tu primera sesión.

## Anatomía de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión tiene un **worktree y una rama de Git propios**, lo que permite ejecutar varias a la vez —por ejemplo, una para añadir una función y otra para corregir un error— sin que sus cambios entren en conflicto. Las sesiones aparecen agrupadas por repositorio en la barra lateral; selecciona una para cambiar de sesión.

En una sesión verás tres elementos: la **conversación** con el agente, la **actividad de sus herramientas** mientras explora y edita archivos, y la lista de **archivos modificados** con sus diferencias.

## Iniciar una sesión y solicitar el cambio

Inicia una sesión nueva para explorar el proyecto e implementar la función. En una [lección anterior][prior-lesson] añadiste el proyecto desde su repositorio de GitHub.

1. Vuelve a la aplicación GitHub Copilot o ábrela.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona `tailspin-toys` como repositorio.
4. Elige **new working tree** y el modo **Interactive** debajo del cuadro de prompt. Envía esta petición:

    ```plaintext
    Muestra el starRating de cada juego sobre 5 en las tarjetas de la página de listado. Si la puntuación es null, muestra "No rating yet". Mantén el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
    ```

5. Pulsa <kbd>Enter</kbd> para enviar el prompt a Copilot.

La aplicación crea un worktree nuevo, una copia aislada del proyecto. Después explora el proyecto, localiza los archivos que hay que actualizar y crea el código necesario. Ya has añadido una funcionalidad con la aplicación Copilot.

## Revisar las diferencias

Revisa todos los cambios generados por IA antes de fusionarlos, incluso los pequeños.

1. En la esquina superior derecha de la aplicación, selecciona **Toggle review panel**. Se abrirá la vista de diferencias con los cambios pendientes de Copilot.

    ![Barra superior de la aplicación GitHub Copilot con una flecha que señala el botón Toggle review panel, a la derecha de Create PR](../../../_images/app-2-review-panel.png)

2. Deberías ver un cambio en `GameCard.astro`, el archivo principal que muestra los detalles de los juegos. Podría parecerse a este fragmento: muestra la puntuación si existe y `No rating yet` cuando `starRating` es `null`.

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico y no determinista. El código exacto puede variar, aunque debería ser parecido al ejemplo.

## Comprobar los cambios

Revisa los resultados de las comprobaciones automatizadas del agente antes de abrir el navegador. Confirma que las pruebas cubren una puntuación numérica y el caso `null`. La falta de un requisito previo o una comprobación omitida no cuenta como aprobada; revisa cualquier solicitud de instalación antes de aprobarla.

Leer el código y dar por hecho que funciona no basta. Pide a Copilot que abra el sitio en un lienzo de navegador integrado.

> [!TIP]
> Un lienzo es un widget interactivo disponible dentro de la aplicación Copilot. Más adelante explorarás lienzos personalizados e incluso crearás uno; por ahora usarás el lienzo de navegador integrado.

1. Pide a Copilot que inicie la aplicación y la abra en el lienzo:

    ```plaintext
    Inicia la aplicación y ábrela en el lienzo del navegador.
    ```

2. Al cabo de unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de Copilot.
3. Confirma que las tarjetas de juegos con puntuación la muestran sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que inició para esta sesión y cierre el lienzo:

    ```plaintext
    Detén el servidor de desarrollo y cierra el lienzo del navegador.
    ```

## Abrir y fusionar tu primera pull request

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se solicita, selecciona **Sign in with your browser** y sigue las indicaciones para autenticarte.
3. Si Copilot indica que la directiva del repositorio exige una incidencia asociada para crear la PR, confirma que debe seguir ese proceso.
4. Copilot empezará a crear la PR.
5. Selecciona la burbuja **PR** situada encima del chat para abrir la solicitud en el panel de revisión. Puedes revisarla allí.
6. Cuando esté lista, selecciona **Ready to merge**.
7. En el nuevo cuadro de diálogo, selecciona **Merge pull request** para fusionarla.

## Resumen y siguientes pasos

Has iniciado tu primera sesión de agente y publicado tu primer cambio. En concreto:

- iniciaste una sesión de agente y aprendiste cómo se organizan las sesiones;
- pediste al agente un cambio pequeño y acotado en las tarjetas de juegos;
- revisaste el cambio en la vista de diferencias;
- ejecutaste la aplicación en local para comprobar la puntuación en el navegador;
- abriste la PR 1, revisaste sus comprobaciones y autorizaste explícitamente la fusión.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para desarrollar una funcionalidad más amplia.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Gestionar incidencias y pull requests con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instalar-y-configurar-la-aplicacion-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
