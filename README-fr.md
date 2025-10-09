> **Développement Actif** — Mis à jour le 4 octobre 2025
> Dernière : Ajout de nouvelles techniques de planification et de sélection de modèles
> [Voir toutes les mises à jour →](CHANGELOG.md)

**Langues disponibles :** [English](README.md) | [Español](README-es.md) | [Deutsch](README-de.md) | [Français](README-fr.md) | [日本語](README-ja.md)

<div align="center">

![GitHub stars](https://img.shields.io/github/stars/inmve/awesome-ai-coding-techniques?style=social)

</div>

# Awesome AI Coding Techniques

**Techniques pratiques pour coder avec l'IA - Piloté par la communauté et testé par des praticiens**

# Il y a un fossé entre les démos de codage IA et la réalité quotidienne

J'utilise Claude Code et Codex CLI quotidiennement depuis 6 semaines, et Cursor pendant plus d'un an avant cela. Bons résultats, définitivement plus rapide qu'avant. Mais en lisant ce que d'autres accomplissent, je me demandais sans cesse : qu'est-ce qui me manque ?

Il s'avère que beaucoup de choses.

Après avoir exploré comment les développeurs utilisent réellement ces outils, j'ai trouvé des techniques spécifiques que beaucoup d'entre nous ne connaissent pas. [Indragie Karunaratne a livré une application macOS entière avec Claude](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code) - mais comment ? Les développeurs décrivent migrer des bibliothèques UI entières en heures au lieu de semaines - en utilisant exactement quel workflow ?

## Les techniques que vous n'utilisez probablement pas

Il y a des modèles spécifiques qui séparent les gains modérés des résultats transformateurs. Exemples :

- **Fichiers mémoire** (CLAUDE.md, .cursorrules) qui persistent le contexte entre les sessions - beaucoup de développeurs ne savent pas qu'ils existent
- **Régénération basée sur les tests** - laissez l'IA itérer contre les tests au lieu de déboguer ligne par ligne
- **Sessions IA parallèles** - exécutez plusieurs agents simultanément en utilisant git worktrees ou containers

Ces techniques sont dispersées à travers la documentation, les articles de blog et les fils de discussion. Les trouver nécessite de savoir quoi chercher.

## À qui cela s'adresse

Si vous utilisez déjà des outils de codage IA mais soupçonnez que vous ne faites qu'effleurer la surface - vous avez probablement raison.

Cette collection comble ces lacunes.

📝 **Contribuer** : Voir [CONTRIBUTING.md](CONTRIBUTING.md) pour partager vos techniques et expériences

## Table des matières
- [Exigences & Planification](#exigences--planification)
- [UI & Prototypage](#ui--prototypage)
- [Codage](#codage)
- [Débogage](#débogage)
- [Tests & QA](#tests--qa)
- [Techniques transversales](#techniques-transversales)

---

## Exigences & Planification

### Lire, Planifier, Coder, Committer

Faites-lui explorer le code, puis faire un plan, l'implémenter et le committer.

> "There's a process that I call 'priming' the agent, where instead of having the agent jump straight to performing a task, I have it read additional context upfront to increase the chances that it will produce good outputs."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=There's%20a%20process%20that%20I%20call)

> "Il y a un processus que j'appelle 'amorçage' de l'agent, où au lieu de faire sauter l'agent directement à l'exécution d'une tâche, je lui fais lire du contexte supplémentaire à l'avance pour augmenter les chances qu'il produise de bons résultats."
> — Traduit par Claude

### Configurer les fichiers mémoire

Créez des fichiers de contexte qui guident de manière persistante les outils sur la structure, les normes et les préférences de votre projet.

> "`CLAUDE.md` is a special file that Claude automatically pulls into context when starting a conversation. This makes it an ideal place for documenting: common bash commands, core files and utility functions, code style guidelines, testing instructions."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=CLAUDE.md%20is%20a%20special%20file)

> "`CLAUDE.md` est un fichier spécial que Claude tire automatiquement dans le contexte lors du démarrage d'une conversation. Cela en fait un endroit idéal pour documenter : les commandes bash courantes, les fichiers centraux et fonctions utilitaires, les directives de style de code, les instructions de test."
> — Traduit par Claude

### Écrire des spécifications détaillées

Donnez des spécifications complètes - même une spécification conversationnelle bat les instructions vagues.

> "Here's a recent example: `Write a Python function that uses asyncio httpx with this signature:` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Given a URL, this downloads the database to a temp directory and returns a path to it. BUT it checks the content length header at the start of streaming back that data and, if it's more than the limit, raises an error... I find LLMs respond extremely well to function signatures like the one I use here."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=Here's%20a%20recent%20example)

> "Voici un exemple récent : `Écris une fonction Python qui utilise asyncio httpx avec cette signature :` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Étant donné une URL, cela télécharge la base de données vers un répertoire temporaire et retourne un chemin vers celle-ci. MAIS il vérifie l'en-tête de longueur du contenu au début du streaming de ces données et, si c'est plus que la limite, lève une erreur... Je trouve que les LLMs répondent extrêmement bien aux signatures de fonction comme celle que j'utilise ici."
> — Traduit par Claude

### Cerveau d'abord, IA ensuite

Ébauchez la solution vous-même d'abord, puis utilisez des assistants pour la raffiner.

> "I'm subconsciously defaulting to AI for all things coding. I've been using pen and paper less. As soon as I need to plan a new feature, my first thought is asking o4-mini-high how to do it, instead of my neurons. I hate this. And I'm changing it."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20subconsciously%20defaulting%20to%20AI)

> "Je recours inconsciemment par défaut à l'IA pour tout ce qui concerne le codage. J'utilise moins le stylo et le papier. Dès que j'ai besoin de planifier une nouvelle fonctionnalité, ma première pensée est de demander à o4-mini-high comment le faire, au lieu d'utiliser mes neurones. Je déteste ça. Et je le change."
> — Traduit par Claude

> "Write the initial version yourself and ask AI to review and improve it."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20the%20initial%20version%20yourself%20and%20ask%20AI%20to%20review%20and%20improve%20it)

> "Écrivez la version initiale vous-même et demandez à l'IA de la réviser et de l'améliorer."
> — Traduit par Claude

### Obtenir plusieurs options

Demandez au LLM de présenter plusieurs approches avec des avantages/inconvénients pour que vous puissiez choisir la meilleure option.

> "I'll use prompts like `what are options for HTTP libraries in Rust? Include usage examples`"
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I'll%20use%20prompts%20like)

> "J'utilise des prompts comme `quelles sont les options pour les bibliothèques HTTP en Rust ? Inclus des exemples d'utilisation`"
> — Traduit par Claude

### Posez des Questions Ouvertes, Pas Suggestives

Évitez les questions comme 'Ai-je raison de penser que...?' - demandez plutôt des avantages/inconvénients, des alternatives et 'Qu'est-ce que je rate ?' pour contrer la tendance du LLM à être d'accord.

### Choisir des bibliothèques ennuyeuses et stables

Choisissez délibérément des bibliothèques bien établies avec une bonne stabilité qui existaient avant les dates de coupure d'entraînement IA pour une meilleure génération de code IA.

> "I gain enough value from LLMs that I now deliberately consider this when picking a library—I try to stick with libraries with good stability and that are popular enough that many examples of them will have made it into the training data. I like applying the principles of boring technology—innovate on your project's unique selling points, stick with tried and tested solutions for everything else."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20gain%20enough%20value%20from%20LLMs)

> "Je tire suffisamment de valeur des LLMs que je considère maintenant délibérément cela lors du choix d'une bibliothèque—j'essaie de m'en tenir à des bibliothèques avec une bonne stabilité et qui sont assez populaires pour que de nombreux exemples d'elles aient été inclus dans les données d'entraînement. J'aime appliquer les principes de la technologie ennuyeuse—innovez sur les arguments de vente uniques de votre projet, restez avec des solutions éprouvées pour tout le reste."
> — Traduit par Claude

### Demander de planifier d'abord

Dites à l'assistant de décrire les étapes, les risques et les tests rapides avant de toucher au code pour que vous puissiez revoir et ajuster l'approche.

> "If you want to iterate on the plan, it helps to explicitly include instructions in the prompt to not proceed with implementation until the plan has been accepted by the user."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20you%20want%20to%20iterate%20on%20the%20plan)

> "Si vous voulez itérer sur le plan, il est utile d'inclure explicitement des instructions dans le prompt pour ne pas procéder à l'implémentation jusqu'à ce que le plan ait été accepté par l'utilisateur."
> — Traduit par Claude

**Implémentations d'outils :**

<details>
<summary><strong>Claude Code</strong></summary>

Appuyez sur `Shift+Tab` pour passer en Mode Plan afin qu'il ne fasse que lire et rédiger. Utilisez le prompt de planification partagé, itérez jusqu'à ce que ça paraisse correct, puis quittez le Mode Plan quand vous donnez le feu vert à l'implémentation.

</details>

<details>
<summary><strong>Cursor</strong></summary>

Cliquez sur le bouton Plan dans Cursor pour qu'il reste en lecture seule pendant que vous itérez. Faites-lui lister les étapes, fichiers impactés, risques et tests rapides, puis quittez le Mode Plan pour ouvrir le diff une fois que vous donnez le feu vert à l'implémentation.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Rappelez à Codex de garder la planification séparée de l'implémentation : listez les étapes, risques et tests rapides, mettez en pause pour votre révision, puis laissez-le implémenter et inspecter le diff une fois approuvé.

</details>

### Planifier avec le mode haute capacité

Lors de la collecte d'exigences ou de la rédaction de spécifications, basculez temporairement vers un modèle de plus haute capacité ou un mode de raisonnement étendu pour qu'il puisse lire, synthétiser et proposer un plan avant de coder.

**Implémentations d'outils :**

<details>
<summary><strong>Claude Code</strong></summary>

Exécutez `/model` et choisissez `opus` (ou un autre niveau supérieur) lors de la définition des exigences pour qu'il puisse raisonner profondément, puis utilisez le Mode Plan si vous voulez qu'il reste en lecture seule jusqu'à ce que vous approuviez les modifications.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Utilisez `/model gpt-5-high` (ou un autre niveau de raisonnement étendu) pour que l'assistant digère le contexte et rédige la spécification, puis redescendez une fois que le plan est verrouillé.

</details>

### Développement piloté par les spécifications : Itérer jusqu'à ce que ça fonctionne

Itérez sur les spécifications en Markdown jusqu'à ce que l'assistant génère du code fonctionnel - en traitant les spécifications comme la source de vérité plutôt que d'écrire directement du code.

> "The workflow involves iterating on specifications in Markdown files, asking AI to compile into code, running/testing the app, and updating the spec if something doesn't work as expected. Developers should treat specifications as living documents, constantly updating and refining them to guide AI code generation with increasing precision."
> — [GitHub Engineering](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-using-markdown-as-a-programming-language-when-building-with-ai/)

> "Le flux de travail implique d'itérer sur les spécifications dans des fichiers Markdown, de demander à l'IA de compiler en code, d'exécuter/tester l'application et de mettre à jour la spécification si quelque chose ne fonctionne pas comme prévu. Les développeurs doivent traiter les spécifications comme des documents vivants, les mettant constamment à jour et les affinant pour guider la génération de code IA avec une précision croissante."
> — Traduit par Claude

> "**Counter-argument:** If, given the prompt, AI does the job perfectly on first or second iteration — fine. Otherwise, stop refining the prompt. Go write some code, then get back to the AI. You'll get much better results."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=If%2C%20given%20the%20prompt%2C%20AI%20does%20the%20job%20perfectly%20on%20first%20or%20second%20iteration%20%E2%80%94%20fine.%20Otherwise%2C%20stop%20refining%20the%20prompt)

> "**Contre-argument:** Si, étant donné l'invite, l'IA fait le travail parfaitement à la première ou deuxième itération — très bien. Sinon, arrêtez d'affiner l'invite. Allez écrire du code, puis revenez à l'IA. Vous obtiendrez de bien meilleurs résultats."
> — Traduit par Claude

## UI & Prototypage

### Codage par vibration

Construisez des projets à travers la conversation plutôt que le codage traditionnel - parlez, acceptez les changements, et itérez jusqu'à ce que ça marche.

> "...I ask for the dumbest things like `decrease the padding on the sidebar by half` because I'm too lazy to find it. I `Accept All` always, I don't read the diffs anymore. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows beyond my usual comprehension, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding—I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works."
> — [Andrej Karpathy](https://x.com/karpathy/status/1886192184808149383)

> "...je demande les choses les plus bêtes comme `diminue le padding de la barre latérale de moitié` parce que je suis trop paresseux pour le trouver. Je `Accepte Tout` toujours, je ne lis plus les diffs. Quand j'obtiens des messages d'erreur, je les copie-colle simplement sans commentaire, généralement ça les corrige. Le code grandit au-delà de ma compréhension habituelle, je devrais vraiment le lire pendant un moment. Parfois les LLMs ne peuvent pas corriger un bug alors je le contourne simplement ou demande des changements aléatoires jusqu'à ce qu'il disparaisse. Ce n'est pas si mal pour des projets de week-end jetables, mais c'est encore assez amusant. Je construis un projet ou une webapp, mais ce n'est pas vraiment du codage—je vois juste des trucs, dis des trucs, exécute des trucs, et copie-colle des trucs, et ça marche en grande partie."
> — Traduit par Claude

### Construire un prototype d'abord

Commencez chaque projet par un prototype généré rapidement pour prouver qu'il peut fonctionner.

> "The best way to start any project is with a prototype that proves that the key requirements of that project can be met. I often find that an LLM can get me to that working prototype within a few minutes of me sitting down with my laptop—or sometimes even while working on my phone."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20best%20way%20to%20start%20any%20project)

> "La meilleure façon de commencer tout projet est avec un prototype qui prouve que les exigences clés de ce projet peuvent être satisfaites. Je trouve souvent qu'un LLM peut m'amener à ce prototype fonctionnel en quelques minutes après m'être assis avec mon ordinateur portable—ou parfois même en travaillant sur mon téléphone."
> — Traduit par Claude

### Montrer des captures d'écran

Incluez des captures d'écran et itérez - prenez une capture d'écran du résultat, comparez, répétez.

> "Give Claude a visual mock by copying / pasting or drag-dropping an image... take screenshots of the result, and iterate until its result matches the mock."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Give%20Claude%20a%20visual%20mock)

> "Donnez à Claude une maquette visuelle en copiant/collant ou en glissant-déposant une image... prenez des captures d'écran du résultat, et itérez jusqu'à ce que son résultat corresponde à la maquette."
> — Traduit par Claude

> "I opened a second copy of Sketch and pasted in a screenshot... `this is ugly, please make it less ugly.`"
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=I%20opened%20a%20second%20copy%20of%20Sketch)

> "J'ai ouvert une deuxième copie de Sketch et collé une capture d'écran... `c'est laid, s'il vous plaît rendez-le moins laid.`"
> — Traduit par Claude

### Rendre plus beau

Demandez simplement de rendre l'UI `plus belle` ou `plus élégante` - ça marche.

> "If Claude doesn't produce a well-designed UI the first time, you can just tell it to `make it more beautiful/elegant/usable`."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20Claude%20doesn't%20produce)

> "Si Claude ne produit pas une UI bien conçue la première fois, vous pouvez simplement lui dire de la `rendre plus belle/élégante/utilisable`."
> — Traduit par Claude

### Demander des wireframes ASCII

Lors du raffinement des mises en page, faites que l'assistant dessine des wireframes ASCII pour que vous puissiez évaluer la hiérarchie et l'espacement avant de toucher au CSS.

> "If Claude doesn't produce a well-designed UI the first time, you can just tell it to `make it more beautiful/elegant/usable`."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20Claude%20doesn't%20produce)

> "Si Claude ne produit pas une UI bien conçue la première fois, vous pouvez simplement lui dire de la `rendre plus belle/élégante/utilisable`."
> — Traduit par Claude

## Codage

### Confirmer la compréhension avant de coder

Demandez explicitement à l'outil de confirmer sa compréhension de la tâche avant de commencer l'implémentation pour assurer l'alignement et réduire les attentes incompatibles.

### Gérer les parties critiques, déléguer le reste

Écrivez vous-même les parties critiques et complexes du code et déléguez l'implémentation directe restante à l'assistant.

> "Write the critical parts and ask AI to do the rest."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20the%20critical%20parts%20and%20ask%20AI%20to%20do%20the%20rest)

> "Écrivez les parties critiques et demandez à l'IA de faire le reste."
> — Traduit par Claude

### Générer du code, pas des dépendances

Écrivez du code personnalisé plutôt que d'inclure plus de bibliothèques lorsque vous travaillez avec des assistants.

> "Be even more conservative about upgrades than before... I strongly prefer more code generation over using more dependencies."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Be%20even%20more%20conservative%20about)

> "Soyez encore plus conservateur concernant les mises à jour qu'avant... Je préfère fortement plus de génération de code plutôt que d'utiliser plus de dépendances."
> — Traduit par Claude

### Amorcer avec du code existant

Commencez par verser du code existant dans le chat pour ensemencer le contexte, puis modifiez à partir de là.

> "I often start a new chat by dumping in existing code to seed that context, then work with the LLM to modify it in some way."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20often%20start%20a%20new%20chat)

> "Je commence souvent une nouvelle conversation en déversant du code existant pour ensemencer ce contexte, puis je travaille avec le LLM pour le modifier d'une manière ou d'une autre."
> — Traduit par Claude

### Définir la structure, déléguer l'implémentation

Fournissez la structure - signatures de fonction, esquissess de code ou échafaudage - et laissez l'assistant remplir les détails d'implémentation.

> "I find LLMs respond extremely well to function signatures like the one I use here. I get to act as the function designer, the LLM does the work of building the body to my specification. I'll often follow-up with `Now write me the tests using pytest`. Again, I dictate my technology of choice—I want the LLM to save me the time of having to type out the code that's sitting in my head already."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20find%20LLMs%20respond%20extremely%20well)

> "Je trouve que les LLMs répondent extrêmement bien aux signatures de fonction comme celle que j'utilise ici. Je peux agir comme le concepteur de la fonction, le LLM fait le travail de construire le corps selon ma spécification. Je suis souvent suivi par `Maintenant écris-moi les tests en utilisant pytest`. Encore une fois, je dicte mon choix de technologie—je veux que le LLM me fasse gagner le temps d'avoir à taper le code qui est déjà dans ma tête."
> — Traduit par Claude

> "Write an outline of the code and ask AI to fill the missing parts."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20an%20outline%20of%20the%20code%20and%20ask%20AI%20to%20fill%20the%20missing%20parts)

> "Écrivez une esquisse du code et demandez à l'IA de remplir les parties manquantes."
> — Traduit par Claude

### Déléguer les tâches fastidieuses

Déléguez les tâches ennuyeuses, systématiques et chronophages à l'IA - des petits renommages de variables aux grandes migrations qui ne nécessitent pas de réflexion architecturale profonde.

> "I'm using LLMs, but for dumber things: `rename all occurrences of this parameter`"
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20using%20LLMs,%20but%20for%20dumber%20things)

> "J'utilise les LLMs, mais pour des choses plus bêtes : `renomme toutes les occurrences de ce paramètre`"
> — Traduit par Claude

> "AI's best use case for me remains writing one-off scripts"
> — [Colton](https://colton.dev/blog/curing-your-ai-10x-engineer-imposter-syndrome/#:~:text=AI's%20best%20use%20case%20for%20me)

> "Le meilleur cas d'usage de l'IA pour moi reste l'écriture de scripts ponctuels"
> — Traduit par Claude

> "I give the agent tasks that I could do without thinking too much but that would take me a lot of time—very systematic tasks that a junior developer could do with the right explanations."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=I%20give%20the%20agent%20tasks%20that%20I%20could%20do%20without%20thinking%20too%20much%20but%20that%20would%20take%20me%20a%20lot%20of%20time%E2%80%94very%20systematic%20tasks%20that%20a%20junior%20developer%20could%20do%20with%20the%20right%20explanations.)

> "Je donne à l'agent des tâches que je pourrais faire sans trop réfléchir mais qui me prendraient beaucoup de temps—des tâches très systématiques qu'un développeur junior pourrait faire avec les bonnes explications."
> — Traduit par Claude

> "The best example I've found for the agent was migrating a huge app from one UI library to another. It's not hard work, but it takes a huge amount of time and is completely uninteresting."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=The%20best%20example%20I%27ve%20found%20for%20the%20agent%20was%20migrating%20a%20huge%20app%20from%20one%20UI%20library%20to%20another.%20It%27s%20not%20hard%20work%2C%20but%20it%20takes%20a%20huge%20amount%20of%20time%20and%20is%20completely%20uninteresting.)

> "Le meilleur exemple que j'ai trouvé pour l'agent était de migrer une énorme application d'une bibliothèque UI à une autre. Ce n'est pas un travail difficile, mais ça prend énormément de temps et c'est complètement inintéressant."
> — Traduit par Claude

### Traiter l'IA comme un stagiaire numérique

Donnez à l'IA des instructions extrêmement précises et détaillées comme vous le feriez avec un stagiaire - fournissez des signatures de fonction exactes et laissez-le gérer l'implémentation.

> "Once I've completed the initial research I change modes dramatically. For production code my LLM usage is much more authoritarian: I treat it like a digital intern, hired to type code for me based on my detailed instructions."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20treat%20it%20like%20a%20digital%20intern)

> "Une fois que j'ai terminé la recherche initiale, je change de mode de manière dramatique. Pour le code de production, mon usage de LLM est beaucoup plus autoritaire : je le traite comme un stagiaire numérique, embauché pour taper du code pour moi basé sur mes instructions détaillées."
> — Traduit par Claude

> "But instead of fixing the code myself, I explained why it was wrong and gave it more precise instructions. When I told it `You misunderstood, it should…` and provided clearer guidance, I was impressed at how it could understand the problem and update the code accordingly."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=But%20instead%20of%20fixing%20the%20code%20myself%2C%20I%20explained%20why%20it%20was%20wrong%20and%20gave%20it%20more%20precise%20instructions.%20When%20I%20told%20it%20%E2%80%9CYou%20misunderstood%2C%20it%20should%E2%80%A6%E2%80%9D%20and%20provided%20clearer%20guidance%2C%20I%20was%20impressed%20at%20how%20it%20could%20understand%20the%20problem%20and%20update%20the%20code%20accordingly.)

> "Mais au lieu de corriger le code moi-même, j'ai expliqué pourquoi c'était faux et lui ai donné des instructions plus précises. Quand je lui ai dit `Tu as mal compris, ça devrait...` et fourni des conseils plus clairs, j'ai été impressionné par sa capacité à comprendre le problème et mettre à jour le code en conséquence."
> — Traduit par Claude

### Garder le code très simple

Écrivez du code direct avec des noms de fonction clairs, évitez l'héritage et les astuces cleveres - le code simple fonctionne mieux avec l'IA.

> "Simple code significantly outperforms complex code in agentic contexts. I just recently wrote about ugly code and I think in the context of agents this is worth re-reading. Have the agent do `the dumbest possible thing that will work`."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Simple%20code%20significantly%20outperforms%20complex%20code)

> "Le code simple surpasse significativement le code complexe dans les contextes agentiques. Je viens d'écrire récemment sur le code laid et je pense que dans le contexte des agents, cela vaut la peine d'être relu. Faites faire à l'agent `la chose la plus bête possible qui fonctionnera`."
> — Traduit par Claude

## Débogage

### Laissez-le se tester et se corriger

Configurez des outils pour faire des changements, exécuter des tests, voir ce qui échoue, et réessayer par eux-mêmes.

> "Claude is most useful when it's capable of independently driving feedback loops that allow it to make a change, test the change, and gather context on what failed to try another iteration."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=Claude%20is%20most%20useful%20when)

> "Claude est plus utile quand il est capable de conduire indépendamment des boucles de rétroaction qui lui permettent de faire un changement, tester le changement, et recueillir le contexte sur ce qui a échoué pour essayer une autre itération."
> — Traduit par Claude

### Utiliser des sous-agents pour vérifier

Générez des sous-agents pour vérifier les détails ou enquêter sur des questions spécifiques.

> "Telling Claude to use subagents to verify details or investigate particular questions it might have, especially early on in a conversation or task, tends to preserve context availability without much downside in terms of lost efficiency."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Telling%20Claude%20to%20use%20subagents)

> "Dire à Claude d'utiliser des sous-agents pour vérifier les détails ou enquêter sur des questions particulières qu'il pourrait avoir, surtout tôt dans une conversation ou tâche, tend à préserver la disponibilité du contexte sans beaucoup d'inconvénient en termes d'efficacité perdue."
> — Traduit par Claude

### Tout enregistrer pour le débogage IA

Concevez des systèmes avec une journalisation complète pour que les agents IA puissent lire les logs pour comprendre ce qui se passe et auto-diagnostiquer les problèmes.

> "In general logging is super important. For instance my app currently has a sign in and register flow that sends an email to the user. In debug mode (which the agent runs in), the email is just logged to stdout. This is crucial! It allows the agent to complete a full sign-in with a remote controlled browser without extra assistance. It knows that emails are being logged thanks to a CLAUDE.md instruction and it automatically consults the log for the necessary link to click."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=In%20general%20logging%20is%20super%20important)

> "En général, la journalisation est super importante. Par exemple, mon application a actuellement un flux de connexion et d'inscription qui envoie un email à l'utilisateur. En mode debug (dans lequel l'agent fonctionne), l'email est simplement enregistré dans stdout. C'est crucial ! Cela permet à l'agent de compléter une connexion complète avec un navigateur contrôlé à distance sans assistance supplémentaire. Il sait que les emails sont enregistrés grâce à une instruction CLAUDE.md et il consulte automatiquement le log pour le lien nécessaire à cliquer."
> — Traduit par Claude

## Tests et QA

### Toujours tester le code soi-même

Vous ne pouvez absolument pas externaliser les tests - vérifiez toujours que le code fonctionne réellement.

> "La seule chose que vous ne pouvez absolument pas externaliser à la machine, c'est tester que le code fonctionne réellement. Votre responsabilité en tant que développeur logiciel est de livrer des systèmes qui fonctionnent. Si vous ne l'avez pas vu tourner, ce n'est pas un système qui fonctionne. Vous devez investir dans le renforcement de ces habitudes de QA manuelle."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20one%20thing%20you%20absolutely%20cannot%20outsource)

### Écrire les tests d'abord, puis le code

Faites écrire à l'IA des tests complets basés sur le comportement attendu, puis itérez sur l'implémentation jusqu'à ce que tous les tests passent.

> "Ask Claude to write tests based on expected input/output pairs. Be explicit about the fact that you're doing test-driven development so that it avoids creating mock implementations, even for functionality that doesn't exist yet in the codebase. Tell Claude to run the tests and confirm they fail. Ask Claude to commit the tests when you're satisfied with them. Ask Claude to write code that passes the tests, instructing it not to modify the tests."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Ask%20Claude%20to%20write%20tests)

> "Demandez à Claude d'écrire des tests basés sur des paires entrée/sortie attendues. Soyez explicite sur le fait que vous faites du développement piloté par les tests pour qu'il évite de créer des implémentations fictives, même pour des fonctionnalités qui n'existent pas encore dans la base de code. Dites à Claude d'exécuter les tests et de confirmer qu'ils échouent. Demandez à Claude de valider les tests quand vous êtes satisfait d'eux. Demandez à Claude d'écrire du code qui passe les tests, en lui demandant de ne pas modifier les tests."
> — Traduit par Claude

### Traiter le code de l'IA comme une Pull Request

Examinez le code généré par l'IA comme s'il s'agissait d'une pull request d'un collègue, en fournissant des commentaires itératifs que l'assistant doit traiter plutôt que de l'éditer directement vous-même.

> "treating the generated code as a Merge Request on which you submit comment for correction"
> — [HN Discussion](https://news.ycombinator.com/item?id=45415232)

> "traiter le code généré comme une Merge Request sur laquelle vous soumettez des commentaires pour correction"
> — Traduit par Claude

## Techniques transversales

### Choisir les outils par style conversationnel

Sélectionnez les assistants de codage selon que vous préférez une collaboration humaine ou une efficacité structurée comme un robot - la personnalité conversationnelle affecte significativement la productivité et le plaisir.

> "En termes de personnalité, c'est l'opposé pour moi : Claude Code me fait l'effet d'un partenaire de programmation en binôme, tandis que Codex me fait l'effet d'un robot (très structuré mais pas très humain dans son style conversationnel). Le problème c'est qu'au bout d'un moment, le 'Vous avez absolument raison !' commence à m'énerver. Codex est sec. Vous pouvez l'insulter et il ne répond même pas. Aucune personnalité. Claude est comme un ami qui admet avoir merdé... Codex est monotone et va droit au but, mais le plus important c'est qu'il n'est pas du tout complaisant. Il vous défiera quand vous suggérez quelque chose de faux et maintiendra son opinion."
> — [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/)

### Choisir le bon modèle pour le travail

Avant de commencer une nouvelle tâche, choisissez deux leviers : le bon modèle (modalité, longueur de contexte, fiabilité des appels d'outils, latence, coût) et le bon niveau de raisonnement (allouer plus/moins de jetons de pensée) — n'utilisez pas aveuglément les valeurs par défaut.

**Implémentations d'outils :**

<details>
<summary><strong>Claude Code</strong></summary>

Deux leviers au début de la tâche : (1) Modèle via `/model` (rapide/bon marché pour les modifications de routine ; long contexte pour multi-fichiers/longs documents ; fort en vision pour UI/captures d'écran). (2) Niveau de raisonnement : activez la pensée étendue lors du débogage complexe, de l'architecture ou de spécifications ambigües pour allouer plus de jetons de raisonnement.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Commencez avec `gpt-5-minimal`/`gpt-5-low` pour les modifications rapides ; choisissez une variante de raisonnement supérieur `gpt-5-high`/`gpt-5-medium` quand la complexité augmente.

</details>

### Exécuter plusieurs agents en parallèle

Arrêtez d'attendre qu'un agent IA termine avant d'en démarrer un autre - exécutez plusieurs agents en parallèle sur des fonctionnalités séparées sans conflits ou confusion.

> "We are exploring solving both of these issues in sketch.dev using containers. By default sketch creates a little development environment in a container with a copy of the source code and the runner has the ability to extract git commits from the container. This lets you run many simultaneously."
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=We%20are%20exploring%20solving%20both%20of%20these%20issues)

> "Nous explorons la résolution de ces deux problèmes dans sketch.dev en utilisant des conteneurs. Par défaut, sketch crée un petit environnement de développement dans un conteneur avec une copie du code source et le runner a la capacité d'extraire des commits git du conteneur. Cela vous permet d'en exécuter plusieurs simultanément."
> — Traduit par Claude

> "I disable all permission checks. Which basically means I run claude --dangerously-skip-permissions. More specifically I have an alias called claude-yolo set up."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=I%20disable%20all%20permission%20checks)

> "Je désactive toutes les vérifications de permissions. Ce qui signifie essentiellement que j'exécute claude --dangerously-skip-permissions. Plus spécifiquement, j'ai un alias appelé claude-yolo configuré."
> — Traduit par Claude

### Apprendre d'eux, coder soi-même

Utilisez les assistants pour apprendre de nouveaux langages et concepts, puis appliquez ces connaissances quand vous codez.

> "I'm leveraging them to learn Go, to upskill myself. And then I apply this new knowledge when I code."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20leveraging%20them%20to%20learn%20Go)

> "Je les exploite pour apprendre Go, pour améliorer mes compétences. Et ensuite j'applique cette nouvelle connaissance quand je code."
> — Traduit par Claude

### Commencer pas cher, escalader quand bloqué

Commencez avec des modèles plus rapides/moins chers pour les tâches routinières, puis escaladez vers des modèles plus puissants seulement quand vous rencontrez des problèmes complexes.

> "Sonnet 4 handles 90% of tasks effectively. Switch to Opus when Sonnet gets stuck. Recommend starting with Sonnet and providing comprehensive context."
> — [Sankalp](https://sankalp.bearblog.dev/my-claude-code-experience-after-2-weeks-of-usage/#:~:text=Sonnet%204%20handles%2090%25)

> "Sonnet 4 gère 90% des tâches efficacement. Basculez vers Opus quand Sonnet est bloqué. Recommande de commencer avec Sonnet et de fournir un contexte complet."
> — Traduit par Claude

### Construire des outils rapides et infaillibles

Créez des outils qui répondent rapidement, fournissent des messages d'erreur clairs, et se protègent contre un usage incorrect par les agents IA.

> "Tools need to be fast. The quicker they respond (and the less useless output they produce) the better. Crashes are tolerable; hangs are problematic. Tools need to be user friendly! Tools must clearly inform agents of misuse or errors to ensure forward progress. Tools need to be protected against an LLM chaos monkey using them completely wrong. There is no such thing as user error or undefined behavior!"
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Tools%20need%20to%20be%20fast)

> "Les outils doivent être rapides. Plus ils répondent rapidement (et moins ils produisent de sortie inutile), mieux c'est. Les crashes sont tolérables ; les blocages sont problématiques. Les outils doivent être conviviaux ! Les outils doivent clairement informer les agents des mauvais usages ou erreurs pour assurer un progrès en avant. Les outils doivent être protégés contre un singe du chaos LLM qui les utilise complètement mal. Il n'y a pas de chose telle qu'une erreur utilisateur ou un comportement indéfini !"
> — Traduit par Claude

### Nettoyer le contexte entre les tâches

Réinitialisez la fenêtre de contexte de l'IA entre les tâches non liées pour prévenir la confusion et améliorer les performances sur les nouveaux problèmes.

> "During long sessions, Claude's context window can fill with irrelevant conversation, file contents, and commands. This can reduce performance and sometimes distract Claude. Use the `/clear` command frequently between tasks to reset the context window."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=During%20long%20sessions%2C%20Claude's%20context)

> "Pendant les longues sessions, la fenêtre de contexte de Claude peut se remplir de conversation non pertinente, de contenus de fichiers, et de commandes. Cela peut réduire les performances et parfois distraire Claude. Utilisez la commande `/clear` fréquemment entre les tâches pour réinitialiser la fenêtre de contexte."
> — Traduit par Claude

### Interrompre et rediriger souvent

Ne laissez pas l'IA aller trop loin sur la mauvaise voie - interrompez, donnez des commentaires, et redirigez dès que vous remarquez des problèmes.

> "Press Escape to interrupt Claude during any phase (thinking, tool calls, file edits), preserving context so you can redirect or expand instructions. Double-tap Escape to jump back in history, edit a previous prompt, and explore a different direction. You can edit the prompt and repeat until you get the result you're looking for."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Press%20Escape%20to%20interrupt%20Claude)

> "Appuyez sur Échap pour interrompre Claude pendant n'importe quelle phase (réflexion, appels d'outils, modifications de fichiers), en préservant le contexte pour que vous puissiez rediriger ou étendre les instructions. Double-tapez Échap pour revenir en arrière dans l'historique, modifier un prompt précédent, et explorer une direction différente. Vous pouvez modifier le prompt et répéter jusqu'à obtenir le résultat que vous cherchez."
> — Traduit par Claude

### Utiliser l'IA comme partenaire de codage

Collaborez comme avec un partenaire de codage - expliquez les problèmes, obtenez des commentaires, et travaillez ensemble sur les solutions.

> "Claude Code feels like pairing with someone with a few years under their belt who just needs the occasional nudge. Then like with pairing, it's review, refactor and test time because it's still your name on the git commit."
> — [Orta Therox](https://blog.puzzmo.com/posts/2025/06/07/orta-on-claude/#:~:text=Claude%20Code%20feels%20like%20pairing)

> "Claude Code ressemble à faire du pair programming avec quelqu'un ayant quelques années d'expérience qui a juste besoin du coup de pouce occasionnel. Puis comme avec le pair programming, c'est le moment de réviser, refactoriser et tester parce que c'est encore votre nom sur le commit git."
> — Traduit par Claude

### Créer des points de rollback pendant le codage

Créez des points de contrôle auxquels vous pouvez revenir lorsque les expériences échouent—capturez des états de travail connus comme bons avant les changements risqués.

**Implémentations d'outils :**

<details>
<summary><strong>Claude Code</strong></summary>

Utilisez `git commit` fréquemment pour créer des points de rollback. Commitez le code qui fonctionne avant les changements risqués pour pouvoir revenir en arrière avec `git reset` ou les changements de branche.

</details>

<details>
<summary><strong>Cursor</strong></summary>

Comptez sur les points de contrôle d'édition IA de Cursor (Cmd/Ctrl+Z) pour un annuler rapide ; créez tout de même des commits git pour des points de rollback durables.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Commitez les états de travail comme points de contrôle avant de laisser Codex appliquer de gros patches ; revenez en arrière avec git si les résultats ne sont pas bons.

</details>