---
title: "Ejercicio 2 - Agente de GitHub Copilot en la nube"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que pocas organizaciones no tengan problemas con la deuda técnica. Puede tratarse de problemas de seguridad pendientes, código heredado que hay que actualizar o solicitudes de funciones que llevan tiempo en el backlog por falta de tiempo para implementarlas. El agente de GitHub Copilot en la nube está diseñado para tareas como actualizar código y añadir funciones de forma autónoma. Cuando termina, crea una pull request (PR) en borrador para que una persona desarrolladora la revise. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

En este ejercicio explorarás cómo usar el agente de Copilot en la nube para:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realizan de forma segura;
- comprender la importancia de definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

## Situaciones

Tailspin Toys tiene deuda técnica que quiere resolver. Las personas que crearon la primera versión del sitio dejaron la documentación en un estado poco adecuado; de hecho, verás que falta por completo. Como primer paso, el equipo quiere añadir comentarios de documentación TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la forma de descubrir juegos. Quiere que la página de detalles de cada juego muestre otros juegos relacionados, es decir, títulos de la misma categoría, para que las personas puedan seguir explorando. Por ahora no necesita una implementación pulida: solo algo que pueda usar para probar la experiencia de usuario. Esto es un bloqueo, aunque hay otras incidencias más prioritarias.

Son ejemplos de tareas que pueden perder prioridad con facilidad y que se pueden asignar al agente de Copilot en la nube. El agente puede trabajar en ellas de forma asíncrona mientras tú te ocupas de otras tareas. Después podrás revisar su trabajo y comprobar que cumple las expectativas.

## Introducción al agente de GitHub Copilot en la nube

El [agente de GitHub Copilot en la nube](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede realizar tareas en segundo plano de forma parecida a una persona desarrolladora. Por ejemplo, puedes [asignarle una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Tras la asignación, Copilot crea una pull request en borrador para registrar su progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o cuando haya terminado. Cuando la solución esté lista para revisarla, Copilot te mencionará en la pull request.

## La importancia de definir bien el alcance

GitHub Copilot no hace magia: no puedes saltarte el trabajo de pensar. Hasta las operaciones que parecen sencillas pueden ser complejas cuando las analizas en detalle. Por eso, [define con cuidado el alcance de las tareas para el agente de Copilot en la nube][cloud-agent-best-practices]. Trátalo como a un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por usar IA generativa.

## Instrucciones personalizadas en este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo orientan a Copilot. Si ya has completado el ejercicio práctico sobre instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, puedes consultarlo ahora. Antes de asignar trabajo al agente de Copilot en la nube, revisa los archivos de instrucciones incluidos en este repositorio para poder observar su efecto más adelante.

Abre los siguientes archivos en la interfaz web de GitHub o en un codespace que ya esté en ejecución:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, en especial las convenciones de TypeScript y los comentarios de documentación TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes la incidencia `Code lacks documentation` al agente en la nube en la siguiente sección, fíjate en la pull request resultante: verás comentarios TSDoc, convenciones de TypeScript y encabezados de comentario. Estas prácticas proceden de los archivos de instrucciones que acabas de revisar. Volveremos a señalarlas al revisar las pull requests en una lección posterior.

## Configurar el entorno de desarrollo para el agente de Copilot en la nube

Para crear código suele hacer falta un entorno concreto y algunos pasos de configuración, que garantizan que todo esté en buen estado. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma similar a una persona ingeniera de software.

El agente en la nube usa [GitHub Actions][github-actions] para configurar el entorno de trabajo. Puedes personalizarlo mediante un [flujo de trabajo de configuración especial][setup-workflow], definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que empiece a trabajar. Así puede acceder a las herramientas de desarrollo y dependencias necesarias. El flujo ya está configurado para este laboratorio e instala Node.js, las dependencias del proyecto, los binarios del navegador para las pruebas de extremo a extremo y la base de datos SQLite migrada y con datos iniciales para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

El archivo se parece a cualquier otro flujo de trabajo de GitHub, pero tiene algunos aspectos clave:

- Contiene un único trabajo, `copilot-setup-steps`, que GitHub Actions ejecuta antes de que Copilot empiece a trabajar en la pull request.
- Fíjate en el desencadenador `workflow_dispatch`, que permite ejecutar el flujo de trabajo manualmente desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona correctamente sin esperar a que lo ejecute Copilot.

## Añadir documentación

Aunque todo el mundo entiende la importancia de la documentación, muchos proyectos tienen información desactualizada o carecen de ella. Esta deuda técnica puede ralentizar el trabajo y dificultar el mantenimiento del código o la incorporación de nuevas personas al equipo. Copilot destaca en la creación de documentación, por lo que esta es una tarea adecuada para el agente de Copilot en la nube. Trabajará en segundo plano para generar la documentación necesaria y volverás a revisar su trabajo en un ejercicio posterior.

1. En una pestaña nueva del navegador, ve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para crear una incidencia en blanco.
5. En **Title**, escribe `Code lacks documentation`.
6. En **Description**, escribe:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la pull request (PR) que acaba de crear el agente. Su título será parecido a `[WIP]: Code lacks documentation`. Si no aparece una PR nueva en la lista, espera un minuto o dos y actualiza la página del navegador.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista de tareas puede tardar varios minutos en aparecer en la PR. Copilot está configurando su entorno (ejecutando el flujo de trabajo anterior), analizando el proyecto y decidiendo cómo abordar la tarea.

13. Revisa la lista y las tareas que va a completar.
14. Desplázate por la cronología de la pull request. Deberías ver un mensaje que indica que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona el botón **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Es posible que tengas que actualizar la página para ver el indicador actualizado.

16. Desplázate por la sesión en directo y observa cómo Copilot resuelve el problema. Verás cómo explora el código, analiza el estado, se detiene para decidir qué plan seguir y crea código.

El proceso puede tardar varios minutos. Una de las principales ventajas del agente de Copilot en la nube es que puede realizar tareas asíncronas y dejarte tiempo para ocuparte de otras cosas. Aprovechemos esta función: asignaremos otra tarea al agente y, mientras trabaja, añadiremos una función a la aplicación.

## Añadir una sección de juegos relacionados a la página de detalles

Una de las grandes ventajas del agente de Copilot en la nube es que permite repartir el trabajo: tú puedes ocuparte de una tarea mientras el agente se encarga de otra. Añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, pero aun así podrías aprovecharlo para otras tareas. Vamos a asignársela al agente de Copilot en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de nueva incidencia.
4. Selecciona **Blank issue** para usar la plantilla en blanco.
5. En **Title**, escribe `Show related games on the game details page`.
6. En **Description**, escribe:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. Selecciona **Create** para crear la incidencia.
8. En el lado derecho, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de Copilot a una incidencia](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.

Poco después, verás el emoji 👀 en el primer comentario de la incidencia. Indica que Copilot ya está trabajando.

![Copilot utiliza el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya se está ocupando de tu segunda solicitud. El agente de Copilot en la nube trabaja de forma similar a una persona ingeniera de software, por lo que no hace falta que lo supervises constantemente; puedes revisar el resultado cuando termine. Ahora pasemos a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente de GitHub Copilot en la nube][copilot-agents]. Puedes asignarle incidencias a Copilot para que trabaje en ellas de forma asíncrona. Puedes usarlo para resolver deuda técnica, crear funciones o ayudar a migrar código de un framework a otro.

Has explorado estos conceptos:

- personalizar el entorno de generación de código;
- garantizar que las operaciones se realizan de forma segura;
- comprender la importancia de definir incidencias con un alcance claro;
- asignar incidencias a Copilot.

Mientras el agente en la nube trabaja en segundo plano, puedes empezar a crear y usar agentes personalizados. El agente en la nube también puede [usar servidores MCP][cloud-agent-mcp] y dispone de instrucciones personalizadas, que ya hemos explorado en módulos anteriores.

## Recursos

- [Acerca del agente de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
