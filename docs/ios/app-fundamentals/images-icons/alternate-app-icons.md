---
title: Xamarin.iOS での代替アプリ アイコン
description: このドキュメントでは、Xamarin.iOS で代替アプリ アイコンを使用する方法について説明します。 これは、Xamarin.iOS プロジェクトにこれらのアイコンを追加する方法、Info.plist ファイルを変更する方法、およびアプリのアイコンをプログラムで管理する方法について説明します。
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 3f009d84d1d38c379f65de52949c66f3e86fc654
ms.sourcegitcommit: ffb0f3dbf77b5f244b195618316bbd8964541e42
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2018
ms.locfileid: "39275990"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Xamarin.iOS での代替アプリ アイコン

_この記事では、Xamarin.iOS での代替アプリ アイコンの使用について説明します。_

Apple には、そのアイコンを管理するアプリを許可する iOS 10.3 にいくつかの機能強化が追加されます。

 - `ApplicationIconBadgeNumber` -を取得します。 または、スプリング ボードで、アプリ アイコンのバッジを設定します。
 - `SupportsAlternateIcons` If`true`アプリには、別のアイコンのセット。
 - `AlternateIconName` -現在選択されている代替アイコンの名前を返しますまたは`null`プライマリ アイコンを使用する場合。
 - `SetAlternameIconName` -このメソッドを使用してアプリのアイコンを指定した代替アイコンに切り替えます。

![](alternate-app-icons-images/icons04.png "アプリのアイコンの変更されたときにアラートの例")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>代替アイコンを Xamarin.iOS プロジェクトに追加します。

代替アイコンに切り替えるアプリを許可するのには、アイコン イメージのコレクションは、Xamarin.iOS アプリ プロジェクトに含まれる必要があります。 これらのイメージは、一般的なを使用してプロジェクトに追加することはできません`Assets.xcassets`を追加、メソッド、**リソース**直接フォルダー。

次の手順で行います。

1. フォルダー内の必要なアイコンのイメージを選択、すべてを選択およびにドラッグ、**リソース**フォルダーで、**ソリューション エクスプ ローラー**:

    ![](alternate-app-icons-images/icons00.png "フォルダーのアイコン イメージを選択します。")

2. メッセージが表示されたら、[**コピー**、**同じアクションを使用して、選択したすべてのファイルの**] をクリックし、 **[ok]** ボタン。

    ![](alternate-app-icons-images/icons02.png "ファイル フォルダ ダイアログ ボックスへの追加")

3. **リソース**フォルダーは、完了したときに、次のようになります。

    ![](alternate-app-icons-images/icons01.png "[リソース] フォルダーは、次のようになります")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Info.plist ファイルの変更

追加の必要なイメージと、**リソース**フォルダー、 [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13)キーは、プロジェクトに追加する必要があります**Info.plist**ファイル。 このキーは、新しいアイコンとそれを構成するイメージの名前を定義します。

次の手順で行います。

1. **ソリューション エクスプローラー**で、**Info.plist** ファイルをダブルクリックして開き、編集します。
2. 切り替えて、**ソース**ビュー。
3. 追加、**バンドル アイコン**キーし、のままに、**型**に設定**ディクショナリ**します。
4. 追加、`CFBundleAlternateIcons`キーし、設定、**型**に**ディクショナリ**します。
5. 追加、`AppIcon2`キーし、設定、**型**に**ディクショナリ**します。 これにより、新しい代替アプリ アイコン セットの名前になります。
6. 追加、`CFBundleIconFiles`キーし、設定、**型**に**配列**
7. 新しい文字列を追加、`CFBundleIconFiles`配列の各アイコン ファイルの拡張子を除外し、 `@2x`、`@3x`などのサフィックス (例`100_icon`)。 代替アイコン セットを構成するすべてのファイルに対してこの手順を繰り返します。
8. 追加、`UIPrerenderedIcon`キーを`AppIcon2`ディクショナリ、設定、**型**に**ブール**し、値を**いいえ**。
9. 変更内容をファイルに保存します。

その結果、 **Info.plist**ファイルが完了すると、次のようになります。

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

## <a name="managing-the-apps-icon"></a>アプリのアイコンを管理します。 

Xamarin.iOS プロジェクトに含まれるアイコン イメージを使用し、 **Info.plist**ファイルが正しく構成されている、開発者 iOS 10.3 に追加された多数の新機能のいずれかの制御に使用できるアプリのアイコン。

`SupportsAlternateIcons`のプロパティ、`UIApplication`クラスにより、開発者はアプリが別のアイコンをサポートしているかを参照してください。 例えば:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber`のプロパティ、`UIApplication`クラスにより、開発者は取得、または、スプリング ボードで現在のアプリ アイコンのバッジの数を設定します。 既定値は、ゼロ (0) です。 例えば:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName`のプロパティ、`UIApplication`クラスにより、開発者は、現在選択されている代替アプリ アイコンの名前を取得または返す`null`アプリは、プライマリのアイコンを使用している場合。 例えば:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName`のプロパティ、`UIApplication`クラスにより、開発者はアプリ アイコンに変更します。 選択するアイコンの名前を渡すまたは`null`プライマリ アイコンに戻ります。 例えば:

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

アプリの実行し、ユーザーが代替アイコンを選択して、次のようにアラートが表示されます。

![](alternate-app-icons-images/icons04.png "アプリのアイコンの変更されたときにアラートの例")

プライマリのアイコンに切り替える場合は、次のようなアラートが表示されます。

![](alternate-app-icons-images/icons05.png "アプリがプライマリのアイコンに変更されたときにアラートの例")

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、代替アプリ アイコンを Xamarin.iOS プロジェクトに追加して、アプリ内でそれらを使用してについて説明しました。



## <a name="related-links"></a>関連リンク

- [iOSTenThree サンプル](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
