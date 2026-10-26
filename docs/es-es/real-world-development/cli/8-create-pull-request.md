---
title: "Lección 8 - Crear y fusionar la PR de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Ahora revisarás todos los cambios en conjunto y abrirás una pull request (PR). La PR de las puntuaciones la fusionaste manualmente; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separarías la funcionalidad, las instrucciones, la actualización de la habilidad y el agente de QA en varias PR. Para simplificar el taller, todo el flujo de filtrado y calidad se ha mantenido en una conversación y una rama.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una pull request;
- inspeccionarás el cambio completo de la funcionalidad y las pruebas de verificación;
- crearás la PR del filtrado;
- habilitarás Agent Merge solo después de revisar el trabajo y confirmarás que se ha fusionado.

## Escenario

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control de la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una pull request. Cuando lo habilitas, Copilot resuelve lo que bloquea la PR —por ejemplo, corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y actualiza la rama base cuando hace falta— y, después, habilita la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de esa responsabilidad, pero primero debes revisar el trabajo y decidir que está listo.

## Usar Agent Merge para gestionar la PR

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará las comprobaciones de integración continua de la PR. Cuando todo termine correctamente, la fusionará.
7. Comprueba que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni la falta de permisos. Si algo bloquea el proceso, revisa el motivo y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: la generación de código, las pruebas y la validación, y ahora el proceso de pull request. En esta lección:

- aprendiste qué es Agent Merge y cómo automatiza el ciclo de vida de una pull request;
- inspeccionaste el cambio completo y las pruebas de verificación;
- creaste la PR del filtrado;
- habilitaste Agent Merge solo después de revisar el trabajo y confirmaste que la PR se había fusionado.

A continuación, explorarás [otros comandos útiles de Copilot CLI][next-lesson] para gestionar el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
