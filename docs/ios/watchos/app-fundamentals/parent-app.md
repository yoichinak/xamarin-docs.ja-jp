---
title: Xamarin での watchOS 親アプリケーションの操作
description: このドキュメントでは、Xamarin で watchOS 親アプリケーションを操作する方法について説明します。 WatchOS アプリ拡張機能、iOS アプリ、共有ストレージなどについて説明します。
ms.prod: xamarin
ms.assetid: 9AD29833-E9CC-41A3-95D2-8A655FF0B511
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 002c57a1549201018cb2068f000a038f686eb2c0
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436733"
---
# <a name="working-with-the-watchos-parent-application-in-xamarin"></a>Xamarin での watchOS 親アプリケーションの操作

Watch アプリとバンドルされている iOS アプリとの間で通信を行うには、さまざまな方法があります。

- Watch アプリは、iPhone の親アプリで [コードを実行](#run-code) できます。

- 監視拡張機能は、親 iPhone アプリと [ストレージの場所を共有](#shared-storage) できます。

- ハンドオフを使用して、通知からのデータを watch アプリに渡し、ユーザーをアプリの特定のインターフェイスコントローラーに送信します。

親アプリは、コンテナーアプリと呼ばれることもあります。

## <a name="run-code"></a>コードの実行

次の2つのサンプルでは、を使用して `WCSession` コードを実行し、watch アプリとペアになっている iPhone との間でメッセージを送信する方法を示します。

- [接続の監視](/samples/xamarin/ios-samples/watchos-watchconnectivity/)
- [SimpleWatchConnectivity](/samples/xamarin/ios-samples/watchos-simplewatchconnectivity/) 

## <a name="shared-storage"></a>ストレージの共有

[アプリグループ](~/ios/watchos/app-fundamentals/app-groups.md)を構成すると、iOS 8 拡張機能 (watch extensions を含む) が親アプリとデータを共有できるようになります。

### <a name="nsuserdefaults"></a>NSUserDefaults

次のコードは、watch アプリ拡張機能と親 iPhone アプリの両方で記述できます。これにより、共通のセットを参照でき `NSUserDefaults` ます。

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

<a name="files"></a>

### <a name="files"></a>Files

IOS アプリと watch の拡張機能は、共通のファイルパスを使用してファイルを共有することもできます。

```csharp
var FileManager = new NSFileManager ();
var appGroupContainer =
            FileManager.GetContainerUrl ("group.com.your-company.watchstuff");
var appGroupContainerPath = appGroupContainer.Path;
Console.WriteLine ("agcpath: " + appGroupContainerPath);
// use the path to create and update files
```

注: パスがの場合は、 `null` [アプリグループの構成](~/ios/watchos/app-fundamentals/app-groups.md) を確認して、プロビジョニングプロファイルが正しく構成されており、開発用コンピューターにダウンロード/インストールされていることを確認します。

詳細については、 [アプリグループ機能](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) のドキュメントを参照してください。

## <a name="related-links"></a>関連リンク

- [Apple の WKInterfaceController リファレンス](https://developer.apple.com/library/prerelease/ios/documentation/WatchKit/Reference/WKInterfaceController_class/index.html#//apple_ref/occ/clm/WKInterfaceController/openParentApplication:reply:)
- [アプリを含む Apple の共有データ](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)