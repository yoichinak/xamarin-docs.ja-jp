---
title: 親アプリケーション Xamarin で watchOS の操作
description: このドキュメントでは、Xamarin で watchOS 親アプリケーションを操作する方法について説明します。 WatchKit アプリの拡張機能、iOS アプリや、共有記憶域がについて説明します。
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 05dfb419834c2eee94f98d023df3a3fe8d6eee90
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2019
ms.locfileid: "70198117"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>親アプリケーション Xamarin で watchOS の操作

> [!IMPORTANT]
> 次の例をのみを使用して、親アプリケーションへのアクセスは、watchOS 1 watch アプリで機能します。


Watch アプリとそれにバンドルされている iOS アプリ間で通信するさまざまな方法はあります。

- Watch extensions は、iPhone のバックグラウンドで実行されている親アプリに対して[メソッドを呼び出す](#code)ことができます。

- ウォッチ拡張機能は[記憶域の場所を共有](#storage)親の iPhone アプリ。

- ハンドオフを使用して、ひとめでもデータを Watch アプリに渡し、ユーザーをアプリ内の特定のインターフェイスコントローラーに送信します。

親アプリは、コンテナー アプリと呼ばれることもあります。


<a name="code" />

## <a name="run-code"></a>コードを実行します。

方法については、ウォッチ拡張機能と親の iPhone アプリ間の通信、 [GpsWatch サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-gpswatch)します。
ウォッチ拡張機能は、その代理の使用に関するいくつかの処理を実行して、親 iOS アプリを要求できる、`OpenParentApplication`メソッド。

これは、機能は、長時間実行されるタスク (などのネットワーク要求) - のみ親 iOS アプリを利用してこれらのタスクを完了して、ウォッチ拡張機能にアクセスできる場所で取得したデータを保存するためにバック グラウンド処理のために特に便利です。



### <a name="watch-kit-app-extension"></a>キット アプリ拡張機能をご覧ください。

呼び出す、`WKInterfaceController.OpenParentApplication`ウォッチ アプリの拡張機能。 返します、`bool`メソッド要求が正常に送信されたかどうかを示します。 チェック、 `error` 、応答でパラメーター`Action`存在する場合を判断する iPhone アプリで実行されているメソッドの中に発生します。

```csharp
WKInterfaceController.OpenParentApplication (new NSDictionary (), (replyInfo, error) => {
    if(error != null) {
        Console.WriteLine (error);
        return;
    }
    Console.WriteLine ("parent app responded");
    // do something with replyInfo[] dictionary
});
```


### <a name="ios-app"></a>iOS アプリ

Watch アプリ拡張機能からのすべての呼び出しは、iPhone アプリのを介してルーティングされます`HandleWatchKitExtensionRequest`メソッド。
Watch アプリでさまざまな要求を行っているかどうかは、このメソッドがクエリを実行する必要があります、`userInfo`要求を処理する方法を決定するためのディクショナリ。


```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    // ... other AppDelegate methods
    public override void HandleWatchKitExtensionRequest
        (UIApplication application, NSDictionary userInfo, Action<NSDictionary> reply)
    {
        var status = 2;
        // do something in the background, and respond
        reply (new NSDictionary (
            "count", NSNumber.FromInt32 ((int)status),
            "value2", new NSString("some-info")
            ));
    }
}
```


<a name="storage" />

## <a name="shared-storage"></a>共有記憶域

構成する場合、[アプリ グループ](~/ios/watchos/app-fundamentals/app-groups.md)し iOS 8 の拡張機能 (ウォッチ拡張機能を含む) は、親アプリとデータを共有することができます。

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

一連の共通に参照できるように、watch アプリ拡張機能と親の iPhone アプリの両方で、次のコードを記述できます`NSUserDefaults`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
        "group.com.your-company.watchstuff",
        NSUserDefaultsType.SuiteName);

// set values
shared.SetInt (2, "count");
shared.Synchronize ();

// get values
shared.Synchronize ();
var count = shared.IntForKey ("count");
```

<a name="files" />

### <a name="files"></a>ファイル

IOS アプリとウォッチ拡張機能には、一般的なファイル パスを使用してファイル共有もできます。

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

注: パスの場合`null`確認し、[アプリ グループの構成](~/ios/watchos/app-fundamentals/app-groups.md)プロビジョニング プロファイルが正しく構成されているし、開発用コンピューターにダウンロード/インストールされていることを確認します。

詳細についてを参照してください、[アプリ グループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)ドキュメント。

## <a name="wormholesharp"></a>WormHoleSharp

WatchOS 1 用の人気のあるオープン ソース メカニズム (に基づいて[MMWormHole](https://github.com/mutualmobile/MMWormhole)) 親 app と watch アプリ間でデータやコマンドを渡す。

ワームホール iOS アプリでこのようなアプリ グループを使用してを構成し、拡張機能を見ることができます。

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

ダウンロード、C#バージョン[WormHoleSharp](https://github.com/Clancey/WormHoleSharp)します。



## <a name="related-links"></a>関連リンク

- [GpsWatch (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [WormHoleSharp (サンプル)](https://github.com/Clancey/WormHoleSharp)
- [Apple の WKInterfaceController 参照](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple のデータを含むアプリを共有します。](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
