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
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "71198492"
---
# <a name="xamarinforms-material-visual"></a>Xamarin. フォーム素材ビジュアル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[マテリアルの設計](https://material.io)は、Google によって作成されたこだわりデザインシステムで、ビューやレイアウトの外観と動作について、サイズ、色、スペース、およびその他の側面を規定します。

Xamarin. Forms Material ビジュアルを使用すると、マテリアルデザインルールを Xamarin. Forms アプリケーションに適用し、同一のアプリケーション、または iOS と Android でほぼ同一のアプリケーションを作成できます。 素材ビジュアルが有効になっている場合、サポートされているビューでは、同じデザインクロスプラットフォームが採用されており、統一されたルックアンドフィールが作成されます。 これは、マテリアルデザインルールを適用するマテリアルレンダラーで実現されます。

アプリケーションで Xamarin を有効にするためのプロセスは次のとおりです。

1. IOS および Android プラットフォームのプロジェクトに、 [Xamarin. Forms. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet パッケージを追加します。 この NuGet パッケージは、iOS および Android で最適化されたマテリアルデザインレンダラーを提供します。 IOS では、パッケージは[MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents)に推移的なC#依存関係を提供します。これは、ios 用の Google の[マテリアルコンポーネント](https://material.io/develop/ios/)へのバインドです。 Android では、TargetFramework が正しくセットアップされていることを確認するために、パッケージにビルドターゲットが用意されています。
1. 各プラットフォームプロジェクトのマテリアルレンダラーを初期化します。 詳細については、「[マテリアルレンダラーの初期化](#initialize-material-renderers)」を参照してください。
1. マテリアルデザインルールを採用する必要のある任意のページで、 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティを `Material` に設定して、マテリアルレンダラーを使用します。 詳細については、「[マテリアルレンダラーの使用](#consume-material-renderers)」を参照してください。
1. optionalマテリアルレンダラーをカスタマイズします。 詳細については、「[マテリアルレンダラーをカスタマイズ](#customize-material-renderers)する」を参照してください。

> [!IMPORTANT]
> Android では、マテリアルレンダラーには 5.0 (API 21) 以上のバージョンと、バージョン 9.0 (API 28) の TargetFramework が必要です。 さらに、プラットフォームプロジェクトには Android サポートライブラリ28.0.0 以上が必要です。また、そのテーマは、マテリアルコンポーネントのテーマを継承するか、AppCompat テーマから継承する必要があります。 詳細については、「 [Android 用のマテリアルコンポーネントの概要](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)」を参照してください。

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

機能的には、素材レンダラーは既定のレンダラーとは異なります。

## <a name="initialize-material-renderers"></a>マテリアルレンダラーの初期化

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

## <a name="consume-material-renderers"></a>素材レンダラーを使用する

アプリケーションでは、ページ、レイアウト、またはビューの[`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティを `Material` に設定することによって、素材レンダラーの使用を選択できます。

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

[@No__t_1](xref:Xamarin.Forms.VisualElement.Visual)プロパティは、`IVisual` を実装する任意の型に設定できます。 [`VisualMarker`](xref:Xamarin.Forms.VisualMarker)クラスでは、次の `IVisual` プロパティを指定できます。

- `Default` –ビューが既定のレンダラーを使用して表示する必要があることを示します。
- `MatchParent` –ビューが直接の親と同じレンダラーを使用する必要があることを示します。
- `Material` –ビューがマテリアルレンダラーを使用して表示する必要があることを示します。

> [!IMPORTANT]
> [@No__t_1](xref:Xamarin.Forms.VisualElement.Visual)プロパティは[`VisualElement`](xref:Xamarin.Forms.VisualElement)クラスで定義され、ビューは親から `Visual` プロパティ値を継承します。 したがって、 [`ContentPage`](xref:Xamarin.Forms.ContentPage)の `Visual` プロパティを設定すると、ページ内のサポートされているすべてのビューでそのビジュアルが使用されるようになります。 また、`Visual` プロパティはビューでオーバーライドできます。

次のスクリーンショットは、既定のレンダラーを使用して表示されるユーザーインターフェイスを示しています。

[![IOS と Android での既定のレンダラーのスクリーンショット](material-visual-images/default-renderers.png "既定のレンダラーを使用したビュー")](material-visual-images/default-renderers-large.png#lightbox)

次のスクリーンショットは、素材レンダラーを使用してレンダリングされたのと同じユーザーインターフェイスを示しています。

[![IOS と Android でのマテリアルレンダラーのスクリーンショット](material-visual-images/material-renderers.png "マテリアルレンダラーを使用したビュー")](material-visual-images/material-renderers-large.png#lightbox)

既定のレンダラーと素材レンダラーの主な違いは、ここで示すように、素材レンダラーはテキスト[`Button`](xref:Xamarin.Forms.Button)を大文字にし、 [`Frame`](xref:Xamarin.Forms.Frame)境界線の角を丸めることです。 ただし、マテリアルレンダラーではネイティブコントロールが使用されるため、フォント、影、色、昇格などの領域では、プラットフォーム間にユーザーインターフェイスの違いが存在する可能性があります。

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

次のコードは、`MaterialProgressBarRenderer` クラスをカスタマイズする例を示しています。

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

この例では、`ExportRendererAttribute` は、`CustomMaterialProgressBarRenderer` クラスを使用して[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)ビューを表示することを指定し、`IVisual` 型を3番目の引数として登録します。

> [!NOTE]
> @No__t_1 の一部として `IVisual` 型を指定するレンダラーは、既定のレンダラーではなくビューにオプトインを表示するために使用されます。 レンダラーの選択時に、ビューの [`Visual`] プロパティが検査され、レンダラーの選択プロセスに含まれます。

カスタムレンダラーの詳細については、「[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [素材ビジュアル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Xamarin. Forms ビジュアルレンダラーを作成する](create.md)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
