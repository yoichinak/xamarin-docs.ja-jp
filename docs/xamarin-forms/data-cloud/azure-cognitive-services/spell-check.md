---
title: Bing Spell Check API を使用したスペルチェック
description: Bing Spell Check は、テキストに対して文脈によるスペルチェックを実行し、スペルミスの語句にインラインでの候補を提供します。 この記事では、Bing Spell Check REST API を使用して、アプリケーションのスペルミスを修正する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: B40EB103-FDC0-45C6-9940-FB4ACDC2F4F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4446e92ee1d9b6938464375ba36c6e8e4cdfed6b
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93365790"
---
# <a name="spell-checking-using-the-bing-spell-check-api"></a>Bing Spell Check API を使用したスペルチェック

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_Bing Spell Check は、テキストに対して文脈によるスペルチェックを実行し、スペルミスの語句にインラインでの候補を提供します。この記事では、Bing Spell Check REST API を使用して、アプリケーションのスペルミスを修正する方法について説明し Xamarin.Forms ます。_

## <a name="overview"></a>概要

Bing Spell Check REST API には2つの動作モードがあり、API への要求を行うときにモードを指定する必要があります。

- `Spell` 大文字と小文字を変更せずに短いテキスト (最大9語) を修正します。
- `Proof` 長いテキストを修正し、大文字小文字の修正と基本的な句読点を提供し、積極的な修正を抑制します。

> [!NOTE]
> [Azure サブスクリプション](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing)をお持ちでない場合は、開始する前に[無料アカウント](https://aka.ms/azfree-docs-mobileapps)を作成してください。

Bing Spell Check API を使用するには、API キーを取得する必要があります。 これは、 [Try Cognitive Services](https://azure.microsoft.com/try/cognitive-services/)

Bing Spell Check API によってサポートされる言語の一覧については、「 [サポートされる言語](/azure/cognitive-services/bing-spell-check/bing-spell-check-supported-languages/)」を参照してください。 Bing Spell Check API の詳細については、 [Bing Spell Check のドキュメント](/azure/cognitive-services/bing-spell-check/)を参照してください。

## <a name="authentication"></a>認証

Bing Spell Check API に対して行われるすべての要求には、ヘッダーの値として指定する必要がある API キーが必要です `Ocp-Apim-Subscription-Key` 。 次のコード例は、API キーを要求のヘッダーに追加する方法を示してい `Ocp-Apim-Subscription-Key` ます。

```csharp
public BingSpellCheckService()
{
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", Constants.BingSpellCheckApiKey);
}
```

Bing Spell Check API に有効な API キーを渡さないと、401応答エラーが発生します。

## <a name="performing-spell-checking"></a>スペルチェックの実行

スペルチェックを実行するには、の API に対して GET 要求または POST 要求を行い `SpellCheck` `https://api.cognitive.microsoft.com/bing/v7.0/SpellCheck` ます。 GET 要求を行うと、スペルチェックされるテキストがクエリパラメーターとして送信されます。 POST 要求を行うと、スペルチェックされるテキストが要求本文で送信されます。 GET 要求は、クエリパラメーター文字列の長さが制限されているため、スペルチェックが1500文字に制限されます。 そのため、短い文字列をスペルチェックする場合を除き、通常は POST 要求を行う必要があります。

サンプルアプリケーションでは、 `SpellCheckTextAsync` メソッドはスペルチェックプロセスを呼び出します。

```csharp
public async Task<SpellCheckResult> SpellCheckTextAsync(string text)
{
    string requestUri = GenerateRequestUri(Constants.BingSpellCheckEndpoint, text, SpellCheckMode.Spell);
    var response = await SendRequestAsync(requestUri);
    var spellCheckResults = JsonConvert.DeserializeObject<SpellCheckResult>(response);
    return spellCheckResults;
}
```

メソッドは、 `SpellCheckTextAsync` 要求 URI を生成し、その要求を API に送信し `SpellCheck` ます。これにより、結果を含む JSON 応答が返されます。 JSON 応答が逆シリアル化され、結果が表示のために呼び出し元のメソッドに返されます。

### <a name="configuring-spell-checking"></a>スペルチェックの構成

スペルチェックプロセスは、HTTP クエリパラメーターを指定することによって構成できます。

```csharp
string GenerateRequestUri(string spellCheckEndpoint, string text, SpellCheckMode mode)
{
  string requestUri = spellCheckEndpoint;
  requestUri += string.Format("?text={0}", text);                         // text to spell check
  requestUri += string.Format("&mode={0}", mode.ToString().ToLower());    // spellcheck mode - proof or spell
  return requestUri;
}
```

このメソッドは、テキストをスペルチェックするように設定し、スペルチェックモードを設定します。

Bing Spell Check REST API の詳細については、「 [Spell Check API v7 reference](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)」を参照してください。

### <a name="sending-the-request"></a>要求を送信しています

メソッドは、 `SendRequestAsync` GET 要求を Bing Spell Check REST API に対して行い、応答を返します。

```csharp
async Task<string> SendRequestAsync(string url)
{
    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

このメソッドは、取得要求を API に送信し `SpellCheck` ます。要求 URL には、変換するテキストを指定し、スペルチェックモードを指定します。 次に、応答が読み取られ、呼び出し元のメソッドに返されます。

要求が有効であり、要求された `SpellCheck` 情報が応答内にあることを示すために、API は応答で HTTP 状態コード 200 (OK) を送信します。 応答オブジェクトの一覧については、「 [応答オブジェクト](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference#response-objects)」を参照してください。

### <a name="processing-the-response"></a>応答の処理

API 応答が JSON 形式で返されます。 次の JSON データは、テキストのスペルが間違っている場合の応答メッセージを示してい `Go shappin tommorow` ます。

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

配列に、 `flaggedTokens` テキストのスペルが正しくない、または文法が間違っているというフラグが付けられたテキスト内の単語の配列が含まれています。 スペルミスまたは文法エラーが見つからない場合、配列は空になります。 配列内のタグは次のとおりです。

- `offset` –文字列の先頭から、フラグが設定された単語までの、0から始まるオフセット。
- `token` –文字列のスペルが正しくない、または文法が間違っている単語。
- `type` –単語のフラグが設定される原因となったエラーの種類。 2つの値 (と) があり `RepeatedToken` `UnknownToken` ます。
- `suggestions` –スペルミスまたは文法エラーを修正する単語の配列。 配列はとで構成され `suggestion` `score` ます。これは、提案された修正が正しいことを示す信頼度のレベルを示します。

サンプルアプリケーションでは、JSON 応答がインスタンスに逆シリアル化され、 `SpellCheckResult` 結果が表示のために呼び出し元のメソッドに返されます。 次のコード例では、 `SpellCheckResult` 表示するインスタンスの処理方法を示します。

```csharp
var spellCheckResult = await bingSpellCheckService.SpellCheckTextAsync(TodoItem.Name);
foreach (var flaggedToken in spellCheckResult.FlaggedTokens)
{
  TodoItem.Name = TodoItem.Name.Replace(flaggedToken.Token, flaggedToken.Suggestions.FirstOrDefault().Suggestion);
}
```

このコードは、コレクションを反復処理 `FlaggedTokens` し、ソーステキスト内のスペルミスまたは文法的に誤りのある単語を最初の候補と置き換えます。 次のスクリーンショットは、スペルチェックの前後を示しています。

![Spell Check 前](spell-check-images/before-spell-check.png)

![Spell Check 後](spell-check-images/after-spell-check.png)

> [!NOTE]
> 上記の例では、 `Replace` わかりやすくするためにを使用していますが、大量のテキストを使用すると、間違ったトークンが置き換えられる可能性があります。 API では、 `offset` 更新を実行するソーステキスト内の正しい場所を識別するために、運用アプリで使用される値が提供されます。

## <a name="summary"></a>まとめ

この記事では、Bing Spell Check REST API を使用して、アプリケーションのスペルミスを修正する方法について説明しました Xamarin.Forms 。 Bing Spell Check は、テキストに対して文脈によるスペルチェックを実行し、スペルミスの語句にインラインでの候補を提供します。

## <a name="related-links"></a>関連リンク

- [Bing Spell Check のドキュメント](/azure/cognitive-services/bing-spell-check/)
- [RESTful Web サービスを使用する](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Todo Cognitive Services (サンプル)](/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [Bing Spell Check API v7 リファレンス](/rest/api/cognitiveservices/bing-spell-check-api-v7-reference/)