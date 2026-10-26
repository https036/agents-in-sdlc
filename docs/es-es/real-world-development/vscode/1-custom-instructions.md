---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave al trabajar con IA generativa. Si una tarea debe hacerse de una forma concreta, asegúrate de que Copilot disponga de esas directrices. Los [archivos de instrucciones][instruction-files] te permiten proporcionar ese contexto sobre la generación de código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto específico del proyecto, las directrices de programación y los estándares de documentación mediante instrucciones personalizadas del repositorio y archivos con ámbito de ruta;
- generarás el primer bloque de datos para el filtrado (un helper de editoriales) con las instrucciones *actuales*;
- añadirás un nuevo estándar general del repositorio a `.github/copilot-instructions.md`;
- volverás a enviar el mismo prompt y observarás cómo el código generado adopta el nuevo estándar;
- confirmarás los cambios de instrucciones y el helper en `main` para que Copilot pueda usar las directrices actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede desviarse de algunos estándares. Copilot no es determinista. El objetivo es observar la *tendencia* del comportamiento después de actualizar las instrucciones, no conseguir una salida idéntica carácter por carácter.

## Archivos de instrucciones

### Escenario

Tailspin Toys tiene un conjunto de directrices y requisitos para sus prácticas de desarrollo:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- El código debe incluir comentarios de documentación TSDoc.
- Al principio de cada archivo debe haber un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot disponga de la información necesaria para seguir estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan contexto y preferencias para que Copilot entienda mejor tu estilo de programación y tus requisitos. Puedes especificar convenciones, bibliotecas y tipos de comentarios preferidos. Puedes crear instrucciones para todo el repositorio o para tipos de archivo concretos.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información general del proyecto relevante para la mayoría de las solicitudes, como la pila tecnológica, una descripción del proyecto, buenas prácticas y directrices globales.
- `.github/instructions/*.instructions.md`: archivos que puedes crear para tareas o tipos de archivo concretos. Sirven para proporcionar directrices para lenguajes como TypeScript o Astro, o tareas como crear componentes de interfaz o pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para las finalizaciones de código ni las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Copilot también [admite otros estándares para los archivos de instrucciones][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar los archivos de instrucciones

Los ejemplos del proyecto muestran un enfoque representativo:

- Mantén `copilot-instructions.md` centrado en las directrices generales del proyecto: qué se está construyendo, la estructura del proyecto y los estándares globales de programación.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivo (pruebas unitarias, componentes Astro, capa de datos) o tareas.
- Usa lenguaje natural y directrices claras. Incluye ejemplos de cómo debería ser el código y de cómo no.

No hay una única forma correcta de crear instrucciones ni de usar IA. La experimentación te ayudará a descubrir qué funciona mejor para tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Al explorar los de este proyecto, puede que encuentres archivos para tareas como [actualizaciones de interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece una experiencia distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección correspondiente lo explicará cuando proceda.
>
> Para encontrar plantillas o un punto de partida, explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar los archivos de instrucciones personalizadas del proyecto

Lee los archivos de instrucciones incluidos en el repositorio antes de hacer cambios.

1. Abre `.github/copilot-instructions.md`.
2. Fíjate en la descripción breve del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa la guía anidada **GitHub Actions Workflows**. Se aplica a las interacciones con Copilot.
3. Explora `.github/instructions`. Encontrarás instrucciones para Astro, la capa de datos de Drizzle, pruebas y otros temas.
4. Abre `.github/instructions/unit-tests.instructions.md`. El campo `applyTo` de la parte superior establece un patrón relativo a la raíz del repositorio que determina a qué archivos se aplican las instrucciones. En este caso, coincide con archivos de pruebas TypeScript, como `**/*.test.ts`.
5. Revisa las instrucciones para crear pruebas unitarias en este proyecto.
6. Abre `.github/instructions/drizzle.instructions.md` y revisa los enlaces a otros archivos de instrucciones y a archivos del proyecto. Así puedes dividir instrucciones extensas en archivos pequeños y reutilizables y mostrar a Copilot ejemplos que debe seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero aún no exige documentación dentro del código. En los pasos siguientes añadirás reglas para los comentarios TSDoc y los encabezados de comentario.

## Crear una rama para los cambios

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal nuevo pulsando <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usar Copilot Chat *antes* de actualizar las instrucciones

Para observar el efecto de las instrucciones personalizadas, primero enviarás un prompt con las instrucciones actuales. Después de actualizarlas, enviarás el mismo prompt de nuevo.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando sea necesario.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot use solo el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ir el helper.
3. En el menú desplegable de agentes de Chat, selecciona **Agent** para que Copilot pueda modificar archivos.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios en el archivo.
   ```

5. Copilot explorará el proyecto y aplicará cambios, a menudo en el helper y sus pruebas.
6. Observa que el helper propuesto es una función tipada que recibe un cliente `db` como primer argumento y devuelve un array tipado de editoriales. Estas características proceden de las convenciones de la capa de datos de `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que el código propuesto **no incluye** comentarios TSDoc ni un encabezado de comentario a nivel de archivo.

> [!CAUTION]
> Copilot es probabilístico, así que puede que añada comentarios TSDoc aunque no se lo pidas. Si ocurre, no pasa nada: el objetivo sigue siendo observar la mejora de *consistencia* después de actualizar las instrucciones.

## Añadir un estándar del repositorio

Modifica `.github/copilot-instructions.md` para documentar los estándares de programación y mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Localiza la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Aún no tiene una regla sobre documentación dentro del código.
3. Añade estas líneas de Markdown debajo de los estándares existentes:

   ```markdown
   - Cada función exportada debe incluir un comentario TSDoc que describa su propósito, sus parámetros y su valor de retorno.
   - Antes de las importaciones o de cualquier código, añade al archivo un bloque de comentarios que explique su propósito.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Las instrucciones del repositorio (`.github/copilot-instructions.md`) son el lugar adecuado para los estándares generales, mientras que `*.instructions.md` sirve para lenguajes, tipos de archivo o tareas concretas.

## Repetir el prompt y observar el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para limpiar la conversación.
2. Abre `src/lib/publishers.ts` para centrar el contexto de Copilot en el archivo.
3. Envía el mismo prompt:

   ```plaintext
   Crea o actualiza src/lib/publishers.ts con un helper de acceso a datos que devuelva una lista de todas las editoriales, con el nombre y el id de cada una. Aplica los cambios en el archivo.
   ```

4. Observa que el archivo propuesto empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Helpers de acceso a datos de editoriales para la plataforma de crowdfunding de Tailspin Toys.
    * Proporciona funciones para recuperar información de editoriales desde la base de datos.
    */
   ```

5. Observa que la función propuesta incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Devuelve una lista de todas las editoriales con su id y su nombre.
    *
    * @param db - El cliente de base de datos de Drizzle.
    * @returns Una promesa que se resuelve en un array de objetos de editoriales.
    */
   ```

Has orientado a Copilot para que siga un estándar nuevo y lo aplique a código real que servirá de base para el ejercicio siguiente.

## Confirmar las instrucciones y enviar la rama

1. Abre un terminal nuevo con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Confirma que están presentes los cambios de instrucciones y del helper:

   ```bash
   git status
   ```

3. Prepara y confirma los cambios:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios auxiliares, por ejemplo, pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Envía la rama al repositorio:

   ```bash
   git push -u origin custom-instructions
   ```

## Crear y fusionar una pull request

Con la rama enviada, crea una pull request y relaciónala con la incidencia del backlog sobre los estándares de documentación. Copilot puede buscar la incidencia, crear una PR que la cierre y fusionar la solicitud.

> [!NOTE]
> En este ejercicio Copilot actuará por primera vez en GitHub. La plantilla del proyecto ya configura las herramientas de GitHub para el espacio de trabajo, así que no tienes que configurarlas. La primera vez que Copilot use una herramienta, VS Code quizá te pida que inicies sesión en GitHub.

1. Abre Copilot Chat en el Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir Copilot Chat y confirma que **Agent** está seleccionado.
3. Pide a Copilot que encuentre la incidencia sobre la actualización de las instrucciones y cree una PR desde la rama actual. Indica que debe describir tanto las actualizaciones de instrucciones como la base del helper:

   ```
   Busca la incidencia relacionada con la actualización del archivo de instrucciones. Crea una PR desde la rama actual, indica que cierra esa incidencia y menciona que también hemos añadido la base del helper de editoriales para el trabajo de filtrado que viene después.
   ```

4. Cuando Copilot te pida permiso para realizar acciones de GitHub, revisa la operación y selecciona **Allow** si procede.
5. Cuando se cree la PR, pide a Copilot que la fusione y vuelva a `main`:

   ```
   Fusiona la PR con main. Después, vuelve localmente a main y descarga el código más reciente para tenerlo actualizado.
   ```

6. Cuando Copilot te pida permiso para realizar acciones de GitHub y ejecutar comandos del shell, revisa las operaciones antes de aprobarlas.

¡Ya has creado y fusionado una pull request con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene contexto de los archivos de instrucciones y has usado Copilot Chat en VS Code para:

- enviar un prompt de generación de código y observar el resultado con las instrucciones *existentes*;
- añadir un nuevo estándar general del repositorio a `.github/copilot-instructions.md`;
- repetir el prompt y observar cómo el código propuesto adopta el nuevo estándar;
- confirmar las instrucciones y la base del helper en `main` para que el siguiente ejercicio pueda continuar el trabajo.

A continuación, aplicarás estas instrucciones en el [modo de agente][next-lesson] mientras Copilot desarrolla una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [5 consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
