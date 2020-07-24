---
title: Xamarin での watchOS プロジェクト参照
description: このドキュメントでは、iOS アプリ、watch アプリ、および watch アプリの拡張機能の関係について説明します。 ここでは、プロジェクト参照とバンドル識別子について説明します。
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/13/2016
ms.openlocfilehash: 96873e1bff34a4ef3ed76d675ca0a2b5c03f0d72
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997242"
---
# <a name="watchos-project-references-in-xamarin"></a>Xamarin での watchOS プロジェクト参照

_IOS アプリ、watch アプリ、および watch 拡張機能の関係に関する説明。_

WatchOS ソリューション内の3つのプロジェクトは、watchOS 3 アプリをビルドして正しくバンドルするために、特定の方法で相互に参照するように*自動的に構成*されます。 以下では、これらのプロジェクト参照とバンドル識別子の設定について説明します。

## <a name="project-references"></a>プロジェクト参照

参照を表示するには、各プロジェクトの [参照] ノードをダブルクリックします。

- **iPhone アプリ**が**ウォッチアプリ**を参照

  ![iPhone アプリがウォッチアプリを参照](project-references-images/catalog-reference1.png)

- **アプリ**参照の監視**アプリの拡張機能**を見る

  ![iPhone アプリがウォッチアプリを参照](project-references-images/catalog-reference2.png)

- **Watch アプリの拡張機能**は、他のプロジェクトのいずれも参照していません

  ![Watch App Extension は他のプロジェクトを参照していません](project-references-images/catalog-reference3.png)

## <a name="bundle-identifiers"></a>バンドル識別子

また、**バンドル識別子**が正しいことを確認する必要もあります。
3つのプロジェクトすべての識別子プレフィックスは*同じ*である必要があります。2つの watch プロジェクトでは、次のように、およびの定義済みの拡張があり `watchkitextension` `watchkitapp` ます ( **WatchKitCatalog**の例の場合)。

- Xamarin. iOS 統合プロジェクト-`com.xamarin.WatchKitCatalog`

- WatchKit Extension プロジェクト-`com.xamarin.WatchKitCatalog.watchkitextension`

- アプリプロジェクトの監視-`com.xamarin.WatchKitCatalog.watchkitapp`

また、次の**情報の plist**設定が正しいことを確認します。

- Watch アプリプロジェクトは、 `WKCompanionAppBundleIdentifier` 親/コンテナーアプリのバンドル ID (iPhone で実行されるもの) に一致します。

- Watch Kit 拡張機能プロジェクトの**WKApp バンドル id**は、watch アプリプロジェクトのバンドル id と一致します。

識別子を編集するには、各プロジェクトの**情報の plist**ファイルをダブルクリックします。

このスクリーンショットは、watch**アプリの**識別子も表示されている**Watch の拡張機能の**情報 plist ファイルです。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

![このスクリーンショットは、Watch 拡張機能の情報の plist ファイルです。](project-references-images/infoplist-extension.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![このスクリーンショットは、Watch 拡張機能の情報の plist ファイルです。](project-references-images/infoplist-extension-vs.png)

-----

このスクリーンショットは、 **Watch アプリの**情報の plist ファイルです。
現在の**WATCH OS**バージョンは8.2 であるため、ウォッチアプリの**デプロイターゲット**は**8.2**である必要があります。 Xcode 6.3 がインストールされている場合は、この値が8.3 に設定されている可能性があることに注意してください。8.2 を変更する必要があります。

![ウォッチ情報の plist ファイル](project-references-images/infoplist-watchapp.png)

Watch アプリのデプロイターゲットは、Watch 拡張機能と iOS アプリとは異なる場合があります。
