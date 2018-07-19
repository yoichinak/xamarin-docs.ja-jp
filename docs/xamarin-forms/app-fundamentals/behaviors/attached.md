---
title: 関連付けられた動作
description: 関連付けられた動作とは、1 つまたは複数の添付プロパティを持つ静的クラスです。 この記事では、作成および関連付けられた動作を使用する方法を示します。
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 2c9bd9ad4e7572b9eae6f0073da8a2c8f1e7c9fc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995347"
---
# <a name="attached-behaviors"></a>関連付けられた動作

_関連付けられた動作とは、1 つまたは複数の添付プロパティを持つ静的クラスです。この記事では、作成および関連付けられた動作を使用する方法を示します。_

## <a name="overview"></a>概要

添付プロパティは、特殊な種類のバインド可能なプロパティです。 1 つのクラスで定義されているが、他のオブジェクトにアタッチされていると、クラスとピリオドで区切ったプロパティ名が含まれている属性としては XAML で認識されます。

添付プロパティを定義できますを`propertyChanged`など、コントロールのプロパティを設定すると、プロパティの値が変更されたときに実行されるデリゲート。 ときに、`propertyChanged`デリゲートの実行への参照をアタッチされる、コントロールとプロパティの新旧の値が含まれているパラメーターに成功します。 このデリゲートは、プロパティは、次のように渡される参照を操作することによってに関連付けられているコントロールに新しい機能を追加できます。

1. `propertyChanged`デリゲートとして受信されるコントロールの参照にキャスト、 [ `BindableObject`](xref:Xamarin.Forms.BindableObject)動作はコントロールの種類を強化するために設計されています。
1. `propertyChanged`デリゲートの動作のコア機能を実装するために、コントロールによって公開されるイベントのコントロール、またはレジスタ イベント ハンドラーのメソッドを呼び出す、コントロールのプロパティを変更します。

関連付けられた動作の問題で定義されていること、`static`クラスと`static`プロパティとメソッド。 状態にある関連付けられた動作の作成を困難になります。 さらに、Xamarin.Forms の動作が動作の構築に推奨されるアプローチとして、関連付けられた動作が置き換えられています。 Xamarin.Forms の動作の詳細については、次を参照してください。 [Xamarin.Forms の動作](~/xamarin-forms/app-fundamentals/behaviors/creating.md)と[再利用可能なビヘイビアー](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)します。

## <a name="creating-an-attached-behavior"></a>接続の動作を作成します。

サンプル アプリケーションでは、`NumericValidationBehavior`にユーザーが入力した値が強調表示する、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールを赤でない場合、`double`します。 動作は、次のコード例に示されます。

```csharp
public static class NumericValidationBehavior
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached (
            "AttachBehavior",
            typeof(bool),
            typeof(NumericValidationBehavior),
            false,
            propertyChanged:OnAttachBehaviorChanged);

    public static bool GetAttachBehavior (BindableObject view)
    {
        return (bool)view.GetValue (AttachBehaviorProperty);
    }

    public static void SetAttachBehavior (BindableObject view, bool value)
    {
        view.SetValue (AttachBehaviorProperty, value);
    }

    static void OnAttachBehaviorChanged (BindableObject view, object oldValue, object newValue)
    {
        var entry = view as Entry;
        if (entry == null) {
            return;
        }

        bool attachBehavior = (bool)newValue;
        if (attachBehavior) {
            entry.TextChanged += OnEntryTextChanged;
        } else {
            entry.TextChanged -= OnEntryTextChanged;
        }
    }

    static void OnEntryTextChanged (object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior`クラスには、という名前の添付プロパティが含まれています。`AttachBehavior`で、 `static` getter および setter を追加または削除のアタッチはコントロールの動作を制御します。 この添付プロパティに、プロパティの値が変更された時に実行される `OnAttachBehaviorChanged` メソッドを登録します。 このメソッドを登録またはのイベント ハンドラーを登録解除、 [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged)の値に基づく、イベント、`AttachBehavior`添付プロパティ。 動作のコア機能によって提供されます、`OnEntryTextChanged`メソッドに入力された値を[ `Entry` ](xref:Xamarin.Forms.Entry)ユーザー、およびセットによって、`TextColor`プロパティ値がない場合は赤、`double`します。

## <a name="consuming-an-attached-behavior"></a>添付の動作の使用

`NumericValidationBehavior`クラスを追加することで使用できる、`AttachBehavior`添付プロパティを[ `Entry` ](xref:Xamarin.Forms.Entry)コントロールが次の XAML コード例に示されています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

相当[ `Entry` ](xref:Xamarin.Forms.Entry) c# では次のコード例で示すようにします。

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

、実行時に、動作は動作の実装に応じて、コントロールとの対話に応答します。 次のスクリーン ショットは、無効な入力に応答して接続されている動作を示しています。

[![](attached-images/screenshots-sml.png "サンプル アプリケーションが接続されている動作で")](attached-images/screenshots.png#lightbox "サンプル アプリケーションで接続されている動作")

> [!NOTE]
> 関連付けられた動作は特定のコントロール型 (または多くのコントロールに適用できるスーパークラス) に書き込まれ、互換性のあるコントロールにのみ追加する必要があります。 原因不明の動作が発生、互換性のないコントロールに動作をアタッチしようとして、動作の実装に依存します。

### <a name="removing-an-attached-behavior-from-a-control"></a>コントロールから、接続されている動作を削除します。

`NumericValidationBehavior`クラスは、設定によってコントロールから削除することができます、`AttachBehavior`添付プロパティを`false`XAML コードの例を次に示すように、します。

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

相当[ `Entry` ](xref:Xamarin.Forms.Entry) c# では次のコード例で示すようにします。

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

実行時に、`OnAttachBehaviorChanged`メソッドになる時に実行の値、`AttachBehavior`添付プロパティに設定されて`false`します。 `OnAttachBehaviorChanged`メソッドは、登録を解除のイベント ハンドラー、 [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged)イベント、ユーザーがコントロールと対話するので、動作が実行されない場合はことを確認します。

## <a name="summary"></a>まとめ

この記事では、作成および関連付けられた動作を使用する方法を示しました。 関連付けられた動作が`static`1 つまたは複数の添付プロパティを持つクラス。


## <a name="related-links"></a>関連リンク

- [関連付けられた動作 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
