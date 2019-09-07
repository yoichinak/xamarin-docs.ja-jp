---
title: Xamarin での watchOS プロジェクト参照
description: このドキュメントでは、iOS アプリ、watch アプリ、および watch アプリの拡張機能の関係について説明します。 ここでは、プロジェクト参照とバンドル識別子について説明します。
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/13/2016
ms.openlocfilehash: dcadb5146df39aa4887e28b65078acc9454f3d34
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767987"
---
# <a name="watchos-project-references-in-xamarin"></a>Xamarin での watchOS プロジェクト参照

_IOS アプリ、watch アプリ、および watch 拡張機能の関係に関する説明。_

WatchOS ソリューション内の3つのプロジェクトは、watchOS 3 アプリをビルドして正しくバンドルするために、特定の方法で相互に参照するように*自動的に構成*されます。 以下では、これらのプロジェクト参照とバンドル識別子の設定について説明します。

## <a name="project-references"></a>プロジェクトの参照

参照を表示するには、各プロジェクトの [参照] ノードをダブルクリックします。

- **iPhone アプリ**が**ウォッチアプリ**を参照

  ![](project-references-images/catalog-reference1.png "iPhone アプリがウォッチアプリを参照")

- **アプリ**参照の監視**アプリの拡張機能**を見る

  ![](project-references-images/catalog-reference2.png "iPhone アプリがウォッチアプリを参照")

- **Watch アプリの拡張機能**は、他のプロジェクトのいずれも参照していません

  ![](project-references-images/catalog-reference3.png "Watch App Extension は他のプロジェクトを参照していません")

## <a name="bundle-identifiers"></a>バンドル識別子

また、**バンドル識別子**が正しいことを確認する必要もあります。
3つのプロジェクトすべての識別子プレフィックスは*同じ*である必要があります。2つ`watchkitextension`の watch プロジェクトでは、次のように、および`watchkitapp`の定義済みの拡張があります ( **WatchKitCatalog**の例の場合)。

- Xamarin. iOS 統合プロジェクト-`com.xamarin.WatchKitCatalog`

- WatchKit Extension プロジェクト-`com.xamarin.WatchKitCatalog.watchkitextension`

- アプリプロジェクトの監視-`com.xamarin.WatchKitCatalog.watchkitapp`

また、次の**情報の plist**設定が正しいことを確認します。

- Watch アプリプロジェクト`WKCompanionAppBundleIdentifier`は、親/コンテナーアプリのバンドル ID (iPhone で実行されるもの) に一致します。

- Watch Kit 拡張機能プロジェクトの**WKApp バンドル id**は、watch アプリプロジェクトのバンドル id と一致します。

識別子を編集するには、各プロジェクトの**情報の plist**ファイルをダブルクリックします。

このスクリーンショットは、watch**アプリの**識別子も表示されている**Watch の拡張機能の**情報 plist ファイルです。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

![](project-references-images/infoplist-extension.png "このスクリーンショットは、Watch 拡張機能の情報の plist ファイルです。")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](project-references-images/infoplist-extension-vs.png "このスクリーンショットは、Watch 拡張機能の情報の plist ファイルです。")

-----

このスクリーンショットは、 **Watch アプリの**情報の plist ファイルです。
現在の**WATCH OS**バージョンは8.2 であるため、ウォッチアプリの**デプロイターゲット**は**8.2**である必要があります。 Xcode 6.3 がインストールされている場合は、この値が8.3 に設定されている可能性があることに注意してください。8.2 を変更する必要があります。

![](project-references-images/infoplist-watchapp.png "ウォッチ情報の plist ファイル")

Watch アプリのデプロイターゲットは、Watch 拡張機能と iOS アプリとは異なる場合があります。
