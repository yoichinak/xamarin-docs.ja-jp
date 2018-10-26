---
title: Xamarin.Android でのインテント サービス
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 1301f34ad1f7a0069c542ba81bf237a673fd239d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112859"
---
# <a name="intent-services-in-xamarinandroid"></a>Xamarin.Android でのインテント サービス

## <a name="intent-services-overview"></a>インテント サービスの概要

どちらも開始して、バインドされたこと、サービスはパフォーマンスをスムーズに保つを非同期的に作業を実行する必要があります、メイン スレッドで実行するサービス。 この問題に対処する最も簡単な方法の 1 つは、_ワーカー キュー プロセッサ パターン_操作を完了するは 1 つのスレッドによって処理されるキューに配置します。 

[ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/)のサブクラスには、`Service`このパターンの特定の Android での実装を提供するクラス。 管理キューの作業をワーカー スレッド、キュー サービスを起動し、プル要求がワーカー スレッドで実行されるキューをオフします。 `IntentService`静かに自体を停止し、キュー内の他の作業がある場合に、ワーカー スレッドを削除します。
 
作成して作業がキューに送信された、`Intent`を渡します`Intent`を`StartService`メソッド。

停止または中断することはできません、`OnHandleIntent`メソッド`IntentService`それが動作中にします。 このデザインにより、`IntentService`おく必要があるステートレス&ndash;アクティブな接続または他のアプリケーションからの通信に使用しないでください。 `IntentService` Statelessly 作業要求を処理するためのものです。

サブクラス化の 2 つの要件がある`IntentService`:

1. 新しい型 (サブクラスによって作成された`IntentService`) のみをオーバーライド、`OnHandleIntent`メソッド。
2. 新しい型のコンス トラクターには、要求を処理するワーカー スレッドの名前に使用される文字列が必要です。 このワーカー スレッドの名前は、主に、アプリケーションのデバッグ時に使用します。

次のコードは、 `IntentService` 、オーバーライドされた実装`OnHandleIntent`メソッド。

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

作業に送信される、`IntentService`をインスタンス化して、`Intent`しを呼び出す、 [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)をパラメーターとしてその意図を持つメソッド。 パラメーターとして、目的が、サービスに渡される、`OnHandleIntent`メソッド。 このコード スニペットでは、インテントに作業要求を送信する例を示します。 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService`このコード スニペットに示すように、目的の値を抽出できます。  

```csharp
protected override void OnHandleIntent (Android.Content.Intent intent)
{
    string fileToDownload = intent.GetStringExtra("file_to_download");
    
    Log.Debug("DemoIntentService", $"File to download: {fileToDownload}.");
}
```


## <a name="related-links"></a>関連リンク

- [IntentService](https://developer.xamarin.com/api/type/Android.App.IntentService/)
- [StartService](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)
