---
title: Xamarin.Forms素材ビジュアル
description: Xamarin.Forms素材ビジュアルを使用して Xamarin.Forms 、iOS と Android でほぼ同一のアプリケーションを作成できます。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: bba7d77d8cf565b1b2db2c1324e171389c5d0280
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84127180"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.Forms素材ビジュアル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[マテリアルの設計](https://material.io)は、Google によって作成されたこだわりデザインシステムで、ビューやレイアウトの外観と動作について、サイズ、色、スペース、およびその他の側面を規定します。

Xamarin.Forms素材ビジュアルを使用して、アプリケーションにマテリアルデザインルールを適用し Xamarin.Forms 、iOS と Android でほぼ同一のアプリケーションを作成できます。 素材ビジュアルが有効になっている場合、サポートされているビューでは、同じデザインクロスプラットフォームが採用されており、統一されたルックアンドフィールが作成されます。

[![素材の視覚的スクリーンショット](material-visual-images/material-visual-cropped.png)](material-visual-images/material-visual.png#lightbox)

アプリケーションで素材を視覚化するためのプロセス Xamarin.Forms は次のとおりです。

1. を追加[ Xamarin.Forms します。Visual。](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) iOS および Android プラットフォームのプロジェクトに NuGet パッケージを作成します。 この NuGet パッケージは、iOS および Android で最適化されたマテリアルデザインレンダラーを提供します。 IOS では、パッケージは[MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents)に推移的な依存関係を提供します。これは、 [Ios 用の Google のマテリアルコンポーネント](https://material.io/develop/ios/)に対する C# バインドです。 Android では、TargetFramework が正しくセットアップされていることを確認するために、パッケージにビルドターゲットが用意されています。
1. 各プラットフォームプロジェクトのマテリアルビジュアルを初期化します。 詳細については、「[マテリアルビジュアルの初期化](#initialize-material-visual)」を参照してください。
1. 素材デザインルールを適用する [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) ページでプロパティをに設定して、素材ビジュアルコントロールを作成し `Material` ます。 詳細については、「[マテリアルレンダラーの使用](#apply-material-visual)」を参照してください。
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

をインストールした後[ Xamarin.Forms 。Visual の](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/)NuGet パッケージ。マテリアルレンダラーは、各プラットフォームプロジェクトで初期化する必要があります。

IOS では、メソッドの後にメソッドを呼び出すことによって、 **AppDelegate.cs**でこれを行う必要があり `Xamarin.Forms.FormsMaterial.Init` *after* `Xamarin.Forms.Forms.Init` ます。

```csharp
global::Xamarin.Forms.Forms.Init();
global::Xamarin.Forms.FormsMaterial.Init();
```

Android では、メソッドの後にメソッドを呼び出すことによって、 **MainActivity.cs**でこれを行う必要があり `Xamarin.Forms.FormsMaterial.Init` *after* `Xamarin.Forms.Forms.Init` ます。

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
global::Xamarin.Forms.FormsMaterial.Init(this, savedInstanceState);
```

## <a name="apply-material-visual"></a>マテリアルビジュアルの適用

アプリケーションでは、 [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) ページ、レイアウト、またはビューのプロパティを次のように設定することによって、素材をビジュアル化できます `Material` 。

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

プロパティをに設定すると、アプリケーションでは、 `VisualElement.Visual` `Material` 既定のレンダラーの代わりに素材ビジュアルレンダラーを使用するように指示されます。 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティは、を実装する任意の型に設定でき `IVisual` [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) ます。クラスでは、次のプロパティを指定し `IVisual` ます。

- `Default`–ビューが既定のレンダラーを使用して表示されることを示します。
- `MatchParent`–ビューが直接の親と同じレンダラーを使用する必要があることを示します。
- `Material`–ビューがマテリアルレンダラーを使用して表示されることを示します。

> [!IMPORTANT]
> [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティはクラスで定義され [`VisualElement`](xref:Xamarin.Forms.VisualElement) 、ビューは親の `Visual` プロパティ値を継承します。 そのため、でプロパティを設定すると、 `Visual` [`ContentPage`](xref:Xamarin.Forms.ContentPage) ページ内のサポートされているすべてのビューでそのビジュアルが使用されるようになります。 また、プロパティは `Visual` ビューでオーバーライドできます。

次のスクリーンショットは、既定のレンダラーを使用して表示されるユーザーインターフェイスを示しています。

[![IOS と Android での既定のレンダラーのスクリーンショット](material-visual-images/default-renderers.png "既定のレンダラーを使用したビュー")](material-visual-images/default-renderers-large.png#lightbox)

次のスクリーンショットは、素材レンダラーを使用してレンダリングされたのと同じユーザーインターフェイスを示しています。

[![IOS と Android でのマテリアルレンダラーのスクリーンショット](material-visual-images/material-renderers.png "マテリアルレンダラーを使用したビュー")](material-visual-images/material-renderers-large.png#lightbox)

ここで示す既定のレンダラーと素材レンダラーの主な違いは、素材レンダラーによってテキストが大文字 [`Button`](xref:Xamarin.Forms.Button) になり、境界の角が丸くなることです [`Frame`](xref:Xamarin.Forms.Frame) 。 ただし、マテリアルレンダラーではネイティブコントロールが使用されるため、フォント、影、色、昇格などの領域では、プラットフォーム間にユーザーインターフェイスの違いが存在する可能性があります。

> [!NOTE]
> マテリアルデザインコンポーネントは、Google のガイドラインに密接に準拠しています。 そのため、素材デザインレンダラーは、そのサイズと動作にバイアスをかけます。 スタイルまたは動作をより細かく制御する必要がある場合でも、独自の[効果](~/xamarin-forms/app-fundamentals/effects/index.md)、[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)、または[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)を作成して、必要な詳細を実現できます。

## <a name="customize-material-visual"></a>素材ビジュアルのカスタマイズ

マテリアルビジュアル NuGet パッケージは、コントロールを認識するレンダラーのコレクションです Xamarin.Forms 。 カスタマイズ (素材を) ビジュアルコントロールは、既定のコントロールのカスタマイズと同じです。

既存のコントロールをカスタマイズすることを目標とする場合は、効果が推奨される方法です。 マテリアルビジュアルレンダラーが存在する場合は、レンダラーをサブクラス化するよりも効果があるため、コントロールをカスタマイズする作業はあまりありません。 効果の詳細については、「 [ Xamarin.Forms 効果](~/xamarin-forms/app-fundamentals/effects/index.md)」を参照してください。

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

素材レンダラーのサブクラス化は、非マテリアルレンダラーとほぼ同じです。 ただし、素材レンダラーをサブクラス化するレンダラーをエクスポートする場合は、型を指定する属性に3番目の引数を指定する必要があり `ExportRenderer` `VisualMarker.MaterialVisual` ます。

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

この例では、は、 `ExportRendererAttribute` `CustomMaterialProgressBarRenderer` [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) `IVisual` 3 番目の引数として登録された型を使用して、ビューを表示するためにクラスを使用することを指定しています。

> [!NOTE]
> `IVisual`の一部として型を指定するレンダラーは、 `ExportRendererAttribute` 既定のレンダラーではなくビューでの表示に使用されます。 レンダラーの選択時に、 `Visual` ビューのプロパティが検査され、レンダラーの選択プロセスに含まれます。

カスタムレンダラーの詳細については、「[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [素材ビジュアル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [ビジュアルレンダラーを作成する Xamarin.Forms](create.md)
- [Xamarin.Forms及ぼす](~/xamarin-forms/app-fundamentals/effects/index.md)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
