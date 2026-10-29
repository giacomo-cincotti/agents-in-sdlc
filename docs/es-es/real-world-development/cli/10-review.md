---
title: "Lección 10 - Recapitulación y siguientes pasos"
description: "Recapitula el flujo de trabajo de Copilot CLI, las dos solicitudes de incorporación de cambios, las personalizaciones reutilizables y otros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Has usado GitHub Copilot CLI a lo largo de un flujo de trabajo continuo en Tailspin Toys. Has:

- preparado un Codespace, instalado Copilot CLI, explorado el proyecto y encontrado la incidencia de filtrado incluida en el repositorio.
- añadido valoraciones con estrellas, revisado el resultado en un navegador reenviado y fusionado manualmente tu primera solicitud de incorporación de cambios (PR).
- partido de la incidencia de filtrado, definido el enfoque en el modo Plan, creado la funcionalidad en Autopilot y revisado el resultado en Interactive.
- guiado al agente con instrucciones personalizadas, personalizado la skill existente `quality-checks` y usado esa skill para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- añadido el servidor Model Context Protocol (MCP) de Playwright y usado el navegador para explorar el filtrado.
- creado y seleccionado un agente personalizado de control de calidad (QA) para evaluar los requisitos, la cobertura, los resultados de la skill y las pruebas obtenidas en el navegador.
- revisado todos los cambios del filtrado y autorizado Agent Merge para fusionar su PR.
- explorado comandos de barra para gestionar el contexto y los modelos, compartir sesiones y delegar tareas opcionalmente en la nube.

## Lo que has publicado

El taller incluye dos hitos de PR, cada uno en su propia rama creada a partir de `main` actualizado:

1. **Valoraciones con estrellas:** mostrar el valor existente de `starRating` y un estado explícito para los juegos sin valorar en sus tarjetas.
2. **Filtrado y flujo de calidad:** implementar el filtrado, actualizar las instrucciones y aplicarlas a la funcionalidad, personalizar el informe de `quality-checks`, crear un perfil de QA e incluir las pruebas asociadas.

Desde la planificación del filtrado hasta la apertura de su PR, has usado la misma conversación y rama. Hemos agrupado ese trabajo en una PR para agilizar el taller.

## Distintos tipos de verificación

Has comprobado el código de varias maneras: con pruebas automatizadas, examinando el sitio en el navegador y usando Copilot para explorar el navegador mediante MCP. La skill `quality-checks` ejecutó las pruebas unitarias, lint y comprobaciones de tipos, y generó un informe en el nuevo formato. QA combinó esos resultados con una revisión de los requisitos y la cobertura antes de abrir la PR.

Las pruebas nuevas deben cubrir carencias reales; una ejecución de QA puede concluir correctamente que no hacen falta pruebas adicionales. Revisa el código y las pruebas antes de autorizar la fusión, y actualiza las pruebas afectadas después de hacer cambios.

## Prácticas recomendadas

El contexto y las herramientas que proporcionas influyen en el trabajo de Copilot. En este taller has actualizado las instrucciones, personalizado una skill, creado un perfil de QA y configurado un servidor MCP. Reutiliza estas personalizaciones entre conversaciones y ajústalas a las necesidades del equipo. Las instrucciones establecen estándares, las skills describen tareas repetibles, los agentes personalizados definen roles especializados y los servidores MCP conectan herramientas externas. Revisa los cambios y los resultados de las herramientas, no solo el resumen del agente.

Adapta el **modo y el modelo** a la tarea. Usa **Plan** para definir el enfoque antes de empezar, **Interactive** para seguir de cerca los cambios concretos y **Autopilot** para las tareas bien acotadas. Elige un modelo más rápido para las ediciones habituales y uno más avanzado para el trabajo complejo.

El contexto sigue siendo tan importante como la infraestructura. Describir con claridad *qué* quieres crear, *por qué* y *cómo* influye de forma importante en el resultado.

## Más cosas por explorar

Has completado el flujo de trabajo principal. Estas funciones adicionales de la CLI también pueden interesarte:

- `/review` para pedir al agente de revisión de código que analice los cambios.
- `/rubber-duck` para explicar un problema y obtener otra perspectiva.
- `/fleet` para coordinar tareas independientes en paralelo.
- `/worktree` para aislar otra tarea.
- `/delegate` para enviar una tarea al agente en la nube de Copilot.

## Siguientes pasos

La mejor forma de mejorar con cualquier herramienta es seguir usándola. Úsala con código de producción, en proyectos personales o para crear esa pequeña aplicación que tienes en mente desde hace años. Comparte con el equipo lo que aprendas y aprende de sus experiencias. Y, como siempre, explora la documentación.

Si quieres ampliar Tailspin Toys desde el terminal, continúa con la serie opcional [Foundry Backer Concierge][foundry]. Para comparar otros entornos, explora el [taller de VS Code][vscode], el [taller de la aplicación GitHub Copilot][app] o el [taller del agente en la nube de Copilot][cloud].

## Recursos

- [Acerca de GitHub Copilot CLI][about-cli]
- [Referencia de comandos de Copilot CLI][cli-reference]
- [Personalizar Copilot CLI][customize-cli]
- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
