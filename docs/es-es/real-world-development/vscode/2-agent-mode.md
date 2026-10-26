---
title: "Ejercicio 2 - Añadir funcionalidad con el modo Agent de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso una actualización sencilla de una aplicación suele requerir modificar varios archivos y ejecutar tareas como las pruebas. El flujo de trabajo puede implicar localizar los archivos necesarios, hacer cambios, ejecutar las pruebas, depurar, descubrir qué archivo falta y volver a modificar el código. La lista puede alargarse.

El modo Agent de Copilot puede ayudar con ese trabajo.

El modo Agent está diseñado para actuar con más autonomía dentro del IDE. Explora la estructura del proyecto, hace las modificaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Explorarás cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque sus nombres se parecen, el modo Agent y el agente en la nube ofrecen experiencias distintas. El modo Agent trabaja en el IDE para permitir ciclos rápidos de comentarios y ajustes. El agente en la nube trabaja de forma asíncrona, como otro miembro del equipo, y se comunica mediante incidencias y pull requests.

En este ejercicio aprenderás a:

- usar el modo Agent de Copilot para explorar el proyecto, encontrar archivos relevantes y coordinar cambios;
- implementar funcionalidades en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generadas antes de fusionarlos.

## Escenario

El catálogo de juegos está creciendo y quieres que los usuarios puedan filtrar por categoría y editorial. En el ejercicio anterior ya añadiste un helper de editoriales. Ahora completarás el trabajo restante de la capa de datos, la interfaz y las pruebas con el modo Agent de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, explora el sitio web para conocer su funcionalidad actual.

Tailspin Toys es una plataforma de financiación colectiva para juegos de mesa con un tema de desarrollo de software. Permite mostrar juegos y sus detalles. Es una sola aplicación Astro que genera HTML estático al compilar. Las páginas consultan una base de datos SQLite local mediante Drizzle ORM desde el frontmatter; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Los helpers reutilizables de acceso a datos están en `src/lib/`, y la interactividad se implementa mediante pequeños elementos Astro `<script>` con las API DOM estándar.

### Iniciar el sitio web

La tarea `npm run dev` inicia el servidor de desarrollo de Astro. Para ejecutarlo en Codespaces:

1. Vuelve al Codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en Codespaces con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que pregunta si quieres abrir `http://localhost:4321` en el navegador, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar un enlace a localhost desde el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`, un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando se ejecute el sitio, explora sus funciones principales:

- **Página de inicio**: muestra juegos de mesa, sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los pasos siguientes, vuelve al Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación limpia. El modo y el modelo varían según el ejercicio; cada paso lo indicará cuando corresponda.

La primera versión del sitio funciona, pero queremos añadir capacidades. El backlog de incidencias se creó automáticamente al configurar el proyecto desde la plantilla. Pide a GitHub Copilot que muestre sus elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal e invoca herramientas.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** entre los modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por un modelo concreto. Durante su creación se usó Claude Sonnet 4.5, por lo que se incluye en las instrucciones para ofrecer una experiencia relativamente coherente. Los modelos de lenguaje son probabilísticos y pueden producir resultados distintos; es normal.

> [!NOTE]
> Por el carácter probabilístico de los modelos, Copilot quizá use un comando MCP distinto, pero debería poder completar la tarea.

3. Pide a Copilot que muestre las incidencias del backlog:

   ```plaintext
   Muéstrame el backlog de incidencias de mi repositorio de GitHub. Ayúdame a priorizarlas según cuáles serán más útiles para los usuarios.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot ha priorizado los elementos según lo que considera más útil para los usuarios.

## Revisar los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que le dará contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que describe cómo añadir funcionalidad al sitio.

1. En el Codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa sus directrices, que incluyen:
   - una descripción general de la arquitectura;
   - principios de diseño de componentes, capacidad de prueba y accesibilidad;
   - enlaces a instrucciones específicas para varios tipos de archivo, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden enlazar a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones, lo que permite reutilizar directrices, dividir instrucciones complejas y ofrecer ejemplos.

## Implementar el filtrado

Para completar el filtrado, habrá que hacer al menos tres cambios en la aplicación:

- añadir o completar la lógica de filtrado de la capa de datos en `src/lib/`;
- añadir o actualizar las pruebas de filtrado;
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Antes de fusionar los cambios, también deben ejecutarse correctamente las pruebas. El modo Agent puede realizar estas tareas.

1. Continúa en la conversación actual de Copilot o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** para añadir el archivo de instrucciones.

   ![Ejemplo de selección del archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú desplegable de agentes.

   ![Selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Actualiza el sitio para añadir filtros por editorial y categoría según los requisitos de la incidencia de GitHub correspondiente en el backlog. Ya existe un helper de editoriales del ejercicio anterior, así que consérvalo y ajústalo si hace falta mientras completas el resto de la capa de datos, la interfaz y las pruebas. Asegúrate de que todas las pruebas pasen antes de terminar. El servidor ya está en ejecución; no hace falta que lo inicies.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relevantes de la capa de datos, la interfaz y las pruebas. Después modificará los archivos y ejecutará las pruebas.

   ![Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot explorará, modificará archivos y ejecutará pruebas. El proceso puede tardar unos minutos, según la complejidad de la tarea y del código. Durante el trabajo quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, aparecerán para todos los cambios; no es necesario seleccionarlos mientras sigue trabajando.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Diálogo de Copilot Chat que solicita permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta que completa la tarea y no detecta errores.

   ![Sesión completa de Copilot Chat con el modo Agent](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Revisa siempre el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas confirmado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y siguientes pasos

En este ejercicio has usado el modo Agent de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido que puede:

- implementar funcionalidades en la interfaz y la capa de datos;
- explorar el proyecto, localizar archivos relevantes y coordinar cambios;
- generar cambios y pruebas que debes revisar antes de fusionarlos.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una pull request.

### Ejercicio adicional: implementar paginación

Al crecer la lista de juegos, puede ser necesario habilitar la paginación. Usa lo aprendido para pedir a Copilot que la añada al sitio. Considera estas cuestiones:

- sigue las buenas prácticas existentes y usa los archivos de instrucciones;
- decide si el usuario puede elegir el tamaño de página o si será fijo;
- proporciona a Copilot las directrices necesarias para implementar el comportamiento deseado;
- itera y aporta contexto adicional si hace falta; es una parte normal del trabajo con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo usarlos][choose-mode]
- [Modo Agent en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
