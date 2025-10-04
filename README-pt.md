> **Desenvolvimento Ativo** — Atualizado em 29 de setembro de 2025
> [Ver todas as atualizações →](CHANGELOG.md)
>
> **Nota:** Para a melhor experiência, visite o [site](https://coding-with-ai.dev) onde você pode ver a popularidade de cada técnica com base no engajamento da comunidade e descobrir quais abordagens os desenvolvedores consideram mais valiosas.

# Há uma lacuna entre as demos de codificação com IA e a realidade diária

Tenho usado Claude Code e Codex CLI diariamente por 6 semanas, e Cursor por mais de um ano antes disso. Bons resultados, definitivamente mais rápido que antes. Mas lendo o que outros alcançam, continuei me perguntando: o que estou perdendo?

Acontece que bastante coisa.

## As técnicas que você provavelmente não está usando

Existem padrões específicos que separam ganhos moderados de resultados transformadores. Exemplos:

- **Arquivos de memória** (CLAUDE.md, .cursorrules) que persistem contexto entre sessões - muitos desenvolvedores não sabem que existem
- **Regeneração orientada por testes** - deixe a IA iterar contra testes em vez de depurar linha por linha
- **Sessões paralelas de IA** - execute múltiplos agentes simultaneamente usando git worktrees ou containers

Essas técnicas estão espalhadas em documentação, posts de blog e threads. Encontrá-las requer saber o que procurar.

## Para quem é isso

Se você já está usando ferramentas de codificação com IA, mas suspeita que está apenas arranhando a superfície - você provavelmente está certo.

Esta coleção curada preenche essas lacunas. É um documento vivo, e você é bem-vindo para compartilhar quaisquer técnicas ausentes, bem como sua experiência com as existentes.

🚀 **Site ao vivo**: [coding-with-ai.dev](https://coding-with-ai.dev)

📝 **Contribuindo**: Veja [CONTRIBUTING.md](CONTRIBUTING.md) para compartilhar suas técnicas e experiências

**Idiomas Disponíveis:** [English](README.md) | [Español](README-es.md) | [Deutsch](README-de.md) | [Français](README-fr.md) | [日本語](README-ja.md) | [Português](README-pt.md)

## Índice
- [Requisitos e Planejamento](#requisitos-e-planejamento)
- [UI e Prototipagem](#ui-e-prototipagem)
- [Codificação](#codificação)
- [Depuração](#depuração)
- [Testes e QA](#testes-e-qa)
- [Técnicas Transversais](#técnicas-transversais)

---

## Requisitos e Planejamento

### Configurar Arquivos de Memória

Crie arquivos de contexto que orientem persistentemente as ferramentas sobre a estrutura, padrões e preferências do seu projeto.

> "`CLAUDE.md` is a special file that Claude automatically pulls into context when starting a conversation. This makes it an ideal place for documenting: common bash commands, core files and utility functions, code style guidelines, testing instructions."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=CLAUDE.md%20is%20a%20special%20file)

> "`CLAUDE.md` é um arquivo especial que o Claude automaticamente puxa para o contexto ao iniciar uma conversa. Isso o torna um lugar ideal para documentar: comandos bash comuns, arquivos principais e funções utilitárias, diretrizes de estilo de código, instruções de teste."
> — Traduzido por Claude

**Implementações de Ferramentas:**

<details>
<summary><strong>Claude Code</strong></summary>

Execute `/init` para gerar automaticamente arquivos de contexto inteligentes que fazem o Claude entender sua base de código instantaneamente. O Claude analisa dependências, scripts e arquitetura automaticamente. Use `/memory` para editar e adicionar problemas específicos do projeto.

- Para novos projetos: Execute `/init` na raiz do seu projeto para criar um `CLAUDE.md` inicial
- Para bases de código existentes: Execute `/init` e o Claude analisará a estrutura do seu projeto, dependências e arquivos de configuração para gerar automaticamente informações essenciais
- Use `/memory` para interface completa do editor ou `#` como atalho rápido para adicionar notas
- Hierarquia de arquivos de memória: `~/.claude/CLAUDE.md` (global) e `./CLAUDE.md` (específico do projeto)

</details>

<details>
<summary><strong>Cursor</strong></summary>

Crie `AGENTS.md` para regras do projeto, depois use @codebase e @docs para contexto dinâmico. O superpoder do Cursor é a compreensão em tempo real de toda a sua base de código através de @-mentions.

- Crie `AGENTS.md` na raiz do projeto (também suporta `.cursorrules` legado)
- Contexto em tempo real: Use `@codebase` para trazer arquivos relevantes automaticamente, `@docs` para referenciar documentação, `@git` para entender mudanças recentes
- Combine regras estáticas (AGENTS.md) com contexto dinâmico (@-mentions) para melhores resultados

</details>

### Ler → Planejar → Codificar → Commit

Faça-o explorar o código, depois fazer um plano, implementá-lo e fazer commit.

> "There's a process that I call 'priming' the agent, where instead of having the agent jump straight to performing a task, I have it read additional context upfront to increase the chances that it will produce good outputs."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=There's%20a%20process%20that%20I%20call)

> "Há um processo que eu chamo de 'preparação' do agente, onde em vez de fazer o agente pular direto para realizar uma tarefa, eu o faço ler contexto adicional antecipadamente para aumentar as chances de que ele produza boas saídas."
> — Traduzido por Claude

### Escrever Especificações Detalhadas

Forneça especificações abrangentes - mesmo uma especificação conversacional supera instruções vagas.

> "Here's a recent example: `Write a Python function that uses asyncio httpx with this signature:` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Given a URL, this downloads the database to a temp directory and returns a path to it. BUT it checks the content length header at the start of streaming back that data and, if it's more than the limit, raises an error... I find LLMs respond extremely well to function signatures like the one I use here."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=Here's%20a%20recent%20example)

> "Aqui está um exemplo recente: `Escreva uma função Python que use asyncio httpx com esta assinatura:` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Dada uma URL, isso baixa o banco de dados para um diretório temporário e retorna um caminho para ele. MAS ele verifica o cabeçalho de comprimento de conteúdo no início do streaming de volta desses dados e, se for mais do que o limite, levanta um erro... Eu descubro que LLMs respondem extremamente bem a assinaturas de função como a que eu uso aqui."
> — Traduzido por Claude

### Cérebro Primeiro, Assistente Depois

Esboce a solução você mesmo primeiro, depois use assistentes para refiná-la.

> "I'm subconsciously defaulting to AI for all things coding. I've been using pen and paper less. As soon as I need to plan a new feature, my first thought is asking o4-mini-high how to do it, instead of my neurons. I hate this. And I'm changing it."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20subconsciously%20defaulting%20to%20AI)

> "Estou subconscientemente recorrendo à IA para tudo relacionado a codificação. Tenho usado menos caneta e papel. Assim que preciso planejar um novo recurso, meu primeiro pensamento é perguntar ao o4-mini-high como fazê-lo, em vez de usar meus neurônios. Odeio isso. E estou mudando."
> — Traduzido por Claude

> "Write the initial version yourself and ask AI to review and improve it."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20the%20initial%20version%20yourself%20and%20ask%20AI%20to%20review%20and%20improve%20it)

> "Escreva a versão inicial você mesmo e peça à IA para revisar e melhorá-la."
> — Traduzido por Claude

### Obter Múltiplas Opções

Peça ao LLM para apresentar várias abordagens com prós/contras para que você possa escolher a melhor opção.

> "I'll use prompts like `what are options for HTTP libraries in Rust? Include usage examples`"
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I'll%20use%20prompts%20like)

> "Eu usarei prompts como `quais são as opções para bibliotecas HTTP em Rust? Inclua exemplos de uso`"
> — Traduzido por Claude

### Escolher Bibliotecas Chatas e Estáveis

Escolha deliberadamente bibliotecas bem estabelecidas com boa estabilidade que existiam antes das datas de corte de treinamento da IA para melhor geração de código de IA.

> "I gain enough value from LLMs that I now deliberately consider this when picking a library—I try to stick with libraries with good stability and that are popular enough that many examples of them will have made it into the training data. I like applying the principles of boring technology—innovate on your project's unique selling points, stick with tried and tested solutions for everything else."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20gain%20enough%20value%20from%20LLMs)

> "Ganho valor suficiente dos LLMs que agora considero deliberadamente isso ao escolher uma biblioteca—tento ficar com bibliotecas com boa estabilidade e que sejam populares o suficiente para que muitos exemplos delas tenham entrado nos dados de treinamento. Gosto de aplicar os princípios de tecnologia chata—inovar nos pontos de venda únicos do seu projeto, ficar com soluções testadas e aprovadas para todo o resto."
> — Traduzido por Claude

### Pedir para Planejar Primeiro

Diga ao assistente para delinear passos, riscos e testes rápidos antes de tocar no código para que você possa revisar e ajustar a abordagem.

> "If you want to iterate on the plan, it helps to explicitly include instructions in the prompt to not proceed with implementation until the plan has been accepted by the user."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20you%20want%20to%20iterate%20on%20the%20plan)

> "Se você quiser iterar sobre o plano, ajuda incluir explicitamente instruções no prompt para não prosseguir com a implementação até que o plano tenha sido aceito pelo usuário."
> — Traduzido por Claude

### Planejar com Modelo de Alta Capacidade

Ao coletar requisitos ou rascunhar especificações, mude temporariamente para um modelo de maior capacidade ou modo de razonamento estendido para que ele possa ler, sintetizar e propor um plano antes de codificar.

**Implementações de Ferramentas:**

<details>
<summary><strong>Claude Code</strong></summary>

Execute `/model` e escolha `opus` (ou outro nível superior) ao delimitar requisitos para que ele possa raciocinar profundamente, depois use o Modo Plano se você quiser que ele permaneça somente leitura até que você aprove as edições.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Use `/model gpt-5-high` (ou outro nível de raciocínio estendido) para que o assistente digira o contexto e rascunhe a especificação, depois reduza uma vez que o plano esteja bloqueado.

</details>

### Desenvolvimento Orientado por Especificações: Iterar Até Funcionar

Itere sobre especificações em Markdown até que o assistente gere código funcional - tratando especificações como a fonte de verdade em vez de escrever código diretamente.

> "The workflow involves iterating on specifications in Markdown files, asking AI to compile into code, running/testing the app, and updating the spec if something doesn't work as expected. Developers should treat specifications as living documents, constantly updating and refining them to guide AI code generation with increasing precision."
> — [GitHub Engineering](https://github.blog/ai-and-ml/generative-ai/spec-driven-development-using-markdown-as-a-programming-language-when-building-with-ai/)

> "O fluxo de trabalho envolve iterar sobre especificações em arquivos Markdown, pedir à IA para compilar em código, executar/testar o aplicativo e atualizar a especificação se algo não funcionar como esperado. Os desenvolvedores devem tratar as especificações como documentos vivos, constantemente atualizando e refinando-os para guiar a geração de código de IA com precisão crescente."
> — Traduzido por Claude

> "**Counter-argument:** If, given the prompt, AI does the job perfectly on first or second iteration — fine. Otherwise, stop refining the prompt. Go write some code, then get back to the AI. You'll get much better results."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=If%2C%20given%20the%20prompt%2C%20AI%20does%20the%20job%20perfectly%20on%20first%20or%20second%20iteration%20%E2%80%94%20fine.%20Otherwise%2C%20stop%20refining%20the%20prompt)

> "**Contra-argumento:** Se, dado o prompt, a IA faz o trabalho perfeitamente na primeira ou segunda iteração — ótimo. Caso contrário, pare de refinar o prompt. Vá escrever algum código, depois volte para a IA. Você obterá resultados muito melhores."
> — Traduzido por Claude

## UI e Prototipagem

### Codificação por Vibração

Construa projetos através de conversa em vez de codificação tradicional - fale, aceite mudanças e itere até funcionar.

> "...I ask for the dumbest things like `decrease the padding on the sidebar by half` because I'm too lazy to find it. I `Accept All` always, I don't read the diffs anymore. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows beyond my usual comprehension, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding—I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works."
> — [Andrej Karpathy](https://x.com/karpathy/status/1886192184808149383)

> "...Peço as coisas mais bobas como `diminua o padding da barra lateral pela metade` porque sou muito preguiçoso para encontrá-lo. Sempre dou `Aceitar Tudo`, não leio mais os diffs. Quando recebo mensagens de erro, apenas copio e colo sem comentário, geralmente isso resolve. O código cresce além da minha compreensão usual, eu teria que realmente lê-lo por um tempo. Às vezes os LLMs não conseguem corrigir um bug, então apenas contorno ou peço mudanças aleatórias até que desapareça. Não é tão ruim para projetos descartáveis de fim de semana, mas ainda bem divertido. Estou construindo um projeto ou webapp, mas não é realmente codificação—eu apenas vejo coisas, digo coisas, executo coisas e copio e colo coisas, e geralmente funciona."
> — Traduzido por Claude

### Construir um Protótipo Primeiro

Comece cada projeto com um protótipo gerado rapidamente para provar que pode funcionar.

> "The best way to start any project is with a prototype that proves that the key requirements of that project can be met. I often find that an LLM can get me to that working prototype within a few minutes of me sitting down with my laptop—or sometimes even while working on my phone."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20best%20way%20to%20start%20any%20project)

> "A melhor maneira de começar qualquer projeto é com um protótipo que prove que os requisitos-chave desse projeto podem ser atendidos. Frequentemente descubro que um LLM pode me levar a esse protótipo funcional em poucos minutos de eu sentar com meu laptop—ou às vezes até enquanto trabalho no meu telefone."
> — Traduzido por Claude

### Mostrar Capturas de Tela

Solte capturas de tela e itere - tire uma captura de tela do resultado, compare, repita.

> "Give Claude a visual mock by copying / pasting or drag-dropping an image... take screenshots of the result, and iterate until its result matches the mock."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Give%20Claude%20a%20visual%20mock)

> "Dê ao Claude um mock visual copiando/colando ou arrastando e soltando uma imagem... tire capturas de tela do resultado e itere até que seu resultado corresponda ao mock."
> — Traduzido por Claude

> "I opened a second copy of Sketch and pasted in a screenshot... `this is ugly, please make it less ugly.`"
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=I%20opened%20a%20second%20copy%20of%20Sketch)

> "Abri uma segunda cópia do Sketch e colei uma captura de tela... `isso está feio, por favor torne menos feio.`"
> — Traduzido por Claude

### Tornar Mais Bonito

Apenas peça para tornar a UI `mais bonita` ou `mais elegante` - funciona.

> "If Claude doesn't produce a well-designed UI the first time, you can just tell it to `make it more beautiful/elegant/usable`."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20Claude%20doesn't%20produce)

> "Se o Claude não produzir uma UI bem projetada na primeira vez, você pode simplesmente dizer para `torná-la mais bonita/elegante/usável`."
> — Traduzido por Claude

### Pedir Wireframes ASCII

Ao refinar layouts, faça o assistente esboçar wireframes ASCII para que você possa avaliar hierarquia e espaçamento antes de tocar no CSS.

## Codificação

### Confirmar Entendimento Antes de Codificar

Peça explicitamente à ferramenta para confirmar seu entendimento da tarefa antes de iniciar a implementação para garantir alinhamento e reduzir expectativas incompatíveis.

### Lidar com Partes Críticas, Delegar o Resto

Escreva você mesmo as partes críticas e complexas do código e delegue a implementação direta restante ao assistente.

> "Write the critical parts and ask AI to do the rest."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20the%20critical%20parts%20and%20ask%20AI%20to%20do%20the%20rest)

> "Escreva as partes críticas e peça à IA para fazer o resto."
> — Traduzido por Claude

### Gerar Código, Não Dependências

Escreva código personalizado em vez de incluir mais bibliotecas ao trabalhar com assistentes.

> "Be even more conservative about upgrades than before... I strongly prefer more code generation over using more dependencies."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Be%20even%20more%20conservative%20about)

> "Seja ainda mais conservador sobre atualizações do que antes... Prefiro fortemente mais geração de código em vez de usar mais dependências."
> — Traduzido por Claude

### Preparar com Código Existente

Comece despejando código existente no chat para semear o contexto, depois modifique a partir daí.

> "I often start a new chat by dumping in existing code to seed that context, then work with the LLM to modify it in some way."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20often%20start%20a%20new%20chat)

> "Frequentemente começo um novo chat despejando código existente para semear esse contexto, depois trabalho com o LLM para modificá-lo de alguma forma."
> — Traduzido por Claude

### Definir Estrutura, Delegar Implementação

Forneça a estrutura - assinaturas de função, esboços de código ou andaimes - e deixe o assistente preencher os detalhes de implementação.

> "I find LLMs respond extremely well to function signatures like the one I use here. I get to act as the function designer, the LLM does the work of building the body to my specification. I'll often follow-up with `Now write me the tests using pytest`. Again, I dictate my technology of choice—I want the LLM to save me the time of having to type out the code that's sitting in my head already."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20find%20LLMs%20respond%20extremely%20well)

> "Descubro que LLMs respondem extremamente bem a assinaturas de função como a que uso aqui. Posso atuar como o designer da função, o LLM faz o trabalho de construir o corpo de acordo com minha especificação. Frequentemente faço acompanhamento com `Agora escreva os testes usando pytest`. Novamente, eu dito minha escolha de tecnologia—quero que o LLM me poupe o tempo de ter que digitar o código que já está na minha cabeça."
> — Traduzido por Claude

> "Write an outline of the code and ask AI to fill the missing parts."
> — [Anton Zhiyanov](https://antonz.org/write-code/#:~:text=Write%20an%20outline%20of%20the%20code%20and%20ask%20AI%20to%20fill%20the%20missing%20parts)

> "Escreva um esboço do código e peça à IA para preencher as partes faltantes."
> — Traduzido por Claude

### Transferir Tarefas Tediosas

Delegue tarefas chatas, sistemáticas e demoradas à IA - desde pequenas renomeações de variáveis até grandes migrações que não requerem pensamento arquitetônico profundo.

> "I'm using LLMs, but for dumber things: `rename all occurrences of this parameter`"
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20using%20LLMs,%20but%20for%20dumber%20things)

> "Estou usando LLMs, mas para coisas mais bobas: `renomear todas as ocorrências deste parâmetro`"
> — Traduzido por Claude

> "AI's best use case for me remains writing one-off scripts"
> — [Colton](https://colton.dev/blog/curing-your-ai-10x-engineer-imposter-syndrome/#:~:text=AI's%20best%20use%20case%20for%20me)

> "O melhor caso de uso da IA para mim continua sendo escrever scripts únicos"
> — Traduzido por Claude

> "I give the agent tasks that I could do without thinking too much but that would take me a lot of time—very systematic tasks that a junior developer could do with the right explanations."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=I%20give%20the%20agent%20tasks%20that%20I%20could%20do%20without%20thinking%20too%20much%20but%20that%20would%20take%20me%20a%20lot%20of%20time%E2%80%94very%20systematic%20tasks%20that%20a%20junior%20developer%20could%20do%20with%20the%20right%20explanations.)

> "Dou ao agente tarefas que eu poderia fazer sem pensar muito, mas que me levariam muito tempo—tarefas muito sistemáticas que um desenvolvedor júnior poderia fazer com as explicações certas."
> — Traduzido por Claude

> "The best example I've found for the agent was migrating a huge app from one UI library to another. It's not hard work, but it takes a huge amount of time and is completely uninteresting."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=The%20best%20example%20I%27ve%20found%20for%20the%20agent%20was%20migrating%20a%20huge%20app%20from%20one%20UI%20library%20to%20another.%20It%27s%20not%20hard%20work%2C%20but%20it%20takes%20a%20huge%20amount%20of%20time%20and%20is%20completely%20uninteresting.)

> "O melhor exemplo que encontrei para o agente foi migrar um aplicativo enorme de uma biblioteca de UI para outra. Não é um trabalho difícil, mas leva muito tempo e é completamente desinteressante."
> — Traduzido por Claude

### Fornecer Contexto para Novas Bibliotecas

Ao usar bibliotecas fora dos dados de treinamento da IA, alimente-a com exemplos e documentação recentes para ensiná-la como a biblioteca funciona.

> "LLMs can still help you work with libraries that exist outside their training data, but you need to put in more work—you'll need to feed them recent examples of how those libraries should be used as part of your prompt."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=LLMs%20can%20still%20help%20you%20work%20with%20libraries)

> "LLMs ainda podem ajudá-lo a trabalhar com bibliotecas que existem fora de seus dados de treinamento, mas você precisa fazer mais trabalho—você precisará alimentá-los com exemplos recentes de como essas bibliotecas devem ser usadas como parte do seu prompt."
> — Traduzido por Claude

### Tratar o Assistente como um Estagiário Digital

Dê à IA instruções extremamente precisas e detalhadas como você faria com um estagiário - forneça assinaturas de função exatas e deixe-a lidar com a implementação.

> "Once I've completed the initial research I change modes dramatically. For production code my LLM usage is much more authoritarian: I treat it like a digital intern, hired to type code for me based on my detailed instructions."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20treat%20it%20like%20a%20digital%20intern)

> "Uma vez que completei a pesquisa inicial, mudo de modo drasticamente. Para código de produção, meu uso de LLM é muito mais autoritário: trato-o como um estagiário digital, contratado para digitar código para mim com base em minhas instruções detalhadas."
> — Traduzido por Claude

> "But instead of fixing the code myself, I explained why it was wrong and gave it more precise instructions. When I told it `You misunderstood, it should…` and provided clearer guidance, I was impressed at how it could understand the problem and update the code accordingly."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=But%20instead%20of%20fixing%20the%20code%20myself%2C%20I%20explained%20why%20it%20was%20wrong%20and%20gave%20it%20more%20precise%20instructions.%20When%20I%20told%20it%20%E2%80%9CYou%20misunderstood%2C%20it%20should%E2%80%A6%E2%80%9D%20and%20provided%20clearer%20guidance%2C%20I%20was%20impressed%20at%20how%20it%20could%20understand%20the%20problem%20and%20update%20the%20code%20accordingly.)

> "Mas em vez de corrigir o código eu mesmo, expliquei por que estava errado e dei instruções mais precisas. Quando disse `Você entendeu mal, deveria...` e forneci orientação mais clara, fiquei impressionado com como ele poderia entender o problema e atualizar o código de acordo."
> — Traduzido por Claude

### Manter o Código Extremamente Simples

Escreva código direto com nomes de função claros, evite herança e hacks inteligentes - código simples funciona melhor com IA.

> "Simple code significantly outperforms complex code in agentic contexts. I just recently wrote about ugly code and I think in the context of agents this is worth re-reading. Have the agent do `the dumbest possible thing that will work`."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Simple%20code%20significantly%20outperforms%20complex%20code)

> "Código simples supera significativamente código complexo em contextos agênticos. Recentemente escrevi sobre código feio e acho que no contexto de agentes vale a pena reler. Faça o agente fazer `a coisa mais boba possível que funcionará`."
> — Traduzido por Claude

## Depuração

### Deixar Testar e Corrigir Sozinho

Configure ferramentas para fazer mudanças, executar testes, ver o que falha e tentar novamente por conta própria.

> "Claude is most useful when it's capable of independently driving feedback loops that allow it to make a change, test the change, and gather context on what failed to try another iteration."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=Claude%20is%20most%20useful%20when)

> "O Claude é mais útil quando é capaz de conduzir independentemente loops de feedback que permitem fazer uma mudança, testar a mudança e reunir contexto sobre o que falhou para tentar outra iteração."
> — Traduzido por Claude

### Usar Subagentes para Verificar

Crie subagentes para verificar detalhes ou investigar questões específicas.

> "Telling Claude to use subagents to verify details or investigate particular questions it might have, especially early on in a conversation or task, tends to preserve context availability without much downside in terms of lost efficiency."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Telling%20Claude%20to%20use%20subagents)

> "Dizer ao Claude para usar subagentes para verificar detalhes ou investigar questões particulares que possa ter, especialmente no início de uma conversa ou tarefa, tende a preservar a disponibilidade de contexto sem muita desvantagem em termos de eficiência perdida."
> — Traduzido por Claude

### Registrar Tudo para Depuração do Assistente

Projete sistemas com registro abrangente para que agentes de IA possam ler logs para entender o que está acontecendo e autodiagnosticar problemas.

> "In general logging is super important. For instance my app currently has a sign in and register flow that sends an email to the user. In debug mode (which the agent runs in), the email is just logged to stdout. This is crucial! It allows the agent to complete a full sign-in with a remote controlled browser without extra assistance. It knows that emails are being logged thanks to a CLAUDE.md instruction and it automatically consults the log for the necessary link to click."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=In%20general%20logging%20is%20super%20important)

> "Em geral, o registro é super importante. Por exemplo, meu aplicativo atualmente tem um fluxo de login e registro que envia um e-mail ao usuário. No modo de depuração (no qual o agente executa), o e-mail é apenas registrado em stdout. Isso é crucial! Permite que o agente complete um login completo com um navegador controlado remotamente sem assistência extra. Ele sabe que os e-mails estão sendo registrados graças a uma instrução CLAUDE.md e consulta automaticamente o log para o link necessário para clicar."
> — Traduzido por Claude

## Testes e QA

### Sempre Testar o Código Você Mesmo

Você absolutamente não pode terceirizar testes - sempre verifique se o código realmente funciona.

> "The one thing you absolutely cannot outsource to the machine is testing that the code actually works. Your responsibility as a software developer is to deliver working systems. If you haven't seen it run, it's not a working system. You need to invest in strengthening those manual QA habits."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20one%20thing%20you%20absolutely%20cannot%20outsource)

> "A única coisa que você absolutamente não pode terceirizar para a máquina é testar se o código realmente funciona. Sua responsabilidade como desenvolvedor de software é entregar sistemas funcionando. Se você não o viu executar, não é um sistema funcionando. Você precisa investir em fortalecer esses hábitos manuais de QA."
> — Traduzido por Claude

### Escrever Testes Primeiro, Depois Código

Faça a IA escrever testes abrangentes com base no comportamento esperado, depois itere na implementação até que todos os testes passem.

> "Ask Claude to write tests based on expected input/output pairs. Be explicit about the fact that you're doing test-driven development so that it avoids creating mock implementations, even for functionality that doesn't exist yet in the codebase. Tell Claude to run the tests and confirm they fail. Ask Claude to commit the tests when you're satisfied with them. Ask Claude to write code that passes the tests, instructing it not to modify the tests."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Ask%20Claude%20to%20write%20tests)

> "Peça ao Claude para escrever testes com base em pares de entrada/saída esperados. Seja explícito sobre o fato de que você está fazendo desenvolvimento orientado a testes para que ele evite criar implementações mock, mesmo para funcionalidades que ainda não existem na base de código. Diga ao Claude para executar os testes e confirmar que falham. Peça ao Claude para fazer commit dos testes quando você estiver satisfeito com eles. Peça ao Claude para escrever código que passe nos testes, instruindo-o a não modificar os testes."
> — Traduzido por Claude

### Pedir ao Agente para Revisar Seu Próprio Código

Faça a IA realizar uma revisão de código em seu próprio trabalho antes da revisão humana para identificar problemas e melhorias.

> "Asking the agent to perform a code review on its own work is surprisingly fruitful."
> — [Chris Dzombak](https://www.dzombak.com/blog/2025/08/getting-good-results-from-claude-code/#:~:text=Asking%20the%20agent%20to%20perform%20a%20code%20review)

> "Pedir ao agente para realizar uma revisão de código em seu próprio trabalho é surpreendentemente frutífero."
> — Traduzido por Claude

### Um Escreve, Outro Revisa

Faça um agente escrever código, depois use um agente novo para revisar e encontrar problemas.

> "Use Claude to write code. Run `/clear` or start a second Claude in another terminal. Have the second Claude review the first Claude's work. Start another Claude (or `/clear` again) to read both the code and review feedback. Have this Claude edit the code based on the feedback. This separation often yields better results than having a single Claude handle everything."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Have%20one%20Claude%20write%20code)

> "Use o Claude para escrever código. Execute `/clear` ou inicie um segundo Claude em outro terminal. Faça o segundo Claude revisar o trabalho do primeiro Claude. Inicie outro Claude (ou `/clear` novamente) para ler tanto o código quanto o feedback da revisão. Faça este Claude editar o código com base no feedback. Esta separação frequentemente produz melhores resultados do que ter um único Claude lidando com tudo."
> — Traduzido por Claude

### Editar Código no Diff

Revise mudanças na visualização diff e digite correções diretamente no diff antes de fazer commit.

> "I manually review all AI-written code and test cases. I'll add test cases for anything I think is missing or needs improvement, either manually or by asking the LLM to write those cases (which I then review)."
> — [Chris Dzombak](https://www.dzombak.com/blog/2025/08/getting-good-results-from-claude-code/#:~:text=I%20manually%20review%20all)

> "Reviso manualmente todo o código e casos de teste escritos por IA. Adicionarei casos de teste para qualquer coisa que acho que está faltando ou precisa de melhoria, seja manualmente ou pedindo ao LLM para escrever esses casos (que eu então reviso)."
> — Traduzido por Claude

## Técnicas Transversais

### Escolher Ferramentas por Estilo Conversacional

Escolha assistentes de codificação com base em se você prefere colaboração humanizada ou eficiência estruturada semelhante a um robô - a personalidade conversacional afeta significativamente a produtividade e o prazer.

> "In terms of personality, it's the opposite for me: Claude Code feels like my pair-programming partner, while Codex feels like a robot (very structured but not very human in its conversational style). Problem is after a while, the 'You are absolutely right!' kinda gets on my nerves. Codex is dry. You can insult it and it doesn't even answer. No personality. Claude is like, a friend who admits messing up... Codex is monotone straight to the point, but most importantly the reason why it is better is because it's not agreeable at all. It will challenge you when you're suggesting something wrong and stay with its opinion."
> — [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/)

> "Em termos de personalidade, é o oposto para mim: Claude Code parece meu parceiro de programação em par, enquanto Codex parece um robô (muito estruturado, mas não muito humano em seu estilo conversacional). O problema é que depois de um tempo, o 'Você está absolutamente certo!' meio que me irrita. Codex é seco. Você pode insultá-lo e ele nem responde. Sem personalidade. Claude é como um amigo que admite ter estragado tudo... Codex é monótono direto ao ponto, mas o mais importante, a razão pela qual é melhor é porque não é nada agradável. Ele irá desafiá-lo quando você estiver sugerindo algo errado e permanecerá com sua opinião."
> — Traduzido por Claude

### Escolher o Modelo Certo para o Trabalho

Antes de iniciar uma nova tarefa, escolha duas alavancas: o modelo certo (modalidade, comprimento de contexto, confiabilidade de chamadas de ferramentas, latência, custo) e o nível de raciocínio certo (alocar mais/menos tokens de pensamento) — não padronize cegamente.

**Implementações de Ferramentas:**

<details>
<summary><strong>Claude Code</strong></summary>

Duas alavancas no início da tarefa: (1) Modelo via `/model` (rápido/barato para edições de rotina; contexto longo para múltiplos arquivos/documentos longos; forte em visão para UI/capturas de tela). (2) Nível de raciocínio: habilite pensamento estendido ao enfrentar depuração complexa, arquitetura ou especificações ambíguas para alocar mais tokens de raciocínio.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Comece com `gpt-5-minimal`/`gpt-5-low` para edições rápidas; escolha uma variante de raciocínio superior `gpt-5-high`/`gpt-5-medium` quando a complexidade aumentar.

</details>

### Mudar Estilos de Saída do Assistente

Selecione o estilo de saída do assistente que corresponda ao seu objetivo atual.

### Centralizar Arquivos de Memória

Mantenha um documento de instrução canônico e roteie todos os outros arquivos de agente para ele com uma linha de ponteiro chamativa, um link simbólico ou uma inclusão @file para que a orientação entre ferramentas permaneça consistente.

**Implementações de Ferramentas:**

<details>
<summary><strong>Claude Code</strong></summary>

Mantenha `CLAUDE.md` como a fonte de verdade e use uma dessas três maneiras.

1. Coloque `@CLAUDE.md` em `AGENTS.md`.

2. Crie um link simbólico de `AGENTS.md` para `CLAUDE.md` com `ln -sf CLAUDE.md AGENTS.md` para que ambas as ferramentas compartilhem o mesmo arquivo.

3. Deixe `AGENTS.md` como uma única linha: `LEIA CLAUDE.md PRIMEIRO!!!`.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Use o mesmo trio do lado do Codex.

1. Se o Codex mantém o texto principal, deixe `AGENTS.md` completo e coloque `@AGENTS.md` dentro de `CLAUDE.md` para que ambas as ferramentas cheguem ao mesmo documento.

2. Execute `ln -sf CLAUDE.md AGENTS.md` para que o arquivo que o Codex lê seja apenas um link simbólico para `CLAUDE.md`.

3. Quando `CLAUDE.md` é canônico, mantenha `AGENTS.md` em uma linha: `LEIA CLAUDE.md PRIMEIRO!!!`.

</details>

### Executar Múltiplos Agentes em Paralelo

Pare de esperar que um agente de IA termine antes de iniciar outro - execute múltiplos agentes em paralelo em recursos separados sem conflitos ou confusão.

> "We are exploring solving both of these issues in sketch.dev using containers. By default sketch creates a little development environment in a container with a copy of the source code and the runner has the ability to extract git commits from the container. This lets you run many simultaneously."
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=We%20are%20exploring%20solving%20both%20of%20these%20issues)

> "Estamos explorando resolver ambos esses problemas no sketch.dev usando containers. Por padrão, o sketch cria um pequeno ambiente de desenvolvimento em um container com uma cópia do código-fonte e o executor tem a capacidade de extrair commits git do container. Isso permite que você execute muitos simultaneamente."
> — Traduzido por Claude

> "I disable all permission checks. Which basically means I run claude --dangerously-skip-permissions. More specifically I have an alias called claude-yolo set up."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=I%20disable%20all%20permission%20checks)

> "Desabilito todas as verificações de permissão. O que basicamente significa que executo claude --dangerously-skip-permissions. Mais especificamente, tenho um alias chamado claude-yolo configurado."
> — Traduzido por Claude

**Implementações de Ferramentas:**

<details>
<parameter name="summary"><strong>Claude Code</strong></summary>

Use `--dangerously-skip-permissions` na inicialização ou `/permissions` para alterar a estratégia de permissão durante a sessão de codificação.

- Execute `claude --dangerously-skip-permissions` para habilitar o modo autônomo onde o Claude é executado ininterruptamente sem prompts de permissão
- Mude durante a sessão: Use `/permissions` para gerenciar permissões de ferramenta no meio da sessão sem reiniciar
- Quando usar: Corrigir erros de lint em múltiplos arquivos, refatoração simples e renomeações de variáveis, atualizações de código de rotina e migrações
- Segurança: Melhor usado em containers ou VMs para isolamento, evite em sistemas de produção críticos
- Configurar alias: Muitos usuários criam `alias cc='claude --dangerously-skip-permissions'` para acesso rápido

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Use `--full-auto` na inicialização ou `/mode` para alterar a estratégia de permissão durante a sessão de codificação.

- Habilite o modo totalmente autônomo com `codex --full-auto` ou use o comando `/mode` na sessão
- Mude durante a sessão: Use `/mode` para alternar entre níveis de permissão sem perder o contexto da sessão
- Modos de permissão: `--suggest` (requer aprovação), `--auto-edit` (edita arquivos automaticamente, pergunta por comandos), `--full-auto` (autonomia completa)
- Quando usar full-auto: Tarefas sistemáticas de refatoração, operações em lote de arquivos, correções de lint e limpeza de código

</details>

### Executar Sem Permissões para Tarefas Fáceis

Habilite o modo autônomo quando as tarefas são diretas o suficiente para que você aceitaria todas as mudanças de qualquer forma - pule a supervisão.

> "I disable all permission checks. Which basically means I run `claude --dangerously-skip-permissions`. More specifically I have an alias called claude-yolo set up."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=I%20disable%20all%20permission%20checks)

> "Desabilito todas as verificações de permissão. O que basicamente significa que executo `claude --dangerously-skip-permissions`. Mais especificamente, tenho um alias chamado claude-yolo configurado."
> — Traduzido por Claude

### Aprender Com Ele, Codificar Você Mesmo

Use assistentes para aprender novas linguagens e conceitos, depois aplique esse conhecimento quando você codifica.

> "I'm leveraging them to learn Go, to upskill myself. And then I apply this new knowledge when I code."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20leveraging%20them%20to%20learn%20Go)

> "Estou aproveitando-os para aprender Go, para me aprimorar. E então aplico esse novo conhecimento quando codifico."
> — Traduzido por Claude

### Começar barato e rápido; escalar quando emperrado

Comece com modelos mais rápidos/baratos para tarefas de rotina, depois escale para modelos mais poderosos apenas quando você encontrar problemas complexos.

> "Sonnet 4 handles 90% of tasks effectively. Switch to Opus when Sonnet gets stuck. Recommend starting with Sonnet and providing comprehensive context."
> — [Sankalp](https://sankalp.bearblog.dev/my-claude-code-experience-after-2-weeks-of-usage/#:~:text=Sonnet%204%20handles%2090%25)

> "Sonnet 4 lida efetivamente com 90% das tarefas. Mude para Opus quando o Sonnet emperrar. Recomendo começar com Sonnet e fornecer contexto abrangente."
> — Traduzido por Claude

**Implementações de Ferramentas:**

<details>
<summary><strong>Claude Code</strong></summary>

Use `/model` para alternar entre modelos com base na complexidade da tarefa. Comece com o Sonnet 4 mais barato para trabalho de rotina, escale para o rápido Opus 4.1 quando emperrado.

- Use `/model` para alternar modelos durante sua sessão
- Opção mais barata e rápida: `Claude Sonnet 4`
- Opção de melhor classificação: `Claude Opus 4.1`

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Use `/model` para escalar quando necessário. Comece com `gpt-5-medium` para a maioria das tarefas, mude para `gpt-5-high` quando você encontrar problemas complexos.

- Use `/model` para alternar modelos durante sua sessão
- Opção mais barata e rápida: `gpt-5-medium`
- Opção de melhor classificação: `gpt-5-high`

</details>

### Construir Ferramentas Rápidas e à Prova de Falhas

Crie ferramentas que respondam rapidamente, forneçam mensagens de erro claras e se protejam contra uso incorreto por agentes de IA.

> "Tools need to be fast. The quicker they respond (and the less useless output they produce) the better. Crashes are tolerable; hangs are problematic. Tools need to be user friendly! Tools must clearly inform agents of misuse or errors to ensure forward progress. Tools need to be protected against an LLM chaos monkey using them completely wrong. There is no such thing as user error or undefined behavior!"
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Tools%20need%20to%20be%20fast)

> "As ferramentas precisam ser rápidas. Quanto mais rápido elas respondem (e menos saída inútil produzem), melhor. Travamentos são toleráveis; travamentos são problemáticos. As ferramentas precisam ser fáceis de usar! As ferramentas devem informar claramente aos agentes sobre uso indevido ou erros para garantir progresso. As ferramentas precisam estar protegidas contra um macaco do caos LLM usando-as completamente errado. Não existe erro do usuário ou comportamento indefinido!"
> — Traduzido por Claude

### Uma Sessão Deve Ter Um Objetivo

Use o prompt `O objetivo desta sessão é <objetivo específico>. Informe-me se nos desviarmos do caminho.` no início de cada sessão ou adicione-o ao seu arquivo de memória (AGENTS.md, CLAUDE.md) para prevenir envenenamento de contexto e aumentar a dirigibilidade do agente - aplicando o Princípio da Responsabilidade Única a conversas com IA.

### Usar Sessões de Recursos

Isole cada recurso ou tarefa em sessões separadas para reduzir o inchaço de contexto e melhorar a precisão, assim como branches de recursos no git isolam mudanças de código.

### Limpar Contexto Entre Tarefas

Redefina a janela de contexto da IA entre tarefas não relacionadas para evitar confusão e melhorar o desempenho em novos problemas.

> "During long sessions, Claude's context window can fill with irrelevant conversation, file contents, and commands. This can reduce performance and sometimes distract Claude. Use the `/clear` command frequently between tasks to reset the context window."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=During%20long%20sessions%2C%20Claude's%20context)

> "Durante sessões longas, a janela de contexto do Claude pode se encher de conversa irrelevante, conteúdo de arquivos e comandos. Isso pode reduzir o desempenho e às vezes distrair o Claude. Use o comando `/clear` frequentemente entre tarefas para redefinir a janela de contexto."
> — Traduzido por Claude

**Implementações de Ferramentas:**

<details>
<summary><strong>Claude Code</strong></summary>

Use o comando `/clear` para redefinir a janela de contexto e começar do zero.

- Execute `/clear` entre tarefas não relacionadas para prevenir poluição de contexto
- Preserva seus arquivos de memória (CLAUDE.md) enquanto limpa o histórico de conversas
- Essencial para sessões longas de codificação para manter o desempenho

</details>

<details>
<summary><strong>Cursor</strong></summary>

Inicie novas sessões de chat ou use recursos de gerenciamento de conversas.

- Use Cmd/Ctrl+Shift+L para iniciar um novo chat para tarefas não relacionadas
- O histórico de chat é automaticamente gerenciado para prevenir sobrecarga de contexto
- Use @-mentions para trazer contexto específico ao começar do zero

</details>

### Fazer Perguntas Abertas, Não Orientadas

Evite perguntas 'Estou certo de que...' - em vez disso peça prós/contras, alternativas e 'O que estou perdendo?' para contrabalançar a tendência de concordância do LLM.

### Usar Ênfase Forte em Prompts

Use IMPORTANTE, NUNCA, SEMPRE liberalmente em prompts para direcionar a IA para longe de erros comuns - ainda é a abordagem mais eficaz.

> "Unfortunately CC is no better when it comes to asking the model to not do something. IMPORTANT, VERY IMPORTANT, NEVER and ALWAYS seem to be the best way to steer the model away from landmines. I expect the models to get more steerable in the future and avoid this ugliness. But for now, CC uses this liberally, and so should you."
> — [Vivek (MinusX AI Team)](https://minusx.ai/blog/decoding-claude-code/#:~:text=Unfortunately%20CC%20is%20no%20better)

> "Infelizmente o CC não é melhor quando se trata de pedir ao modelo para não fazer algo. IMPORTANTE, MUITO IMPORTANTE, NUNCA e SEMPRE parecem ser a melhor maneira de direcionar o modelo para longe de armadilhas. Espero que os modelos se tornem mais direcionáveis no futuro e evitem essa feiúra. Mas por agora, o CC usa isso liberalmente, e você também deveria."
> — Traduzido por Claude

### Interromper e Redirecionar Frequentemente

Não deixe a IA ir muito longe no caminho errado - interrompa, forneça feedback e redirecione assim que você notar problemas.

> "Press Escape to interrupt Claude during any phase (thinking, tool calls, file edits), preserving context so you can redirect or expand instructions. Double-tap Escape to jump back in history, edit a previous prompt, and explore a different direction. You can edit the prompt and repeat until you get the result you're looking for."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Press%20Escape%20to%20interrupt%20Claude)

> "Pressione Escape para interromper o Claude durante qualquer fase (pensamento, chamadas de ferramentas, edições de arquivo), preservando o contexto para que você possa redirecionar ou expandir instruções. Toque duplo em Escape para voltar no histórico, editar um prompt anterior e explorar uma direção diferente. Você pode editar o prompt e repetir até obter o resultado que procura."
> — Traduzido por Claude

**Implementações de Ferramentas:**

<details>
<summary><strong>Claude Code</strong></summary>

Use a tecla Escape para interromper e redirecionar o Claude a qualquer momento.

- Pressione `Escape` para interromper o Claude durante qualquer fase (pensamento, chamadas de ferramentas, edições de arquivo)
- Toque duplo em `Escape` para voltar no histórico e editar um prompt anterior
- O contexto é preservado para que você possa redirecionar ou expandir instruções
- Edite o prompt e tente novamente até obter o resultado desejado

</details>

<details>
<summary><strong>Cursor</strong></summary>

Use Cmd/Ctrl+K para parar a geração e fornecer novas instruções.

- Pare a geração com `Cmd/Ctrl+K` quando você vir que está saindo do caminho
- Forneça feedback corretivo imediatamente em vez de esperar
- Use a interface de chat para esclarecer requisitos antes de deixá-lo continuar

</details>

### Criar Pontos de Reversão Ao Codificar

Crie checkpoints para os quais você pode reverter quando experimentos falham—capture estados funcionando conhecidos antes de mudanças arriscadas.

**Implementações de Ferramentas:**

<details>
<summary><strong>Claude Code</strong></summary>

Use `git commit` frequentemente para criar pontos de reversão. Faça commit de código funcionando antes de mudanças arriscadas para que você possa reverter com `git reset` ou trocas de branch.

</details>

<details>
<summary><strong>Cursor</strong></summary>

Confie nos checkpoints de edição de IA do Cursor (Cmd/Ctrl+Z) para desfazer rápido; ainda crie commits git para pontos de reversão duráveis.

</details>

<details>
<summary><strong>Codex CLI</strong></summary>

Faça commit de estados funcionando como checkpoints antes de deixar o Codex aplicar grandes patches; reverta com git se os resultados não forem bons.

</details>

### Usar um Agente como um Parceiro de Codificação

Colabore como com um parceiro de codificação - explique problemas, obtenha feedback e trabalhe juntos em soluções.

> "Claude Code feels like pairing with someone with a few years under their belt who just needs the occasional nudge. Then like with pairing, it's review, refactor and test time because it's still your name on the git commit."
> — [Orta Therox](https://blog.puzzmo.com/posts/2025/06/07/orta-on-claude/#:~:text=Claude%20Code%20feels%20like%20pairing)

> "Claude Code parece emparejar com alguém com alguns anos de experiência que só precisa de um empurrãozinho ocasional. Então, como com o emparelhamento, é hora de revisar, refatorar e testar porque ainda é seu nome no commit git."
> — Traduzido por Claude
