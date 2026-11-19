---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se encargara de tres tareas independientes para mejorar la experiencia de usuario y añadir funcionalidad. Aunque el agente en la nube está diseñado para trabajar de forma asíncrona y autónoma, es importante poder supervisar esas tareas.

Dispones de varias herramientas para gestionar las tareas que asignas al agente en la nube, entre ellas la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas de agente que tienen solicitudes de incorporación de cambios abiertas. Puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso para orientarla.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube;
- dirigirás una sesión en curso para pedir funcionalidad adicional.

## Escenario

Tras asignar al agente la tarea de crear un modo de alto contraste, el equipo se dio cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba trabajando en actualizar el estilo del sitio y añadir la opción de cambiar de modo, parecía lógico incluir esta función. Vas a orientar el trabajo del agente para que añada tanto el modo claro como el modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado de todas las tareas que has asignado al agente en la nube de Copilot:

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código;
   - generar API para modificar juegos;
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que ha realizado Copilot. Pueden incluir:
   - obtener el código del repositorio;
   - preparar el entorno para Copilot;
   - configurar servidores MCP;
   - completar distintos pasos de la tarea asignada.

> [!NOTE]
> Los pasos concretos que aparezcan dependerán del estado del trabajo de Copilot y del enfoque que haya seguido.

4. Observa también el panel de la solicitud de incorporación de cambios (PR), que aparece a la derecha. Desde ahí puedes consultar la PR y los archivos modificados para seguir el trabajo.

## Dirige al agente en la nube

Ahora que has visto las tareas activas, pide a Copilot que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión correspondiente a la creación del modo de alto contraste. El título exacto dependerá del nombre que haya utilizado Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, añade este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la nube en la página de agentes, con el cuadro de diálogo para dirigir una sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot se pondrá a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al recibir el mensaje.

Como este proceso tardará varios minutos, puedes aprovechar para hacer una pausa y repasar lo que has aprendido y explorado hasta ahora.

## Resumen y siguientes pasos

En esta lección has explorado la página de agentes de Copilot, el centro desde el que puedes supervisar y orientar las tareas del agente en la nube de GitHub Copilot. Desde este panel puedes seguir todas las tareas activas y completadas, revisar el trabajo realizado e incluso redirigir tareas en curso para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- explorar el centro de control y la página de agentes para supervisar sus tareas;
- redirigir una sesión en curso para pedir funcionalidad adicional.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

## Recursos

- [Página de agentes de Copilot][agents-page]
- [Acerca de los agentes personalizados][custom-agents]

---

| [← Lección anterior: Agentes personalizados][previous-lesson] | [Siguiente lección: Iterar sobre el trabajo de Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
