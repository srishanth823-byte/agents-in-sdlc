---
title: "Lección 2 - Añadir valoraciones con estrellas: un cambio rápido"
description: "Usa Copilot CLI para hacer un pequeño cambio en las tarjetas de juegos, revisarlo en un navegador reenviado y fusionarlo en tu primera pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Ahora que has instalado Copilot CLI y probado una conversación, harás tu primer cambio en el proyecto. Será pequeño: los datos de los juegos ya incluyen una valoración con estrellas, pero las tarjetas de la página de inicio todavía no la muestran. Pedirás al agente que la incluya, revisarás el cambio y lo fusionarás en tu primera pull request.

En esta lección:

- iniciarás una conversación centrada en una rama de función;
- pedirás al agente que haga un cambio pequeño en el proyecto;
- revisarás el cambio con `/diff`;
- iniciarás la aplicación para confirmar el cambio en un navegador reenviado;
- crearás y fusionarás tu primera pull request.

## Situación

Cada juego de Tailspin Toys puede tener una valoración con estrellas, que ya aparece en la página de detalles. Sin embargo, las tarjetas de juegos de la página de inicio solo muestran el título, la categoría, la editorial y la descripción. Como primer ejercicio, pedirás al agente que muestre la valoración existente en cada tarjeta: es un cambio pequeño e independiente, perfecto para tu primera sesión.

## Estructura de una conversación

Una **conversación** es el espacio donde trabajas con Copilot CLI en una tarea. A diferencia de la aplicación Copilot, una conversación normal de la CLI usa el repositorio y la rama que tienes actualmente en el terminal, en lugar de crear un worktree específico. Las conversaciones guardadas te permiten volver al mismo debate, mientras que los archivos y la rama siguen siendo el estado habitual de Git en el disco.

Dentro de una conversación verás tres cosas: los prompts y las respuestas del agente; su actividad con herramientas mientras explora y edita archivos; y los cambios que puedes revisar con `/diff`.

## Inicia una conversación y solicita el cambio

Vamos a iniciar una conversación nueva para implementar la función.

1. Vuelve a tu codespace.
2. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Si aún no lo has iniciado, ejecuta este comando para iniciar Copilot:

   ```bash
   copilot --yolo
   ```

4. Inicia una sesión nueva con el comando de barra `/new` y pulsa <kbd>Enter</kbd>.
5. Usa este prompt para solicitar el cambio:

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot explorará el proyecto, localizará los archivos que muestran los detalles de los juegos y creará el código necesario. ¡Ya has añadido una función con Copilot CLI!

## Revisa el diff

Antes de fusionar cualquier cambio generado por IA, hay que revisarlo, aunque sea pequeño. Vamos a examinar los cambios directamente en Copilot CLI.

1. Escribe `/diff` e inspecciona todos los archivos modificados.
2. Confirma que la tarjeta del juego muestra la valoración numérica cuando existe y `No rating yet` cuando `starRating` es `null`.
3. Confirma que las pruebas cubren ambos casos.
4. Revisa los resultados de las comprobaciones ejecutadas por Copilot y pídele que corrija los fallos.
5. Cuando termines la revisión, pulsa <kbd>Esc</kbd> para salir de la vista de diff.

> [!NOTE]
> Copilot, como todas las herramientas de IA generativa, es probabilístico, no determinista, por lo que el código exacto puede variar. Revisa el comportamiento, no esperes una implementación idéntica.

## Comprueba los cambios

No basta con leer el código y dar por hecho que funciona. Pide a Copilot que inicie el sitio para que puedas examinar la interfaz actualizada en el navegador reenviado por Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Confirma que las tarjetas de juegos muestran la valoración sobre cinco.
4. Vuelve a Copilot y pídele que detenga el servidor que ha iniciado:

   ```plaintext
   Stop the development server you started.
   ```

## Abre y fusiona tu primera pull request

Ya has creado la función. Es el momento de abrir una pull request (PR) para integrarla en el proyecto.

1. Pide al agente predeterminado que confirme el cambio:

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. Escribe `/pr create`. Copilot CLI enviará la confirmación existente al crear la PR y mostrará su URL.
3. Abre la PR manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la URL que muestra Copilot CLI.
4. Revisa los archivos modificados y las comprobaciones.
5. Cuando esté todo listo, selecciona **Merge pull request** y confirma la fusión.
6. Vuelve a tu codespace y sal de Copilot CLI con `/exit`.
7. Actualiza tu rama local `main`:

   ```bash
   git checkout main
   git pull
   ```

## Resumen y siguientes pasos

¡Enhorabuena! Has entregado tu primer cambio con GitHub Copilot CLI. En concreto:

- has iniciado una conversación centrada en una rama de función;
- has indicado al agente que hiciera un cambio pequeño en las tarjetas de juegos;
- has revisado el cambio con `/diff`;
- has iniciado la aplicación para comprobar la valoración con estrellas en un navegador reenviado;
- has creado y fusionado tu primera pull request.

A continuación, [partirás de la incidencia de filtrado y usarás los modos Plan y Autopilot][next-lesson] para crear una función más amplia.

## Recursos

- [Acerca de GitHub Copilot CLI][about-copilot-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
