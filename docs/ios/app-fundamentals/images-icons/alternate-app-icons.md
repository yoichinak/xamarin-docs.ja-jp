---
title: Xamarin. iOS の別のアプリアイコン
description: このドキュメントでは、Xamarin. iOS で別のアプリアイコンを使用する方法について説明します。 これらのアイコンを Xamarin の iOS プロジェクトに追加する方法、情報の plist ファイルを変更する方法、およびプログラムによってアプリのアイコンを管理する方法について説明します。
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: e90ee165073dbbe792e4ca1916463517ad86255d
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572300"
---
# <a name="alternate-app-icons-in-xamarinios"></a>Xamarin. iOS の別のアプリアイコン

_この記事では、Xamarin の別のアプリアイコンの使用について説明します。_

Apple では、アプリによるアイコンの管理を可能にする iOS 10.3 の機能強化がいくつか追加されています。

- `ApplicationIconBadgeNumber`-スプリングボードのアプリアイコンのバッジを取得または設定します。
- `SupportsAlternateIcons`- `true` アプリに別のアイコンのセットがある場合。
- `AlternateIconName`-現在選択されている代替アイコンの名前を返し `null` ます。プライマリアイコンを使用する場合はを返します。
- `SetAlternameIconName`-アプリのアイコンを指定した代替アイコンに切り替えるには、このメソッドを使用します。

![](alternate-app-icons-images/icons04.png "A sample alert when an app changes its icon")

<a name="Adding-Alternate-Icons"></a>

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Xamarin. iOS プロジェクトへの代替アイコンの追加

アプリが別のアイコンに切り替えることができるようにするには、アイコンイメージのコレクションを Xamarin. iOS アプリプロジェクトに含める必要があります。 これらのイメージは、通常のメソッドを使用してプロジェクトに追加することはできません `Assets.xcassets` 。**リソース**フォルダーに直接追加する必要があります。

次の操作を行います。

1. フォルダー内の必要なアイコンイメージを選択し、[すべて] を選択して、**ソリューションエクスプローラー**の [**リソース**] フォルダーにドラッグします。

    ![](alternate-app-icons-images/icons00.png "Select the icons images from a folder")

2. メッセージが表示されたら、[**コピー**] を選択し、**選択したすべてのファイルに同じアクションを使用**して、[ **OK** ] ボタンをクリックします。

    ![](alternate-app-icons-images/icons02.png "The Add File to Folder dialog box")

3. 完了すると、 **Resources**フォルダーは次のようになります。

    ![](alternate-app-icons-images/icons01.png "The Resources folder should look like this")

<a name="Modifying-the-Info.plist-File"></a>

## <a name="modifying-the-infoplist-file"></a>情報の plist ファイルの変更

**リソース**フォルダーに必要なイメージを追加したら、 [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13)キーをプロジェクトの**情報の plist**ファイルに追加する必要があります。 このキーは、新しいアイコンの名前とそれを構成するイメージを定義します。

次の操作を行います。

1. **ソリューション エクスプローラー**で、**Info.plist** ファイルをダブルクリックして開き、編集します。
2. **ソース**ビューに切り替えます。
3. **バンドルアイコン**キーを追加し、**型**を**Dictionary**に設定したままにします。
4. キーを追加 `CFBundleAlternateIcons` し、**型**を**Dictionary**に設定します。
5. キーを追加 `AppIcon2` し、**型**を**Dictionary**に設定します。 これは、新しいアプリアイコンセットの名前になります。
6. キーを追加 `CFBundleIconFiles` し、**型**を**配列**に設定します。
7. 拡張機能と、、など `CFBundleIconFiles` の `@2x` `@3x` サフィックス (例) を除いて、各アイコンファイルの配列に新しい文字列を追加し `100_icon` ます。 代替アイコンセットを構成するすべてのファイルに対して、この手順を繰り返します。
8. `UIPrerenderedIcon`ディクショナリにキーを追加し `AppIcon2` 、[**型**] を [**ブール**] に設定し、値を [**いいえ**] に設定します。
9. 変更内容をファイルに保存します。

完成した**情報の plist**ファイルは、次のようになります。

![](alternate-app-icons-images/icons03.png "The completed Info.plist file")

または、テキストエディターで開かれた場合は、次のようになります。

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

<a name="Managing-the-Apps-Icon"></a>

## <a name="managing-the-apps-icon"></a>アプリのアイコンを管理する 

Xamarin. iOS プロジェクトに含まれるアイコンイメージと、適切に構成された**情報**ファイルを使用して、開発者は ios 10.3 に追加された多くの新機能のいずれかを使用して、アプリのアイコンを制御できます。

`SupportsAlternateIcons`クラスのプロパティに `UIApplication` より、開発者は、アプリで代替アイコンがサポートされているかどうかを確認できます。 次に例を示します。

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber`クラスのプロパティを `UIApplication` 使用すると、開発者はスプリングボードのアプリアイコンの現在のバッジ番号を取得または設定できます。 既定値は 0 です。 次に例を示します。

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName`クラスのプロパティを使用すると、 `UIApplication` 開発者は現在選択されている代替アプリのアイコンの名前を取得でき `null` ます。また、アプリがプライマリアイコンを使用している場合は、を返します。 次に例を示します。

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName`クラスのプロパティを `UIApplication` 使用すると、開発者はアプリアイコンを変更できます。 アイコンの名前を渡して、選択するか、 `null` プライマリアイコンに戻ります。 次に例を示します。

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

アプリが実行され、ユーザーが別のアイコンを選択すると、次のようなアラートが表示されます。

![](alternate-app-icons-images/icons04.png "A sample alert when an app changes its icon")

ユーザーがプライマリアイコンに戻ると、次のようなアラートが表示されます。

![](alternate-app-icons-images/icons05.png "A sample alert when an app changes to the primary icon")

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、別のアプリアイコンを Xamarin の iOS プロジェクトに追加し、アプリ内で使用する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [iOSTenThree サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
