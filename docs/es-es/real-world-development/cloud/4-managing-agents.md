---
title: "Ejercicio 4 - Supervisar y gestionar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los últimos ejercicios has pedido al agente en la nube de Copilot que se ocupe de tres tareas diferentes para mejorar la experiencia de usuario y añadir funciones. Aunque trabaja de forma autónoma y asíncrona, es importante poder supervisar estas tareas.

Hay varias herramientas para gestionar las tareas asignadas al agente en la nube, incluida la [página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver las tareas de agente que tienen solicitudes de incorporación de cambios abiertas, explorar las operaciones realizadas y guiar una sesión en curso.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- guiarás una sesión en curso para pedir una funcionalidad adicional.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo ha decidido que también convendría añadir un modo claro. Como ya se están actualizando los estilos y añadiendo controles de alternancia, es lógico incluir también esta función. Guiarás al agente para que añada el modo claro además del modo de alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Veamos el estado actual de las tareas asignadas al agente en la nube de Copilot.

1. Abre la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Fíjate en la lista de tareas del panel principal y del panel izquierdo. Deberías ver las tareas que has asignado a Copilot, como:
   - actualizar la documentación del código.
   - generar API para modificar juegos.
   - añadir un modo de alto contraste al sitio.
3. Selecciona una de las tareas en ejecución. Revisa las operaciones realizadas por Copilot, que pueden incluir:
   - obtener el código del repositorio.
   - crear el entorno de trabajo.
   - configurar servidores MCP.
   - realizar distintas acciones para completar la tarea.

> [!NOTE]
> Los pasos concretos variarán según el estado del trabajo de Copilot y el enfoque que haya adoptado.

4. Fíjate también en el panel de solicitudes de incorporación de cambios, a la derecha. Desde ahí puedes consultar la PR y los archivos modificados.

## Guía al agente en la nube

Ya has visto las tareas activas. Pidamos ahora al agente que añada el modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con el modo de alto contraste. El título exacto dependerá del nombre que le haya dado Copilot y del estado del trabajo.

    ![Sesión de accesibilidad en el panel de agentes](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que la configuración ha terminado y que ha empezado el trabajo. Verás mensajes parecidos a los siguientes.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Captura de la tarea del agente en la página de agentes, con el cuadro de diálogo para guiar una sesión activa resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot lo confirma y lo incorpora al flujo de trabajo.

## Deja que Copilot trabaje

Copilot continuará con la tarea actualizada. Incorporará la nueva solicitud al flujo cuando termine el paso en el que estaba trabajando cuando enviaste el mensaje.

Como antes, el proceso tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre lo que has aprendido y explorado.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro de control para supervisar y guiar las tareas del agente en la nube de GitHub Copilot. Desde ahí puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir una sesión en curso para ajustar su alcance o añadir indicaciones.

Has explorado estos conceptos:

- usar el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- redirigir una sesión en curso para solicitar una funcionalidad adicional.

Mientras Copilot termina el trabajo sobre las funciones de accesibilidad, puedes [iterar sobre sus solicitudes de incorporación de cambios][next-lesson].

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
