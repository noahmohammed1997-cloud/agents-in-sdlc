---
title: "Lección 2 - Añade valoraciones por estrellas: una mejora rápida"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, haz un pequeño cambio en las tarjetas de juegos y combínalo en tu primera solicitud de incorporación de cambios."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Ya has explorado el espacio de trabajo y probado un chat rápido. Ahora iniciarás una **sesión de agente** y harás tu primer cambio en el proyecto. Será pequeño: los juegos ya incluyen una valoración por estrellas en sus datos, pero las tarjetas de la página de inicio no la muestran. Pedirás al agente que la muestre, revisarás el cambio y lo combinarás en tu primera solicitud de incorporación de cambios.

En esta lección aprenderás a:

- iniciar una sesión de agente y conocer su estructura.
- pedir al agente que realice un cambio pequeño y específico en el proyecto.
- revisar el cambio en la vista de diferencias del espacio de trabajo.
- ejecutar la aplicación localmente y comprobar el cambio en el navegador.
- abrir y combinar tu primera solicitud de incorporación de cambios.

## Escenario

Cada juego de Tailspin Toys puede tener una valoración por estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de juegos de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Para empezar, pedirás al agente que muestre la valoración existente en cada tarjeta. Es un cambio pequeño y autocontenido, perfecto para tu primera sesión.

## Partes de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión obtiene un **worktree y una rama de Git dedicados**. Así puedes ejecutar varias sesiones a la vez, por ejemplo, una para añadir una funcionalidad y otra para corregir un error, sin que sus cambios entren en conflicto. Las sesiones aparecen en la barra lateral, agrupadas por repositorio; selecciona una para cambiar a ella.

En una sesión verás tres elementos: la **conversación** con el agente, la **actividad de las herramientas** mientras explora y edita archivos, y la lista de **archivos modificados** con sus diferencias.

## Inicia una sesión y solicita el cambio

Inicia una sesión para explorar el proyecto e implementar la funcionalidad. En la [configuración de la aplicación][prior-lesson] añadiste el proyecto desde su repositorio de GitHub. Ahora iniciarás una sesión para ese repositorio y solicitarás el cambio.

1. Vuelve a abrir la aplicación GitHub Copilot.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona `tailspin-toys` como repositorio.
4. Elige un **new working tree** y el modo **Interactive** debajo del cuadro del prompt. Envía este prompt:

   ```plaintext
   Muestra la propiedad starRating de cada juego sobre 5 en las tarjetas de la lista. Si la valoración es null, muestra "No rating yet". Mantén el diseño actual de las tarjetas, añade pruebas y ejecuta las comprobaciones pertinentes.
   ```

5. Pulsa <kbd>Enter</kbd> para enviar el prompt a Copilot.

La aplicación Copilot empieza por crear un worktree, una copia aislada del proyecto. Después explora el proyecto, localiza los archivos que debe actualizar para añadir la funcionalidad y crea el código necesario.

## Revisa el diff

Hay que revisar todos los cambios generados por inteligencia artificial antes de combinarlos, aunque sean pequeños. Examínalos en la aplicación Copilot.

1. En la esquina superior derecha de la aplicación, selecciona **Toggle review panel**. En el panel que se abre, selecciona **Changes** para ver los archivos modificados y sus diferencias.

   ![Barra de herramientas superior de la aplicación GitHub Copilot con una flecha que señala el botón Toggle review panel, a la derecha de Create PR](../../../_images/app-2-review-panel.png)

2. Comprueba si se ha añadido código a `GameCard.astro`, el archivo principal utilizado para mostrar los detalles de los juegos. Debería parecerse a este bloque pequeño, que muestra la valoración si existe y, si `starRating` es `null`, muestra `No rating yet`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas las herramientas de inteligencia artificial generativa, Copilot es probabilístico. El código puede diferir del ejemplo, aunque debería ser parecido.

## Comprueba los cambios

Revisa los resultados de las comprobaciones automatizadas del agente antes de abrir un navegador. Confirma que las pruebas cubren una valoración `starRating` numérica y la alternativa para `null`. Un requisito previo ausente o una comprobación omitida no cuentan como superados; revisa cualquier solicitud de instalación antes de aprobarla.

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que abra el sitio para examinar la interfaz de usuario (UI) actualizada en un lienzo de navegador.

> [!TIP]
> Un lienzo es un widget interactivo disponible dentro de la aplicación Copilot. Más adelante explorarás lienzos personalizados e incluso crearás uno, pero por ahora utilizarás el lienzo de navegador integrado.

1. Envía este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo de navegador:

   ```plaintext
   Inicia la aplicación y ábrela en el lienzo del navegador.
   ```

2. En unos instantes, se iniciará la aplicación y aparecerá una ventana de navegador dentro de la aplicación Copilot.
3. Confirma que las tarjetas de los juegos valorados muestran su puntuación sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que inició para esta sesión:

   ```plaintext
   Detén el servidor de desarrollo y cierra el lienzo del navegador.
   ```

## Abre y combina tu primera solicitud de incorporación de cambios

Ya has creado la funcionalidad. Ahora abrirás una solicitud de incorporación de cambios (PR) para añadir el código al proyecto.

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se te solicita, selecciona **Sign in with your browser** y sigue las instrucciones para autenticarte.
3. Si Copilot indica que la directiva del repositorio exige relacionar la PR con una incidencia, confirma que debe seguir ese proceso.
4. Copilot creará la PR.
5. Selecciona la burbuja **PR** justo encima del chat para abrir la solicitud en el panel de revisión.
6. Cuando estés conforme, selecciona **Ready to merge**.
7. En el nuevo cuadro de diálogo, selecciona **Merge pull request** para combinar la solicitud.

## Resumen y siguientes pasos

Has publicado tu primer cambio con la aplicación GitHub Copilot. En concreto, has:

- iniciado una sesión de agente y conocido su estructura.
- pedido al agente que hiciera un cambio pequeño y específico en las tarjetas de juegos.
- revisado el cambio en la vista de diferencias del espacio de trabajo.
- ejecutado la aplicación localmente para comprobar las valoraciones por estrellas en el navegador.
- abierto la PR 1, revisado sus comprobaciones y autorizado explícitamente la combinación.

A continuación, [partirás de la incidencia de filtrado y utilizarás los modos Plan y Autopilot][next-lesson] para implementar una funcionalidad mayor.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Administrar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#familiarizate-con-el-espacio-de-trabajo
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
