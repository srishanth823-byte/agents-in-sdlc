---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código base, propone cambios, edita archivos y ejecuta comandos. Como ese trabajo se realiza en tu equipo y en tiempo real, Copilot Chat en VS Code te muestra cada llamada a herramientas y edición de archivos a medida que ocurren. Puedes revisar cada diff en línea, aceptar o rechazar cambios concretos y orientar la conversación con prompts de seguimiento para ajustar o ampliar el trabajo sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad;
- revisarás y aceptarás los cambios propuestos en los archivos;
- guiarás al agente durante la sesión para añadir un control del modo claro junto al modo de alto contraste.

## Situación

El agente personalizado de accesibilidad que usaste en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño ha añadido otra solicitud: también quiere un control para el modo claro, destinado a las personas a quienes les resulta demasiado oscuro el tema predeterminado. En vez de iniciar otra conversación desde cero, guiarás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y los cambios que propone. Cada edición aparece en el chat con un diff y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a herramientas. Verás lecturas y ediciones de archivos, además de comandos del terminal cuya ejecución haya solicitado.
3. Cuando el agente proponga una edición, aparecerá un diff en línea:
   - Selecciona **Keep** para aceptar el cambio.
   - Selecciona **Undo** para revertirlo y dejar que el agente vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** en la parte superior del chat para ver todos los archivos que el agente ha modificado durante la sesión.

> [!NOTE]
> Si una llamada a herramientas requiere permiso (por ejemplo, para ejecutar un comando del terminal), el agente se detendrá y mostrará una solicitud **Allow**. Aprueba solo los comandos que te parezcan adecuados.

5. Espera a que el agente termine de implementar el modo de alto contraste. Deberías ver un mensaje final que resuma los cambios.

## Guía al agente para añadir el modo claro

Ya está implementado el modo de alto contraste. Amplía la misma conversación para añadir un control del modo claro. Como continúas en la misma sesión de chat, el agente conserva el contexto del trabajo anterior: los componentes modificados, el enfoque de estilos y el patrón de almacenamiento local.

1. En la entrada de Copilot Chat, envía este prompt:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente continuará desde el punto en que se quedó, revisará los cambios existentes y propondrá lo necesario para el modo claro.
3. Revisa cada cambio nuevo a medida que aparezca, igual que hiciste con el modo de alto contraste. Selecciona **Keep** o **Undo** para cada diff.
4. Si el agente toma una dirección que no te convence (por ejemplo, si intenta reorganizar la interfaz de controles de una forma que no te gusta), puedes detenerlo con el botón **Stop** situado junto a la entrada del chat y enviarle un mensaje de seguimiento para aclararlo.

> [!TIP]
> Guiar una sesión en curso es más rápido que empezar de nuevo. Usa los prompts de seguimiento para pedir ajustes («simplifica el CSS; usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración existente») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos que ha modificado el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa los diffs. Confirma que se aplican las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia mediante almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel **Source Control**. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y guiar una sesión en curso: has revisado cada cambio de archivo, aceptado o rechazado ediciones y enviado prompts de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección:

- has supervisado el trabajo del agente en tiempo real;
- has aceptado y rechazado ediciones de archivos;
- has guiado una sesión en curso mediante prompts de seguimiento.

En la [siguiente lección][next-lesson] publicarás el trabajo de accesibilidad en una pull request y revisarás los cambios que ha generado el agente personalizado local durante este laboratorio.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Usar el modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
