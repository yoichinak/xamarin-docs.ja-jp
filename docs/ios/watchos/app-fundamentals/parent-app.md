---
title: 親アプリケーション Xamarin で watchOS の操作
description: このドキュメントでは、Xamarin で watchOS 親アプリケーションを操作する方法について説明します。 WatchOS アプリ拡張機能、iOS アプリ、共有ストレージなどについて説明します。
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 3e11b163d16be9711bf09102e3ab8604d98299d7
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75487764"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>親アプリケーション Xamarin で watchOS の操作

Watch アプリとそれにバンドルされている iOS アプリ間で通信するさまざまな方法はあります。

- Watch アプリは、iPhone の親アプリで[コードを実行](#run-code)できます。

- ウォッチ拡張機能は[記憶域の場所を共有](#shared-storage)親の iPhone アプリ。

- ハンドオフを使用して、通知からのデータを watch アプリに渡し、ユーザーをアプリの特定のインターフェイスコントローラーに送信します。

親アプリは、コンテナー アプリと呼ばれることもあります。

## <a name="run-code"></a>コードを実行します。

次の2つのサンプルでは、`WCSession` を使用してコードを実行し、watch アプリとペアの iPhone との間でメッセージを送信する方法を示します。

- [接続の監視](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchconnectivity/)
- [SimpleWatchConnectivity](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-simplewatchconnectivity/) 

## <a name="shared-storage"></a>共有記憶域

構成する場合、[アプリ グループ](~/ios/watchos/app-fundamentals/app-groups.md)し iOS 8 の拡張機能 (ウォッチ拡張機能を含む) は、親アプリとデータを共有することができます。

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

## <a name="related-links"></a>関連リンク

- [Apple の WKInterfaceController 参照](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [Apple のデータを含むアプリを共有します。](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
