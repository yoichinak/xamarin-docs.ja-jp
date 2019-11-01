---
title: Xamarin での watchOS プロジェクト参照
description: このドキュメントでは、iOS アプリ、watch アプリ、および watch アプリの拡張機能の関係について説明します。 ここでは、プロジェクト参照とバンドル識別子について説明します。
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 3dcd5f17b35b9829831adcf997d8bde97c0572e7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030169"
---
# <a name="watchos-project-references-in-xamarin"></a>Xamarin での watchOS プロジェクト参照

_IOS アプリ、watch アプリ、および watch 拡張機能の関係に関する説明。_

WatchOS ソリューション内の3つのプロジェクトは、watchOS 3 アプリをビルドして正しくバンドルするために、特定の方法で相互に参照するように*自動的に構成*されます。 以下では、これらのプロジェクト参照とバンドル識別子の設定について説明します。

## <a name="project-references"></a>プロジェクト参照

参照を表示するには、各プロジェクトの [参照] ノードをダブルクリックします。

- **iPhone アプリ**が**ウォッチアプリ**を参照

  ![](project-references-images/catalog-reference1.png "iPhone app references Watch App")

- **アプリ**参照の監視**アプリの拡張機能**を見る

  ![](project-references-images/catalog-reference2.png "iPhone app references Watch App")

- **Watch アプリの拡張機能**は、他のプロジェクトのいずれも参照していません

  ![](project-references-images/catalog-reference3.png "Watch App Extension does not reference the other projects")

## <a name="bundle-identifiers"></a>バンドル識別子

また、**バンドル識別子**が正しいことを確認する必要もあります。
3つのプロジェクトはすべて*同じ*識別子プレフィックスを持つ必要があります。2つの watch プロジェクトには `watchkitextension` と `watchkitapp`の事前定義された拡張機能があり、 **WatchKitCatalog**の例では次のようになっています。

- Xamarin. iOS 統合プロジェクト-`com.xamarin.WatchKitCatalog`

- WatchKit 拡張機能プロジェクト-`com.xamarin.WatchKitCatalog.watchkitextension`

- アプリプロジェクトを見る-`com.xamarin.WatchKitCatalog.watchkitapp`

また、次の**情報の plist**設定が正しいことを確認します。

- Watch アプリプロジェクトの `WKCompanionAppBundleIdentifier` が、親/コンテナーアプリのバンドル ID (iPhone で実行されているもの) に一致します。

- Watch Kit 拡張機能プロジェクトの**WKApp バンドル id**は、watch アプリプロジェクトのバンドル id と一致します。

識別子を編集するには、各プロジェクトの**情報の plist**ファイルをダブルクリックします。

このスクリーンショットは、watch**アプリの**識別子も表示されている**Watch の拡張機能の**情報 plist ファイルです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](project-references-images/infoplist-extension.png "This screenshot is the Watch Extension's Info.plist file")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](project-references-images/infoplist-extension-vs.png "This screenshot is the Watch Extension's Info.plist file")

-----

このスクリーンショットは、 **Watch アプリの**情報の plist ファイルです。
現在の**WATCH OS**バージョンは8.2 であるため、ウォッチアプリの**デプロイターゲット**は**8.2**である必要があります。 Xcode 6.3 がインストールされている場合は、この値が8.3 に設定されている可能性があることに注意してください。8.2 を変更する必要があります。

![](project-references-images/infoplist-watchapp.png "The watch Info.plist file")

Watch アプリのデプロイターゲットは、Watch 拡張機能と iOS アプリとは異なる場合があります。
