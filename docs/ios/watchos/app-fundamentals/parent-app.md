---
title: WatchOS Xamarin で親アプリケーションの使用
description: このドキュメントでは、Xamarin で watchOS 親アプリケーションを操作する方法について説明します。 これは、WatchKit アプリ拡張機能、iOS アプリ、共有ストレージ、および詳細について説明します。
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 3af2cce0d84e3934eeb89917990f111d29aadef1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790693"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>WatchOS Xamarin で親アプリケーションの使用

> [!IMPORTANT]
> 次の例をのみを使用して親アプリケーションへのアクセスは、watchOS 1 watch アプリで機能します。


Watch アプリとそれがバンドルされている iOS アプリ間で通信するさまざまな方法があります。

- ウォッチ拡張機能は[メソッドを呼び出す](#code)iPhone でバック グラウンドで実行されている親アプリに対してです。

- ウォッチ拡張機能は[記憶域の場所を共有](#storage)親 iPhone アプリのです。

- ハンドオフを使用して、データ概要または通知をアプリで特定のインターフェイス コント ローラーにユーザーを送信する、Watch アプリに渡します。

親アプリは、コンテナー アプリケーションと呼ばれることもあります。


<a name="code" />

## <a name="run-code"></a>コードを実行します。

ウォッチ拡張機能と親の iPhone アプリ間の通信はではデモンストレーション、 [GpsWatch サンプル](https://developer.xamarin.com/samples/GpsWatch)です。
ウォッチ拡張機能は、処理を行うものを使用して、代わりに親の iOS アプリを要求できる、`OpenParentApplication`メソッドです。

これは、長時間実行タスク (などのネットワーク要求) の親のみを iOS アプリを利用してこれらのタスクを完了し、ウォッチ拡張機能にアクセスできる場所で取得したデータを保存するバック グラウンドの処理のために特に便利です。



### <a name="watch-kit-app-extension"></a>ウォッチ キット アプリ拡張機能

呼び出す、`WKInterfaceController.OpenParentApplication`ウォッチ アプリの拡張機能です。 返します、`bool`メソッド要求が正常に送信されたかどうかを示すです。 チェック、`error`応答でパラメーター `Action` iPhone アプリで実行されているメソッド中に発生した場合を決定します。

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

IPhone アプリのを介して watch アプリ拡張機能からのすべての呼び出しがルーティングされる`HandleWatchKitExtensionRequest`メソッドです。
Watch アプリでさまざまな要求を行うかどうかは、このメソッドはクエリを実行する必要があります、`userInfo`要求を処理する方法を決定するためのディクショナリ。


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

構成する場合、[アプリ グループ](~/ios/watchos/app-fundamentals/app-groups.md)し iOS 8 の拡張機能 (ウォッチ拡張機能を含む) は、親のアプリとデータを共有することができます。

<a name="nsuserdefaults" />

### <a name="nsuserdefaults"></a>NSUserDefaults

一連の共通参照できるように、ウォッチ アプリ拡張機能と親の iPhone アプリの両方で、次のコードを記述できます`NSUserDefaults`:

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

IOS アプリとウォッチ拡張機能には、共通のファイルのパスを使用してファイルを共有できます。

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

注: パスの場合`null`確認し、[アプリ グループ構成](~/ios/watchos/app-fundamentals/app-groups.md)プロビジョニング プロファイルが正しく構成されているし、開発用コンピューターにダウンロード/インストールされていることを確認します。

詳細についてを参照してください、[アプリ グループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md)ドキュメント。

## <a name="wormholesharp"></a>WormHoleSharp

WatchOS 1 の一般的なオープン ソース メカニズム (に基づいて[MMWormHole](https://github.com/mutualmobile/MMWormhole)) を親アプリと watch アプリ間でデータやコマンドを渡します。

次のように、iOS アプリでのアプリ グループを使用して穴を構成して拡張機能を見る。

```csharp
// AppDelegate (iOS) or InterfaceController (watch extension)
Wormhole wormHole;
// ...
wormHole = new Wormhole ("group.com.your-company.watchstuff", "messageDir");
```

C# バージョンをダウンロード[WormHoleSharp](https://github.com/Clancey/WormHoleSharp)です。



## <a name="related-links"></a>関連リンク

- [GpsWatch (サンプル)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [WormHoleSharp (サンプル)](https://github.com/Clancey/WormHoleSharp)
- [Apple の WKInterfaceController 参照](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple のアプリを含むデータを共有します。](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
