---
title: 関連付けられた動作
description: 関連付けられた動作は、1 つまたは複数の添付プロパティを持つ静的クラスです。 この記事では、作成および接続されている動作を使用する方法を示します。
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 32573ac3ed0dfecf8ddf1c731613c9a5f88fb1e7
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/07/2018
ms.locfileid: "34845994"
---
# <a name="attached-behaviors"></a>関連付けられた動作

_関連付けられた動作は、1 つまたは複数の添付プロパティを持つ静的クラスです。この記事では、作成および接続されている動作を使用する方法を示します。_

## <a name="overview"></a>概要

添付プロパティは、特殊な種類のバインド可能プロパティです。 1 つのクラスで定義されているが、他のオブジェクトにアタッチされ、XAML で認識可能なクラスとピリオドで区切ったプロパティ名を含む属性として。

添付プロパティを定義できます、`propertyChanged`コントロールにプロパティを設定する場合などのプロパティの値が変更されたときに実行されるデリゲート。 ときに、`propertyChanged`デリゲートが実行されるへの参照をアタッチされる、コントロールとプロパティの新旧の値を含むパラメーターに成功します。 プロパティは、次のように渡される参照を操作することによってに関連付けられているコントロールに新しい機能を追加するは、このデリゲートを使用できます。

1. `propertyChanged`デリゲートとして受信されるコントロールの参照にキャスト、 [ `BindableObject`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)動作はコントロールの種類を強化するために設計されています。
1. `propertyChanged`デリゲートが動作のコア機能を実装する、このコントロールによって公開されているイベントのコントロール、またはレジスタ イベント ハンドラーのメソッドを呼び出す、コントロールのプロパティを変更します。

関連付けられた動作に問題がで定義されていること、`static`クラスと`static`プロパティとメソッド。 これによりした状態にある接続されている動作を作成するが困難です。 さらに、Xamarin.Forms の動作は接続されている動作を置き換え動作の構築に優先のアプローチとしています。 Xamarin.Forms の動作に関する詳細については、次を参照してください。 [Xamarin.Forms 動作](~/xamarin-forms/app-fundamentals/behaviors/creating.md)と[再利用可能な動作](~/xamarin-forms/app-fundamentals/behaviors/reusable/index.md)です。

## <a name="creating-an-attached-behavior"></a>接続されている動作を作成します。

サンプル アプリケーションは、`NumericValidationBehavior`にユーザーが入力した値が強調表示されますが、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)にない場合は赤で制御、`double`です。 次のコード例では、動作が表示されます。

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

`NumericValidationBehavior`クラスには、添付プロパティ名前にはが含まれています。`AttachBehavior`で、 `static` get アクセス操作子および set アクセス操作子を追加またはアタッチがコントロールする動作の削除を制御します。 この添付プロパティに、プロパティの値が変更された時に実行される `OnAttachBehaviorChanged` メソッドを登録します。 このメソッドを登録またはのイベント ハンドラーを登録解除、 [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/)の値に基づく、イベント、`AttachBehavior`添付プロパティ。 動作のコア機能が用意されて、`OnEntryTextChanged`に入力された値を解析するメソッド、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)ユーザー、およびセットによって、`TextColor`プロパティ値がない場合は赤を`double`です。

## <a name="consuming-an-attached-behavior"></a>関連付けられた動作の使用

`NumericValidationBehavior`クラスを追加することで使用できる、`AttachBehavior`添付プロパティを[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールが次の XAML コードの例に示されています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

該当するショートカットは、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) c# では次のコード例で示すようにします。

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

実行時に、動作が応答する、コントロールとの対話動作実装に従ってします。 次のスクリーン ショットは、無効な入力に応答して接続されている動作を示しています。

[![](attached-images/screenshots-sml.png "サンプル アプリケーションが接続されている動作で")](attached-images/screenshots.png#lightbox "サンプル アプリケーションに関連付けられた動作")

> [!NOTE]
> 関連付けられた動作は特定のコントロール型 (または多くのコントロールに適用できるスーパークラス) に書き込まれ、互換性のあるコントロールにのみ追加する必要があります。 互換性のないコントロールにビヘイビアーをアタッチしようとしています。 不明の動作が発生し、動作の実装に依存します。

### <a name="removing-an-attached-behavior-from-a-control"></a>コントロールから、接続されている動作を削除します。

`NumericValidationBehavior`クラスは、設定によってコントロールから削除することができます、`AttachBehavior`添付プロパティ`false`XAML コードの例を次に示すように、します。

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

該当するショートカットは、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) c# では次のコード例で示すようにします。

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

、実行時に、`OnAttachBehaviorChanged`メソッドになるときに実行の値、`AttachBehavior`に添付プロパティを設定`false`です。 `OnAttachBehaviorChanged`メソッドは、登録を解除のイベント ハンドラー、 [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/)イベント、ユーザーがコントロールと対話するので、動作が実行されない場合はことを確認します。

## <a name="summary"></a>まとめ

この記事では、作成および接続されている動作を使用する方法を示しました。 接続されている動作は`static`1 つまたは複数の添付プロパティを持つクラス。


## <a name="related-links"></a>関連リンク

- [接続されている動作 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/attachednumericvalidationbehavior/)
