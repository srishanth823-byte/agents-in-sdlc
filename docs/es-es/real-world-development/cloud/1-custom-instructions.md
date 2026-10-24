---
title: "Ejercicio 1 - Instrucciones personalizadas (Agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente de Copilot en la nube →][next-lesson]

El contexto es clave cuando trabajas con IA generativa. Si una tarea debe hacerse de una manera concreta, o hay información de fondo que Copilot debería conocer, conviene que ese contexto esté disponible. Los [archivos de instrucciones][instruction-files] te permiten proporcionar esa guía para que Copilot entienda no solo *qué* quieres, sino también *cómo* debe hacerlo.

En este ejercicio:

- explorarás cómo el contexto específico del proyecto, las directrices de programación y los estándares de documentación llegan a Copilot mediante las instrucciones personalizadas del repositorio y los archivos de instrucciones con ámbito de ruta;
- añadirás un estándar para todo el repositorio a `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los entornos de VS Code y CLI, aquí no ejecutarás un prompt «antes y después». El agente de Copilot en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar el impacto en una comparación inmediata. Verás cómo influyen las instrucciones más adelante en este entorno, cuando revises las pull requests del agente.

## Archivos de instrucciones

### Situación

Como cualquier buen equipo de desarrollo, Tailspin Toys tiene directrices y requisitos para sus prácticas de desarrollo, entre ellos:

- La capa de datos siempre necesita pruebas unitarias.
- La interfaz debe estar en modo oscuro y tener un aspecto moderno.
- Debe añadirse documentación al código mediante comentarios de documentación TSDoc.
- Debe añadirse al principio de cada archivo un bloque de comentarios que describa su función.

Con los archivos de instrucciones, te asegurarás de que Copilot dispone de la información necesaria para realizar las tareas de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas te permiten proporcionar contexto y preferencias a Copilot para que entienda mejor tu estilo de programación y tus requisitos. Esta potente función puede ayudarte a obtener sugerencias y fragmentos de código más relevantes. Puedes especificar tus convenciones de programación, bibliotecas e incluso los comentarios que prefieres incluir en el código. Puedes crear instrucciones para todo el repositorio o para determinados tipos de archivo, a fin de aportar contexto específico para una tarea.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md` es un único archivo de instrucciones que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información a nivel de proyecto, es decir, contexto relevante para la mayoría de las solicitudes que se envían a Copilot desde el chat o la CLI. Puede incluir la pila tecnológica, una descripción general de lo que se está creando, buenas prácticas y otras directrices globales.
- Puedes crear archivos `.github/instructions/*.instructions.md` para tareas o tipos de archivo concretos. Puedes usarlos para proporcionar directrices sobre lenguajes concretos, como TypeScript o Astro, o para tareas como crear un componente de interfaz o un nuevo conjunto de pruebas unitarias.

> [!NOTE]
> En el IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para sugerir la siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente de Copilot en la nube usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` (con frontmatter `applyTo`) al generar código.
>
> Además, Copilot [admite archivos de instrucciones que usan otros estándares][custom-instructions-support], como AGENTS.md y CLAUDE.md.

### Buenas prácticas para gestionar archivos de instrucciones

Una conversación completa sobre cómo crear archivos de instrucciones queda fuera del alcance de este taller. Sin embargo, los ejemplos del proyecto de muestra ofrecen un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en las directrices de todo el proyecto, como una descripción de lo que se está creando, la estructura del proyecto y los estándares globales de programación.
- Usa archivos `*.instructions.md` para proporcionar directrices específicas de tipos de archivo (pruebas unitarias, componentes Astro o capa de datos) o tareas.
- Usa lenguaje natural. Expresa las directrices con claridad y ofrece ejemplos de cómo debe ser el código y de cómo no debe ser.

No hay una única forma de crear archivos de instrucciones, igual que no hay una sola forma de usar la IA. La experimentación te ayudará a descubrir qué funciona mejor en tu proyecto.

> [!TIP]
> Todos los proyectos que usan GitHub Copilot deberían tener un conjunto sólido de archivos de instrucciones. Al explorar los archivos de este proyecto, quizá veas que hay instrucciones para muchos tipos de tareas, como las [actualizaciones de la interfaz][ui-instructions] y [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de una manera distinta (por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI); la lección del entorno que estés usando la mencionará cuando corresponda.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas de este proyecto

Dedica un momento a leer los archivos de instrucciones que incluye este repositorio: hay un archivo principal `copilot-instructions.md` y una colección de archivos `*.instructions.md` para varias tareas. Ábrelos en tu editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y en secciones como **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, fíjate en la directriz anidada **GitHub Actions Workflows**. Estas directrices se aplican a cualquier interacción que tengas con Copilot.
3. Abre la carpeta `.github/instructions` y explórala. Verás instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y más.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: establece un glob (relativo a la raíz del repositorio) que determina a qué archivos se aplican las instrucciones. En este caso, se aplican a los archivos de pruebas TypeScript que coincidan, por ejemplo, con `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y mostrar a Copilot ejemplos que seguir al generar código. (Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio).

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares de programación del proyecto, pero todavía no exige documentación dentro del código. En los pasos siguientes, añadirás reglas para los comentarios TSDoc y los encabezados de comentario a nivel de archivo.

## Añade un estándar para todo el repositorio

El siguiente paso es el único cambio que harás aquí: añadir una regla general al archivo `copilot-instructions.md` para que la documentación aparezca en el código como comentarios TSDoc y encabezados de comentario a nivel de archivo. El agente en la nube la tendrá en cuenta al trabajar en las incidencias que le asignes más adelante en este entorno.

Antes de editar, crea una rama de trabajo. El agente en la nube leerá las instrucciones de la rama a la que se asigne la incidencia, pero de todos modos confirmarás los cambios:

1. En el terminal del codespace, crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se indicó antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añade un estándar de programación al repositorio para mejorar las sugerencias de código.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Observa cómo documenta los estándares de programación, pero todavía no incluye una regla para la documentación dentro del código, por lo que el helper generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown justo debajo de los estándares actuales para indicar a Copilot que incluya encabezados de comentario a nivel de archivo y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como viste en la lección anterior, puedes crear archivos de instrucciones a nivel del repositorio (`.github/copilot-instructions.md`) para establecer directrices generales, o archivos `*.instructions.md` para lenguajes, tipos de archivo o tareas concretos. El archivo del repositorio es el lugar adecuado para estándares generales, como la regla de comentarios de documentación que acabas de añadir.

## Confirma, envía y fusiona la actualización de las instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se asigna la incidencia. Cuando le asignes una incidencia a Copilot en el siguiente ejercicio, Copilot creará una rama a partir de `main`. Por eso, tus cambios de instrucciones deben llegar a `main` para que el agente en la nube los use.

1. Prepara y confirma el cambio:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envía la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una pull request de `custom-instructions-cloud` a `main` y fusiónala. El agente en la nube leerá estas instrucciones cuando trabaje en las incidencias que le asignes a continuación.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios en `main`. El paso de la rama sirve para que el taller refleje cómo trabajarías en un proyecto real.

## Resumen y siguientes pasos

Has explorado cómo Copilot utiliza el contexto de los archivos de instrucciones de este proyecto y has añadido un estándar general a `.github/copilot-instructions.md`. Verás cómo se aplica en las pull requests que genere el agente en la nube durante el resto de este entorno.

A continuación, [asignarás tu primera incidencia al agente de Copilot en la nube][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar las respuestas de GitHub Copilot][instruction-files]
- [Buenas prácticas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
