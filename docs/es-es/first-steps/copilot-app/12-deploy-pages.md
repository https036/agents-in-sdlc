---
title: "Lección 12 - Desplegar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su pull request y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Dale al Space Quiz un espacio público desplegándolo con GitHub Pages. Copilot puede preparar el flujo de trabajo y guiar el despliegue, pero tú eres responsable de revisar los cambios y confirmar que el sitio está publicado.

En esta lección:

- pedirás a Copilot que configure el despliegue en GitHub Pages;
- revisarás el flujo de trabajo y la pull request antes de fusionarla;
- seguirás el despliegue y comprobarás el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero aún hace falta una forma sencilla de probarlo. Publicar el sitio de un solo archivo con GitHub Pages permite acceder a él mediante una URL pública, sin añadir un servidor ni dependencias de ejecución.

## Pedir a Copilot que despliegue el sitio

En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

```plaintext
Habilita GitHub Pages en este repositorio. Crea un flujo de trabajo de GitHub Actions que compile el sitio web si hace falta y lo despliegue en GitHub Pages. Si aún no existe un archivo README, añade uno que describa la aplicación y explique cómo ejecutarla en local. Crea una pull request con estos cambios y fusiónala cuando pasen las comprobaciones obligatorias. Supervisa el despliegue hasta que finalice correctamente y, después, indica la URL final del sitio publicado. Si no tienes permiso para habilitar Pages o fusionar los cambios, dime exactamente qué debo aprobar o hacer; no informes de que la tarea ha terminado hasta que el sitio esté publicado.
```

![Diagrama de tres etapas: una pull request de space-quiz fusionada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

> [!WARNING]
> Este prompt pide al agente que cree y fusione una pull request y cambie la configuración de Pages del repositorio. Revisa las diferencias, los permisos de despliegue y cualquier solicitud de aprobación de GitHub antes de continuar.

## Revisar el flujo de trabajo y comprobar el despliegue

1. Confirma que el archivo README solo se añade si el repositorio aún no tiene uno.
2. Revisa los cambios de la pull request, incluidos los pasos de compilación y despliegue del flujo de trabajo.
3. Fusiona los cambios solo cuando hayan pasado las comprobaciones obligatorias de la pull request.
4. Sigue la ejecución del despliegue de GitHub Actions hasta que termine correctamente.
5. Abre la URL indicada y confirma que se carga el Space Quiz.

Si el agente no puede habilitar Pages o fusionar la pull request, sigue las instrucciones concretas que te proporcione. No des la tarea por terminada hasta que el despliegue finalice correctamente y el sitio publicado se cargue.

## Resumen y siguientes pasos

El Space Quiz ya está disponible como sitio publicado y has comprobado el despliegue en lugar de confiar únicamente en el informe del agente. Continúa con la [Lección 13: Revisión y próximos pasos][next-lesson].

[next-lesson]: ../13-review/
