---
title: Xamarin. Forms アプリケーションでのダークモードの検出
description: ダークモードは、ResourceDictionaries、DynamicResources、および platform knowledge の組み合わせを使用するすべての Xamarin. Forms アプリケーションでサポートされています。
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 03/13/2020
ms.openlocfilehash: 104237155797ca90c52ad385e8349480f9666c4c
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303503"
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

プラットフォームコードの適切な場所で、アプリケーションの新しいテーマを簡単に設定できます。 新しいテーマを読み込むには、アプリケーションの現在のリソースディクショナリを、選択したテーマを持つリソースディクショナリに置き換えるだけです。

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="detect-and-apply-theme"></a>テーマの検出と適用

現在実行中のテーマを検出するには、 [Xamarin. Essentials](~/essentials/index.md)の[`RequestedTheme`](~/essentials/app-theme.md)機能 (バージョンの場合は、それ以降) を使用します。 次に、新しいクラスまたは `App` クラスにヘルパーメソッドを作成して、テーマを構成できます。

```csharp
public partial class App : Application
{
    public static void ApplyTheme()
    {
        if (AppInfo.RequestedTheme == AppTheme.Dark)
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
}
```

## <a name="react-to-appearance-mode-changes"></a>外観モードの変更に対処する

デバイスの表示モードは、ユーザーが設定をどのように構成しているかに応じて、さまざまな理由で変更される可能性があります。モード、時刻、または低光などの環境要因を明示的に選択する方法です。 アプリケーションがこれらの変更に対応できるようにプラットフォームコードを追加する必要があります。以降のセクションでは、この点について詳しく説明します。

### <a name="android"></a>Android

アプリでダークモードをサポートするには、アプリのテーマを更新する必要があります。このテーマは、`DayNight` テーマから継承するために `Resources/values/styles.xml`で見つけることができます。

```xml
<style name="MainTheme.Base" parent="Theme.AppCompat.DayNight">
```

AndroidX の[マテリアルコンポーネント](https://www.nuget.org/packages/Xamarin.Google.Android.Material/)(1.1.0) 以降にアップグレードした場合は、次のものを使用できます。

```xml
<style name="MainTheme.Base" parent="Theme.MaterialComponents.DayNight">
```

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
    App.ApplyTheme();
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
                App.ApplyTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);

            App.ApplyTheme();
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
        Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
        {
            App.ApplyTheme();
        });
    }
}
```

## <a name="related-links"></a>関連リンク

- [テーマ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [リソース ディクショナリ](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Xamarin 形式の動的スタイル](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
