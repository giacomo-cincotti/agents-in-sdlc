---
slug: es-es/real-world-development/cli
title: "GitHub Copilot CLI"
description: "Crea, verifica y entrega dos cambios de Tailspin Toys mientras aprendes los modos, las personalizaciones, las herramientas MCP y la automatización de solicitudes de incorporación de cambios de Copilot CLI."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

**[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** incorpora GitHub Copilot a tu terminal como asistente de programación con agentes. Explora bases de código, genera código, ejecuta comandos y se conecta a herramientas externas, todo desde la línea de comandos, para que puedas mantener el flujo sin cambiar a un editor gráfico.

El taller sigue un flujo continuo de Tailspin Toys:

1. Prepara el proyecto en GitHub Codespaces, instala Copilot CLI y familiarízate con él.
2. Haz un cambio pequeño en las valoraciones con estrellas, revísalo en el navegador y fusiona manualmente tu primera solicitud de incorporación de cambios (PR).
3. Parte de la incidencia de filtrado, define el enfoque en el modo Plan, impleméntalo con Autopilot y revísalo en Interactive.
4. Actualiza las instrucciones del repositorio y aplícalas al trabajo de filtrado.
5. Personaliza la skill existente `quality-checks` y úsala para ejecutar las comprobaciones del proyecto.
6. Añade el servidor Model Context Protocol (MCP) de Playwright y úsalo para explorar el filtrado en un navegador.
7. Crea un agente personalizado de control de calidad (QA) y úsalo para revisar requisitos, cobertura y pruebas de verificación.
8. Revisa todos los cambios del filtrado y usa Agent Merge para su PR.
9. Explora comandos de barra útiles para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente a la nube.

Para que el taller sea manejable, crearás dos PR: una para las valoraciones con estrellas y otra para el filtrado, con las actualizaciones de instrucciones, la skill, el perfil de QA y las pruebas. El filtrado y el flujo de calidad comparten una conversación y una rama para que puedas continuar el trabajo mientras exploras cada herramienta.

## Ejercicios

| Ejercicio | Tema | Descripción |
|----------|-------|-------------|
| [0. Requisitos previos][ex0] | Configuración | Crea tu repositorio y tu codespace |
| [1. Instalación de Copilot CLI][ex1] | Instalación | Instala y autentica Copilot CLI |
| [2. Añadir valoraciones con estrellas: una mejora rápida][ex2] | Primer cambio | Muestra las valoraciones existentes y el estado sin valoración; después, fusiona tu primera PR |
| [3. Modos de agente: Plan y Autopilot][ex3] | Modos de agente | Planifica la función desde su incidencia, créala con Autopilot y revísala con Interactive |
| [4. Guía a Copilot con instrucciones personalizadas][ex4] | Contexto | Explora y actualiza las instrucciones y aplícalas al filtrado |
| [5. Personaliza y usa la skill quality-checks][ex5] | Comprobaciones repetibles | Explora la skill existente, cambia el formato del informe y ejecútala |
| [6. Valida la funcionalidad con Playwright MCP][ex6] | Observación en navegador | Configura MCP en la CLI e inspecciona el filtrado |
| [7. Crea y usa un agente de QA][ex7] | Requisitos y cobertura | Crea y selecciona un perfil especializado y reúne las pruebas finales |
| [8. Crea y fusiona la PR de la funcionalidad][ex8] | Revisión y fusión | Revisa todos los cambios, crea la PR y usa Agent Merge |
| [9. Comandos de barra de GitHub Copilot CLI][ex9] | Funciones de la CLI | Explora el contexto, los modelos, el uso compartido y la delegación opcional |
| [10. Recapitulación y siguientes pasos][ex10] | Resumen | Repasa el flujo, las personalizaciones reutilizables y otros recursos |
| [Opcional: incorpora Foundry][foundry] | Agentes hospedados | Prepara un modelo, crea y despliega el concierge y conéctalo al sitio web en tres módulos |

## Requisitos previos

Antes de asistir a este taller, asegúrate de tener:

- [ ] Una cuenta de GitHub con un plan activo de **Copilot Student, Pro, Pro+, Business o Enterprise**
- [ ] Conocimientos básicos de operaciones de terminal/línea de comandos
- [ ] Git instalado y configurado

> [!TIP]
> ¿No tienes un plan de pago? Los estudiantes verificados pueden obtener GitHub Copilot gratis a través de [GitHub Education][callout-student-plan-education]. El plan **Copilot Student** incluye las funciones de agente, MCP, revisión de código y Copilot CLI que utiliza este taller, así que puedes completar con él todos los recorridos.

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Si usas Copilot Business o Copilot Enterprise, asegúrate de que tu administrador haya habilitado Copilot CLI.

## Primeros pasos

**[Empieza con el ejercicio 0: Requisitos previos →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[ex10]: 10-review/
[foundry]: 8-foundry-agent/
