**利用可能言語：** [English](README.md) | [Español](README-es.md) | [Deutsch](README-de.md) | [Français](README-fr.md) | [日本語](README-ja.md)

# AIコーディングデモと日常の現実の間にはギャップがある

私はClaude CodeとCodex CLIを6週間毎日使用し、その前にCursorを1年以上使ってきました。良い結果で、確実に以前より速くなっています。しかし、他の人が達成していることを読んで、私は常に疑問に思っていました：何を見逃しているのか？

かなり多くのことが判明しました。

開発者がこれらのツールを実際にどのように使用しているかを調査した後、多くの人が知らない具体的な技術を発見しました。[Indragie KarunaratneはClaudeで完全なmacOSアプリを配布しました](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code) - しかし、どのように？開発者は週単位ではなく時間単位でUIライブラリ全体を移行すると述べています - 正確にどのワークフローを使っているのか？

## あなたがおそらく使っていない技術

適度な向上と変革的な結果を分ける特定のパターンがあります。例：

- **メモリファイル**（CLAUDE.md、.cursorrules）- セッション間でコンテキストを持続させる - 多くの開発者はこれらが存在することを知りません
- **テスト駆動再生成** - 一行ずつデバッグする代わりに、AIにテストに対して反復させる
- **並列AIセッション** - git worktreeやコンテナを使用して複数のエージェントを同時に実行する

これらの技術は、ドキュメント、ブログ投稿、スレッドに散らばっています。それらを見つけるには、何を探すべきかを知る必要があります。

## これは誰のためのものか

すでにAIコーディングツールを使っているが、表面をなぞっているだけなのではないかと疑っている場合 - おそらく正しいです。

このコレクションがそのギャップを埋めます。

📝 **貢献**：[CONTRIBUTING.md](CONTRIBUTING.md)を参照して、あなたの技術と経験を共有してください

## 目次
- [要件と計画](#要件と計画)
- [UIとプロトタイピング](#uiとプロトタイピング)
- [コーディング](#コーディング)
- [デバッグ](#デバッグ)
- [テストとQA](#テストとqa)
- [横断的技術](#横断的技術)

---

## 要件と計画

### 読む、計画する、コーディングする、コミットする

コードを探索させ、計画を立て、実装し、コミットさせます。

> "There's a process that I call 'priming' the agent, where instead of having the agent jump straight to performing a task, I have it read additional context upfront to increase the chances that it will produce good outputs."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=There's%20a%20process%20that%20I%20call)

> "私が「エージェントのプライミング」と呼ぶプロセスがあります。エージェントにタスクに直接飛び込ませる代わりに、良いアウトプットを生成する可能性を高めるために、事前に追加のコンテキストを読ませるのです。"
> — Claudeによる翻訳

### メモリファイルをセットアップする

プロジェクトの構造、標準、および設定についてツールを永続的にガイドするコンテキストファイルを作成します。

> "`CLAUDE.md` is a special file that Claude automatically pulls into context when starting a conversation. This makes it an ideal place for documenting: common bash commands, core files and utility functions, code style guidelines, testing instructions."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=CLAUDE.md%20is%20a%20special%20file)

> "`CLAUDE.md`は、Claudeが会話を開始する際に自動的にコンテキストに取り込む特別なファイルです。これにより、以下の文書化に理想的な場所となります：一般的なbashコマンド、コアファイルとユーティリティ関数、コードスタイルガイドライン、テスト指示。"
> — Claudeによる翻訳

### 詳細な仕様書を書く

包括的な仕様を与える - 会話形式の仕様でも曖昧な指示に勝ります。

> "Here's a recent example: `Write a Python function that uses asyncio httpx with this signature:` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`. Given a URL, this downloads the database to a temp directory and returns a path to it. BUT it checks the content length header at the start of streaming back that data and, if it's more than the limit, raises an error... I find LLMs respond extremely well to function signatures like the one I use here."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=Here's%20a%20recent%20example)

> "最近の例がこちらです：`このシグネチャでasyncio httpxを使用するPython関数を書いてください：` `async def download_db(url, max_size_bytes=5 * 1025 * 1025): -> pathlib.Path`。URLが与えられると、これはデータベースを一時ディレクトリにダウンロードし、そのパスを返します。しかし、そのデータのストリーミング開始時にコンテンツ長ヘッダーをチェックし、制限を超えている場合はエラーを発生させます...私が使用するような関数シグネチャにLLMは非常によく反応することがわかります。"
> — Claudeによる翻訳

### 脳を最初に、AIを二番目に

最初に自分で解決策を下書きし、その後アシスタントを使用して洗練させます。

> "I'm subconsciously defaulting to AI for all things coding. I've been using pen and paper less. As soon as I need to plan a new feature, my first thought is asking o4-mini-high how to do it, instead of my neurons. I hate this. And I'm changing it."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20subconsciously%20defaulting%20to%20AI)

> "私は無意識のうちにコーディングのすべてをAIにデフォルトで頼っています。ペンと紙を使うことが少なくなりました。新しい機能を計画する必要があるとすぐに、私の最初の考えは私のニューロンの代わりにo4-mini-highにそれをする方法を尋ねることです。これを嫌っています。そして私はそれを変えています。"
> — Claudeによる翻訳

### 複数の選択肢を得る

最良の選択肢を選べるように、LLMに長所/短所を含む複数のアプローチを提示させます。

> "I'll use prompts like `what are options for HTTP libraries in Rust? Include usage examples`"
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I'll%20use%20prompts%20like)

> "`RustのHTTPライブラリの選択肢は何ですか？使用例を含めてください`のようなプロンプトを使用します"
> — Claudeによる翻訳

### 退屈で安定したライブラリを選ぶ

より良いAIコード生成のために、AIトレーニングの締切日前に存在した、良い安定性を持つ確立されたライブラリを意図的に選びます。

> "I gain enough value from LLMs that I now deliberately consider this when picking a library—I try to stick with libraries with good stability and that are popular enough that many examples of them will have made it into the training data. I like applying the principles of boring technology—innovate on your project's unique selling points, stick with tried and tested solutions for everything else."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20gain%20enough%20value%20from%20LLMs)

> "私はLLMから十分な価値を得ているため、ライブラリを選ぶときに意図的にこれを考慮するようになりました—良い安定性を持ち、多くの例がトレーニングデータに含まれるほど人気のあるライブラリに固執しようとします。私は退屈な技術の原則を適用することを好みます—あなたのプロジェクトのユニークなセールスポイントで革新し、他のすべてには試行錯誤された解決策に固執する。"
> — Claudeによる翻訳

### まず考えるように求める

コーディング前により慎重な計画を引き出すために`think`または`think hard`を使用します。

> "Claude tends to jump straight into implementation without sufficient background, which generates poor quality results. Another tactic for priming the agent is asking Claude to use its extended thinking mode and make a plan first. The extended thinking is activated by this set of magic keywords: `think` < `think hard` < `think harder` < `ultrathink.` These are not just suggestions to the model—they are specific phrases that activate various levels of extended thinking."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=Claude%20tends%20to%20jump%20straight)

> "Claudeは十分な背景なしに直接実装に飛び込む傾向があり、これは低品質の結果を生成します。エージェントをプライミングするもう一つの戦術は、Claudeに拡張思考モードを使用し、最初に計画を立てるように求めることです。拡張思考は、この魔法のキーワードセットによって活性化されます：`think` < `think hard` < `think harder` < `ultrathink`。これらはモデルへの単なる提案ではありません—これらは様々なレベルの拡張思考を活性化する特定のフレーズです。"
> — Claudeによる翻訳

## UIとプロトタイピング

### バイブコーディング

従来のコーディングではなく会話を通じてプロジェクトを構築します - 話し、変更を受け入れ、動作するまで反復します。

> "...I ask for the dumbest things like `decrease the padding on the sidebar by half` because I'm too lazy to find it. I `Accept All` always, I don't read the diffs anymore. When I get error messages I just copy paste them in with no comment, usually that fixes it. The code grows beyond my usual comprehension, I'd have to really read through it for a while. Sometimes the LLMs can't fix a bug so I just work around it or ask for random changes until it goes away. It's not too bad for throwaway weekend projects, but still quite amusing. I'm building a project or webapp, but it's not really coding—I just see stuff, say stuff, run stuff, and copy paste stuff, and it mostly works."
> — [Andrej Karpathy](https://x.com/karpathy/status/1886192184808149383)

> "...私は`サイドバーのパディングを半分に減らして`のような最も愚かなことを求めます。それを見つけるのが面倒だからです。私は常に`すべて受け入れ`、もうdiffを読みません。エラーメッセージを受け取ったときは、コメントなしでそれをコピペするだけで、通常それで修正されます。コードは私の通常の理解を超えて成長し、しばらく本当にそれを読み通す必要があります。時々LLMはバグを修正できないので、私はそれを回避するか、なくなるまでランダムな変更を求めます。捨てる週末プロジェクトには悪くありませんが、それでもかなり面白いです。私はプロジェクトやウェブアプリを構築していますが、それは本当にコーディングではありません—私はただものを見て、ものを言って、ものを実行して、ものをコピペするだけで、それはほとんど動作します。"
> — Claudeによる翻訳

### まずプロトタイプを構築する

すべてのプロジェクトを、それが動作することを証明する迅速に生成されたプロトタイプで始めます。

> "The best way to start any project is with a prototype that proves that the key requirements of that project can be met. I often find that an LLM can get me to that working prototype within a few minutes of me sitting down with my laptop—or sometimes even while working on my phone."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=The%20best%20way%20to%20start%20any%20project)

> "プロジェクトを始める最良の方法は、そのプロジェクトの主要要件が満たされることを証明するプロトタイプです。私は、ラップトップの前に座って数分以内に、時には携帯電話で作業している間でさえ、LLMが動作するプロトタイプまで導いてくれることをよく発見します。"
> — Claudeによる翻訳

### スクリーンショットを見せる

スクリーンショットを投入して反復する - 結果のスクリーンショットを撮り、比較し、繰り返します。

> "Give Claude a visual mock by copying / pasting or drag-dropping an image... take screenshots of the result, and iterate until its result matches the mock."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Give%20Claude%20a%20visual%20mock)

> "画像をコピー/貼り付けまたはドラッグアンドドロップしてClaudeに視覚的なモックを提供し... 結果のスクリーンショットを撮り、その結果がモックと一致するまで反復します。"
> — Claudeによる翻訳

> "I opened a second copy of Sketch and pasted in a screenshot... `this is ugly, please make it less ugly.`"
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=I%20opened%20a%20second%20copy%20of%20Sketch)

> "私はSketchの2番目のコピーを開いて、スクリーンショットを貼り付けました...`これは醜い、もっと美しくしてください。`"
> — Claudeによる翻訳

### もっと美しくする

UIを`もっと美しく`または`もっとエレガント`にするように頼むだけです - それは機能します。

> "If Claude doesn't produce a well-designed UI the first time, you can just tell it to `make it more beautiful/elegant/usable`."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=If%20Claude%20doesn't%20produce)

> "Claudeが最初に適切に設計されたUIを生成しない場合、単純に`より美しく/エレガント/使いやすくする`ように指示することができます。"
> — Claudeによる翻訳

## コーディング

### 依存関係ではなくコードを生成する

アシスタントと作業する際は、より多くのライブラリを取り込むよりもカスタムコードを記述します。

> "Be even more conservative about upgrades than before... I strongly prefer more code generation over using more dependencies."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Be%20even%20more%20conservative%20about)

> "以前よりもアップグレードについてさらに保守的になってください... 私はより多くの依存関係を使用するよりも、より多くのコード生成を強く好みます。"
> — Claudeによる翻訳

### 既存のコードでプライミングする

チャットに既存のコードをダンプしてコンテキストをシードし、そこから変更を開始します。

> "I often start a new chat by dumping in existing code to seed that context, then work with the LLM to modify it in some way."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20often%20start%20a%20new%20chat)

> "私はしばしば既存のコードを投入してそのコンテキストをシードすることで新しいチャットを始め、その後LLMと連携してそれを何らかの方法で修正します。"
> — Claudeによる翻訳

### 正確な関数シグネチャを与える

欲しい関数シグネチャを正確に与える - 実装の詳細は任せましょう。

> "I find LLMs respond extremely well to function signatures like the one I use here. I get to act as the function designer, the LLM does the work of building the body to my specification. I'll often follow-up with `Now write me the tests using pytest`. Again, I dictate my technology of choice—I want the LLM to save me the time of having to type out the code that's sitting in my head already."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20find%20LLMs%20respond%20extremely%20well)

> "私はLLMがここで使用するような関数シグネチャに非常によく反応することを発見しています。私は関数設計者として振る舞うことができ、LLMは私の仕様に従ってボディを構築する作業を行います。私はしばしば`今度はpytestを使ってテストを書いて`でフォローアップします。再び、私は私の技術選択を指示します—私はLLMが私の頭の中にすでにあるコードをタイプアウトする時間を節約してくれることを望みます。"
> — Claudeによる翻訳

### 退屈なタスクをオフロードする

退屈で体系的で時間のかかるタスクをAIに委譲します - 小さな変数の名前変更から、深いアーキテクチャ思考を必要としない大規模な移行まで。

> "I'm using LLMs, but for dumber things: `rename all occurrences of this parameter`"
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20using%20LLMs,%20but%20for%20dumber%20things)

> "私はLLMを使用していますが、より愚かなことのために：`このパラメータのすべての出現を名前変更する`"
> — Claudeによる翻訳

> "AI's best use case for me remains writing one-off scripts"
> — [Colton](https://colton.dev/blog/curing-your-ai-10x-engineer-imposter-syndrome/#:~:text=AI's%20best%20use%20case%20for%20me)

> "私にとってのAIの最良の使用例は、一回限りのスクリプトを書くことです"
> — Claudeによる翻訳

> "I give the agent tasks that I could do without thinking too much but that would take me a lot of time—very systematic tasks that a junior developer could do with the right explanations."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=I%20give%20the%20agent%20tasks%20that%20I%20could%20do%20without%20thinking%20too%20much%20but%20that%20would%20take%20me%20a%20lot%20of%20time%E2%80%94very%20systematic%20tasks%20that%20a%20junior%20developer%20could%20do%20with%20the%20right%20explanations.)

> "私はエージェントに、あまり考えずにできるが多くの時間がかかるタスクを与えます—適切な説明があればジュニア開発者ができる非常に体系的なタスクです。"
> — Claudeによる翻訳

> "The best example I've found for the agent was migrating a huge app from one UI library to another. It's not hard work, but it takes a huge amount of time and is completely uninteresting."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=The%20best%20example%20I%27ve%20found%20for%20the%20agent%20was%20migrating%20a%20huge%20app%20from%20one%20UI%20library%20to%20another.%20It%27s%20not%20hard%20work%2C%20but%20it%20takes%20a%20huge%20amount%20of%20time%20and%20is%20completely%20uninteresting.)

> "エージェントで見つけた最良の例は、巨大なアプリを一つのUIライブラリから別のものに移行することでした。難しい作業ではありませんが、膨大な時間がかかり、完全に興味深くありません。"
> — Claudeによる翻訳

### AIをデジタルインターンとして扱う

インターンにするように、AIに極めて正確で詳細な指示を与えます - 正確な関数シグネチャを提供し、実装を任せます。

> "Once I've completed the initial research I change modes dramatically. For production code my LLM usage is much more authoritarian: I treat it like a digital intern, hired to type code for me based on my detailed instructions."
> — [Simon Willison](https://simonwillison.net/2025/Mar/11/using-llms-for-code/#:~:text=I%20treat%20it%20like%20a%20digital%20intern)

> "初期調査を完了すると、私は劇的にモードを変更します。本番コードでは、私のLLM使用ははるかに権威主義的です：私の詳細な指示に基づいてコードをタイプするために雇われたデジタルインターンのように扱います。"
> — Claudeによる翻訳

> "But instead of fixing the code myself, I explained why it was wrong and gave it more precise instructions. When I told it `You misunderstood, it should…` and provided clearer guidance, I was impressed at how it could understand the problem and update the code accordingly."
> — [Between the Prompts](https://betweentheprompts.com/design-partner/#:~:text=But%20instead%20of%20fixing%20the%20code%20myself%2C%20I%20explained%20why%20it%20was%20wrong%20and%20gave%20it%20more%20precise%20instructions.%20When%20I%20told%20it%20%E2%80%9CYou%20misunderstood%2C%20it%20should%E2%80%A6%E2%80%9D%20and%20provided%20clearer%20guidance%2C%20I%20was%20impressed%20at%20how%20it%20could%20understand%20the%20problem%20and%20update%20the%20code%20accordingly.)

> "しかし、コードを自分で修正する代わりに、なぜそれが間違っているかを説明し、より正確な指示を与えました。`誤解しています、それは...すべきです`と伝え、より明確なガイダンスを提供したとき、それがどのように問題を理解し、それに応じてコードを更新できるかに感銘を受けました。"
> — Claudeによる翻訳

### コードを超シンプルに保つ

明確な関数名を持つ直接的なコードを書き、継承や巧妙なハックを避けます - シンプルなコードはAIとよりよく動作します。

> "Simple code significantly outperforms complex code in agentic contexts. I just recently wrote about ugly code and I think in the context of agents this is worth re-reading. Have the agent do `the dumbest possible thing that will work`."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Simple%20code%20significantly%20outperforms%20complex%20code)

> "シンプルなコードは、エージェント的コンテキストにおいて複雑なコードを大幅に上回ります。私は最近醜いコードについて書いたばかりで、エージェントのコンテキストでは再読する価値があると思います。エージェントに`動作する最も愚かなことを`させてください。"
> — Claudeによる翻訳

## デバッグ

### 自分でテストして修正させる

変更を加え、テストを実行し、何が失敗したかを確認し、自分で再試行するためのツールをセットアップします。

> "Claude is most useful when it's capable of independently driving feedback loops that allow it to make a change, test the change, and gather context on what failed to try another iteration."
> — [Indragie Karunaratne](https://www.indragie.com/blog/i-shipped-a-macos-app-built-entirely-by-claude-code#:~:text=Claude%20is%20most%20useful%20when)

> "Claudeは、変更を加え、変更をテストし、何が失敗したかのコンテキストを収集して別のイテレーションを試すことを可能にするフィードバックループを独立して駆動できるとき、最も有用です。"
> — Claudeによる翻訳

### サブエージェントを使用して再確認

詳細を確認したり、特定の質問を調査するためにサブエージェントを生成します。

> "Telling Claude to use subagents to verify details or investigate particular questions it might have, especially early on in a conversation or task, tends to preserve context availability without much downside in terms of lost efficiency."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Telling%20Claude%20to%20use%20subagents)

> "Claudeにサブエージェントを使用して詳細を検証したり、特に会話やタスクの早い段階で持つ可能性のある特定の質問を調査するように指示することは、失われた効率の面であまり不利益なしにコンテキストの可用性を保持する傾向があります。"
> — Claudeによる翻訳

### AIデバッグのためにすべてをログする

AIエージェントがログを読んで何が起こっているかを理解し、問題を自己診断できるように、包括的なログ記録を持つシステムを設計します。

> "In general logging is super important. For instance my app currently has a sign in and register flow that sends an email to the user. In debug mode (which the agent runs in), the email is just logged to stdout. This is crucial! It allows the agent to complete a full sign-in with a remote controlled browser without extra assistance. It knows that emails are being logged thanks to a CLAUDE.md instruction and it automatically consults the log for the necessary link to click."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=In%20general%20logging%20is%20super%20important)

> "一般的にログは非常に重要です。例えば、私のアプリは現在、ユーザーにメールを送信するサインインと登録フローを持っています。デバッグモード（エージェントが実行するモード）では、メールは単にstdoutにログされます。これは重要です！これにより、エージェントは追加の支援なしにリモートコントロールされたブラウザーで完全なサインインを完了できます。CLAUDE.mdの指示のおかげでメールがログされていることを知っており、クリックする必要があるリンクのログを自動的に参照します。"
> — Claudeによる翻訳

## テストとQA

### まずテストを書いて、次にコードを書く

期待される動作に基づいてAIに包括的なテストを書かせ、すべてのテストが通るまで実装を反復します。

> "Ask Claude to write tests based on expected input/output pairs. Be explicit about the fact that you're doing test-driven development so that it avoids creating mock implementations, even for functionality that doesn't exist yet in the codebase. Tell Claude to run the tests and confirm they fail. Ask Claude to commit the tests when you're satisfied with them. Ask Claude to write code that passes the tests, instructing it not to modify the tests."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Ask%20Claude%20to%20write%20tests)

> "期待される入力/出力ペアに基づいてClaudeにテストを書くように依頼してください。テスト駆動開発を行っていることを明示して、コードベースにまだ存在しない機能であっても、モック実装の作成を避けるようにしてください。Claudeにテストを実行して失敗することを確認するように伝えてください。満足したらClaudeにテストをコミットするように依頼してください。テストを変更しないよう指示して、テストをパスするコードを書くようにClaudeに依頼してください。"
> — Claudeによる翻訳

## 横断的技術

### 複数のエージェントを並行して実行

一つのAIエージェントが終了するのを待つのをやめましょう - 競合や混乱なしに、個別の機能で複数のエージェントを並行して実行します。

> "We are exploring solving both of these issues in sketch.dev using containers. By default sketch creates a little development environment in a container with a copy of the source code and the runner has the ability to extract git commits from the container. This lets you run many simultaneously."
> — [David Crawshaw](https://crawshaw.io/blog/programming-with-agents#:~:text=We%20are%20exploring%20solving%20both%20of%20these%20issues)

> "私たちはsketch.devでコンテナを使用してこれらの両方の問題を解決することを探求しています。デフォルトでsketchは、ソースコードのコピーを持つコンテナ内に小さな開発環境を作成し、ランナーはコンテナからgitコミットを抽出する能力を持っています。これにより、多くを同時に実行することができます。"
> — Claudeによる翻訳

> "I disable all permission checks. Which basically means I run claude --dangerously-skip-permissions. More specifically I have an alias called claude-yolo set up."
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=I%20disable%20all%20permission%20checks)

> "私はすべての権限チェックを無効にします。これは基本的にclaude --dangerously-skip-permissionsを実行することを意味します。より具体的には、claude-yoloというエイリアスを設定しています。"
> — Claudeによる翻訳

### そこから学び、自分でコーディングする

新しい言語や概念を学ぶためにアシスタントを使用し、コーディング時にその知識を適用します。

> "I'm leveraging them to learn Go, to upskill myself. And then I apply this new knowledge when I code."
> — [Alberto Fortin](https://albertofortin.com/writing/coding-with-ai#:~:text=I'm%20leveraging%20them%20to%20learn%20Go)

> "私はGoを学び、自分をスキルアップするためにそれらを活用しています。そして、コーディングするときにこの新しい知識を適用します。"
> — Claudeによる翻訳

### 安価に始めて、行き詰まったときにエスカレート

ルーチンタスクには高速で/安価なモデルから始めて、複雑な問題に遭遇したときのみより強力なモデルにエスカレートします。

> "Sonnet 4 handles 90% of tasks effectively. Switch to Opus when Sonnet gets stuck. Recommend starting with Sonnet and providing comprehensive context."
> — [Sankalp](https://sankalp.bearblog.dev/my-claude-code-experience-after-2-weeks-of-usage/#:~:text=Sonnet%204%20handles%2090%25)

> "Sonnet 4は90%のタスクを効果的に処理します。Sonnetが行き詰まったときはOpusに切り替えてください。Sonnetから始めて包括的なコンテキストを提供することを推奨します。"
> — Claudeによる翻訳

### 高速で確実なツールを構築

迅速に応答し、明確なエラーメッセージを提供し、AIエージェントによる誤った使用から保護するツールを作成します。

> "Tools need to be fast. The quicker they respond (and the less useless output they produce) the better. Crashes are tolerable; hangs are problematic. Tools need to be user friendly! Tools must clearly inform agents of misuse or errors to ensure forward progress. Tools need to be protected against an LLM chaos monkey using them completely wrong. There is no such thing as user error or undefined behavior!"
> — [Armin Ronacher](https://lucumr.pocoo.org/2025/6/12/agentic-coding/#:~:text=Tools%20need%20to%20be%20fast)

> "ツールは高速である必要があります。より早く応答し（より無用な出力を生成しない）ほど良いです。クラッシュは許容可能ですが、ハングは問題です。ツールはユーザーフレンドリーである必要があります！ツールは前進を確保するために、誤用やエラーをエージェントに明確に通知する必要があります。ツールは、完全に間違って使用するLLMカオスモンキーから保護される必要があります。ユーザーエラーや未定義の動作というものは存在しません！"
> — Claudeによる翻訳

### タスク間でコンテキストをクリア

関連のないタスク間でAIのコンテキストウィンドウをリセットして混乱を防ぎ、新しい問題でのパフォーマンスを向上させます。

> "During long sessions, Claude's context window can fill with irrelevant conversation, file contents, and commands. This can reduce performance and sometimes distract Claude. Use the `/clear` command frequently between tasks to reset the context window."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=During%20long%20sessions%2C%20Claude's%20context)

> "長時間のセッション中に、Claudeのコンテキストウィンドウは無関係な会話、ファイルの内容、コマンドで満たされる可能性があります。これはパフォーマンスを低下させ、時にはClaudeの気を散らすことがあります。タスク間で頻繁に`/clear`コマンドを使用してコンテキストウィンドウをリセットしてください。"
> — Claudeによる翻訳

### 頻繁に中断してリダイレクト

AIが間違った道を進みすぎないようにしましょう - 問題に気づいたらすぐに中断し、フィードバックを提供し、リダイレクトします。

> "Press Escape to interrupt Claude during any phase (thinking, tool calls, file edits), preserving context so you can redirect or expand instructions. Double-tap Escape to jump back in history, edit a previous prompt, and explore a different direction. You can edit the prompt and repeat until you get the result you're looking for."
> — [Anthropic](https://www.anthropic.com/engineering/claude-code-best-practices#:~:text=Press%20Escape%20to%20interrupt%20Claude)

> "任意の段階（思考中、ツール呼び出し、ファイル編集）でEscapeを押してClaudeを中断し、指示をリダイレクトまたは拡張できるようにコンテキストを保持します。Escapeをダブルタップして履歴に戻り、前のプロンプトを編集し、異なる方向を探索します。求めている結果が得られるまでプロンプトを編集して繰り返すことができます。"
> — Claudeによる翻訳

### AIをコーディングパートナーとして使用

コーディングパートナーのように協力する - 問題を説明し、フィードバックを得て、解決策に一緒に取り組みます。

> "Claude Code feels like pairing with someone with a few years under their belt who just needs the occasional nudge. Then like with pairing, it's review, refactor and test time because it's still your name on the git commit."
> — [Orta Therox](https://blog.puzzmo.com/posts/2025/06/07/orta-on-claude/#:~:text=Claude%20Code%20feels%20like%20pairing)

> "Claude Codeは、数年の経験を持ち、時々の促しが必要な人とのペアプログラミングのような感覚です。そして、ペアプログラミングと同様に、レビュー、リファクタリング、テストの時間です。なぜなら、gitコミットにはまだあなたの名前があるからです。"
> — Claudeによる翻訳