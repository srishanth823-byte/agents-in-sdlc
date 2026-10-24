---
title: "Lección 10 - Resumen y siguientes pasos"
description: "Repasa el flujo de trabajo con Copilot CLI, las dos pull requests, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI en un flujo de trabajo continuo con Tailspin Toys. Has:

- preparado un codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado con datos de ejemplo;
- añadido valoraciones con estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primera pull request (PR);
- empezado por la incidencia de filtrado, definido el enfoque en el modo Plan, implementado la función en el modo Autopilot y revisado el resultado en el modo Interactive;
- guiado al agente con instrucciones personalizadas y, después, personalizado la habilidad `quality-checks` existente para ejecutar pruebas unitarias, lint y comprobaciones de tipos;
- añadido el servidor Model Context Protocol (MCP) de Playwright y usado el navegador para explorar el filtrado;
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de las habilidades y las pruebas del navegador;
- revisado todos los cambios de filtrado y autorizado Agent Merge para la PR de filtrado;
- explorado comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar trabajo opcionalmente en la nube.

## Cambios entregados

El taller incluye dos hitos de pull request, cada uno en su propia rama creada desde `main` actualizada:

1. **Valoraciones con estrellas:** muestra el `starRating` existente y un estado explícito para los juegos sin valorar en las tarjetas.
2. **Filtrado y flujo de calidad:** implementa el filtrado, actualiza las instrucciones y las aplica a la función, personaliza el informe de `quality-checks`, crea un perfil de QA e incluye las pruebas correspondientes.

Desde la planificación del filtrado hasta la creación de su pull request, has usado la misma conversación y rama. Para simplificar el taller, agrupamos el trabajo en una sola PR.

## Diferentes tipos de verificación

Has verificado el código de varias formas: pruebas automatizadas, una comprobación manual en el navegador y una exploración del navegador por parte de Copilot mediante MCP. La habilidad `quality-checks` ejecutó las pruebas unitarias, el lint y las comprobaciones de tipos, e informó de los resultados con el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura de las pruebas antes de crear la PR.

Las pruebas nuevas deben cubrir carencias reales; puede ser correcto que una ejecución de QA no necesite añadir ninguna. Revisa el código y las pruebas antes de autorizar la fusión, y vuelve a obtener las pruebas afectadas después de hacer cambios.

## Buenas prácticas

El contexto y las herramientas que proporcionas a Copilot influyen en su trabajo. En este taller has actualizado instrucciones, personalizado una habilidad, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones en distintas conversaciones y ajústalas a las necesidades del equipo. Las instrucciones definen los estándares, las habilidades describen tareas repetibles, los agentes personalizados establecen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Elige el **modo y el modelo** según la tarea. Usa **Plan** para definir el enfoque antes de empezar, **Interactive** para participar en los cambios centrados y **Autopilot** para tareas bien delimitadas. Elige un modelo más rápido para las ediciones rutinarias y otro más capaz para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Explicar con claridad *qué* quieres crear, *por qué* y *cómo* cambia de forma significativa el resultado.

## Más cosas que explorar

Ya has cubierto el flujo de trabajo principal. Estas son algunas funciones más de la CLI que puedes probar:

- `/review` para pedir al agente de revisión de código que analice los cambios;
- `/rubber-duck` para analizar un problema y obtener otra perspectiva;
- `/fleet` para coordinar subtareas independientes en paralelo;
- `/worktree` para aislar otra tarea;
- `/delegate` para enviar una tarea al agente de Copilot en la nube.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala para código de producción, proyectos personales o esa pequeña aplicación que tienes en mente desde hace años y nunca habías llegado a crear. Comparte lo que aprendas con el equipo y aprende también de las demás personas. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional de [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente de Copilot en la nube][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar pull requests con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
