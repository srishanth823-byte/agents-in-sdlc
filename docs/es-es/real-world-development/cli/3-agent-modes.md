---
title: "Lección 3 - Modos de agente: Plan y Autopilot"
description: "Usa Plan para acordar un enfoque, Autopilot para crear el filtrado a partir de una incidencia e Interactive para revisar el resultado."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Hemos empezado añadiendo una función pequeña al proyecto, pero los cambios más amplios necesitan un proceso más sólido. GitHub Copilot CLI está diseñado para integrarse en los flujos de trabajo de una organización y ayudarnos a crear lo que necesitamos de la forma adecuada. Esta es la primera de varias lecciones en las que seguirás un proceso habitual de desarrollo con agentes: empezarás con una incidencia para crear una función, comprobarás que el código es válido y que la función se comporta como esperas y, por último, integrarás el cambio en el proyecto.

> [!NOTE]
> Usarás la misma conversación y rama durante todo el flujo de trabajo de la función. Normalmente usarías ramas o PR distintas para los diferentes tipos de archivo, pero en este taller haremos una excepción para centrarnos en los conceptos principales.

En esta lección:

- iniciarás una conversación de Copilot a partir de una incidencia de GitHub;
- definirás los requisitos en el modo Plan;
- implementarás la función en el modo Autopilot;
- revisarás el código;
- comprobarás manualmente la función en un navegador reenviado.

Mientras desarrollas la función, actualizarás las instrucciones del repositorio, personalizarás la habilidad `quality-checks`, añadirás validación con MCP, crearás un agente de QA y abrirás la PR de la función.

## Situación

El catálogo de Tailspin Toys está creciendo y las visitas necesitan filtrar los juegos por categoría y editorial. La incidencia del backlog describe la función, pero hace falta acordar algunos detalles, como combinar categorías, antes de escribir código. Usarás el modo Plan para resolver esas decisiones y, después, autorizarás una implementación con un alcance definido.

## Contexto

Incorporar agentes de programación con IA al flujo de trabajo no cambia los fundamentos del desarrollo; de hecho, los hace aún más importantes. La mayoría de las personas desarrolladoras sigue un proceso parecido a este:

1. Empezar por una incidencia registrada que describa el trabajo.
2. Planificar lo que hay que crear.
3. Crear y revisar el código.
4. Ejecutar las pruebas para validar el código.
5. Validar manualmente la nueva funcionalidad.
6. Crear una pull request (PR).
7. Fusionar el código después de que se revise y de que se supere la integración continua.

> [!NOTE]
> Los detalles concretos dependerán del equipo y la organización, pero la mayoría seguirá alguna variante de este proceso.

Si sigues este enfoque estándar, te asegurarás de que el código generado por IA cumple los requisitos y pasa por el mismo proceso de revisión que el código escrito por una persona.

## Modos de conversación

El **modo de conversación** controla el nivel de autonomía del agente. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar de modo:

- **Interactive**: tú y el agente trabajáis en colaboración. El agente propone cambios y espera tus indicaciones antes de continuar.
- **Plan**: el agente crea un plan y no puede editar archivos del proyecto.
- **Autopilot**: el agente trabaja de forma autónoma: escribe código, ejecuta pruebas e itera hasta completar la tarea.

Empieza en el modo Plan, revisa el plan y, después, usa Autopilot para implementarlo.

## Empieza por la incidencia

Antes de empezar a trabajar en el filtrado, vuelve a tu codespace y comprueba que el repositorio y el terminal están preparados.

1. Vuelve a tu codespace. Si está detenido, reinícialo antes de continuar.
2. Confirma que se ha fusionado la PR de las valoraciones con estrellas.
3. Si el terminal no está abierto, pulsa <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
4. Actualiza `main` y crea una rama para el trabajo de filtrado:

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Inicia Copilot CLI:

   ```bash
   copilot --yolo
   ```

6. Pulsa <kbd>Tab</kbd> dos veces para abrir la pestaña **Issues**.
7. Pulsa <kbd>A</kbd> para mostrar todas las incidencias.
8. Usa las teclas de dirección para seleccionar la incidencia **Allow users to filter games by category and publisher**.
9. Pulsa <kbd>C</kbd> para añadir la incidencia al prompt y volver a la pestaña **Session**.

Observa que ahora el prompt empieza por `#7` (o un número parecido). El símbolo `#` permite añadir al contexto una incidencia o pull request (PR) de GitHub.

## Planifica la función de filtrado

La planificación te permite definir cómo se implementará una función o se realizará una tarea antes de asignársela a Copilot. Siempre conviene dedicar tiempo a planificar las tareas complejas. Cambiemos al modo Plan y pidamos a Copilot que elabore el plan.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para cambiar al modo Plan. Confirma que el indicador bajo el prompt muestra **Plan**.
2. Después de la referencia a la incidencia que añadiste en el paso anterior, escribe este prompt:

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot empezará a preparar el plan. Primero explorará el proyecto y después decidirá cómo abordar la tarea.

3. Durante el proceso, Copilot podría hacerte preguntas sobre el funcionamiento de los filtros. Respóndelas según tus preferencias. No hay una única respuesta correcta.
4. Cuando el plan esté listo, pulsa <kbd>Control</kbd>+<kbd>E</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) para expandirlo.
5. Desplázate por el plan para revisarlo.
6. Pide a Copilot que cambie cualquier parte que no coincida con tus decisiones.

## Autoriza Autopilot

Una vez escrito y revisado el plan, es hora de implementarlo. Vamos a dejar que Copilot trabaje con el modo Autopilot.

Autopilot permite que Copilot itere sobre el problema hasta que considera que lo ha resuelto.

1. Selecciona **Accept plan and build on autopilot (recommended)** o la opción con un nombre parecido que aparezca en la versión instalada.
2. Confirma que el indicador bajo el prompt muestra **Autopilot**.
3. Observa cómo Copilot sigue el plan, genera código y ejecuta pruebas.

> [!NOTE]
> La aprobación puede iniciar la implementación de inmediato, así que revisa el plan antes de aceptarlo. Si Copilot indica que faltan dependencias o que hay un conflicto de puertos, resuelve el problema de configuración antes de dar por completadas las comprobaciones.

## Revisa y verifica la implementación

Una vez generado el código, debes revisarlo antes de fusionarlo, como harías con cualquier otro cambio. Revisemos el código y ejecutemos el sitio para comprobar que todo funciona correctamente.

1. Pulsa <kbd>Shift</kbd>+<kbd>Tab</kbd> para volver al modo Interactive. Confirma que el indicador ya no muestra **Plan** ni **Autopilot**.
2. Escribe `/diff` e inspecciona la implementación del filtrado y las pruebas.
3. Compara el resultado con la incidencia y las decisiones tomadas durante la planificación.
4. Cuando hayas revisado el código, pulsa <kbd>Esc</kbd> para salir de la vista de diff.
5. Revisa los resultados de las comprobaciones del proyecto y pide a Copilot que resuelva los fallos.

## Explora la nueva funcionalidad

El código parece correcto, pero ¿funciona? Inicia la aplicación como antes y abre el sitio mediante el puerto reenviado de Codespaces.

1. Pide a Copilot que inicie la aplicación:

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Cuando Codespaces indique que el puerto `4321` está disponible, selecciona **Open in Browser**.
3. Prueba el filtrado por categoría, el filtrado por editorial y las combinaciones acordadas en el plan.
4. Confirma que el restablecimiento y el comportamiento sin resultados coinciden con la incidencia y tus decisiones.
5. Vuelve al codespace y pide a Copilot que detenga el servidor de desarrollo que ha iniciado.

## Resumen y siguientes pasos

Has usado distintos modos de conversación para crear y revisar una función. En esta lección:

- has iniciado una conversación de Copilot a partir de una incidencia de GitHub;
- has definido los requisitos en el modo Plan;
- has implementado la función en el modo Autopilot;
- has revisado el código;
- has comprobado manualmente la función en un navegador reenviado.

A continuación, veremos cómo orientar la generación de código para que siga las prácticas documentadas mediante las [instrucciones personalizadas][next-lesson].

## Recursos

- [Autopilot en GitHub Copilot CLI][autopilot]
- [Referencia de comandos de Copilot CLI][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
