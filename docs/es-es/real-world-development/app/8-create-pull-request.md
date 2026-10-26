---
title: "Lección 8 - Crear y fusionar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil de QA y las pruebas; después, autoriza Agent Merge y confirma que la PR se ha fusionado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es hora de revisarlos juntos y abrir una pull request. Ya has fusionado manualmente la pull request (PR) de las puntuaciones; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separarías la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de fusión;
- inspeccionarás la PR completa de la funcionalidad y las pruebas de verificación;
- autorizarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha fusionado.

## Escenario

A lo largo del flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización de fusión bajo el control del desarrollador.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una pull request en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la PR y resuelve sus bloqueos —corrige comprobaciones de integración continua (CI) fallidas, responde a comentarios de revisión y actualiza la rama cuando hace falta— y la fusiona en cuanto GitHub lo permite. Funciona en segundo plano, continúa tras reiniciar la aplicación y se desactiva cuando se ha fusionado la PR.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad, pero su capacidad para editar código y fusionar cambios sigue necesitando tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para fusionar.

## Usar Agent Merge para gestionar la PR

Con el código creado y revisado, deja que Agent Merge gestione el proceso de la PR.

1. Usa el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú no basta para iniciarlo.

Agent Merge empezará a trabajar. Hará lo siguiente:

- creará la pull request con un título y una descripción;
- si iniciaste la sesión desde una incidencia, la mencionará en el cuerpo de la descripción;
- actualizará la rama o gestionará posibles conflictos con la rama de destino;
- supervisará CI para comprobar que pasen todas las comprobaciones;
- vigilará la PR por si otros desarrolladores o la revisión de código de Copilot dejan comentarios, y hará cambios para resolverlos;
- de forma opcional, fusionará automáticamente la PR cuando todo termine correctamente.

Permite que Agent Merge también fusione la PR cuando hayan pasado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para fusionar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que fusione la PR:

    ```plaintext
    Fusiona la PR cuando hayan pasado todas las comprobaciones y aprobaciones obligatorias. No eludas las protecciones del repositorio.
    ```

9. Supervisa el progreso y resuelve los bloqueos que indique. Antes de continuar, confirma que el estado de la PR es **Merged**. Tener habilitado el permiso de fusión o ver al agente en ejecución no significa que la PR se haya fusionado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni la falta de permisos. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: generación de código, pruebas, validación y ahora el proceso de pull request. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de fusión;
- inspeccionaste la PR completa y las pruebas de verificación;
- autorizaste Agent Merge solo después de revisar los cambios y confirmaste que la PR se había fusionado.

A continuación, [usarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar otra forma de inspeccionar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y pull requests con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
