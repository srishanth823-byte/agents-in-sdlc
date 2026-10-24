---
title: "Lección 8 - Crear y fusionar la PR de la función"
description: "Revisa el filtrado, las instrucciones, la habilidad actualizada, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las instrucciones, la habilidad actualizada, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es hora de revisar todo junto y abrir una pull request. Fusionaste la pull request (PR) de las valoraciones con estrellas manualmente; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> En condiciones normales, separaríamos la función, las instrucciones, los cambios de la habilidad y el agente de QA en varias PR. Para simplificar el taller, has mantenido todo el flujo de filtrado y calidad en una conversación y una rama; todo irá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una pull request;
- inspeccionarás todos los cambios de la función y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge solo después de revisar los cambios y confirmarás que la PR se ha fusionado.

## Situación

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el resto del trabajo de la PR, sin quitar a la persona desarrolladora el control para autorizar la fusión.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una pull request. Cuando lo habilitas, Copilot se ocupa de los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a los comentarios de revisión y vuelve a aplicar los cambios sobre la rama base cuando hace falta. Después habilita la fusión automática de GitHub si la configuración del repositorio lo permite.

Hasta ahora has seleccionado **Merge pull request** manualmente. Agent Merge puede encargarse de ese proceso. Revisa los cambios y confirma que están listos antes de habilitarlo.

## Usa Agent Merge para gestionar la PR

Ya has creado el código. Ahora revisémoslo, creemos la PR y dejemos que Agent Merge gestione el resto.

1. Vuelve a tu codespace.
2. Abre el cuadro de diálogo del agente con `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot supervisará el proceso de integración continua de la PR. Cuando todo salga bien, completará la fusión.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo indicado y completa manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varias partes del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar la pull request. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una pull request;
- has inspeccionado todos los cambios de la función y las pruebas de verificación;
- has creado la PR de filtrado;
- has habilitado Agent Merge solo después de revisar los cambios y has confirmado que la PR se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto y los modelos, compartir sesiones y delegar trabajo opcionalmente en la nube.

## Recursos

- [Gestionar pull requests con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
