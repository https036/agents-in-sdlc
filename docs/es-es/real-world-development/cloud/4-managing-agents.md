---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se encargue de tres tareas distintas para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente trabaja de forma asíncrona y autónoma, es importante poder supervisar esas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida [la página de agentes][agents-page] en GitHub.com. Desde este centro de control puedes ver las tareas con pull requests abiertas, examinar las operaciones realizadas e incluso orientar una sesión activa.

En esta lección:

- explorarás la página de agentes para supervisar las tareas;
- orientarás una sesión activa para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación del modo de alto contraste, el equipo decidió que también sería buen momento para añadir un modo claro. Ya se está actualizando el estilo del sitio y añadiendo la función de alternar modos, así que conviene incluirlo en el mismo trabajo. Dirigirás al agente para que implemente ambos modos.

## Revisar las tareas del agente en la nube

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Examina las tareas en el panel principal y en el lateral. Deberías ver las que has asignado a Copilot, como:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea activa y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - preparar el entorno;
   - configurar servidores MCP;
   - realizar pasos para completar la tarea.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el enfoque que haya elegido.

4. Observa también el panel de la pull request (PR) que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados.

## Orientar al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada un modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión que corresponde al modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Espera unos minutos hasta que la sesión termine la configuración y empiece a trabajar. Lo sabrás cuando aparezcan mensajes parecidos a los del ejemplo.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    Mientras trabajamos en el modo de alto contraste, añadamos también un modo claro. Debe haber un control para este modo que permita a los usuarios elegir el modo de visualización que prefieran.
    ```

    ![Tarea del agente en la nube en la página de agentes con el cuadro para orientar una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma la petición y la incorpora a su trabajo.

## Dejar que Copilot trabaje

Copilot se ocupará de la tarea actualizada y añadirá la nueva petición después de terminar el paso que estaba realizando cuando enviaste el mensaje.

El proceso tardará unos minutos. Puedes aprovechar para repasar lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

Has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas del agente en la nube. Puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir sesiones en curso para cambiar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar el centro de control y la página de agentes para supervisar las tareas;
- redirigir una sesión activa para solicitar funcionalidad adicional.

Mientras Copilot termina el trabajo de accesibilidad, pasarás a [iterar sobre las pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
