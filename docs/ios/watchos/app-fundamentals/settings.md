---
title: "設定の操作"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B2EB192-F0A2-4010-B141-0431520594C0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 3e699fdc2092d17834c348c07f2440e40441ad86
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-settings"></a>設定の操作

Apple Watch アプリは iOS アプリとして設定と同じ機能を使用できます - で、[設定] ユーザー インターフェイスが表示されます、 **Apple Watch** iPhone アプリが、値は、iPhone アプリとの両方もウォッチ拡張機能にアクセスします。

![](settings-images/intro.png "Apple Watch アプリは iOS アプリとして設定と同じ機能を使用することができます。")

設定は、iOS アプリとによって定義された、ウォッチ アプリ拡張機能の両方にアクセスできる共有ファイルの場所に格納されます、**アプリ グループ**です。 行う必要があります[アプリ グループを構成する](~/ios/watchos/app-fundamentals/app-groups.md)以下の手順を使用して設定を追加する前にします。

## <a name="add-settings-in-a-watch-solution"></a>ウォッチ ソリューションで設定を追加します。

**IPhone アプリ**、ソリューション (*いない*watch アプリまたは拡張機能)。

1. 右クリック**追加 > 新しいファイル.**選択**Settings.bundle** (内の名前を編集することはできません、**新しいファイル**ダイアログ)。

   [![](settings-images/settings-add-sml.png "新しい設定バンドルを追加します。")](settings-images/settings-add.png#lightbox)

2. 名前を変更**設定 Watch.bundle** (を選択し、入力**コマンド + R**名前を変更する)。

   ![](settings-images/settings-rename.png "バンドルの名前を変更します。")

3. 新しいキーを追加`ApplicationGroupContainerIdentifier`を**Root.plist**値を構成したら、(アプリ グループを設定して `group.com.xamarin.WatchSettings` サンプルでは、)。

   [ ![](settings-images/settings-appgroup-sml.png "ApplicationGroupContainerIdentifier キー、Root.plist への追加します。")](settings-images/settings-appgroup.png#lightbox)

4. 編集、 **Settings-Watch.bundle/Root.plist**テンプレート ファイルには - 使用するオプションを格納するには、グループが含まれています。
  textfield、トグル スイッチおよびスライダーを既定では (これを削除して、独自の設定を置き換えます):

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

## <a name="apple-watch-app"></a>Apple Watch App

[![](settings-images/settings-app-sml.png "IPhone 上の新しい Apple Watch アプリ")](settings-images/settings-app.png#lightbox)

ユーザーが新しいを介して設定**Apple Watch** iPhone 上のアプリです。 このアプリがユーザーに視聴、およびもを使用して、設定が公開される編集上のアプリの表示/非表示を許可、**設定 Watch.bundle**です。

![](settings-images/applewatch-1.png "アプリの設定例") ![ ](settings-images/applewatch-2.png "アプリ設定の例")



## <a name="related-links"></a>関連リンク

- [Apple の設定のドキュメント](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/Settings.html#//apple_ref/doc/uid/TP40014969-CH22-SW1)
