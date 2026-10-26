---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: usa Plan para acordar un enfoque, Autopilot para crear el filtrado desde una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios necesitan un proceso más sólido. La aplicación GitHub Copilot está diseñada para trabajar con los flujos existentes de una organización y ayudar a crear las soluciones adecuadas. En esta serie de lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la función se comporta como esperas, y finalmente la integrarás en el proyecto.

> [!NOTE]
> Usarás la misma sesión durante todo el flujo de trabajo de la funcionalidad. Normalmente se usarían sesiones o PR distintas para los diferentes tipos de archivo, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una sesión de agente nueva desde una incidencia de GitHub;
- definirás los requisitos en modo **Plan**;
- implementarás la funcionalidad con **Autopilot**;
- revisarás el código;
- comprobarás manualmente la funcionalidad en un lienzo del navegador.

Al continuar con esta funcionalidad, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR.

## Escenario

El catálogo de Tailspin Toys está creciendo y quienes lo visitan necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero hay que acordar algunos detalles antes de programarla, como la combinación de categorías. Usarás Plan para decidir esos aspectos y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

Introducir agentes de programación con IA en el flujo de desarrollo no cambia los principios fundamentales; incluso los hace más importantes. Muchos desarrolladores siguen un proceso parecido:

1. Abrir una incidencia que describa el trabajo.
2. Crear un plan de lo que se debe desarrollar.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Comprobar manualmente la nueva funcionalidad.
6. Crear una pull request (PR).
7. Cuando se haya revisado el código y la integración continua termine correctamente, fusionar los cambios.

> [!NOTE]
> Los detalles concretos varían según el equipo y la organización, pero la mayoría sigue alguna versión del proceso anterior.

Este enfoque ayuda a que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla el nivel de autonomía del agente. Puedes configurarlo en el menú desplegable que aparece debajo del campo de prompt y cambiarlo cuando quieras:

- **Interactive**: colaboras con el agente. Este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan. Lo revisas y apruebas antes de que lo ejecute.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan, revisa el plan y después usa Autopilot para implementarlo.

## Iniciar una sesión desde la incidencia

Antes de empezar, confirma que la PR de puntuaciones se ha fusionado y que tu rama local `main` está actualizada.

1. En la barra lateral, dentro de **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, en la navegación superior del repositorio, selecciona **Issues**. Abre **Allow users to filter games by category and publisher**. Es una incidencia existente creada por la plantilla; no tienes que crear otra.
2. Selecciona **New session** y elige un **new working tree** basado en `main` actualizado.

    ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está vinculada a la sesión y selecciona **Plan** en el selector de modos.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal pero no en la sesión, revisa `PATH` o la configuración del gestor de versiones y, después, cierra la aplicación por completo y vuelve a abrirla. Reintenta la operación y las comprobaciones fallidas; no reinstales npm solo porque el agente no lo encuentre.

## Planificar el filtrado

La planificación permite revisar el enfoque antes de que Copilot escriba código. Al iniciar la sesión desde la incidencia, Copilot ya tiene la solicitud en su contexto. Envía:

```plaintext
Implementa esta funcionalidad.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluya filtrado por categoría y editorial, controles accesibles, cambios de acceso a datos y pruebas. Aclara cualquier comportamiento dudoso, como la combinación de varias categorías o qué ocurre cuando no hay juegos que coincidan.

El plan debe incluir lint, pruebas unitarias, pruebas E2E y comprobación de tipos, con las herramientas existentes del proyecto. Céntrate en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide cambios en el plan antes de aprobarlo y guarda la URL de la incidencia y las aclaraciones acordadas para la validación posterior.

## Aprobar Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en lugar de dar por hecho que la aprobación lo cambia. El indicador debe mostrar **Autopilot**.

Copilot empezará a implementar el plan, generar código y ejecutar pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación inmediatamente, así que revisa antes el plan. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar las comprobaciones por terminadas. Detén solo los servidores que hayas iniciado.

## Revisar y verificar la implementación

Como cualquier otro código, la implementación debe revisarse antes de fusionarla.

1. Abre **Changes** e inspecciona la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de varias categorías y editoriales. Comprueba que se siguen las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y la comprobación de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright crea una compilación y un servidor de vista previa, y puede reutilizar un servidor local. Confirma que el servidor probado pertenece a este worktree, no a una lección anterior.

## Explorar la funcionalidad

El código parece correcto, pero ¿se ejecuta? Pide a Copilot que inicie la aplicación y la abra en el lienzo del navegador.

1. Envía este prompt:

    ```plaintext
    Inicia la aplicación y ábrela en el lienzo del navegador.
    ```

2. Al cabo de unos instantes, se iniciará la aplicación y se abrirá una ventana del navegador dentro de Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y comprueba también sus resultados.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin resultados y comprueba el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo de esta sesión y cierre el lienzo:

    ```plaintext
    Detén el servidor de desarrollo y cierra el lienzo del navegador.
    ```

## Conservar la sesión para las lecciones siguientes

Todavía no crees ni fusiones la PR del filtrado. Conserva esta sesión, el worktree y la rama para las lecciones 4 a 8. A continuación, aplicarás instrucciones personalizadas al código, añadirás mejoras a la habilidad y a QA, y después crearás y fusionarás la PR combinada en la lección 8. Tras detener el servidor de desarrollo, puedes dejar la sesión inactiva y volver a ella en la siguiente lección.

## Resumen y siguientes pasos

Has usado varios modos de agente para desarrollar y revisar una funcionalidad. En esta lección:

- iniciaste una sesión de agente desde una incidencia de GitHub;
- definiste los requisitos en modo **Plan**;
- implementaste la funcionalidad con **Autopilot**;
- revisaste el código;
- comprobaste manualmente la funcionalidad en un lienzo del navegador.

A continuación, explorarás cómo se genera el código y cómo aplicar las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
