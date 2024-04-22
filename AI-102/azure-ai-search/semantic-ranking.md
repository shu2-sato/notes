
■セマンティック ランキング（元「セマンティック検索」）

2022/10 セマンティック検索 プレビュー。
https://blog.johtani.info/blog/2021/07/01/semantic-search-in-azure-cognitive-search/

「セマンティック ランク付け」、「セマンティック優先度付け」とも。

AI を使用して検索結果を再スコア付けし、セマンティック関連性の高い結果をリストの先頭に移動する機能。

たとえば"what hotel has a good restaurant on site (良い館内レストランがあるホテルはどこか)"というクエリを実行すると、「hotel」「restaurant」「site」などのキーワードが含まれるドキュメントが検索される。

「セマンティック ランク付け」を有効にしてクエリを実行すると、検索結果がランク付けされ、関連度の高い順に並び替えされ、「良い館内レストランがあるホテル」のドキュメントが検索上位に並ぶようになる。

使用するにはBasic以上の価格レベルを選択してAzure AI Searchリソースを作成し、リソースの設定で「セマンティック ランキング」を有効化する必要がある。

セマンティック ランキング有効化の際には「Free」と「Standard」を選べる（これはリソース自体の価格レベルとは別のもの）。

インデックスに「セマンティック構成」という設定が追加されるだけなので、既存のインデックスを作り直す必要がない。