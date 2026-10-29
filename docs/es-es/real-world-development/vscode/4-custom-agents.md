---
title: "Ejercicio 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## ¿Qué son los agentes personalizados?

Los [agentes personalizados][custom-agents-concept] de GitHub Copilot permiten crear asistentes de IA especializados en tareas o ámbitos concretos de tu flujo de desarrollo. Puedes definirlos mediante archivos Markdown en la carpeta `.github/agents` del repositorio e incluir instrucciones, prácticas recomendadas, patrones de programación y conocimientos específicos para que Copilot realice determinados tipos de trabajo de manera más eficaz. Los equipos pueden convertir sus conocimientos en agentes reutilizables: uno de accesibilidad que aplique las pautas [WCAG][wcag], uno de seguridad que siga prácticas de programación segura o uno de pruebas que mantenga patrones coherentes.

Los agentes personalizados se definen en archivos Markdown dentro de `.github/agents` del proyecto o, globalmente, en `~/.copilot/agents`. Cada archivo tiene un frontmatter YAML que incluye al menos `name` y `description`, seguido de un mensaje Markdown que define el comportamiento, la experiencia y las instrucciones del agente.

### Diferencias entre los agentes personalizados y las skills de agente

Los agentes personalizados y las [skills de agente][agent-skills-concept] se solapan en algunos aspectos. Ambos se definen principalmente mediante archivos Markdown e indican a una IA cómo realizar ciertas operaciones. La forma más sencilla de distinguirlos es que el **agente personalizado** es quien realiza el trabajo y las **skills** son las herramientas.

Los agentes personalizados tienen su propia ventana de contexto y coordinan skills e incluso otros agentes. En este taller, el agente personalizado de accesibilidad revisará y actualizará el sitio según las pautas de accesibilidad; para ello, podría recurrir a skills como una que gestione el flujo de solicitudes de incorporación de cambios o una que ejecute y administre pruebas.

> [!NOTE]
> No hay una única manera correcta de crear un agente personalizado. Como ocurre con cualquier herramienta de IA, prueba distintas opciones e itera para encontrar las que funcionan en tus entornos y escenarios.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Escenario

Tailspin Toys se compromete a que todas las personas puedan usar su plataforma de financiación colectiva, con independencia de sus capacidades o preferencias visuales. Los comentarios recientes indican que algunas personas tienen dificultades para leer el tema oscuro actual debido al bajo contraste entre el texto y el fondo. Para responder a esta necesidad de accesibilidad, el equipo de diseño ha solicitado un modo de alto contraste que se pueda activar y desactivar.

Como la accesibilidad es importante, quieres implementar esta funcionalidad cuanto antes. Para generarla, usarás un agente personalizado.

En este ejercicio:

- revisarás un agente personalizado de accesibilidad ya creado.
- usarás el agente de accesibilidad de Copilot Chat para implementar un modo de alto contraste.

## Revisa el agente personalizado de accesibilidad

Ya se ha creado un agente personalizado de accesibilidad. Revisemos su contenido para entender cómo guiará a Copilot.

Vuelve a tu Codespace y abre un terminal. Crea una rama nueva a partir de `main` para el trabajo de accesibilidad; así mantendrás separada la PR del ejercicio anterior:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abre `.github/agents/accessibility.md`.
2. Observa los campos `name` y `description` del frontmatter YAML.

> [!CAUTION]
> Los campos `name` y `description` del frontmatter son obligatorios para los agentes personalizados.

3. Revisa las secciones siguientes, que describen:
   - las responsabilidades principales al generar código para un sitio web accesible.
   - las prácticas recomendadas de accesibilidad.
   - ejemplos de código HTML, CSS y JavaScript.
   - errores y problemas habituales.

## Usa el agente personalizado en Copilot Chat

VS Code muestra todos los agentes personalizados definidos en `.github/agents` en el menú desplegable de agentes, en la parte inferior de Copilot Chat. Puedes seleccionar un agente personalizado para limitar la sesión de chat a sus instrucciones y herramientas.

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu Codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo dependen del ejercicio; cada paso indicará cuándo debes seleccionarlos.

1. Selecciona **Agent** en el menú desplegable de agentes de Chat, si aún no está seleccionado.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona el menú desplegable de agentes en la parte inferior de Chat. Muestra el agente activo, que de forma predeterminada es **default**.
3. Selecciona **Accessibility agent** en la lista de agentes.
4. Envía al agente de accesibilidad este mensaje:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat explorará el código, propondrá cambios y los aplicará a los archivos del proyecto. Cada edición aparecerá en el chat, junto con la ruta del archivo y las diferencias para que puedas revisarlas.
6. El indicador **Files changed** se actualizará a medida que el agente modifique archivos.

> [!NOTE]
> Este proceso puede tardar varios minutos. Copilot realizará cambios reales en el repositorio: editará componentes Astro, CSS y pruebas relacionadas cuando sea necesario.

En el siguiente ejercicio revisarás y dirigirás este trabajo en curso.

## Resumen y siguientes pasos

En esta lección has explorado los [agentes personalizados][custom-agents] de GitHub Copilot: asistentes especializados en tareas y ámbitos concretos. Permiten convertir los conocimientos y estándares del equipo en agentes reutilizables que orientan el trabajo de Copilot.

Has explorado estos conceptos:

- cómo se definen los agentes personalizados.
- cómo usar un agente personalizado en el modo de agente de Copilot Chat.

A continuación, [supervisarás y dirigirás el trabajo del agente][next-lesson], revisarás los cambios mientras se realizan y añadirás un control para el modo claro en la misma sesión.

## Recursos

- [Acerca de los agentes personalizados][custom-agents]
- [Crear agentes personalizados en el IDE][creating-custom-agents-ide]
- [Agentes personalizados en VS Code][custom-agents-vscode]
- [Configuración de agentes personalizados][custom-agents-config]
- [Agentes personalizados de awesome-copilot][awesome-copilot-agents]

---

| [← Lección anterior: Prueba tu función con el servidor Playwright MCP][previous-lesson] | [Siguiente lección: Supervisar y gestionar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
