---
title: スペル チェック、Bing Spell Check API を使用します。
description: Bing Spell Check 実行コンテキストに応じたスペルをチェック、テキストのスペル ミスの語句のインラインの候補を提供します。 この記事では、Bing Spell Check REST API を使用して、Xamarin.Forms アプリケーションでスペル ミスを修正する方法について説明します。
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 9a54743ed7dc3ce23c3306589c0bae1e0fd3206c
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/08/2019
ms.locfileid: "67658679"
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>スペル チェック、Bing Spell Check API を使用します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)

_Bing Spell Check 実行コンテキストに応じたスペルをチェック、テキストのスペル ミスの語句のインラインの候補を提供します。この記事では、Bing Spell Check REST API を使用して、Xamarin.Forms アプリケーションでスペル ミスを修正する方法について説明します。_

## <a name="overview"></a>概要

Bing Spell Check REST API は 2 つの動作モードを備え、API に要求を行う場合、モードを指定する必要があります。

- `Spell` 大文字小文字の区別、変更せずに短いテキスト (最大 9 個の単語) を修正します。
- `Proof` 長いテキストの修正、大文字と小文字の修正と基本的な句読点調整などを提供および積極的な修正を抑制します。

Bing Spell Check API を使用する API キーを取得する必要があります。 これから入手できる[Cognitive Services をお試しください](https://azure.microsoft.com/try/cognitive-services/)

Bing Spell Check API でサポートされる言語の一覧は、[サポートされる言語](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/)を参照してください。 Bing Spell Check API の詳細については、[Bing Spell 確認ドキュメント](/azure/cognitive-services/bing-spell-check/)を参照してください。

## <a name="authentication"></a>認証

Bing Spell Check API に加えられたすべての要求が API キーの値として指定する必要があります、`Ocp-Apim-Subscription-Key`ヘッダー。 次のコード例は、API キーを追加する方法を示しています、`Ocp-Apim-Subscription-Key`要求のヘッダー。

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Bing Spell Check API に有効な API キーを渡すの失敗は、401 の応答、エラーが発生されます。

## <a name="performing-spell-checking"></a>スペル チェックを実行します。

GET または POST 要求を送信してスペル チェックを実現できます、`SpellCheck`で API を`https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck`します。 GET 要求を行うときに、スペル チェックの対象に、テキストは、クエリ パラメーターとして送信されます。 POST 要求を行うときに、スペル チェックの対象に、テキストは、要求本文で送信されます。 GET 要求では、スペル チェックのクエリ パラメーターの文字列の長さの制限により 1500 文字に制限されます。 したがって、POST 要求は、短い文字列には、スペル チェックがされている場合を除きに通常作成する必要があります。

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

`SpellCheckTextAsync`メソッドが要求 URI を生成しする要求を送信し、 `SpellCheck` API で、結果を含む JSON 応答を返します。 JSON 応答は、表示するため、呼び出し元メソッドに返される結果を逆シリアル化です。

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

このメソッドは、スペル チェック、およびスペル チェック モードにするテキストを設定します。

Bing Spell Check REST API の詳細については、[Spell Check API v7 参照](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)を参照してください。

### <a name="sending-the-request"></a>要求を送信します。

`SendRequestAsync`メソッドは、GET 要求を Bing Spell Check REST API に実行し、応答を返します。

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

このメソッドが GET 要求を送信、 `SpellCheck` API を変換するテキストを指定する要求 URL と、スペル チェック モードを使用します。 応答が読み取られ、呼び出し元メソッドに返されます。

`SpellCheck` API は、要求が有効である、要求が成功したことを示すし、の要求された情報は、応答で提供される応答には、HTTP 状態コード 200 (OK) を送信します。 応答オブジェクトの一覧は、[応答オブジェクト](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects)を参照してください。

### <a name="processing-the-response"></a>応答の処理

API 応答は JSON 形式で返されます。 次の JSON データには、スペル ミスのテキストの応答メッセージが表示されます`Go shappin tommorow`:。

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

`flaggedTokens`テキストに正しく入力されていないまたは間違いとしてフラグ付けされた単語の配列が配列に含まれています。 スペルや文法エラーが見つからない場合は空になります。 配列内のタグは次のとおりです。

- `offset` – フラグが設定された単語をテキスト文字列の先頭からの 0 から始まるオフセット。
- `token` – スペルが正しくないまたは文法的に正しくないテキスト文字列内の単語。
- `type` – フラグを設定する word の原因となったエラーの種類。 2 つの値がある`RepeatedToken`と`UnknownToken`します。
- `suggestions` – スペルまたは文法エラーを修正する単語の配列。 配列から成る、`suggestion`と`score`修正案が正しいことを信頼度のレベルを示します。

サンプル アプリケーションで JSON 応答が逆シリアル化、`SpellCheckResult`インスタンスを表示するため、呼び出し元メソッドに返される結果。 次のコード例に示す方法、`SpellCheckResult`表示のインスタンスの処理します。

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

このコードの反復、`FlaggedTokens`コレクションと、スペル ミスの置換、または最初の修正候補のソース テキスト内の文法的に誤った単語。 次のスクリーン ショットは、スペル チェックの前後に表示します。

![](spell-check-images/before-spell-check.png "スペル チェックする前に")

![](spell-check-images/after-spell-check.png "スペル チェックの後")

> [!NOTE]
> 使用して上記の例`Replace`間違ったトークンを置き換えることができますが、わかりやすくするため、大量のテキストの間で。 API は、提供、`offset`値の更新プログラムを実行するソース テキスト内の正しい場所を識別するために運用アプリで使用する必要があります。

## <a name="summary"></a>まとめ

この記事では、Bing Spell Check REST API を使用して、Xamarin.Forms アプリケーションでスペル ミスを修正する方法について説明します。 Bing Spell Check 実行コンテキストに応じたスペルをチェック、テキストのスペル ミスの語句のインラインの候補を提供します。

## <a name="related-links"></a>関連リンク

- [Bing Spell Check ドキュメント](/azure/cognitive-services/bing-spell-check/)
- [RESTful Web サービスを使用します。](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoCognitiveServices/)
- [Bing Spell Check API v7 の詳細の参照](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)
