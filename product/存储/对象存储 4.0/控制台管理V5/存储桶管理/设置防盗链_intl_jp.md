## 概要
悪意のあるプログラムがリソースURLを使用してパブリックネットワークトラフィックを盗み、または悪意のある方法を使用してリソースを盗むことで、ユーザーに不要な損失をもたらすことを防ぐために、Tencent Cloud COSはホットリンク保護の構成をサポートし、セキュリティ保護のため、コンソールのホットリンク保護設定によってブラック/ホワイトリストを設定することをお勧めします。

## 設定ステップ
1. [COSコンソール](https://console.cloud.tencent.com/cos5)にログインして、左側のメニューバー【バケットリスト】に入り、ホットリンク保護を設定する必要があるバケット（例えば、examplebucket-1250000000）を選択して、バケットに入ります。
![](https://main.qcloudimg.com/raw/b373ba0eba6a1723236fce8e4a945c64.png)
2. 【基本構成】をクリックし、ホットリンク保護設定を見つけ、【編集】ボタンをクリックして編集可能な状態に入ります。
![ホットリンク保護設定1](https://main.qcloudimg.com/raw/97dceabb6297080878dfd871f143af79.png)
3. 現在の状態を有効に変更し、リストのタイプ（ブラックリストまたはホワイトリスト）を選択し、対応するドメイン名を設定し、設定が完了した後に【保存】をクリックします。
![ホットリンク保護設定2](https://main.qcloudimg.com/raw/619a86e9eb8b1f4fc9741abcebd915d8.png)

>?
>- ユーザーはホットリンク保護の状態を有効に設定した後、対応するドメイン名を入力する必要があります。
>- ブラックリスト：バケットのデフォルトアクセスアドレスにアクセスするリスト内のドメイン名を制限し、リスト内のドメイン名がバケットのデフォルトアクセスアドレスにアクセスする場合は403を返します。
>- ホワイトリスト：バケットのデフォルトアクセスアドレスにアクセスするリスト外のドメイン名を制限し、リスト外のドメイン名がバケットのデフォルトアクセスアドレスにアクセスする場合は403を返します。
>- HTTPリクエストにおいて、headerがブランクのrefererです（すなわち、refererフィールドがないか、refererフィールドがブランクです）
>- 最大10個のドメイン名をサポートし、かつプレフィックスがマッチングするようにドメイン名を設定し、ドメイン名、IPおよびワイルドカード \* などの形式のアドレスをサポートし、1アドレスは1行であり、複数のアドレスは改行してください。
  例（あくまでも例として、実用的な意味はありません）:
  `www.example.com`を構成し、`www.example.com/123`、`www.example.com.cn`などの`www.example.com`をプレフィックスとするアドレスを制限できます；
  ポート付きドメイン名とIPをサポートし、例えば、`www.example.com:8080`、`10.10.10.10:8080`などのアドレスです；
  ` * .example.com`を構成し、`a.b.example.com/123`、`a.example.com`などのアドレスを制限できます。
>- CDNドメイン名を介してアクセスを加速する場合、CDNのホットリンク保護規則を優先的に実行した後、COSのホットリンク保護規則を実行します。


## 例
APPIDは1250000000のユーザーに、名称がexamplebucket-1250000000のバケットを作成し、かつルートディレクトリに画像picture.jpgを配置し、COSは、規則に基づいてデフォルトのアクセスアドレスを生成します：
```shell
examplebucket-1250000000.file.myqcloud.com/picture.jpg
```
ユーザーAはWebサイトを所有します：
```shell
www.example.com
```
この画像をホームページindex.htmlに埋め込みます。

この時、WebマスターBはWebサイトを所有します：
```shell
www.fake.com
```
WebマスターBはこの画像を`www.fake.com`に入れることを望みます。トラフィック料金を負担したくないゆえ、以下のアドレスを介してpicture.jpgを直接引用し、`www.fake.com`のホームページindex.htmlに掲載しました。
```shell
examplebucket-1250000000.file.myqcloud.com/picture.jpg
```

ユーザーAの損失を避けるために、上記状況に対して、ホットリンク保護を有効化する方式を2つ提供します。

#### 有効化方式1

**ブラックリスト**モードを構成し、ドメイン名の設定に`*.fake.com`を入力し、保存して有効にします。

#### 有効化方式2

**ホワイトリスト**モードを構成し、ドメイン名の設定に`*.example.com`を入力し、保存して有効にします。

#### 有効化の前

`http://www.example.com/index.html`にアクセスして画像が正常に表示されます。
`http://www.fake.com/index.html`にアクセスして画像も正常に表示されます。

#### 有効化の後

`http://www.example.com/index.html`にアクセスして画像が正常に表示されます。
`http://www.fake.com/index.html`にアクセスして画像が表示できません。
