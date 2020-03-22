---
title: Xamarin. Android のインテントサービス
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 1fa1e5f06ebc847775a11c92ccdfc1055bee196a
ms.sourcegitcommit: 6c60914b380ff679bbffd7790edd4d5e18005d0a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/21/2020
ms.locfileid: "80070324"
---
# <a name="intent-services-in-xamarinandroid"></a>Xamarin. Android のインテントサービス

開始とバインドの両方のサービスがメインスレッドで実行されます。つまり、パフォーマンスを円滑に保つために、サービスは非同期に作業を実行する必要があります。 この問題に対処する最も簡単な方法の1つは、_ワーカーキュープロセッサパターン_を使用することです。このパターンでは、実行する作業が1つのスレッドによって処理されるキューに配置されます。

[`IntentService`](xref:Android.App.IntentService)は、このパターンの Android 固有の実装を提供する、`Service` クラスのサブクラスです。 キューの機能を管理し、キューを処理するワーカースレッドを起動し、ワーカースレッドで実行する要求をキューからプルします。 キューに作業がなくなったときに、`IntentService` が自動的に停止し、ワーカースレッドが削除されます。

作業をキューに送信するには、`Intent` を作成し、その `Intent` を `StartService` メソッドに渡します。

操作中に `OnHandleIntent` メソッド `IntentService` を停止または中断することはできません。 この設計のため、`IntentService` は、アクティブな接続またはアプリケーションの他の部分との通信に依存しないように &ndash; ステートレスに保つ必要があります。 `IntentService` は、作業要求を statelessly 処理することを目的としています。

`IntentService`のサブクラス化には、次の2つの要件があります。

1. 新しい型 (サブクラス化によって作成された `IntentService`) は、`OnHandleIntent` メソッドだけをオーバーライドします。
2. 新しい型のコンストラクターには、要求を処理するワーカースレッドの名前を指定するために使用される文字列が必要です。 このワーカースレッドの名前は、主にアプリケーションをデバッグするときに使用されます。

次のコードは、オーバーライドされた `OnHandleIntent` メソッドを使用した `IntentService` の実装を示しています。

```csharp
[Service]
public class DemoIntentService: IntentService
{
    public DemoIntentService () : base("DemoIntentService")
    {
    }

    protected override void OnHandleIntent (Android.Content.Intent intent)
    {
        Console.WriteLine ("perform some long running work");
        ...
        Console.WriteLine ("work complete");
    }
}
```

`Intent` をインスタンス化し、そのインテントをパラメーターとして[`StartService`](xref:Android.Content.Context.StartService*)メソッドを呼び出すことによって、作業が `IntentService` に送信されます。 インテントは、`OnHandleIntent` メソッドのパラメーターとしてサービスに渡されます。 次のコードスニペットは、作業要求をインテントに送信する例です。 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.PutExtra("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService` は、次のコードスニペットに示すように、意図した値を抽出できます。  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");

    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```

## <a name="related-links"></a>関連リンク

- [IntentService](xref:Android.App.IntentService)
- [StartService](xref:Android.Content.Context.StartService*)
