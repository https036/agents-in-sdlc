---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado desde una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Empezamos añadiendo una pequeña funcionalidad al proyecto, pero los cambios más amplios requieren un proceso más sólido. GitHub Copilot CLI está diseñado para trabajar con los flujos de trabajo existentes de una organización y ayudarte a crear las soluciones adecuadas. En esta serie de lecciones seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás que el código es válido y que la funcionalidad se comporta como esperas, y finalmente la fusionarás en el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama mientras completas el flujo de la funcionalidad. Normalmente se usarían ramas o PR distintas para los diferentes tipos de archivo, pero aquí simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot desde una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la funcionalidad con el modo Autopilot;
- revisarás el código;
- comprobarás manualmente la funcionalidad en un navegador reenviado.

Al continuar, actualizarás las instrucciones del repositorio, personalizarás la habilidad existente `quality-checks`, añadirás la validación MCP, crearás un agente de QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y los visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programarla hay que acordar detalles como la combinación de categorías. Usarás el modo Plan para tomar esas decisiones y después autorizarás una implementación acotada con Autopilot.

## Contexto

La IA generativa no cambia los fundamentos del desarrollo de software; incluso los hace más importantes. La mayoría de los desarrolladores siguen un proceso parecido a este:

1. Partir de una incidencia que describa el trabajo.
2. Crear un plan de implementación.
3. Desarrollar y revisar el código.
4. Ejecutar pruebas para validarlo.
5. Comprobar manualmente la funcionalidad.
6. Crear una pull request (PR).
7. Cuando el código se haya revisado y la integración continua se complete correctamente, fusionarlo.

> [!NOTE]
> Los detalles concretos dependen del equipo y de la organización, pero la mayoría de los procesos siguen un patrón parecido.

Este enfoque estándar ayuda a garantizar que el código generado por IA cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de conversación

El **modo de conversación** determina el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para alternar entre los modos:

- **Interactive**: colaboras con el agente. Este propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea primero un plan y no puede editar los archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en modo Plan, revisa el plan y luego usa Autopilot para implementarlo.

## Empezar por la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu Codespace y asegúrate de que el repositorio y el terminal están listos.

1. Vuelve a tu Codespace. Si está detenido, reinícialo.
2. Confirma que la PR de puntuaciones se ha fusionado.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Con las flechas, selecciona **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

El prompt incluirá ahora `#7` (o un número parecido). El símbolo `#` permite incorporar al contexto una incidencia o pull request (PR) de GitHub.

## Planificar el filtrado

La planificación permite acordar cómo implementar una funcionalidad antes de delegar el trabajo a Copilot. Para tareas complejas, conviene dedicar tiempo a concretar el enfoque.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Comprueba que el indicador bajo el prompt muestra **Plan**.
2. Después de la referencia a la incidencia, introduce:

   ```plaintext
   Crea un plan para implementar esta funcionalidad.
   ```

Copilot explorará el proyecto y determinará el enfoque más adecuado.

3. Responde a las preguntas de Copilot sobre cómo debería funcionar el filtrado. No hay respuestas incorrectas.
4. Cuando termine el plan, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para ampliarlo.
5. Revisa el plan.
6. Pide a Copilot que cambie cualquier parte que no se ajuste a tus decisiones.

## Aprobar Autopilot

Con el plan redactado y revisado, puedes implementar la funcionalidad.

1. Selecciona **Accept plan and build on autopilot (recommended)** o una opción con un nombre parecido en tu versión.
2. Confirma que el indicador bajo el prompt muestra **Autopilot**.
3. Observa cómo Copilot recorre el plan, genera el código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa el plan antes. Si Copilot informa de dependencias ausentes o de un conflicto de puertos, resuelve el problema de configuración antes de dar por terminadas las comprobaciones.

## Revisar y verificar la implementación

Como cualquier otro código, el código generado debe revisarse antes de fusionarlo.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para entrar en el modo Interactive. Comprueba que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones que tomaste al planificar.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diferencias.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que corrija los fallos.

## Explorar la nueva funcionalidad

Inicia la aplicación y ábrela en el puerto reenviado de Codespaces para comprobar que funciona.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Inicia la aplicación para que pueda probar la funcionalidad de filtrado en el navegador. Dime la URL y deja el servidor en ejecución.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, por editorial y las combinaciones acordadas.
4. Comprueba que las opciones para restablecer los filtros y mostrar resultados vacíos se ajustan a la incidencia y a tus decisiones.
5. Vuelve a tu Codespace y pide a Copilot que detenga el servidor de desarrollo.

## Resumen y siguientes pasos

Has usado diferentes modos de conversación para desarrollar y revisar una funcionalidad. En esta lección:

- iniciaste una conversación de Copilot desde una incidencia de GitHub;
- definiste los requisitos en el modo Plan;
- implementaste la funcionalidad con Autopilot;
- revisaste el código;
- comprobaste manualmente la funcionalidad en un navegador reenviado.

A continuación, profundizarás en la generación de código para asegurarte de que siga las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
