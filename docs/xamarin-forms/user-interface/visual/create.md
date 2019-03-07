---
title: Xamarin.Forms Visual レンダラーを作成します。
description: Xamarin.Forms のビジュアルでは、Xamarin.Forms コントロールのサブクラス化することがなく選択的に、VisualElement オブジェクトに適用するレンダラーを使用できます。
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2019
ms.openlocfilehash: 3c614bcd0044a0aa4a698c830d2f2af5aba6c65a
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557514"
---
# <a name="xamarinforms-visual"></a>Xamarin.Forms のビジュアル

Xamarin.Forms Visual により、選択的に適用するレンダラー `VisualElement` Xamarin.Forms コントロールのサブクラス化することがなく、オブジェクト。 指定する任意のレンダラーを`VisualType`の一部として、`ExportRendererAttribute`は既定のレンダラーではなく、レンダラーのビューに使用されます。 レンダラーの選択時に、`Visual`ビューのプロパティを検査および表示機能の選択プロセスに含まれています。

> [!IMPORTANT]
> 現在、`Visual`コントロールが表示されたらが、これは将来のリリースで変更後に変更することはできません。

Xamarin.Forms には、素材のデザインに基づく、視覚的な外観が含まれています。 詳細については、次を参照してください。 [Xamarin.Forms マテリアル Visual](material-visual.md)します。

## <a name="create-a-visual"></a>ビジュアルを作成します。

派生した型を作成、クロス プラットフォーム ライブラリ`IVisual`:

```csharp
public class CustomVisual : IVisual
{
}
```

## <a name="register-the-visual-type"></a>ビジュアルの種類を登録します。

必要なビューのレンダラーを作成し、プラットフォームの一部として、ビジュアルの種類を登録`ExportRenderAttribute`:

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ContentPage), typeof(CustomPageRenderer), new[] { typeof(CustomVisual) })]
namespace MyApp.Android
{
    public class CustomPageRenderer : PageRenderer
    {
        ...
    }
}
```

## <a name="consume-the-visual"></a>ビジュアルを使用します。

アプリケーションを有効にでカスタム ビジュアルを使用できます、`Visual`プロパティ。

```xaml
<ContentPage ...
             Visual="Custom">
    ...
</ContentPage>
```

同等の C# コードに示します。

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = new CustomVisual();
```

## <a name="register-a-name-for-a-visual"></a>ビジュアルの名前を登録します。

異なる Visual ライブラリ間を競合に置くことがあります。 または、おそらくする別の名前でビジュアルを参照してください。 このシナリオでは、所与のビジュアルに別の名前を登録するのにアセンブリの属性を使用できます。

```csharp
[assembly: Visual("MyMaterialName", typeof(VisualMarker.MaterialVisual))]
```

カスタム ビジュアルは、その登録名を使用できます。

```xaml
<ContentPage ...
             Visual="MyMaterialName">
    ...
</ContentPage>
````

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms マテリアル Visual](material-visual.md)
- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
