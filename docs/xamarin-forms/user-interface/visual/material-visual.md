---
title: Xamarin.Forms マテリアル Visual
description: IOS と Android でまったく同じ、またはほぼ同じですが、外観の Xamarin.Forms アプリケーションを作成する Xamarin.Forms マテリアル Visual を使用できます。
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 46ffe29f898e2f7bd8fcbe759f5a2f54e3dd60e1
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557504"
---
# <a name="xamarinforms-material-visual"></a>Xamarin.Forms マテリアル Visual

IOS と Android でまったく同じ、またはほぼ同じですが、外観の Xamarin.Forms アプリケーションを作成する Xamarin.Forms マテリアル Visual を使用できます。 アプリケーションを使用した、外観を選択すると、素材の外観を実装するその他のレンダラーを使用して、これは、`Visual`プロパティ。

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>
```

同等の C# コードに示します。

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

> [!IMPORTANT]
> `Visual`でプロパティが定義されている、`VisualElement`クラスのインスタンス ビューが継承すると、`Visual`その親からのプロパティの値。 そのため、設定、`Visual`プロパティを`ContentPage`により、ページで、サポートされているビューがその視覚的な外観を使用するようになります。 さらに、`Visual`ビューのプロパティをオーバーライドできます。

素材のレンダラーは iOS と Android では、次のビューに現在含まれています。

- [`Button`](xref:Xamarin.Forms.Button)
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

IOS と Android では、プラットフォーム プロジェクトがあります、 [Xamarin.Forms.Visual.Material](https://www.nuget.org/packages/ https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet パッケージをインストールします。 各プラットフォーム プロジェクトに NuGet パッケージをインストールすると、初期化コードが必要な*後*、`Xamarin.Forms.Forms.Init`メソッドの呼び出し。 Ios の場合、次のコードを使用します。

```csharp
global::Xamarin.Forms.Forms.Init();
FormsMaterial.Init();
```

Android に渡されるのと同じパラメーターを渡す必要があります`Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.Init(this, bundle);
FormsMaterial.Init(this, bundle);
```

> [!IMPORTANT]
> Android では、マテリアルは TargetFramework 9.0 (API 28) でのみ動作します。 さらに、プラットフォーム プロジェクトを v28 サポート ライブラリを使用し、マテリアル コンポーネント テーマから継承か続行 AppCompat のテーマを継承する必要がありますのテーマ。 詳細については、次を参照してください。 [Android 用資料のコンポーネントの概要](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)します。

次のスクリーン ショットを表示するマテリアルのレンダラーは存在するが、既定のレンダラーを使用してレンダリングの 4 つのビューを含むユーザー インターフェイス。

[![IOS と Android での既定のレンダラーのスクリーン ショット](material-visual-images/default-renderers.png "既定レンダラーを使用してビュー")](material-visual-images/default-renderers-large.png#lightbox)

次のスクリーン ショットは、素材のレンダラーを使用するレンダリングと同じユーザー インターフェイスを表示します。

[![素材レンダラーでは、iOS や Android 上のスクリーン ショット](material-visual-images/material-renderers.png "素材のレンダラーを使用してビュー")](material-visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> 素材のレンダラーでは、レンダリングされたコントロールは引き続きネイティブ コントロールしたがってされますが、フォント、影、色、および昇格などの分野のプラットフォーム間でのユーザー インターフェイスの違い。

## <a name="material-renderers"></a>素材のレンダラー

素材のレンダラーをカスタマイズできるで既定のレンダラーと同じように次の基本クラスを使用しています。

- `MaterialButtonRenderer`
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

## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
