---
title: "Ejercicio 5 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lección anterior: Agentes personalizados][previous-lesson] |
|:--|

Cuando usas el modo de agente de GitHub Copilot, trabaja de forma autónoma: explora el código, propone cambios, edita archivos y ejecuta comandos. Como el trabajo sucede en tiempo real en tu equipo, Copilot Chat en VS Code muestra cada llamada a una herramienta y cada edición. Puedes revisar las diferencias en línea, aceptar o rechazar cambios concretos y orientar la conversación con nuevos mensajes sin salir del editor.

En este ejercicio:

- supervisarás en tiempo real el trabajo del agente de accesibilidad.
- revisarás y aceptarás los cambios propuestos en los archivos.
- pedirás al agente durante la sesión que añada un control para el modo claro junto al de alto contraste.

## Escenario

El agente personalizado de accesibilidad que has usado en el [Ejercicio 4][previous-lesson] está implementando el modo de alto contraste. Mientras trabaja, el equipo de diseño hace otra solicitud: quiere añadir un control para el modo claro, para las personas a las que el tema predeterminado les resulta demasiado oscuro. En lugar de iniciar otra conversación, dirigirás la sesión actual para ampliar el trabajo.

## Supervisa el progreso de Copilot

La vista Copilot Chat muestra cada paso del agente: los archivos que explora, los comandos que ejecuta y las ediciones que propone. Cada edición aparece en línea junto con las diferencias y los controles **Keep** y **Undo**.

1. Vuelve a la vista Copilot Chat del Ejercicio 4.
2. Mientras trabaja el agente, desplázate por el chat para ver cada llamada a una herramienta, lectura o edición de archivo y comando de terminal que solicita.
3. Cuando proponga una edición, aparecerá junto a ella la diferencia:
    - selecciona **Keep** para aceptar el cambio.
    - selecciona **Undo** para revertirlo y pedir al agente que vuelva a intentarlo.
4. También puedes abrir el indicador **Files changed** de la parte superior del chat para ver todos los archivos modificados durante la sesión.

> [!NOTE]
> Si una llamada a una herramienta pide permiso —por ejemplo, para ejecutar un comando de terminal—, el agente se detendrá y mostrará una solicitud **Allow**. Autoriza solo los comandos que te parezcan adecuados.

5. Espera a que termine de implementar el modo de alto contraste. Al acabar, debería mostrar un mensaje con un resumen de los cambios.

## Pide al agente que añada el modo claro

Ahora que ya está disponible el modo de alto contraste, ampliarás la misma conversación con un control para el modo claro. Como continuarás en la misma sesión, el agente conserva el contexto de su trabajo: los componentes modificados, el enfoque de estilo y el uso del almacenamiento local.

1. Envía este mensaje de seguimiento en Copilot Chat:

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. El agente retomará el trabajo, explorará los cambios y propondrá cómo añadir el modo claro.
3. Revisa cada cambio nuevo cuando aparezca, igual que con el modo de alto contraste. Usa **Keep** o **Undo** para cada diferencia.
4. Si el agente sigue un enfoque que no quieres —por ejemplo, refactoriza el control de una forma que no te convence—, puedes detenerlo con el botón **Stop** junto al campo de chat y enviarle una aclaración.

> [!TIP]
> Dirigir una sesión en curso es más rápido que empezar de nuevo. Usa mensajes de seguimiento para solicitar ajustes («simplifica el CSS y usa variables CSS en lugar de duplicar selectores»), corregir el enfoque («no añadas un tercer control; integra el modo claro en el panel de configuración actual») o ampliar el alcance («añade también métodos abreviados de teclado para ambos controles»).

## Revisa los cambios

Antes de confirmar el trabajo, revisa rápidamente todos los archivos modificados por el agente.

1. Abre la vista **Source Control** de VS Code.
2. Revisa la lista completa de archivos modificados. Deberías ver cambios en componentes Astro, estilos y pruebas relacionadas.
3. Abre algunos archivos y revisa las diferencias. Confirma que se reflejan las pautas de accesibilidad del agente personalizado: atributos ARIA, navegación con teclado, HTML semántico y persistencia en el almacenamiento local.
4. Cuando estés conforme, prepara y confirma los cambios desde el panel Source Control. Los publicarás en [la siguiente lección][next-lesson].

## Resumen y siguientes pasos

Has usado el modo de agente de Copilot Chat para supervisar y dirigir una sesión en curso: has revisado las ediciones mientras aparecían, las has aceptado o rechazado y has enviado mensajes de seguimiento para ampliar el trabajo sin perder el contexto.

En esta lección has explorado:

- cómo supervisar en tiempo real el trabajo de un agente.
- cómo aceptar y rechazar ediciones concretas.
- cómo dirigir una sesión con mensajes de seguimiento.

En la [siguiente lección][next-lesson] publicarás tu trabajo de accesibilidad en una solicitud de incorporación de cambios y revisarás los cambios generados por el agente personalizado local durante el taller.

## Recursos

- [Copilot Chat en VS Code][copilot-chat-vscode]
- [Uso del modo de agente][agent-mode]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
