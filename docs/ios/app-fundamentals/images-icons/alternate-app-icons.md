---
title: 代替アプリのアイコン
description: この記事では、Xamarin.iOS で代替アプリ アイコンの使用について説明します。
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 8d9f27d58a881878aabeda4326805eec726c247c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="alternate-app-icons"></a>代替アプリのアイコン

_この記事では、Xamarin.iOS で代替アプリ アイコンの使用について説明します。_

Apple では、そのアイコンを管理するアプリを許可する iOS 10.3 にいくつかの機能強化が追加します。

 - `ApplicationIconBadgeNumber` -を取得またはのスプリング ボードで、アプリ アイコンのバッジを設定します。
 - `SupportsAlternateIcons` If`true`アプリは別のアイコンのセット。
 - `AlternateIconName` 現在選択されている別のアイコンの名前を返しますまたは`null`プライマリ アイコンを使用する場合。
 - `SetAlternameIconName` -指定した代替アイコンに、アプリのアイコンを切り替えるには、このメソッドを使用します。

![](alternate-app-icons-images/icons04.png "アプリのアイコンが変更されたときに、サンプル アラート")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>代替のアイコンを Xamarin.iOS プロジェクトに追加します。

代替のアイコンに切り替えるには、アプリを許可するのには、アイコンのイメージのコレクションは、Xamarin.iOS アプリ プロジェクトに含まれる必要があります。 これらのイメージを使用して、一般的なプロジェクトに追加することはできません`Assets.xcassets`メソッドを追加しなければなりませんを**リソース**直接フォルダーです。

次の手順で行います。

1. フォルダーに [必須] アイコンの画像を選択して、すべてを選択して、ドラッグして、**リソース**内のフォルダー、**ソリューション エクスプ ローラー**:

    ![](alternate-app-icons-images/icons00.png "フォルダーからアイコン イメージを選択します。")

2. メッセージが表示されたら、[**コピー**、**同じアクションを使用して、選択したすべてのファイルの**] をクリックし、 **[ok]**ボタン。

    ![](alternate-app-icons-images/icons02.png "ファイル フォルダ ダイアログ ボックスへの追加")

3. **リソース**フォルダーが完了したときに、次のようになります。

    ![](alternate-app-icons-images/icons01.png "リソース フォルダーは次のようになります")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Info.plist ファイルを変更します。

追加の必要なイメージと、**リソース**、フォルダー、 [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13)キーは、プロジェクトに追加する必要があります**Info.plist**ファイル。 このキーは、新規アイコンとそれを構成するイメージの名前を定義します。

次の手順で行います。

1. **ソリューション エクスプローラー**で、**Info.plist** ファイルをダブルクリックして開き、編集します。
2. 切り替えて、**ソース**ビュー。
3. 追加、**アイコンをバンドル**キーし、のままにして、**型**に設定**ディクショナリ**です。
4. 追加、`CFBundleAlternateIcons`キーし、設定、**型**に**ディクショナリ**です。
5. 追加、`AppIcons2`キーし、設定、**型**に**ディクショナリ**です。 新しい代替アプリ アイコン セットの名前になります。
6. 追加、`CFBundleIconFiles`キーし、設定、**型**に**配列**
7. 新しい文字列を追加、`CFBundleIconFiles`配列の各アイコン ファイルの拡張子は除外され、 `@2x`、`@3x`などのサフィックス (例`100_icon`)。 すべてのファイルの代替アイコン セットを構成するには、この手順を繰り返します。
8. 追加、`UIPrerenderedIcon`キーを`AppIcons2`ディクショナリ、設定、**型**に**ブール**し、値を**いいえ**です。
9. 変更内容をファイルに保存します。

その結果、 **Info.plist**ファイルが完了したときに、次のようになります。

![](alternate-app-icons-images/icons03.png "完成した Info.plist ファイル")

または、テキスト エディターで開かれている場合、このような。

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon" />

## <a name="managing-the-apps-icon"></a>アプリのアイコンの管理 

Xamarin.iOS プロジェクトに含まれる、アイコン イメージを使用し、 **Info.plist**ファイルが正しく構成されて、開発者が使用できる iOS 10.3 に追加された多くの新機能のいずれかをアプリのアイコンを制御します。

`SupportsAlternateIcons`のプロパティ、`UIApplication`クラスにより、開発者は、アプリが代替のアイコンをサポートしているかを参照してください。 例えば:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber`のプロパティ、`UIApplication`クラスにより、開発者を取得またはのスプリング ボードで、アプリ アイコンの現在のバッジ番号を設定します。 既定値は、ゼロ (0) です。 例えば:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName`のプロパティ、`UIApplication`クラスにより、開発者は、現在選択されている代替アプリ アイコンの名前を取得するかを返します`null`アプリがプライマリのアイコンを使用している場合。 例えば:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName`のプロパティ、`UIApplication`クラスにより、開発者はアプリ アイコンを変更します。 選択するアイコンの名前を渡すまたは`null`プライマリ アイコンに戻ります。 例えば:

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

アプリが実行され、ユーザーが代替アイコンを選択して、ときに、次のような警告が表示されます。

![](alternate-app-icons-images/icons04.png "アプリのアイコンが変更されたときに、サンプル アラート")

プライマリのアイコンに切り替える場合は、次のようなアラートが表示されます。

![](alternate-app-icons-images/icons05.png "アプリがプライマリのアイコンに変化したときに、サンプル アラート")

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事は、代替アプリ アイコンを Xamarin.iOS プロジェクトに追加して、アプリ内で使用するについて説明しました。



## <a name="related-links"></a>関連リンク

- [iOSTenThree サンプル](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
