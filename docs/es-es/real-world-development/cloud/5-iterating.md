---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este laboratorio has trabajado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funciones. Le has pedido que añada documentación al código, cree una función de juegos relacionados para que el equipo de diseño pueda iterar sobre ella e implemente funciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisemos los cambios de código y, si es necesario, demos indicaciones a Copilot para mejorar el resultado.

### Situación

Como ya hemos señalado varias veces, los fundamentos del diseño de software y DevOps no cambian por añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con eso en mente, vamos a revisar las sugerencias de GitHub Copilot para crear documentación, la función de juegos relacionados y las mejoras de accesibilidad antes de activar las revisiones para el resto del equipo.

## Seguridad y agente de GitHub Copilot en la nube

Como el agente de Copilot en la nube realiza tareas de forma asíncrona y sin supervisión, se han establecido ciertas medidas de seguridad:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para sus cambios.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero para trabajar.
- Una persona debe aprobar cualquier flujo de trabajo de GitHub Actions antes de que se ejecute.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera pull request (PR) que generó el agente de GitHub Copilot en la nube: la que añade documentación al código. Usarás la interfaz estándar de PR de GitHub.com.

> [!NOTE]
> Al abrir la PR, quizá veas un aviso que indica que el cortafuegos ha bloqueado a GitHub Copilot. Es lo esperado: Copilot tiene acceso limitado de forma predeterminada a los recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente de Copilot en la nube][agent-firewall].

1. Vuelve al repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de pull requests.
3. Abre la pull request titulada algo parecido a **Add missing documentation** o con un título más descriptivo.

> [!NOTE]
> Si Copilot todavía está trabajando en la tarea, la pull request tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes tomarte un descanso o repasar lo que has aprendido.

4. Cuando la pull request esté lista, selecciona la pestaña **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que incluye los comentarios de documentación TSDoc y otros cambios de documentación. El resultado exacto puede variar.

    Al revisar los cambios, busca comentarios de documentación TSDoc, convenciones de TypeScript y encabezados de comentario. Proceden de los archivos de instrucciones personalizadas que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían finalizar correctamente. Puede tardar unos minutos.

## Pide cambios a GitHub Copilot

Trabajar con Copilot en una pull request no es una actividad unidireccional. Puedes mencionarlo en comentarios de la pull request o en comentarios insertados en el código, como harías con cualquier miembro del equipo. Copilot verá los comentarios e iniciará otra sesión para atenderlos. Como los resultados no son deterministas, no podemos ofrecer un texto prescriptivo. Algunas ideas para pedirle cambios son:

- Añadir encabezados de comentario al principio de cada archivo de código con una breve descripción de su función.
- Añadir comentarios de documentación TSDoc a los archivos TypeScript y Astro.
- Crear un README que describa la estructura de la aplicación Astro.

1. Añade un comentario para pedir un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Puedes usar una de las ideas anteriores o proponer otro cambio de documentación.
2. Selecciona **View Session** para observar el trabajo de Copilot. Verás que inicia una sesión nueva para hacer los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la pull request.

    ![Volver a la pull request](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot haya terminado, aparecerá una nueva confirmación en la pull request.
5. Selecciona la pestaña **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que el resultado te satisfaga. Cuando esté listo, puedes marcar la PR como lista para revisión y fusionarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos a la PR que generó Copilot para resolver la incidencia sobre los juegos relacionados en la página de detalles de cada juego.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título se parezca a **Show related games on the game details page** o sea más descriptivo.
4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
7. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían finalizar correctamente. Puede tardar unos minutos.
9. **Opcional:** Puedes cambiar a esta rama en tu codespace para probar manualmente la función de juegos relacionados. Ve al codespace, abre el terminal y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot ha creado la función de juegos relacionados. Como antes, puedes iterar con el agente de Copilot en la nube para pedir cambios. Por ejemplo, puedes pedirle que ajuste la cantidad de juegos relacionados que muestra o que añada encabezados de comentario y comentarios TSDoc (recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas). Puedes enviar estas solicitudes mediante un comentario nuevo en la pestaña **Conversation**; Copilot lo verá e iniciará una sesión nueva.

## Revisa las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado de accesibilidad. Esta PR debería incluir el modo de alto contraste que asignaste en el Ejercicio 3 y el modo claro que solicitaste desde mission control en el Ejercicio 4.

1. Vuelve al repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR cuyo título se parezca a **Add high contrast mode to website** o sea más descriptivo.

> [!NOTE]
> Si Copilot todavía está trabajando en la tarea, la pull request tendrá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos.

4. Selecciona la pestaña **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de la interfaz para cambiar entre modos;
   - el uso del almacenamiento local para conservar las preferencias;
   - los cambios de CSS o estilos para los modos de alto contraste y claro;
   - los atributos de accesibilidad (etiquetas ARIA, navegación con teclado, etc.);
   - cualquier código JavaScript o TypeScript que gestione el cambio de modo.

6. Cuando hayas revisado los cambios y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador que señala que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos de trabajo pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos de trabajo deberían ponerse en cola en la sección de comprobaciones de la pull request. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro deberían finalizar correctamente. Puede tardar unos minutos.
10. **Opcional:** Puedes cambiar a esta rama en tu codespace para probar manualmente las funciones de accesibilidad. Ve al codespace, abre el terminal y ejecuta los comandos siguientes (sustituye `<branch-name>` por el nombre de la rama creada por Copilot):

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    A continuación, inicia la aplicación y prueba los controles de modo de alto contraste y modo claro en el navegador. Confirma que funcionan y que las preferencias se conservan al volver a cargar la página.

Observa cómo el agente personalizado de accesibilidad ayudó a Copilot a implementar estas funciones siguiendo buenas prácticas de accesibilidad. Si detectas algún problema de accesibilidad o una posible mejora, puedes mencionar a **@copilot** en un comentario y pedir que lo solucione, como hiciste con las PR anteriores.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta más útil cuando puedes asignarle tareas reales del backlog y ocuparte mientras tanto de otras cosas. Para adquirir este hábito, crea varias incidencias en tu repositorio y asígnaselas a Copilot. Algunas ideas:

- Crear un formulario para expresar interés en un juego en la página de detalles.
- Implementar la paginación en la página de la lista de juegos.
- Añadir validación de entrada y gestión de errores a los helpers de acceso a datos.

## Resumen

Has completado el entorno del agente en la nube. En estas lecciones:

- **Has revisado los archivos de instrucciones personalizados que incluye el repositorio** para observar más adelante su efecto en el agente en la nube.
- **Has asignado incidencias al agente de Copilot en la nube** y has visto cómo configura su entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para implementar funciones de accesibilidad con modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y guiar la sesión de accesibilidad en curso.
- **Has revisado e iterado sobre las pull requests del agente en la nube**, mencionando a `@copilot` para pedir cambios y aprobando flujos de trabajo.

## Repaso y siguientes pasos

Has completado el entorno del agente en la nube. Si quieres seguir explorando, los otros entornos complementan lo que has practicado:

- 🖥️ **[Entorno de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración de MCP directamente desde el IDE.
- 💻 **[Entorno de CLI](../../cli/)**: entrega dos cambios revisados desde el terminal con los modos Plan y Autopilot, habilidades de agente, un agente personalizado de QA, Playwright MCP, Agent Merge y un puente opcional `/delegate` de vuelta al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- Asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- Crea un agente personalizado para otro ámbito.
- Configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Buenas prácticas para el flujo de configuración del agente de Copilot en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente de Copilot en la nube][agent-firewall]

---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
