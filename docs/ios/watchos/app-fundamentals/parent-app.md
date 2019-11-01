---
title: Xamarin での watchOS 親アプリケーションの操作
description: このドキュメントでは、Xamarin で watchOS 親アプリケーションを操作する方法について説明します。 WatchKit アプリ拡張機能、iOS アプリ、共有ストレージなどについて説明します。
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 08637fe13b28565e7c6ab1c89b291c6db3b81025
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001477"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Xamarin での watchOS 親アプリケーションの操作

> [!IMPORTANT]
> 以下の例を使用した親アプリケーションへのアクセスは、watchOS 1 watch アプリでのみ機能します。

Watch アプリとバンドルされている iOS アプリとの間で通信を行うには、さまざまな方法があります。

- Watch extensions は、iPhone のバックグラウンドで実行されている親アプリに対して[メソッドを呼び出す](#code)ことができます。

- 監視拡張機能は、親 iPhone アプリと[ストレージの場所を共有](#storage)できます。

- ハンドオフを使用して、ひとめでもデータを Watch アプリに渡し、ユーザーをアプリ内の特定のインターフェイスコントローラーに送信します。

親アプリは、コンテナーアプリと呼ばれることもあります。

<a name="code" />

## <a name="run-code"></a>コードの実行

" [Gpswatch" サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-gpswatch)では、watch 拡張機能と親 iPhone アプリ間の通信について説明します。
Watch 拡張機能では、`OpenParentApplication` 方法を使用して、親 iOS アプリに代わって処理を行うように要求できます。

これは、実行時間の長いタスク (ネットワーク要求を含む) で特に便利です。親 iOS アプリのみがバックグラウンド処理を利用してこれらのタスクを完了し、取得したデータを watch 拡張機能からアクセスできる場所に保存することができます。

### <a name="watch-kit-app-extension"></a>Watch Kit アプリ拡張機能

Watch アプリ拡張機能で `WKInterfaceController.OpenParentApplication` を呼び出します。 メソッドの要求が正常に送信されたかどうかを示す `bool` を返します。 応答 `Action` の `error` パラメーターを調べて、iPhone アプリで実行されているメソッドの実行中に発生したかどうかを確認します。

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

Watch アプリの拡張機能からのすべての呼び出しは、iPhone アプリの `HandleWatchKitExtensionRequest` メソッドを介してルーティングされます。
Watch アプリで別の要求を行っている場合、このメソッドは `userInfo` ディクショナリに対してクエリを実行し、要求の処理方法を決定する必要があります。

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

[アプリグループ](~/ios/watchos/app-fundamentals/app-groups.md)を構成すると、iOS 8 拡張機能 (watch extensions を含む) が親アプリとデータを共有できるようになります。

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

次のコードは、watch アプリ拡張機能と親 iPhone アプリの両方で記述できます。これにより、`NSUserDefaults`の共通セットを参照できます。

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

IOS アプリと watch の拡張機能は、共通のファイルパスを使用してファイルを共有することもできます。

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

注: パスが `null` 場合は、[アプリグループの構成](~/ios/watchos/app-fundamentals/app-groups.md)を確認して、プロビジョニングプロファイルが正しく構成されており、開発用コンピューターにダウンロード/インストールされていることを確認します。

詳細については、[アプリグループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)のドキュメントを参照してください。

## <a name="wormholesharp"></a>WormHoleSharp

WatchOS 1 ( [Mmwormhole](https://github.com/mutualmobile/MMWormhole)に基づく) 用の一般的なオープンソースメカニズムで、親アプリと watch アプリの間でデータまたはコマンドを渡します。

IOS アプリと watch 拡張機能で、次のようなアプリグループを使用して、ワームホールを構成できます。

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

バージョン [WormHoleSharp](https://github.com/Clancey/WormHoleSharp) C#をダウンロードします。

## <a name="related-links"></a>関連リンク

- [GpsWatch (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [WormHoleSharp (サンプル)](https://github.com/Clancey/WormHoleSharp)
- [Apple の WKInterfaceController リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [アプリを含む Apple の共有データ](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
