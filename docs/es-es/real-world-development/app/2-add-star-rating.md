---
title: "Lección 2 - Añadir valoraciones con estrellas: un cambio rápido"
description: "Inicia tu primera sesión de agente en la aplicación GitHub Copilot, modifica las tarjetas de juegos y fusiona el cambio en tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Ahora que has explorado el espacio de trabajo y utilizado un chat rápido, es el momento de iniciar una **sesión de agente** y hacer tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen valoraciones con estrellas, pero las tarjetas de la página de inicio todavía no las muestran. Pedirás al agente que las muestre, revisarás el cambio y lo fusionarás en tu primera pull request.

En esta lección:

- iniciarás una sesión de agente y aprenderás cómo se estructura;
- pedirás al agente que haga un cambio pequeño y centrado en el proyecto;
- revisarás el cambio en la vista de diff del espacio de trabajo;
- ejecutarás la aplicación en local para comprobar el cambio en el navegador;
- crearás y fusionarás tu primera pull request.

## Situación

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de juegos de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como primer ejercicio, pedirás al agente que muestre la valoración existente en cada tarjeta: es un cambio pequeño e independiente, perfecto para tu primera sesión.

## Estructura de una sesión

Una **sesión** es una conversación con un agente que se ejecuta en su propio espacio de trabajo aislado. Cada sesión tiene un **worktree y una rama de Git propios**, lo que permite ejecutar varias sesiones a la vez —por ejemplo, una para añadir una función y otra para corregir un error— sin que sus cambios entren en conflicto. Las sesiones aparecen en la barra lateral agrupadas por repositorio; selecciona cualquiera para cambiar a ella.

Dentro de una sesión verás tres elementos: la **conversación** con el agente, su **actividad con herramientas** mientras explora y edita archivos, y la lista de **archivos modificados** con sus diffs.

## Inicia una sesión y solicita el cambio

Vamos a iniciar una sesión nueva para explorar el proyecto e implementar la función. Durante la [configuración de la aplicación][prior-lesson], añadiste el proyecto desde su repositorio de GitHub. Crearemos una sesión nueva para ese repositorio y pediremos el cambio.

1. Vuelve a la aplicación GitHub Copilot o ábrela.
2. Selecciona **+** en la parte superior de la barra lateral para iniciar una sesión.
3. Selecciona `tailspin-toys` como repositorio.
4. Elige un **new working tree** y el modo **Interactive** debajo del cuadro del prompt. Usa este prompt para solicitar el cambio:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pulsa <kbd>Enter</kbd> para enviar el prompt a Copilot.

La aplicación Copilot empieza creando un worktree nuevo, una copia aislada del proyecto. Después explora el proyecto, localiza los archivos que hay que actualizar para añadir la función y crea el código necesario. ¡Ya has añadido una función con la aplicación Copilot!

## Revisa el diff

Antes de fusionar cualquier cambio generado por IA, hay que revisarlo, aunque sea pequeño. Vamos a examinar los cambios en la aplicación Copilot.

1. En la esquina superior derecha de la aplicación, selecciona **Toggle review panel**. En el panel que se abre, selecciona **Changes** para ver los archivos modificados y sus diffs.

    ![Barra de herramientas superior de la aplicación GitHub Copilot con una flecha que apunta al botón Toggle review panel, a la derecha de Create PR](../../../_images/app-2-review-panel.png)

2. Deberías ver código añadido a `GameCard.astro`, el archivo principal que se usa para mostrar los detalles de los juegos. Debería ser parecido al siguiente: un pequeño bloque que muestra la valoración cuando existe y usa «No rating yet» cuando `starRating` es `null`:

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
> Como Copilot, al igual que todas las herramientas de IA generativa, es probabilístico y no determinista, el código exacto puede variar respecto al ejemplo. Debería ser relativamente parecido.

## Comprueba los cambios

Antes de abrir el navegador, revisa los resultados de las comprobaciones automatizadas del agente. Confirma que las pruebas cubren un `starRating` numérico y el valor alternativo para `null`. Si falta un requisito previo o se ha omitido una comprobación, no la des por superada; revisa cualquier solicitud de instalación antes de aprobarla.

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que abra el sitio web para que puedas examinar la interfaz actualizada. Le pediremos que inicie el sitio y lo abra en un lienzo de navegador.

> [!TIP]
> Un lienzo es un widget interactivo que está disponible dentro de la aplicación Copilot. Más adelante explorarás lienzos personalizados e incluso crearás uno, pero por ahora usaremos el lienzo de navegador integrado.

1. Usa este prompt para pedir a Copilot que inicie la aplicación y abra la página en el lienzo de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. En unos instantes, la aplicación se iniciará y se abrirá una ventana del navegador dentro de la aplicación Copilot.
3. Confirma que las tarjetas de juegos con valoración muestran su valor sobre cinco.
4. Cuando termines, pide a Copilot que detenga el servidor de desarrollo que ha iniciado para esta sesión y cierre el lienzo de navegador:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abre y fusiona tu primera pull request

Ya has creado la función. Es el momento de abrir una pull request (PR) para integrar el código nuevo en el proyecto.

1. Selecciona **Create PR** en la esquina superior derecha.
2. Si se te solicita, selecciona **Sign in with your browser** y sigue las instrucciones para autenticarte.
3. Si Copilot explica que la política del repositorio exige asociar una incidencia antes de crear la PR, confirma que debe seguir ese proceso.
4. Copilot empezará a crear la PR.
5. Selecciona la burbuja **PR**, justo encima del chat, para abrir la solicitud en el panel de revisión. Desde allí puedes revisar la PR.
6. Cuando esté lista, selecciona **Ready to merge**.
7. En el nuevo cuadro de diálogo, selecciona **Merge pull request** para fusionar la pull request.

## Resumen y siguientes pasos

¡Enhorabuena! Has entregado tu primer cambio con la aplicación GitHub Copilot. En concreto:

- has iniciado una sesión de agente y aprendido cómo se estructuran las sesiones;
- has indicado al agente que hiciera un cambio pequeño y centrado en las tarjetas de juegos;
- has revisado el cambio en la vista de diff del espacio de trabajo;
- has ejecutado la aplicación en local para comprobar la valoración con estrellas en el navegador;
- has abierto la PR 1, revisado sus comprobaciones y autorizado explícitamente la fusión.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una función más amplia.

## Recursos

- [Trabajar con sesiones de agente en la aplicación GitHub Copilot][agent-sessions]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]
- [Gestionar incidencias y pull requests con la aplicación GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instalar-y-configurar-la-aplicacion-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
