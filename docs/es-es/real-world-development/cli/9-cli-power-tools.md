---
title: "Lección 9 - Comandos útiles de GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como muchas herramientas de CLI, GitHub Copilot CLI incluye numerosos comandos de barra. Estos permiten acceder a funciones avanzadas, información interna y opciones de configuración. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. En esta lección explorarás otros comandos útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usarás `/model` para explorar los modelos disponibles;
- aprenderás cómo `/share` puede exportar o compartir una sesión;
- explorarás comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar en el agente en la nube.

## Escenario

Ya has completado el flujo de trabajo principal de la CLI. Ahora explorarás otras capacidades: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar tareas opcionalmente en [Copilot cloud agent][about-cloud-agent].

## Explorar el contexto de Copilot CLI

En tareas largas o complejas, puedes acercarte al límite de la ventana de contexto del modelo. Copilot CLI compacta automáticamente la conversación cuando es necesario; también puedes inspeccionar o compactar el contexto con comandos de barra.

1. Vuelve a tu Codespace e inicia Copilot CLI desde la raíz del repositorio, si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Observa el modelo, el uso actual de tokens y cómo se distribuye el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Vuelve a ejecutar `/context` y compara el resultado. Puede que no haya una diferencia notable si la conversación aún es breve.

> [!NOTE]
> Copilot CLI compacta automáticamente el contexto cuando se llena la ventana. Usa `/compact` para elegir cuándo hacerlo. Usa `/clear` o `/new` si quieres empezar una conversación nueva para una tarea distinta.

## Elegir un modelo

Los modelos tienen fortalezas distintas y cada desarrollador puede tener sus preferencias. Copilot CLI permite mostrar los modelos y seleccionar el que quieras usar.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información de uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Compartir lo que se aprende ayuda a todo el equipo. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartirla o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en tu versión.
2. Si quieres compartir la sesión, elige un destino adecuado, como `/share file` para crear un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo a alguien o publicarlo. La exportación puede incluir prompts, respuestas y detalles del proyecto.

La publicación de un enlace o gist es opcional. No publiques contenido del repositorio o de la conversación que el equipo no quiera compartir.

## Opcional: ampliar o delegar el trabajo

El flujo principal del taller ha terminado. Copilot CLI también ofrece comandos para tareas más amplias:

- `/fleet` puede dividir tareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un árbol de trabajo Git aislado para otra tarea.
- `/delegate` puede enviar una tarea a Copilot cloud agent, que trabaja de forma asíncrona y puede abrir una pull request.

Estos comandos son opcionales porque pueden crear árboles de trabajo adicionales o iniciar tareas remotas. Antes de probarlos, inicia una tarea nueva y bien delimitada; después, revisa el resultado con el flujo de trabajo habitual. Para saber más sobre el trabajo asíncrono con agentes, continúa con el [taller de Cloud agent][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra de Copilot CLI permiten configurar la herramienta, compartir sesiones y observar lo que ocurre internamente. En esta lección:

- usaste `/context` y `/compact` para explorar la gestión del contexto;
- usaste `/model` para explorar los modelos disponibles;
- aprendiste cómo `/share` puede exportar o compartir una sesión;
- exploraste comandos opcionales para trabajar en paralelo, usar árboles de trabajo y delegar tareas.

Hay muchos más comandos de barra por descubrir. Para cerrar el recorrido, [repasarás lo aprendido][next-lesson] y los próximos pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca de Copilot cloud agent][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
