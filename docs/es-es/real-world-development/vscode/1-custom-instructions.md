---
title: "Ejercicio 1 - Instrucciones personalizadas (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lección anterior: Requisitos previos][previous-lesson] · [Siguiente lección: Modo de agente →][next-lesson]

El contexto es clave cuando se trabaja con IA generativa. Si una tarea debe realizarse de una forma concreta o Copilot necesita conocer información de fondo, debes asegurarte de que pueda acceder a ese contexto. Hay varias formas de compartir información específica con Copilot. Una de las principales son los [archivos de instrucciones][instruction-files], que proporcionan pautas para generar código.

En este ejercicio:

- explorarás cómo llegan a Copilot el contexto del proyecto, las pautas de programación y los estándares de documentación mediante las instrucciones personalizadas del repositorio y las instrucciones específicas para ciertas rutas.
- generarás la primera parte de la capa de datos para el filtrado (una función auxiliar para consultar editores) con las instrucciones actuales.
- añadirás un estándar para todo el repositorio en `.github/copilot-instructions.md`.
- repetirás el mismo mensaje y observarás cómo el código generado incorpora el nuevo estándar.
- confirmarás los cambios de instrucciones y de la primera parte del filtrado en `main` para que Copilot use las pautas actualizadas en el siguiente ejercicio.

> [!CAUTION]
> El código generado puede no seguir todos los estándares que hayas definido. Copilot no es determinista. El objetivo del ejercicio es observar la tendencia del cambio después de actualizar las instrucciones, no obtener exactamente el mismo código carácter por carácter.

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

- `.github/copilot-instructions.md`: un único archivo que se envía a Copilot en **todas** las solicitudes del repositorio. Debe contener información del proyecto pertinente para la mayoría de las solicitudes, como la pila tecnológica, un resumen de lo que se desarrolla, las prácticas recomendadas y otras pautas generales.
- `.github/instructions/*.instructions.md`: archivos para tareas o tipos de archivos específicos. Puedes proporcionar pautas para determinados lenguajes, como TypeScript o Astro, o para tareas como crear un componente de interfaz o pruebas unitarias.

> [!NOTE]
> En un IDE, los archivos de instrucciones solo se usan para generar código en Copilot Chat, no para completar código ni para las sugerencias de siguiente edición.
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

## Crea una rama para los cambios

Sigamos algunas prácticas recomendadas y creemos una rama para los cambios.

1. Vuelve al Codespace del ejercicio anterior.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama nueva y cámbiate a ella:

   ```bash
   git checkout -b custom-instructions
   ```

## Usa Copilot Chat *antes* de actualizar las instrucciones

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; cada paso indicará cuándo debes seleccionarlos.

Para observar el efecto de las instrucciones personalizadas, primero envía un mensaje con el archivo de instrucciones actual. Más adelante, lo actualizarás y volverás a enviar el mismo mensaje para comparar el resultado.

1. Cierra las pestañas abiertas de ejercicios anteriores para que Copilot solo use el contexto necesario.
2. Abre `src/lib/publishers.ts` para indicar a Copilot dónde debe ubicarse la función auxiliar.
3. En el menú desplegable de agentes de Chat, selecciona **Agent** para que Copilot pueda aplicar cambios a los archivos.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Envía este mensaje:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot explorará el proyecto y aplicará cambios, a menudo tanto al archivo auxiliar como a sus pruebas.
6. Observa que la función auxiliar propuesta tiene tipos, recibe un cliente `db` como primer argumento y devuelve una matriz tipada de editores. Esto se debe a las convenciones de la capa de datos en `.github/instructions/drizzle.instructions.md`, que se aplica a `src/lib/*.ts`.
7. Observa que al código propuesto **le faltan** comentarios TSDoc y un comentario de cabecera del archivo.

> [!CAUTION]
> Como Copilot no es determinista, puede que añada comentarios de documentación aunque no se lo pidas. No pasa nada: el objetivo es observar la mejora de coherencia tras actualizar las instrucciones.

## Añade un estándar para todo el repositorio

Como se ha explicado antes, `.github/copilot-instructions.md` proporciona información del proyecto a Copilot. Añadamos los estándares de programación para mejorar sus sugerencias.

1. Vuelve a abrir `.github/copilot-instructions.md`.
2. Busca la sección **Code formatting requirements**, que debería estar cerca de la línea 27. Describe los estándares del proyecto, pero aún no incluye reglas para documentar el código; por eso la función auxiliar generada no tenía comentarios de documentación.
3. Añade las siguientes líneas de Markdown debajo de los estándares actuales para pedir comentarios de cabecera y comentarios TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. Guarda `copilot-instructions.md`.

> [!TIP]
> Como has visto en la lección anterior, puedes crear archivos de instrucciones en el nivel del repositorio (`.github/copilot-instructions.md`) para pautas globales o como archivos `*.instructions.md` para lenguajes, tipos de archivos o tareas concretos. Las reglas de documentación del código que acabas de añadir son estándares generales del proyecto, por lo que corresponden al archivo del repositorio.

## Repite el mensaje y observa el cambio

1. Vuelve a Copilot Chat y selecciona **New Chat** para vaciar el contexto.
2. Selecciona de nuevo `src/lib/publishers.ts` para que Copilot se centre en el archivo adecuado.
3. Envía **el mismo mensaje** que antes:

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. Observa que ahora el archivo empieza con un bloque de comentarios parecido a este:

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. Observa que la función propuesta ahora incluye un comentario TSDoc parecido a este:

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Acabas de orientar a Copilot para que siga un estándar nuevo y lo aplique al código que servirá de base para el siguiente ejercicio.

## Confirma las instrucciones y publica la rama

Los archivos de instrucciones se gestionan con el control de versiones igual que cualquier otro elemento del repositorio. Confirmemos los cambios y publiquemos la rama.

1. Abre un terminal con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
2. Ejecuta este comando para comprobar que estén los cambios de instrucciones y de la función auxiliar:

   ```bash
   git status
   ```

3. Prepara y confirma la actualización de instrucciones y todos los archivos modificados por Copilot para crear la función auxiliar:

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # Si git status muestra otros cambios relacionados, como pruebas, añádelos también.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. Publica la rama:

   ```bash
   git push -u origin custom-instructions
   ```

## Crea y fusiona una solicitud de incorporación de cambios

Ya has publicado la rama. Ahora abre una solicitud de incorporación de cambios y relaciónala con la incidencia sobre los estándares de documentación de la lista de tareas. Copilot puede buscar la incidencia, crear la PR y fusionarla usando las herramientas de GitHub conectadas al proyecto.

> [!NOTE]
> Este es el primer ejercicio en el que Copilot actúa por ti en GitHub. La plantilla del proyecto ya conecta las herramientas de GitHub al área de trabajo, así que no hay que configurar nada. La primera vez que Copilot use una herramienta, VS Code podría pedirte que inicies sesión en GitHub. Sigue las instrucciones para autorizarlo. En un ejercicio posterior verás cómo funciona esta conexión con Model Context Protocol.

1. Abre Copilot Chat en tu Codespace.
2. Pulsa <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) para abrir la vista Copilot Chat y confirma que está seleccionado **Agent**.
3. Pide a Copilot que encuentre la incidencia sobre la actualización de los archivos de instrucciones y cree una PR desde la rama actual que describa tanto las instrucciones como la función auxiliar de editores:

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot empezará a buscar la incidencia y crear la PR.
5. Si Copilot te pide **Allow** para realizar acciones de GitHub, revisa el comando y autorízalo cuando corresponda.
6. Cuando se cree la PR, pídele que la fusione en `main` y actualice la rama local con este mensaje:

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Si Copilot te pide **Allow** para realizar acciones de GitHub y ejecutar comandos de shell, revisa el comando y autorízalo cuando corresponda.

¡Has creado y fusionado una solicitud de incorporación de cambios con ayuda de GitHub Copilot!

## Resumen y siguientes pasos

Has explorado cómo Copilot obtiene el contexto de los archivos de instrucciones del proyecto y has usado Copilot Chat en VS Code para:

- enviar un mensaje de generación de código y observar el resultado con las instrucciones *existentes*.
- añadir un estándar para todo el repositorio en `.github/copilot-instructions.md`.
- repetir el mismo mensaje y observar cómo el código propuesto incorpora el nuevo estándar.
- confirmar los cambios de instrucciones y la función auxiliar en `main` para que el siguiente ejercicio pueda usarlos.

A continuación, aplicarás esas instrucciones en el [modo de agente][next-lesson] mientras Copilot añade una funcionalidad en varias partes del código.

## Recursos

- [Archivos de instrucciones para personalizar GitHub Copilot][instruction-files]
- [Prácticas recomendadas para crear instrucciones personalizadas][instructions-best-practices]
- [Cinco consejos para escribir mejores instrucciones personalizadas para Copilot][copilot-instructions-five-tips]
- [Instrucciones personalizadas personales para GitHub Copilot][personal-instructions]
- [Awesome Copilot: colección de archivos de instrucciones y otros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
