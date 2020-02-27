---
title: Xamarin. フォーム素材ビジュアル
description: Xamarin. Forms Material ビジュアルを使用すると、iOS と Android でほぼ同じ外観の Xamarin アプリケーションを作成できます。
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/25/2019
ms.openlocfilehash: 81ef3c44d6eb8aaf4dd0ec467e11ef04adb02a76
ms.sourcegitcommit: 5a6124271a679b8961fa9430bd738fcb18544e92
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77618403"
---
# <a name="xamarinforms-material-visual"></a>Xamarin. フォーム素材ビジュアル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[マテリアルの設計](https://material.io)は、Google によって作成されたこだわりデザインシステムで、ビューやレイアウトの外観と動作について、サイズ、色、スペース、およびその他の側面を規定します。

Xamarin. Forms Material ビジュアルを使用すると、マテリアルデザインルールを Xamarin. Forms アプリケーションに適用し、iOS と Android でほぼ同じように見えるアプリケーションを作成できます。 素材ビジュアルが有効になっている場合、サポートされているビューでは、同じデザインクロスプラットフォームが採用されており、統一されたルックアンドフィールが作成されます。

[![素材の視覚的スクリーンショット](material-visual-images/material-visual-cropped.png)](material-visual-images/material-visual.png#lightbox)

アプリケーションで Xamarin を有効にするためのプロセスは次のとおりです。

1. IOS および Android プラットフォームのプロジェクトに、 [Xamarin. Forms. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet パッケージを追加します。 この NuGet パッケージは、iOS および Android で最適化されたマテリアルデザインレンダラーを提供します。 IOS では、パッケージは[MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents)に推移的なC#依存関係を提供します。これは、ios 用の Google の[マテリアルコンポーネント](https://material.io/develop/ios/)へのバインドです。 Android では、TargetFramework が正しくセットアップされていることを確認するために、パッケージにビルドターゲットが用意されています。
1. 各プラットフォームプロジェクトのマテリアルビジュアルを初期化します。 詳細については、「[マテリアルビジュアルの初期化](#initialize-material-visual)」を参照してください。
1. 素材デザインルールを採用する必要がある任意のページで、 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティを `Material` に設定して、素材ビジュアルコントロールを作成します。 詳細については、「[マテリアルレンダラーの使用](#apply-material-visual)」を参照してください。
1. optionalマテリアルコントロールをカスタマイズします。 詳細については、「[マテリアルコントロールをカスタマイズ](#customize-material-visual)する」を参照してください。

> [!IMPORTANT]
> Android では、マテリアルビジュアルには 5.0 (API 21) 以上のバージョンと、バージョン 9.0 (API 28) の TargetFramework が必要です。 さらに、プラットフォームプロジェクトには Android サポートライブラリ28.0.0 以上が必要です。また、そのテーマは、マテリアルコンポーネントのテーマを継承するか、AppCompat テーマから継承する必要があります。 詳細については、「 [Android 用のマテリアルコンポーネントの概要](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)」を参照してください。

素材ビジュアルは現在、次のコントロールをサポートしています。

- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Button`](xref:Xamarin.Forms.Button)
- [`CheckBox`](xref:Xamarin.Forms.CheckBox)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

マテリアルコントロールは、マテリアルデザインルールを適用するマテリアルレンダラーによって実現されます。 機能的には、素材レンダラーは既定のレンダラーとは異なります。 詳細については、「[素材ビジュアルをカスタマイズ](#customize-material-visual)する」を参照してください。

## <a name="initialize-material-visual"></a>マテリアルビジュアルの初期化

[Xamarin. Forms. material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet パッケージをインストールした後、マテリアルレンダラーを各プラットフォームプロジェクトで初期化する必要があります。

IOS では、これは、`Xamarin.Forms.Forms.Init` メソッドの*後*に `Xamarin.Forms.FormsMaterial.Init` メソッドを呼び出すことによって**AppDelegate.cs**で発生する必要があります。

```csharp
global::Xamarin.Forms.Forms.Init();
global::Xamarin.Forms.FormsMaterial.Init();
```

Android では、これは、`Xamarin.Forms.Forms.Init` メソッドの*後*に `Xamarin.Forms.FormsMaterial.Init` メソッドを呼び出すことによって**MainActivity.cs**で発生する必要があります。

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
global::Xamarin.Forms.FormsMaterial.Init(this, savedInstanceState);
```

## <a name="apply-material-visual"></a>マテリアルビジュアルの適用

アプリケーションでは、ページ、レイアウト、またはビューの[`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティを `Material`に設定することによって、素材を視覚的に表示できます。

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

同等の C# コードを次に示します。

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

`VisualElement.Visual` プロパティを `Material` に設定すると、アプリケーションでは、既定のレンダラーの代わりに素材ビジュアルレンダラーを使用するように指示されます。 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティは、`IVisual`を実装する任意の型に設定できます。 [`VisualMarker`](xref:Xamarin.Forms.VisualMarker)クラスでは、次の `IVisual` プロパティを指定できます。

- `Default` –ビューが既定のレンダラーを使用して表示する必要があることを示します。
- `MatchParent` –ビューが直接の親と同じレンダラーを使用する必要があることを示します。
- `Material` –ビューがマテリアルレンダラーを使用して表示する必要があることを示します。

> [!IMPORTANT]
> [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティは[`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスで定義され、ビューは親から `Visual` プロパティ値を継承します。 したがって、 [`ContentPage`](xref:Xamarin.Forms.ContentPage)の `Visual` プロパティを設定すると、ページ内のサポートされているすべてのビューでそのビジュアルが使用されるようになります。 また、`Visual` プロパティはビューでオーバーライドできます。

次のスクリーンショットは、既定のレンダラーを使用して表示されるユーザーインターフェイスを示しています。

[![IOS と Android での既定のレンダラーのスクリーンショット](material-visual-images/default-renderers.png "既定のレンダラーを使用したビュー")](material-visual-images/default-renderers-large.png#lightbox)

次のスクリーンショットは、素材レンダラーを使用してレンダリングされたのと同じユーザーインターフェイスを示しています。

[![IOS と Android でのマテリアルレンダラーのスクリーンショット](material-visual-images/material-renderers.png "マテリアルレンダラーを使用したビュー")](material-visual-images/material-renderers-large.png#lightbox)

既定のレンダラーと素材レンダラーの主な違いは、ここで示すように、素材レンダラーはテキスト[`Button`](xref:Xamarin.Forms.Button)を大文字にし、 [`Frame`](xref:Xamarin.Forms.Frame)境界線の角を丸めることです。 ただし、マテリアルレンダラーではネイティブコントロールが使用されるため、フォント、影、色、昇格などの領域では、プラットフォーム間にユーザーインターフェイスの違いが存在する可能性があります。

> [!NOTE]
> マテリアルデザインコンポーネントは、Google のガイドラインに密接に準拠しています。 そのため、素材デザインレンダラーは、そのサイズと動作にバイアスをかけます。 スタイルまたは動作をより細かく制御する必要がある場合でも、独自の[効果](~/xamarin-forms/app-fundamentals/effects/index.md)、[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)、または[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)を作成して、必要な詳細を実現できます。

## <a name="customize-material-visual"></a>素材ビジュアルのカスタマイズ

Material Visual NuGet パッケージは、Xamarin. Forms コントロールを認識するレンダラーのコレクションです。 カスタマイズ (素材を) ビジュアルコントロールは、既定のコントロールのカスタマイズと同じです。

既存のコントロールをカスタマイズすることを目標とする場合は、効果が推奨される方法です。 マテリアルビジュアルレンダラーが存在する場合は、レンダラーをサブクラス化するよりも効果があるため、コントロールをカスタマイズする作業はあまりありません。 効果の詳細については、「 [Xamarin. フォームの効果](~/xamarin-forms/app-fundamentals/effects/index.md)」を参照してください。

カスタムレンダラーは、マテリアルレンダラーが存在しない場合に推奨される方法です。 次のレンダラークラスは、素材ビジュアルに含まれています。

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

素材レンダラーのサブクラス化は、非マテリアルレンダラーとほぼ同じです。 ただし、素材レンダラーをサブクラス化するレンダラーをエクスポートする場合は、`VisualMarker.MaterialVisual` 型を指定する `ExportRenderer` 属性に3番目の引数を指定する必要があります。

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        //...
    }
}
```

この例では、`ExportRendererAttribute` は、`CustomMaterialProgressBarRenderer` クラスを使用して[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)ビューを表示することを指定し、`IVisual` 型を3番目の引数として登録します。

> [!NOTE]
> `ExportRendererAttribute`の一部として `IVisual` 型を指定するレンダラーは、既定のレンダラーではなくビューにオプトインを表示するために使用されます。 レンダラーの選択時に、ビューの [`Visual`] プロパティが検査され、レンダラーの選択プロセスに含まれます。

カスタムレンダラーの詳細については、「[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [素材ビジュアル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Xamarin. Forms ビジュアルレンダラーを作成する](create.md)
- [Xamarin. フォームの効果](~/xamarin-forms/app-fundamentals/effects/index.md)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
