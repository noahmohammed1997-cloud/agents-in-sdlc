---
title: "Lección 8 - Crea y combina la PR de la funcionalidad"
description: "Revisa todo el cambio de filtrado y las pruebas de verificación, crea una PR y utiliza Agent Merge después de autorizarlo."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios. Ya combinaste manualmente la PR de valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería separar la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama.

En esta lección aprenderás a:

- comprender qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionar todos los cambios de la funcionalidad y las pruebas de verificación.
- autorizar Agent Merge solo después de la revisión y confirmar que la PR se ha combinado.

## Escenario

Durante el flujo de filtrado has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener el control del desarrollador sobre la autorización para combinarla.

## Acerca de Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una PR mediante la aplicación GitHub Copilot. Al habilitarlo, la sesión lee la solicitud y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta. Después la combina en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa tras reiniciar la aplicación y se desactiva cuando la PR se combina.

Hasta ahora has seleccionado manualmente **Merge pull request**. Agent Merge puede encargarse de esa tarea, pero todavía necesita tu autorización explícita para editar código y combinar. Revisa las acciones permitidas y el trabajo antes de concederle permiso para combinar.

## Utiliza Agent Merge para administrar la PR

Ya has creado y revisado el código. Ahora permitirás que Agent Merge administre el proceso de la PR.

1. En el selector de agentes, selecciona **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se iniciará el proceso de Agent Merge. El agente:

- creará la solicitud de incorporación de cambios con un título y una descripción.
- incluirá en la descripción una referencia a la incidencia relacionada si iniciaste la sesión desde una incidencia.
- volverá a aplicar la base o resolverá posibles conflictos con la rama de destino.
- supervisará el proceso de CI para confirmar que se superan todas las comprobaciones.
- supervisará los comentarios de otros desarrolladores y la revisión de código de Copilot y hará cambios para resolverlos.
- podrá combinar automáticamente la PR cuando todo se haya completado correctamente.

Permite que Agent Merge combine la PR cuando se hayan superado todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para combinar.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que la opción **Merge pull request** está marcada.
8. Pide explícitamente al agente que combine la PR:

   ```plaintext
   Combina la solicitud cuando se hayan superado todas las comprobaciones y aprobaciones obligatorias. No omitas las protecciones del repositorio.
   ```

9. Supervisa el progreso y resuelve los bloqueos que indique. Antes de continuar, confirma que el estado de la PR es **Merged**. Tener habilitado el permiso de combinación o un agente en ejecución no confirma que la PR se haya combinado.

> [!IMPORTANT]
> Agent Merge no omite las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación del código, y el proceso de la solicitud de incorporación de cambios. Has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una PR.
- inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- autorizado Agent Merge después de revisar los cambios y confirmado que la PR se ha combinado.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Administrar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
