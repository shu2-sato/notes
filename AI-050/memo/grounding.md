# グラウンディング（接地）

■LLM、グラウンディング、ファインチューニング、Retrieval-Augmented Generation(RAG)

Retrieval-Augmented Generation(RAG) 検索拡張生成・取得拡張生成 など

https://techcommunity.microsoft.com/t5/fasttrack-for-azure/grounding-llms/ba-p/3843857

グラウンディングの主な動機は、LLMが豊富な知識を持っていてもデータベースではないということです。これらは、一般的な推論およびテキストエンジンとして使用するように設計されています。LLMは、広範な情報のコーパスについて訓練されており、その一部は保持されており、言語、世界、推論、およびテキスト操作について幅広い理解を与えています。ただし、知識の蓄積ではなく、エンジンとして使用する必要があります。

その広範な知識にもかかわらず、LLMには限界があります。彼らは特定の時点(たとえば、最近のGPTモデルの場合は2021年<>月)までしかトレーニングされておらず、継続的に更新されないため、知識は古くなっています。さらに、彼らは公開情報にのみアクセスでき、企業のファイアウォールの背後にあるもの、個人データ、またはユースケース固有の情報に関する知識が不足しています。したがって、LLMの一般的な機能とユースケースに関連する特定の情報を組み合わせる方法が必要です。グラウンディングは、この課題に対するソリューションを提供し、必要なコンテキストとデータを組み込みながら、LLMの力を活用することを可能にします。

検索拡張生成(RAG)は、グラウンディングの主要な手法であり、詳細に説明する唯一の手法です。RAGは、タスクに関連する情報を取得し、プロンプトとともに言語モデルに提供し、応答時にこの特定の情報を使用するモデルに依存するプロセスです。接地と同じ意味で使用されることもありますが、RAGは重複するものの、異なる手法です。これは強力で使いやすい方法であり、多くのユースケースに適用できます。

一方、ファインチューニングは、接地に関しては「名誉ある言及」です。これには、追加のトレーニング手順を調整して、一般的なトレーニングに基づいて構築され、モデルにタスク関連情報を注入する新しいバージョンのモデルを作成することが含まれます。以前は、能力の低いモデルがあったとき、ファインチューニングがより一般的でした。ただし、時間がかかり、費用がかかり、多くのシナリオで大きな利点を提供しないため、関連性が低くなっています。

■シンボルグラウンディング問題

https://ledge.ai/articles/symbol_grounding_problem

シンボルグラウンディング問題とは、記号システム内のシンボルがどのようにして実世界と意味と結びつけられるかという問題。
コンピュータには、記号の「意味」が分かっていないので、記号の操作だけで知能は実現できない。シンボルを、その意味するものと結びつける（グラウンドさせる）ことが必要であり、困難である。

記号（symbol）を現実世界に接地（ground）する（ことができない）問題


■グラウンディング/シンボルグラウンディング問題

https://book.st-hakky.com/docs/llm-grounding/

近年、人工知能の性能が段々向上します。しかし、仮想世界で存在するシステムを現実に繋げるのはまだ困難です。その問題は一般的に「シンボルグラウンディング問題」と呼ばれていますが、LLMの分野では「Grounding」と言います。

すなわち、Groundingはシステムに現実へアクセスを提供することです。様々な分野でGroundingが使用されていますが、本記事では、自然言語処理のLLMのGroundingについて説明します。

■Microsoft 365 Copilotにおけるグラウンディング

https://zenn.dev/microsoft/articles/ad14d45121abe7

https://xtech.nikkei.com/atcl/nxt/column/18/00001/07952/

ユーザーが入力したプロンプトは、直接LLMに送られるのではなく、まずCopilotが受け取る。Copilotはプロンプトを「Microsoft Graph」に蓄えられたデータに基づいて検証し、プロンプトの内容を補完する。Microsoft Graphとは、Microsoft 365の各種アプリが使用するOfficeファイルや電子メール、連絡先、カレンダー、チャットといった様々なデータを分析可能な形で蓄積したデータベースである。

　CopilotはMicrosoft Graphのデータを使って、プロンプトに対して「グラウンディング（根拠付け）」を実行している。プロンプトに様々な業務データを反映させることで、プロンプトそのものの精度が高まるという仕組みだ。

https://memo.tyoshida.me/office-365/the-future-of-work-with-ai-summary/

今見ていただいたものを少し説明させてください。Copilot を構築するために、私たちは ChatGPT を Microsoft 365 に接続しただけではありません。Microsoft 365 Copilot は、高度な処理とオーケストレーションのエンジンである The Copilot System と呼ばれるものによって支えられています。これは、3つの基盤技術の力を利用するものです。Microsoft 365のアプリ（Word、Excel、PowerPoint、Outlook Teamsなど）。

Microsoft Graphは、すべてのコンテンツとコンテキスト、電子メール、ファイル、会議、チャット、カレンダー、大規模言語モデル（LLM）、人間が読めるテキストを解析して生成できるクリエイティブエンジンであり、すべて自然言語を通してアクセスできます。Copilotは、アプリであなたからのプロンプトを受け取ると、グラウンディングと呼ばれるアプローチによって、プロンプトを前処理します。

簡単に言うと、グラウンディングはプロンプトの品質を向上させ、適切で実行可能な回答を得ることができます。グラウンディングの最も重要な部分の1つは、Microsoft Graphを呼び出して、ビジネスコンテンツとコンテキストを取得することです。Copilotは、グラフから得たこのユーザーデータを他の入力と組み合わせて、プロンプトを改善します。そして、修正されたプロンプトをLLMに送信します。