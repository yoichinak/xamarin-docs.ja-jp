---
title: IOS 固有の書式設定の追加
description: この記事では、カスタムレンダラーを使用せずに iOS 固有の外観を設定する方法について説明 Xamarin.Forms します。
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f447a89ca4b4f21554a75ec52c5771ee9f9d35fd
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562991"
---
# <a name="adding-ios-specific-formatting"></a>IOS 固有の書式設定の追加

IOS 固有の書式設定を行う1つの方法は、コントロールの [カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) を作成し、プラットフォームごとにプラットフォーム固有のスタイルと色を設定することです。

IOS アプリの外観を制御する他のオプションは Xamarin.Forms 次のとおりです。

- 情報の表示オプションの構成[ **Info.plist**](#customizing-infoplist)
- [ `UIAppearance` API](#uiappearance-api)を使用したコントロールスタイルの設定

これらの代替方法については、以下で説明します。

## <a name="customizing-infoplist"></a>情報 plist のカスタマイズ

**情報 plist**ファイルを使用すると、ステータスバーを表示する方法 (およびその有無) など、iOS アプリケーションのさまざまな機能を構成できます。

たとえば、 [Todo サンプル](/samples/xamarin/xamarin-forms-samples/todo) では、次のコードを使用して、すべてのプラットフォームのナビゲーションバーの色とテキストの色を設定します。

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

結果は、次の画面スニペットに示されています。 ステータスバーの項目は黒であることに注意してください (これ Xamarin.Forms はプラットフォーム固有の機能であるため、この項目を設定することはできません)。

![iOS のテーマ](theme-images/status-default-sml.png)

理想的には、ステータスバーは、iOS プロジェクトで直接実行できる問題でもあります。 次のエントリを **情報 plist** に追加して、ステータスバーを強制的に白にします。

![iOS 情報 plist エントリ](theme-images/info-plist.png)

または、対応する **情報の plist** ファイルを直接編集して、次の内容を含めます。

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

アプリを実行すると、ナビゲーションバーは緑色になり、テキストは (書式設定により) 白になり Xamarin.Forms ます。また、iOS 固有の構成により、ステータスバー *の* テキストも白になります。

![iOS のテーマ](theme-images/status-white-sml.png)

## <a name="uiappearance-api"></a>UIAppearance API

[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)を使用すると、[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)を作成し*なく*ても、多くの iOS コントロールでビジュアルプロパティを設定できます。

**AppDelegate.cs**メソッドに1行のコードを追加すると、 `FinishedLaunching` そのプロパティを使用して、特定の種類のすべてのコントロールのスタイルを設定でき `Appearance` ます。 次のコードには、タブバーとスイッチコントロールをグローバルにスタイル設定するという2つの例が含まれています。

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

既定では、で選択されたタブバーアイコンが [`TabbedPage`](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
青になります。

![TabbedPage の既定の iOS タブバーアイコン](theme-images/tabbar-default.png)

この動作を変更するには、次のようにプロパティを設定し `UITabBar.Appearance` ます。

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

これにより、選択したタブが緑色になります。

![TabbedPage の緑の [iOS] タブバーアイコン](theme-images/tabbar-custom.png)

この API を使用すると、次のような外観をカスタマイズできます。 Xamarin.Forms
`TabbedPage` コードがほとんどない iOS の場合。 カスタムレンダラーを使用してタブに特定のフォントを設定する方法の詳細については、 [タブのカスタマイズ](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) に関するレシピを参照してください。

### <a name="uiswitch"></a>UISwitch

コントロールは、 `Switch` 簡単にスタイルを設定できるもう1つの例です。

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

次の2つの画面キャプチャでは、既定のコントロールが左側に表示され、カスタマイズされた `UISwitch` バージョン (設定 `Appearance` ) が [Todo サンプル](/samples/xamarin/xamarin-forms-samples/todo)の右側に表示されます。

![既定の UISwitch の色](theme-images/switch-default.png) ![カスタマイズされた UISwitch の色](theme-images/switch-custom.png)

### <a name="other-controls"></a>その他の制御

多くの iOS ユーザーインターフェイスコントロールは、既定の色やその他の属性を[ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)を使用して設定できます。

## <a name="related-links"></a>関連リンク

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [タブのカスタマイズ](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)