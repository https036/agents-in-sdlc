---
title: "Ejercicio 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados para tareas o dominios concretos de tu flujo de desarrollo. Al definirlos mediante archivos Markdown en `.github/agents`, puedes proporcionar a Copilot instrucciones específicas, buenas prácticas, patrones de programación y conocimiento del dominio para que realice ciertos tipos de trabajo con mayor eficacia. Los equipos pueden plasmar su experiencia en agentes reutilizables: por ejemplo, uno que aplique las directrices de [WCAG][wcag], otro que siga prácticas de programación segura o uno que mantenga patrones coherentes de pruebas.

Los agentes personalizados se definen mediante archivos Markdown en `.github/agents` o, de forma global, en `~/.copilot/agents`. Cada archivo incluye al menos los campos `name` y `description` en el frontmatter YAML, seguidos de un prompt Markdown que describe el comportamiento, la especialización y las instrucciones del agente.

### Agentes personalizados y habilidades de agente

Los agentes personalizados y las [habilidades de agente][agent-skills-concept] se solapan en algunos aspectos. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar tareas. La distinción más sencilla es que un **agente personalizado** realiza el trabajo y las **habilidades** son herramientas.

Los agentes personalizados tienen su propia ventana de contexto y pueden coordinar habilidades, e incluso otros agentes, como parte de su trabajo. En este laboratorio, el agente de accesibilidad revisa y actualiza el sitio según las directrices de accesibilidad; podría usar habilidades como una de flujo de trabajo de pull requests o una para ejecutar y gestionar pruebas.

> [!NOTE]
> No hay una única forma correcta de crear un agente personalizado. Como ocurre con cualquier función de IA, prueba e itera para descubrir qué funciona mejor en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Explorarás estos aspectos de los agentes personalizados:

- cómo se definen;
- cómo asignarles una tarea.

## Escenario

Tailspin Toys quiere que su plataforma de financiación colectiva sea accesible para todas las personas, independientemente de sus capacidades visuales o preferencias. Algunos usuarios han comentado que el tema oscuro actual es difícil de leer debido al escaso contraste entre el texto y el fondo. Para responder a esta necesidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es esencial, usarás un agente personalizado para generar la funcionalidad lo antes posible.

## Revisar el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisa su contenido para entender cómo guiará a Copilot.

1. Vuelve a tu Codespace y abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible;
   - las buenas prácticas de accesibilidad;
   - ejemplos de HTML, CSS y JavaScript;
   - errores habituales que se deben evitar.

## Crear y asignar una incidencia

El centro de control es el espacio principal para trabajar con agentes del entorno. Allí puedes asignar tareas al agente en la nube, supervisarlas e incluso redirigir una sesión activa y proporcionar indicaciones adicionales. Asigna una tarea para crear el modo de alto contraste.

1. Ve a tu repositorio.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación.
4. Selecciona **Blank issue**.
5. Escribe `Add high contrast mode to website` en **Title**.
6. En **Description**, escribe:

    ```plaintext
    Necesitamos un modo de alto contraste para el sitio. Debe haber un control para activarlo y desactivarlo, y la preferencia debe guardarse en el almacenamiento local del navegador.
    ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot**.
9. Selecciona **Accessibility agent** en la lista de agentes personalizados.

    ![Cuadro de asignación del agente en la nube con el agente personalizado de accesibilidad seleccionado](../../../_images/ex5-select-custom-agent.png)

10. Selecciona **Assign**.
11. Copilot empezará a trabajar en segundo plano.

## Resumen y siguientes pasos

Has explorado los [agentes personalizados][custom-agents] de GitHub Copilot, asistentes de IA especializados para tareas y dominios concretos. Con ellos puedes codificar la experiencia y los estándares del equipo en agentes reutilizables que guían a Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados;
- cómo asignar una tarea a un agente personalizado.

Mientras Copilot implementa el modo de alto contraste, puedes [supervisar y dirigir la sesión del agente][next-lesson] desde el centro de control.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Prepararse para usar agentes personalizados en una organización][org-custom-agents]
- [Prepararse para usar agentes personalizados en una empresa][enterprise-custom-agents]

---

| [← Lección anterior: Agente en la nube de GitHub Copilot][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
