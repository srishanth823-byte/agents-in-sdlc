---
title: "Ejercicio 2 - Añadir funciones nuevas con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Hasta las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones como ejecutar pruebas. El flujo habitual de desarrollo puede incluir localizar los archivos necesarios, hacer los cambios, ejecutar pruebas, depurar, averiguar qué archivo falta, volver a modificarlo... Y la lista continúa.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar de forma más autónoma en el IDE. Se comporta de manera parecida a una persona desarrolladora: explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Vamos a ver cómo puedes usar el modo de agente para añadir funciones al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube están diseñados para experiencias distintas. El modo de agente trabaja en el IDE y permite ciclos rápidos de comentarios e interacción. El agente en la nube está pensado como un compañero que trabaja de forma asíncrona, como cualquier otra persona del equipo, y se comunica contigo mediante incidencias y pull requests.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto, identificar archivos relevantes y coordinar cambios;
- usar el modo de agente de GitHub Copilot para implementar funciones en la interfaz y la capa de datos;
- revisar los cambios y las pruebas generados antes de fusionarlos en el código base.

## Situación

El catálogo de juegos está creciendo y quieres que las personas puedan filtrarlos por categoría y editorial. En el ejercicio anterior ya has añadido un helper de editoriales; ahora completarás la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecutar el sitio web de Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para entender cómo funciona actualmente.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con una temática tecnológica. Permite consultar una lista de juegos y ver sus detalles. Se trata de una única aplicación Astro que genera las páginas como HTML estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local desde su frontmatter mediante Drizzle ORM: no hay una API de backend independiente ni un framework de interfaz en el cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade con un pequeño elemento `<script>` de Astro, con ámbito limitado y API DOM estándar.

### Iniciar el sitio web

Para facilitar la ejecución del sitio web, se proporciona la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en tu GitHub Codespace siguiendo estos pasos:

1. Vuelve a tu codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre una ventana de terminal nueva en el codespace pulsando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta el siguiente comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, verás un mensaje con la URL, parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pregunta si quieres abrir un navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Para abrir el sitio web, mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección del servidor de Astro, `http://localhost:4321`, en el terminal.

> [!NOTE]
> En un codespace, al seleccionar el enlace de localhost en el terminal, se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado al codespace, que ahora aloja el servidor web.

### Explorar el sitio web

Cuando el sitio web esté en ejecución, puedes explorar sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, accederás a una página con más información, como el título, la descripción, la editorial y la categoría.

## Explorar el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los siguientes ejercicios, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían en cada ejercicio; cada paso los indicará cuando corresponda.

La implementación inicial del sitio funciona, pero queremos mejorarla con nuevas funciones. Primero revisaremos el backlog. Al configurar el proyecto a partir de la plantilla, se creó automáticamente un conjunto de incidencias. Pide a GitHub Copilot que te las muestre.

1. Selecciona **Agent** en el selector de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos de forma autónoma, ejecuta comandos del terminal e invoca herramientas.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Las personas autoras de este taller no recomiendan un modelo específico. Al crear el taller usamos Claude Sonnet 4.5, por eso lo indicamos en estas instrucciones. El objetivo es que las sugerencias de código sean relativamente coherentes y la experiencia resulte agradable. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden diferir de los ejemplos. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot podría utilizar otro comando MCP, pero debería poder completar la tarea igualmente.

3. Pide a Copilot que muestre el backlog de incidencias con este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que muestra todas las incidencias.
5. Revisa la lista de incidencias generada.

Observa que Copilot también ha priorizado las incidencias según cuáles podrían resultar más útiles para las personas usuarias.

## Revisa los archivos de instrucciones

Antes de pedirle al agente que genere el código, revisa el archivo de instrucciones que usarás para proporcionar contexto a Copilot. Utilizarás el archivo de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que contiene información sobre cómo añadir funciones al sitio web.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las directrices generales para añadir funciones, entre ellas:
   - una descripción de la arquitectura;
   - principios para diseñar componentes, facilitar las pruebas y garantizar la accesibilidad;
   - enlaces a archivos de instrucciones para varios tipos de archivo, como:
     - Astro;
     - Tailwind CSS;
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden hacer referencia a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así puedes reutilizar contenido, dividir conjuntos complejos de instrucciones en partes más manejables y proporcionar ejemplos y plantillas.

## Implementar la funcionalidad de filtrado

Para completar el filtrado, hay que hacer al menos tres cambios en la aplicación:

- añadir o mejorar la lógica de filtrado restante en la capa de datos (`src/lib/`);
- añadir o actualizar las pruebas del comportamiento del filtrado;
- actualizar la página de la lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios en el código base. El modo de agente de Copilot puede hacer estas tareas. Vamos a añadir la función.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de la selección del archivo de instrucciones UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el selector de agentes de la vista Chat.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la función según la incidencia correspondiente del backlog con este prompt:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función: tanto los helpers de la capa de datos y la interfaz como las pruebas. Después empezará a modificar los archivos y a ejecutar las pruebas.

   ![Captura de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. Puede tardar unos minutos, según la complejidad de la tarea y el código base. Mientras tanto, quizá aparezcan los botones **Keep** y **Undo** en el editor. Cuando Copilot termine, cada cambio tendrá las opciones **Keep** y **Undo**; no hace falta que las selecciones mientras el agente sigue trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del cuadro de diálogo de Copilot Chat que pide confirmación para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completa de Copilot Chat en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva función.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio hemos explorado cómo usar el modo de agente de GitHub Copilot para añadir funciones al sitio web de Tailspin Toys. Hemos aprendido a:

- usar el modo de agente de GitHub Copilot para implementar funciones en la interfaz y la capa de datos;
- usar el modo de agente de Copilot para explorar el proyecto, identificar los archivos relevantes y coordinar los cambios;
- revisar los cambios y las pruebas generados antes de fusionarlos en el código base.

Ahora vamos a [probar la función con el servidor MCP de Playwright][next-lesson] y abrir una pull request.

### Ejercicio adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario añadir paginación. Usa lo que has aprendido en este ejercicio y pide a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las buenas prácticas existentes, incluidos los archivos de instrucciones;
- decide cómo quieres implementar la paginación, por ejemplo, si la persona usuaria puede elegir el tamaño de página o si será fijo;
- incluye en el prompt las directrices necesarias para que Copilot implemente lo que necesitas;
- quizá tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es normal trabajar así con Copilot.

## Recursos

- [Modos Ask, Edit y Agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
