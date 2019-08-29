---
title: IOS 固有の書式設定の追加
description: この記事では、Xamarin. Forms カスタムレンダラーを使用せずに、iOS 固有の外観を設定する方法について説明します。
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: be353c6274dcf69946740e2d195b9e4d64208313
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121571"
---
# <a name="adding-ios-specific-formatting"></a>IOS 固有の書式設定の追加

IOS 固有の書式設定を行う1つの方法は、コントロールの[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)を作成し、プラットフォームごとにプラットフォーム固有のスタイルと色を設定することです。

次に、Xamarin の iOS アプリの外観の方法を制御するためのオプションを示します。

- [**情報**](#info-plist)の表示オプションの構成
- API を使用したコントロールスタイルの設定[ `UIAppearance`](#uiappearance)

これらの代替方法については、以下で説明します。

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>情報 plist のカスタマイズ

**情報 plist**ファイルを使用すると、ステータスバーを表示する方法 (およびその有無) など、iOS アプリケーションのさまざまな機能を構成できます。

たとえば、 [Todo サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)では、次のコードを使用して、すべてのプラットフォームのナビゲーションバーの色とテキストの色を設定します。

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

結果は、次の画面スニペットに示されています。 ステータスバーの項目は黒であることに注意してください (これはプラットフォーム固有の機能であるため、Xamarin では設定できません)。

![](theme-images/status-default-sml.png "iOS のテーマ")

理想的には、ステータスバーは、iOS プロジェクトで直接実行できる問題でもあります。 次のエントリを**情報 plist**に追加して、ステータスバーを強制的に白にします。

![](theme-images/info-plist.png "iOS 情報 plist エントリ")

または、対応する**情報の plist**ファイルを直接編集して、次の内容を含めます。

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

これでアプリが実行されると、ナビゲーションバーは緑色になり、テキストは白になり (Xamarin 形式の書式設定による)、iOS 固有の構成によりステータスバーのテキストも白になります。

![](theme-images/status-white-sml.png "iOS のテーマ")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)を使用すると、[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)を作成し*なく*ても、多くの iOS コントロールでビジュアルプロパティを設定できます。

`Appearance` AppDelegate.cs`FinishedLaunching`メソッドに1行のコードを追加すると、そのプロパティを使用して、特定の種類のすべてのコントロールのスタイルを設定できます。 次のコードには、タブバーとスイッチコントロールをグローバルにスタイル設定するという2つの例が含まれています。

IOS プロジェクトの**AppDelegate.cs**

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

既定では、で選択されたタブバーアイコンが[`TabbedPage`](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
青になります。

![](theme-images/tabbar-default.png "TabbedPage の既定の iOS タブバーアイコン")

この動作を変更するには`UITabBar.Appearance` 、次のようにプロパティを設定します。

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

これにより、選択したタブが緑色になります。

![](theme-images/tabbar-custom.png "TabbedPage の緑の [iOS] タブバーアイコン")

この API を使用すると、非常に少ないコードで、 `TabbedPage` iOS 上の Xamarin. Forms の外観をカスタマイズできます。 カスタムレンダラーを使用してタブに特定のフォントを設定する方法の詳細については、[タブのカスタマイズ](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)に関するレシピを参照してください。

### <a name="uiswitch"></a>UISwitch

コントロール`Switch`は、簡単にスタイルを設定できるもう1つの例です。

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

次の2つの画面キャプチャ`UISwitch`では、既定のコントロールが左側に表示`Appearance`され、カスタマイズされたバージョン (設定) が[Todo サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)の右側に表示されます。

![](theme-images/switch-default.png "既定の色の UISwitch") ![](theme-images/switch-custom.png "UISwitch 色のカスタマイズ")

### <a name="other-controls"></a>その他のコントロール

多くの iOS ユーザーインターフェイスコントロールは、 [ `UIAppearance` ](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)既定の色やその他の属性を API を使用して設定できます。



## <a name="related-links"></a>関連リンク

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [タブのカスタマイズ](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
