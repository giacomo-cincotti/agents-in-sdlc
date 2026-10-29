---
title: "Ejercicio 2 - Agente en la nube de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Instrucciones personalizadas][previous-lesson] |
|:--|

Es probable que casi todas las organizaciones tengan deuda técnica: problemas de seguridad sin resolver, código heredado que necesita actualizarse o solicitudes de funcionalidad relegadas en la lista de tareas por falta de tiempo. El agente en la nube de GitHub Copilot puede realizar tareas como actualizar código y añadir funciones de forma autónoma. Cuando termina, crea un borrador de solicitud de incorporación de cambios (PR) para que lo revise una persona. Así puedes delegar tareas tediosas, acelerar el desarrollo y centrarte en cuestiones de mayor alcance.

Con el agente en la nube de Copilot explorarás:

- cómo personalizar el entorno de generación de código.
- cómo asegurarte de que las operaciones se realicen de forma segura.
- por qué es importante delimitar claramente las incidencias.
- cómo asignar incidencias a Copilot.

## Escenarios

Tailspin Toys tiene deuda técnica pendiente. Los contratistas que crearon la primera versión del sitio dejaron la documentación en un estado deficiente: el código carece de ella. Como primer paso, quieren añadir comentarios TSDoc a todas las funciones exportadas de la aplicación.

Además, el equipo de diseño quiere mejorar la exploración de juegos. Quiere mostrar en la página de detalles de cada juego otros títulos de la misma categoría para que los visitantes puedan seguir explorando. No necesita una implementación refinada todavía; quiere algo que sirva para probar la experiencia de usuario. Esta tarea bloquea actualmente el trabajo, aunque ahora mismo hay otras incidencias de mayor prioridad.

Son ejemplos de tareas que pueden quedar relegadas y que conviene asignar al agente en la nube de Copilot. Este puede trabajar en ellas de forma asíncrona, de modo que el desarrollador se ocupe de otras tareas y vuelva más tarde a revisar el resultado.

## Presentación del agente en la nube de GitHub Copilot

El [agente en la nube de GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) puede trabajar en segundo plano de forma parecida a un desarrollador. Puedes, por ejemplo, [asignar una incidencia de GitHub a Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Una vez asignada, Copilot crea un borrador de solicitud de incorporación de cambios para registrar el progreso, configura un entorno y empieza a trabajar. Puedes consultar la sesión mientras está en curso o una vez terminada. Cuando la solución esté lista para revisarse, Copilot te mencionará en la PR.

## La importancia de delimitar bien las instrucciones

GitHub Copilot no hace magia: no te exime de pensar. Incluso las tareas aparentemente sencillas pueden ser complejas al analizarlas en detalle, así que [define cuidadosamente el alcance de las tareas para el agente en la nube][cloud-agent-best-practices]. Trátalo como un compañero de programación con IA: trabaja por etapas, aprende, experimenta y adapta el enfoque. Los fundamentos del desarrollo de software no cambian por incorporar IA generativa.

## Instrucciones personalizadas de este repositorio

En ejercicios anteriores has conocido las instrucciones personalizadas y cómo guían a Copilot. Si ya completaste el ejercicio práctico de instrucciones personalizadas, habrás visto `.github/copilot-instructions.md` en acción; si no, este es un buen momento para echarle un vistazo. Antes de asignar tareas al agente en la nube, revisa los archivos de instrucciones del repositorio para poder reconocer su efecto más adelante.

Abre estos archivos en la interfaz web de GitHub o en un Codespace:

- `.github/copilot-instructions.md`: revisa la sección **Code standards**, especialmente las convenciones de TypeScript y los comentarios TSDoc.
- `.github/instructions/unit-tests.instructions.md`: fíjate en el frontmatter `applyTo`, que aplica estas instrucciones a los archivos `**/*.test.ts`.

Cuando asignes al agente en la nube la incidencia `Code lacks documentation`, observa si la PR incluye comentarios TSDoc, respeta las convenciones de TypeScript y añade comentarios de cabecera. Estas pautas proceden de los archivos de instrucciones; volveremos a examinarlas al revisar las PR.

## Configura el entorno de desarrollo del agente en la nube

La creación de código suele requerir un entorno concreto y la ejecución de ciertos scripts de configuración. Lo mismo ocurre cuando asignas tareas a Copilot, que trabaja de forma parecida a un ingeniero de software.

El agente en la nube usa [GitHub Actions][github-actions] para configurar su entorno. Puedes personalizarlo mediante un [flujo de trabajo de configuración][setup-workflow] especial, definido en `.github/workflows/copilot-setup-steps.yml`, que se ejecuta antes de que el agente empiece. Así puedes proporcionarle las herramientas y dependencias necesarias. Este flujo ya está configurado para el taller e instala Node.js y las dependencias del proyecto, los navegadores de Playwright para las pruebas de extremo a extremo y la base de datos SQLite migrada y cargada para la aplicación Astro:

```yaml
name: "Copilot Setup Steps"

# Permite probar los pasos de configuración desde la pestaña "Actions" del repositorio.
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Establece los permisos mínimos necesarios para *tus pasos*. Copilot recibirá su propio token para realizar sus operaciones.
    permissions:
      # Si quieres clonar el repositorio como parte de los pasos de configuración, por ejemplo, para instalar dependencias, necesitarás el permiso `contents: read`. Si no lo clonas en esos pasos, Copilot lo hará automáticamente cuando terminen.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuración del frontend y la aplicación: Node.js (ahora toda la aplicación usa Astro y Drizzle/libSQL).
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migra y carga datos iniciales en SQLite local para que las compilaciones y pruebas dispongan de datos.
      - name: Set up the database
        run: npm run db:setup
```

Como en cualquier otro flujo de trabajo de GitHub, hay algunos aspectos importantes:

- Incluye un único trabajo llamado `copilot-setup-steps`. GitHub Actions lo ejecuta antes de que Copilot empiece a trabajar en la solicitud de incorporación de cambios.
- El desencadenador `workflow_dispatch` permite ejecutar manualmente el flujo desde la pestaña **Actions** del repositorio. Así puedes comprobar que funciona sin esperar a que Copilot lo ejecute.

## Añade documentación

Todos reconocen la importancia de la documentación, pero muchos proyectos tienen información obsoleta o carecen de ella. Esta deuda técnica puede reducir la productividad y dificultar el mantenimiento del código o la incorporación de nuevos desarrolladores. Copilot destaca en la creación de documentación, por lo que esta es una buena tarea para asignarle. Trabajará en segundo plano; en un ejercicio posterior volverás a revisar el resultado.

1. Abre tu repositorio en github.com en una pestaña nueva del navegador.
2. Selecciona la pestaña **Issues**.
3. Selecciona **New issue** para abrir el cuadro de diálogo de incidencias.
4. Selecciona **Blank issue**.
5. Escribe `Code lacks documentation` en el campo **Title**.
6. Escribe lo siguiente en **Description**:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecciona **Create**.
8. A la derecha, selecciona **Assign to Copilot** para abrir el cuadro de asignación.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

9. Selecciona **Assign**.
10. Selecciona la pestaña **Pull Requests**.
11. Abre la nueva PR, cuyo título será parecido a `[WIP]: Code lacks documentation`. Si aún no aparece, espera un momento y actualiza la página.
12. Al cabo de unos minutos, Copilot debería crear una lista de tareas.

> [!NOTE]
> La lista puede tardar varios minutos en aparecer en la PR. Copilot está preparando su entorno mediante el flujo de trabajo descrito, analizando el proyecto y definiendo cómo resolver la incidencia.

13. Revisa la lista y las tareas previstas.
14. Desplázate por la cronología de la PR hasta ver una actualización que indique que Copilot ha empezado a trabajar en la incidencia.
15. Selecciona **View session**.

  ![Vista de la sesión de Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Puede que tengas que actualizar la página para ver el indicador actualizado.

16. Observa la sesión en curso y cómo Copilot explora el código y comprende su estado, se detiene a planificar el enfoque y crea código.

El proceso puede tardar varios minutos. Uno de los objetivos principales del agente en la nube de Copilot es trabajar de forma asíncrona para que puedas ocuparte de otras tareas. Aprovecharemos esa función asignándole otra tarea y, mientras tanto, añadiremos funcionalidades a la aplicación.

## Añade una sección de juegos relacionados a la página de detalles

Una de las ventajas del agente en la nube de GitHub Copilot es que permite dividir el trabajo: puedes ocuparte de un conjunto de tareas mientras él resuelve otras. Añadir una sección de juegos relacionados para el equipo de diseño quizá no lleve mucho tiempo, pero podrías dedicar ese tiempo a otra tarea. Asignemos esta tarea al agente en la nube.

1. Vuelve al repositorio en github.com.
2. Selecciona **Issues** y, después, **New issue**.
3. Selecciona **Blank issue**.
4. Escribe `Show related games on the game details page` en **Title**.
5. Escribe lo siguiente en **Description**:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

6. Selecciona **Create**.
7. A la derecha, selecciona **Assign to Copilot**.

  ![Asignación de una incidencia a Copilot](../../../_images/shared-assign-copilot.png)

8. Selecciona **Assign**.

Poco después, verás varios 👀 en el primer comentario de la incidencia, lo que indica que Copilot está trabajando en ella.

![Copilot usa el emoji de ojos para indicar que está trabajando en la incidencia](../../../_images/ex4-issue-eyes-emoji.png)

Copilot ya está trabajando en la segunda solicitud. Como el agente en la nube trabaja de manera similar a un ingeniero de software, no hace falta que lo supervises continuamente; puedes revisar el resultado cuando termine. Mientras tanto, vamos a crear y usar agentes personalizados.

## Resumen y siguientes pasos

En esta lección has explorado el [agente en la nube de GitHub Copilot][copilot-agents]. Puedes asignarle incidencias para que trabaje en ellas de forma asíncrona. Copilot puede abordar deuda técnica, crear funcionalidades o ayudar a migrar código entre marcos de trabajo.

Has explorado estos conceptos:

- personalizar el entorno para generar código.
- asegurar que las operaciones se realicen de forma segura.
- delimitar claramente las incidencias.
- asignar incidencias a Copilot.

Ahora que el agente trabaja en segundo plano, vamos a crear y usar agentes personalizados. [El agente en la nube de Copilot también puede usar servidores MCP][cloud-agent-mcp] y puede acceder a las instrucciones personalizadas que has explorado en módulos anteriores.

## Recursos

- [Acerca del agente en la nube de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]

| [Siguiente lección: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
