---
title: Xamarin.Forms アプリケーションで暗いモードを検出します。
description: ダーク モードは、リソース ディクショナリ、動的リソース、およびプラットフォームの知識の組み合わせを使用して、任意の Xamarin.Forms アプリケーションでサポートできます。
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 03/13/2020
ms.openlocfilehash: 7fe1a98e6a497a5791f26df2fc96d41781b71ef6
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628312"
---
# <a name="detect-dark-mode-in-xamarinforms-applications"></a>Xamarin.Forms アプリケーションで暗いモードを検出します。

Xamarin.Forms アプリケーションは、テーマをサポートするのと同じ戦略を使用して、オペレーティング システム[のテーマの](theming.md)変更に応答できます。 主な違いは、テーマの変更がトリガされる方法にあります。 ダークモードとは、オペレーティングシステムレベルで設定できる幅広い外観設定を指し、どのアプリケーションが即座に尊重して応答することが期待されているかを指します。

Xamarin.Forms アプリケーションでダーク モードを使用するための最小要件は次のとおりです。

- iOS 13以上。
- アンドロイド9以上(アンドロイド9は、あなたがクエリできる外観モードをサポートしています)。
- アンドロイド10以上(アンドロイド10は、モードの変更のアプリを通知します)。
- UWP v10.0.10240.0 以上。
- Xamarin.Forms (任意のバージョン)。

明暗を含む外観モードをサポートするプロセスは次のとおりです。

1. アプリケーション全体で共有されるリソースを 定義[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。
2. 変更する各スタイルに固有のオーバーライドを提供する、各表示モードに対してリソース ディクショナリのテーマを定義します。
3. アプリケーションの**App.xaml**ファイルで、既定の外観モードのテーマを設定します。
4. 外観モードが変更されたときに`DynamicResource`スタイルを反映させるマークアップ拡張機能を使用して、アプリケーションのスタイルを設定します。
5. OS テーマの変更をリッスンするコードを追加し、アプリケーションの対応するテーマを読み込みます。

次のスクリーンショットは、明暗モードのテーマページを示しています。

[![テーマアプリのメインページのスクリーンショット、iOSとAndroid](theming-images/main-page-both-themes.png "テーマアプリのメインページ")](theming-images/main-page-both-themes-large.png#lightbox "テーマアプリのメインページ")
の[![テーマアプリの詳細ページのスクリーンショット、iOSとAndroid上](theming-images/detail-page-both-themes.png "テーマアプリの詳細ページ")](theming-images/detail-page-both-themes-large.png#lightbox "テーマアプリの詳細ページ")

## <a name="define-themes"></a>テーマの定義

暗い[テーマ](theming.md)と明るいテーマの作成に関する詳細をステップごとにテーマガイドに従ってください。

プラットフォーム コードの適切な場所に、アプリケーションの新しいテーマを簡単に設定できます。 新しいテーマを読み込むには、アプリケーションの現在のリソースディクショナリを、選択したテーマのリソースディクショナリに置き換えるだけです。

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="detect-and-apply-theme"></a>テーマを検出して適用する

現在実行中のテーマの検出は[`RequestedTheme`](~/essentials/app-theme.md)[、Xamarin.Essentials](~/essentials/index.md) (バージョン 1.4.0 以降) の機能を使用して実現できます。 その後、新しいクラスまたは`App`クラスでヘルパー メソッドを作成してテーマを構成できます。

```csharp
public partial class App : Application
{
    public static void ApplyTheme()
    {
        if (AppInfo.RequestedTheme == AppTheme.Dark)
        {
            // Change to dark theme
            // e.g. App.Current.Resources = new YourDarkTheme();
        }
        else
        {
            // Change to light theme
            // e.g. App.Current.Resources = new YourLightTheme();
        }
    }
}
```

## <a name="react-to-appearance-mode-changes"></a>外観モードの変更に対応する

デバイスの表示モードは、ユーザーが設定した構成方法に応じて、さまざまな理由で、モード、時間帯、または低照度などの環境要因を明示的に選択するなど、さまざまな理由で変更される場合があります。 アプリケーションがこれらの変更に対応できるようにプラットフォーム コードを追加する必要があります。

### <a name="android"></a>Android

アプリでダーク モードをサポートするには、テーマから継承するために、アプリのテーマを で確認`Resources/values/styles.xml`できるように更新する`DayNight`必要があります。

```xml
<style name="MainTheme.Base" parent="Theme.AppCompat.DayNight">
```

AndroidX の[マテリアル コンポーネント](https://www.nuget.org/packages/Xamarin.Google.Android.Material/)(1.1.0-rc2) 以降にアップグレードした場合は、次の方法を使用できます。

```xml
<style name="MainTheme.Base" parent="Theme.MaterialComponents.DayNight">
```

アプリケーションの**MainActivity.cs**ファイルで、属性の`ConfigChanges.UiMode``ConfigurationChanges`プロパティにフィールドを`Activity`追加して、アプリに UI モードの変更が通知されるようにします。

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

同じ**MainActivity.cs**ファイルで、メソッド`OnConfigurationChanged`をオーバーライドします。

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);
    App.ApplyTheme();
}
```

### <a name="ios"></a>iOS

iOS では、外観モードの変更が UIView コントローラーで通知されます`ContentPage`。 このメソッドをオーバーライドするには、iOS プロジェクトで`PageRenderer.cs`カスタム レンダラーを作成します。

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

メソッドを`TraitCollectionDidChange`オーバーライドすると、変更に対して操作`UserInterfaceStyle`できます。

### <a name="uwp"></a>UWP

UWP で、アプリケーションの**MainPage.xaml.cs**ファイルに次のコードを追加します。

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
- [Xamarin.Forms の動的スタイル](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
