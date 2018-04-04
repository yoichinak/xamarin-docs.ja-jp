---
title: スペル チェック Bing スペル チェック API を使用して
description: Bing スペル チェックを実行コンテキストのスペル チェック テキスト、スペル ミスのインラインの推奨事項を提供します。 この記事では、Bing スペル チェックの REST API を使用して、Xamarin.Forms アプリケーション内のスペル ミスを修正する方法について説明します。
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 41bd79b22aa193dd5303847997bc07e8e8d12e58
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>スペル チェック Bing スペル チェック API を使用して

_Bing スペル チェックを実行コンテキストのスペル チェック テキスト、スペル ミスのインラインの推奨事項を提供します。この記事では、Bing スペル チェックの REST API を使用して、Xamarin.Forms アプリケーション内のスペル ミスを修正する方法について説明します。_

## <a name="overview"></a>概要

Bing スペル チェックの REST API は 2 つの動作モードを持ち、API に要求を行うときに、モードを指定する必要があります。

- `Spell` 大文字と小文字は変更せずには、短いテキスト (最大 9 個の単語) を修正します。
- `Proof` 長いテキストの修正、大文字と小文字の修正と基本的な区切り記号、およびこ積極的な修正を抑制します。

API キーは、Bing スペル チェック API を使用して取得する必要があります。 これはに[認知サービスを実行してください](https://azure.microsoft.com/try/cognitive-services/)

Bing スペル チェック API でサポートされる言語の一覧は、次を参照してください。[サポートされる言語](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/)します。 Bing スペル チェック API の詳細については、次を参照してください。 [Bing スペル チェック ドキュメント](/azure/cognitive-services/bing-spell-check/)です。

## <a name="authentication"></a>認証

Bing スペル チェック API に加えられたすべての要求の値として指定する必要がある API キーが必要です、`Ocp-Apim-Subscription-Key`ヘッダー。 次のコード例は、API キーを追加する方法を示します、`Ocp-Apim-Subscription-Key`要求のヘッダー。

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Bing スペル チェック API に有効な API キーを渡すエラー 401 応答エラーが発生します。

## <a name="performing-spell-checking"></a>スペル チェックを実行します。

GET または POST 要求を送信してスペル チェック機能を実現できます、 `SpellCheck` API`https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`です。 GET 要求を行うときは、スペル チェックの対象にテキストがクエリ パラメーターとして送信されます。 POST 要求を行うとき、スペル チェックの対象にテキストが要求本文で送信されます。 GET 要求は、スペル チェックのクエリ パラメーターの文字列の長さの制限により 1500 文字に制限されます。 したがって、POST 要求は、短い文字列は、スペル チェックをしている場合を除きに通常作成する必要があります。

サンプル アプリケーションで、`SpellCheckTextAsync`メソッドは、スペル チェックのプロセスを呼び出します。

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

`SpellCheckTextAsync`メソッドが要求 URI を生成し、要求を送信し、 `SpellCheck` API で、結果を含む JSON 応答を返します。 JSON 応答の逆シリアル化、表示するための呼び出し元のメソッドに返される結果を使用します。

### <a name="configuring-spell-checking"></a>スペル チェックを構成します。

スペル チェックのプロセスは、HTTP クエリ パラメーターを指定して構成できます。

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

このメソッドは、スペル チェック、およびスペル チェック モードであるテキストを設定します。

Bing スペル チェックの REST API の詳細については、次を参照してください。[スペル チェック API v7 リファレンス](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)です。

### <a name="sending-the-request"></a>要求を送信します。

`SendRequestAsync`メソッドは Bing スペル チェックの REST API に GET 要求を出すし、応答を返します。

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

API キーの値として追加すると、このメソッドが GET 要求を作成、`Ocp-Apim-Subscription-Key`ヘッダー。 GET 要求に送信し、 `SpellCheck` API を変換するテキストを指定する要求の URL とスペル チェック モードを使用します。 応答が読み取られ、呼び出し元メソッドに返されます。

`SpellCheck` API は、要求が有効である、要求が成功したことを示すことと、要求された情報が応答で提供される、応答の HTTP ステータス コード 200 (OK) を送信します。 応答オブジェクトの一覧は、次を参照してください。[応答オブジェクト](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects)です。

### <a name="processing-the-response"></a>応答の処理

API 応答は、JSON 形式で返されます。 次の JSON データには、スペル ミスのテキストの応答メッセージが表示されます`Go shappin tommorow`:。

```json
{  
   "_type":"SpellCheck",
   "flaggedTokens":[  
      {  
         "offset":3,
         "token":"shappin",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"shopping",
               "score":1
            }
         ]
      },
      {  
         "offset":11,
         "token":"tommorow",
         "type":"UnknownToken",
         "suggestions":[  
            {  
               "suggestion":"tomorrow",
               "score":1
            }
         ]
      }
   ],
   "correctionType":"High"
}
```

`flaggedTokens`配列には、テキスト内で正しく入力されていないか、間違いとしてフラグ付けされた単語の配列が含まれています。 配列はスペルや文法エラーが見つからない場合は、空になります。 配列内のタグは次のとおりです。

- `offset` – フラグが付けられたの単語にテキスト文字列の先頭からの 0 から始まるオフセット。
- `token` – スペルが正しくないか、正しい文法されていないテキスト文字列内の単語です。
- `type` – フラグが付けられます word の原因となったエラーの種類。 2 つの値がある`RepeatedToken`と`UnknownToken`です。
- `suggestions` – スペルや文法エラーを修正する単語の配列。 配列から成る、`suggestion`と`score`、修正候補が正しいことを信頼度のレベルを示します。

サンプル アプリケーションで JSON 応答が逆シリアル化、`SpellCheckResult`表示するための呼び出し元のメソッドに返される結果のインスタンス。 次のコード例に示す方法、`SpellCheckResult`表示のインスタンスの処理します。

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

このコードが反復、`FlaggedTokens`コレクションとそのスペル ミスがある後継または最初の候補のソース テキスト内の文法的に誤った単語。 次のスクリーン ショットは、スペル チェックの前後に表示します。

![](spell-check-images/before-spell-check.png "スペル チェックする前に")

![](spell-check-images/after-spell-check.png "スペル チェックした後")

## <a name="summary"></a>まとめ

この記事では、Bing スペル チェックの REST API を使用して、Xamarin.Forms アプリケーション内のスペル ミスを修正する方法について説明します。 Bing スペル チェックを実行コンテキストのスペル チェック テキスト、スペル ミスのインラインの推奨事項を提供します。

## <a name="related-links"></a>関連リンク

- [Bing スペル チェックのドキュメント](/azure/cognitive-services/bing-spell-check/)
- [RESTful Web サービスの使用](~/xamarin-forms/data-cloud/consuming/rest.md)
- [Todo 認知サービス (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing スペル チェック API v7 リファレンス](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
