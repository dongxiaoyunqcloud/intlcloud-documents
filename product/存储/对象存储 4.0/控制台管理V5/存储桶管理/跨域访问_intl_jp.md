## 概要
COSコンソールを介して、バケット内のオブジェクトに対してクロスオリジンアクセスを設定できます。COSは、OPTIONSリクエストに応答する構成を提供し、複数の規則をサポートします。地域間アクセスは、即ち、HTTPリクエストを介して1つのドメインから別のドメインのリソースをリクエストします。プロトコル、ドメイン名、およびポートのいずれかが異なる限り、異なるドメインとして扱われます。

COSは、クロスオリジンアクセスに対して、OPTIONSリクエストへの応答をサポートし、かつ開発者によって設定される規則に基づいて具体的に設定される規則をブラウザに返します。しかしながら、サーバーは、後で開始されるクロスオリジンアクセスが規則を満たすかどうかを検証しません。詳細については、[HTTPアクセス制御に関する説明](https://developer.mozilla.org/ja/docs/Web/HTTP/CORS)を参照してください。

## 操作手順

1. [COSコンソール](https://console.cloud.tencent.com/cos5)にログインし、左側のメニューバー【バケットリスト】を選択し、バケットリストページに入ります。クロスオリジンアクセスを設定する必要があるバケットをクリックし、バケットに入ります。
![](https://main.qcloudimg.com/raw/b90ad17947a0ec530db87210f4b9027d.png)
2. 【基本構成】をクリックし、バケットの基本構成ページに入り、**CORS設定**を見つけ、【規則の追加】をクリックします。
![](https://main.qcloudimg.com/raw/6f3d6f81cb550bac4076d54861efdc60.png)
3. 規則情報を追加し（*のついた項目は必須項目です）、構成項目の説明は以下のとおりです：

 **ソースOrigin**：クロスオリジンリクエストを許可するソース。
 - 複数のソースを同時に指定できるが、1行に1つしか入力できません。
 - 構成が`*`をサポートし、すべてのドメイン名が許可されることを意味するので、推奨しません。
 - `http://www.abc.com`のような単一の具体的なドメイン名をサポートします。
 - `http://*.abc.com`のようなセカンドレベルワイルドカード形式のドメイン名をサポートしますが、1行に1つの`*`しかありません。
 - プロトコル名httpまたはhttpsを見逃さないように注意してください、ポートがデフォルトの80でない場合、ポートを付ける必要があります。

 **操作Methods**：GET、PUT、POST、DELETE、HEADをサポートします。列挙型は、1つ以上のクロスオリジンリクエスト方法を許可します。

 **Allow-Headers**：OPTIONSリクエストを送信する時に、後続のリクエストにどのカスタマイズされたHTTPリクエストヘッダーを使用できるかをサーバーに指示します（例えば：x-cos-meta-md5）。
 - 複数のHeadersを同時に指定できるが、1行に1つしか入力できません。
 - Headerが見逃しやすいので、特に必要がない場合、`*`に設定することをお勧めします（すべて許可を意味する）。
 - 英大文字［a-z、A-Z］をサポートし、アンダースコア`_`を使用できません。
 - Access-Control-Request-Headersで指定された各Headerは、Allowed-Headerで対応項目が必要です。

 **Expose-Headers**：Expose-HeaderにCOSのよく使用されるHeaderが返され、詳細については、[共通リクエストヘッダー](https://cloud.tencent.com/document/product/436/7728)を参照してください。具体的な構成はアプリケーションのニーズに応じて決定する必要があり、デフォルトでは、Etagが推奨されます。ワイルドカードは使用できず、大文字と小文字が区別されず、複数行がサポートされ、また、1行に1つしか入力できません。

 **タイムアウトMax-Age**：OPTIONSリクエストの結果を取得するための有効期間（秒）を設定します。数値は正の整数である必要がある、例えば：600

 ![](https://main.qcloudimg.com/raw/7ca1c22c33129a0602c2a83573c31fef.png)

4. 設定が完了した後、【提出】をクリックします。この時、クロスオリジンアクセス規則が追加されたことが表示されます。変更する必要がある場合、【変更】ボタンをクリックして設定してください。
![](https://main.qcloudimg.com/raw/e42826a0832f1b4283952a1e7af6c826.png)
