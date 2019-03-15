---
title: Xamarin.Forms Visual レンダラーを作成します。
description: Xamarin.Forms のビジュアルでは、Xamarin.Forms のビューのサブクラス化することがなく選択的に、VisualElement オブジェクトに適用するレンダラーを使用できます。
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: 1bd56d09932c97508dd0a05fbc0eb2bad3af3f0e
ms.sourcegitcommit: 97dca3face7c4ad5555dfaca88f5b45a70ca556d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2019
ms.locfileid: "57972586"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Xamarin.Forms Visual レンダラーを作成します。

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)

Xamarin.Forms Visual により作成され、選択的に適用するレンダラー [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)サブクラス Xamarin.Forms のビューをしなくてものオブジェクト。 指定するレンダラー、`IVisual`型の一部としてその`ExportRendererAttribute`既定のレンダラーではなく、ビュー、オプトイン レンダリングに使用します。 レンダラーの選択時に、`Visual`ビューのプロパティを検査および表示機能の選択プロセスに含まれています。

> [!IMPORTANT]
> 現在、 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual)ビューが表示されたら、これは将来のリリースで変更した後、プロパティを変更できません。

作成および Xamarin.Forms Visual レンダラーを使用するためのプロセスは次のとおりです。

1. 必要なビューのプラットフォームのレンダラーを作成します。 詳細については、次を参照してください。[レンダラーを作成する](#create-platfomr-renderers)します。
1. 派生した型を作成する`IVisual`します。 詳細については、次を参照してください。 [IVisual 型を作成](#create-an-ivisual-type)です。
1. 登録、`IVisual`型の一部として、`ExportRendererAttribute`レンダラーを装飾します。 詳細については、次を参照してください。 [IVisual の種類を登録](#register-the-ivisual-type)します。
1. 設定して、ビジュアルのレンダラーを使用、 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual)に対してビューのプロパティ、`IVisual`名。 詳細については、次を参照してください。 [Visual レンダラーを消費する](#consume-the-visual-renderer)します。
1. [省略可能]名前を登録、`IVisual`型。 詳細については、次を参照してください。 [IVisual 型名登録](#register-a-name-for-the-ivisual-type)します。

## <a name="create-platform-renderers"></a>プラットフォームのレンダラーを作成します。

レンダラー クラスを作成する方法の詳細については、次を参照してください。[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)します。 ただし、ビューをサブクラス化することがなく、ビューを Xamarin.Forms Visual レンダラーに適用します。

ここで説明されているレンダラー クラスは実装してカスタム[ `Button` ](xref:Xamarin.Forms.Button)影付きテキストを表示します。

### <a name="ios"></a>iOS

次のコード例では、iOS 用のボタンのレンダラーを示します。

```csharp
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.TitleShadowOffset = new CoreGraphics.CGSize(1, 1);
            Control.SetTitleShadowColor(Color.Black.ToUIColor(), UIKit.UIControlState.Normal);
        }
    }
}
```

### <a name="android"></a>Android

次のコード例では、Android 用のボタンのレンダラーを示します。

```csharp
public class CustomButtonRenderer : Xamarin.Forms.Platform.Android.AppCompat.ButtonRenderer
{
    public CustomButtonRenderer(Context context) : base(context)
    {
    }

    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        base.OnElementChanged(e);

        if (e.OldElement != null)
        {
            // Cleanup
        }

        if (e.NewElement != null)
        {
            Control.SetShadowLayer(5, 3, 3, Color.Black.ToAndroid());
        }
    }
}
```

## <a name="create-an-ivisual-type"></a>IVisual 型を作成します。

派生した型を作成、クロス プラットフォーム ライブラリ`IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

`CustomVisual`型に登録できます、レンダラー クラスに対して許可[ `Button` ](xref:Xamarin.Forms.Button)レンダラーを使用して有効にするオブジェクト。

## <a name="register-the-ivisual-type"></a>IVisual 型を登録します。

プラットフォームのプロジェクトでのレンダラー クラスを装飾、 `ExportRendererAttribute`:

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
public class CustomButtonRenderer : ButtonRenderer
{
    protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
    {
        ...
    }
}
```

この例で、`ExportRendererAttribute`ことを指定します、`CustomButtonRenderer`クラスを使用して消費をレンダリング[ `Button` ](xref:Xamarin.Forms.Button)オブジェクトで、 `IVisual` 3 番目の引数として登録されている型。 指定するレンダラー、`IVisual`型の一部としてその`ExportRendererAttribute`既定のレンダラーではなく、ビュー、オプトイン レンダリングに使用します。

## <a name="consume-the-visual-renderer"></a>ビジュアルのレンダラーを使用します。

A [ `Button` ](xref:Xamarin.Forms.Button)オブジェクトを有効に設定して、レンダラー クラスを使用してできます、 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual)プロパティを`Custom`:

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> 型コンバーターを XAML で"Visual"サフィックスを含める必要がある、 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual)プロパティの値。 ただし、完全な型名が指定こともできます。

同等の C# コードに示します。

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

レンダラーの選択時に、 [ `Visual` ](xref:Xamarin.Forms.VisualElement.Visual)のプロパティ、 [ `Button` ](xref:Xamarin.Forms.Button)検査あり、レンダラーの選択プロセスに含まれます。 レンダラーが存在していない場合は、Xamarin.Forms の既定のレンダラーが使用されます。

次のスクリーン ショット、レンダリングされた[ `Button` ](xref:Xamarin.Forms.Button)、影付きテキストが表示されます。

[![IOS と Android でのシャドウ テキストを含むカスタム ボタンのスクリーン ショット](material-visual-images/custom-button.png "テキストの影付きのボタン")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>IVisual 型の名前を登録します。

[ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute)必要に応じて別の名前を登録するために使用できる、`IVisual`型。 この方法は、その型の名前とは異なる名前で、さまざまなビジュアル ライブラリ間またはだけ必要がある場合、ビジュアルを参照する名前の競合を解決するのには使用できます。

[ `VisualAttribute` ](xref:Xamarin.Forms.VisualAttribute)いずれかのクロス プラットフォーム ライブラリ、またはプラットフォームのプロジェクトにアセンブリ レベルで定義する必要があります。

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

`IVisual`型登録された名前を使用できます。

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> 登録された名前でビジュアルを利用する場合、"Visual"サフィックスが含める必要があります。

## <a name="related-links"></a>関連リンク

- [素材 Visual (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VisualDemos/)
- [Xamarin.Forms マテリアル Visual](material-visual.md)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
