---
title: Xamarin.Forms マテリアル Visual
description: IOS と Android でまったく同じ、またはほぼ同じですが、外観の Xamarin.Forms アプリケーションを作成する Xamarin.Forms マテリアル Visual を使用できます。
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: a626532ac507185b6c01abb5327efa7015c787f5
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2019
ms.locfileid: "67512900"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.Forms マテリアル Visual

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

[素材のデザイン](https://material.io)は Google では、サイズ、色、間隔、およびその他の側面は、どのビューとレイアウト外観し、動作を規定するによって作成された設計のための厳密なシステムです。

Xamarin.Forms マテリアル Visual は iOS と Android でまったく同じ、またはほぼ同じですが、外観のアプリケーションを作成、Xamarin.Forms アプリケーションにマテリアル デザイン規則を適用する使用できます。 マテリアルのビジュアルが有効な場合、サポートされているビューは、同じデザイン クロス プラットフォーム、統合されたルック アンド フィールを作成するを採用します。 これは、マテリアル デザイン規則が適用される素材のレンダラーで実現されます。

アプリケーションで Xamarin.Forms マテリアル ビジュアルを有効にするプロセスは次のとおりです。

1. 追加、 [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) iOS および Android プラットフォームのプロジェクトに NuGet パッケージ。 この NuGet パッケージは、iOS と Android での最適化されたマテリアル デザイン レンダラーを提供します。 Ios では、パッケージは、推移的依存関係を提供[Xamarin.iOS.MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents)、これは、 C# Google へのバインド[iOS 用資料コンポーネント](https://material.io/develop/ios/)します。 Android では、パッケージは、TargetFramework が正しく設定されていることを確認するビルド ターゲットを提供します。
1. 各プラットフォーム プロジェクトで素材のレンダラーを初期化します。 詳細については、次を参照してください。[素材のレンダラーを初期化](#initialize-material-renderers)します。
1. 素材のレンダラーを使用するには、設定、 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual)プロパティを`Material`マテリアル デザイン規則を採用すべきページにします。 詳細については、次を参照してください。[素材のレンダラーを消費する](#consume-material-renderers)します。
1. [省略可能]素材のレンダラーをカスタマイズします。 詳細については、次を参照してください。[素材のレンダラーをカスタマイズ](#customize-material-renderers)します。

> [!IMPORTANT]
> Android では、素材のレンダラーが 5.0 (API 21) の最小バージョンを必要または以上、およびバージョン 9.0 の TargetFramework (API 28)。 さらに、プラットフォーム プロジェクトには、Android サポート ライブラリ 28.0.0 が必要です。 以降、のテーマは、マテリアル コンポーネント テーマから継承または AppCompat のテーマを継承する続行する必要があります。 詳細については、[Android 用資料のコンポーネントの概要](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)を参照してください。

素材のレンダラーは現在に含まれる、 [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/)次のビュー用の NuGet パッケージ。

- [`Button`](xref:Xamarin.Forms.Button)
- `CheckBox`
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)

機能的には、素材のレンダラーは既定のレンダラーと変わりません。

## <a name="initialize-material-renderers"></a>素材のレンダラーを初期化します。

インストールした後、 [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet パッケージ、マテリアルのレンダラーは、各プラットフォーム プロジェクトで初期化する必要があります。

Ios では、このエラーは発生で**AppDelegate.cs**を呼び出すことによって、`FormsMaterial.Init`メソッド*後*、`Xamarin.Forms.Forms.Init`メソッド。

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

Android では、このエラーは発生で**MainActivity.cs**を呼び出すことによって、`FormsMaterial.Init`メソッド*後*、`Xamarin.Forms.Forms.Init`メソッド。

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
FormsMaterial.Init(this, savedInstanceState);
```

## <a name="consume-material-renderers"></a>素材のレンダラーを使用します。

アプリケーションを有効に設定して、素材のレンダラーを使用してできます、 [ `VisualElement.Visual` ](xref:Xamarin.Forms.VisualElement.Visual)プロパティ ページ、レイアウト、またはビューに`Material`:

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

同等の C# コードに示します。

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

[ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual)プロパティを実装する任意の型に設定することができます`IVisual`で、 [ `VisualMarker` ](xref:Xamarin.Forms.VisualMarker)クラスは、次を提供する`IVisual`プロパティ。

- `Default` – 既定のレンダラーを使用して、ビューを表示することを示します。
- `MatchParent` – ビューが直接の親として同じレンダラーを使用することを示します。
- `Material` – 素材のレンダラーを使用して、ビューを表示することを示します。

> [!IMPORTANT]
> [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual)でプロパティが定義されている、 [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)クラスのインスタンス ビューが継承すると、`Visual`その親からのプロパティの値。 そのため、設定、`Visual`プロパティを[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)により、ページで、サポートされているビューがそのビジュアルを使用するようになります。 さらに、`Visual`ビューのプロパティをオーバーライドできます。

次のスクリーン ショットは、ユーザー インターフェイスが既定のレンダラーを使用して表示を表示します。

[![IOS と Android での既定のレンダラーのスクリーン ショット](material-visual-images/default-renderers.png "既定レンダラーを使用してビュー")](material-visual-images/default-renderers-large.png#lightbox)

次のスクリーン ショットは、素材のレンダラーを使用するレンダリングと同じユーザー インターフェイスを表示します。

[![素材レンダラーでは、iOS や Android 上のスクリーン ショット](material-visual-images/material-renderers.png "素材のレンダラーを使用してビュー")](material-visual-images/material-renderers-large.png#lightbox)

既定のレンダラーと素材のレンダラーでは、ここのメインの明白な違いは素材のレンダラーが大文字に[ `Button` ](xref:Xamarin.Forms.Button) 、テキストの角を丸めると[ `Frame` ](xref:Xamarin.Forms.Frame)境界線。 ただし、素材レンダラーは、ネイティブ コントロールを使用し、したがってまだありますフォント、影、色、および昇格などの分野のプラットフォーム間でのユーザー インターフェイスの違い。

> [!NOTE]
> 素材のデザイン コンポーネントは、Google のガイドラインに密接に従います。 その結果、マテリアル デザイン レンダラーは、そのサイズと動作に偏っています。 スタイルまたは動作の制御を必要とするときに作成できます独自[効果](~/xamarin-forms/app-fundamentals/effects/index.md)、[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)、または[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)必要な詳細を実現するためにします。

## <a name="customize-material-renderers"></a>素材のレンダラーをカスタマイズします。

素材のレンダラー必要に応じてカスタマイズできます、次の基本クラスを使用して、既定のレンダラーと同様。

- `MaterialButtonRenderer`
- `MaterialCheckBoxRenderer`
- `MaterialEntryRenderer`
- `MaterialFrameRenderer`
- `MaterialProgressBarRenderer`
- `MaterialDatePickerRenderer`
- `MaterialTimePickerRenderer`
- `MaterialPickerRenderer`
- `MaterialActivityIndicatorRenderer`
- `MaterialEditorRenderer`
- `MaterialSliderRenderer`
- `MaterialStepperRenderer`

次のコードをカスタマイズする例を示して、`MaterialProgressBarRenderer`クラス。

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        ...
    }
}
```

この例で、`ExportRendererAttribute`ことを指定します、`CustomMaterialProgressBarRenderer`クラスを使用してレンダリング、 [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar)ビューで、 `IVisual` 3 番目の引数として登録されている型。

> [!NOTE]
> 指定するレンダラー、`IVisual`型の一部としてその`ExportRendererAttribute`既定のレンダラーではなく、ビュー、オプトイン レンダリングに使用します。 レンダラーの選択時に、`Visual`ビューのプロパティを検査および表示機能の選択プロセスに含まれています。

カスタム レンダラーの詳細については、次を参照してください。[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)します。

## <a name="related-links"></a>関連リンク

- [素材 Visual (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Xamarin.Forms Visual レンダラーを作成します。](create.md)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
