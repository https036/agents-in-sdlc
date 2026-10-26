---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado desde la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de realizar todo el trabajo por tu cuenta, y para automatizar tareas repetitivas. Ya tienes Node.js instalado y una copia del proyecto preparada; ahora instalarás la aplicación y conectarás ese repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión;
- añadirás a la aplicación tu proyecto desde el repositorio de GitHub;
- conocerás el espacio de trabajo y el backlog que preparó la plantilla;
- probarás un chat rápido para conocer la aplicación.

## Escenario

Tu equipo está adoptando agentes de IA para abordar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar para dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y fusionar pull requests. En esta lección instalarás y conectarás la aplicación, y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instalar y configurar la aplicación GitHub Copilot

La aplicación está disponible para Windows, macOS y Linux. Instálala, autentícate y añade el repositorio de Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga e instala la aplicación para tu plataforma siguiendo las instrucciones de la página.
3. Abre la aplicación.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para autenticarte. Si usas GitHub Enterprise Cloud con residencia de datos, selecciona **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se solicite. GitHub Enterprise Server no es compatible.
5. Si el proceso inicial permite seleccionar repositorios, selecciona el repositorio Tailspin Toys que creaste desde la plantilla. También puedes omitir este paso y añadirlo después.
6. Elige un tema cuando se te solicite y termina la configuración inicial.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no tienes que añadirlo otra vez.
8. Si aún no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, **Add GitHub repository**. Introduce el propietario y el nombre del repositorio que anotaste en el ejercicio de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En este diseño, las carpetas de repositorio, como `tailspin-toys`, se agrupan en **Sessions** y sus sesiones aparecen debajo.

## Conocer el espacio de trabajo

Con el proyecto conectado, familiarízate con las áreas de la barra lateral:

- **New**: inicia una sesión de chat con Copilot.
- **Pull requests**: explora y filtra tus pull requests, consulta el estado de CI y revísalas mediante la integración nativa con GitHub.
- **Issues**: explora y filtra incidencias e inicia una sesión a partir de una de ellas.
- **Automations**: tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para gestionar listas de tareas, realizar mantenimiento periódico o delegar tareas repetitivas. El repaso las presenta como siguiente paso, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, complementos, habilidades y otros componentes. Lo usarás para configurar Playwright MCP.
- **Chats**: conversaciones ligeras para preguntas e ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar tu primer chat. Si no aparece, selecciona **+** junto a **Sessions** para iniciar un chat.
- **Sessions**: agrupa las carpetas de repositorio y sus sesiones de agente. Selecciona un repositorio, como `tailspin-toys`, para abrir su vista, o una sesión para volver a esa conversación. Cada sesión usa un espacio de trabajo aislado, así que puedes ejecutar varias sin que sus cambios entren en conflicto. Iniciarás la primera al añadir las puntuaciones por estrellas.

Explorarás el espacio de trabajo a lo largo del taller.

> [!TIP]
> Si tienes dudas sobre cómo hacer algo o si es posible, pregunta a Copilot; te ayudará a orientarte.

### Localizar el backlog inicial

La plantilla creó el backlog de Tailspin Toys al crear el repositorio.

1. En la barra lateral, selecciona la carpeta del repositorio Tailspin Toys dentro de **Sessions**, no una sesión concreta. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, sin dar por hecho sus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también permite iniciar una sesión de agente. Partirás de la incidencia de filtrado después de completar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** muestran automáticamente los elementos de los repositorios que has añadido a la aplicación Copilot. Para ver el trabajo de otros repositorios, añádelos a la aplicación.

## Probar un chat rápido

Los chats rápidos permiten formular preguntas o plantear ideas sin crear una rama ni un worktree. Son adecuados para consultas breves que no necesitan una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si aún no aparece **Chats**, selecciona **+** junto a **Sessions** para iniciar tu primer chat; después aparecerá **Chats**.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   ¿Cómo usa worktrees la aplicación GitHub Copilot?
   ```

3. Lee la respuesta. Verás que cada sesión utiliza su propio worktree aislado de Git, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar un chat nuevo cuando quieras.

## Resumen y siguientes pasos

Has instalado la aplicación GitHub Copilot, conectado tu proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub;
- añadir un proyecto desde su repositorio de GitHub;
- conocer el espacio de trabajo y localizar el backlog en **Issues**;
- usar un chat rápido para una consulta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] para mostrar las puntuaciones en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Primeros pasos con la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
