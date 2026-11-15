---
title: "Lección 8 - Crea y combina la solicitud de incorporación de cambios (PR) de la funcionalidad"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de control de calidad y las pruebas; después crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Ahora los revisarás juntos y abrirás una solicitud de incorporación de cambios. Combinaste manualmente la solicitud de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** administre el proceso.

> [!NOTE]
> Lo habitual sería separar la funcionalidad, las instrucciones, la actualización de la habilidad y el agente QA en varias solicitudes. Para simplificar el taller, has mantenido el flujo completo de filtrado y calidad en una conversación y una rama, y lo incluirás todo en esta PR.

En esta lección aprenderás a:

- comprender qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- inspeccionar todos los cambios de la funcionalidad y las pruebas de verificación.
- crear la PR de filtrado.
- habilitar Agent Merge solo después de revisar los cambios y confirmar que la PR se ha combinado.

## Escenario

Durante el flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Tailspin Toys quiere automatizar el trabajo restante de la PR sin quitar al desarrollador el control de autorización de la combinación.

## Acerca de Agent Merge

**Agent Merge** automatiza el trabajo restante para combinar una solicitud de incorporación de cambios. Al habilitarlo, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a aplicar la base cuando hace falta. Después, habilita la combinación automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado manualmente **Merge pull request**. Agent Merge puede encargarse de esta parte, pero primero debes revisar el trabajo y confirmar que está listo.

## Deja que Agent Merge administre la PR

Ya has creado el código. Ahora lo revisarás, crearás la PR y permitirás que Agent Merge se ocupe del resto.

1. Vuelve al codespace.
2. Escribe `/agent` para abrir el cuadro de diálogo de agentes.
3. Selecciona **Default** en la lista y pulsa <kbd>Enter</kbd>.
4. Antes de crear la PR, escribe `/diff` y revisa todos los cambios. Consulta también el informe de QA de la lección anterior y confirma que las comprobaciones automatizadas y el comportamiento en el navegador se han revisado.
5. Si detectas un problema, no habilites Agent Merge. Pide a Copilot que lo corrija, vuelve a ejecutar la habilidad `quality-checks` y repite las comprobaciones del navegador que correspondan antes de continuar.
6. Crea una PR con el comando `/pr create`.
7. Activa Agent Merge con `/pr agentmerge` solo después de confirmar que los cambios están revisados y listos.
8. Copilot supervisará el proceso de integración continua de la PR. Cuando todas las comprobaciones se superen, realizará la combinación.
9. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».
10. Si Agent Merge se bloquea, revisa el motivo indicado y completa manualmente la combinación revisada cuando la configuración del repositorio lo permita.

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de combinación, la configuración del repositorio ni los permisos que falten.

## Resumen y siguientes pasos

Has automatizado varias partes del desarrollo: la generación de código, las pruebas y la validación del código, y el proceso de la solicitud de incorporación de cambios. Has:

- aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una solicitud de incorporación de cambios.
- inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- creado la PR de filtrado.
- habilitado Agent Merge después de revisar los cambios y confirmado que la PR se ha combinado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para el contexto, los modelos, el uso compartido y la delegación opcional al agente en la nube.

## Recursos

- [Administrar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
