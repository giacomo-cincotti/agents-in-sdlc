---
title: "Ejercicio 5: Iterar sobre el trabajo de GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Gestión de agentes][previous-lesson] |
|:--|

## Revisa el trabajo

Durante este taller has colaborado con GitHub Copilot en varias tareas para mejorar la experiencia de usuario y añadir funcionalidades. Le has pedido que documente el código, cree una función de juegos relacionados para que el equipo de diseño pueda probarla e implemente opciones de accesibilidad, incluidos los modos de alto contraste y claro. Revisemos los cambios y, si hace falta, demos indicaciones a Copilot para mejorarlos.

### Escenario

Como hemos destacado varias veces, los fundamentos del diseño de software y DevOps no cambian al añadir IA generativa. Siempre hay que revisar el código generado y seguir el proceso habitual de DevOps. Con esto en mente, revisemos las sugerencias de GitHub Copilot para la documentación, la función de juegos relacionados y las funciones de accesibilidad antes de activar la revisión para el resto del equipo.

## Seguridad y el agente en la nube de GitHub Copilot

Como el agente en la nube de Copilot realiza tareas de forma asíncrona y sin supervisión, se han establecido restricciones de seguridad para proteger el entorno:

- Copilot solo tiene acceso de lectura al repositorio y acceso de escritura **únicamente** a la rama que usa para su código.
- El agente en la nube se ejecuta en GitHub Actions, donde crea un entorno independiente y efímero.
- Una persona debe aprobar todos los flujos de trabajo de GitHub Actions antes de que puedan ejecutarse.
- [El acceso a recursos externos está limitado de forma predeterminada](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluidos los servidores MCP.

## Revisa la documentación generada

Empecemos por la primera solicitud de incorporación de cambios (PR) creada por el agente en la nube de GitHub Copilot: la que añade documentación al código. Revisarás los cambios mediante la interfaz habitual de PR de GitHub.com.

> [!NOTE]
> Es posible que al abrir la PR veas un aviso de que el cortafuegos ha bloqueado GitHub Copilot. Es **normal**: de forma predeterminada, Copilot tiene acceso limitado a recursos externos, incluidas las llamadas a servidores MCP externos. Si quieres, puedes [personalizar o deshabilitar el cortafuegos del agente en la nube][agent-firewall].

1. Vuelve a tu repositorio en github.com.
2. Selecciona **Pull Requests** para abrir la lista de PR.
3. Abre la solicitud titulada, por ejemplo, **Add missing documentation**.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la solicitud incluirá la etiqueta **[WIP]**. En ese caso, espera a que termine. Puede tardar unos minutos; puedes descansar o repasar lo que has aprendido.

4. Cuando la PR esté lista, selecciona **Files changed** y revisa los cambios.

    ![Pestaña Files changed](../../../_images/shared-pr-files-changed.png)

5. Explora el código actualizado, que ahora incluye comentarios TSDoc y otra documentación. Los cambios concretos pueden variar.

    Al revisar los cambios, busca comentarios TSDoc, convenciones de TypeScript y comentarios de cabecera. Proceden de los archivos de instrucciones que revisaste al principio del Ejercicio 2.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si hay flujos pendientes, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.

## Pide cambios a GitHub Copilot

La colaboración con Copilot en una PR no tiene por qué ser unidireccional. Puedes mencionarlo en comentarios de la PR o en comentarios insertados en el código, como harías con cualquier otra persona del equipo. Copilot verá esos comentarios e iniciará otra sesión para resolverlos. Como los resultados no son deterministas, no podemos darte un texto exacto. Puedes pedirle, por ejemplo:

- que añada en la cabecera de cada archivo de código un comentario breve sobre su propósito.
- que añada comentarios TSDoc a los archivos TypeScript y Astro.
- que cree un README con una descripción de la estructura de la aplicación Astro.

1. Añade un comentario solicitando un cambio en la documentación generada y menciona a **@copilot**, como harías con cualquier otra persona. Usa una de las ideas anteriores u otra sugerencia de documentación.
2. Selecciona **View Session** para observar cómo trabaja Copilot. Verás que inicia una sesión nueva para realizar los cambios.
3. Puedes seleccionar **Back to pull request** para volver a la solicitud.

    ![Volver a la solicitud de incorporación de cambios](../../../_images/ex7-back-to-pr.png)

4. Cuando Copilot termine, debería aparecer una confirmación nueva en la PR.
5. Selecciona **Files changed** para revisar los cambios.

Puedes seguir iterando hasta que estés conforme. Después, puedes marcar la PR como lista para revisión y fusionarla en la rama principal.

![Marcar la PR como lista para revisión](../../../_images/ex7-ready-for-review.png)

## Revisa la función de juegos relacionados

Volvamos a la PR que Copilot creó para resolver la incidencia sobre los juegos relacionados en la página de detalles.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Show related games on the game details page**.
4. Selecciona **Files changed** para revisar el código generado.
5. Cuando hayas terminado y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
6. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
7. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

8. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
9. **Opcional:** puedes cambiar a esta rama en tu Codespace para probar manualmente la función de juegos relacionados. Abre el terminal y ejecuta los siguientes comandos, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot, por ejemplo, **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

¡Copilot ha creado la función de juegos relacionados! Como antes, puedes iterar y pedir al agente en la nube que realice cambios. Por ejemplo, puedes modificar la cantidad de juegos relacionados o pedir que añada comentarios de cabecera y comentarios TSDoc. Recuerda que esta tarea se asignó **antes** de actualizar las instrucciones personalizadas. Para solicitar cambios, añade un comentario en la pestaña **Conversation**; Copilot lo verá e iniciará otra sesión.

## Revisa las funciones de accesibilidad

Por último, revisemos las funciones de accesibilidad implementadas con el agente personalizado. Esta PR debería incluir el modo de alto contraste que pediste en el Ejercicio 3 y el modo claro solicitado desde el centro de control en el Ejercicio 4.

1. Vuelve a tu repositorio en GitHub.com.
2. Selecciona la pestaña **Pull Requests**.
3. Selecciona la PR titulada, por ejemplo, **Add high contrast mode to website**.

> [!NOTE]
> Si Copilot sigue trabajando, el título de la PR incluirá la etiqueta **[WIP]**. En ese caso, espera unos minutos a que termine.

4. Selecciona **Files changed** para revisar el código generado.
5. Revisa la implementación y presta especial atención a:
   - los componentes de alternancia para cambiar de modo.
   - el uso del almacenamiento local para conservar las preferencias.
   - los cambios de CSS o estilo para los modos de alto contraste y claro.
   - los atributos de accesibilidad, como las etiquetas ARIA y la navegación con teclado.
   - el código JavaScript/TypeScript que gestiona los cambios de modo.

6. Cuando hayas revisado las actualizaciones y todo parezca correcto, vuelve a la pestaña **Conversation** y desplázate hacia abajo.
7. Deberías ver un indicador de que hay flujos de trabajo pendientes de aprobación.
8. Si los hay, selecciona **Approve and run workflows**.

   ![Aprobar y ejecutar flujos de trabajo](../../../_images/shared-approve-workflows.png)

9. Los flujos deberían aparecer en la sección de comprobaciones de la PR. Si todo va bien, las comprobaciones del proyecto para la aplicación Astro se superarán. El proceso puede tardar unos minutos.
10. **Opcional:** cambia a esta rama en tu Codespace para probar manualmente las funciones de accesibilidad. Abre el terminal y ejecuta los siguientes comandos, sustituyendo `<branch-name>` por el nombre de la rama creada por Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Después, inicia la aplicación y prueba en el navegador los controles de los modos de alto contraste y claro. Confirma que funcionan y que la preferencia se mantiene al recargar la página.

Observa cómo el agente personalizado de accesibilidad orientó a Copilot para aplicar prácticas recomendadas. Si detectas algún problema o mejora de accesibilidad, puedes mencionar a **@copilot** en un comentario y pedirle que lo corrija, igual que con las demás PR.

## Ejercicio opcional: sigue delegando

El agente en la nube resulta útil para delegarle tareas reales de la lista de trabajo mientras te ocupas de otras cosas. Para practicar, registra varias incidencias en tu repositorio y asígnalas a Copilot. Algunas ideas:

- crear un formulario para indicar interés en una campaña en la página de detalles del juego.
- añadir paginación a la página de la lista de juegos.
- validar las entradas y gestionar los errores en las funciones auxiliares de acceso a datos.

## Resumen

Has completado el itinerario sobre agentes en la nube. En estas lecciones:

- **Has inspeccionado los archivos de instrucciones personalizadas del repositorio** para reconocer su influencia en los resultados del agente.
- **Has asignado incidencias al agente en la nube de Copilot** y has observado cómo prepara el entorno, planifica y trabaja de forma asíncrona.
- **Has creado y usado un agente personalizado** para implementar modos de alto contraste y claro.
- **Has usado la página de agentes de Copilot como centro de control** para supervisar y dirigir la sesión de accesibilidad.
- **Has revisado las PR del agente en la nube e iterado sobre ellas**, mencionando a `@copilot` para solicitar cambios y aprobando los flujos de trabajo.

## Revisión y siguientes pasos

Has completado el itinerario del agente en la nube. Los otros itinerarios complementan lo que has aprendido:

- 🖥️ **[Itinerario de VS Code](../../vscode/)**: explora el modo de agente de Copilot Chat y la integración con MCP directamente desde el IDE.
- 💻 **[Itinerario de CLI](../../cli/)**: publica dos cambios revisados desde el terminal con los modos Plan y Autopilot, skills de agente, un agente de QA personalizado, Playwright MCP, Agent Merge y un puente opcional `/delegate` al agente en la nube.

En tu propio repositorio, prueba estas ideas:

- asigna al agente en la nube una incidencia de refactorización o cobertura de pruebas.
- crea un agente personalizado para otro ámbito.
- configura `copilot-setup-steps.yml` para otra pila tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Acerca de los agentes de Copilot][copilot-agents]
- [Asignar incidencias de GitHub a Copilot][assign-issue]
- [Prácticas recomendadas para el flujo de configuración del agente en la nube][cloud-agent-best-practices]
- [Configurar el cortafuegos del agente en la nube de Copilot][agent-firewall]

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
