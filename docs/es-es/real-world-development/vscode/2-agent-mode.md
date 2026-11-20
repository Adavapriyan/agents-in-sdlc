---
title: "Ejercicio 2 - Añade funcionalidad con el modo de agente de Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Incluso los cambios más sencillos suelen implicar actualizar varios archivos y realizar operaciones como ejecutar pruebas. Como desarrollador, el flujo suele consistir en localizar todos los archivos necesarios, hacer los cambios, ejecutar las pruebas, depurar, averiguar qué archivo falta, hacer otra actualización y continuar con el ciclo.

El modo de agente de Copilot puede ayudarte con este trabajo.

El modo de agente está diseñado para trabajar con más autonomía en el IDE. Se comporta de forma parecida a un desarrollador: primero explora la estructura del proyecto, realiza las actualizaciones necesarias, ejecuta tareas como las pruebas y corrige automáticamente los errores detectados. Veamos cómo usarlo para añadir funcionalidad al sitio.

> [!NOTE]
> Aunque los nombres sean parecidos, el modo de agente y el agente en la nube ofrecen experiencias distintas. El modo de agente trabaja en tu IDE y permite ciclos de respuesta rápidos e interacción directa. El agente en la nube trabaja de forma asíncrona, como un miembro del equipo, y se comunica contigo mediante incidencias y solicitudes de incorporación de cambios.

En este ejercicio aprenderás a:

- usar el modo de agente de Copilot para explorar el proyecto e identificar los archivos pertinentes.
- implementar funciones en la interfaz y la capa de datos.
- revisar los cambios y las pruebas generados por Copilot antes de fusionarlos.

## Escenario

A medida que crece la lista de juegos, quieres que los usuarios puedan filtrarla por categoría y editorial. En el ejercicio anterior añadiste una función auxiliar para editoriales; ahora completarás con Copilot el resto del trabajo de datos, interfaz y pruebas.

## Ejecuta el sitio de Tailspin Toys

Antes de hacer cambios, exploremos el sitio para entender su funcionalidad actual.

El sitio es una plataforma de financiación colectiva de juegos de mesa con una temática para desarrolladores. Permite mostrar juegos y sus detalles. Es una aplicación Astro que genera páginas HTML estáticas durante la compilación. Las páginas consultan directamente una base de datos SQLite local mediante Drizzle ORM; no hay una API de backend independiente ni un framework de interfaz del lado del cliente. Las funciones reutilizables de acceso a datos se encuentran en `src/lib/`, y la interactividad se implementa con scripts Astro pequeños y acotados que usan las API DOM estándar.

### Inicia el sitio

El proyecto incluye la tarea `npm run dev`, que inicia el único servidor de desarrollo de Astro. Para ejecutarla en Codespaces:

1. Vuelve a tu codespace. Después del ejercicio anterior deberías estar en `main`.
2. Abre un terminal con <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) o <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Actualiza `main` y crea una rama para los filtros:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicia el sitio:

   ```bash
   npm run dev
   ```

   Cuando el servidor Astro esté listo, verás un mensaje parecido a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Si aparece un cuadro de diálogo para abrir el navegador en `http://localhost:4321`, ciérralo seleccionando **x**.

5. Mantén pulsada la tecla <kbd>Command</kbd> (Mac) o <kbd>Ctrl</kbd> (Windows/Linux) y selecciona la dirección `http://localhost:4321` en el terminal.

> [!NOTE]
> En Codespaces, al seleccionar el enlace localhost en el terminal se redirige automáticamente a `https://<your-codespace-name>-4321.app.github.dev/`. Este túnel privado permite acceder al servidor web que se ejecuta en tu codespace.

### Explora el sitio

Cuando el sitio esté en ejecución, podrás explorar sus funciones principales:

- **Página de inicio**: muestra juegos de mesa con sus títulos, imágenes y descripciones.
- **Página de detalles del juego**: al seleccionar un juego, verás información adicional, como el título, la descripción, la editorial y la categoría.

## Explora el backlog con Copilot

> [!TIP]
> **Abre Copilot Chat**
>
> Antes de empezar, vuelve a tu codespace, abre el panel Copilot Chat y selecciona **New Chat** para iniciar una conversación nueva. El modo y el modelo varían según el ejercicio; los pasos correspondientes indicarán qué seleccionar.

La implementación inicial del sitio funciona, pero queremos añadir capacidades. Empecemos por el backlog. Al crear el proyecto desde la plantilla, se generó un backlog de incidencias automáticamente. Pide a GitHub Copilot que te muestre esas incidencias.

1. Selecciona **Agent** en el selector de agentes de Chat. El agente planifica e implementa cambios en varios archivos, ejecuta comandos de terminal y usa herramientas de forma autónoma.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

2. Selecciona **Claude Sonnet 4.5** en la lista de modelos disponibles.

> [!CAUTION]
> Los autores de este taller no recomiendan un modelo concreto. Durante su desarrollo usamos Claude Sonnet 4.5 y lo indicamos para que las sugerencias sean relativamente coherentes y la experiencia sea más uniforme. Como los modelos de lenguaje no son deterministas, es normal que las sugerencias que recibas sean distintas.

> [!NOTE]
> Debido al comportamiento probabilístico de los modelos de lenguaje, Copilot podría usar un comando MCP diferente y aun así completar la tarea.

3. Envía este prompt:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecciona **Continue** para permitir que se ejecute el comando que enumera las incidencias.
5. Revisa la lista generada.

Observa cómo Copilot también ha priorizado las incidencias según lo que considera más útil para los usuarios.

## Revisa los archivos de instrucciones

Antes de pedir al agente que genere código, revisa el archivo de instrucciones que proporcionará contexto. Usarás el archivo de [interfaz de usuario (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica cómo añadir funciones al sitio.

1. En tu codespace, abre `.github/instructions/ui.instructions.md`.
2. Revisa las directrices, que incluyen:
   - una descripción general de la arquitectura.
   - principios de diseño de componentes, capacidad de prueba y accesibilidad.
   - enlaces a archivos de instrucciones para Astro, Tailwind CSS y Drizzle.

> [!TIP]
> Los archivos de instrucciones pueden incluir referencias a otros archivos de instrucciones y a archivos del proyecto. Las rutas son relativas al archivo de instrucciones. Así puedes reutilizar contenido, dividir conjuntos extensos de directrices y proporcionar ejemplos y plantillas a Copilot.

## Implementa los filtros

Para completar los filtros hay que hacer al menos tres cambios:

- añadir o ajustar la lógica de filtrado en la capa de datos (`src/lib/`).
- añadir o actualizar las pruebas del comportamiento de filtrado.
- actualizar la página con la lista de juegos para incluir la interfaz de filtrado.

Antes de fusionar el código, también hay que ejecutar las pruebas y comprobar que se superan. El modo de agente de Copilot puede realizar este trabajo.

1. Continúa en la conversación actual o selecciona **New Chat** para iniciar otra.
2. Selecciona **Add Context**, **Instructions** y **ui** como archivo de instrucciones.

   ![Captura de cómo seleccionar el archivo de instrucciones de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirma que **Agent** sigue seleccionado.

   ![Captura del selector de agente en la vista Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirma que **Claude Sonnet 4.5** sigue seleccionado.
5. Pide a Copilot que implemente la funcionalidad según la incidencia del backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observa cómo Copilot explora el proyecto y localiza los archivos pertinentes de datos, interfaz y pruebas. Después, modifica los archivos y ejecuta las pruebas.

   ![Captura de Copilot explorando archivos del proyecto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot realizará varias tareas: explorar el proyecto, modificar archivos y ejecutar pruebas. El proceso puede tardar varios minutos. Mientras trabaja, puede que aparezcan los botones **Keep** y **Undo** en el editor. No tienes que seleccionarlos mientras el agente está trabajando; al terminar tendrás la opción de conservar o deshacer todos los cambios.

7. Cuando Copilot lo solicite, selecciona **Continue** para ejecutar las pruebas.

   ![Captura de un aviso de Copilot Chat que pide permiso para ejecutar pruebas](../../../_images/ex3-agent-mode-run-tests.png)

8. Puede que haya pausas o que algunas pruebas fallen durante el proceso. Copilot alterna entre generar código y ejecutar pruebas hasta completar la tarea y dejar de detectar errores.

   ![Captura de una sesión completada de Copilot Chat en modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explora el código generado para detectar posibles problemas.

> [!CAUTION]
> Recuerda que siempre es importante revisar el código generado por Copilot o cualquier otra herramienta de IA.

10. Vuelve al navegador donde se ejecuta el sitio y explora la nueva funcionalidad.
11. Cuando hayas comprobado que funciona y revisado el código, selecciona **Keep** en Copilot Chat.

## Resumen y pasos siguientes

¡Enhorabuena! Has usado el modo de agente de GitHub Copilot para añadir funciones al sitio de Tailspin Toys. Has aprendido a:

- implementar funciones en la interfaz y la capa de datos con el modo de agente.
- explorar el proyecto e identificar los archivos pertinentes para coordinar cambios.
- revisar los cambios y las pruebas antes de fusionarlos.

A continuación, [probarás la funcionalidad con el servidor Playwright MCP][next-lesson] y abrirás una solicitud de incorporación de cambios.

### Ejercicio de exploración adicional: implementa la paginación

A medida que crezca la lista de juegos, será necesario habilitar la paginación. Aplica lo aprendido en este ejercicio y pide a Copilot que la implemente. Ten en cuenta lo siguiente:

- sigue las prácticas recomendadas y usa los archivos de instrucciones existentes.
- decide cómo quieres implementar la paginación: por ejemplo, si permites que el usuario elija el tamaño de página o si lo estableces de forma fija.
- da a Copilot las directrices necesarias para crear la implementación que deseas.
- puede que tengas que iterar, pedir cambios y aportar contexto. Es normal trabajar así con Copilot.

## Recursos

- [Modos ask, edit y agent de Copilot: qué hacen y cuándo se usan][choose-mode]
- [Modo de agente en VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
