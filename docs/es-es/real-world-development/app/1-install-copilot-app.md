---
title: "Lección 1 - Instala la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La [**aplicación GitHub Copilot**][about-copilot-app] es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y las canalizaciones de integración continua (CI) funcionan directamente. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, y automatizas tareas repetitivas. Ya tienes Node.js instalado y una copia del proyecto; ahora instalarás la aplicación y conectarás el repositorio.

En esta lección aprenderás a:

- instalar la aplicación GitHub Copilot e iniciar sesión.
- añadir el proyecto a la aplicación desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y el backlog que incluye la plantilla.
- probar un chat rápido para conocer la propia aplicación.

## Escenario

Tu equipo está adoptando agentes de inteligencia artificial para trabajar en un backlog cada vez mayor. La aplicación Copilot proporciona un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y combinar solicitudes de incorporación de cambios. En esta lección instalarás y conectarás la aplicación y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Necesitas un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si utilizas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instala y configura la aplicación GitHub Copilot

La aplicación tiene versiones para Windows, macOS y Linux. Instálala, autentícate y añade el repositorio Tailspin Toys.

1. En un navegador, abre la [página de la aplicación GitHub Copilot][download-app].
2. Descarga e instala la aplicación para tu plataforma siguiendo las instrucciones de la página.
3. Abre la aplicación.
4. Selecciona **Sign in to GitHub** y sigue las indicaciones para iniciar sesión con una cuenta de GitHub.com o GHE.com. Si utilizas GitHub Enterprise Cloud con residencia de datos, elige **GitHub Enterprise Cloud** e introduce el nombre de host `*.ghe.com` cuando se te solicite.

   Si utilizas GitHub Enterprise Server, primero completa la incorporación con tu cuenta en la nube. Después, abre **Settings**, selecciona **Accounts**, selecciona **Add account** y elige **GitHub Enterprise Server**. Sigue las instrucciones para autorizar la aplicación en el navegador, como se explica en la [guía de inicio de sesión en Enterprise Server][ghes-sign-in].

5. Si durante la incorporación se te pide que selecciones repositorios, elige el repositorio Tailspin Toys que creaste a partir de la plantilla. También puedes omitir este paso y añadirlo después.
6. Cuando se te solicite, elige un tema y termina la incorporación.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece, no tienes que volver a añadirlo.
8. Si todavía no está conectado, selecciona **+** junto a **Sessions** para crear una sesión y, después, selecciona **Add GitHub repository**. Introduce el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En el diseño de este taller, las carpetas de repositorio, como `tailspin-toys`, están agrupadas en **Sessions**, con las sesiones individuales debajo de cada repositorio.

## Familiarízate con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer la aplicación. La barra lateral organiza el trabajo en varias áreas:

- **New:** permite iniciar una sesión de chat con Copilot.
- **Pull requests:** muestra las solicitudes de incorporación de cambios mediante la integración nativa con GitHub. Puedes explorarlas y filtrarlas, consultar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues:** muestra las incidencias. Puedes explorarlas y filtrarlas e iniciar una sesión a partir de una incidencia.
- **Automations:** tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Sirven para administrar listas de tareas, realizar tareas periódicas de mantenimiento del proyecto u ocuparse de otras tareas repetitivas. El resumen las propone como siguiente paso, no como ejercicio del taller.
- **Customize:** permite añadir funciones a la aplicación Copilot, como servidores Model Context Protocol (MCP), plugins y habilidades. La utilizarás para configurar Playwright MCP.
- **Chats:** conversaciones ligeras para preguntas o lluvias de ideas que no necesitan una rama ni un espacio de trabajo propios. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para empezar uno.
- **Sessions:** agrupa las carpetas de repositorio y las sesiones de agente. Selecciona el nombre de un repositorio, como `tailspin-toys`, para abrir su vista, o selecciona una sesión debajo para volver a la conversación. Cada sesión se ejecuta en su propio espacio de trabajo aislado, por lo que puedes ejecutar varias a la vez sin que sus cambios entren en conflicto. Iniciarás la primera sesión cuando añadas las valoraciones por estrellas.

Explorarás el espacio de trabajo durante el taller.

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puede ayudarte a orientarte.

### Localiza el backlog inicial

Tailspin Toys tiene un backlog de incidencias que se creó al crear el repositorio a partir de la plantilla. Localiza algunas de las incidencias que utilizarás.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, sin dar por hecho el número:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia y lee los detalles. Cada incidencia también permite iniciar una sesión de agente. Tras un cambio rápido, partirás de la incidencia de filtrado.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** muestran automáticamente solo los elementos de los repositorios que has añadido a la aplicación Copilot. Para ver trabajo de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Un chat rápido permite formular una pregunta o plantear ideas sin crear una rama ni un worktree. Es una buena opción para una consulta breve y desechable que no requiere una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar el primer chat; después aparecerá la sección **Chats**.
2. Pregunta cómo funcionan las sesiones de la aplicación:

   ```plaintext
   ¿Cómo utiliza worktrees la aplicación GitHub Copilot?
   ```

3. Lee la respuesta. Verás que cada sesión se ejecuta en su propio worktree de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes continuar la conversación o iniciar otra cuando quieras.

## Resumen y siguientes pasos

Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub.
- añadir un proyecto desde su repositorio de GitHub.
- familiarizarte con el espacio de trabajo y localizar el backlog inicial en **Issues**.
- utilizar un chat rápido para formular una pregunta breve y desechable.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y mostrarás una valoración por estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
[ghes-sign-in]: https://docs.github.com/enterprise-server@3.22/copilot/copilot-on-ghes/sign-in-on-copilot-app
