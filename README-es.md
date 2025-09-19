> **Desarrollo Activo** — Actualizado 16 de septiembre de 2025  
> Último: Añadidas nuevas técnicas de planificación y selección de modelos  
> [Ver todas las actualizaciones →](CHANGELOG.md)

**Idiomas disponibles:** [English](README.md) | [Español](README-es.md) | [Deutsch](README-de.md) | [Français](README-fr.md) | [日本語](README-ja.md)

# Hay una brecha entre las demos de codificación con IA y la realidad diaria

He estado usando Claude Code y Codex CLI diariamente durante 6 semanas, y Cursor durante más de un año antes de eso. Buenos resultados, definitivamente más rápido que antes. Pero leyendo lo que otros logran, me seguía preguntando: ¿qué me estoy perdiendo?

Resulta que bastante.

Después de investigar cómo los desarrolladores están realmente usando estas herramientas, encontré técnicas específicas que muchos de nosotros no conocemos. [Indragie Karunaratne envió una aplicación completa de macOS con Claude](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code) - ¿pero cómo? Los desarrolladores describen migrar bibliotecas de interfaz completas en horas en lugar de semanas - ¿usando exactamente qué flujo de trabajo?

## Las técnicas que probablemente no estás usando

Hay patrones específicos que separan las ganancias moderadas de los resultados transformadores. Ejemplos:

- **Archivos de memoria** (CLAUDE.md, .cursorrules) que persisten contexto entre sesiones - muchos desarrolladores no saben que existen
- **Regeneración basada en pruebas** - deja que la IA itere contra las pruebas en lugar de depurar línea por línea
- **Sesiones paralelas de IA** - ejecuta múltiples agentes simultáneamente usando git worktrees o contenedores

Estas técnicas están dispersas en documentación, posts de blog e hilos. Encontrarlas requiere saber qué buscar.

## Para quién es esto

Si ya estás usando herramientas de codificación con IA pero sospechas que solo estás arañando la superficie - probablemente tienes razón.

Esta colección llena esos vacíos.

📝 **Contribuyendo**: Ve [CONTRIBUTING.md](CONTRIBUTING.md) para compartir tus técnicas y experiencias

## Tabla de Contenidos
- [Requisitos y Planificación](#requisitos-y-planificación)
- [Interfaz y Prototipado](#interfaz-y-prototipado)
- [Codificación](#codificación)
- [Depuración](#depuración)
- [Pruebas y Control de Calidad](#pruebas-y-control-de-calidad)
- [Técnicas Transversales](#técnicas-transversales)

---

## Requisitos y Planificación

### Leer, Planificar, Codificar, Confirmar

Haz que explore el código, luego haga un plan, lo implemente y lo confirme.

> "There's a process that I call 'priming' the agent, where instead of having the agent jump straight to performing a task, I have it read additional context upfront to increase the chances that it will produce good outputs."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=There's%20a%20process%20that%20I%20call)

> "Hay un proceso al que llamo 'preparación' del agente, donde en lugar de hacer que el agente salte directamente a realizar una tarea, le hago leer contexto adicional por adelantado para aumentar las posibilidades de que produzca buenas salidas."
> — Traducido por Claude

### Configurar Archivos de Memoria

Crea archivos de contexto que guíen persistentemente las herramientas sobre la estructura, estándares y preferencias de tu proyecto.

> "`CLAUDE.md` is a special file that Claude automatically pulls into context when starting a conversation. This makes it an ideal place for documenting: common bash commands, core files and utility functions, code style guidelines, testing instructions."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=CLAUDE.md%20is%20a%20special%20file)

> "`CLAUDE.md` es un archivo especial que Claude automáticamente incluye en el contexto al iniciar una conversación. Esto lo convierte en un lugar ideal para documentar: comandos bash comunes, archivos centrales y funciones utilitarias, pautas de estilo de código, instrucciones de prueba."
> — Traducido por Claude

### Escribir Especificaciones Detalladas

Da especificaciones completas - incluso una especificación conversacional supera las instrucciones vagas.

> "Here's a recent example: `Write a Python function that uses asyncio httpx with this signature:` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Given a URL, this downloads the database to a temp directory and returns a path to it. BUT it checks the content length header at the start of streaming back that data and, if it's more than the limit, raises an error... I find LLMs respond extremely well to function signatures like the one I use here."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=Here's%20a%20recent%20example)

> "Aquí hay un ejemplo reciente: `Escribe una función de Python que use asyncio httpx con esta firma:` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Dada una URL, esto descarga la base de datos a un directorio temporal y devuelve una ruta hacia ella. PERO verifica el encabezado de longitud del contenido al inicio de transmitir esos datos y, si es más que el límite, lanza un error... Encuentro que los LLMs responden extremadamente bien a firmas de función como la que uso aquí."
> — Traducido por Claude

### Cerebro Primero, IA Segundo

Bosqueja la solución tú mismo primero, luego usa asistentes para refinarla.

> "I'm subconsciously defaulting to AI for all things coding. I've been using pen and paper less. As soon as I need to plan a new feature, my first thought is asking o4-mini-high how to do it, instead of my neurons. I hate this. And I'm changing it."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20subconsciously%20defaulting%20to%20AI)

> "Subconscientemente estoy recurriendo por defecto a la IA para todo lo relacionado con la codificación. He estado usando menos lápiz y papel. Tan pronto como necesito planificar una nueva función, mi primer pensamiento es preguntar a o4-mini-high cómo hacerlo, en lugar de usar mis neuronas. Odio esto. Y lo estoy cambiando."
> — Traducido por Claude

### Obtener Múltiples Opciones

Pide al LLM que presente varios enfoques con pros/contras para que puedas elegir la mejor opción.

> "I'll use prompts like `what are options for HTTP libraries in Rust? Include usage examples`"
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I'll%20use%20prompts%20like)

> "Usaré prompts como `¿cuáles son las opciones para bibliotecas HTTP en Rust? Incluye ejemplos de uso`"
> — Traducido por Claude

### Haz Preguntas Abiertas, No Dirigidas

Evita preguntas como '¿Tengo razón en que...?' - en su lugar, pide pros/contras, alternativas y '¿Qué me estoy perdiendo?' para contrarrestar la tendencia del LLM a estar de acuerdo.

### Elegir Bibliotecas Aburridas y Estables

Elige deliberadamente bibliotecas bien establecidas con buena estabilidad que existieron antes de las fechas de corte del entrenamiento de IA para una mejor generación de código con IA.

> "I gain enough value from LLMs that I now deliberately consider this when picking a library—I try to stick with libraries with good stability and that are popular enough that many examples of them will have made it into the training data. I like applying the principles of boring technology—innovate on your project's unique selling points, stick with tried and tested solutions for everything else."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20gain%20enough%20value%20from%20LLMs)

> "Obtengo suficiente valor de los LLMs que ahora considero esto deliberadamente al elegir una biblioteca—trato de ceñirme a bibliotecas con buena estabilidad y que sean lo suficientemente populares como para que muchos ejemplos de ellas hayan llegado a los datos de entrenamiento. Me gusta aplicar los principios de la tecnología aburrida—innova en los puntos de venta únicos de tu proyecto, usa soluciones probadas y comprobadas para todo lo demás."
> — Traducido por Claude

### Pedir Planificar Primero

Dile al asistente que delinee pasos, riesgos y pruebas rápidas antes de tocar código para que puedas revisar y ajustar el enfoque.

> "If you want to iterate on the plan, it helps to explicitly include instructions in the prompt to not proceed with implementation until the plan has been accepted by the user."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20you%20want%20to%20iterate%20on%20the%20plan)

> "Si quieres iterar sobre el plan, ayuda incluir explícitamente instrucciones en el prompt para no proceder con la implementación hasta que el plan haya sido aceptado por el usuario."
> — Traducido por Claude

**Implementaciones de Herramientas:**

<details>
<summary><strong>Claude Code</strong></summary>

Presiona `Shift+Tab` para entrar en Modo Plan para que solo lea y haga borradores. Usa el prompt de planificación compartido, itera hasta que se vea bien, luego sal del Modo Plan cuando des luz verde a la implementación.

</details>

<details>
<summary><strong>Cursor</strong></summary>

Haz clic en el botón Plan en Cursor para que permanezca de solo lectura mientras iteras. Haz que liste pasos, archivos impactados, riesgos y pruebas rápidas, luego sal del Modo Plan para abrir el diff una vez que des luz verde a la implementación.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Recuerda a Codex mantener la planificación separada de la implementación: lista pasos, riesgos y pruebas rápidas, pausa para tu revisión, luego déjalo implementar e inspeccionar el diff una vez aprobado.

</details>

### Planificar con Modo de Alta Capacidad

Al recopilar requisitos o redactar especificaciones, cambia temporalmente a un modelo de mayor capacidad o modo de razonamiento extendido para que pueda leer, sintetizar y proponer un plan antes de codificar.

**Implementaciones de Herramientas:**

<details>
<summary><strong>Claude Code</strong></summary>

Ejecuta `/model` y elige `opus` (u otro nivel superior) al definir requisitos para que pueda razonar profundamente, luego usa Modo Plan si quieres que permanezca de solo lectura hasta que apruebes las ediciones.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Usa `/model gpt-5-high` (u otro nivel de razonamiento extendido) para que el asistente digiera el contexto y redacte la especificación, luego baja de nivel una vez que el plan esté bloqueado.

</details>

## Interfaz y Prototipado

### Codificación por Vibra

Construye proyectos a través de conversación en lugar de codificación tradicional - habla, acepta cambios, e itera hasta que funcione.

> "...I ask for the dumbest things like `decrease the padding on the sidebar by half` because I'm too lazy to find it. I `Accept All` always, I don't read the diffs anymore. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows beyond my usual comprehension, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding—I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works."
> — [Andrej Karpathy](https://x.com/karpathy/status/1886192184808149383)

> "...pido las cosas más tontas como `disminuye el relleno de la barra lateral a la mitad` porque soy demasiado perezoso para encontrarlo. Siempre `Acepto Todo`, ya no leo los diffs. Cuando obtengo mensajes de error solo los copio y pego sin comentario, usualmente eso lo arregla. El código crece más allá de mi comprensión usual, tendría que realmente leerlo durante un tiempo. A veces los LLMs no pueden arreglar un bug así que simplemente lo sorteo o pido cambios aleatorios hasta que desaparece. No está tan mal para proyectos desechables de fin de semana, pero aún es bastante divertido. Estoy construyendo un proyecto o webapp, pero realmente no es codificación—solo veo cosas, digo cosas, ejecuto cosas, y copio y pego cosas, y mayormente funciona."
> — Traducido por Claude

### Construir un Prototipo Primero

Comienza cada proyecto con un prototipo generado rápido para demostrar que puede funcionar.

> "The best way to start any project is with a prototype that proves that the key requirements of that project can be met. I often find that an LLM can get me to that working prototype within a few minutes of me sitting down with my laptop—or sometimes even while working on my phone."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20best%20way%20to%20start%20any%20project)

> "La mejor manera de comenzar cualquier proyecto es con un prototipo que demuestre que los requisitos clave de ese proyecto se pueden cumplir. A menudo encuentro que un LLM puede llevarme a ese prototipo funcional dentro de unos pocos minutos de sentarme con mi laptop—o a veces incluso mientras trabajo en mi teléfono."
> — Traducido por Claude

### Mostrar Capturas de Pantalla

Incluye capturas de pantalla e itera - toma una captura del resultado, compara, repite.

> "Give Claude a visual mock by copying / pasting or drag-dropping an image... take screenshots of the result, and iterate until its result matches the mock."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Give%20Claude%20a%20visual%20mock)

> "Dale a Claude una simulación visual copiando/pegando o arrastrando y soltando una imagen... toma capturas de pantalla del resultado, e itera hasta que su resultado coincida con la simulación."
> — Traducido por Claude

> "I opened a second copy of Sketch and pasted in a screenshot... `this is ugly, please make it less ugly.`"
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=I%20opened%20a%20second%20copy%20of%20Sketch)

> "Abrí una segunda copia de Sketch y pegué una captura de pantalla... `esto es feo, por favor hazlo menos feo.`"
> — Traducido por Claude

### Hazlo Más Hermoso

Solo pide hacer la UI `más hermosa` o `más elegante` - funciona.

> "If Claude doesn't produce a well-designed UI the first time, you can just tell it to `make it more beautiful/elegant/usable`."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20Claude%20doesn't%20produce)

> "Si Claude no produce una UI bien diseñada la primera vez, puedes simplemente decirle que la `haga más hermosa/elegante/usable`."
> — Traducido por Claude

### Pedir Wireframes ASCII

Al refinar diseños, haz que el asistente dibuje wireframes ASCII para que puedas evaluar jerarquía y espaciado antes de tocar CSS.

> "If Claude doesn't produce a well-designed UI the first time, you can just tell it to `make it more beautiful/elegant/usable`."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20Claude%20doesn't%20produce)

> "Si Claude no produce una UI bien diseñada la primera vez, puedes simplemente decirle que la `haga más hermosa/elegante/usable`."
> — Traducido por Claude

## Codificación

### Confirmar Entendimiento Antes de Codificar

Pide explícitamente a la herramienta que confirme su entendimiento de la tarea antes de comenzar la implementación para asegurar alineación y reducir expectativas no coincidentes.

### Generar Código, No Dependencias

Escribe código personalizado en lugar de incorporar más librerías cuando trabajes con asistentes.

> "Be even more conservative about upgrades than before... I strongly prefer more code generation over using more dependencies."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Be%20even%20more%20conservative%20about)

> "Sé aún más conservador con las actualizaciones que antes... Prefiero fuertemente más generación de código sobre usar más dependencias."
> — Traducido por Claude

### Preparar con Código Existente

Comienza volcando código existente en el chat para sembrar el contexto, luego modifica desde ahí.

> "I often start a new chat by dumping in existing code to seed that context, then work with the LLM to modify it in some way."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20often%20start%20a%20new%20chat)

> "A menudo comienzo un nuevo chat volcando código existente para sembrar ese contexto, luego trabajo con el LLM para modificarlo de alguna manera."
> — Traducido por Claude

### Dar Firmas de Función Exactas

Da exactamente qué firma de función quieres - deja que maneje los detalles de implementación.

> "I find LLMs respond extremely well to function signatures like the one I use here. I get to act as the function designer, the LLM does the work of building the body to my specification. I'll often follow-up with `Now write me the tests using pytest`. Again, I dictate my technology of choice—I want the LLM to save me the time of having to type out the code that's sitting in my head already."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20find%20LLMs%20respond%20extremely%20well)

> "Encuentro que los LLMs responden extremadamente bien a firmas de función como la que uso aquí. Puedo actuar como el diseñador de la función, el LLM hace el trabajo de construir el cuerpo según mi especificación. A menudo continúo con `Ahora escríbeme las pruebas usando pytest`. De nuevo, dicto mi elección de tecnología—quiero que el LLM me ahorre el tiempo de tener que escribir el código que ya está en mi cabeza."
> — Traducido por Claude

### Delegar Tareas Tediosas

Delega tareas aburridas, sistemáticas y que consumen tiempo a la IA - desde pequeñas renombraciones de variables hasta grandes migraciones que no requieren pensamiento arquitectónico profundo.

> "I'm using LLMs, but for dumber things: `rename all occurrences of this parameter`"
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20using%20LLMs,%20but%20for%20dumber%20things)

> "Estoy usando LLMs, pero para cosas más tontas: `renombra todas las ocurrencias de este parámetro`"
> — Traducido por Claude

> "AI's best use case for me remains writing one-off scripts"
> — [Colton](https://colton.dev/blog/curing-your-ai-10x-engineer-imposter-syndrome/#:~:text=AI's%20best%20use%20case%20for%20me)

> "El mejor caso de uso de la IA para mí sigue siendo escribir scripts únicos"
> — Traducido por Claude

> "I give the agent tasks that I could do without thinking too much but that would take me a lot of time—very systematic tasks that a junior developer could do with the right explanations."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=I%20give%20the%20agent%20tasks%20that%20I%20could%20do%20without%20thinking%20too%20much%20but%20that%20would%20take%20me%20a%20lot%20of%20time%E2%80%94very%20systematic%20tasks%20that%20a%20junior%20developer%20could%20do%20with%20the%20right%20explanations.)

> "Le doy al agente tareas que podría hacer sin pensar mucho pero que me tomarían mucho tiempo—tareas muy sistemáticas que un desarrollador junior podría hacer con las explicaciones correctas."
> — Traducido por Claude

> "The best example I've found for the agent was migrating a huge app from one UI library to another. It's not hard work, but it takes a huge amount of time and is completely uninteresting."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=The%20best%20example%20I%27ve%20found%20for%20the%20agent%20was%20migrating%20a%20huge%20app%20from%20one%20UI%20library%20to%20another.%20It%27s%20not%20hard%20work%2C%20but%20it%20takes%20a%20huge%20amount%20of%20time%20and%20is%20completely%20uninteresting.)

> "El mejor ejemplo que he encontrado para el agente fue migrar una aplicación enorme de una librería de UI a otra. No es trabajo difícil, pero toma una cantidad enorme de tiempo y es completamente desinteresante."
> — Traducido por Claude

### Proporcionar Contexto para Nuevas Librerías

Cuando uses librerías fuera de los datos de entrenamiento de la IA, proporcionale ejemplos recientes y documentación para enseñarle cómo funciona la librería.

> "LLMs can still help you work with libraries that exist outside their training data, but you need to put in more work—you'll need to feed them recent examples of how those libraries should be used as part of your prompt."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=LLMs%20can%20still%20help%20you%20work%20with%20libraries)

> "Los LLMs aún pueden ayudarte a trabajar con librerías que existen fuera de sus datos de entrenamiento, pero necesitas hacer más trabajo—necesitarás alimentarlos con ejemplos recientes de cómo esas librerías deben ser usadas como parte de tu prompt."
> — Traducido por Claude

### Tratar la IA como un Interno Digital

Da a la IA instrucciones extremadamente precisas y detalladas como lo harías con un interno - proporciona firmas de función exactas y deja que maneje la implementación.

> "Once I've completed the initial research I change modes dramatically. For production code my LLM usage is much more authoritarian: I treat it like a digital intern, hired to type code for me based on my detailed instructions."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20treat%20it%20like%20a%20digital%20intern)

> "Una vez que he completado la investigación inicial, cambio de modo dramáticamente. Para código de producción mi uso de LLM es mucho más autoritario: lo trato como un interno digital, contratado para escribir código para mí basado en mis instrucciones detalladas."
> — Traducido por Claude

> "But instead of fixing the code myself, I explained why it was wrong and gave it more precise instructions. When I told it `You misunderstood, it should…` and provided clearer guidance, I was impressed at how it could understand the problem and update the code accordingly."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=But%20instead%20of%20fixing%20the%20code%20myself%2C%20I%20explained%20why%20it%20was%20wrong%20and%20gave%20it%20more%20precise%20instructions.%20When%20I%20told%20it%20%E2%80%9CYou%20misunderstood%2C%20it%20should%E2%80%A6%E2%80%9D%20and%20provided%20clearer%20guidance%2C%20I%20was%20impressed%20at%20how%20it%20could%20understand%20the%20problem%20and%20update%20the%20code%20accordingly.)

> "Pero en lugar de arreglar el código yo mismo, expliqué por qué estaba mal y le di instrucciones más precisas. Cuando le dije `Malentendiste, debería...` y proporcioné una guía más clara, me impresionó cómo pudo entender el problema y actualizar el código en consecuencia."
> — Traducido por Claude

### Mantener el Código Súper Simple

Escribe código directo con nombres de función claros, evita la herencia y trucos inteligentes - el código simple funciona mejor con IA.

> "Simple code significantly outperforms complex code in agentic contexts. I just recently wrote about ugly code and I think in the context of agents this is worth re-reading. Have the agent do `the dumbest possible thing that will work`."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Simple%20code%20significantly%20outperforms%20complex%20code)

> "El código simple supera significativamente al código complejo en contextos agénticos. Recientemente escribí sobre código feo y creo que en el contexto de agentes vale la pena releerlo. Haz que el agente haga `la cosa más tonta posible que funcione`."
> — Traducido por Claude

## Depuración

### Deja que se Pruebe y se Arregle Solo

Configura herramientas para hacer cambios, ejecutar pruebas, ver qué falla, e intentar de nuevo por su cuenta.

> "Claude is most useful when it's capable of independently driving feedback loops that allow it to make a change, test the change, and gather context on what failed to try another iteration."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=Claude%20is%20most%20useful%20when)

> "Claude es más útil cuando es capaz de manejar independientemente bucles de retroalimentación que le permiten hacer un cambio, probar el cambio, y recopilar contexto sobre qué falló para intentar otra iteración."
> — Traducido por Claude

### Usar Subagentes para Verificar Dos Veces

Generar subagentes para verificar detalles o investigar preguntas específicas.

> "Telling Claude to use subagents to verify details or investigate particular questions it might have, especially early on in a conversation or task, tends to preserve context availability without much downside in terms of lost efficiency."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Telling%20Claude%20to%20use%20subagents)

> "Decirle a Claude que use subagentes para verificar detalles o investigar preguntas particulares que pueda tener, especialmente al principio de una conversación o tarea, tiende a preservar la disponibilidad del contexto sin mucha desventaja en términos de eficiencia perdida."
> — Traducido por Claude

### Registrar Todo para Depuración con IA

Diseña sistemas con registros comprehensivos para que los agentes de IA puedan leer registros para entender qué está pasando y autodiagnosticar problemas.

> "In general logging is super important. For instance my app currently has a sign in and register flow that sends an email to the user. In debug mode (which the agent runs in), the email is just logged to stdout. This is crucial! It allows the agent to complete a full sign-in with a remote controlled browser without extra assistance. It knows that emails are being logged thanks to a CLAUDE.md instruction and it automatically consults the log for the necessary link to click."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=In%20general%20logging%20is%20super%20important)

> "En general el registro es súper importante. Por ejemplo mi aplicación actualmente tiene un flujo de inicio de sesión y registro que envía un email al usuario. En modo de depuración (en el que ejecuta el agente), el email simplemente se registra en stdout. ¡Esto es crucial! Permite al agente completar un inicio de sesión completo con un navegador controlado remotamente sin asistencia extra. Sabe que los emails están siendo registrados gracias a una instrucción de CLAUDE.md y automáticamente consulta el registro para el enlace necesario para hacer clic."
> — Traducido por Claude

## Pruebas y Control de Calidad

### Siempre Prueba el Código Tú Mismo

Absolutamente no puedes externalizar las pruebas - siempre verifica que el código realmente funcione.

> "Lo único que absolutamente no puedes externalizar a la máquina es probar que el código realmente funciona. Tu responsabilidad como desarrollador de software es entregar sistemas que funcionen. Si no lo has visto ejecutarse, no es un sistema que funciona. Necesitas invertir en fortalecer esos hábitos de QA manual."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20one%20thing%20you%20absolutely%20cannot%20outsource)

### Escribir Pruebas Primero, Luego Código

Haz que la IA escriba pruebas comprehensivas basadas en el comportamiento esperado, luego itera en la implementación hasta que todas las pruebas pasen.

> "Ask Claude to write tests based on expected input/output pairs. Be explicit about the fact that you're doing test-driven development so that it avoids creating mock implementations, even for functionality that doesn't exist yet in the codebase. Tell Claude to run the tests and confirm they fail. Ask Claude to commit the tests when you're satisfied with them. Ask Claude to write code that passes the tests, instructing it not to modify the tests."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Ask%20Claude%20to%20write%20tests)

> "Pide a Claude que escriba pruebas basadas en pares de entrada/salida esperados. Sé explícito sobre el hecho de que estás haciendo desarrollo basado en pruebas para que evite crear implementaciones simuladas, incluso para funcionalidad que aún no existe en la base de código. Dile a Claude que ejecute las pruebas y confirme que fallan. Pide a Claude que confirme las pruebas cuando estés satisfecho con ellas. Pide a Claude que escriba código que pase las pruebas, instruyéndole que no modifique las pruebas."
> — Traducido por Claude

### Pedir al Agente que Revise su Propio Código

Haz que la IA realice una revisión de código de su propio trabajo antes de la revisión humana para descubrir problemas y mejoras.

> "Asking the agent to perform a code review on its own work is surprisingly fruitful."
> — [Chris Dzombak](https://www.dzombak.com/blog/2025/08/getting-good-results-from-claude-code/#:~:text=Asking%20the%20agent%20to%20perform%20a%20code%20review)

> "Pedir al agente que realice una revisión de código de su propio trabajo es sorprendentemente fructífero."
> — Traducido por Claude

### Uno Escribe, Otro Revisa

Haz que un agente escriba código, luego usa un agente nuevo para revisar y encontrar problemas.

> "Use Claude to write code. Run `/clear` or start a second Claude in another terminal. Have the second Claude review the first Claude's work. Start another Claude (or `/clear` again) to read both the code and review feedback. Have this Claude edit the code based on the feedback. This separation often yields better results than having a single Claude handle everything."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Have%20one%20Claude%20write%20code)

> "Usa Claude para escribir código. Ejecuta `/clear` o inicia un segundo Claude en otra terminal. Haz que el segundo Claude revise el trabajo del primer Claude. Inicia otro Claude (o `/clear` nuevamente) para leer tanto el código como los comentarios de la revisión. Haz que este Claude edite el código basándose en los comentarios. Esta separación a menudo produce mejores resultados que hacer que un solo Claude maneje todo."
> — Traducido por Claude

### Editar Código en el Diff

Revisa los cambios en la vista de diff y escribe correcciones directamente en el diff antes de confirmar.

> "I manually review all AI-written code and test cases. I'll add test cases for anything I think is missing or needs improvement, either manually or by asking the LLM to write those cases (which I then review)."
> — [Chris Dzombak](https://www.dzombak.com/blog/2025/08/getting-good-results-from-claude-code/#:~:text=I%20manually%20review%20all)

> "Reviso manualmente todo el código y casos de prueba escritos por IA. Añadiré casos de prueba para cualquier cosa que creo que falta o necesita mejora, ya sea manualmente o pidiendo al LLM que escriba esos casos (que luego reviso)."
> — Traducido por Claude

## Técnicas Transversales

### Elegir Herramientas por Estilo Conversacional

Selecciona asistentes de codificación según prefieras colaboración humana o eficiencia estructurada como robot - la personalidad conversacional afecta significativamente la productividad y el disfrute.

> "En términos de personalidad, para mí es lo opuesto: Claude Code se siente como mi compañero de programación en pareja, mientras que Codex se siente como un robot (muy estructurado pero no muy humano en su estilo conversacional). El problema es que después de un rato, el '¡Tienes absolutamente razón!' me molesta. Codex es seco. Puedes insultarlo y ni siquiera responde. Sin personalidad. Claude es como un amigo que admite haberse equivocado... Codex es monótono y directo al grano, pero lo más importante es que no es complaciente en absoluto. Te desafiará cuando estés sugiriendo algo incorrecto y mantendrá su opinión."
> — [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/)

### Elegir el Modelo Correcto para el Trabajo

Antes de comenzar una nueva tarea, elige dos palancas: el modelo correcto (modalidad, longitud de contexto, confiabilidad de llamadas de herramientas, latencia, costo) y el nivel de razonamiento correcto (asignar más/menos tokens de pensamiento) — no uses los valores por defecto a ciegas.

**Implementaciones de Herramientas:**

<details>
<summary><strong>Claude Code</strong></summary>

Dos palancas al inicio de tarea: (1) Modelo vía `/model` (rápido/barato para ediciones rutinarias; contexto largo para multi-archivos/documentos largos; fuerte en visión para UI/capturas). (2) Nivel de razonamiento: habilita pensamiento extendido al abordar debugging complejo, arquitectura o especificaciones ambiguas para asignar más tokens de razonamiento.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Comienza con `gpt-5-minimal`/`gpt-5-low` para ediciones rápidas; elige una variante de mayor razonamiento `gpt-5-high`/`gpt-5-medium` cuando la complejidad aumenta.

</details>

### Centralizar Archivos de Memoria

Mantén un documento de instrucciones canónico y dirige todos los demás archivos de agente hacia él con una línea de puntero gritona, un enlace simbólico o una inclusión @archivo para que la orientación entre herramientas se mantenga consistente.

**Implementaciones de Herramientas:**

<details>
<summary><strong>Claude Code</strong></summary>

Mantén `CLAUDE.md` como la fuente de verdad y usa una de estas tres formas.

1. Pon `@CLAUDE.md` en `AGENTS.md`.

2. Enlaza simbólicamente `AGENTS.md` a `CLAUDE.md` con `ln -sf CLAUDE.md AGENTS.md` para que ambas herramientas compartan el mismo archivo.

3. Deja `AGENTS.md` como una sola línea: `¡LEE CLAUDE.md PRIMERO!`.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Usa el mismo trío desde el lado de Codex.

1. Si Codex mantiene el texto primario, deja `AGENTS.md` completo y coloca `@AGENTS.md` dentro de `CLAUDE.md` para que ambas herramientas lleguen al mismo documento.

2. Ejecuta `ln -sf CLAUDE.md AGENTS.md` para que el archivo que lee Codex sea solo un enlace simbólico a `CLAUDE.md`.

3. Cuando `CLAUDE.md` es canónico, mantén `AGENTS.md` en una línea: `¡LEE CLAUDE.md PRIMERO!`.

</details>

### Ejecutar Múltiples Agentes en Paralelo

Deja de esperar a que un agente de IA termine antes de iniciar otro - ejecuta múltiples agentes en paralelo en características separadas sin conflictos o confusión.

> "We are exploring solving both of these issues in sketch.dev using containers. By default sketch creates a little development environment in a container with a copy of the source code and the runner has the ability to extract git commits from the container. This lets you run many simultaneously."
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=We%20are%20exploring%20solving%20both%20of%20these%20issues)

> "Estamos explorando resolver ambos problemas en sketch.dev usando contenedores. Por defecto sketch crea un pequeño entorno de desarrollo en un contenedor con una copia del código fuente y el ejecutor tiene la capacidad de extraer commits de git del contenedor. Esto te permite ejecutar muchos simultáneamente."
> — Traducido por Claude

> "I disable all permission checks. Which basically means I run claude --dangerously-skip-permissions. More specifically I have an alias called claude-yolo set up."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=I%20disable%20all%20permission%20checks)

> "Deshabilito todas las verificaciones de permisos. Lo que básicamente significa que ejecuto claude --dangerously-skip-permissions. Más específicamente tengo un alias llamado claude-yolo configurado."
> — Traducido por Claude

### Aprender de Ello, Programar Tú Mismo

Usa asistentes para aprender nuevos lenguajes y conceptos, luego aplica ese conocimiento cuando programes.

> "I'm leveraging them to learn Go, to upskill myself. And then I apply this new knowledge when I code."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20leveraging%20them%20to%20learn%20Go)

> "Los estoy aprovechando para aprender Go, para mejorar mis habilidades. Y luego aplico este nuevo conocimiento cuando programo."
> — Traducido por Claude

### Una Sesión Debe Tener Un Objetivo

Usa el prompt `El objetivo de esta sesión es <objetivo específico>. Infórmame si nos desviamos del rumbo.` ya sea al inicio de cada sesión o agrégalo a tu archivo de memoria (AGENTS.md, CLAUDE.md) para prevenir contaminación de contexto y aumentar la direccionabilidad del agente - aplicando el Principio de Responsabilidad Única a las conversaciones con IA.

### Usar Sesiones de Funcionalidad

Aísla cada funcionalidad o tarea en sesiones separadas para reducir la hinchazón del contexto y mejorar la precisión, al igual que las ramas de funcionalidad en git aíslan los cambios de código.

### Empezar Barato, Escalar Cuando Te Atasques

Comienza con modelos más rápidos/baratos para tareas rutinarias, luego escala a modelos más poderosos solo cuando encuentres problemas complejos.

> "Sonnet 4 handles 90% of tasks effectively. Switch to Opus when Sonnet gets stuck. Recommend starting with Sonnet and providing comprehensive context."
> — [Sankalp](https://sankalp.bearblog.dev/my-claude-code-experience-after-2-weeks-of-usage/#:~:text=Sonnet%204%20handles%2090%25)

> "Sonnet 4 maneja el 90% de las tareas efectivamente. Cambia a Opus cuando Sonnet se atasque. Recomiendo empezar con Sonnet y proporcionar contexto comprehensivo."
> — Traducido por Claude

### Construir Herramientas Rápidas y a Prueba de Tontos

Crea herramientas que respondan rápidamente, proporcionen mensajes de error claros, y se protejan contra ser usadas incorrectamente por agentes de IA.

> "Tools need to be fast. The quicker they respond (and the less useless output they produce) the better. Crashes are tolerable; hangs are problematic. Tools need to be user friendly! Tools must clearly inform agents of misuse or errors to ensure forward progress. Tools need to be protected against an LLM chaos monkey using them completely wrong. There is no such thing as user error or undefined behavior!"
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Tools%20need%20to%20be%20fast)

> "Las herramientas necesitan ser rápidas. Mientras más rápido respondan (y menos salida inútil produzcan) mejor. Los crashes son tolerables; los cuelgues son problemáticos. ¡Las herramientas necesitan ser amigables para el usuario! Las herramientas deben informar claramente a los agentes sobre mal uso o errores para asegurar progreso hacia adelante. Las herramientas necesitan estar protegidas contra un mono del caos LLM usándolas completamente mal. ¡No existe tal cosa como error de usuario o comportamiento indefinido!"
> — Traducido por Claude

### Usar Énfasis Fuerte en Prompts

Usa IMPORTANTE, NUNCA, SIEMPRE liberalmente en prompts para dirigir a la IA lejos de errores comunes - sigue siendo el enfoque más efectivo.

> "Unfortunately CC is no better when it comes to asking the model to not do something. IMPORTANT, VERY IMPORTANT, NEVER and ALWAYS seem to be the best way to steer the model away from landmines. I expect the models to get more steerable in the future and avoid this ugliness. But for now, CC uses this liberally, and so should you."
> — [Vivek (MinusX AI Team)](https://minusx.ai/blog/decoding-claude-code/#:~:text=Unfortunately%20CC%20is%20no%20better)

> "Desafortunadamente CC no es mejor cuando se trata de pedirle al modelo que no haga algo. IMPORTANTE, MUY IMPORTANTE, NUNCA y SIEMPRE parecen ser la mejor manera de dirigir al modelo lejos de minas terrestres. Espero que los modelos se vuelvan más dirigibles en el futuro y eviten esta fealdad. Pero por ahora, CC usa esto liberalmente, y tú también deberías."
> — Traducido por Claude

### Limpiar Contexto Entre Tareas

Reinicia la ventana de contexto de la IA entre tareas no relacionadas para prevenir confusión y mejorar el rendimiento en nuevos problemas.

> "During long sessions, Claude's context window can fill with irrelevant conversation, file contents, and commands. This can reduce performance and sometimes distract Claude. Use the `/clear` command frequently between tasks to reset the context window."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=During%20long%20sessions%2C%20Claude's%20context)

> "Durante sesiones largas, la ventana de contexto de Claude puede llenarse con conversación irrelevante, contenidos de archivo y comandos. Esto puede reducir el rendimiento y a veces distraer a Claude. Usa el comando `/clear` frecuentemente entre tareas para reiniciar la ventana de contexto."
> — Traducido por Claude

### Interrumpir y Redirigir a Menudo

No dejes que la IA vaya muy lejos por el camino equivocado - interrumpe, proporciona retroalimentación, y redirige tan pronto como notes problemas.

> "Press Escape to interrupt Claude during any phase (thinking, tool calls, file edits), preserving context so you can redirect or expand instructions. Double-tap Escape to jump back in history, edit a previous prompt, and explore a different direction. You can edit the prompt and repeat until you get the result you're looking for."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Press%20Escape%20to%20interrupt%20Claude)

> "Presiona Escape para interrumpir a Claude durante cualquier fase (pensando, llamadas de herramientas, ediciones de archivo), preservando el contexto para que puedas redirigir o expandir instrucciones. Doble-toca Escape para saltar hacia atrás en el historial, editar un prompt anterior, y explorar una dirección diferente. Puedes editar el prompt y repetir hasta obtener el resultado que buscas."
> — Traducido por Claude

### Usar IA como Compañero de Programación

Colabora como con un compañero de programación - explica problemas, obtén retroalimentación, y trabajen juntos en soluciones.

> "Claude Code feels like pairing with someone with a few years under their belt who just needs the occasional nudge. Then like with pairing, it's review, refactor and test time because it's still your name on the git commit."
> — [Orta Therox](https://blog.puzzmo.com/posts/2025/06/07/orta-on-claude/#:~:text=Claude%20Code%20feels%20like%20pairing)

> "Claude Code se siente como hacer pair programming con alguien con algunos años de experiencia que solo necesita el empujón ocasional. Luego como con pair programming, es tiempo de revisar, refactorizar y probar porque sigue siendo tu nombre en el commit de git."
> — Traducido por Claude

### Crear Puntos de Rollback Mientras Codificas

Crea checkpoints a los que puedes volver cuando los experimentos fallan—captura estados de trabajo conocidos como buenos antes de cambios riesgosos.

**Implementaciones de Herramientas:**

<details>
<summary><strong>Claude Code</strong></summary>

Usa `git commit` frecuentemente para crear puntos de rollback. Hace commit del código que funciona antes de cambios riesgosos para que puedas revertir con `git reset` o cambios de rama.

</details>

<details>
<summary><strong>Cursor</strong></summary>

Confía en los checkpoints de edición de IA de Cursor (Cmd/Ctrl+Z) para deshacer rápido; aún crea commits de git para puntos de rollback duraderos.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Haz commit de estados de trabajo como checkpoints antes de dejar que Codex aplique parches grandes; revierta con git si los resultados no son buenos.

</details>
