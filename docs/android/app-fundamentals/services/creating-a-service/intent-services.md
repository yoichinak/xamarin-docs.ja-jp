---
title: Xamarin.Android で目的のサービス
ms.prod: xamarin
ms.assetid: A5B86FE4-C8E2-4B0A-84CA-EF8F5119E31B
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 02/16/2018
ms.openlocfilehash: 80849213649707615f8bd8e941e1a51c6b54e76e
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="intent-services-in-xamarinandroid"></a>Xamarin.Android で目的のサービス

## <a name="intent-services-overview"></a>目的のサービスの概要

どちらも開始され、サービスが実行をするためにパフォーマンス smooth、サービスに必要な作業を非同期的に実行することを意味、メイン スレッドをバインドします。 この問題に対処する最も簡単な方法の 1 つは、_ワーカー キュー プロセッサ パターン_作業が必要が 1 つのスレッドによって提供されるキューに保存されます。 

[ `IntentService` ](https://developer.xamarin.com/api/type/Android.App.IntentService/)のサブクラスは、`Service`このパターンの特定の Android での実装を提供するクラス。 キュー サービスを提供するワーカー スレッドの起動、キューの作業を管理して、プル要求がワーカー スレッドで実行するキューをオフします。 `IntentService`サイレント モードでそれ自体を停止し、キュー内の他の作業がある場合は、ワーカー スレッドを削除します。
 
作業が作成することで、キューに送信される、`Intent`を渡す、`Intent`を`StartService`メソッドです。

停止または中断することはできません、`OnHandleIntent`メソッド`IntentService`動作しているときにします。 この設計により、`IntentService`ステートレスを保持するか&ndash;アクティブな接続または他のアプリケーションからの通信に依存する必要があります。 `IntentService` Statelessly 作業要求を処理するためのものです。

サブクラス化の 2 つの要件がある`IntentService`:

1. 新しい型 (サブクラスによって作成された`IntentService`) が上書きされるだけ、`OnHandleIntent`メソッドです。
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

作業に送信される、`IntentService`をインスタンス化して、`Intent`しを呼び出す、 [ `StartService` ](https://developer.xamarin.com/api/member/Android.Content.Context.StartService/p/Android.Content.Intent/)をパラメーターとしてその目的を持つメソッドです。 サービスへのパラメーターとして渡されます、目的とした、`OnHandleIntent`メソッドです。 このコード スニペットは、目的に、作業要求を送信する例を示します。 

```csharp
// This code might be called from within an Activity, for example in an event
// handler for a button click.
Intent downloadIntent = new Intent(this, typeof(DemoIntentService));

// This is just one example of passing some values to an IntentService via the Intent:
downloadIntent.Put
("file_to_download", "http://www.somewhere.com/file/to/download.zip");

StartService(downloadIntent);
```

`IntentService`このコード スニペットに示すように、意図した値を抽出できます。  

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
