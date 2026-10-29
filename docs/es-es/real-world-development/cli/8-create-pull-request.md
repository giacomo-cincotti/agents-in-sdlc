---
title: "Lección 8 - Crea y fusiona la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la skill, el perfil de QA y las pruebas; después, crea una PR y usa Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

La implementación del filtrado, las actualizaciones de las instrucciones y de la skill, el perfil de control de calidad (QA) y las pruebas están guardados en una rama. Es el momento de revisarlos conjuntamente y abrir una solicitud de incorporación de cambios. Tú mismo fusionaste la PR de la valoración con estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Lo habitual sería separar la funcionalidad, las instrucciones, la actualización de la skill y el agente de QA en varias PR. Para agilizar el taller, has mantenido todo el trabajo de filtrado y calidad en una conversación y una rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de fusión.
- inspeccionarás todos los cambios de la funcionalidad y las pruebas de verificación.
- crearás la PR del filtrado.
- activarás Agent Merge solo después de la revisión y confirmarás que la PR se ha fusionado.

## Escenario

Durante el flujo de trabajo del filtrado, has usado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR, pero mantener bajo control del desarrollador la autorización para fusionarla.

## Presentación de Agent Merge

**Agent Merge** automatiza el trabajo restante necesario para integrar una solicitud de incorporación de cambios. Cuando lo activas, Copilot resuelve los bloqueos de la PR: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a basar la rama cuando sea necesario. Después, activa la fusión automática de GitHub si el repositorio lo permite.

Hasta ahora has seleccionado tú **Merge pull request**. Agent Merge puede encargarse de esa tarea. Revisa el trabajo y confirma que está listo antes de activarlo.

## Usa Agent Merge para gestionar la PR

Ya has creado todo el código. Revisémoslo conjuntamente, creemos la PR y dejemos que Agent Merge se ocupe del resto.

1. Vuelve a tu Codespace.
2. Abre el cuadro de diálogo de agente escribiendo `/agent`.
3. Selecciona **Default** en la lista de opciones y pulsa <kbd>Enter</kbd>.
4. Crea una PR nueva con el comando `/pr create`.
5. Activa Agent Merge con `/pr agentmerge`.
6. Copilot observará el proceso de integración continua de la PR. Cuando todo se complete correctamente, realizará la fusión.
7. Confirma que Copilot muestra un mensaje parecido a «PR #14 was squash-merged successfully».

> [!IMPORTANT]
> Agent Merge no omite las aprobaciones obligatorias, la protección de ramas, las colas de fusión, la configuración del repositorio ni los permisos que falten. Si se bloquea, lee el motivo y realiza manualmente la fusión revisada cuando el repositorio lo permita.

## Resumen y siguientes pasos

Has automatizado varios pasos del proceso de desarrollo: generar código, probarlo y validarlo, y gestionar la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de fusión.
- has inspeccionado todos los cambios de la funcionalidad y las pruebas de verificación.
- has creado la PR de filtrado.
- has activado Agent Merge después de la revisión y confirmado que la PR se ha fusionado.

A continuación, [explorarás otros comandos de barra útiles de Copilot CLI][next-lesson] para gestionar el contexto, seleccionar modelos, compartir sesiones y delegar tareas opcionalmente a un agente en la nube.

## Recursos

- [Gestionar solicitudes de incorporación de cambios con Copilot CLI][manage-prs]
- [Referencia de comandos de Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
