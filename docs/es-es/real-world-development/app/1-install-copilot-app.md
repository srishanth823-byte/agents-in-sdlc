---
title: "Lección 1 - Instalar la aplicación GitHub Copilot"
description: "Instala la aplicación GitHub Copilot, conecta el repositorio creado a partir de la plantilla, familiarízate con el espacio de trabajo y prueba un chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La **[aplicación GitHub Copilot][about-copilot-app]** es una aplicación de escritorio para el desarrollo dirigido por agentes. Se basa en GitHub Copilot CLI y se integra de forma nativa con GitHub, por lo que los repositorios, las ramas y los flujos de CI funcionan sin configuración adicional. Está diseñada para flujos de trabajo en los que diriges varios agentes en paralelo, cada uno en su propio espacio de trabajo aislado, en lugar de realizar todo el trabajo por tu cuenta, y para automatizar tareas repetitivas. Con Node.js instalado y tu copia del proyecto preparada, el siguiente paso es instalar la aplicación y conectar ese repositorio.

En esta lección:

- instalarás la aplicación GitHub Copilot e iniciarás sesión;
- añadirás el proyecto a la aplicación desde su repositorio de GitHub;
- conocerás el espacio de trabajo, incluida la lista de incidencias que ha creado la plantilla;
- probarás un chat rápido para obtener información sobre la propia aplicación.

## Situación

Tu equipo está adoptando agentes de IA para gestionar un backlog cada vez mayor. La aplicación Copilot ofrece un único lugar desde el que dirigir ese trabajo: seleccionar incidencias, ejecutar agentes, revisar cambios y fusionar pull requests. En esta lección instalarás y conectarás la aplicación y aprenderás a iniciar una conversación sobre el proyecto.

> [!NOTE]
> Se requiere un plan de Copilot válido: Copilot Free, Copilot Student o cualquier plan de pago (Pro, Pro+, Business o Enterprise). Si usas Copilot Business o Copilot Enterprise, el administrador debe mantener habilitada la directiva **GitHub Copilot app**.

## Instalar y configurar la aplicación GitHub Copilot

El primer paso para usar la aplicación GitHub Copilot es instalarla. Hay versiones para Windows, macOS y Linux. Vamos a instalar la aplicación, autenticarnos y añadir el repositorio Tailspin Toys.

1. En un navegador, abre la [página de inicio de la aplicación GitHub Copilot][download-app].
2. Descarga la aplicación para tu plataforma e instálala siguiendo las instrucciones de la página.
3. Abre la aplicación después de instalarla.
4. Selecciona **Sign in to GitHub** y sigue las instrucciones para autenticarte. Si usas GitHub Enterprise Cloud con residencia de datos, elige **Use GitHub Enterprise** e introduce el nombre de host `*.ghe.com` cuando se te solicite. GitHub Enterprise Server no es compatible.
5. Si durante la configuración se te pide que selecciones un repositorio, selecciona el repositorio Tailspin Toys que creaste a partir de la plantilla. También puedes omitir este paso y añadir el proyecto después.
6. Cuando se te pida que elijas un tema, selecciona el que más te guste y completa la configuración.
7. Busca el repositorio en **Sessions**, en la barra lateral. Si ya aparece en la lista, no hace falta que lo añadas de nuevo.
8. Si aún no está conectado, selecciona **+** junto a **Sessions** para iniciar una sesión nueva y, después, selecciona **Add GitHub repository**. Introduce el propietario y el nombre del repositorio que anotaste en la lección de requisitos previos.
9. Confirma que el repositorio aparece en la barra lateral. En la disposición que se muestra aquí, las carpetas de repositorios como `tailspin-toys` están agrupadas en **Sessions**, con las sesiones individuales debajo de cada repositorio.

## Familiarizarse con el espacio de trabajo

Con el proyecto conectado, dedica un momento a conocer el espacio de trabajo. La aplicación organiza sus funciones en varias áreas de la barra lateral:

- **New**: inicia una sesión de chat nueva con Copilot.
- **Pull requests**: accede a tus pull requests mediante la integración nativa de GitHub. Puedes examinarlas y filtrarlas, comprobar el estado de CI y revisarlas sin salir de la aplicación.
- **Issues**: accede a tus incidencias de la misma forma. Puedes examinarlas y filtrarlas e iniciar una sesión a partir de una incidencia.
- **Automations**: consulta tareas de agente guardadas que se ejecutan según una programación o bajo demanda. Son útiles para gestionar listas de tareas, realizar el mantenimiento periódico del proyecto o delegar tareas repetitivas. En el resumen se indican como un siguiente paso, no como otro ejercicio del taller.
- **Customize**: añade funciones a la aplicación Copilot mediante servidores MCP, plugins, habilidades y otros componentes. La usarás para configurar MCP de Playwright.
- **Chats**: inicia conversaciones ligeras para preguntas y lluvias de ideas que no necesitan su propia rama o espacio de trabajo. Esta sección aparece después de iniciar el primer chat. Si no está visible, selecciona **+** junto a **Sessions** para empezar un chat.
- **Sessions**: agrupa las carpetas de repositorios y sus sesiones de agente. Selecciona el nombre de un repositorio, como `tailspin-toys`, para abrir su vista, o selecciona una sesión debajo para volver a esa conversación. Cada sesión se ejecuta en un espacio de trabajo aislado, por lo que puedes ejecutar varias sin que sus cambios entren en conflicto. Iniciarás tu primera sesión al añadir las valoraciones con estrellas.

¡Explorarás el espacio de trabajo durante el taller!

> [!TIP]
> Si tienes dudas, pregunta a Copilot. Si no sabes cómo hacer algo o si es posible, puedes consultarlo y te ayudará.

### Encuentra el backlog creado para ti

Probablemente no haya ningún proyecto sin backlog, y Tailspin Toys no es una excepción. Vamos a revisar el backlog que se creó al generar la plantilla.

1. En **Sessions**, selecciona la carpeta del repositorio Tailspin Toys, no una sesión individual que aparezca debajo. Después, selecciona **Issues** en la navegación superior del repositorio.
2. Busca estas incidencias por su título, sin dar por hecho el número que tienen:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecciona una incidencia para leer sus detalles. Cada incidencia también permite iniciar una sesión de agente. Empezarás por la incidencia de filtrado después de realizar un primer cambio rápido.

> [!NOTE]
> Las listas de **Issues** y **Pull requests** se filtran automáticamente y solo muestran elementos de los repositorios que has añadido a la aplicación Copilot. Para ver elementos de otros repositorios, añádelos a la aplicación.

## Prueba un chat rápido

Una buena forma de familiarizarte con la aplicación es preguntarle sobre la *propia aplicación*. Para eso sirve un **chat rápido**: permite hacer una pregunta o proponer ideas sin crear una rama ni un worktree, así que es perfecto para una consulta breve que no requiere una sesión.

1. En la barra lateral, selecciona **+** junto a **Chats** para abrir un chat nuevo. Si **Chats** aún no está visible, selecciona **+** junto a **Sessions** para iniciar tu primer chat; después aparecerá **Chats**.
2. Pregunta a la aplicación cómo funcionan sus sesiones:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Lee la respuesta en la vista de conversación. Verás que cada sesión se ejecuta en su propio worktree de Git aislado, lo que permite ejecutar varios agentes en paralelo sin que sus cambios entren en conflicto. Puedes seguir con la conversación o iniciar un chat nuevo en cualquier momento.

## Resumen y siguientes pasos

¡Enhorabuena! Has instalado la aplicación GitHub Copilot, conectado el proyecto y explorado el espacio de trabajo. Has aprendido a:

- instalar la aplicación e iniciar sesión en GitHub;
- añadir un proyecto desde su repositorio de GitHub;
- conocer el espacio de trabajo y localizar el backlog creado en **Issues**;
- usar un chat rápido para formular una pregunta breve.

A continuación, [iniciarás tu primera sesión de agente][next-lesson] y la usarás para mostrar una valoración con estrellas en las tarjetas de juegos.

## Recursos

- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Introducción a la aplicación GitHub Copilot][getting-started]
- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
