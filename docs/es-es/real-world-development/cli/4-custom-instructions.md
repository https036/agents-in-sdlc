---
title: "Lección 4 - Guiar Copilot con instrucciones personalizadas"
description: "Explora las instrucciones del repositorio, añade un estándar de documentación y aplícalo al código de filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

El contexto es esencial al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta, asegúrate de que Copilot disponga de esas directrices. Los [archivos de instrucciones][instruction-files] describen no solo *qué* código quieres, sino también *cómo* debe estructurarse. Ahora que has creado el filtrado, explorarás las instrucciones que usó Copilot, añadirás un estándar de documentación y lo aplicarás al código.

En esta lección:

- explorarás cómo llegan al agente las instrucciones del repositorio y las instrucciones con ámbito de ruta;
- actualizarás el archivo de instrucciones para que se sigan los estándares de desarrollo;
- observarás el efecto de los archivos de instrucciones en el código.

## Escenario

Tailspin Toys tiene un conjunto de directrices y requisitos para sus prácticas de desarrollo:

- Los comentarios deben explicar la intención y las decisiones no evidentes, no repetir el código.
- Las funciones exportadas de `db/` y `src/lib/` deben documentar su propósito, parámetros y valores de retorno con TSDoc/JSDoc, incluido el argumento `db` inyectable cuando corresponda.
- Los componentes Astro reutilizables deben documentar sus contratos `Props`, y los comentarios deben mantenerse actualizados cuando cambie el código relacionado.
- Deben conservarse las directrices de formato y lint existentes.

Los archivos de instrucciones permiten proporcionar a Copilot el contexto necesario para que haga el trabajo de acuerdo con estas prácticas.

## Archivos de instrucciones

Las instrucciones personalizadas aportan contexto y preferencias para que Copilot entienda mejor tu estilo de desarrollo y tus requisitos. Puedes indicar convenciones, bibliotecas y tipos de comentarios preferidos. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información relevante para la mayoría de las solicitudes.
- `.github/instructions/*.instructions.md`: archivos que proporcionan directrices para lenguajes, tipos de archivo o tareas concretas.

> [!NOTE]
> La compatibilidad con otros formatos de instrucciones depende del entorno. Consulta la [referencia sobre compatibilidad con instrucciones personalizadas][custom-instructions-support] antes de basarte en un formato concreto.

## Explorar los archivos de instrucciones personalizadas del proyecto

El proyecto inicial ya incluye varios archivos de instrucciones. Revísalos para entender cómo funcionan antes de hacer cambios.

1. Vuelve a tu Codespace.
2. En el editor de Codespaces, no en el terminal, abre `.github/copilot-instructions.md`.
3. Lee la descripción breve del proyecto y sus directrices de desarrollo. Estas instrucciones se aplican a todas las interacciones con Copilot en este repositorio.
4. Abre `.github/instructions` y explora sus archivos. Hay instrucciones para Astro, la capa de datos de Drizzle, las pruebas y otros temas.
5. Abre `.github/instructions/unit-tests.instructions.md` y observa el campo `applyTo`, que establece un patrón para determinar a qué archivos se aplican las instrucciones.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa sus enlaces a otros archivos de instrucciones y a archivos del proyecto. Así puedes dividir instrucciones extensas en archivos reutilizables más pequeños y mostrar a Copilot ejemplos que debe seguir.

## Actualizar las instrucciones del equipo

Modifica el archivo principal `copilot-instructions.md` para que las nuevas funciones de TypeScript tengan comentarios TSDoc.

1. Busca la sección **Code formatting guidance** en `.github/copilot-instructions.md`, aproximadamente hacia la línea 35.
2. Añade este punto al final de la sección:

   ```markdown
   - Todo el TypeScript nuevo debe incluir comentarios TSDoc con fines de documentación.
   ```

El archivo se guardará automáticamente.

## Aplicar las instrucciones actualizadas

Copilot CLI carga las instrucciones del repositorio al iniciar una conversación. Después de guardar el cambio, sal de la conversación y retómala para que Copilot aplique las nuevas directrices sin perder el contexto de la funcionalidad.

1. Si la conversación de filtrado sigue abierta, escribe `/exit`. Copilot CLI guarda las conversaciones automáticamente.
2. Desde la raíz del repositorio, en la rama `game-filters-cli`, retoma la conversación guardada:

   ```bash
   copilot --resume
   ```

   Si se te solicita, selecciona la conversación de filtrado y confirma que está activa la rama `game-filters-cli`.
3. Pide a Copilot que aplique las nuevas directrices:

   ```plaintext
   Acabamos de actualizar las instrucciones y las directrices de código. ¿Puedes actualizar el código generado para que las siga?
   ```

4. Escribe `/diff` y revisa los archivos TypeScript modificados. Comprueba que los comentarios TSDoc explican correctamente el código.

## Resumen y siguientes pasos

Has explorado cómo Copilot CLI obtiene el contexto de los archivos de instrucciones y has aplicado un nuevo estándar a la funcionalidad. En concreto:

- exploraste cómo llegan al agente las instrucciones del repositorio y las instrucciones con ámbito de ruta;
- actualizaste las instrucciones para asegurar que se sigan los estándares;
- observaste el efecto de los archivos de instrucciones en el código.

A continuación, personalizarás y ejecutarás la habilidad reutilizable `quality-checks` para asegurarte de que se ejecutan las pruebas y el lint de forma coherente.

## Recursos

- [Añadir instrucciones personalizadas para Copilot CLI][instruction-files]
- [Compatibilidad con instrucciones personalizadas][custom-instructions-support]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Awesome Copilot: una colección de instrucciones y otros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
