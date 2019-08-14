---
title: Xamarin. Forms ビジュアルレンダラーを作成する
description: Xamarin ビューをサブクラス化しなくても、VisualElement オブジェクトに選択的に適用される Xamarin 形式のビジュアルを作成します。
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: bc95b9be0605c353ee9f914cb065f79711b9f92b
ms.sourcegitcommit: 41a029c69925e3a9d2de883751ebfd649e8747cd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2019
ms.locfileid: "68978287"
---
# <a name="create-a-xamarinforms-visual-renderer"></a>Xamarin. Forms ビジュアルレンダラーを作成する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

Xamarin を使用すると、xamarin のビューをサブクラス化[`VisualElement`](xref:Xamarin.Forms.VisualElement)しなくても、レンダラーを作成し、オブジェクトに対して選択的に適用できます。 の一部として`IVisual`型を指定するレンダラーは、既定のレンダラーではなくビューでの表示に使用されます。 `ExportRendererAttribute` レンダラーの選択時に、`Visual`ビューのプロパティを検査および表示機能の選択プロセスに含まれています。

> [!IMPORTANT]
> 現在、 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)ビューを表示した後でプロパティを変更することはできませんが、今後のリリースでは変更されません。

Xamarin. Forms ビジュアルレンダラーを作成して使用するプロセスは次のとおりです。

1. 必要なビューのプラットフォームレンダラーを作成します。 詳細については、「[レンダラーを作成する](#create-platform-renderers)」を参照してください。
1. から`IVisual`派生する型を作成します。 詳細については、「 [IVisual 型の作成](#create-an-ivisual-type)」を参照してください。
1. レンダラーを`IVisual`装飾`ExportRendererAttribute`するの一部として、型を登録します。 詳細については、「 [IVisual 型の登録](#register-the-ivisual-type)」を参照してください。
1. ビューの[`Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティを`IVisual`名前に設定して、ビジュアルレンダラーを使用します。 詳細については、「[ビジュアルレンダラーの使用](#consume-the-visual-renderer)」を参照してください。
1. optional`IVisual`型の名前を登録します。 詳細については、「 [IVisual 型の名前を登録する](#register-a-name-for-the-ivisual-type)」を参照してください。

## <a name="create-platform-renderers"></a>プラットフォームレンダラーを作成する

レンダラークラスの作成の詳細については、「[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)」を参照してください。 ただし、ビューをサブクラス化しなくても、ビューには Xamarin 形式のビジュアルレンダラーが適用されることに注意してください。

ここで説明するレンダラークラスは、 [`Button`](xref:Xamarin.Forms.Button)テキストを影付きで表示するカスタムを実装します。

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

クロスプラットフォームライブラリで、から`IVisual`派生する型を作成します。

```csharp
public class CustomVisual : IVisual
{
}
```

その後、 [`Button`](xref:Xamarin.Forms.Button) 型をレンダラークラスに対して登録し、オブジェクトにレンダラーの使用を許可すること`CustomVisual`ができます。

## <a name="register-the-ivisual-type"></a>IVisual 型の登録

プラットフォームプロジェクトで、アセンブリレベルで`ExportRendererAttribute`を追加します。

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

IOS プラットフォームプロジェクトのこの例では、は`ExportRendererAttribute` 、3番`CustomButtonRenderer`目の引数として登録[`Button`](xref:Xamarin.Forms.Button)された`IVisual`型を使用して、クラスを使用してオブジェクトをレンダリングすることを指定します。 の一部として`IVisual`型を指定するレンダラーは、既定のレンダラーではなくビューでの表示に使用されます。 `ExportRendererAttribute`

## <a name="consume-the-visual-renderer"></a>ビジュアルレンダラーを使用する

オブジェクト[`Button`](xref:Xamarin.Forms.Button)は、その[`Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティをに設定する`Custom`ことによって、レンダラークラスの使用を選択できます。

```xaml
<Button Visual="Custom"
        Text="CUSTOM BUTTON"
        BackgroundColor="{StaticResource PrimaryColor}"
        TextColor="{StaticResource SecondaryTextColor}"
        HorizontalOptions="FillAndExpand" />
```

> [!NOTE]
> XAML では、型コンバーターによって、 [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)プロパティ値に "Visual" サフィックスを含める必要がなくなります。 ただし、完全な型名を指定することもできます。

同等の C# コードに示します。

```csharp
Button button = new Button { Text = "CUSTOM BUTTON", ... };
button.Visual = new CustomVisual();
```

[`Visual`](xref:Xamarin.Forms.VisualElement.Visual) [レンダラー`Button`](xref:Xamarin.Forms.Button)の選択時に、のプロパティが検査され、レンダラーの選択プロセスに含まれます。 レンダラーが見つからない場合は、Xamarin. フォームの既定のレンダラーが使用されます。

次のスクリーンショットは、 [`Button`](xref:Xamarin.Forms.Button)表示されたを示しています。これは、テキストを影付きで表示します。

[![IOS と Android でのシャドウテキスト付きカスタムボタンのスクリーンショット](material-visual-images/custom-button.png "影付きのボタン")](material-visual-images/custom-button-large.png#lightbox)

## <a name="register-a-name-for-the-ivisual-type"></a>IVisual 型の名前を登録する

は[`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) 、必要に応じて、 `IVisual`型に別の名前を登録するために使用できます。 この方法を使用すると、さまざまなビジュアルライブラリ間での名前の競合を解決できます。また、その型名とは異なる名前でビジュアルを参照する場合もあります。

は[`VisualAttribute`](xref:Xamarin.Forms.VisualAttribute) 、クロスプラットフォームライブラリまたはプラットフォームプロジェクトのアセンブリレベルで定義する必要があります。

```csharp
[assembly: Visual("MyVisual", typeof(CustomVisual))]
```

この`IVisual`型は、登録された名前を使用して使用できます。

```xaml
<Button Visual="MyVisual"
        ... />
```

> [!NOTE]
> 登録された名前を使用してビジュアルを使用する場合は、任意の "ビジュアル" サフィックスを含める必要があります。

## <a name="related-links"></a>関連リンク

- [素材ビジュアル (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Xamarin. フォーム素材ビジュアル](material-visual.md)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
