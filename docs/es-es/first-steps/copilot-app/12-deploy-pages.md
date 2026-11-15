---
title: "Lección 12 - Publicar el sitio con GitHub Pages"
description: "Usa la aplicación GitHub Copilot para configurar un flujo de trabajo de Pages, revisar su solicitud de incorporación de cambios y comprobar el Space Quiz publicado."
authors:
  - jamesmontemagno
lastUpdated: 2026-10-12
---

Publica el Space Quiz con GitHub Pages para que todo el mundo pueda acceder a él. Copilot puede preparar el flujo de trabajo y guiar la publicación, pero debes revisar los cambios y confirmar que el sitio está disponible.

En esta lección, aprenderás a:

- pedir a Copilot que configure una publicación en GitHub Pages.
- revisar el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarla.
- seguir la publicación y verificar el sitio publicado.

## Escenario

El Space Quiz ya está en un repositorio de GitHub, pero hace falta una forma sencilla de probarlo. Publicar el sitio, que consta de un solo archivo, con GitHub Pages permite acceder a él desde una URL pública sin añadir un servidor ni una dependencia de entorno de ejecución.

## Pide a Copilot que prepare la publicación

> [!WARNING]
> Crear una solicitud de incorporación de cambios puede confirmar y enviar código. El prompt solicita que se cree una, así que ten en cuenta la configuración de aprobación y el repositorio de destino. Si está seleccionada la opción **Approve all**, no esperes otro aviso de confirmación.

Para pedir a Copilot que prepare los archivos de publicación y abra una solicitud para revisar, sigue estos pasos:

1. En la sesión de la aplicación Copilot para `space-quiz`, envía este prompt:

   ```plaintext
   Prepara la publicación de este repositorio en GitHub Pages. Crea un flujo de trabajo de GitHub Actions que compile el sitio si es necesario y lo publique en GitHub Pages cuando los cambios lleguen a la rama predeterminada. Si el repositorio aún no tiene un archivo README, añade uno que describa la aplicación y cómo ejecutarla localmente. Crea una solicitud de incorporación de cambios con estos cambios e indica su enlace y cualquier configuración o permiso necesario de Pages. No combines la solicitud, no habilites la combinación automática, no cambies la configuración de Pages ni publiques el sitio. Detente y espera a que revise los cambios.
   ```

## Revisa el flujo de trabajo y la solicitud de incorporación de cambios

Antes de autorizar nada, comprueba que la solicitud solo contiene los cambios solicitados y que el flujo de trabajo publica desde la rama correcta.

1. Confirma que el archivo README solo se ha añadido si el repositorio no tenía ya uno.
2. Revisa los cambios de la solicitud, incluidos los pasos de compilación y publicación del flujo de trabajo, sus desencadenadores y sus permisos.
3. Confirma que el flujo de trabajo publica desde la rama predeterminada, no desde una solicitud de incorporación de cambios que aún no se ha combinado.
4. Pide las correcciones necesarias y revisa el diff actualizado. Espera a que se superen las comprobaciones obligatorias de la solicitud antes de autorizar la combinación.

## Autoriza la combinación y la publicación

Cuando hayas revisado la solicitud y se hayan superado sus comprobaciones, puedes autorizar por separado la combinación y la publicación.

> [!WARNING]
> El prompt autoriza al agente a combinar la solicitud revisada, cambiar la configuración de Pages del repositorio y publicar el sitio. Según la configuración de aprobación, estas acciones podrían realizarse sin otro aviso.

1. Sustituye `<PR_NUMBER>` por el número de la solicitud de incorporación de cambios que has revisado y envía este prompt por separado:

   ```plaintext
   He revisado la solicitud de incorporación de cambios <PR_NUMBER> de este repositorio y autorizo su combinación y la publicación. Habilita GitHub Pages mediante GitHub Actions, combina únicamente esa solicitud cuando se hayan superado las comprobaciones obligatorias y supervisa la publicación hasta que termine correctamente. Indica la URL final del sitio publicado. Si no tienes permiso para habilitar Pages o combinar la solicitud, dime exactamente qué debo aprobar o hacer. No indiques que se ha completado hasta que el sitio esté disponible.
   ```

![Diagrama en tres etapas: una solicitud de incorporación de cambios de space-quiz combinada; una ejecución correcta de Deploy website en GitHub Actions con los pasos Build website, Upload artifact y Deploy to Pages; y el Space Quiz publicado en yourname.github.io/space-quiz/.](../../../_images/first-steps-app-pages-deploy.svg)

## Verifica la publicación

Confirma que GitHub Actions ha completado el despliegue y que el sitio responde en la URL publicada.

1. Sigue la ejecución de publicación de GitHub Actions hasta que termine correctamente.
2. Abre la URL indicada y confirma que se carga el Space Quiz.
3. Si el agente no pudo habilitar Pages o combinar la solicitud, sigue las instrucciones concretas que te proporcione y repite la verificación. No des la tarea por terminada hasta que la publicación haya finalizado correctamente y el sitio esté disponible.

## Resumen y siguientes pasos

En esta lección has:

- pedido a Copilot que configure la publicación en GitHub Pages.
- revisado el flujo de trabajo y la solicitud de incorporación de cambios antes de combinarlos.
- seguido la publicación y verificado el sitio publicado.

Ahora que el Space Quiz está disponible, continúa con la [Lección 13: Revisión y siguientes pasos][next-lesson].

[next-lesson]: ../13-review/
