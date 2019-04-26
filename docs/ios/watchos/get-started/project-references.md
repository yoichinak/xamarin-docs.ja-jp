---
title: watchOS で Xamarin プロジェクトの参照
description: このドキュメントでは、iOS アプリ、watch アプリでは、watch アプリ拡張機能の関係について説明します。 プロジェクトの参照、およびバンドルがについて説明します識別子。
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/13/2016
ms.openlocfilehash: c900ab714fed2bb1e02367ba39ad3c5a0a76121e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61408350"
---
# <a name="watchos-project-references-in-xamarin"></a>watchOS で Xamarin プロジェクトの参照

_IOS アプリ、watch アプリ、およびウォッチ拡張機能間のリレーションシップの説明です。_

WatchOS ソリューションの 3 つのプロジェクトは*自動的に構成された*3 watchOS アプリを構築して正しくバンドルの特定の方法で相互に参照します。 これらのプロジェクト参照とバンドルの識別子の設定は、以下の参照を説明します。

## <a name="project-references"></a>プロジェクトの参照

各プロジェクトの参照ノードをダブルクリックして、参照を表示します。

- **iPhone アプリ**参照**Watch アプリ**

![](project-references-images/catalog-reference1.png "iPhone アプリは、Watch アプリを参照します。")

- **アプリを見る**参照**Watch アプリの拡張機能**

![](project-references-images/catalog-reference2.png "iPhone アプリは、Watch アプリを参照します。")


 - **Watch アプリの拡張機能**他のプロジェクトのいずれかを参照していません

![](project-references-images/catalog-reference3.png "Watch アプリの拡張機能は、他のプロジェクトを参照していません")



## <a name="bundle-identifiers"></a>バンドル識別子

確認する必要がありますも、**バンドル識別子**が正しい。
次の 3 つのすべてのプロジェクトがあります、*同じ*定義済みの拡張機能のある 2 つのウォッチ プロジェクトで、識別子のプレフィックス`watchkitextension`と`watchkitapp`、次のように (用、 **WatchKitCatalog**例):

 - Xamarin.iOS 統合プロジェクト- `com.xamarin.WatchKitCatalog`

 - WatchKit 拡張機能プロジェクト- `com.xamarin.WatchKitCatalog.watchkitextension`

 - Watch アプリ プロジェクトの場合- `com.xamarin.WatchKitCatalog.watchkitapp`

確認これら**Info.plist**設定が正しい。

 - Watch アプリ プロジェクトの`WKCompanionAppBundleIdentifier`親/コンテナー アプリのバンドル ID と一致する (ie。 iPhone で実行されている 1 つ)。

 - ウォッチ キットの拡張機能プロジェクトの**WKApp バンドル ID** Watch アプリ プロジェクトのバンドル ID と一致

ダブルクリックすると、識別子を編集することができます、 **Info.plist**各プロジェクト ファイル。

このスクリーン ショットは、**ウォッチ拡張機能の**Info.plist ファイルの表示、 **Watch アプリの**も識別子。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)
    
![](project-references-images/infoplist-extension.png "このスクリーン ショットは、ウォッチ拡張機能の Info.plist ファイルです。")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
![](project-references-images/infoplist-extension-vs.png "このスクリーン ショットは、ウォッチ拡張機能の Info.plist ファイルです。")

-----

このスクリーン ショットは、 **Watch アプリの**Info.plist ファイル。
現在**ウォッチ OS**バージョンは、8.2、ため、**配置ターゲット**Watch アプリの**8.2**します。 Xcode 6.3 がインストールした場合、8.3 にこの値を設定することがあります -、変更する必要がありますに注意してください 8.2 します。

![](project-references-images/infoplist-watchapp.png "ウォッチの Info.plist ファイル")

Watch アプリの展開ターゲットは、ウォッチ拡張機能と iOS アプリは別に指定できます。

