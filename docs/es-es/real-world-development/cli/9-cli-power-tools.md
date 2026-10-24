---
title: "Lección 9 - Comandos de barra en GitHub Copilot CLI"
description: "Explora comandos de barra para gestionar el contexto, elegir modelos, compartir sesiones y delegar trabajo opcionalmente al agente en la nube."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como cualquier buena herramienta de CLI, GitHub Copilot CLI incluye muchos comandos de barra. Permiten acceder a funciones avanzadas, información sobre lo que ocurre en segundo plano y opciones de configuración adicionales. Ya has usado comandos como `/diff`, `/mcp`, `/skills`, `/agent` y `/pr`. Vamos a explorar otros que también resultan útiles.

En esta lección:

- usarás `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- usarás `/model` para consultar los modelos disponibles;
- aprenderás a usar `/share` para exportar o compartir una sesión;
- explorarás comandos opcionales para trabajar en paralelo, usar worktrees y delegar tareas al agente en la nube.

## Situación

Ya has completado el flujo de trabajo principal de la CLI. Ahora veremos otras funciones: gestionar el contexto, cambiar de modelo, compartir sesiones y delegar trabajo opcionalmente al [agente de Copilot en la nube][about-cloud-agent].

## Explorar el contexto de Copilot CLI

Al trabajar en tareas grandes o complejas, podrías acercarte al límite de la ventana de contexto del modelo. Copilot CLI compacta la conversación automáticamente cuando hace falta; también puedes consultar o compactar el contexto mediante comandos de barra.

1. Vuelve a tu codespace e inicia Copilot CLI desde la raíz del repositorio si aún no está abierto.
2. Escribe:

   ```plaintext
   /context
   ```

3. Fíjate en el modelo, el uso actual de tokens y cómo se divide el contexto entre las instrucciones del sistema, las herramientas, los mensajes y el espacio libre.
4. Compacta la conversación:

   ```plaintext
   /compact
   ```

5. Escribe `/context` de nuevo y compara el resultado. Es posible que no haya grandes diferencias si la conversación ya es corta.

> [!NOTE]
> Copilot CLI compacta el contexto automáticamente cuando se llena la ventana. Usa `/compact` si quieres elegir cuándo hacerlo. Usa `/clear` o `/new` cuando cambies a una tarea sin relación y quieras empezar una conversación nueva.

## Elegir un modelo

Los modelos tienen distintos puntos fuertes y cada persona tiene sus preferencias. Copilot CLI te permite consultar los modelos disponibles y elegir uno.

1. Escribe:

   ```plaintext
   /model
   ```

2. Explora los modelos disponibles y la información sobre su uso.
3. Mantén el modelo actual, selecciona otro o pulsa <kbd>Esc</kbd> para cerrar la lista.

## Compartir una sesión

Trabajar en equipo y compartir lo aprendido ayuda a que todo el mundo aproveche mejor las herramientas de IA. El comando `/share` puede exportar una sesión a un archivo Markdown o HTML, crear un enlace para compartir o publicar un GitHub gist.

1. Escribe `/help` y revisa las opciones de `/share` disponibles en la versión instalada.
2. Si quieres compartir la sesión, elige un destino adecuado, como `/share file` para exportarla a un archivo Markdown local.
3. Revisa el contenido exportado antes de enviarlo a alguien o publicarlo. Las exportaciones de sesiones pueden incluir prompts, respuestas e información del proyecto.

Publicar un enlace o gist es opcional. No publiques contenido del repositorio o de la conversación que tu equipo no tenga previsto compartir.

## Opcional: ampliar el trabajo o delegarlo

El taller principal ya ha terminado. Copilot CLI también ofrece comandos para tareas más grandes:

- `/fleet` puede repartir subtareas independientes entre subagentes y ejecutarlas en paralelo.
- `/worktree` puede crear un worktree de Git aislado para una tarea aparte.
- `/delegate` puede enviar una tarea al agente de Copilot en la nube, que trabaja de forma asíncrona y puede abrir una pull request.

Estos comandos son opcionales porque pueden crear worktrees adicionales o trabajo remoto. Si quieres probarlos, empieza por una tarea nueva y con un alcance claro, y revisa el resultado mediante el flujo de trabajo habitual. Si quieres profundizar en el trabajo asíncrono con agentes, continúa con el [taller del agente en la nube][cloud-workshop].

## Resumen y siguientes pasos

Los comandos de barra te permiten configurar Copilot CLI, compartir sesiones y consultar lo que ocurre en segundo plano. En esta lección:

- has usado `/context` y `/compact` para explorar cómo gestiona Copilot el contexto de la conversación;
- has usado `/model` para consultar los modelos disponibles;
- has aprendido a usar `/share` para exportar o compartir una sesión;
- has explorado comandos opcionales para trabajar en paralelo, usar worktrees y delegar trabajo al agente en la nube.

Copilot CLI tiene más comandos de barra y muchas otras funciones que explorar. Terminemos el recorrido [repasando lo aprendido][next-lesson] y los siguientes pasos.

## Recursos

- [Referencia de comandos de Copilot CLI][cli-reference]
- [Gestión del contexto en Copilot CLI][context-management]
- [Acerca del agente de Copilot en la nube][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
