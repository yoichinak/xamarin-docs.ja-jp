---
title: アタッチされたビヘイビアー
description: アタッチされたビヘイビアーは、1 つ以上のプロパティがアタッチされた静的クラスです。 この記事では、アタッチされたビヘイビアーを作成して使用する方法を示します。
ms.prod: xamarin
ms.assetid: ECEE6AEC-44FA-4AF7-BAD0-88C6EE48422E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 6c8c43a877a102d9e16070da00813b00e7ef1c38
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91561366"
---
# <a name="attached-behaviors"></a>アタッチされたビヘイビアー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-attachednumericvalidationbehavior)

_アタッチされたビヘイビアーは、1 つ以上のプロパティがアタッチされた静的クラスです。この記事では、アタッチされたビヘイビアーを作成して使用する方法を示します。_

## <a name="overview"></a>概要

添付プロパティは、特殊な種類のバインド可能プロパティです。 1 つのクラスで定義される一方で他のオブジェクトにアタッチされ、XAML 内でピリオドで区切られたクラスとプロパティ名が含まれる属性として認識されます。

添付プロパティでは、プロパティがコントロールに設定されたときなど、プロパティの値が変更されたときに実行される `propertyChanged` のデリゲートを定義できます。 `propertyChanged` のデリゲートが実行されると、アタッチされているコントロールへの参照と、プロパティの古い値と新しい値を含むパラメーターが渡されます。 このデリゲートを使用すると、次のように、渡された参照を操作することで、プロパティがアタッチされているコントロールに新しい機能を追加できます。

1. `propertyChanged` のデリゲートによって、[`BindableObject`](xref:Xamarin.Forms.BindableObject) として受け取られるコントロールの参照が、ビヘイビアーが強化されるように設計されたコントロールの種類にキャストされます。
1. `propertyChanged` のデリゲートによってコントロールのプロパティ変更、コントロールのメソッド呼び出し、またはコントロールで公開されているイベントに対するイベント ハンドラーの登録が行われ、コア ビヘイビアー機能が実装されます。

アタッチされたビヘイビアーに関する問題は、`static` プロパティおよびメソッドを使用して `static` クラスで定義されている点です。 そのため、状態があるアタッチされたビヘイビアーを作成することは困難です。 さらに、ビヘイビアー構築の推奨されるアプローチとして、Xamarin.Forms ビヘイビアーによって、アタッチされたビヘイビアーが置き換えられます。 Xamarin.Forms ビヘイビアーの詳細については、[Xamarin.Forms のビヘイビアー](~/xamarin-forms/app-fundamentals/behaviors/creating.md)に関する記事を参照してください。

## <a name="creating-an-attached-behavior"></a>アタッチされたビヘイビアーを作成する

サンプル アプリケーションでは、`NumericValidationBehavior` の例を示しています。ユーザーが [`Entry`](xref:Xamarin.Forms.Entry) コントロールに入力した値が `double` でない場合に、その値は赤色で強調表示されます。 このビヘイビアーを次のコード例に示します。

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

`NumericValidationBehavior` クラスには、`static` getter と setter がある `AttachBehavior` という名前の添付プロパティが含まれています。このプロパティにより、それがアタッチされるコントロールのビヘイビアーの追加または削除が制御されます。 この添付プロパティにより、プロパティの値が変更されるときに実行される `OnAttachBehaviorChanged` メソッドが登録されます。 このメソッドによって、`AttachBehavior` 添付プロパティの値に基づいて [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) イベントのイベント ハンドラーが登録または登録解除されます。 `OnEntryTextChanged` メソッドにより、ビヘイビアーのコア機能が提供され、ユーザーが [`Entry`](xref:Xamarin.Forms.Entry) に入力した値が解析され、その値が `double` でなければ `TextColor` プロパティが赤に設定されます。

## <a name="consuming-an-attached-behavior"></a>アタッチされたビヘイビアーを使用する

`NumericValidationBehavior` クラスを使用するには、次の XAML コード例に示すように、`AttachBehavior` 添付プロパティを [`Entry`](xref:Xamarin.Forms.Entry) コントロールに追加します。

```xaml
<ContentPage ... xmlns:local="clr-namespace:WorkingWithBehaviors;assembly=WorkingWithBehaviors" ...>
    ...
    <Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="true" />
    ...
</ContentPage>
```

C# での同等の [`Entry`](xref:Xamarin.Forms.Entry) を次のコード例に示します。

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, true);
```

実行時、ビヘイビアーは、ビヘイビアーの実装に従って、コントロールとのやりとりに応答します。 次のスクリーン ショットは、無効な入力に応答しているアタッチされたビヘイビアーを示しています。

[![アタッチされたビヘイビアーを使用したサンプル アプリケーション](attached-images/screenshots-sml.png)](attached-images/screenshots.png#lightbox "アタッチされたビヘイビアーを使用したサンプル アプリケーション")

> [!NOTE]
> アタッチされたビヘイビアーは特定のコントロールの種類 (または複数のコントロールに適用できるスーパークラス) に対して記述され、互換性のあるコントロールにのみ追加する必要があります。 互換性のないコントロールにビヘイビアーをアタッチしようとすると、不明なビヘイビアーになり、結果はビヘイビアーの実装によって変わります。

### <a name="removing-an-attached-behavior-from-a-control"></a>アタッチされたビヘイビアーをコントロールから削除する

次の XAML コード例に示すように、`NumericValidationBehavior` クラスをコントロールから削除するには `AttachBehavior` 添付プロパティを `false` に設定します。

```xaml
<Entry Placeholder="Enter a System.Double" local:NumericValidationBehavior.AttachBehavior="false" />
```

C# での同等の [`Entry`](xref:Xamarin.Forms.Entry) を次のコード例に示します。

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
NumericValidationBehavior.SetAttachBehavior (entry, false);
```

実行時に、`AttachBehavior` 添付プロパティの値が `false` に設定されていると、`OnAttachBehaviorChanged` メソッドが実行されます。 `OnAttachBehaviorChanged` メソッドによって [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) イベントのイベント ハンドラーの登録が解除され、ユーザーがコントロールを操作してもそのビヘイビアーが実行されなくなります。

## <a name="summary"></a>まとめ

この記事では、アタッチされたビヘイビアーを作成して使用する方法を説明しました。 アタッチされたビヘイビアーは、1 つ以上のプロパティがアタッチされた `static` クラスです。

## <a name="related-links"></a>関連リンク

- [アタッチされたビヘイビアー (サンプル)](/samples/xamarin/xamarin-forms-samples/behaviors-attachednumericvalidationbehavior)