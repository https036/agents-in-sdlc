---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan dificultades con la deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad que llevan tiempo en el backlog porque nunca hubo tiempo para implementarlas. El agente en la nube de GitHub Copilot puede actualizar código y añadir funcionalidades de forma autónoma. Cuando termina, crea una pull request (PR) en borrador para que la revise un desarrollador. Así se pueden delegar tareas tediosas, acelerar el desarrollo y dejar que los desarrolladores se centren en objetivos más amplios.

Con el agente en la nube explorarás cómo:

- personalizar el entorno de generación de código;
- asegurarte de que las operaciones se realizan de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Escenarios

Tailspin Toys quiere abordar parte de su deuda técnica. La documentación que dejaron los contratistas que crearon la primera versión del sitio está en mal estado: descubrirás que no existe. Como primer paso, quieren añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere que la página de detalles de cada juego muestre otros títulos de la misma categoría para que las personas puedan seguir explorando. Todavía no necesita una implementación pulida; quiere algo que le permita probar la experiencia de usuario. Es un bloqueo, aunque hay otras incidencias de mayor prioridad.

Estas tareas suelen quedar relegadas y son buenas candidatas para asignarlas al agente en la nube de Copilot. El agente puede trabajar en ellas de forma asíncrona mientras el desarrollador se ocupa de otras tareas, y después revisar el resultado.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Igual que con una persona, puedes asignarle trabajo de varias maneras, por ejemplo, [asignándole una incidencia de GitHub](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Al recibirla, Copilot crea una pull request en borrador para registrar el progreso, prepara un entorno y empieza a trabajar. Puedes consultar la sesión mientras está activa o cuando haya terminado. Cuando la solución propuesta esté lista para que la revises, Copilot te mencionará en la pull request.

## La importancia de acotar bien las instrucciones

GitHub Copilot no hace magia: no permite dejar de pensar. Incluso las tareas que parecen sencillas pueden tener muchas complicaciones, así que presta atención a cómo defines el alcance de las tareas para el [agente en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por incorporar IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has aprendido cómo guían a Copilot las instrucciones personalizadas. Si ya has completado el ejercicio práctico de instrucciones personalizadas, has visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar trabajo al agente en la nube, revisa en modo de solo lectura los archivos de instrucciones incluidos en el repositorio. Así podrás identificar su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace activo:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, sobre todo las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que limita la aplicación de estas instrucciones a los archivos `**/*.test.ts`.

Cuando en la siguiente sección asignes al agente la incidencia **Code lacks documentation**, observa si la pull request resultante incluye comentarios TSDoc, sigue las convenciones de TypeScript y añade encabezados de comentario. Estas directrices proceden de los archivos de instrucciones, que volverás a mencionar al revisar las PR en un ejercicio posterior.

## Preparar el entorno de desarrollo del agente en la nube de Copilot

La creación de código suele requerir un entorno específico y la ejecución de scripts de configuración. Lo mismo ocurre al asignar tareas a Copilot, que trabaja de manera parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] como entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow] en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece el trabajo. Así el agente dispone de las herramientas y dependencias necesarias. Este flujo ya está configurado para el laboratorio: instala Node.js y las dependencias, los binarios del navegador para las pruebas end-to-end y prepara la base de datos SQLite de la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar estos pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Asigna los permisos mínimos necesarios para estos pasos. Copilot recibe su propio token para realizar sus operaciones.
    permissions:
      # Si los pasos de configuración clonan el repositorio, por ejemplo para instalar dependencias, necesitan el permiso contents: read. Si no lo clonan, Copilot lo hará automáticamente al finalizar los pasos.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración de la interfaz y la aplicación: Node.js (la aplicación completa ahora usa Astro y Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga la base de datos SQLite local para que la compilación y las pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

Como en cualquier otro flujo de trabajo de GitHub, hay algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`, que se ejecuta en GitHub Actions antes de que Copilot empiece a trabajar en la pull request.
- El evento `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Es útil para comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añadir documentación

Aunque todo el mundo reconoce la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica ralentiza el trabajo y dificulta el mantenimiento del código y la incorporación de desarrolladores. Copilot es especialmente útil para crear documentación, así que esta es una buena incidencia para asignarle al agente en la nube. Trabajará en segundo plano y volverás a revisar el resultado en un ejercicio posterior.

1. Abre una pestaña nueva del navegador y ve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de creación de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. En **Description**, escribe:

    ```plaintext
    Nuestra organización exige que las funciones y los métodos incluyan comentarios de documentación TSDoc cuando sean útiles. Por desgracia, las actualizaciones recientes no han seguido este estándar. Hay que actualizar el código existente para incluir comentarios de documentación cuando aclaren el comportamiento.
    ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

  ![Detalles de la asignación de Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecciona la pestaña **Pull Requests**.
11. Abre la pull request recién creada. Su título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la ventana.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando el entorno (ejecutando el flujo de trabajo anterior), analizando el proyecto y determinando el mejor enfoque.

13. Revisa la lista de tareas y el trabajo que Copilot piensa realizar.
14. Desplázate por la cronología de la pull request. Deberías ver una actualización que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la ventana para ver el indicador actualizado.

16. Observa la sesión en directo y cómo Copilot resuelve el problema: explora el código, entiende su estado, se detiene a pensar en el plan adecuado y crea código.

El proceso puede tardar varios minutos. Una de las ventajas principales del agente en la nube es que trabaja de forma asíncrona y te permite centrarte en otras tareas. Aprovecharemos esa característica asignándole otra tarea y, mientras tanto, escribiremos código para añadir funcionalidad a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas del agente en la nube es poder repartir el trabajo: tú te ocupas de unas tareas mientras Copilot trabaja en otras. Añadir una sección de juegos relacionados quizá no lleve mucho tiempo, pero ese tiempo puede dedicarse a otras tareas. ¡Asignémosela al agente!

1. Vuelve a tu repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue**.
4. Selecciona **Blank issue**.
5. Escribe `Show related games on the game details page` en **Title**.
6. En **Description**, escribe:

    ```markdown
    Queremos ayudar a descubrir más juegos mostrando otros títulos relacionados en la página de detalles de cada juego. El equipo de diseño quiere explorar la experiencia de usuario y realizar algunas pruebas de aceptación. Estos son los requisitos:

   - Añadir en `src/lib/` un helper de acceso a datos que devuelva los demás juegos de la misma categoría que un juego dado, sin incluir ese juego.
   - Mostrar en la página de detalles del juego una sección «Related games» que use el helper.
   - Gestionar el caso en que un juego no tenga otros juegos relacionados.
   - Crear pruebas unitarias para el nuevo helper.
   - Antes de crear la PR, comprobar que pasan todas las pruebas.
    ```

7. Selecciona **Create**.
8. En el lado derecho, selecciona **Assign to Copilot**.

  ![Asignar Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás unos 👀 en el primer comentario de la incidencia, que indican que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en tu segunda solicitud. Como el agente en la nube trabaja de forma parecida a un ingeniero de software, no hace falta supervisarlo activamente; podrás revisar el resultado cuando termine. Ahora crearás y usarás agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje de forma asíncrona. Copilot puede ayudarte a abordar deuda técnica, crear funcionalidades o migrar código entre frameworks.

Has explorado estos conceptos:

- personalizar el entorno para generar código;
- asegurarte de que las operaciones se realicen de forma segura;
- definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente trabaja en segundo plano, crearás y usarás agentes personalizados. [El agente en la nube también puede usar servidores MCP][cloud-agent-mcp] y las instrucciones personalizadas que exploraste en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
