---
title: ビジュアルレンダラーを作成する Xamarin.Forms
description: Xamarin.Formsビューをサブクラス化せずに、VisualElement オブジェクトに選択的に適用するビジュアルを作成し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 23edbb007e912d13858686d1c5ec574c9e3349c7
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84127142"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>ビジュアルレンダラーを作成する Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

Xamarin.Formsビジュアルを使用すると、ビューをサブクラス化しなくても、レンダラーを作成し、オブジェクトに対して選択的に適用でき [`VisualElement`](xref:Xamarin.Forms.VisualElement) Xamarin.Forms ます。 `IVisual`の一部として型を指定するレンダラーは、 `ExportRendererAttribute` 既定のレンダラーではなくビューでの表示に使用されます。 レンダラーの選択時に、 `Visual` ビューのプロパティが検査され、レンダラーの選択プロセスに含まれます。

> [!IMPORTANT]
> 現在、 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) ビューを表示した後でプロパティを変更することはできませんが、今後のリリースでは変更されません。

ビジュアルレンダラーを作成して使用するためのプロセス Xamarin.Forms は次のとおりです。

1. 必要なビューのプラットフォームレンダラーを作成します。 詳細については、「[レンダラーを作成する](#create-platform-renderers)」を参照してください。
1. から派生する型を作成 `IVisual` します。 詳細については、「 [IVisual 型の作成](#create-an-ivisual-type)」を参照してください。
1. レンダラーを `IVisual` 装飾するの一部として、型を登録し `ExportRendererAttribute` ます。 詳細については、「 [IVisual 型の登録](#register-the-ivisual-type)」を参照してください。
1. ビューのプロパティを名前に設定して、ビジュアルレンダラーを使用し [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) `IVisual` ます。 詳細については、「[ビジュアルレンダラーの使用](#consume-the-visual-renderer)」を参照してください。
1. optional型の名前を登録 `IVisual` します。 詳細については、「 [IVisual 型の名前を登録する](#register-a-name-for-the-ivisual-type)」を参照してください。

## <a name="create-platform-renderers"></a>プラットフォームのレンダラーを作成する

レンダラークラスの作成の詳細については、「[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」を参照してください。 ただし、ビュー Xamarin.Forms をサブクラス化しなくても、ビジュアルレンダラーがビューに適用されることに注意してください。

ここで説明するレンダラークラスは、 [`Button`](xref:Xamarin.Forms.Button) テキストを影付きで表示するカスタムを実装します。

### <a name="ios"></a>iOS

次のコード例は、iOS 用のボタンレンダラーを示しています。

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

次のコード例は、Android 用の button レンダラーを示しています。

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

## <a name="create-an-ivisual-type"></a>IVisual 型を作成する

クロスプラットフォームライブラリで、から派生する型を作成し `IVisual` ます。

```csharp
public class CustomVisual : IVisual
{
}
```

`CustomVisual`その後、型をレンダラークラスに対して登録し、 [`Button`](xref:Xamarin.Forms.Button) オブジェクトにレンダラーの使用を許可することができます。

## <a name="register-the-ivisual-type"></a>IVisual 型の登録

プラットフォームプロジェクトで、アセンブリレベルでを追加し `ExportRendererAttribute` ます。

```csharp
[assembly: ExportRenderer(typeof(Xamarin.Forms.Button), typeof(CustomButtonRenderer), new[] { typeof(CustomVisual) })]
namespace VisualDemos.iOS
{
    public class CustomButtonRenderer : ButtonRenderer
    {
        protected override void OnElementChanged(ElementChangedEventArgs<Button> e)
        {
            // ...
        }
    }
}
```

IOS プラットフォームプロジェクトのこの例では、は、 `ExportRendererAttribute` `CustomButtonRenderer` [`Button`](xref:Xamarin.Forms.Button) `IVisual` 3 番目の引数として登録された型を使用して、クラスを使用してオブジェクトをレンダリングすることを指定します。 `IVisual`の一部として型を指定するレンダラーは、 `ExportRendererAttribute` 既定のレンダラーではなくビューでの表示に使用されます。

## <a name="consume-the-visual-renderer"></a>ビジュアルレンダラーを使用する

オブジェクトは、 [`Button`](xref:Xamarin.Forms.Button) そのプロパティをに設定することによって、レンダラークラスの使用を選択でき [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) `Custom` ます。

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> XAML では、型コンバーターによって、プロパティ値に "Visual" サフィックスを含める必要がなく [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) なります。 ただし、完全な型名を指定することもできます。

これに相当する C# コードを次に示します。

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

レンダラーの選択時に、 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) のプロパティ [`Button`](xref:Xamarin.Forms.Button) が検査され、レンダラーの選択プロセスに含まれます。 レンダラーが見つからない場合は、 Xamarin.Forms 既定のレンダラーが使用されます。

次のスクリーンショットは、表示されたを示してい [`Button`](xref:Xamarin.Forms.Button) ます。これは、テキストを影付きで表示します。

[![IOS と Android でのシャドウテキスト付きカスタムボタンのスクリーンショット](material-visual-images/custom-button.png "影付きのボタン")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>IVisual 型の名前を登録する

は、 [`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) 必要に応じて、型に別の名前を登録するために使用でき `IVisual` ます。 この方法を使用すると、さまざまなビジュアルライブラリ間での名前の競合を解決できます。また、その型名とは異なる名前でビジュアルを参照する場合もあります。

は、 [`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) クロスプラットフォームライブラリまたはプラットフォームプロジェクトのアセンブリレベルで定義する必要があります。

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

この `IVisual` 型は、登録された名前を使用して使用できます。

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> 登録された名前を使用してビジュアルを使用する場合は、任意の "ビジュアル" サフィックスを含める必要があります。

## <a name="related-links"></a>関連リンク

- [素材ビジュアル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Xamarin.Forms の素材のビジュアル](material-visual.md)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
