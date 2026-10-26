---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando activas el modo Agent de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como todo ocurre en tu máquina y en tiempo real, Copilot Chat en VS Code muestra cada llamada a herramientas y cada edición. Puedes revisar las diferencias, aceptar o rechazar cambios y orientar la conversación con prompts de seguimiento sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás las ediciones propuestas;
- orientarás al agente durante la sesión para añadir un control de modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que utilizaste en el [ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño solicita también un modo claro para quienes consideran demasiado oscuro el tema predeterminado. En lugar de empezar desde cero, ampliarás la sesión existente.

## Supervisar el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en el chat con sus diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del ejercicio 4.
2. Mientras trabaja el agente, recorre el chat para observar las llamadas a herramientas, lecturas de archivos, ediciones y comandos del terminal.
3. Cuando proponga una edición, verás las diferencias en el chat:
   - selecciona **Keep** para aceptar el cambio;
   - selecciona **Undo** para revertirlo y permitir que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados en esta sesión.

> [!NOTE]
> Si una herramienta solicita permiso, por ejemplo, para ejecutar un comando del terminal, el agente se detendrá y mostrará una indicación **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Al final debería resumir los cambios.

## Pedir al agente que añada el modo claro

Ahora que el modo de alto contraste está implementado, ampliarás la conversación para añadir un control de modo claro. Al continuar en el mismo chat, el agente conserva el contexto de su trabajo: los componentes modificados, el enfoque de estilos y el uso del almacenamiento local.

1. En el cuadro de entrada de Copilot Chat, envía este prompt:

    ```plaintext
    ¡Buen trabajo! Ahora añade un control para el modo claro junto al de alto contraste. Sigue el mismo patrón: conserva la selección en el almacenamiento local y permite cambiar el modo desde la misma zona de la interfaz. El usuario debe poder activar el alto contraste y el modo claro de forma independiente.
    ```

2. El agente retomará el trabajo, explorará los cambios existentes y propondrá modificaciones para el modo claro.
3. Revisa cada cambio cuando aparezca. Usa **Keep** o **Undo** como antes.
4. Si el agente toma una dirección que no quieres —por ejemplo, intenta reorganizar los controles de una forma que no te convence—, detenlo con **Stop** junto al cuadro de entrada y envía una aclaración.

> [!TIP]
> Orientar una sesión activa es más rápido que empezar de nuevo. Envía prompts de seguimiento para pedir ajustes («simplifica el CSS; usa variables en lugar de duplicar selectores»), corregir el enfoque («integra el modo claro en el panel de configuración existente, no añadas un tercer control») o ampliar el alcance («añade también atajos de teclado para ambos controles»).

## Revisar los cambios

Antes de confirmar el trabajo, revisa los archivos modificados.

1. Abre **Source Control** en VS Code.
2. Revisa la lista completa de archivos. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos modificados y revisa las diferencias. Confirma que se aplican las prácticas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la lección siguiente][next-lesson].

## Resumen y siguientes pasos

Has usado el modo Agent de Copilot Chat para supervisar y orientar una sesión activa: has revisado cada edición, aceptado o rechazado cambios y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- supervisaste el trabajo del agente en tiempo real;
- aceptaste y rechazaste ediciones individuales;
- orientaste la sesión con prompts de seguimiento.

En la [lección siguiente][next-lesson] publicarás el trabajo de accesibilidad como pull request y revisarás los cambios generados por el agente personalizado local.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo Agent][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
