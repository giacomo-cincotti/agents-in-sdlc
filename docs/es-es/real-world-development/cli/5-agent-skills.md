---
title: "Lección 5 - Personaliza y usa la skill quality-checks"
description: "Explora la skill quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar no consiste únicamente en escribir código. Ya hemos comprobado manualmente que funciona y hemos usado archivos de instrucciones para asegurar que sigue nuestros estándares. Pero ¿qué ocurre con las pruebas, lint y otras partes de la integración continua (CI)?

Para este tipo de tareas, las **skills de agente** son la opción adecuada. Ayudan a Copilot a entender cómo realizar correctamente operaciones como estas.

En esta lección:

- explorarás la skill existente `quality-checks`.
- personalizarás el formato de sus resultados.
- volverás a cargar la skill y la ejecutarás.

## Escenario

Tailspin Toys usa la skill `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las skills de agente agrupan instrucciones de tareas reutilizables, scripts ejecutables y recursos de apoyo que el agente carga cuando los necesita. En esencia, son carpetas con el nombre de la skill y un archivo Markdown llamado `SKILL.md`. El Markdown incluye frontmatter con el nombre y la descripción de la skill, una descripción general de su propósito e indicaciones sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos.

> [!NOTE]
> Las carpetas y los archivos adicionales son opcionales. La skill `quality-checks` de Tailspin Toys solo contiene `SKILL.md`, porque usa los comandos existentes del proyecto.

Las skills pueden residir en la carpeta `.github/skills` del proyecto para que el equipo pueda compartirlas y reutilizarlas, o en la carpeta de skills del usuario `~/.copilot/skills`.

## Explora la skill

Exploremos la skill `quality-checks` que creó el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos.

1. Vuelve a tu Codespace y abre `.github/skills/quality-checks/SKILL.md` en el editor de Codespaces.
2. Lee el campo `name` y la descripción de la parte superior. La descripción ayuda a Copilot a decidir cuándo debe usar la skill.
3. Lee las instrucciones y observa cómo guían a Copilot durante las pruebas y la ejecución de lint.
4. Fíjate en que la skill aún no incluye una sección **Results output formatting**.

## Ejecuta la skill antes de modificarla

Puedes llamar a las skills directamente desde Copilot CLI o pedirlo en lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la skill.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Usa este mensaje:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personaliza el informe

Queremos un informe mejor que indique qué se ha ejecutado, si se ha superado y qué han comunicado realmente las herramientas. Actualicemos la skill para generar ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la skill actualizada

Ya has modificado la skill. Veamos cómo funciona. Copilot CLI puede volver a cargar las skills editadas sin reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo mensaje que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una skill de agente existente. En esta lección:

- has explorado la skill `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos.
- has personalizado el formato de sus resultados.
- has vuelto a cargar y ejecutado la skill.

Ese cambio acompañará al filtrado en la PR de la funcionalidad. A continuación, dejarás que Copilot interactúe directamente con el sitio mediante el [servidor Playwright MCP][next-lesson].

## Más ejemplos de skills

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de contribución: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas y script de exportación incluido: `drawio`][drawio-example]
- [Pruebas del navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
