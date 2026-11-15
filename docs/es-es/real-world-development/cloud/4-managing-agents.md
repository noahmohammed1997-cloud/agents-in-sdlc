---
title: "Ejercicio 4 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas para mejorar la experiencia de usuario y añadir funcionalidades. Aunque el agente trabaja de forma asíncrona y autónoma, es importante supervisar esas tareas.

Puedes administrar las tareas asignadas al agente en la nube con varias herramientas, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro puedes ver las tareas de los agentes que tienen solicitudes de incorporación de cambios abiertas, consultar las operaciones realizadas y orientar una sesión que sigue en curso.

En esta lección aprenderás a:

- explorar la página de agentes para supervisar las tareas del agente en la nube.
- orientar una sesión en curso para solicitar funcionalidad adicional.

## Escenario

Después de asignar al agente la implementación del modo de alto contraste, el equipo decidió que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo controles de cambio de tema, quieres orientar al agente para que implemente ambos modos.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado de las tareas que has asignado al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Examina las tareas que aparecen en el panel principal y en el panel izquierdo. Deberías encontrar las que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una tarea en curso y revisa las operaciones realizadas por Copilot. Pueden incluir:
   - obtener el código del repositorio.
   - preparar el entorno de trabajo.
   - configurar servidores MCP.
   - realizar distintas tareas para completar el trabajo.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Fíjate también en el panel de solicitudes de incorporación de cambios que aparece a la derecha. Desde allí puedes consultar la solicitud y los archivos modificados para supervisar el trabajo.

## Orienta al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que haya elegido Copilot y del estado del trabajo.

   ![Sesión de accesibilidad en Mission Control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado de preparar el entorno y ha empezado a trabajar. Lo notarás cuando aparezcan mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

   ```plaintext
   Ya que estamos trabajando en un modo de alto contraste, añade también un modo claro. Debe haber un control para este modo, de modo que cada usuario pueda elegir la apariencia que prefiera.
   ```

   ![Tarea del agente en la nube con el cuadro de diálogo para orientar la sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Comprueba que Copilot reconoce la solicitud y la incorpora al trabajo.

## Deja que Copilot trabaje

Copilot incorporará la nueva solicitud al trabajo después de completar el paso que estaba realizando cuando enviaste el mensaje.

La tarea tardará varios minutos. Aprovecha este tiempo para repasar lo que has aprendido hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que se supervisan y orientan las tareas del agente en la nube. Permite seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar su alcance o proporcionarle más contexto.

Has explorado estos conceptos:

- explorar Mission Control y la página de agentes para supervisar las tareas.
- redirigir una sesión en curso para pedir funcionalidad adicional.

Mientras Copilot termina el trabajo sobre las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
