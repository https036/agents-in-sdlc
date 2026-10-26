---
title: "Lección 5 - Personalizar y usar la habilidad quality-checks"
description: "Explora la habilidad quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código es solo una parte del trabajo. Ya has comprobado manualmente que funciona y has usado archivos de instrucciones para que siga los estándares del equipo. ¿Y las pruebas, el lint y las demás partes de la integración continua (CI)?

Las **habilidades de agente** son adecuadas para estas tareas. Ayudan a Copilot a comprender cómo realizar correctamente operaciones repetibles.

En esta lección:

- explorarás la habilidad existente `quality-checks`;
- personalizarás el formato de los resultados;
- recargarás y ejecutarás la habilidad.

## Escenario

Tailspin Toys usa la habilidad `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que sus resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las habilidades de agente agrupan instrucciones reutilizables, scripts ejecutables y recursos auxiliares que el agente carga cuando los necesita. En esencia, son carpetas con el nombre de la habilidad y un archivo Markdown `SKILL.md`. Este archivo contiene un frontmatter con el nombre y la descripción de la habilidad, una descripción general y directrices sobre cuándo usarla. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> No es obligatorio incluir más archivos o carpetas en una habilidad. La habilidad `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque usa los comandos existentes del proyecto.

Las habilidades pueden guardarse en `.github/skills` para compartirlas con el equipo como recursos del repositorio o en la carpeta de habilidades personales `~/.copilot/skills`.

## Explorar la habilidad

Revisa la habilidad `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee `name` y `description` al principio. La descripción ayuda a Copilot a decidir cuándo utilizar la habilidad.
3. Lee las instrucciones y observa cómo guían a Copilot al ejecutar pruebas y lint.
4. Fíjate en que todavía no hay una sección **Results output formatting**.

## Ejecutar la habilidad antes de modificarla

Copilot CLI puede ejecutar habilidades directamente o a partir de una petición en lenguaje natural. Pídele que ejecute las tres comprobaciones.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe final.

## Personalizar el informe

Vamos a mejorar el informe para que indique qué se ejecutó, si se completó correctamente y qué resultados comunicaron las herramientas.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Al terminar, informa de cada comando ejecutado e indica si se superó, falló o quedó bloqueado. Incluye el número de pruebas, la duración, los errores, las advertencias y otras métricas solo cuando la herramienta las comunique. Indica la acción siguiente para cada error o bloqueo y no describas nunca como superada una comprobación omitida o incompleta.
   ```

3. El archivo se guarda automáticamente.

## Ejecutar la habilidad actualizada

Copilot CLI puede recargar las habilidades editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt que antes:

   ```plaintext
   Ejecuta la habilidad quality-checks para las pruebas unitarias, lint y las comprobaciones de tipos.
   ```

3. Lee el informe y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y usado una habilidad de agente existente. En esta lección:

- exploraste `quality-checks` para pruebas unitarias, lint y comprobaciones de tipos;
- personalizaste el formato de los resultados;
- recargaste y ejecutaste la habilidad.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor MCP de Playwright][next-lesson].

## Más ejemplos de habilidades

Estos ejemplos de la comunidad sirven de referencia, no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas en el navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
