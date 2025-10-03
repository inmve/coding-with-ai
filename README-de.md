> **Aktive Entwicklung** — Aktualisiert am 19. September 2025  
> Neueste: Neue Planungs- und Modellauswahl-Techniken hinzugefügt  
> [Alle Updates ansehen →](CHANGELOG.md)

**Verfügbare Sprachen:** [English](README.md) | [Español](README-es.md) | [Deutsch](README-de.md) | [Français](README-fr.md) | [日本語](README-ja.md)

# Es gibt eine Lücke zwischen AI-Coding-Demos und der täglichen Realität

Ich verwende Claude Code und Codex CLI täglich seit 6 Wochen und Cursor über ein Jahr davor. Gute Ergebnisse, definitiv schneller als früher. Aber beim Lesen was andere erreichen, fragte ich mich immer: Was verpasse ich?

Wie sich herausstellt, ziemlich viel.

Nach der Untersuchung, wie Entwickler diese Tools tatsächlich verwenden, fand ich spezifische Techniken, die viele von uns nicht kennen. [Indragie Karunaratne hat eine komplette macOS-App mit Claude entwickelt](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code) - aber wie? Entwickler beschreiben die Migration ganzer UI-Bibliotheken in Stunden statt Wochen - mit welchem Workflow genau?

## Die Techniken, die Sie wahrscheinlich nicht verwenden

Es gibt spezifische Muster, die moderate Verbesserungen von transformativen Ergebnissen unterscheiden. Beispiele:

- **Speicherdateien** (CLAUDE.md, .cursorrules), die Kontext zwischen Sitzungen persistieren - viele Entwickler wissen nicht, dass diese existieren
- **Testbasierte Regeneration** - lassen Sie die AI gegen Tests iterieren, anstatt Zeile für Zeile zu debuggen
- **Parallele AI-Sitzungen** - führen Sie mehrere Agenten gleichzeitig mit git worktrees oder Containern aus

Diese Techniken sind über Dokumentation, Blog-Posts und Threads verstreut. Sie zu finden erfordert zu wissen, wonach man suchen muss.

## Für wen das ist

Wenn Sie bereits AI-Coding-Tools verwenden, aber vermuten, dass Sie nur an der Oberfläche kratzen - Sie haben wahrscheinlich recht.

Diese Sammlung füllt diese Lücken.

📝 **Mitwirken**: Siehe [CONTRIBUTING.md](CONTRIBUTING.md), um Ihre Techniken und Erfahrungen zu teilen

## Inhaltsverzeichnis
- [Anforderungen & Planung](#anforderungen--planung)
- [UI & Prototyping](#ui--prototyping)
- [Programmierung](#programmierung)
- [Debugging](#debugging)
- [Testen & QA](#testen--qa)
- [Übergreifende Techniken](#übergreifende-techniken)

---

## Anforderungen & Planung

### Lesen, Planen, Programmieren, Committen

Lassen Sie es den Code erkunden, dann einen Plan erstellen, implementieren und committen.

> "There's a process that I call 'priming' the agent, where instead of having the agent jump straight to performing a task, I have it read additional context upfront to increase the chances that it will produce good outputs."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=There's%20a%20process%20that%20I%20call)

> "Es gibt einen Prozess, den ich 'Vorbereitung' des Agenten nenne, bei dem der Agent, anstatt direkt zu einer Aufgabe zu springen, zusätzlichen Kontext im Voraus liest, um die Chancen zu erhöhen, dass er gute Ergebnisse produziert."
> — Übersetzt von Claude

### Speicherdateien einrichten

Erstellen Sie Kontextdateien, die Tools dauerhaft über Struktur, Standards und Präferenzen Ihres Projekts leiten.

> "`CLAUDE.md` is a special file that Claude automatically pulls into context when starting a conversation. This makes it an ideal place for documenting: common bash commands, core files and utility functions, code style guidelines, testing instructions."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=CLAUDE.md%20is%20a%20special%20file)

> "`CLAUDE.md` ist eine spezielle Datei, die Claude automatisch in den Kontext zieht, wenn eine Unterhaltung beginnt. Das macht sie zu einem idealen Ort für die Dokumentation von: häufigen bash-Befehlen, Kerndateien und Hilfsfunktionen, Code-Style-Richtlinien, Testanweisungen."
> — Übersetzt von Claude

### Detaillierte Spezifikationen schreiben

Geben Sie umfassende Spezifikationen - sogar eine gesprächige Spezifikation schlägt vage Anweisungen.

> "Here's a recent example: `Write a Python function that uses asyncio httpx with this signature:` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Given a URL, this downloads the database to a temp directory and returns a path to it. BUT it checks the content length header at the start of streaming back that data and, if it's more than the limit, raises an error... I find LLMs respond extremely well to function signatures like the one I use here."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=Here's%20a%20recent%20example)

> "Hier ist ein aktuelles Beispiel: `Schreibe eine Python-Funktion, die asyncio httpx mit dieser Signatur verwendet:` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Bei einer URL lädt dies die Datenbank in ein temporäres Verzeichnis herunter und gibt einen Pfad dazu zurück. ABER es prüft den Content-Length-Header zu Beginn des Streamings dieser Daten und, wenn er mehr als das Limit ist, wirft es einen Fehler... Ich finde, LLMs reagieren extrem gut auf Funktionssignaturen wie die, die ich hier verwende."
> — Übersetzt von Claude

### Gehirn zuerst, AI zweitens

Entwerfen Sie die Lösung zuerst selbst, dann verwenden Sie Assistenten zur Verfeinerung.

> "I'm subconsciously defaulting to AI for all things coding. I've been using pen and paper less. As soon as I need to plan a new feature, my first thought is asking o4-mini-high how to do it, instead of my neurons. I hate this. And I'm changing it."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20subconsciously%20defaulting%20to%20AI)

> "Ich verwende unbewusst standardmäßig AI für alles, was mit Programmieren zu tun hat. Ich benutze Stift und Papier weniger. Sobald ich ein neues Feature planen muss, ist mein erster Gedanke, o4-mini-high zu fragen, wie man es macht, anstatt meine Neuronen. Ich hasse das. Und ich ändere es."
> — Übersetzt von Claude

> "Write the initial version yourself and ask AI to review and improve it."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20the%20initial%20version%20yourself%20and%20ask%20AI%20to%20review%20and%20improve%20it)

> "Schreiben Sie die erste Version selbst und bitten Sie die KI, sie zu überprüfen und zu verbessern."
> — Übersetzt von Claude

### Mehrere Optionen erhalten

Bitten Sie das LLM, mehrere Ansätze mit Vor-/Nachteilen zu präsentieren, damit Sie die beste Option wählen können.

> "I'll use prompts like `what are options for HTTP libraries in Rust? Include usage examples`"
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I'll%20use%20prompts%20like)

> "Ich verwende Prompts wie `was sind Optionen für HTTP-Bibliotheken in Rust? Füge Verwendungsbeispiele hinzu`"
> — Übersetzt von Claude

### Offene Fragen stellen, keine suggestiven

Vermeiden Sie Fragen wie 'Habe ich recht, dass...?' - fragen Sie stattdessen nach Vor-/Nachteilen, Alternativen und 'Was übersehe ich?' um der Tendenz des LLM entgegenzuwirken, zuzustimmen.

### Langweilige, stabile Bibliotheken wählen

Wählen Sie bewusst etablierte Bibliotheken mit guter Stabilität, die vor AI-Training-Stichtagen existierten, für bessere AI-Code-Generierung.

> "I gain enough value from LLMs that I now deliberately consider this when picking a library—I try to stick with libraries with good stability and that are popular enough that many examples of them will have made it into the training data. I like applying the principles of boring technology—innovate on your project's unique selling points, stick with tried and tested solutions for everything else."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20gain%20enough%20value%20from%20LLMs)

> "Ich gewinne genug Wert aus LLMs, dass ich das jetzt bewusst bei der Auswahl einer Bibliothek berücksichtige—ich versuche bei Bibliotheken mit guter Stabilität zu bleiben, die populär genug sind, dass viele Beispiele davon in die Trainingsdaten gelangt sind. Ich wende gerne die Prinzipien langweiliger Technologie an—innoviere bei den einzigartigen Verkaufsargumenten deines Projekts, bleibe bei bewährten Lösungen für alles andere."
> — Übersetzt von Claude

### Um Planung zuerst bitten

Sagen Sie dem Assistenten, er soll Schritte, Risiken und schnelle Tests skizzieren, bevor er Code berührt, damit Sie den Ansatz überprüfen und anpassen können.

> "If you want to iterate on the plan, it helps to explicitly include instructions in the prompt to not proceed with implementation until the plan has been accepted by the user."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20you%20want%20to%20iterate%20on%20the%20plan)

> "Wenn Sie über den Plan iterieren möchten, hilft es, explizit Anweisungen in den Prompt zu incluieren, nicht mit der Implementierung fortzufahren, bis der Plan vom Benutzer akzeptiert wurde."
> — Übersetzt von Claude

**Tool-Implementierungen:**

<details>
<summary><strong>Claude Code</strong></summary>

Drücken Sie `Shift+Tab`, um in den Planmodus zu gelangen, sodass es nur liest und entwirft. Verwenden Sie den gemeinsamen Planungsprompt, iterieren Sie, bis es richtig aussieht, dann verlassen Sie den Planmodus, wenn Sie die Implementierung freigeben.

</details>

<details>
<summary><strong>Cursor</strong></summary>

Klicken Sie auf den Plan-Schalter in Cursor, damit es schreibgeschützt bleibt, während Sie iterieren. Lassen Sie es Schritte, betroffene Dateien, Risiken und schnelle Tests auflisten, dann verlassen Sie den Planmodus, um das Diff zu öffnen, sobald Sie die Implementierung freigeben.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Erinnern Sie Codex daran, Planung von Implementierung getrennt zu halten: Schritte, Risiken und schnelle Tests auflisten, für Ihre Überprüfung pausieren, dann lassen Sie es implementieren und das Diff inspizieren, sobald es genehmigt ist.

</details>

### Mit hochkapazitärem Modus planen

Beim Sammeln von Anforderungen oder Entwerfen von Spezifikationen wechseln Sie vorübergehend zu einem leistungsfähigeren Modell oder erweiterten Denkermodus, damit es lesen, synthetisieren und einen Plan vorschlagen kann, bevor es codiert.

**Tool-Implementierungen:**

<details>
<summary><strong>Claude Code</strong></summary>

Führen Sie `/model` aus und wählen Sie `opus` (oder einen anderen höheren Tier), wenn Sie Anforderungen definieren, damit es tief denken kann, dann verwenden Sie den Planmodus, wenn Sie möchten, dass es schreibgeschützt bleibt, bis Sie Änderungen genehmigen.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Verwenden Sie `/model gpt-5-high` (oder einen anderen erweiterten Denkertier), um den Assistenten Kontext verdauen und die Spezifikation entwerfen zu lassen, dann steigen Sie wieder herunter, sobald der Plan feststeht.

</details>

### Spezifikationsgesteuerte Entwicklung: Iterieren bis es funktioniert

Iterieren Sie über Spezifikationen in Markdown, bis der Assistent funktionierenden Code generiert - behandeln Sie Spezifikationen als die einzige Quelle der Wahrheit statt Code direkt zu schreiben.

> "The workflow involves iterating on specifications in Markdown files, asking AI to compile into code, running/testing the app, and updating the spec if something doesn't work as expected. Developers should treat specifications as living documents, constantly updating and refining them to guide AI code generation with increasing precision."
> — [GitHub Engineering](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-using-markdown-as-a-programming-language-when-building-with-ai/)

> "Der Arbeitsablauf umfasst das Iterieren über Spezifikationen in Markdown-Dateien, die KI zu bitten, in Code zu kompilieren, die App auszuführen/zu testen und die Spezifikation zu aktualisieren, wenn etwas nicht wie erwartet funktioniert. Entwickler sollten Spezifikationen als lebende Dokumente behandeln, die ständig aktualisiert und verfeinert werden, um die KI-Codegenerierung mit zunehmender Präzision zu leiten."
> — Übersetzt von Claude

> "**Counter-argument:** If, given the prompt, AI does the job perfectly on first or second iteration — fine. Otherwise, stop refining the prompt. Go write some code, then get back to the AI. You'll get much better results."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=If%2C%20given%20the%20prompt%2C%20AI%20does%20the%20job%20perfectly%20on%20first%20or%20second%20iteration%20%E2%80%94%20fine.%20Otherwise%2C%20stop%20refining%20the%20prompt)

> "**Gegenargument:** Wenn die KI bei der gegebenen Eingabeaufforderung die Arbeit in der ersten oder zweiten Iteration perfekt erledigt — gut. Andernfalls hören Sie auf, die Eingabeaufforderung zu verfeinern. Gehen Sie und schreiben Sie etwas Code, dann kehren Sie zur KI zurück. Sie werden viel bessere Ergebnisse erzielen."
> — Übersetzt von Claude

## UI & Prototyping

### Vibe Coding

Bauen Sie Projekte durch Unterhaltung statt traditionellem Programmieren - sprechen, Änderungen akzeptieren und iterieren, bis es funktioniert.

> "...I ask for the dumbest things like `decrease the padding on the sidebar by half` because I'm too lazy to find it. I `Accept All` always, I don't read the diffs anymore. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows beyond my usual comprehension, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding—I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works."
> — [Andrej Karpathy](https://x.com/karpathy/status/1886192184808149383)

> "...ich frage nach den dümmsten Dingen wie `reduziere das Padding in der Seitenleiste um die Hälfte`, weil ich zu faul bin, es zu finden. Ich `Akzeptiere Alles` immer, ich lese die Diffs nicht mehr. Wenn ich Fehlermeldungen bekomme, kopiere und füge ich sie einfach ohne Kommentar ein, normalerweise behebt das den Fehler. Der Code wächst über mein übliches Verständnis hinaus, ich müsste ihn wirklich eine Weile durchlesen. Manchmal können die LLMs einen Bug nicht beheben, also umgehe ich ihn einfach oder frage nach zufälligen Änderungen, bis er verschwindet. Es ist nicht zu schlecht für Wegwerf-Wochenendprojekte, aber trotzdem ziemlich amüsant. Ich baue ein Projekt oder eine Webapp, aber es ist nicht wirklich Programmieren—ich sehe einfach Dinge, sage Dinge, führe Dinge aus und kopiere und füge Dinge ein, und es funktioniert meistens."
> — Übersetzt von Claude

### Zuerst einen Prototyp erstellen

Beginnen Sie jedes Projekt mit einem schnell generierten Prototyp, um zu beweisen, dass es funktionieren kann.

> "The best way to start any project is with a prototype that proves that the key requirements of that project can be met. I often find that an LLM can get me to that working prototype within a few minutes of me sitting down with my laptop—or sometimes even while working on my phone."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20best%20way%20to%20start%20any%20project)

> "Der beste Weg, ein Projekt zu beginnen, ist mit einem Prototyp, der beweist, dass die Hauptanforderungen dieses Projekts erfüllt werden können. Ich stelle oft fest, dass ein LLM mich zu diesem funktionierenden Prototyp innerhalb weniger Minuten bringen kann, nachdem ich mich mit meinem Laptop hingesetzt habe—oder manchmal sogar während ich an meinem Telefon arbeite."
> — Übersetzt von Claude

### Screenshots zeigen

Fügen Sie Screenshots ein und iterieren Sie - machen Sie einen Screenshot des Ergebnisses, vergleichen Sie, wiederholen Sie.

> "Give Claude a visual mock by copying / pasting or drag-dropping an image... take screenshots of the result, and iterate until its result matches the mock."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Give%20Claude%20a%20visual%20mock)

> "Geben Sie Claude eine visuelle Vorlage durch Kopieren/Einfügen oder Drag-and-Drop eines Bildes... machen Sie Screenshots des Ergebnisses und iterieren Sie, bis das Ergebnis der Vorlage entspricht."
> — Übersetzt von Claude

> "I opened a second copy of Sketch and pasted in a screenshot... `this is ugly, please make it less ugly.`"
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=I%20opened%20a%20second%20copy%20of%20Sketch)

> "Ich öffnete eine zweite Kopie von Sketch und fügte einen Screenshot ein... `das ist hässlich, bitte mach es weniger hässlich.`"
> — Übersetzt von Claude

### Es schöner machen

Bitten Sie einfach darum, die UI `schöner` oder `eleganter` zu machen - es funktioniert.

> "If Claude doesn't produce a well-designed UI the first time, you can just tell it to `make it more beautiful/elegant/usable`."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20Claude%20doesn't%20produce)

> "Wenn Claude beim ersten Mal keine gut gestaltete UI produziert, können Sie es einfach bitten, sie `schöner/eleganter/benutzbarer zu machen`."
> — Übersetzt von Claude

### Um ASCII-Wireframes bitten

Beim Verfeinern von Layouts lassen Sie den Assistenten ASCII-Wireframes skizzieren, damit Sie Hierarchie und Abstände bewerten können, bevor Sie CSS berühren.

## Programmierung

### Verständnis vor dem Programmieren bestätigen

Bitten Sie das Tool explizit, sein Verständnis der Aufgabe zu bestätigen, bevor Sie mit der Implementierung beginnen, um Abstimmung sicherzustellen und nicht übereinstimmende Erwartungen zu reduzieren.

### Kritische Teile handhaben, den Rest delegieren

Schreiben Sie die kritischen, komplexen Teile des Codes selbst und delegieren Sie die verbleibende unkomplizierte Implementierung an den Assistenten.

> "Write the critical parts and ask AI to do the rest."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20the%20critical%20parts%20and%20ask%20AI%20to%20do%20the%20rest)

> "Schreiben Sie die kritischen Teile und bitten Sie die KI, den Rest zu erledigen."
> — Übersetzt von Claude

### Code generieren, nicht Abhängigkeiten

Schreiben Sie benutzerdefinierten Code, anstatt beim Arbeiten mit Assistenten mehr Bibliotheken einzubeziehen.

> "Be even more conservative about upgrades than before... I strongly prefer more code generation over using more dependencies."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Be%20even%20more%20conservative%20about)

> "Seien Sie noch konservativer bei Updates als zuvor... Ich bevorzuge stark mehr Code-Generierung gegenüber der Verwendung von mehr Abhängigkeiten."
> — Übersetzt von Claude

### Mit vorhandenem Code vorbereiten

Beginnen Sie damit, vorhandenen Code in den Chat zu laden, um den Kontext zu setzen, dann modifizieren Sie von dort aus.

> "I often start a new chat by dumping in existing code to seed that context, then work with the LLM to modify it in some way."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20often%20start%20a%20new%20chat)

> "Ich beginne oft einen neuen Chat, indem ich vorhandenen Code hineinlade, um diesen Kontext zu setzen, dann arbeite ich mit dem LLM zusammen, um ihn auf irgendeine Weise zu modifizieren."
> — Übersetzt von Claude

### Struktur definieren, Implementierung delegieren

Geben Sie die Struktur an - Funktionssignaturen, Code-Skizzen oder Gerüste - und lassen Sie den Assistenten die Implementierungsdetails ausfüllen.

> "I find LLMs respond extremely well to function signatures like the one I use here. I get to act as the function designer, the LLM does the work of building the body to my specification. I'll often follow-up with `Now write me the tests using pytest`. Again, I dictate my technology of choice—I want the LLM to save me the time of having to type out the code that's sitting in my head already."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20find%20LLMs%20respond%20extremely%20well)

> "Ich finde, LLMs reagieren extrem gut auf Funktionssignaturen wie die, die ich hier verwende. Ich kann als Funktions-Designer agieren, das LLM macht die Arbeit, den Körper nach meiner Spezifikation zu erstellen. Oft folge ich mit `Jetzt schreib mir die Tests mit pytest`. Wieder diktiere ich meine Technologie-Wahl—ich möchte, dass das LLM mir die Zeit spart, den Code austippen zu müssen, der bereits in meinem Kopf ist."
> — Übersetzt von Claude

> "Write an outline of the code and ask AI to fill the missing parts."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20an%20outline%20of%20the%20code%20and%20ask%20AI%20to%20fill%20the%20missing%20parts)

> "Schreiben Sie eine Code-Skizze und bitten Sie die KI, die fehlenden Teile auszufüllen."
> — Übersetzt von Claude

### Langweilige Aufgaben auslagern

Delegieren Sie langweilige, systematische und zeitraubende Aufgaben an KI - von kleinen Variablen-Umbenennungen bis zu großen Migrationen, die kein tiefes architektonisches Denken erfordern.

> "I'm using LLMs, but for dumber things: `rename all occurrences of this parameter`"
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20using%20LLMs,%20but%20for%20dumber%20things)

> "Ich verwende LLMs, aber für dümmere Dinge: `alle Vorkommen dieses Parameters umbenennen`"
> — Übersetzt von Claude

> "AI's best use case for me remains writing one-off scripts"
> — [Colton](https://colton.dev/blog/curing-your-ai-10x-engineer-imposter-syndrome/#:~:text=AI's%20best%20use%20case%20for%20me)

> "KIs bester Anwendungsfall für mich bleibt das Schreiben von einmaligen Skripten"
> — Übersetzt von Claude

> "I give the agent tasks that I could do without thinking too much but that would take me a lot of time—very systematic tasks that a junior developer could do with the right explanations."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=I%20give%20the%20agent%20tasks%20that%20I%20could%20do%20without%20thinking%20too%20much%20but%20that%20would%20take%20me%20a%20lot%20of%20time%E2%80%94very%20systematic%20tasks%20that%20a%20junior%20developer%20could%20do%20with%20the%20right%20explanations.)

> "Ich gebe dem Agenten Aufgaben, die ich ohne viel Nachdenken erledigen könnte, aber die mir viel Zeit kosten würden—sehr systematische Aufgaben, die ein Junior-Entwickler mit den richtigen Erklärungen erledigen könnte."
> — Übersetzt von Claude

> "The best example I've found for the agent was migrating a huge app from one UI library to another. It's not hard work, but it takes a huge amount of time and is completely uninteresting."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=The%20best%20example%20I%27ve%20found%20for%20the%20agent%20was%20migrating%20a%20huge%20app%20from%20one%20UI%20library%20to%20another.%20It%27s%20not%20hard%20work%2C%20but%20it%20takes%20a%20huge%20amount%20of%20time%20and%20is%20completely%20uninteresting.)

> "Das beste Beispiel, das ich für den Agenten gefunden habe, war die Migration einer riesigen App von einer UI-Bibliothek zu einer anderen. Es ist keine schwere Arbeit, aber es dauert enorm lange und ist völlig uninteressant."
> — Übersetzt von Claude

### KI als digitalen Praktikanten behandeln

Geben Sie der KI extrem präzise, detaillierte Anweisungen, wie Sie es bei einem Praktikanten tun würden - geben Sie exakte Funktionssignaturen und lassen Sie sie die Implementierung übernehmen.

> "Once I've completed the initial research I change modes dramatically. For production code my LLM usage is much more authoritarian: I treat it like a digital intern, hired to type code for me based on my detailed instructions."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20treat%20it%20like%20a%20digital%20intern)

> "Sobald ich die anfängliche Recherche abgeschlossen habe, wechsle ich drastisch den Modus. Für Produktionscode ist meine LLM-Nutzung viel autoritärer: Ich behandle es wie einen digitalen Praktikanten, der angestellt wurde, um Code für mich basierend auf meinen detaillierten Anweisungen zu tippen."
> — Übersetzt von Claude

> "But instead of fixing the code myself, I explained why it was wrong and gave it more precise instructions. When I told it `You misunderstood, it should…` and provided clearer guidance, I was impressed at how it could understand the problem and update the code accordingly."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=But%20instead%20of%20fixing%20the%20code%20myself%2C%20I%20explained%20why%20it%20was%20wrong%20and%20gave%20it%20more%20precise%20instructions.%20When%20I%20told%20it%20%E2%80%9CYou%20misunderstood%2C%20it%20should%E2%80%A6%E2%80%9D%20and%20provided%20clearer%20guidance%2C%20I%20was%20impressed%20at%20how%20it%20could%20understand%20the%20problem%20and%20update%20the%20code%20accordingly.)

> "Aber anstatt den Code selbst zu reparieren, erklärte ich, warum er falsch war, und gab präzisere Anweisungen. Als ich sagte `Du hast missverstanden, es sollte...` und klarere Führung gab, war ich beeindruckt, wie es das Problem verstehen und den Code entsprechend aktualisieren konnte."
> — Übersetzt von Claude

### Code todseinfach halten

Schreiben Sie geradlinigen Code mit klaren Funktionsnamen, vermeiden Sie Vererbung und clevere Tricks - einfacher Code funktioniert besser mit KI.

> "Simple code significantly outperforms complex code in agentic contexts. I just recently wrote about ugly code and I think in the context of agents this is worth re-reading. Have the agent do `the dumbest possible thing that will work`."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Simple%20code%20significantly%20outperforms%20complex%20code)

> "Einfacher Code übertrifft komplexen Code in agentischen Kontexten erheblich. Ich habe kürzlich über hässlichen Code geschrieben und denke, im Kontext von Agenten ist das eine Neulektüre wert. Lassen Sie den Agenten `das Dümmste machen, was funktioniert`."
> — Übersetzt von Claude

## Debugging

### Lassen Sie es sich selbst testen und reparieren

Richten Sie Tools ein, um Änderungen zu machen, Tests auszuführen, zu sehen, was fehlschlägt, und es eigenständig erneut zu versuchen.

> "Claude is most useful when it's capable of independently driving feedback loops that allow it to make a change, test the change, and gather context on what failed to try another iteration."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=Claude%20is%20most%20useful%20when)

> "Claude ist am nützlichsten, wenn es fähig ist, unabhängig Feedback-Schleifen zu steuern, die es ihm ermöglichen, eine Änderung zu machen, die Änderung zu testen und Kontext darüber zu sammeln, was fehlgeschlagen ist, um eine weitere Iteration zu versuchen."
> — Übersetzt von Claude

### Subagenten zur Doppelkontrolle verwenden

Spawnen Sie Subagenten, um Details zu überprüfen oder spezifische Fragen zu untersuchen.

> "Telling Claude to use subagents to verify details or investigate particular questions it might have, especially early on in a conversation or task, tends to preserve context availability without much downside in terms of lost efficiency."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Telling%20Claude%20to%20use%20subagents)

> "Claude zu sagen, dass es Subagenten verwenden soll, um Details zu überprüfen oder bestimmte Fragen zu untersuchen, die es haben könnte, besonders früh in einer Unterhaltung oder Aufgabe, neigt dazu, die Kontextverfügbarkeit zu bewahren, ohne viel Nachteil in Bezug auf verlorene Effizienz."
> — Übersetzt von Claude

### Alles für KI-Debugging protokollieren

Entwerfen Sie Systeme mit umfassender Protokollierung, damit KI-Agenten Logs lesen können, um zu verstehen, was passiert, und Probleme selbst zu diagnostizieren.

> "In general logging is super important. For instance my app currently has a sign in and register flow that sends an email to the user. In debug mode (which the agent runs in), the email is just logged to stdout. This is crucial! It allows the agent to complete a full sign-in with a remote controlled browser without extra assistance. It knows that emails are being logged thanks to a CLAUDE.md instruction and it automatically consults the log for the necessary link to click."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=In%20general%20logging%20is%20super%20important)

> "Im Allgemeinen ist Logging super wichtig. Zum Beispiel hat meine App derzeit einen Anmelde- und Registrierungsflow, der eine E-Mail an den Benutzer sendet. Im Debug-Modus (in dem der Agent läuft) wird die E-Mail einfach zu stdout geloggt. Das ist entscheidend! Es ermöglicht dem Agenten, eine vollständige Anmeldung mit einem ferngesteuerten Browser ohne zusätzliche Hilfe zu vervollständigen. Es weiß, dass E-Mails geloggt werden, dank einer CLAUDE.md-Anweisung, und es konsultiert automatisch das Log für den notwendigen Link zum Klicken."
> — Übersetzt von Claude

## Testen & QA

### Zuerst Tests schreiben, dann Code

Lassen Sie die KI umfassende Tests basierend auf erwartetem Verhalten schreiben, dann iterieren Sie über die Implementierung, bis alle Tests bestehen.

> "Ask Claude to write tests based on expected input/output pairs. Be explicit about the fact that you're doing test-driven development so that it avoids creating mock implementations, even for functionality that doesn't exist yet in the codebase. Tell Claude to run the tests and confirm they fail. Ask Claude to commit the tests when you're satisfied with them. Ask Claude to write code that passes the tests, instructing it not to modify the tests."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Ask%20Claude%20to%20write%20tests)

> "Bitten Sie Claude, Tests basierend auf erwarteten Input/Output-Paaren zu schreiben. Seien Sie explizit über die Tatsache, dass Sie testgetriebene Entwicklung machen, damit es vermeidet, Mock-Implementierungen zu erstellen, auch für Funktionalität, die noch nicht in der Codebasis existiert. Sagen Sie Claude, es soll die Tests ausführen und bestätigen, dass sie fehlschlagen. Bitten Sie Claude, die Tests zu committen, wenn Sie mit ihnen zufrieden sind. Bitten Sie Claude, Code zu schreiben, der die Tests besteht, und weisen Sie es an, die Tests nicht zu modifizieren."
> — Übersetzt von Claude

## Übergreifende Techniken

### Tools nach Konversationsstil wählen

Wählen Sie Coding-Assistenten basierend darauf, ob Sie menschenähnliche Zusammenarbeit oder strukturierte, roboterhafte Effizienz bevorzugen - die Konversationspersönlichkeit beeinflusst Produktivität und Zufriedenheit erheblich.

> "In Bezug auf die Persönlichkeit ist es für mich das Gegenteil: Claude Code fühlt sich wie mein Pair-Programming-Partner an, während sich Codex wie ein Roboter anfühlt (sehr strukturiert, aber nicht sehr menschlich in seinem Konversationsstil). Das Problem ist, dass mich das 'Du hast absolut recht!' nach einer Weile nervt. Codex ist trocken. Man kann es beleidigen und es antwortet nicht einmal. Keine Persönlichkeit. Claude ist wie ein Freund, der zugibt, Mist gebaut zu haben... Codex ist monoton und direkt, aber am wichtigsten ist, dass es überhaupt nicht gefällig ist. Es wird Sie herausfordern, wenn Sie etwas Falsches vorschlagen und bei seiner Meinung bleiben."
> — [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/)

### Das richtige Modell für die Aufgabe wählen

Bevor Sie eine neue Aufgabe beginnen, wählen Sie zwei Hebel: das richtige Modell (Modalität, Kontextlänge, Tool-Calling-Zuverlässigkeit, Latenz, Kosten) und das richtige Reasoning-Level (mehr/weniger Denkzeit zuweisen) — nutzen Sie nicht blind die Standardeinstellungen.

**Tool-Implementierungen:**

<details>
<summary><strong>Claude Code</strong></summary>

Zwei Hebel bei Aufgabenbeginn: (1) Modell über `/model` (schnell/günstig für Routineänderungen; langer Kontext für Multi-Dateien/lange Dokumente; vision-stark für UI/Screenshots). (2) Reasoning-Level: erweiterten Denkmodus aktivieren bei komplexem Debugging, Architektur oder mehrdeutigen Spezifikationen, um mehr Reasoning-Tokens zuzuweisen.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Beginnen Sie mit `gpt-5-minimal`/`gpt-5-low` für schnelle Änderungen; wählen Sie eine höhere Reasoning-Variante `gpt-5-high`/`gpt-5-medium`, wenn die Komplexität steigt.

</details>

### Mehrere Agenten parallel ausführen

Hören Sie auf, darauf zu warten, dass ein KI-Agent fertig ist, bevor Sie einen anderen starten - führen Sie mehrere Agenten parallel an separaten Features aus, ohne Konflikte oder Verwirrung.

> "We are exploring solving both of these issues in sketch.dev using containers. By default sketch creates a little development environment in a container with a copy of the source code and the runner has the ability to extract git commits from the container. This lets you run many simultaneously."
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=We%20are%20exploring%20solving%20both%20of%20these%20issues)

> "Wir erforschen die Lösung beider Probleme in sketch.dev mit Containern. Standardmäßig erstellt sketch eine kleine Entwicklungsumgebung in einem Container mit einer Kopie des Quellcodes, und der Runner hat die Fähigkeit, git-Commits aus dem Container zu extrahieren. Das lässt Sie viele gleichzeitig ausführen."
> — Übersetzt von Claude

> "I disable all permission checks. Which basically means I run claude --dangerously-skip-permissions. More specifically I have an alias called claude-yolo set up."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=I%20disable%20all%20permission%20checks)

> "Ich deaktiviere alle Berechtigungsprüfungen. Was grundsätzlich bedeutet, dass ich claude --dangerously-skip-permissions ausführe. Genauer gesagt habe ich einen Alias namens claude-yolo eingerichtet."
> — Übersetzt von Claude

### Davon lernen, selbst programmieren

Verwenden Sie Assistenten, um neue Sprachen und Konzepte zu lernen, dann wenden Sie dieses Wissen an, wenn Sie programmieren.

> "I'm leveraging them to learn Go, to upskill myself. And then I apply this new knowledge when I code."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20leveraging%20them%20to%20learn%20Go)

> "Ich nutze sie, um Go zu lernen, um mich weiterzubilden. Und dann wende ich dieses neue Wissen an, wenn ich programmiere."
> — Übersetzt von Claude

### Günstig anfangen, eskalieren wenn festgefahren

Beginnen Sie mit schnelleren/günstigeren Modellen für Routineaufgaben, dann eskalieren Sie zu leistungsfähigeren Modellen nur, wenn Sie auf komplexe Probleme stoßen.

> "Sonnet 4 handles 90% of tasks effectively. Switch to Opus when Sonnet gets stuck. Recommend starting with Sonnet and providing comprehensive context."
> — [Sankalp](https://sankalp.bearblog.dev/my-claude-code-experience-after-2-weeks-of-usage/#:~:text=Sonnet%204%20handles%2090%25)

> "Sonnet 4 bewältigt 90% der Aufgaben effektiv. Wechseln Sie zu Opus, wenn Sonnet steckenbleibt. Empfehle, mit Sonnet zu beginnen und umfassenden Kontext zu liefern."
> — Übersetzt von Claude

### Schnelle, narrensichere Tools bauen

Erstellen Sie Tools, die schnell reagieren, klare Fehlermeldungen liefern und sich gegen falschen Gebrauch durch KI-Agenten schützen.

> "Tools need to be fast. The quicker they respond (and the less useless output they produce) the better. Crashes are tolerable; hangs are problematic. Tools need to be user friendly! Tools must clearly inform agents of misuse or errors to ensure forward progress. Tools need to be protected against an LLM chaos monkey using them completely wrong. There is no such thing as user error or undefined behavior!"
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Tools%20need%20to%20be%20fast)

> "Tools müssen schnell sein. Je schneller sie reagieren (und je weniger nutzlose Ausgabe sie produzieren), desto besser. Abstürze sind tolerierbar; Hänger sind problematisch. Tools müssen benutzerfreundlich sein! Tools müssen Agenten klar über Missbrauch oder Fehler informieren, um Fortschritt zu gewährleisten. Tools müssen gegen einen LLM-Chaos-Affen geschützt werden, der sie völlig falsch verwendet. So etwas wie Benutzerfehler oder undefiniertes Verhalten gibt es nicht!"
> — Übersetzt von Claude

### Kontext zwischen Aufgaben löschen

Setzen Sie das Kontextfenster der KI zwischen unabhängigen Aufgaben zurück, um Verwirrung zu vermeiden und die Leistung bei neuen Problemen zu verbessern.

> "During long sessions, Claude's context window can fill with irrelevant conversation, file contents, and commands. This can reduce performance and sometimes distract Claude. Use the `/clear` command frequently between tasks to reset the context window."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=During%20long%20sessions%2C%20Claude's%20context)

> "Während langen Sitzungen kann sich Claudes Kontextfenster mit irrelevanter Unterhaltung, Dateiinhalten und Befehlen füllen. Das kann die Leistung reduzieren und manchmal Claude ablenken. Verwenden Sie den `/clear`-Befehl häufig zwischen Aufgaben, um das Kontextfenster zurückzusetzen."
> — Übersetzt von Claude

### Oft unterbrechen und umleiten

Lassen Sie die KI nicht zu weit auf dem falschen Weg gehen - unterbrechen Sie, geben Sie Feedback und leiten Sie um, sobald Sie Probleme bemerken.

> "Press Escape to interrupt Claude during any phase (thinking, tool calls, file edits), preserving context so you can redirect or expand instructions. Double-tap Escape to jump back in history, edit a previous prompt, and explore a different direction. You can edit the prompt and repeat until you get the result you're looking for."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Press%20Escape%20to%20interrupt%20Claude)

> "Drücken Sie Escape, um Claude während jeder Phase zu unterbrechen (Denken, Tool-Aufrufe, Dateibearbeitungen), wobei der Kontext erhalten bleibt, damit Sie umleiten oder Anweisungen erweitern können. Doppel-Tippen Sie Escape, um in der Geschichte zurückzuspringen, einen vorherigen Prompt zu bearbeiten und eine andere Richtung zu erkunden. Sie können den Prompt bearbeiten und wiederholen, bis Sie das gewünschte Ergebnis erhalten."
> — Übersetzt von Claude

### KI als Programmierpartner verwenden

Arbeiten Sie wie mit einem Programmierpartner zusammen - erklären Sie Probleme, holen Sie sich Feedback und arbeiten Sie gemeinsam an Lösungen.

> "Claude Code feels like pairing with someone with a few years under their belt who just needs the occasional nudge. Then like with pairing, it's review, refactor and test time because it's still your name on the git commit."
> — [Orta Therox](https://blog.puzzmo.com/posts/2025/06/07/orta-on-claude/#:~:text=Claude%20Code%20feels%20like%20pairing)

> "Claude Code fühlt sich an wie Pair Programming mit jemandem mit ein paar Jahren Erfahrung, der nur gelegentlich einen Schubs braucht. Dann wie beim Pairing ist es Zeit für Review, Refactoring und Tests, weil es immer noch Ihr Name auf dem Git-Commit ist."
> — Übersetzt von Claude

### Rollback-Punkte beim Programmieren erstellen

Erstellen Sie Checkpoints, zu denen Sie zurückkehren können, wenn Experimente fehlschlagen—erfassen Sie bekannt-gute Arbeitszustände vor riskanten Änderungen.

**Tool-Implementierungen:**

<details>
<summary><strong>Claude Code</strong></summary>

Verwenden Sie `git commit` häufig, um Rollback-Punkte zu erstellen. Committen Sie funktionierenden Code vor riskanten Änderungen, damit Sie mit `git reset` oder Branch-Wechseln zurückkehren können.

</details>

<details>
<summary><strong>Cursor</strong></summary>

Verlassen Sie sich auf Cursors AI-Edit-Checkpoints (Cmd/Ctrl+Z) für schnelles Rückgängigmachen; erstellen Sie trotzdem Git-Commits für dauerhafte Rollback-Punkte.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Commitken Sie Arbeitszustände als Checkpoints, bevor Sie Codex große Patches anwenden lassen; kehren Sie mit Git zurück, wenn die Ergebnisse nicht gut sind.

</details>