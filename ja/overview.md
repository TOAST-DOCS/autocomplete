<!-- pre-align:aligned sig=6c5319d20a1b -->

<a id="search-autocomplete-overview"></a>
## Search > Autocomplete > 概要 { #search-autocomplete-overview }

- 検索ウィンドウに検索ワードを入力した時、オートコンプリート機能を提供するサービスです。
    - インデックスREST APIを利用して、オートコンプリートに使用するデータを入力します。
    - オートコンプリート REST APIを利用して、オートコンプリート結果を取得します。

<a id="developing-autocomplete-service"></a>
### オートコンプリートサービスの開発プロセス { #developing-autocomplete-service }

サービスの構成図は次のとおりです。

![img](http://static.toastoven.net/prod_autocomplete/block_diagrm-ja-20200304.png)

サービス開発プロセス

1. サービス作成

    - オートコンプリートサービスを作成します。

2. インデックス

    - Autocompleteの入力形式に合わせてJSONデータを作成します。
    - 作成したJSONデータをREST APIを利用して、Autocompleteに入力します。

3. オートコンプリート

    - オートコンプリートREST APIの結果を利用して、フロント画面を構成します。
