---
title: Xamarin での watchOS 設定の使用
description: このドキュメントでは、Xamarin で watchOS 設定を使用する方法について説明します。 ここでは、watch アプリソリューションに設定を追加する方法、アプリで設定を使用する方法、および iPhone で Apple Watch アプリを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: c5ad40c375320ed21acb3f0a75c5c66f2efc7824
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938652"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>Xamarin での watchOS 設定の使用

Apple Watch アプリは iOS アプリと同じ設定機能を使用できます。 [設定] ユーザーインターフェイスが**Apple Watch** iphone アプリに表示されますが、値は iphone アプリと Watch 拡張機能の両方でアクセスできます。

![Apple Watch アプリは iOS アプリと同じ設定機能を使用できます](settings-images/intro.png)

設定は、**アプリグループ**によって定義された iOS アプリと watch アプリ拡張機能の両方からアクセスできる共有ファイルの場所に格納されます。 次の手順に従って、設定を追加する前に、[アプリグループを構成](~/ios/watchos/app-fundamentals/app-groups.md)する必要があります。

## <a name="add-settings-in-a-watch-solution"></a>Watch ソリューションに設定を追加する

ソリューション内の**iPhone アプリ**(watch アプリまたは拡張機能では*ありません*) で、次のようにします。

1. [**新しいファイルの追加 >** ] を右クリックし、[設定] を選択し**ます**([**新しいファイル**] ダイアログで名前を編集することはできません)。

   [![新しい設定バンドルを追加する](settings-images/settings-add-sml.png)](settings-images/settings-add.png#lightbox)

2. 名前を "**設定-Watch. バンドル**" に変更します (select と type **Command + R**の名前を変更します)。

   ![バンドルの名前を変更する](settings-images/settings-rename.png)

3. 構成した `ApplicationGroupContainerIdentifier` アプリグループに設定された値を使用して、**ルート plist**に新しいキーを追加します (例として、 `group.com.xamarin.WatchSettings`このサンプルでは、次のことを行います。

   [![ApplicationGroupContainerIdentifier キーをルートに追加します。 plist](settings-images/settings-appgroup-sml.png)](settings-images/settings-appgroup.png#lightbox)

4. 使用するオプションが含まれるように**Settings-Watch/** を編集します。テンプレートファイルには、グループが含まれています。
  textfield、トグルスイッチ、スライダー (既定では、削除して独自の設定で置き換えることができます):

  [![Settings-Watch/ルートを編集します。](settings-images/rootplist-sml.png)](settings-images/rootplist.png#lightbox)

## <a name="use-settings-in-the-watch-app"></a>Watch アプリで設定を使用する

ユーザーが選択した値にアクセスするには、 `NSUserDefaults` アプリグループを使用してインスタンスを作成し、次のように指定し `NSUserDefaultsType.SuiteName` ます。

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch アプリ

[![IPhone 上の新しい Apple Watch アプリ](settings-images/settings-app-sml.png)](settings-images/settings-app.png#lightbox)

ユーザーは、iPhone の新しい**Apple Watch**アプリを使用して設定を操作します。 このアプリを使用すると、ユーザーはウォッチでアプリの表示/非表示を切り替えたり、**設定**を使用して公開された設定を編集したりすることができます。

![アプリ設定の例](settings-images/applewatch-1.png) ![アプリ設定の例](settings-images/applewatch-2.png)

## <a name="related-links"></a>関連リンク

- [Apple の設定ドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
