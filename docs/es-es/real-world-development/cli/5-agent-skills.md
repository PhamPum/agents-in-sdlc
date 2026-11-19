---
title: "Lección 5 - Personalizar y utilizar una aptitud quality-checks"
description: "Explora la aptitud quality-checks existente, personaliza el formato de su informe y úsala para validar el filtrado."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Escribir código implica mucho más que programar. Hemos comprobado manualmente que el código funciona y utilizado archivos de instrucciones para asegurarnos de que sigue nuestros estándares. Pero ¿qué ocurre con las pruebas, el lint y el resto de las tareas de integración continua (CI)?

Las **aptitudes de agente** son la mejor opción para este tipo de tareas. Ayudan a Copilot a entender cómo ejecutar operaciones como estas.

En esta lección:

- explorarás la aptitud `quality-checks` existente;
- personalizarás el formato de sus resultados;
- volverás a cargar la aptitud y la ejecutarás.

## Escenario

Tailspin Toys utiliza la aptitud `quality-checks` para ejecutar pruebas unitarias, lint y comprobaciones de tipos. El equipo quiere mejorar el informe para que los resultados sean más fáciles de leer.

## Instrucciones, scripts y recursos

Las aptitudes de agente agrupan instrucciones reutilizables para tareas, scripts ejecutables y recursos de apoyo que un agente carga cuando los necesita. En esencia, son carpetas con el nombre de la aptitud y un archivo Markdown llamado `SKILL.md`. El archivo Markdown incluye un frontmatter con el nombre y la descripción de la aptitud, una explicación de su función y directrices sobre cuándo debe utilizarse. La carpeta también puede incluir subcarpetas con scripts y otros recursos que la aptitud utiliza cuando se invoca.

> [!NOTE]
> No es obligatorio incluir carpetas y archivos adicionales. La aptitud `quality-checks` de Tailspin Toys solo contiene `SKILL.md` porque utiliza los comandos existentes del proyecto.

Las aptitudes pueden estar en la carpeta `.github/skills` del proyecto para que se compartan y reutilicen como recursos del repositorio, o en la carpeta de aptitudes del usuario `~/.copilot/skills`.

## Explora la aptitud

Exploremos la aptitud que ha creado el equipo de Tailspin Toys para ejecutar pruebas unitarias, lint y comprobaciones de tipos: `quality-checks`.

1. Vuelve a tu Codespace. En el editor de Codespaces, abre `.github/skills/quality-checks/SKILL.md`.
2. Lee el `name` y la `description` de la parte superior. La descripción ayuda a Copilot a decidir cuándo utilizar la aptitud.
3. Lee las instrucciones y observa cómo guían a Copilot por el proceso de pruebas y lint.
4. Fíjate en que la aptitud todavía no incluye una sección **Results output formatting**.

## Ejecuta la aptitud antes de hacer cambios

Puedes invocar aptitudes directamente desde Copilot CLI o utilizar lenguaje natural. Pidamos a Copilot que ejecute las tres comprobaciones de la aptitud.

1. Vuelve a la conversación de filtrado en modo Interactive.
2. Envía este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final.

## Personaliza el informe

Queremos un informe mejor que indique qué se ha ejecutado, si ha terminado correctamente y qué resultados han devuelto las herramientas. Actualizaremos la aptitud para que genere ese informe.

1. Vuelve a `.github/skills/quality-checks/SKILL.md`.
2. Añade al final del archivo esta sección:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. El archivo se guarda automáticamente.

## Ejecuta la aptitud actualizada

Ya hemos aplicado el cambio; veamos el resultado. Copilot CLI puede volver a cargar las aptitudes editadas sin que tengas que reiniciar la conversación.

1. Escribe:

   ```plaintext
   /skills reload
   ```

2. Envía exactamente el mismo prompt que antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observa el informe final y compáralo con el primero.

## Resumen y siguientes pasos

Has personalizado y utilizado una aptitud de agente existente. En esta lección:

- has explorado la aptitud `quality-checks` para las pruebas unitarias, el lint y las comprobaciones de tipos;
- has personalizado el formato de sus resultados;
- has vuelto a cargar y ejecutado la aptitud.

Este cambio acompañará al filtrado en la PR de la función. A continuación, permitirás que Copilot interactúe directamente con el sitio [mediante el servidor Playwright MCP][next-lesson].

## Más ejemplos de aptitudes

Estos ejemplos de la comunidad sirven de referencia; no son tareas adicionales:

- [Especificación de Agent Skills][skill-spec]
- [Flujo de trabajo de contribución: `make-repo-contribution`][contribution-example]
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
