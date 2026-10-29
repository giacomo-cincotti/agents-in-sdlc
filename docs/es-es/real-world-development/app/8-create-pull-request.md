---
title: "Lección 8 - Crear y combinar la PR de la funcionalidad"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la habilidad, el perfil QA y las pruebas; después, crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones y de la habilidad, el perfil de control de calidad (QA) y las pruebas están guardados en una sola rama. Es hora de revisarlos juntos y abrir una solicitud de incorporación de cambios. Ya has combinado personalmente la solicitud de incorporación de cambios (PR) de valoraciones por estrellas; esta vez permitirás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la funcionalidad, las actualizaciones de instrucciones y de la habilidad y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el flujo de filtrado y calidad en una sesión y una rama, y todo ese trabajo se incluirá en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- examinarás la PR completa de la funcionalidad y las pruebas de verificación.
- autorizarás Agent Merge solo después de la revisión y confirmarás que la PR está combinada.

## Escenario

A lo largo del flujo de filtrado, has utilizado Copilot para planificar, implementar y verificar una funcionalidad. Ahora Tailspin Toys quiere automatizar el trabajo restante de la PR y mantener la autorización para combinar bajo el control del desarrollador.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para integrar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la PR y resuelve los bloqueos: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y vuelve a basar la rama cuando es necesario. La fusiona en cuanto GitHub lo permite. Se ejecuta en segundo plano, sigue funcionando tras reiniciar la aplicación y se desactiva cuando la PR se fusiona.

Hasta ahora has seleccionado **Merge pull request** personalmente. Agent Merge puede asumir esa responsabilidad, pero su capacidad de editar código y combinar sigue necesitando tu autorización explícita. Revisa sus acciones permitidas y el trabajo antes de conceder permiso para combinar.

## Utiliza Agent Merge para gestionar la PR

Ya has creado y revisado todo el código. Dejemos que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para seleccionar **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

El proceso de Agent Merge comienza. Se encargará de lo siguiente:

- Crear la solicitud de incorporación de cambios con un título y una descripción.
- Si has iniciado la sesión desde una incidencia, incluir en el cuerpo de la descripción una referencia a ella.
- Volver a basar la rama o gestionar los posibles conflictos con la rama de destino.
- Supervisar la integración continua para comprobar que se superen todas las verificaciones.
- Supervisar la PR por si hay comentarios de otros desarrolladores o de la revisión de código de Copilot, y aplicar los cambios necesarios para resolverlos.
- Fusionar la PR automáticamente cuando todo se haya completado correctamente, si lo autorizas.

Permitamos que Agent Merge fusione la PR cuando se hayan superado todas las comprobaciones.

5. Revisa las diferencias de la PR y las pruebas de verificación antes de conceder permiso para fusionarla.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Confirma que está marcada la opción **Merge pull request**.
8. Pide explícitamente al agente que fusione la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Sigue el progreso y resuelve los bloqueos indicados. Antes de continuar, confirma que el estado de la PR es **Merged**. Que el permiso de fusión esté habilitado o que el agente esté ejecutándose no significa que la PR se haya fusionado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos ausentes. Resuelve esos bloqueos antes de continuar.

## Resumen y pasos siguientes

Has automatizado varias partes del proceso de desarrollo, como la generación, las pruebas y la validación de código, y ahora también el proceso de solicitud de incorporación de cambios. En concreto:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una combinación.
- has examinado la PR completa de la funcionalidad y las pruebas de verificación.
- has autorizado Agent Merge solo después de la revisión y has confirmado que la PR estaba combinada.

A continuación, [utilizarás un lienzo existente y crearás uno de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app