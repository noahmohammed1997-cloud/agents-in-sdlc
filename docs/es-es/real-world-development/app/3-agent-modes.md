---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Explora los modos de agente: utiliza Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar y verificar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Empezamos añadiendo una funcionalidad pequeña al proyecto. Sin embargo, los cambios mayores requieren un proceso más sólido. La aplicación GitHub Copilot está diseñada para adaptarse al flujo de desarrollo de una organización y ayudar a construir lo adecuado de la forma correcta. En esta lección y en las siguientes seguirás un proceso habitual de desarrollo con agentes: partirás de una incidencia para crear una funcionalidad, comprobarás el código y su comportamiento y, finalmente, la integrarás en el proyecto.

> [!NOTE]
> Utilizarás la misma sesión durante todo el flujo de trabajo de esta funcionalidad. Normalmente se usarían sesiones o solicitudes de incorporación de cambios (PR) distintas para diferentes tipos de archivo, pero simplificaremos el proceso para centrarnos en los conceptos principales.

En esta lección aprenderás a:

- iniciar una sesión de agente nueva a partir de una incidencia de GitHub.
- definir los requisitos en modo **Plan**.
- implementar la funcionalidad con el modo **Autopilot**.
- revisar el código.
- comprobar manualmente la funcionalidad en un lienzo de navegador.

En las siguientes lecciones actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación de Model Context Protocol (MCP), crearás un agente QA y abrirás la PR de la funcionalidad.

## Escenario

El catálogo de Tailspin Toys está creciendo y sus visitantes necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la funcionalidad, pero antes de programar hay que acordar detalles, como la combinación de categorías. Utilizarás el modo Plan para tomar esas decisiones y, después, autorizarás una implementación acotada con Autopilot.

## Contexto

La incorporación de agentes de programación con inteligencia artificial no cambia los fundamentos del desarrollo; los hace aún más importantes. La mayoría de los desarrolladores siguen un flujo parecido:

1. Abrir una incidencia registrada con los detalles del trabajo.
2. Crear un plan de lo que hay que implementar.
3. Crear y revisar el código.
4. Ejecutar pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una solicitud de incorporación de cambios (PR).
7. Combinar el código después de que se haya revisado y se hayan superado las comprobaciones de integración continua.

> [!NOTE]
> Los detalles varían entre equipos y organizaciones, pero la mayoría siguen alguna variante del proceso anterior.

Este enfoque ayuda a garantizar que el código generado por inteligencia artificial cumpla los requisitos y pase por el mismo proceso de revisión que el código escrito manualmente.

## Modos de sesión

El **modo de sesión** controla la autonomía del agente. Puedes seleccionarlo en el menú desplegable situado debajo del campo del prompt y cambiarlo en cualquier momento:

- **Interactive:** tú y el agente trabajáis juntos. El agente propone cambios y espera tus indicaciones antes de continuar.
- **Plan:** el agente prepara primero un plan. Revisas y apruebas el plan antes de que el agente lo ejecute.
- **Autopilot:** el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera sin esperar indicaciones.

Empieza en modo Plan y revisa el plan. Después, utiliza Autopilot para implementarlo.

## Inicia una sesión desde la incidencia

Antes de empezar, confirma que la PR de valoraciones por estrellas está combinada y que tu `main` local está actualizado.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys y, después, selecciona **Issues** en la navegación superior del repositorio. Abre **Allow users to filter games by category and publisher**. Es una incidencia que ya incluye la plantilla; no tienes que crear otra.
2. Selecciona **New session** y crea un **new working tree** a partir de la rama `main` actualizada.

   ![Vista de una incidencia en la aplicación GitHub Copilot con una flecha que señala el botón New session](../../../_images/app-new-session-from-issue.png)

3. Confirma que la incidencia está adjunta a la sesión y selecciona **Plan** en el selector de modo.

> [!NOTE]
> Si no se encuentra Node.js o npm, pide al agente que ejecute `node --version` y `npm --version`. Si funcionan en el terminal, pero no en la sesión, comprueba la configuración de `PATH` o del administrador de versiones de la aplicación y ciérrala por completo antes de volver a abrirla. Reintenta la operación y las comprobaciones fallidas. No reinstales npm solo porque el agente no lo encuentre.

## Planifica la funcionalidad de filtrado

La planificación permite revisar el enfoque antes de que Copilot escriba código. Como has iniciado la sesión desde la incidencia, Copilot ya tiene la solicitud como contexto. Envía este prompt:

```plaintext
Implementa esta funcionalidad.
```

Responde a las preguntas de Copilot y compara el plan con los criterios de aceptación de la incidencia. Comprueba que incluye el filtrado por categoría y editorial, controles accesibles, cambios en el acceso a datos y pruebas. Aclara cualquier comportamiento poco definido, como la combinación de varias categorías o qué ocurre cuando ningún juego coincide.

El plan debe incluir lint, pruebas unitarias, pruebas de extremo a extremo (E2E) y comprobaciones de tipos con las herramientas existentes del proyecto. Céntralo en implementar y probar el filtrado; crearás la PR después de completar el flujo de calidad. Pide cambios en el plan antes de aprobarlo y conserva la URL de la incidencia y las aclaraciones acordadas para validarlas más adelante.

## Autoriza Autopilot explícitamente

Cuando estés conforme con el plan, selecciona **Approve and implement this plan** o la opción equivalente de tu versión. Comprueba el modo activo en vez de asumir que cambia automáticamente al aprobarlo. El indicador debe mostrar **Autopilot**.

Copilot empezará a implementar la funcionalidad. Seguirá el plan establecido, generará código y ejecutará pruebas.

> [!NOTE]
> Al aprobar el plan, la implementación puede empezar inmediatamente. Revísalo primero. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema antes de considerar completas las comprobaciones. Detén solo los servidores que hayas iniciado.

## Revisa y comprueba la implementación

Revisa el código generado antes de combinarlo, como harías con cualquier otro cambio. Examina el código y ejecuta el sitio para comprobar que todo funciona.

1. Abre **Changes** y revisa la implementación del filtrado y las pruebas.
2. Compara el resultado con la incidencia y las aclaraciones aprobadas, incluidas las combinaciones de categorías y editoriales. Comprueba que los cambios siguen las instrucciones existentes del repositorio.
3. Revisa los resultados de lint, las pruebas unitarias, las pruebas E2E y las comprobaciones de tipos. Una comprobación omitida no cuenta como superada.
4. Resuelve los fallos y vuelve a ejecutar las comprobaciones afectadas antes de aceptar la implementación. La configuración E2E de Playwright compila y sirve una vista previa y puede reutilizar un servidor local; asegúrate de que el servidor probado pertenece a este worktree, no a una lección anterior.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿se ejecuta? Inicia la aplicación como antes y abre el sitio en un lienzo de navegador.

1. Envía este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo:

   ```plaintext
   Inicia la aplicación y ábrela en el lienzo del navegador.
   ```

2. En unos instantes, se iniciará la aplicación y aparecerá una ventana de navegador dentro de la aplicación Copilot.
3. Selecciona una categoría y confirma que los resultados coinciden. Borra el filtro, selecciona una editorial y confirma que los resultados corresponden a esa editorial.
4. Combina los filtros de categoría y editorial y compara los resultados con la incidencia y el plan aprobado. Si se admiten varias categorías, comprueba el comportamiento acordado.
5. Prueba una combinación sin juegos coincidentes y revisa el estado vacío. Borra o restablece los filtros y confirma que vuelve a aparecer la lista completa.
6. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que inició para esta sesión:

   ```plaintext
   Detén el servidor de desarrollo y cierra el lienzo del navegador.
   ```

## Conserva esta sesión para las lecciones siguientes

Todavía no crees ni combines la PR de filtrado. Conserva esta sesión, el worktree y la rama para las lecciones 4 a 8. A continuación aplicarás instrucciones personalizadas al código de filtrado y añadirás las mejoras de la habilidad y del agente QA antes de crear y combinar la PR conjunta en la Lección 8. Puedes dejar la sesión inactiva después de detener su servidor de desarrollo y volver a ella en la siguiente lección.

## Resumen y siguientes pasos

Has utilizado distintos modos de agente para crear y revisar una funcionalidad. En esta lección has:

- iniciado una sesión de agente nueva a partir de una incidencia de GitHub.
- definido los requisitos en modo **Plan**.
- implementado la funcionalidad con el modo **Autopilot**.
- revisado el código.
- comprobado manualmente la funcionalidad en un lienzo de navegador.

A continuación, profundizarás en cómo se genera el código y cómo seguir las prácticas documentadas mediante [instrucciones personalizadas][next-lesson].

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
