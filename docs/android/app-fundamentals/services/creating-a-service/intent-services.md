---
title: Xamarin. Android のインテントサービス
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 4c868623ae08ac1366c1c9ea55c8d635f0a6a061
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509136"
---
# <a name="intent-services-in-xamarinandroid"></a>Xamarin. Android のインテントサービス

## <a name="intent-services-overview"></a>インテントサービスの概要

開始とバインドの両方のサービスがメインスレッドで実行されます。つまり、パフォーマンスを円滑に保つために、サービスは非同期に作業を実行する必要があります。 この問題に対処する最も簡単な方法の1つは、_ワーカーキュープロセッサパターン_を使用することです。このパターンでは、実行する作業が1つのスレッドによって処理されるキューに配置されます。

は[`IntentService`](xref:Android.App.IntentService) 、このパターンの Android `Service`固有の実装を提供するクラスのサブクラスです。 キューの機能を管理し、キューを処理するワーカースレッドを起動し、ワーカースレッドで実行する要求をキューからプルします。 は`IntentService` 、キューに作業がなくなったときに、自動的に停止し、ワーカースレッドを削除します。

を作成`Intent`し、それを`Intent` `StartService`メソッドに渡すことによって、作業がキューに送信されます。

操作中に`OnHandleIntent`メソッド`IntentService`を停止または中断することはできません。 この設計のため、は`IntentService`ステートレス&ndash;に保持する必要があります。アクティブな接続やアプリケーションの他の部分との通信に依存しないようにしてください。 は、作業要求を statelessly 処理することを目的としています。`IntentService`

サブクラス`IntentService`化には、次の2つの要件があります。

1. (サブクラス`IntentService`によって作成された) `OnHandleIntent`新しい型は、メソッドのみをオーバーライドします。
2. 新しい型のコンストラクターには、要求を処理するワーカースレッドの名前を指定するために使用される文字列が必要です。 このワーカースレッドの名前は、主にアプリケーションをデバッグするときに使用されます。

次のコードは、 `IntentService`オーバーライド`OnHandleIntent`されたメソッドを使用したの実装を示しています。

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

作業がに`IntentService`送信されるのは`Intent` 、をインスタンス化[`StartService`](xref:Android.Content.Context.StartService*)してから、そのインテントをパラメーターとしてメソッドを呼び出すことによって行われます。 インテントは、 `OnHandleIntent`メソッドのパラメーターとしてサービスに渡されます。 次のコードスニペットは、作業要求をインテントに送信する例です。 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

は`IntentService` 、次のコードスニペットに示すように、意図した値を抽出できます。  

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
