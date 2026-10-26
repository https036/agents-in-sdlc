---
title: "Lección 0 - Requisitos previos"
description: "Prepara las lecciones de la aplicación GitHub Copilot: instala Node.js para Tailspin Toys y crea tu propia copia del repositorio a partir de la plantilla."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La aplicación GitHub Copilot es una aplicación de escritorio que funciona como centro de operaciones para Copilot y GitHub. Ofrece acceso rápido a incidencias y pull requests y permite desarrollar con GitHub Copilot. Durante este taller trabajarás en local con la aplicación Tailspin Toys, creada con Astro, mediante la aplicación GitHub Copilot. Antes de empezar, instala Node.js en el equipo y, después, la aplicación Copilot.

En esta lección:

- instalarás Node.js para poder ejecutar las pruebas del proyecto en el equipo;
- crearás tu propia copia del proyecto Tailspin Toys a partir de la plantilla.

## Instalar Node.js

En varias lecciones pedirás a un agente que cree funcionalidades y ejecute el conjunto de pruebas de Tailspin Toys en local. Para ello necesitas **[Node.js][nodejs]**, el único entorno de ejecución que requiere el proyecto. Instala la versión **LTS** actual.

La opción más sencilla para cualquier plataforma es el instalador oficial:

1. Abre un terminal en tu sistema operativo, por ejemplo, Windows Terminal o Terminal de macOS.
2. Ejecuta estos comandos para comprobar las versiones instaladas:

    ```shell
    node --version
    npm --version
    ```

3. Si ambos comandos se ejecutan correctamente y Node.js cumple los requisitos del README y de `package.json` del proyecto, pasa a la sección siguiente.

> [!TIP]
> Solo tienes que completar estos pasos si aún no tienes Node instalado o necesitas actualizarlo.

4. Abre la [página de descarga de Node.js][node-download].
5. Descarga la versión **LTS** correspondiente a tu sistema operativo.
6. Ejecuta el instalador y acepta las opciones predeterminadas. En Windows, deja seleccionada la opción **Add to PATH**.
7. Después de instalarlo, abre un terminal nuevo.
8. Confirma la instalación en el terminal nuevo:

    ```bash
    node --version
    npm --version
    ```

9. Deberían aparecer las versiones instaladas de Node.js y npm.

> [!IMPORTANT]
> El agente de la aplicación Copilot también debe poder ejecutar `node --version` y `npm --version`. Que funcionen en tu terminal habitual no garantiza que estén disponibles en la aplicación. Cierra la aplicación por completo y vuelve a abrirla después de instalar Node.js o cambiar `PATH`. Si alguno de los comandos sigue fallando en una sesión, sigue los [pasos para solucionar problemas del entorno de sesión][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree también necesita las dependencias del proyecto y Chromium de Playwright para las comprobaciones E2E. Sigue el README del repositorio Tailspin Toys al preparar un worktree y revisa cualquier solicitud de instalación antes de aprobarla.

## Configurar el repositorio del laboratorio

Trabajarás con tu propia copia del proyecto Tailspin Toys. Créala a partir del [repositorio de plantilla][template-repository]. El nuevo repositorio incluye todos los archivos del laboratorio y lo conectarás al instalar la aplicación.

1. En una ventana nueva del navegador, ve al repositorio de GitHub del laboratorio: `https://github.com/github-samples/tailspin-toys`.
2. Crea tu copia seleccionando **Use this template** en la página del repositorio. Después, selecciona **Create a new repository**.

    ![Botón Use this template con la opción Create a new repository seleccionada en el menú desplegable](../../../_images/app-0-use-template.png)

3. Si participas en un evento dirigido por GitHub o Microsoft, sigue las indicaciones de los mentores. De lo contrario, puedes crear el repositorio en una organización donde tengas acceso a GitHub Copilot.

    ![Formulario Create a new repository con github-samples/tailspin-toys como plantilla y el nombre del repositorio completado](../../../_images/app-0-create-repository.png)

4. Anota la ruta del repositorio que has creado (**organization-or-user-name/repository-name**); la necesitarás más adelante.

> [!NOTE]
> Al crear el repositorio desde la plantilla, se genera automáticamente un backlog de incidencias de GitHub. Trabajarás con ellas durante el taller; no necesitas crear ninguna.

Usa una copia nueva de la plantilla del taller. Incluye instrucciones del repositorio, código de la aplicación, pruebas, la habilidad `quality-checks` y una extensión de lienzo existente. Personalizarás la habilidad y crearás un agente de QA durante el taller. Si usas una copia anterior, confirma con quien imparte el taller que contiene los archivos necesarios.

## Resumen y siguientes pasos

Ya tienes el entorno preparado. En esta lección:

- instalaste Node.js para que el proyecto se pueda compilar y probar en el equipo;
- creaste tu propia copia del repositorio Tailspin Toys a partir de la plantilla.

A continuación, [instalarás la aplicación GitHub Copilot][next-lesson], conectarás el repositorio que acabas de crear y conocerás el espacio de trabajo.

## Recursos

- [Descargar Node.js][node-download]
- [Crear un repositorio a partir de una plantilla][template-repository]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#iniciar-una-sesion-desde-la-incidencia
