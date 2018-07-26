---
title: IOS 固有の書式設定を追加します。
description: この記事では、Xamarin.Forms カスタム レンダラーを使用せず、iOS 固有の外観を設定する方法について説明します。
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 3b8a440617dedfbe23f869e865b3cedae21d6c5b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241378"
---
# <a name="adding-ios-specific-formatting"></a>IOS 固有の書式設定を追加します。

IOS 固有の設定方法の 1 つの書式設定を作成するが、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)コントロールおよびプラットフォーム固有のスタイルの設定、各プラットフォーム用の色。

その他のオプション、Xamarin.Forms の iOS アプリの外観を含める方法を制御するには:

* オプションを表示する構成[ **Info.plist**](#info-plist)
* 使用してコントロールのスタイルを設定、 [ `UIAppearance` API](#uiappearance)

これらの方法を以下に示します。

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Info.plist をカスタマイズします。

**Info.plist**ファイルでは、ステータス バーが表示される方法 (とかどうか) などの iOS アプリケーションの renderering の一部の側面を構成することができます。

たとえば、 [Todo サンプル](https://developer.xamarin.com/samples/xamarin-forms/Todo/)次のコードを使用して、すべてのプラットフォームでのナビゲーション バーの色とテキストの色を設定します。

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

結果は、次のスニペットの画面に表示されます。 ステータス バーの項目が黒ことに注意してください (これは設定できません Xamarin.Forms 内でプラットフォーム固有機能であるため)。

![](theme-images/status-default-sml.png "iOS のテーマ")

理想的には、ステータス バーもなります白 - iOS プロジェクトで直接実行できます何か。 次のエントリを追加、 **Info.plist**白でステータス バーを強制します。

![](theme-images/info-plist.png "iOS Info.plist エントリ")

または、対応する編集**Info.plist**直接に含めるファイル。

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

ナビゲーション バーが緑色であり、テキストが白 (Xamarin.Forms の書式設定) のため、アプリが実行されるようになりました*と*ステータス バーのテキストも iOS 固有の構成に白いありがとうございます。

![](theme-images/status-white-sml.png "iOS のテーマ")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) iOS の多くのコントロールのビジュアルのプロパティを設定することできます*せず*を作成すること、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)します。

コードの 1 つの行を追加、 **AppDelegate.cs** `FinishedLaunching`メソッドを使用して、指定した型のすべてのコントロールのスタイル、`Appearance`プロパティ。 次のコードには、2 つの例 - グローバルに、タブのスタイル設定が含まれています。 横棒グラフとスイッチを制御します。

**AppDelegate.cs**で iOS プロジェクト

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>UITabBar

既定で選択されているタブ バーのアイコン、 [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)青になります。

![](theme-images/tabbar-default.png "既定の iOS TabbedPage タブ バーのアイコン")

この動作を変更するには、設定、`UITabBar.Appearance`プロパティ。

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

これにより、選択したタブが緑色にします。

![](theme-images/tabbar-custom.png "緑の iOS TabbedPage タブ バーのアイコン")

この API を使用すると、Xamarin.Forms の外観をカスタマイズできます`TabbedPage`ごくわずかなコードを iOS にします。 参照してください、[タブのカスタマイズのレシピ](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)のカスタム レンダラーを使用して、タブの特定のフォントを設定する詳細についてはします。

### <a name="uiswitch"></a>UISwitch

`Switch`コントロールは簡単にスタイルを設定できるもう 1 つの例です。

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

これら 2 つの画面キャプチャは、既定値を表示する`UISwitch`左に、カスタマイズされたバージョン コントロール (設定`Appearance`) の右側、 [Todo サンプル](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "既定の色の UISwitch") ![ ] (theme-images/switch-custom.png "UISwitch 色のカスタマイズ")

### <a name="other-controls"></a>その他のコントロール

IOS ユーザー インターフェイス コントロールの多くは、既定の色とその他の属性を使用して設定を持つことができます、 [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)します。



## <a name="related-links"></a>関連リンク

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [タブをカスタマイズします。](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
