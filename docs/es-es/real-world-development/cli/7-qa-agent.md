---
title: "Lección 7 - Crear y usar un agente de control de calidad"
description: "Crea un agente de QA que combine los requisitos de la incidencia, la habilidad quality-checks y Playwright MCP."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Has usado la habilidad `quality-checks` para ejecutar comprobaciones automatizadas y Playwright MCP para observar el filtrado en un navegador. Ahora combinarás esas capacidades en un agente personalizado con un proceso de QA claramente definido.

En esta lección:

- explorarás cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP;
- crearás y revisarás un perfil reutilizable de control de calidad;
- seleccionarás el agente de QA y revisarás sus conclusiones comparándolas con la incidencia del filtrado.

## Escenario

Tailspin Toys quiere revisar de manera coherente los requisitos, la calidad del código, las comprobaciones automatizadas, la cobertura de pruebas y el comportamiento en el navegador antes de abrir una pull request (PR). Un agente personalizado puede coordinar este proceso de QA y proporcionar un informe reutilizable.

## ¿Qué es un agente personalizado?

Un agente personalizado es una versión especializada de Copilot definida en un perfil Markdown. El perfil describe su propósito, sus instrucciones y las herramientas disponibles. En este taller definirás el rol de QA en `.github/agents/qa.agent.md` y lo seleccionarás en Copilot CLI.

Las personalizaciones usadas hasta ahora tienen funciones distintas: las instrucciones del repositorio describen los estándares del equipo; la habilidad `quality-checks` agrupa comprobaciones repetibles; Playwright MCP proporciona herramientas para el navegador; y el perfil de QA indica a Copilot cómo usar esas capacidades para evaluar los requisitos y comunicar los resultados. El perfil no sustituye a las otras personalizaciones ni requiere otra conversación.

## Crear el perfil de QA

Antes de abrir la PR de la funcionalidad, pide a Copilot que cree un perfil de QA reutilizable. El perfil definirá las comprobaciones y sus límites.

1. Vuelve a tu Codespace y confirma que la conversación de filtrado está en modo Interactive.
2. Pide al agente predeterminado que cree el nuevo agente personalizado:

   ```plaintext
   Crea un agente personalizado llamado QA en .github/agents/qa.agent.md. Debe comprobar las funcionalidades comparándolas con sus incidencias y los requisitos acordados, seguir las instrucciones del repositorio, ejecutar la habilidad quality-checks, usar Playwright MCP para verificar el comportamiento y añadir pruebas si falta cobertura.

   Haz que informe de cada requisito como aprobado, fallido o bloqueado, con las pruebas correspondientes. Debe pedir permiso antes de modificar el código de implementación y no debe confirmar cambios ni abrir pull requests.

   Por ahora, limítate a crear el perfil para que pueda revisarlo.
   ```

## Revisar el perfil

Antes de usar el agente, revisa el perfil para confirmar que Copilot ha registrado el flujo de QA y los límites previstos.

1. Escribe `/diff` y abre `.github/agents/qa.agent.md`.
2. Lee el frontmatter. `description` es obligatorio; `name` es opcional, pero facilita identificar el agente.
3. Comprueba que las instrucciones indican a QA que empiece por los requisitos, siga las instrucciones del repositorio, ejecute la habilidad `quality-checks` y use Playwright MCP.
4. Confirma que el agente aporta pruebas para sus conclusiones, pide permiso antes de cambiar el código de implementación y no confirma cambios ni abre pull requests.
5. Si falta alguna responsabilidad o límite, pide al agente predeterminado que lo corrija antes de continuar.

## Ejecutar QA para la incidencia

Copilot CLI carga los agentes del proyecto al iniciar una conversación. Después de crear el perfil, retoma la conversación de filtrado y selecciona QA para que pueda usar la incidencia y las decisiones de planificación que ya están en el contexto.

1. Activa el agente con:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como acabas de crear el agente, quizá no aparezca en la lista de agentes disponibles. El comando anterior lo activará.

2. Pídele que revise la funcionalidad:

   ```plaintext
   Revisa la funcionalidad de filtrado según la incidencia y las decisiones de nuestro plan. ¿Está lista para una PR?
   ```

3. Espera a que el agente de QA termine.
4. Lee el informe.

## Resumen y siguientes pasos

Has añadido un rol especializado reutilizable y has revisado su trabajo. En esta lección:

- exploraste cómo trabaja un agente personalizado con instrucciones, habilidades y herramientas MCP;
- creaste y revisaste un perfil de control de calidad;
- seleccionaste el agente de QA y revisaste sus conclusiones sobre la incidencia del filtrado.

Ya tienes la implementación, la actualización de la habilidad, el perfil de QA, las pruebas y el informe de verificación. A continuación, reunirás todo en la [PR de la funcionalidad y usarás Agent Merge][next-lesson].

## Recursos

- [Crear agentes personalizados para Copilot CLI][create-agents]
- [Configuración de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
