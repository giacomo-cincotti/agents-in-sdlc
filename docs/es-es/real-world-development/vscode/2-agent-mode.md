---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y tareas como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos pertinentes, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a modificarlo. Y la lista sigue.

Para eso sirve el modo de agente de Copilot.

El modo de agente de Copilot está diseñado para trabajar con mayor autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza los cambios necesarios, ejecuta tareas como las pruebas y corrige automáticamente los errores que encuentra. Veamos cómo usarlo para añadir una funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en el IDE y permite recibir comentarios rápidamente e interactuar con él. El agente en la nube funciona como un compañero de programación asíncrono que se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás cómo:

- el modo de agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- el modo de agente de GitHub Copilot puede implementar funciones que abarcan la interfaz y la capa de datos.
- revisar los cambios y las pruebas generadas por el modo de agente antes de fusionarlos en el repositorio.

## Escenario

A medida que crece la lista de juegos, quieres permitir que los usuarios filtren por categoría y editor. En el ejercicio anterior ya añadiste una función auxiliar para consultar editores. Ahora completarás el trabajo restante en la capa de datos, la interfaz y las pruebas con el modo de agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de hacer cambios, exploremos el sitio web de Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con un tema de desarrollo. Permite mostrar juegos y consultar sus detalles. El sitio es una única aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un marco de interfaz del lado del cliente. Las funciones auxiliares reutilizables de acceso a datos están en `src/lib/` y la interactividad se implementa con pequeños elementos `<script>` de Astro que usan las API DOM estándar.

### Inicia el sitio web

Para simplificar el inicio del sitio, se ha preparado la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Puedes ejecutarla en tu Codespace de GitHub:

1. Vuelve a tu Codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal dentro de tu Codespace con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crea una rama para el trabajo de filtrado y cámbiate a ella:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir el navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio manteniendo pulsada <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando en el terminal la dirección del servidor Astro `http://localhost:4321`.

> [!NOTE]
> En un Codespace, seleccionar un enlace localhost desde el terminal redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Es un túnel privado a tu Codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté funcionando, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre una página con más información, como el título, la descripción, el editor y la categoría.

## Explora la lista de tareas con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; cada paso indicará cuándo debes seleccionarlos.

La implementación inicial del sitio funciona, pero queremos añadir funciones. Empecemos por revisar la lista de tareas. Al crear el proyecto a partir de la plantilla, se generó una lista de incidencias automáticamente. Pide a GitHub Copilot que te muestre esos elementos.

1. Selecciona **Agent** en el menú desplegable de agentes de la vista Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos del terminal e invoca herramientas de forma autónoma.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores del taller no expresan una preferencia por un modelo concreto. Durante su desarrollo usamos Claude Sonnet 4.5 y lo mencionamos aquí para obtener sugerencias relativamente coherentes y ofrecer una buena experiencia. Sin embargo, como los modelos de lenguaje grandes son probabilísticos, las sugerencias pueden diferir de las descritas en el taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje grandes, Copilot puede usar un comando MCP diferente, pero debería completar la tarea igualmente.

3. Pide a Copilot que muestre las incidencias pendientes:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según cuáles podrían resultar más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere el código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu Codespace, abre `.github/instructions/ui.instructions.md`.
2. Observa las pautas generales para añadir funciones. Incluyen:
   - un resumen de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones para distintos tipos de archivos, como Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Así se pueden reutilizar pautas, dividir instrucciones complejas en archivos más manejables e incluir ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, habrá que realizar al menos tres cambios independientes en la aplicación:

- añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento del filtrado.
- actualizar la página de listado de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de fusionar los cambios. El modo de agente de Copilot puede encargarse de estas tareas.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de interfaz](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado en el menú de agentes de Chat.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la función según la incidencia relacionada de la lista:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos relacionados con la función: las funciones auxiliares de la capa de datos, la interfaz y las pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar unos minutos según la complejidad de la tarea y del código. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. Al terminar, habrá un botón **Keep** o **Undo** para cada cambio; no hace falta seleccionar nada mientras el agente esté trabajando.

7. Cuando Copilot te lo pida, selecciona **Continue** para ejecutar las pruebas.

   ![Captura del panel Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que Copilot se detenga varias veces e incluso que algunas pruebas fallen durante el proceso. No pasa nada: alterna entre generar código y ejecutar pruebas hasta completar la tarea y no detectar errores.

   ![Captura de una sesión de Copilot Agent Mode completada](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado y busca posibles problemas.

> [!CAUTION]
> Revisa siempre el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio y explora la nueva función.
11. Cuando hayas confirmado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y siguientes pasos

¡Enhorabuena! En este ejercicio has aprendido a usar el modo de agente de GitHub Copilot para añadir funciones al sitio web Tailspin Toys. Has aprendido que:

- el modo de agente de GitHub Copilot puede implementar funciones en la interfaz y la capa de datos.
- el modo de agente puede explorar el proyecto, identificar los archivos pertinentes y coordinar cambios.
- debes revisar los cambios y las pruebas generadas antes de fusionarlos.

Ahora [probarás la función con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar paginación

A medida que crece la lista de juegos, será necesario añadir paginación. Usa lo aprendido en este ejercicio y pide a Copilot que actualice el sitio. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas, incluidos los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá seleccionar el tamaño de página o si estará fijado.
- al redactar el mensaje, proporciona a Copilot las indicaciones necesarias para crear la implementación deseada.
- puede que tengas que iterar con GitHub Copilot, pedir cambios y proporcionar contexto. Es lo habitual al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: para qué sirven y cuándo usarlos][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
