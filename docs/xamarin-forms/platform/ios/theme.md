---
title: IOS に固有の書式設定を追加します。
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 280ca523d3e3b4f5037d626cc5fd0bd5b31d0e8b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="adding-ios-specific-formatting"></a>IOS に固有の書式設定を追加します。

IOS に固有の設定方法の 1 つの書式設定を作成するが、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)のコントロールとプラットフォームごとに色のセットのプラットフォームに固有のスタイル。

その他のオプション、Xamarin.Forms iOS アプリの外観を含める方法を制御するには:

* オプションが表示を構成する[ **Info.plist**](#info-plist)
* 使用してコントロールのスタイルを設定、 [ `UIAppearance` API](#uiappearance)

これらの代替方法を次に説明します。

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Info.plist のカスタマイズ

**Info.plist**ファイルでは、ステータス バーが表示される方法 (およびかどうか) などの iOS アプリケーションの renderering の一部の機能を構成できます。

たとえば、 [Todo サンプル](https://developer.xamarin.com/samples/xamarin-forms/Todo/)すべてのプラットフォームで、ナビゲーション バーの色とテキストの色を設定する次のコードを使用します。

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

結果は、次のスニペット画面に表示されます。 ステータス バーの項目は黒色ことに注意してください (これは設定できません Xamarin.Forms 内で、プラットフォーム固有の機能になっているため)。

![](theme-images/status-default-sml.png "iOS テーマ")

理想的には、ステータス バーも空白にする iOS プロジェクトで直接実行できるもの。 次のエントリを追加、 **Info.plist**白でステータス バーを強制します。

![](theme-images/info-plist.png "iOS Info.plist Entries")

または、対応する編集**Info.plist**のためファイルを直接含めます。

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

ナビゲーション バーが緑色であり、テキストの色は白 (Xamarin.Forms は、次の形式) のため、アプリの実行時に*と*ステータス バーのテキストも iOS に固有の構成に白いありがとうございました。

![](theme-images/status-white-sml.png "iOS テーマ")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) iOS の多くのコントロールのビジュアル プロパティを設定することできます*せず*を作成すること、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)です。

コードの 1 つの行を追加する、 **<code>appdelegate.cs</code>** `FinishedLaunching`メソッドを使用して、指定された型のすべてのコントロールにスタイルを設定、`Appearance`プロパティです。 次のコードには、グローバルに、タブのスタイル設定の 2 つの例が含まれています。 バーと管理をスイッチ。

**<code>appdelegate.cs</code>** Ios プロジェクト

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

既定では、選択されているタブ バーのアイコン、 [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)青になります。

![](theme-images/tabbar-default.png "既定の iOS TabbedPage タブ バーのアイコン")

この動作を変更するには、設定、`UITabBar.Appearance`プロパティ。

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

これは、緑を選択したタブをによりします。

![](theme-images/tabbar-custom.png "緑 iOS TabbedPage タブ バーのアイコン")

この API を使用すると、Xamarin.Forms の外観をカスタマイズできます`TabbedPage`非常にわずかなコードでの iOS でします。 参照してください、[タブのカスタマイズのレシピ](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/)カスタム レンダラーを使用してタブの特定のフォントの設定の詳細についてはします。

### <a name="uiswitch"></a>UISwitch

`Switch`コントロールが簡単にスタイルを設定できる別の例を示します。

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

これら 2 つの画面キャプチャは、既定値を表示する`UISwitch`左に、カスタマイズされたバージョン コントロール (設定`Appearance`) の右側、 [Todo サンプル](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "既定の UISwitch 色") ![ ] (theme-images/switch-custom.png "UISwitch 色のカスタマイズ")

### <a name="other-controls"></a>その他のコントロール

IOS ユーザー インターフェイスの多くのコントロールは、既定の色とその他の属性を使用して設定を持つことができます、 [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)です。



## <a name="related-links"></a>関連リンク

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [タブをカスタマイズします。](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/)
