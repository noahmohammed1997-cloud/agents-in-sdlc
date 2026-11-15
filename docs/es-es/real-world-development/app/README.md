---
slug: es-es/real-world-development/app
title: "Aplicación GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La [**aplicación GitHub Copilot**](https://docs.github.com/copilot/concepts/agents/github-copilot-app) es una aplicación de escritorio basada en Copilot CLI que reúne el desarrollo dirigido por agentes en un espacio de trabajo específico. Añade sesiones de agentes en paralelo, modos de sesión intercambiables, lienzos compartidos y administración nativa de incidencias y solicitudes de incorporación de cambios de GitHub. También incluye **Agent Merge**, que acompaña una solicitud durante la actualización de la base, los comentarios de revisión, las correcciones de integración continua (CI) y la combinación.

El taller sigue un flujo de trabajo continuo con Tailspin Toys:

1. Prepara el proyecto, instala la aplicación, conecta el repositorio y explora el espacio de trabajo y el backlog inicial.
2. Haz un cambio específico para añadir valoraciones por estrellas, revísalo en el navegador y combina manualmente tu primera solicitud de incorporación de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en modo **Plan**, impleméntalo en modo **Autopilot** y revísalo en modo **Interactive**.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la habilidad `quality-checks` existente y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y utilízalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y utilízalo para revisar los requisitos, la cobertura y las pruebas de verificación.
8. Revisa todo el cambio de filtrado y utiliza Agent Merge para la segunda PR.
9. Comprueba qué lienzos están disponibles, utiliza Database Explorer y crea y prueba un lienzo de clasificación asociado al repositorio. Si no están disponibles las funciones necesarias, registra el bloqueo, utiliza la alternativa de solo lectura o continúa con el resumen.

Para mantener el taller centrado, crearás dos PR: una para las valoraciones por estrellas y otra para el filtrado con las actualizaciones de instrucciones y de la habilidad, el perfil QA y las pruebas. Empieza cada PR desde `main` actualizada. El flujo de filtrado y calidad comparte una sesión, un worktree y una rama para que puedas aprovechar el trabajo realizado mientras exploras cada herramienta. El ejercicio final de lienzo permanece en su propia sesión para que puedas centrarte en crear y probar la superficie compartida, sin repetir el flujo de PR.

## Lecciones

| Lección | Tema | Descripción |
|--------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Instala Node.js y crea tu copia del proyecto Tailspin Toys |
| [1. Instalar la aplicación Copilot][ex1] | Configuración | Instala la aplicación, conecta el proyecto y familiarízate con el espacio de trabajo |
| [2. Añadir valoraciones por estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y la alternativa para `null`; después, combina la PR 1 |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la funcionalidad a partir de su incidencia, impleméntala con Autopilot y revísala en modo Interactive |
| [4. Guiar a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personalizar y utilizar una habilidad quality-checks][ex5] | Comprobaciones repetibles | Explora la habilidad existente, cambia el formato del informe y ejecútala |
| [6. Validar la funcionalidad con Playwright MCP][ex6] | Observación en el navegador | Configura MCP desde Customize y examina el comportamiento del filtrado |
| [7. Crear y utilizar un agente QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas de verificación finales |
| [8. Crear y combinar la PR de la funcionalidad][ex8] | Revisión y combinación | Revisa el filtrado, las instrucciones, la habilidad, el perfil QA y las pruebas; después, utiliza Agent Merge para la segunda PR |
| [9. Explorar y crear lienzos][ex9] | Colaboración | Utiliza Database Explorer y, después, crea y prueba un lienzo de clasificación asociado al repositorio |
| [10. Repaso y pasos siguientes][ex10] | Resumen | Repasa el flujo de trabajo, los elementos creados y otros recursos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Un equipo con **macOS, Linux o Windows**
- [ ] [Git instalado][install-git] en el equipo

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis mediante [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que se utilizan en este taller, por lo que permite completar todos los recorridos.

> [!NOTE]
> La aplicación Copilot se ejecuta en tu equipo, no en un codespace. Por eso, el [ejercicio de requisitos previos][ex0] explica cómo instalar Node.js y crear una copia del proyecto antes de instalar la aplicación.

> [!NOTE]
> Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**. Es una directiva independiente de la de Copilot CLI.

## Empezar

**[Empieza por los requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-app/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-canvases/
[ex10]: 10-review/
[install-git]: https://github.com/git-guides/install-git
[callout-student-plan-education]: https://github.com/education/students
