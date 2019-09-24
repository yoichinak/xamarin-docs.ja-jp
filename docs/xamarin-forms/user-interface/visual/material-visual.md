---
title: Xamarin. フォーム素材ビジュアル
description: Xamarin. Forms Material ビジュアルを使用すると、iOS と Android で同一またはほぼ同一の外観のフォームアプリケーションを作成できます。
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: b735541d51321231775b025745e68c54552697d3
ms.sourcegitcommit: 76f930ce63b193ca3f7f85f768b031e59cb342ec
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198492"
---
# <a name="xamarinforms-material-visual"></a>Xamarin. フォーム素材ビジュアル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[マテリアルの設計](https://material.io)は、Google によって作成されたこだわりデザインシステムで、ビューやレイアウトの外観と動作について、サイズ、色、スペース、およびその他の側面を規定します。

Xamarin. Forms Material ビジュアルを使用すると、マテリアルデザインルールを Xamarin. Forms アプリケーションに適用し、同一のアプリケーション、または iOS と Android でほぼ同一のアプリケーションを作成できます。 素材ビジュアルが有効になっている場合、サポートされているビューでは、同じデザインクロスプラットフォームが採用されており、統一されたルックアンドフィールが作成されます。 これは、マテリアルデザインルールを適用するマテリアルレンダラーで実現されます。

アプリケーションで Xamarin を有効にするためのプロセスは次のとおりです。

1. IOS および Android プラットフォームのプロジェクトに、 [Xamarin. Forms. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet パッケージを追加します。 この NuGet パッケージは、iOS および Android で最適化されたマテリアルデザインレンダラーを提供します。 IOS では、パッケージは[MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents)に推移的なC#依存関係を提供します。これは、ios 用の Google の[マテリアルコンポーネント](https://material.io/develop/ios/)へのバインドです。 Android では、TargetFramework が正しくセットアップされていることを確認するために、パッケージにビルドターゲットが用意されています。
1. 各プラットフォームプロジェクトのマテリアルレンダラーを初期化します。 詳細については、「[マテリアルレンダラーの初期化](#initialize-material-renderers)」を参照してください。
1. マテリアルデザインルールを適用するページ[`Visual`](xref:Xamarin.Forms.VisualElement.Visual)でプロパティ`Material`をに設定して、マテリアルレンダラーを使用します。 詳細については、「[マテリアルレンダラーの使用](#consume-material-renderers)」を参照してください。
1. optionalマテリアルレンダラーをカスタマイズします。 詳細については、「[マテリアルレンダラーをカスタマイズ](#customize-material-renderers)する」を参照してください。

> [!IMPORTANT]
> Android では、マテリアルレンダラーには 5.0 (API 21) 以上のバージョンと、バージョン 9.0 (API 28) の TargetFramework が必要です。 さらに、プラットフォームプロジェクトには Android サポートライブラリ28.0.0 以上が必要です。また、そのテーマは、マテリアルコンポーネントのテーマを継承するか、AppCompat テーマから継承する必要があります。 詳細については、次を参照してください。 [Android 用資料のコンポーネントの概要](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)します。

次のビューについては、現在、マテリアルレンダラーが[Xamarin. Forms. material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet パッケージに含まれています。

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

## <a name="initialize-material-renderers"></a>マテリアルレンダラーの初期化

[Xamarin. Forms. material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet パッケージをインストールした後、マテリアルレンダラーを各プラットフォームプロジェクトで初期化する必要があります。

IOS では、 `Xamarin.Forms.Forms.Init`メソッドの*後*にメソッドを`Xamarin.Forms.FormsMaterial.Init`呼び出すことによって、 **AppDelegate.cs**でこれを行う必要があります。

```csharp
global::Xamarin.Forms.Forms.Init();
global::Xamarin.Forms.FormsMaterial.Init();
```

Android では、 `Xamarin.Forms.Forms.Init`メソッドの*後*にメソッドを`Xamarin.Forms.FormsMaterial.Init`呼び出すことによって、 **MainActivity.cs**でこれを行う必要があります。

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
global::Xamarin.Forms.FormsMaterial.Init(this, savedInstanceState);
```

## <a name="consume-material-renderers"></a>素材レンダラーを使用する

アプリケーションでは、ページ、レイアウト、またはビュー [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual)のプロパティを次のように設定する`Material`ことによって、マテリアルレンダラーの使用を選択できます。

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

これに相当する C# コードを次に示します。

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

プロパティは、を実装`IVisual`する任意の型に設定できます[`VisualMarker`](xref:Xamarin.Forms.VisualMarker) 。クラスでは`IVisual` 、次のプロパティを指定します。 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

- `Default`–ビューが既定のレンダラーを使用して表示されることを示します。
- `MatchParent`–ビューが直接の親と同じレンダラーを使用する必要があることを示します。
- `Material`–ビューがマテリアルレンダラーを使用して表示されることを示します。

> [!IMPORTANT]
> プロパティは[`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスで定義され`Visual` 、ビューは親のプロパティ値を継承します。 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) そのため、で`Visual`プロパティ[`ContentPage`](xref:Xamarin.Forms.ContentPage)を設定すると、ページ内のサポートされているすべてのビューでそのビジュアルが使用されるようになります。 さらに、`Visual`ビューのプロパティをオーバーライドできます。

次のスクリーンショットは、既定のレンダラーを使用して表示されるユーザーインターフェイスを示しています。

[![IOS と Android での既定のレンダラーのスクリーンショット](material-visual-images/default-renderers.png "既定のレンダラーを使用したビュー")](material-visual-images/default-renderers-large.png#lightbox)

次のスクリーン ショットは、素材のレンダラーを使用するレンダリングと同じユーザー インターフェイスを表示します。

[![IOS と Android でのマテリアルレンダラーのスクリーンショット](material-visual-images/material-renderers.png "マテリアルレンダラーを使用したビュー")](material-visual-images/material-renderers-large.png#lightbox)

ここで示す既定のレンダラーと素材レンダラーの主な違いは、素材レンダラーによってテキスト[`Button`](xref:Xamarin.Forms.Button)が大文字になり、境界の[`Frame`](xref:Xamarin.Forms.Frame)角が丸くなることです。 ただし、マテリアルレンダラーではネイティブコントロールが使用されるため、フォント、影、色、昇格などの領域では、プラットフォーム間にユーザーインターフェイスの違いが存在する可能性があります。

> [!NOTE]
> マテリアルデザインコンポーネントは、Google のガイドラインに密接に準拠しています。 そのため、素材デザインレンダラーは、そのサイズと動作にバイアスをかけます。 スタイルまたは動作をより細かく制御する必要がある場合でも、独自の[効果](~/xamarin-forms/app-fundamentals/effects/index.md)、[動作](~/xamarin-forms/app-fundamentals/behaviors/index.md)、または[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)を作成して、必要な詳細を実現できます。

## <a name="customize-material-renderers"></a>素材レンダラーをカスタマイズする

必要に応じて、次の基本クラスを使用して、既定のレンダラーと同様に、マテリアルレンダラーをカスタマイズすることもできます。

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

次のコードは、クラスを`MaterialProgressBarRenderer`カスタマイズする例を示しています。

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

この例では、 `ExportRendererAttribute`は、3 `CustomMaterialProgressBarRenderer`番目の引数として[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)登録された`IVisual`型を使用して、ビューを表示するためにクラスを使用することを指定しています。

> [!NOTE]
> の一部として`IVisual`型を指定するレンダラーは、既定のレンダラーではなくビューでの表示に使用されます。 `ExportRendererAttribute` レンダラーの選択時に、`Visual`ビューのプロパティを検査および表示機能の選択プロセスに含まれています。

カスタムレンダラーの詳細については、「[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [素材ビジュアル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Xamarin. Forms ビジュアルレンダラーを作成する](create.md)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
