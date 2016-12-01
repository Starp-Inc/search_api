# Overview

ここでは、Gifly API V1リソースに関する基本的なことを記載しています。

* [Current Version](https://github.com/gifly/gifly_project/blob/master/docs/apis/v1/overview.md#current-version)
* [Schema](https://github.com/gifly/gifly_project/blob/master/docs/apis/v1/overview.md#schema)
* [Client Errors](https://github.com/gifly/gifly_project/blob/master/docs/apis/v1/overview.md#client-errors)
* [HTTP Verbs](https://github.com/gifly/gifly_project/blob/master/docs/apis/v1/overview.md#http-verbs)


## Current Version
現在のGifly APIのデフォルトバージョンは、`V1`です。

## Schema

全てのAPIは`https://api.gifly.jp`にアクセスする必要があります。
データフォーマットはJSON形式となります。

```
curl -i https://api.gifly.jp/v1/searches?tags=foobar

HTTP/1.1 200 OK
Server: nginx
Date: Fri, 12 Oct 2012 23:33:14 GMT
Content-Type: application/json; charset=utf-8
Connection: keep-alive
Status: 200 OK
ETag: "a00049ba79152d03380c34652f2cb612"
X-Gifly-Media-Type: gifly.v1

Content-Length: 100
Cache-Control: max-age=0, private, must-revalidate
```

全てのタイムスタンプはISO 8601 形式で返答されます。
```
YYYY-MM-DDTHH:MM:SSZ
```

## Client Errors

クライアントエラーの際は、`400`をステータスコードとしてレスポンスを返答します。

#### 400エラーが返る主な原因
* 壊れたJSONを送信した(Parse Error)
* 仕様と異なるJSON値を送信した(Validation Error) (例. searchs apiで、tagsを5個以上送信してしまった場合など。

### エラーのフォーマット

現在は、下記の一種類のみとなります。

```
HTTP/1.1 400 Bad Request
Content-Length: 38

{"message":"here is an error message"}
```


## HTTP Verbs

| Name | Description |
|:-----------|:------------|
|GET	|	リソースを検索/取得する際に使用します|
|POST	|	リソースを作成する際に使用します。|
|PATCH	|リソースの一部を更新する際に使用します。|
|PUT	|	リソース、またはコレクションをリプレイする際に使用します。|
|DELETE	|リソースを削除する際に利用します。|


## Authentication
現在はAuthenticationはサポートしていません。


# GIF Search API


## List Searched Gifs

検索クエリにマッチしたGIFs Listをレスポンスとして返します。

```
GET /searches
```

### Parameters

| Name | Type | Description |
|:-----------|:------------|:------------|
| tags       |  array of string   |  完全なタグ名を入力します。カンマ区切りで複数タグを検索可能です。(最大5個) (*qとの併用はできません)       |
| q    |  string     |  URLEncode済みの検索キーワードを入力します。<br>このキーワードはタグの前方一致検索として機能します。<br> Giflyではアルファベットや漢字を含むタグもひらがな、カタカナで検索可能です。<br> (例 `Star Wars`, `Star trek`というタグを含むGIFは、`q=スター`で取得可能。 (*tagsとの併用はできません)<br><br> 高度な検索<br>・OR検索: `q=a+OR+b`と、`+OR+`でキーワード同士を繋ぐことでOR検索を行うことが出来ます。|
| page      | integer | ページ番号 |


[](
| sort      | integer | ソートに使用するキーです。<br><br>利用可能なキーの一覧<br>・score<br>・created_at |
| direction | string  | ソートオーダーを`asc` or `desc`から指定します。
)


### Response


#### Example
```
curl -H 'Accept: application/json;' https://api.gifly.jp/v1/searches?q=shark&page=2
```

```
Status: 200 OK
```
```json
{
  "total_count": 2,
  "items": [
    {
      "id": "d25e5b51-7d9f-11e6-b2e3-6d612654d530",
      "user_id": 1,
      "image": "https://gifly.jp/gifs/96504291-9929-11e6-aee9-dfdc2f6b25ef",
      "poster_image": {
        "large":"https://cdn.gifly.jp/gif_poster_images/96504291-9929-11e6-aee9-dfdc2f6b25ef/large/966d8e90-9929-11e6-aee9-dfdc2f6b25ef.jpg",
        "medium":"https://cdn.gifly.jp/gif_poster_images/96504291-9929-11e6-aee9-dfdc2f6b25ef/medium/966d8e90-9929-11e6-aee9-dfdc2f6b25ef.jpg",
        "small":"https://cdn.gifly.jp/gif_poster_images/96504291-9929-11e6-aee9-dfdc2f6b25ef/small/966d8e90-9929-11e6-aee9-dfdc2f6b25ef.jpg",
        "square":"https://cdn.gifly.jp/gif_poster_images/96504291-9929-11e6-aee9-dfdc2f6b25ef/square/966d8e90-9929-11e6-aee9-dfdc2f6b25ef.jpg"
      },
      "poster_image_content_type": "image\/jpeg",
      "category_id": 1,
      "child_category_id": 1,
      "source_url": "https:\/\/www.youtube.com\/watch?v=foobar",
      "duration": 3,
      "created_at": "2016-09-18T13:00:09.000Z",
      "updated_at": "2016-09-18T13:00:09.000Z",
      "tags": [
        {
          "id": 1,
          "name": "shark"
        },
        {
          "id": 2,
          "name": "planet"
        }
      ]
    }
  ]
}
```
