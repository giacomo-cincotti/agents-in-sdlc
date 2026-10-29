---
title: "Ejercicio 1 - Instrucciones personalizadas (agente en la nube)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Agente en la nube de Copilot →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta, o Copilot necesita conocer información de fondo, debes asegurarte de que pueda acceder a ese contexto. Los [archivos de instrucciones][instruction-files] proporcionan esa orientación para que Copilot entienda no solo *qué* quieres que haga, sino también *cómo*.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas para ciertas rutas.
- añadirás un nuevo estándar para todo el repositorio en `.github/copilot-instructions.md`.

> [!NOTE]
> A diferencia de los itinerarios de VS Code y CLI, aquí no compararás los resultados de un mensaje antes y después de cambiar las instrucciones. El agente en la nube trabaja de forma asíncrona en incidencias de GitHub, por lo que es más difícil mostrar el efecto en tiempo real. Verás la influencia de las instrucciones más adelante, cuando revises las PR creadas por el agente.

## Archivos de instrucciones

### Escenario

Como cualquier empresa de desarrollo competente, Tailspin Toys tiene pautas y requisitos para sus prácticas de desarrollo. Entre ellos:

- La capa de datos siempre debe tener pruebas unitarias.
- La interfaz debe usar el modo oscuro y tener un aspecto moderno.
- El código debe incluir documentación en forma de comentarios TSDoc.
- Cada archivo debe empezar con un bloque de comentarios que describa su propósito.

Con los archivos de instrucciones te asegurarás de que Copilot tenga la información necesaria para trabajar de acuerdo con estas prácticas.

### Instrucciones personalizadas

Las instrucciones personalizadas proporcionan a Copilot contexto y preferencias para que comprenda mejor tu estilo de programación y tus requisitos. Te permiten orientar sus sugerencias y fragmentos de código. Puedes especificar las convenciones de programación, las bibliotecas e incluso los comentarios que prefieres. Puedes crear instrucciones para todo el repositorio o para tipos concretos de archivos y tareas.

Hay dos tipos de archivos de instrucciones:

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes de chat o CLI, como la pila tecnológica, un resumen de lo que se desarrolla, las prácticas recomendadas y otras pautas generales.
- `.github/instructions/*.instructions.md`: archivos para tareas o tipos de archivos específicos. Puedes proporcionar pautas para determinados lenguajes, como TypeScript o Astro, o para tareas como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> Cuando trabajas en un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
>
> Copilot Chat, Copilot CLI y el agente en la nube de Copilot usan tanto las instrucciones del repositorio como los archivos `*.instructions.md` con metadatos `applyTo`.
>
> Copilot también admite archivos de instrucciones de otros estándares, como AGENTS.md y CLAUDE.md.

### Prácticas recomendadas para gestionar archivos de instrucciones

Aunque una explicación completa sobre cómo crear archivos de instrucciones queda fuera del ámbito del taller, los ejemplos del proyecto muestran un enfoque representativo. En general:

- Mantén `copilot-instructions.md` centrado en las pautas del proyecto, como su descripción, estructura y estándares globales de programación.
- Usa `*.instructions.md` para instrucciones específicas de tipos de archivos —pruebas unitarias, componentes de Astro o capa de datos— o de tareas concretas.
- Escribe en lenguaje natural. Explica las pautas con claridad e incluye ejemplos del código adecuado e inadecuado.

No hay una única manera correcta de crear archivos de instrucciones, igual que no hay una única manera de usar la IA. La experimentación te ayudará a encontrar el enfoque adecuado para tu proyecto.

> [!TIP]
> Todo proyecto que use GitHub Copilot debería tener una colección sólida de archivos de instrucciones. Mientras exploras los archivos de este proyecto, verás que hay instrucciones para distintos tipos de tareas, como [actualizar la interfaz][ui-instructions] y trabajar con [Astro][astro-instructions].
>
> Copilot también puede ayudarte a generar archivos de instrucciones. Cada entorno ofrece esta función de forma distinta; por ejemplo, **Configure Chat → Generate Agent Instructions** en VS Code o `/init` en Copilot CLI. La lección correspondiente explicará cómo hacerlo.
>
> ¿Buscas plantillas o un punto de partida? Explora [awesome-copilot][awesome-copilot], un repositorio con archivos de instrucciones, agentes personalizados y otros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explora los archivos de instrucciones personalizadas del proyecto

Dedica un momento a leer los archivos de instrucciones incluidos en este repositorio: hay un archivo principal `copilot-instructions.md` y varios archivos `*.instructions.md` para diferentes tareas. Ábrelos en el editor o en la interfaz web de GitHub.

1. Abre `.github/copilot-instructions.md`.
2. Explora el archivo y fíjate en la breve descripción del proyecto y las secciones **Agent notes**, **Code standards**, **Scripts** y **Repository Structure**. En **Code standards**, observa las pautas anidadas para los flujos de trabajo de GitHub Actions. Se aplican a cualquier interacción con Copilot.
3. Abre la carpeta `.github/instructions` y explora su contenido. Hay instrucciones para archivos Astro, la capa de datos de Drizzle, pruebas y otros ámbitos.
4. Abre `.github/instructions/unit-tests.instructions.md`. Fíjate en el campo `applyTo` de la parte superior: define un patrón glob, relativo a la raíz del repositorio, que determina a qué archivos se aplican las instrucciones. En este caso, coincide con los archivos de pruebas TypeScript, como los que siguen el patrón `**/*.test.ts`.
5. Observa las instrucciones específicas para crear pruebas unitarias en este proyecto.
6. Por último, abre `.github/instructions/drizzle.instructions.md` y desplázate hasta el final. Fíjate en los enlaces a otros archivos de instrucciones, como `unit-tests.instructions.md`, y a archivos existentes del proyecto. Así puedes dividir conjuntos de instrucciones grandes en archivos más pequeños y reutilizables, y señalar ejemplos que seguir. Las rutas son relativas al archivo de instrucciones, no a la raíz del repositorio.

> [!NOTE]
> La sección **Code formatting requirements** de `copilot-instructions.md` documenta los estándares del proyecto, pero aún no exige documentación en el código. En los siguientes pasos añadirás reglas para los comentarios TSDoc y los comentarios de cabecera de los archivos.

## Añade un estándar para todo el repositorio

El siguiente paso consiste en añadir una regla del proyecto para incluir comentarios TSDoc y un comentario de cabecera en los archivos. El agente en la nube la aplicará a las incidencias que le asignes más adelante.

Antes de editar, crea una rama (el agente en la nube leerá las instrucciones de la rama a la que se dirija la incidencia, pero también confirmarás los cambios de forma ordenada):

1. Desde el terminal de tu Codespace, crea una rama y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Como se ha explicado antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añadamos los estándares de programación para mejorar sus sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero aún no incluye reglas para documentar el código; por eso el asistente generado no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown debajo de los estándares actuales para pedir comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para pautas globales o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. Las reglas de documentación del código que acabas de añadir son estándares generales del proyecto, por lo que corresponden al archivo del repositorio.

## Confirma, publica y fusiona la actualización de instrucciones

El agente en la nube lee los archivos de instrucciones de la rama a la que se dirige la incidencia. Cuando le asignes una incidencia en el siguiente ejercicio, partirá de `main`; por eso, los cambios de instrucciones deben llegar a `main` para que pueda usarlos.

1. Prepara la confirmación y créala:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Publica la rama:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. En github.com, abre una solicitud de incorporación de cambios de `custom-instructions-cloud` a `main` y fusiónala. Así, el agente en la nube leerá estas instrucciones cuando le asignes las incidencias del siguiente ejercicio.

> [!TIP]
> Si prefieres trabajar directamente en `main` durante este taller, puedes omitir la rama y confirmar los cambios directamente en `main`. El paso de la rama permite practicar un flujo de trabajo habitual en proyectos reales.

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has añadido un estándar para todo el repositorio en `.github/copilot-instructions.md`. Verás cómo influye ese estándar en las PR que genere el agente en la nube durante el resto del itinerario.

A continuación, [asignarás tu primera incidencia al agente en la nube de Copilot][next-lesson].

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
