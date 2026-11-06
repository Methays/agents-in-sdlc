---
title: "Ejercicio 4 - Supervisar y administrar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

En los dos ejercicios anteriores, has pedido al agente en la nube de Copilot que se ocupe de tres tareas diferentes para mejorar la experiencia de usuario y añadir funcionalidades. Aunque está diseñado para trabajar de forma autónoma y asíncrona, es importante que puedas supervisar las tareas.

Hay varias herramientas para administrar las tareas asignadas al agente en la nube, incluida [la página de agentes][agents-page] de GitHub.com. Desde este centro de control puedes ver todas las tareas del agente que tienen solicitudes de incorporación de cambios (PR) abiertas. También puedes explorar las operaciones realizadas e incluso dirigir una sesión en curso para orientarla.

En esta lección:

- explorarás la página de agentes para supervisar las tareas del agente en la nube.
- dirigirás una sesión en curso para solicitar funcionalidades adicionales.

## Escenario

Después de asignar al agente la creación de un modo de alto contraste, el equipo se ha dado cuenta de que también sería un buen momento para añadir un modo claro. Como ya se estaba actualizando el estilo del sitio y añadiendo funcionalidad para alternar entre modos, parecía lógico incluirlo. Quieres dirigir el trabajo del agente para asegurarte de que añada tanto el modo claro como el alto contraste.

## Revisa las tareas del agente en la nube de Copilot

Consulta el estado de todas las tareas asignadas al agente en la nube de Copilot.

1. Ve a la [página de agentes de Copilot](https://github.com/copilot/agents).
2. Observa la lista de tareas en el panel principal y en el panel izquierdo. Deberías ver las tareas que has asignado a Copilot, entre ellas:
   - actualizar la documentación del código.
   - generar interfaces de programación de aplicaciones (API) para modificar juegos.
   - añadir un modo de alto contraste al sitio web.
3. Selecciona una de las tareas en curso. Revisa las operaciones que Copilot ha realizado, entre ellas:
   - obtener el código del repositorio.
   - crear el entorno de trabajo de Copilot.
   - configurar servidores del Protocolo de contexto de modelo (Model Context Protocol, MCP).
   - realizar varios pasos para completar la tarea asignada.

> [!NOTE]
> Los pasos concretos de la lista variarán según el estado del trabajo de Copilot y el enfoque que haya seguido.

4. Fíjate también en el panel de solicitud de incorporación de cambios (PR), que aparece a la derecha. Desde allí puedes consultar la solicitud y los archivos modificados para supervisar el trabajo.

## Dirige el agente en la nube

Ahora que has visto las tareas activas, pídele a Copilot que añada el conmutador de modo claro mientras trabaja en el modo de alto contraste.

1. Selecciona la sesión relacionada con la incorporación del modo de alto contraste. El título exacto dependerá del nombre que haya utilizado Copilot y del estado actual del trabajo.

    ![Sesión de accesibilidad en el centro de control](../../../_images/ex6-accessibility-session.png)

2. Observa la sesión durante unos minutos, hasta que indique que ha terminado la configuración y ha empezado a trabajar. Lo sabrás cuando empiecen a aparecer mensajes similares a los que se muestran a continuación.
3. En el cuadro de diálogo **Steer active session while Copilot is working**, escribe este prompt:

    ```plaintext
    Mientras trabajamos en un modo de alto contraste, añadamos también un modo claro. Este modo debe tener un conmutador para que los usuarios puedan seleccionar cómo quieren ver el sitio.
    ```

    ![Captura de pantalla de la tarea del agente en la página de agentes, con el cuadro de diálogo para dirigir la sesión activa de Copilot resaltado](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pulsa <kbd>Enter</kbd> para enviar el prompt.
5. Observa cómo Copilot confirma que ha recibido el prompt y lo incorpora a su flujo de trabajo.

## Deja que Copilot trabaje

Como antes, Copilot empezará a trabajar en la tarea actualizada. Incorporará la nueva solicitud a su flujo cuando termine el paso en el que estaba trabajando al recibir tu mensaje.

Este proceso también tardará varios minutos. Es un buen momento para hacer una pausa y reflexionar sobre todo lo que has aprendido y explorado hasta ahora.

## Resumen y pasos siguientes

En esta lección has explorado la página de agentes de Copilot, el centro principal para supervisar y dirigir las tareas del agente en la nube de GitHub Copilot. Desde este centro de control puedes seguir las tareas activas y completadas, revisar el trabajo y redirigir las tareas en curso para ajustar su alcance o proporcionar más orientación.

Has explorado estos conceptos:

- Has explorado el centro de control y la página de agentes para supervisar las tareas del agente en la nube.
- Has redirigido una sesión en curso para solicitar funcionalidades adicionales.

Mientras Copilot termina de trabajar en las funciones de accesibilidad, puedes centrarte en [iterar sobre las solicitudes de incorporación de cambios que ha creado][next-lesson].

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
