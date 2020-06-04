---
title: Xamarin.Forms の再利用可能な RoundEffect
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fc3776934a4c109b2527132b11c6c6a93b7d9f9e
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138854"
---
# <a name="xamarinforms-reusable-roundeffect"></a>Xamarin.Forms の再利用可能な RoundEffect

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-roundeffect/)

RoundEffect を利用すると、円として VisualElement から派生するあらゆるコントロールのレンダリングが簡単になります。 このエフェクトを使用し、円形の画像、ボタン、その他のコントロールを作成できます。

[![iOS と Android での RoundEffect のスクリーンショット](example-roundeffect-images/round-effect-cropped.png)](example-roundeffect-images/round-effect.png#lightbox)

## <a name="create-a-shared-routingeffect"></a>共有 RoutingEffect を作成する

クロスプラットフォームの効果を作成するには、効果クラスを共有プロジェクトで作成する必要があります。 このサンプル アプリケーションでは、`RoutingEffect` クラスから派生する空の `RoundEffect` クラスが作成されます。

```csharp
public class RoundEffect : RoutingEffect
{
    public RoundEffect() : base($"Xamarin.{nameof(RoundEffect)}")
    {
    }
}
```

このクラスを利用すると、コードまたは XAML の効果の参照を共有プロジェクトで解決できますが、機能は与えられません。 この効果では、プラットフォームごとの実装が必要になります。

## <a name="implement-the-android-effect"></a>Android 効果を実装する

Android プラットフォーム プロジェクトでは、`PlatformEffect` から派生する `RoundEffect` クラスが定義されます。 このクラスには、Xamarin.Forms で効果クラスを解決できるようにする `assembly` 属性がタグ付けされます。

```csharp
[assembly: ResolutionGroupName("Xamarin")]
[assembly: ExportEffect(typeof(RoundEffectDemo.Droid.RoundEffect), nameof(RoundEffectDemo.Droid.RoundEffect))]
namespace RoundEffectDemo.Droid
{
    public class RoundEffect : PlatformEffect
    {
        // ...
    }
}
```

Android プラットフォームでは、`OutlineProvider` の概念を使用し、コントロールの端が定義されます。 このサンプル プロジェクトには、`ViewOutlineProvider` クラスから派生する `CornerRadiusProvider` クラスが含まれています。

```csharp
class CornerRadiusOutlineProvider : ViewOutlineProvider
{
    Element element;

    public CornerRadiusOutlineProvider(Element formsElement)
    {
        element = formsElement;
    }

    public override void GetOutline(Android.Views.View view, Outline outline)
    {
        float scale = view.Resources.DisplayMetrics.Density;
        double width = (double)element.GetValue(VisualElement.WidthProperty) * scale;
        double height = (double)element.GetValue(VisualElement.HeightProperty) * scale;
        float minDimension = (float)Math.Min(height, width);
        float radius = minDimension / 2f;
        Rect rect = new Rect(0, 0, (int)width, (int)height);
        outline.SetRoundRect(rect, radius);
    }
}
```

このクラスでは、Xamarin.Forms `Element` インスタンスの `Width` プロパティと `Height` プロパティを使用し、最短寸法の半分である半径が計算されます。

輪郭を与えるものが定義されると、`RoundEffect` クラスではそれを使用して効果を実装できます。

```csharp
public class RoundEffect : PlatformEffect
{
    ViewOutlineProvider originalProvider;
    Android.Views.View effectTarget;

    protected override void OnAttached()
    {
        try
        {
            effectTarget = Control ?? Container;
            originalProvider = effectTarget.OutlineProvider;
            effectTarget.OutlineProvider = new CornerRadiusOutlineProvider(Element);
            effectTarget.ClipToOutline = true;
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed to set corner radius: {ex.Message}");
        }
    }

    protected override void OnDetached()
    {
        if(effectTarget != null)
        {
            effectTarget.OutlineProvider = originalProvider;
            effectTarget.ClipToOutline = false;
        }
    }
}
```

`OnAttached` メソッドは、効果が要素にアタッチされると呼び出されます。 既存の `OutlineProvider` オブジェクトは保存され、効果がデタッチされたときに復元できます。 `CornerRadiusOutlineProvider` の新しいインスタンスが `OutlineProvider` として使用され、`ClipToOutline` が true に設定され、オーバーフローする要素が輪郭の縁取りに留められます。

効果が要素から削除されると `OnDetatched` メソッドが呼び出され、元の `OutlineProvider` 値が復元されます。

> [!NOTE]
> 要素の型によっては、`Control` プロパティは null になることもならないこともあります。 `Control` プロパティが null でない場合、丸い角をコントロールに直接適用できます。 ただし、null の場合、丸い角は `Container` オブジェクトに適用する必要があります。 `effectTarget` フィールドを利用すると、適切なオブジェクトに効果を適用できます。

## <a name="implement-the-ios-effect"></a>iOS 効果を実装する

iOS プラットフォーム プロジェクトでは、`PlatformEffect` から派生する `RoundEffect` クラスが定義されます。 このクラスには、Xamarin.Forms で効果クラスを解決できるようにする `assembly` 属性がタグ付けされます。

```csharp
[assembly: ResolutionGroupName("Xamarin")]
[assembly: ExportEffect(typeof(RoundEffectDemo.iOS.RoundEffect), nameof(RoundEffectDemo.iOS.RoundEffect))]
namespace RoundEffectDemo.iOS
{
    public class RoundEffect : PlatformEffect
    {
        // ...
    }
```

iOS では、コントロールに `Layer` プロパティが与えられ、このプロパティには `CornerRadius` プロパティが含まれています。 iOS での `RoundEffect` クラス実装では、角の半径が適切に計算され、レイヤーの `CornerRadius` プロパティが更新されます。

```csharp
public class RoundEffect : PlatformEffect
{
    nfloat originalRadius;
    UIKit.UIView effectTarget;

    protected override void OnAttached()
    {
        try
        {
            effectTarget = Control ?? Container;
            originalRadius = effectTarget.Layer.CornerRadius;
            effectTarget.ClipsToBounds = true;
            effectTarget.Layer.CornerRadius = CalculateRadius();
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed to set corner radius: {ex.Message}");
        }
    }

    protected override void OnDetached()
    {
        if (effectTarget != null)
        {
            effectTarget.ClipsToBounds = false;
            if (effectTarget.Layer != null)
            {
                effectTarget.Layer.CornerRadius = originalRadius;
            }
        }
    }

    float CalculateRadius()
    {
        double width = (double)Element.GetValue(VisualElement.WidthRequestProperty);
        double height = (double)Element.GetValue(VisualElement.HeightRequestProperty);
        float minDimension = (float)Math.Min(height, width);
        float radius = minDimension / 2f;

        return radius;
    }
}
```

`CalculateRadius` メソッドでは、Xamarin.Forms の `Element` の最小寸法に基づいて半径が計算されます。 効果がコントロールにアタッチされると `OnAttached` メソッドが呼び出され、レイヤーの `CornerRadius` プロパティが更新されます。 オーバーフローする要素がコントロールの縁取りに留められるよう、`ClipToBounds` プロパティが `true` に設定されます。 効果がコントロールから削除されると `OnDetatched` メソッドが呼び出され、変更が元に戻され、元の角の半径が復元されます。

> [!NOTE]
> 要素の型によっては、`Control` プロパティは null になることもならないこともあります。 `Control` プロパティが null でない場合、丸い角をコントロールに直接適用できます。 ただし、null の場合、丸い角は `Container` オブジェクトに適用する必要があります。 `effectTarget` フィールドを利用すると、適切なオブジェクトに効果を適用できます。

## <a name="consume-the-effect"></a>効果を使用する

プラットフォーム間で効果が実装されると、それは Xamarin.Forms コントロールで使用できます。 `RoundEffect` の一般的な応用では、`Image` オブジェクトが丸くなります。 次の XAML では、効果が `Image` インスタンスに適用された場合を確認できます。

```xaml
<Image Source=outdoors"
       HeightRequest="100"
       WidthRequest="100">
    <Image.Effects>
        <local:RoundEffect />
    </Image.Effects>
</Image>
```

この効果はコードでも適用できます。

```csharp
var image = new Image
{
    Source = ImageSource.FromFile("outdoors"),
    HeightRequest = 100,
    WidthRequest = 100
};
image.Effects.Add(new RoundEffect());
```

`RoundEffect` クラスは、`VisualElement` から派生するすべてのコントロールに適用できます。

> [!NOTE]
> 効果で正しい半径を計算するには、それが適用されるコントロールに明示的なサイズ設定を与える必要があります。 そのため、`HeightRequest` プロパティと `WidthRequest` プロパティを定義する必要があります。 影響を受けるコントロールが `StackLayout` に表示される場合、その `HorizontalOptions` プロパティでは、`LayoutOptions.CenterAndExpand` など、**Expand** 値を使用しないでください。使用した場合、寸法が精確になりません。

## <a name="related-links"></a>関連リンク

- [RoundEffect サンプル アプリケーション](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/effects-roundeffect/)
- [エフェクトの概要](~/xamarin-forms/app-fundamentals/effects/introduction.md)
- [エフェクトの作成](~/xamarin-forms/app-fundamentals/effects/creating.md)
