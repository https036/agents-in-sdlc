---
slug: es-es/real-world-development/app
title: "Aplicación GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot](https://docs.github.com/copilot/concepts/agents/github-copilot-app)** es una aplicación de escritorio basada en Copilot CLI que reúne el desarrollo dirigido por agentes en un único espacio de trabajo. Incluye sesiones de agentes en paralelo, modos de sesión intercambiables, lienzos compartidos y gestión nativa de incidencias y pull requests de GitHub, incluida **Agent Merge**, que guía una pull request durante las actualizaciones de rama, los comentarios de revisión, las correcciones de integración continua (CI) y la fusión.

El taller sigue un flujo de trabajo continuo en Tailspin Toys:

1. Prepara el proyecto, instala la aplicación, conecta el repositorio y explora el espacio de trabajo y el backlog inicial.
2. Añade una mejora acotada para mostrar las puntuaciones, revísala en el navegador y fusiona manualmente tu primera pull request (PR).
3. Parte de la incidencia de filtrado, define el enfoque en modo **Plan**, impleméntalo en modo **Autopilot** y revísalo en modo **Interactive**.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad existente `quality-checks` y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa el cambio completo de filtrado y usa Agent Merge para la segunda PR.
9. Comprueba si los lienzos están disponibles; después, usa Database Explorer y crea y prueba un lienzo de clasificación respaldado por el repositorio. Si no están disponibles las capacidades necesarias, registra el bloqueo y usa la alternativa de solo lectura o continúa con el repaso.

Para centrarte en el taller, crearás dos PR: primero, la de las puntuaciones; después, la del filtrado con las actualizaciones de instrucciones y de la habilidad, el perfil de QA y las pruebas. Inicia cada una desde `main` actualizado. El flujo de filtrado y calidad comparte una sesión, un worktree y una rama para que puedas aprovechar el trabajo mientras exploras las herramientas. El ejercicio final de lienzos permanece en su propia sesión para que puedas centrarte en crear y probar la experiencia compartida sin repetir el flujo de PR.

## Lecciones

| Lección | Tema | Descripción |
|--------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Instala Node.js y crea tu copia del proyecto Tailspin Toys |
| [1. Instalar la aplicación Copilot][ex1] | Configuración | Instala la aplicación, conecta el proyecto y familiarízate con el espacio de trabajo |
| [2. Añadir puntuaciones: una mejora rápida][ex2] | Primer cambio | Muestra las puntuaciones existentes y el estado sin puntuación; después, fusiona la PR 1 |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad desde su incidencia, impleméntala con Autopilot y revísala en modo Interactive |
| [4. Guiar Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones, y aplícalas al filtrado |
| [5. Personalizar y usar la habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP desde Customize y examina el comportamiento del filtrado |
| [7. Crear y usar un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado, y recopila las pruebas finales |
| [8. Crear y fusionar la PR de funcionalidad][ex8] | Revisión y fusión | Revisa el filtrado, las instrucciones, la habilidad, el perfil de QA y las pruebas; después, usa Agent Merge en la segunda PR |
| [9. Explorar y crear lienzos][ex9] | Colaboración | Usa Database Explorer y crea y prueba un lienzo de clasificación respaldado por el repositorio |
| [10. Repaso y próximos pasos][ex10] | Resumen | Repasa el flujo de trabajo, los recursos creados y otros materiales |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Un ordenador con **macOS, Linux o Windows**
- [ ] [Git instalado][install-git] en el ordenador

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza el taller, por lo que podrás completar todos los recorridos.

> [!NOTE]
> Como la aplicación Copilot se ejecuta en tu ordenador y no en un Codespace, el [ejercicio de requisitos previos][ex0] explica cómo instalar Node.js y crear tu copia del proyecto antes de instalar la aplicación.

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**. Es una directiva independiente de la de Copilot CLI.

## Primeros pasos

**[Empieza por los requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
