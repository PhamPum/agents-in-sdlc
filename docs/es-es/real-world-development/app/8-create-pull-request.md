---
title: "Lección 8 - Crear y fusionar la PR de la función"
description: "Revisa conjuntamente el filtrado, las instrucciones, la actualización de la aptitud, el perfil QA y las pruebas; después crea una PR y utiliza Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

La implementación del filtrado, las actualizaciones de instrucciones, la actualización de la aptitud, el perfil de control de calidad (QA) y las pruebas están guardados en una misma rama. Es el momento de revisarlos juntos y abrir una solicitud de incorporación de cambios. Tú mismo fusionaste la PR de las valoraciones por estrellas; esta vez dejarás que **Agent Merge** gestione el proceso.

> [!NOTE]
> Normalmente separaríamos la función, las actualizaciones de instrucciones, la actualización de la aptitud y el agente QA en varias PR. Para agilizar el taller, has mantenido todo el trabajo de filtrado y calidad en una sola sesión y rama, y lo incluirás en esta PR.

En esta lección:

- aprenderás qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión;
- inspeccionarás todos los cambios de la función y las pruebas de verificación;
- crearás la PR de filtrado;
- habilitarás Agent Merge solo después de revisarla y confirmarás que se ha fusionado.

## Escenario

A lo largo del flujo de trabajo de filtrado, has utilizado Copilot para planificar, implementar y verificar una función. Ahora Tailspin Toys quiere automatizar el trabajo pendiente de la PR, pero mantener bajo el control del desarrollador la autorización para fusionarla.

## Introducción a Agent Merge

**Agent Merge** automatiza el trabajo restante para incorporar una solicitud de incorporación de cambios en la aplicación GitHub Copilot. Cuando lo habilitas, la sesión de la aplicación lee la solicitud y resuelve lo que la bloquea: corrige las comprobaciones de integración continua (CI) que fallan, responde a comentarios de revisión y actualiza la rama con la rama de destino cuando es necesario. Después, fusiona la solicitud en cuanto GitHub lo permite. Se ejecuta en segundo plano, continúa aunque reinicies la aplicación y se desactiva cuando se fusiona la solicitud.

Hasta ahora seleccionabas tú mismo **Merge pull request**. Agent Merge puede encargarse de esa tarea, pero su capacidad de editar código y fusionarlo sigue requiriendo tu autorización explícita. Revisa las acciones permitidas y el trabajo antes de conceder permiso para fusionar.

## Utiliza Agent Merge para gestionar la PR

Ya has creado y revisado todo el código; ahora permitiremos que Agent Merge gestione el proceso de la PR.

1. Utiliza el selector de agentes para elegir **Default agent**.
2. Selecciona el menú desplegable junto a **Create PR**.
3. Selecciona **Agent Merge**. El botón cambiará a **Agent Merge**.
4. Selecciona el botón **Agent Merge** para iniciar el proceso. Elegir la opción del menú desplegable no lo inicia por sí solo.

Se inicia el proceso de Agent Merge. Hará lo siguiente:

- crear la solicitud de incorporación de cambios con un título y una descripción;
- si has iniciado la sesión desde una incidencia, incluir una referencia a ella en el cuerpo de la descripción;
- actualizar la rama con la rama de destino o resolver posibles conflictos de fusión;
- supervisar el proceso de CI para asegurarse de que se superen todas las comprobaciones;
- supervisar los comentarios de otros desarrolladores o de la revisión de código de Copilot y realizar los cambios necesarios para resolverlos;
- opcionalmente, fusionar automáticamente la PR cuando todo se haya completado correctamente.

Dejaremos que Agent Merge fusione también la PR cuando se hayan superado todas las comprobaciones.

5. Revisa el diff de la PR y las pruebas de verificación antes de conceder permiso para fusionarla.
6. Selecciona el menú desplegable junto a **Agent Merge**.
7. Comprueba que **Merge pull request** está marcado.
8. Pide explícitamente al agente que fusione la PR:

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. Supervisa el progreso y resuelve los bloqueos que se indiquen. Antes de continuar, confirma que el estado de la PR es **Merged**; que se haya habilitado el permiso de fusión o que haya un agente trabajando no confirma que la PR se haya fusionado.

> [!IMPORTANT]
> Agent Merge no elude las protecciones del repositorio ni los permisos que falten. Resuelve esos bloqueos antes de continuar.

## Resumen y siguientes pasos

Has automatizado varios aspectos del proceso de desarrollo: generar código, probarlo y validarlo, y ahora gestionar la solicitud de incorporación de cambios. En esta lección:

- has aprendido qué es Agent Merge y cómo automatiza el ciclo de vida de una fusión;
- has inspeccionado todos los cambios de la función y las pruebas de verificación;
- has creado la PR de filtrado;
- has habilitado Agent Merge solo después de la revisión y has confirmado la fusión de la PR.

A continuación, [utilizarás un canvas existente y crearás un canvas de clasificación][next-lesson] para explorar una forma más completa de examinar, planificar y visualizar el trabajo con el agente.

## Recursos

- [Gestionar incidencias y solicitudes de incorporación de cambios con la aplicación GitHub Copilot][managing-issues-prs]
- [Acerca de la aplicación GitHub Copilot][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
