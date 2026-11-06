---
title: "Ejercicio 2 - Añade funcionalidades con el modo Agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso las actualizaciones más sencillas de una aplicación suelen requerir cambios en varios archivos y operaciones, como ejecutar pruebas. Como desarrollador, normalmente tienes que localizar todos los archivos necesarios, realizar los cambios, ejecutar las pruebas, depurar, averiguar qué archivo te has dejado y volver a modificarlo. Y así sucesivamente.

Para eso sirve el modo Agente de Copilot.

El modo Agente está diseñado para trabajar de forma más autónoma en el entorno de desarrollo integrado (IDE). Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores que detecta. Explora cómo utilizar el modo Agente para añadir funcionalidades al sitio.

> [!NOTE]
> Aunque los nombres se parecen, el modo Agente y el agente en la nube ofrecen experiencias distintas. El modo Agente realiza las tareas en el IDE, donde puedes interactuar y recibir comentarios rápidamente. El agente en la nube está pensado para trabajar de forma asíncrona como un miembro del equipo, interactuando contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio, aprenderás cómo:

- el modo Agente de Copilot puede explorar el proyecto, identificar los archivos pertinentes y realizar cambios coordinados.
- el modo Agente de GitHub Copilot puede implementar nuevas funcionalidades en la interfaz de usuario y la capa de datos.
- revisar los cambios y las pruebas generados por el modo Agente de Copilot antes de combinarlos en el código.

## Escenario

A medida que aumenta la lista de juegos, quieres permitir que los usuarios filtren por categoría y por editora. En el ejercicio anterior ya añadiste un helper de editoras. Ahora terminarás el trabajo restante en la capa de datos, la interfaz de usuario y las pruebas con el modo Agente de Copilot.

## Ejecuta el sitio web Tailspin Toys

Antes de realizar cambios, explora el sitio web Tailspin Toys para conocer sus funciones actuales.

El sitio web es una plataforma de financiación colectiva de juegos de mesa con un tema de desarrollo. Permite mostrar juegos y sus detalles. Es una única aplicación Astro que genera las páginas como lenguaje de marcado de hipertexto (HTML) estático durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle, un mapeador objeto-relacional (ORM), en su frontmatter; no hay una interfaz de programación de aplicaciones (API) de backend independiente ni un framework de interfaz de usuario del lado del cliente. Los helpers de acceso a datos reutilizables están en `src/lib/`, y la interactividad se añade mediante un pequeño `<script>` de Astro con ámbito limitado y las interfaces de programación de aplicaciones (API) estándar del modelo de objetos del documento (DOM).

### Inicia el sitio web

Para facilitar la ejecución del sitio, se ha incluido la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Sigue estos pasos para ejecutarla en GitHub Codespaces:

1. Vuelve al codespace. Después del ejercicio anterior, deberías estar en `main`.
2. Abre un terminal en el codespace con <kbd>Control</kbd>+<kbd>`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux).
3. Crea una rama y cámbiate a ella para trabajar en el filtrado:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Ejecuta este comando para iniciar el sitio web:

   ```bash
   npm run dev
   ```

   Cuando el servidor de desarrollo de Astro esté listo, debería aparecer un mensaje con la URL, similar a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo que te pide abrir una ventana del navegador para `http://localhost:4321`, ciérralo seleccionando **x**.

5. Abre el sitio web manteniendo pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y seleccionando la dirección del servidor Astro `http://localhost:4321` en el terminal.

> [!NOTE]
> Si utilizas un codespace, al seleccionar el enlace localhost en el terminal se te redirigirá automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Se trata de un túnel privado a tu codespace, que ahora aloja el servidor web.

### Explora el sitio web

Cuando el sitio esté en ejecución, explora sus funciones principales:

- **Página de inicio**: muestra una lista de juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, se abre su página de detalles, que contiene más información, como el título, la descripción, la editora y la categoría.

## Explora las tareas pendientes con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar los ejercicios siguientes, vuelve al codespace, abre el panel de Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; cada paso los indica cuando corresponde.

La implementación inicial del sitio funciona, pero queremos ampliarla con nuevas funciones. Empieza por revisar las tareas pendientes. Al crear el proyecto a partir de la plantilla, se generó automáticamente una lista de incidencias; pide a GitHub Copilot que te muestre esos elementos.

1. En el menú desplegable de agentes de la vista Chat, selecciona **Agent**. El agente planifica e implementa cambios de forma autónoma en varios archivos, ejecuta comandos de terminal e invoca herramientas.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no expresan preferencia por un modelo u otro. Al crear el taller, utilizamos Claude Sonnet 4.5 y lo incluimos en las instrucciones para que las sugerencias de código sean relativamente coherentes y la experiencia resulte adecuada. Sin embargo, como los modelos de lenguaje de gran tamaño (LLM) son probabilísticos, es posible que las sugerencias difieran de las que se muestran en el taller. Es normal y esperable.

> [!NOTE]
> Debido al carácter probabilístico de los modelos de lenguaje de gran tamaño (LLM), Copilot podría utilizar un comando distinto del Protocolo de contexto de modelo (Model Context Protocol, MCP), pero debería completar la tarea de todos modos.

3. Pide a Copilot que muestre las incidencias pendientes enviándole este prompt:

   ```plaintext
   Muéstrame la lista de tareas pendientes de mi repositorio de GitHub. Ayúdame a priorizarlas según su utilidad para el usuario.
   ```

4. Selecciona **Continue** para ejecutar el comando que enumera todas las incidencias.
5. Revisa la lista generada.

Observa que Copilot también ha priorizado los elementos según los que considera más útiles para el usuario.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto a Copilot. Utilizarás el archivo de la [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funcionalidades al sitio web.

1. En el codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las pautas generales para añadir funcionalidades. Incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones específicos para varios tipos de archivo, entre ellos:
     - Astro.
     - Tailwind CSS (hojas de estilo en cascada).
     - Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas a la ubicación del archivo de instrucciones. Esto permite reutilizar contenido, dividir instrucciones complejas en archivos más manejables y proporcionar ejemplos y plantillas.

## Implementa el filtrado

Para completar el filtrado, tendrás que realizar al menos tres cambios independientes en la aplicación:

1. Añadir o ajustar la lógica restante de filtrado en la capa de datos (`src/lib/`).
2. Añadir o actualizar las pruebas del comportamiento del filtrado.
3. Actualizar la página de lista de juegos para incluir la interfaz de filtrado.

Además, las pruebas deben ejecutarse y superarse antes de combinar los cambios en el código. El modo Agente de Copilot puede ocuparse de estas tareas.

1. Puedes continuar la conversación actual con Copilot o seleccionar **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de pantalla de un ejemplo de selección del archivo de instrucciones de interfaz de usuario](../../../_images/ex3-select-instructions-file.png)

3. Comprueba que **Agent** sigue seleccionado en el menú desplegable de agentes de la vista Chat.

   ![Captura de pantalla del selector de agentes en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Comprueba que **Claude Sonnet 4.5** sigue seleccionado como modelo.
5. Pide a Copilot que implemente la funcionalidad según la incidencia relacionada de la lista de tareas pendientes, utilizando este prompt:

   ```plaintext
   Actualiza el sitio para incluir el filtrado por editora y categoría según los requisitos de la incidencia relacionada en la lista de tareas pendientes de GitHub. Ya existe un helper de editoras del ejercicio anterior, así que conserva y ajusta ese trabajo cuando sea necesario mientras completas el resto de la capa de datos, la interfaz de usuario y las pruebas. Asegúrate de que todas las pruebas se superen antes de terminar. El servidor ya está en ejecución; no hace falta que lo inicies.
   ```

6. Observa cómo Copilot empieza por explorar el proyecto y localizar los archivos relacionados con la funcionalidad. Deberías ver que encuentra los helpers de la capa de datos, la interfaz de usuario y las pruebas. A continuación, empieza a modificar los archivos y a ejecutar las pruebas.

   ![Captura de pantalla de Copilot explorando los archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas, como explorar el proyecto, modificar archivos y ejecutar pruebas. Según la complejidad de la tarea y del código, puede tardar unos minutos. Durante el proceso, es posible que aparezcan los botones **Keep** y **Undo** en el editor de código. Cuando Copilot termine, verás las opciones **Keep** y **Undo** para todos los cambios; no tienes que seleccionarlas mientras siga trabajando.

7. Cuando Copilot te lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de pantalla de un cuadro de diálogo de Copilot Chat que pide confirmar la ejecución de las pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Es posible que haya pausas y que algunas pruebas fallen durante el proceso. No pasa nada: Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de pantalla de una sesión completa de Copilot en modo Agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código que genera Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se está ejecutando el sitio web y explora la nueva funcionalidad.
11. Cuando hayas comprobado que todo funciona y revisado el código, selecciona **Keep** en la ventana de Copilot Chat.

## Resumen y pasos siguientes

En este ejercicio, has explorado cómo utilizar el modo Agente de GitHub Copilot para añadir funcionalidades a Tailspin Toys. Has aprendido a:

- Has utilizado el modo Agente de Copilot para explorar el proyecto, identificar los archivos pertinentes y realizar cambios coordinados.
- Has implementado nuevas funcionalidades en la interfaz de usuario y la capa de datos con el modo Agente de GitHub Copilot.
- Has revisado los cambios y las pruebas generados por el modo Agente de Copilot antes de combinarlos en el código.

A continuación, [probarás la funcionalidad con el servidor MCP de Playwright][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementar la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Utiliza lo que has aprendido para pedir a Copilot que actualice el sitio e implemente esa función. Ten en cuenta lo siguiente:

1. Sigue las prácticas recomendadas, incluido el uso de los archivos de instrucciones existentes.
2. Decide cómo quieres implementar la paginación: por ejemplo, si el usuario podrá seleccionar el tamaño de página o si este será fijo.
3. Al redactar el prompt, proporciona a Copilot la orientación necesaria para que genere la implementación que quieres.
4. Puede que tengas que iterar con GitHub Copilot, solicitar cambios y proporcionar contexto. Este es el flujo normal al trabajar con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo utilizarlos][choose-mode]
- [Modo Agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
