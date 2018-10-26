---
title: WatchOS で Xamarin の設定の操作
description: このドキュメントでは、Xamarin で watchOS の設定を操作する方法について説明します。 Iphone の場合、アプリと、Apple Watch アプリでこれらの設定を使用して、watch アプリ ソリューションに追加の設定について説明します。
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 36164e1e9f92b5a5520d10f769f3953cfa2ceb85
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106352"
---
# <a name="working-with-watchos-settings-in-xamarin"></a>WatchOS で Xamarin の設定の操作

Apple Watch アプリは iOS アプリと同じ設定機能を使用することができます - でユーザー インターフェイスの設定が表示されます、 **Apple Watch** iPhone アプリが、値は、iPhone アプリ両方とウォッチ拡張機能でアクセスできるようにします。

![](settings-images/intro.png "Apple Watch アプリは iOS アプリと同じ設定機能を使用することができます。")

設定を iOS アプリとによって定義された、watch アプリ拡張機能の両方にアクセスできる共有ファイルの場所に格納されます、**アプリ グループ**します。 必要があります[アプリ グループを構成する](~/ios/watchos/app-fundamentals/app-groups.md)以下の手順を使用して、設定を追加する前にします。

## <a name="add-settings-in-a-watch-solution"></a>監視ソリューションで設定を追加します。

**IPhone アプリ**ソリューションに (*いない*watch アプリまたは拡張機能)。

1. 右クリックして**追加 > 新しいファイル.** 選択**Settings.bundle** (内の名前を編集することはできません、**新しいファイル**ダイアログ)。

   [![](settings-images/settings-add-sml.png "新しい設定バンドルを追加します。")](settings-images/settings-add.png#lightbox)

2. 名を変更して**設定 Watch.bundle** (選択し、入力**command+r**名前を変更する)。

   ![](settings-images/settings-rename.png "バンドルの名前を変更します。")

3. 新しいキーを追加`ApplicationGroupContainerIdentifier`を**Root.plist**値 (例: を構成したアプリ グループを設定して `group.com.xamarin.WatchSettings` サンプル)。

   [ ![](settings-images/settings-appgroup-sml.png "Root.plist ApplicationGroupContainerIdentifier キーを追加します。")](settings-images/settings-appgroup.png#lightbox)

4. 編集、 **Settings-Watch.bundle/Root.plist**テンプレート ファイルには - 使用するオプションを格納するには、グループが含まれています。
  テキスト フィールド、トグル スイッチおよびスライダーが既定で (これを削除して、独自の設定に置き換えてください)。

  [![](settings-images/rootplist-sml.png "Settings-Watch.bundle/Root.plist を編集します。")](settings-images/rootplist.png#lightbox)


## <a name="use-settings-in-the-watch-app"></a>Watch アプリの設定を使用します。

ユーザーが選択した値にアクセスするには、作成、`NSUserDefaults`アプリ グループを使用し、指定のインスタンス`NSUserDefaultsType.SuiteName`:

```csharp
NSUserDefaults shared = new NSUserDefaults(
    "group.com.xamarin.WatchSettings",
    NSUserDefaultsType.SuiteName);

var isEnabled = shared.BoolForKey ("enabled_preference");
var userName = shared.StringForKey ("name_preference");
```

## <a name="apple-watch-app"></a>Apple Watch アプリ

[![](settings-images/settings-app-sml.png "IPhone の新しい Apple Watch アプリ")](settings-images/settings-app.png#lightbox)

ユーザーが使用して、新しい設定**Apple Watch** iPhone 上のアプリ。 このアプリには、視聴、およびも編集を使用して、設定が公開でアプリの表示/非表示をユーザーができるように、**設定 Watch.bundle**します。

![](settings-images/applewatch-1.png "アプリの設定例") ![](settings-images/applewatch-2.png "アプリ設定の例")



## <a name="related-links"></a>関連リンク

- [Apple の設定のドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
