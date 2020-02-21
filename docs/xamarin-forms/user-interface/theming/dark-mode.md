---
title: Xamarin. Forms アプリケーションでのダークモードの検出
description: ダークモードは、ResourceDictionaries、DynamicResources、および platform knowledge の組み合わせを使用するすべての Xamarin. Forms アプリケーションでサポートされています。
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/19/2020
ms.openlocfilehash: 7136e3240a39321b2d67ca29c16a0758cf5c4cfb
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77486290"
---
# <a name="detect-dark-mode-in-xamarinforms-applications"></a>Xamarin. Forms アプリケーションでのダークモードの検出

Xamarin アプリケーションは、テーマ[をサポートする](theming.md)のと同じ方法を使用して、オペレーティングシステムのテーマの変更に応答できます。 主な違いは、テーマの変更がトリガーされる方法です。 ダークモードとは、オペレーティングシステムレベルで設定できるより広範な外観設定を指します。また、すぐに考慮して応答する必要があるアプリケーションを示します。

Xamarin. フォームアプリケーションでダークモードを使用するための最小要件は次のとおりです。

- iOS 13 以上。
- Android 9 以降 (Android 9 では、クエリ可能な表示モードがサポートされています)。
- Android 10 以上 (Android 10 は、モードの変更をアプリに通知します)。
- UWP v 10.0.10240.0 以上。
- Xamarin. フォーム (任意のバージョン)。

ライトとダークを含む表示モードをサポートするプロセスは次のとおりです。

1. アプリケーション全体で共有されるリソースを[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)で定義します。
2. 変更する各スタイルに固有のオーバーライドを提供する、表示モードごとにリソースディクショナリのテーマを定義します。
3. アプリケーションの**app.xaml**ファイルで、既定の外観モードのテーマを設定します。
4. 外観モードが変更されたときにスタイルを反応させる `DynamicResource` マークアップ拡張機能を使用して、アプリケーションのスタイルを設定します。
5. OS テーマの変更をリッスンし、アプリケーションの対応するテーマを読み込むコードを追加します。

次のスクリーンショットは、明るいモードとダークモードのテーマが適用されたページを示しています。

テーマが適用さ[![れたアプリのメインページのスクリーンショット (ios および
android](theming-images/main-page-both-themes.png "テーマ付きアプリのメインページ")](theming-images/main-page-both-themes-large.png#lightbox "テーマ付きアプリのメインページ")の場合)、テーマが適用さ[![れたアプリの詳細ページのスクリーンショット (ios および android)](theming-images/detail-page-both-themes.png "テーマ付きアプリの詳細ページ")](theming-images/detail-page-both-themes-large.png#lightbox "テーマ付きアプリの詳細ページ")

## <a name="define-themes"></a>テーマを定義する

ダークテーマの作成に関する詳細な手順については、「[テーマガイド」](theming.md)を参照してください。

## <a name="react-to-appearance-mode-changes"></a>外観モードの変更に対処する

デバイスの表示モードは、ユーザーが設定をどのように構成しているかに応じて、さまざまな理由で変更される可能性があります。モード、時刻、または低光などの環境要因を明示的に選択する方法です。 アプリケーションがこれらの変更に対応できるようにプラットフォームコードを追加する必要があります。以降のセクションでは、この点について詳しく説明します。

### <a name="android"></a>Android

アプリケーションの**MainActivity.cs**ファイルで、`Activity` 属性の `ConfigurationChanges` プロパティに `ConfigChanges.UiMode` フィールドを追加します。これにより、アプリに UI モードの変更が通知されます。

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

同じ**MainActivity.cs**ファイルで、`OnConfigurationChanged` メソッドをオーバーライドします。

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);

    if ((newConfig.UiMode & UiMode.NightNo) != 0)
    {
        // change to light theme
        // e.g. App.Current.Resources = new YourLightTheme();
    }
    else
    {
        // change to dark theme
        // e.g. App.Current.Resources = new YourDarkTheme();
    }
}
```

### <a name="ios"></a>iOS

IOS では、デザインモードの変更が UIViewController で通知されます。これは、Xamarin では `ContentPage`です。 このメソッドをオーバーライドするには、`PageRenderer.cs`という名前の iOS プロジェクトにカスタムレンダラーを作成します。

```csharp
using System;
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(ContentPage), typeof(YourApp.iOS.Renderers.PageRenderer))]
namespace YourApp.iOS.Renderers
{
    public class PageRenderer : Xamarin.Forms.Platform.iOS.PageRenderer
    {
        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetAppTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);

            if(this.TraitCollection.UserInterfaceStyle != previousTraitCollection.UserInterfaceStyle)
            {
                SetAppTheme();
            }
        }

        void SetAppTheme()
        {
            if (this.TraitCollection.UserInterfaceStyle == UIUserInterfaceStyle.Dark)
            {
                // change to dark theme
                // e.g. App.Current.Resources = new YourDarkTheme();
            }
            else
            {
                // change to light theme
                // e.g. App.Current.Resources = new YourLightTheme();
            }
        }
    }
}
```

`TraitCollectionDidChange` メソッドをオーバーライドすると、`UserInterfaceStyle` の変更に対して操作を行うことができます。

### <a name="uwp"></a>UWP

UWP で、次のコードをアプリケーションの**MainPage.xaml.cs**ファイルに追加します。

```csharp
public sealed partial class MainPage
{

    UISettings uiSettings;

    public MainPage()
    {
        this.InitializeComponent();

        LoadApplication(new YourApp.App());

        uiSettings = new UISettings();
        uiSettings.ColorValuesChanged += ColorValuesChanged;
    }

    private void ColorValuesChanged(UISettings sender, object args)
    {
        var backgroundColor = sender.GetColorValue(UIColorType.Background);
        var isDarkMode = backgroundColor == Colors.Black;
        if(isDarkMode)
        {
            Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
            {
                // change to dark theme
                // e.g. App.Current.Resources = new YourDarkTheme();
            });
        }
        else
        {
            Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
            {
                // change to light theme
                // e.g. App.Current.Resources = new YourLightTheme();
            });
        }
    }
}
```

## <a name="set-dark-and-light-themes"></a>ダークテーマと明るいテーマを設定する

[テーマ](theming.md)ガイドに従うと、上記のプラットフォームコードの適切な場所で、アプリケーションの新しいテーマを簡単に設定できるようになります。 新しいテーマを読み込むには、アプリケーションの現在のリソースディクショナリを、選択したテーマを持つリソースディクショナリに置き換えるだけです。

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="related-links"></a>関連リンク

- [テーマ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin 形式の動的スタイル](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
