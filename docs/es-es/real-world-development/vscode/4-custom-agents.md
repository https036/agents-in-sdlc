---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o dominios concretos del flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar instrucciones específicas, buenas prácticas, patrones de programación y conocimiento del dominio. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, uno que aplique las directrices de [WCAG][wcag], otro que siga prácticas de programación segura o uno que mantenga patrones coherentes de pruebas.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo incluye al menos los campos `name` y `description` en el frontmatter YAML, seguidos de un prompt Markdown que describe el comportamiento, la especialización y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar tareas. La distinción más sencilla es que un **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades, e incluso otros agentes, como parte de su trabajo. En este laboratorio, el agente de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; podría usar habilidades como una de flujo de trabajo de pull requests o una para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier función de IA, prueba e itera para descubrir qué funciona mejor en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han comentado que el tema oscuro actual es difícil de leer debido al escaso contraste entre el texto y el fondo. El equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es esencial, usarás un agente personalizado para generar la funcionalidad.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad existente;
- usarás el agente en Copilot Chat para implementar un modo de alto contraste.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace, abre un terminal y crea una rama nueva a partir de `main` para mantener separado el trabajo de accesibilidad de la PR de filtrado del ejercicio 3:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de HTML, CSS y JavaScript;
   - errores habituales que se deben evitar.

## Usar el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes de Copilot Chat. Puedes seleccionar uno para limitar la sesión a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent** si aún no está seleccionado.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes en la parte inferior de la vista Chat. Muestra el agente activo; de manera predeterminada, es **default**.
3. Selecciona **Accessibility agent**.
4. Envía este prompt:

    ```plaintext
    Añade un modo de alto contraste al sitio. Debe haber un control para activar y desactivar el alto contraste, y la preferencia debe conservarse después de recargar la página mediante el almacenamiento local del navegador.
    ```

5. Copilot Chat explorará el código, propondrá cambios y editará los archivos del proyecto. Cada edición aparecerá en el chat con la ruta del archivo y las diferencias para que puedas revisarla.
6. El indicador **Files changed** se actualizará con los archivos modificados por el agente.

> [!NOTE]
> El proceso puede tardar unos minutos. Copilot modificará archivos reales del repositorio, como componentes Astro, estilos y pruebas relacionadas.

En el ejercicio siguiente revisarás y orientarás el trabajo en curso.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents], asistentes especializados para tareas y dominios concretos. Con ellos puedes codificar la experiencia y los estándares del equipo en agentes reutilizables que guían a Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo usar un agente personalizado en el modo Agent de Copilot Chat.

A continuación, [supervisarás y orientarás el trabajo del agente][next-lesson]: revisarás los cambios a medida que se producen y añadirás un control de modo claro a la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados en awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Probar la funcionalidad con el servidor MCP de Playwright][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
