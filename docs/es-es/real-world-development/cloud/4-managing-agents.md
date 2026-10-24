---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente de Copilot en la nube que se ocupe de tres tareas distintas para mejorar la experiencia de usuario y añadir funciones. Aunque está diseñado para trabajar de forma asíncrona y autónoma, sigue siendo importante poder supervisar esas tareas.

Dispones de varias herramientas para gestionar las tareas asignadas al agente en la nube, como la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de los agentes que tienen pull requests abiertas. Puedes explorar las operaciones realizadas e incluso guiar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- guiarás una sesión en curso para pedir que añada otra función.

## Situación

Después de asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar el estilo del sitio y añadir controles para cambiarlo, parecía lógico incluir esta función. Quieres orientar el trabajo del agente para que añada tanto el modo claro como el modo de alto contraste.

## Revisa las tareas del agente de Copilot en la nube

Vamos a consultar el estado de las tareas asignadas al agente de Copilot en la nube.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código base;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - crear el entorno de trabajo de Copilot;
   - configurar servidores MCP;
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos exactos dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Fíjate también en el panel de pull requests (PR) que aparece a la derecha. Desde allí puedes consultar la PR y los archivos modificados para supervisar el trabajo.

## Guía al agente en la nube

Ya has visto las tareas activas. Ahora vamos a pedirle al agente que añada un modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que use Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en mission control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes como los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para guiar una sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud al flujo de trabajo cuando termine el paso que estaba realizando al recibir el mensaje.

El proceso tardará varios minutos, así que puedes aprovechar el tiempo para repasar todo lo que has aprendido y explorado.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro de GitHub Copilot para supervisar y guiar las tareas del agente en la nube. Desde aquí puedes seguir todas las tareas activas y completadas, revisar el trabajo en curso e incluso redirigir tareas para cambiar su alcance o dar indicaciones adicionales.

Has explorado estos conceptos:

- has explorado mission control y la página de agentes para supervisar las tareas del agente en la nube;
- has redirigido una sesión en curso para pedir que se añada otra función.

Mientras Copilot termina el trabajo de accesibilidad, puedes [iterar sobre las pull requests que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
