# SentMessage一覧取得
### 概要
送信メッセージ情報の一覧を取得する
### 必要な権限
message または message-read
### 制限事項
* リクエストヘッダのContent-Typeは全てapplication/jsonとして扱う
* リクエストボディはjson形式のみ受け付ける
* レスポンスヘッダのContent-Typeはapplication/jsonのみをサポートし、レスポンスボディはjson形式とする
* $formatクエリオプションにatom または xmlを指定した場合、エラーとはならないが、レスポンスボディのデータの保証はない

<br>
### リクエスト
#### リクエストURL
```
/{CellName}/__ctl/SentMessage
```
#### メソッド
GET
#### リクエストクエリ
以下のクエリパラメタが利用可能です。

|クエリ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|p_cookie_peer<br>|クッキー認証値<br>|認証時にサーバから返却されたクッキー認証値<br>|×<br>|Authorizationヘッダの指定が無い場合のみ有効<br>クッキーの認証情報を利用する場合に指定する<br>|

[$select クエリ](406_Select_Query.html)

[$expand クエリ](405_Expand_Query.html)

[$format クエリ](404_Format_Query.html)

[$filter クエリ](403_Filter_Query.html)

[$inlinecount クエリ](407_Inlinecount_Query.html)

[$orderby クエリ](400_Orderby_Query.html)

[$top クエリ](401_Top_Query.html)

[$skip クエリ](402_Skip_Query.html)

[全文検索(q)クエリ](408_Full_Text_Search_Query.html)

#### リクエストヘッダ
##### 共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|X-HTTP-Method-Override<br>|メソッドオーバーライド機能<br>|任意<br>|×<br>|POSTメソッドでリクエスト時にこの値を指定すると、指定した値がメソッドとして使用される<br>|
|X-Override<br>|ヘッダオーバライド機能<br>|${上書きするヘッダ名}:${値}<br>|×<br>|通常のHTTPヘッダの値を上書きします。複数のヘッダを上書きする場合はX-Overrideヘッダを複数指定する<br>|
|X-Personium-RequestKey<br>|イベントログに出力するRequestKeyフィールドの値<br>|半角英数、-(半角ハイフン)と_(半角アンダーバー)<br>最大128文字<br>|×<br>|指定がない場合、PCS-${UNIX時間}を設定する<br>V1.1.7以降で対応<br>|
##### OData共通リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Authorization<br>|OAuth2.0形式で、認証情報を指定する<br>|Bearer {UnitUserToken}<br>|×<br>|※認証トークンは認証トークン取得APIで取得したトークン<br>|
##### OData一覧取得リクエストヘッダ
|ヘッダ名<br>|概要<br>|有効値<br>|必須<br>|備考<br>|
|:--|:--|:--|:--|:--|
|Accept<br>|レスポンスボディの形式を指定する<br>|application / json<br>|×<br>|省略時は[application/json]として扱う<br>|
#### リクエストボディ
なし
#### リクエストサンプル
なし

<br>
### レスポンス
#### ステータスコード
200
#### レスポンスヘッダ
なし
#### レスポンスボディ
##### 共通レスポンスボディ
レスポンスはJSONオブジェクトで、オブジェクト（サブオブジェクト）に定義されるキー(名前)と型、並びに値の対応は以下のとおりです。

|オブジェクト<br>|名前【キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|ルート<br>|d<br>|object<br>|オブジェクト{1}<br>|
|{1}<br>|results<br>|array<br>|オブジェクト{2}の配列<br>|
|{2}<br>|__metadata<br>|object<br>|オブジェクト{3}<br>|
|{3}<br>|uri<br>|string<br>|作成したリソースへのURL<br>|
|{3}<br>|etag<br>|string<br>|Etag値<br>|
|{2}<br>|__published<br>|string<br>|作成日(UNIX時間)<br>|
|{2}<br>|__updated<br>|string<br>|更新日(UNIX時間)<br>|
|{1}<br>|__count<br>|string<br>|$inlinecountクエリでの取得結果件数<br>|

##### OutMessage固有レスポンスボディ
|オブジェクト<br>|名前【キー）<br>|型<br>|値<br>|
|:--|:--|:--|:--|
|{3}<br>|type<br>|string<br>|CellCtl.ReceivedMessage<br>|
|{2}<br>|__id<br>|string<br>|受信メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却<br>|
|{2}<br>|_Box.Name<br>|string<br>|関係対象のボックス名<br>|
|{2}<br>|InReplyTo<br>|string<br>|受信元メッセージID<br>UUIDで「b5d008e9092f489c8d3c574a768afc33」のような32文字の文字列を返却<br>|
|{2}<br>|To<br>|string<br>|送信先CellURL<br>|
|{2}<br>|ToRelation<br>|string<br>|送信対象の関係名<br>|
|{2}<br>|Type<br>|string<br>|メッセージタイプ<br>メッセージ：message<br>関係登録依頼：req.relation.build<br>関係削除依頼：req.relation.break<br>|
|{2}<br>|Title<br>|string<br>|メッセージタイトル<br>|
|{2}<br>|Body<br>|string<br>|メッセージ本文<br>|
|{2}<br>|Priority<br>|string<br>|優先度<br>(高)1&#65374;5(低)<br>|
|{2}<br>|RequestRelation<br>|string<br>|登録依頼するリレーションクラスURL、またはリレーションインスタンスURL<br>メッセージタイプが関係登録/削除依頼の場合のみ<br>|
|{2}<br>|RequestRelationTarget<br>|string<br>|関係を結ぶCellURL<br>メッセージタイプが関係登録/削除依頼の場合のみ<br>|
|{2}<br>|Result<br>|array<br>|送信先Cell毎の送信結果<br>オブジェクト{4}の配列<br>|
|{4}<br>|To<br>|string<br>|送信先CellURL<br>|
|{4}<br>|Code<br>|string<br>|ステータスコード<br>|
|{4}<br>|Reason<br>|string<br>|詳細メッセージ<br>|
#### エラーメッセージ一覧
[エラーメッセージ一覧](004_Error_Messages.html)を参照

#### レスポンスサンプル
```json
{
  "d": {
    "results": [
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('c87b42e10df846a9bee842225d1383fe')",
          "etag": "W/\"1-1486683974323\"",
          "type": "CellCtl.SentMessage"
        },
        "__id": "c87b42e10df846a9bee842225d1383fe",
        "_Box.Name": null,
        "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ",
        "To": "https://{UnitFQDN}/{CellName}",
        "ToRelation": null,
        "Type": "message",
        "Title": "メッセージサンプルタイトル",
        "Body": "メッセージサンプル本文です。",
        "Priority": 3,
        "RequestRelation": null,
        "RequestRelationTarget": null,
        "Result": [
          {
            "To": "https://{UnitFQDN}/{CellName}/",
            "Code": "201",
            "Reason": "Created."
          }
        ],
        "__published": "/Date(1486683974323)/",
        "__updated": "/Date(1486683974323)/",
        "_Box": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('c87b42e10df846a9bee842225d1383fe')/_Box"
          }
        }
      },
      {
        "__metadata": {
          "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('f87358607d0d46deae61eec6bb0ea490')",
          "etag": "W/\"1-1486685761907\"",
          "type": "CellCtl.SentMessage"
        },
        "__id": "f87358607d0d46deae61eec6bb0ea490",
        "_Box.Name": null,
        "InReplyTo": "xnKXmd4TTZCw-bfSEw4f0AxnKXmd4TTZ",
        "To": "https://{UnitFQDN}/{CellName}",
        "ToRelation": null,
        "Type": "message",
        "Title": "メッセージサンプルタイトル",
        "Body": "メッセージサンプル本文です。",
        "Priority": 3,
        "RequestRelation": null,
        "RequestRelationTarget": null,
        "Result": [
          {
            "To": "https://{UnitFQDN}/{CellName}/",
            "Code": "201",
            "Reason": "Created."
          }
        ],
        "__published": "/Date(1486685761907)/",
        "__updated": "/Date(1486685761907)/",
        "_Box": {
          "__deferred": {
            "uri": "https://{UnitFQDN}/{CellName}/__ctl/SentMessage('f87358607d0d46deae61eec6bb0ea490')/_Box"
          }
        }
      }
    ]
  }
}
```

<br>
### CURLサンプル

```sh
curl "https://{UnitFQDN}/{CellName}/__ctl/SentMessage" -X GET -i
-H 'Authorization: Bearer {UnitUserToken}' -H 'Accept: application/json'
```
<br>
<br>
<br>
###### Copyright 2017    FUJITSU LIMITED