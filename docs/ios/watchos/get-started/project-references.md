---
title: watchOS Xamarin のプロジェクト参照
description: このドキュメントでは、iOS アプリ、watch アプリ、およびウォッチ アプリ拡張機能の関係について説明します。 プロジェクトの参照、およびバンドルについても説明識別子。
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: 1bd950d0929beae7133b0eb8ef6b2a69bc116f50
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791489"
---
# <a name="watchos-project-references-in-xamarin"></a>watchOS Xamarin のプロジェクト参照

_IOS アプリ、watch アプリ、およびウォッチ拡張機能間のリレーションシップの説明。_

WatchOS ソリューション内の次の 3 つのプロジェクトは*自動的に構成された*watchOS 3 アプリをビルドし、正しくバンドルする特定の方法で相互に参照します。 これらのプロジェクト参照とバンドル識別子の設定は、下リファレンスについては説明します。

## <a name="project-references"></a>プロジェクト参照

プロジェクトごとに参照ノードをダブルクリックすると、参照を参照してください。

- **iPhone アプリ**参照**Watch アプリ**

![](project-references-images/catalog-reference1.png "iPhone アプリ Watch アプリを参照します。")

- **アプリを見る**参照**Watch アプリ拡張機能**

![](project-references-images/catalog-reference2.png "iPhone アプリ Watch アプリを参照します。")


 - **Watch アプリ拡張機能**他のプロジェクトのいずれかを参照していません

![](project-references-images/catalog-reference3.png "Watch アプリ拡張機能は、他のプロジェクトを参照していません")



## <a name="bundle-identifiers"></a>バンドルの識別子

またことを確認する必要があります、**バンドル識別子**が正しい。
次の 3 つのすべてのプロジェクトが必要、*同じ*ウォッチ式の 2 つのプロジェクトの定義済みの拡張機能のことで、識別子のプレフィックス`watchkitextension`と`watchkitapp`、次のように (用、 **WatchKitCatalog**例):

 - Xamarin.iOS 統合プロジェクト- `com.xamarin.WatchKitCatalog`

 - WatchKit 拡張機能プロジェクトの場合- `com.xamarin.WatchKitCatalog.watchkitextension`

 - Watch アプリ プロジェクト `com.xamarin.WatchKitCatalog.watchkitapp`

確認これら**Info.plist**設定が正しい。

 - Watch アプリ プロジェクトの`WKCompanionAppBundleIdentifier`親/コンテナー アプリのバンドル ID と一致する (ie。 iPhone で実行されている 1 つ)。

 - ウォッチ キットの拡張機能プロジェクトの**WKApp バンドル ID** Watch アプリ プロジェクトのバンドル ID と一致

ダブルクリックして、識別子を編集することができます、 **Info.plist**それぞれのプロジェクト ファイルです。

このスクリーン ショットは、**ウォッチ拡張機能の**を示す、Info.plist のファイル、 **Watch アプリ**も識別子。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)
    
![](project-references-images/infoplist-extension.png "このスクリーン ショットは、ウォッチ拡張機能の Info.plist ファイルです。")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
![](project-references-images/infoplist-extension-vs.png "このスクリーン ショットは、ウォッチ拡張機能の Info.plist ファイルです。")

-----

このスクリーン ショットは、 **Watch アプリ**Info.plist ファイル。
現在**ウォッチ OS**バージョンは 8.2、ため、**配置ターゲット**Watch アプリにする必要があります**8.2**です。 注 Xcode 6.3 がインストールされていればに 8.3 形式にこの値を設定することがあります -、変更する必要があります 8.2 です。

![](project-references-images/infoplist-watchapp.png "ウォッチ Info.plist ファイル")

Watch アプリの展開先をウォッチ拡張機能と iOS アプリは別に指定できます。

