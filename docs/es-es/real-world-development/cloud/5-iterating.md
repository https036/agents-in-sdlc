---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

## Revisar el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidad. Le has pedido que documente el código, cree una funcionalidad de juegos relacionados para que el equipo de diseño pueda probarla y añada funciones de accesibilidad, incluidos los modos de alto contraste y claro. Ahora revisarás los cambios de código y, si hace falta, darás indicaciones a Copilot para mejorarlos.

### Escenario

Los fundamentos del diseño de software y DevOps no cambian al incorporar IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Por eso, revisarás las sugerencias de GitHub Copilot para la documentación, los juegos relacionados y las funciones de accesibilidad antes de habilitar la revisión para el resto del equipo.

## Seguridad y agente en la nube de GitHub Copilot

Como el agente en la nube trabaja de forma asíncrona y sin supervisión, hay restricciones de seguridad para mantener el entorno protegido:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que utilizará para el código.
- El agente se ejecuta en GitHub Actions, donde crea un entorno efímero y separado.
- Una persona debe aprobar los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisar la documentación generada

Empieza por la primera pull request (PR) creada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Usa la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> En la PR quizá aparezca un aviso que indica que un firewall bloqueó a GitHub Copilot. Es **lo esperado**, porque el acceso del agente a recursos externos —incluidas las llamadas a servidores MCP externos— está limitado de forma predeterminada. Si quieres, puedes [personalizar o desactivar el firewall del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la pull request titulada **Add missing documentation** o con un nombre parecido.

> [!NOTE]
> Si Copilot aún está trabajando, la PR mostrará la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, incluidos los comentarios de documentación TSDoc. Los cambios exactos pueden variar. Busca comentarios TSDoc, convenciones de TypeScript y encabezados de comentario: proceden de los archivos de instrucciones que revisaste al principio del ejercicio 2.
6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto deberían pasar en la aplicación Astro. Puede tardar unos minutos.

## Pedir cambios a GitHub Copilot

Trabajar con Copilot en una pull request no es un proceso unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios en línea del código, igual que mencionarías a otro miembro del equipo. Copilot leerá el comentario e iniciará otra sesión para atenderlo. Como los resultados no son deterministas, no hay un texto exacto que debas usar. Por ejemplo, podrías pedirle que:

- añada un encabezado breve al principio de cada archivo para describir su contenido;
- añada comentarios TSDoc a archivos TypeScript y Astro;
- cree un README que describa la estructura de la aplicación Astro.

1. Añade un comentario que solicite un cambio en la documentación generada y menciona a **@copilot**. Puedes usar una de las ideas anteriores u otra petición sobre la documentación del código.
2. Selecciona **View Session** para observar cómo Copilot trabaja. Iniciará una sesión nueva para hacer los cambios.
3. Selecciona **Back to pull request** para volver a la PR.

    ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, aparecerá un commit nuevo en la pull request.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta quedar satisfecho. Después, puedes convertir la PR en borrador a una PR lista para revisión y fusionarla.

![Convertir la PR en lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisar la funcionalidad de juegos relacionados

Vuelve a la PR que Copilot generó para resolver la incidencia sobre la sección de juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada **Show related games on the game details page** o con un nombre parecido.
4. Selecciona **Files changed** y revisa el código generado.
5. Cuando hayas revisado los cambios, vuelve a **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían pasar en la aplicación Astro. Puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en Codespaces para probar manualmente la funcionalidad. Abre el terminal y ejecuta estos comandos, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la funcionalidad de juegos relacionados. Puedes pedirle cambios de forma iterativa mediante comentarios nuevos en la pestaña **Conversation**. Por ejemplo, podrías ajustar el número de juegos relacionados o pedir encabezados de comentario y comentarios TSDoc. Recuerda que la incidencia se asignó antes de actualizar las instrucciones personalizadas.

## Revisar las funciones de accesibilidad

Por último, revisa las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debe incluir tanto el modo de alto contraste solicitado en el ejercicio 3 como el modo claro que pediste desde el centro de control en el ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada **Add high contrast mode to website** o con un nombre parecido.

> [!NOTE]
> Si Copilot aún está trabajando, la PR mostrará la marca **[WIP]**. Espera a que termine; puede tardar unos minutos.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación, prestando especial atención a:
   - los controles para cambiar entre modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado;
   - el código JavaScript o TypeScript que gestiona el cambio de modo.

6. Cuando hayas revisado los cambios, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto deberían pasar en la aplicación Astro. Puede tardar unos minutos.
10. **Opcional:** puedes cambiar a esta rama en Codespaces para probar las funciones de accesibilidad. Abre el terminal y ejecuta estos comandos, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba los controles de alto contraste y modo claro en el navegador. Confirma que funcionan y que conservan la selección al volver a cargar la página.

El agente personalizado de accesibilidad ayudó a orientar a Copilot para implementar estas funciones según buenas prácticas de accesibilidad. Si detectas algún problema o aspecto que pueda mejorarse, menciona a **@copilot** en un comentario para solicitar cambios, igual que hiciste con las otras PR.

## Ejercicio opcional: seguir delegando

El agente en la nube resulta útil para asignarle tareas reales del backlog y ocuparte de otras cosas. Para practicar, crea más incidencias en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- crear un formulario de interés para quienes apoyan el proyecto, en la página de detalles del juego;
- implementar paginación en la lista de juegos;
- añadir validación de entradas y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el recorrido del agente en la nube. En estas lecciones:

- **Revisaste los archivos de instrucciones personalizados del repositorio** para comprobar su efecto en el trabajo del agente.
- **Asignaste incidencias al agente en la nube de Copilot** y observaste cómo preparaba el entorno, planificaba y trabajaba de forma asíncrona.
- **Creaste y usaste un agente personalizado** para implementar funciones de accesibilidad, como los modos de alto contraste y claro.
- **Usaste la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad.
- **Revisaste e iteraste sobre las pull requests del agente**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el recorrido del agente en la nube. Los otros recorridos complementan lo que has practicado:

- 🖥️ **[Recorrido de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente en el IDE.
- 💻 **[Recorrido de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y el puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente una incidencia de refactorización o cobertura de pruebas;
- crea un agente personalizado para otro dominio;
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas del flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el firewall del agente en la nube de Copilot][agent-firewall]

---

| [← Lección anterior: Gestionar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
