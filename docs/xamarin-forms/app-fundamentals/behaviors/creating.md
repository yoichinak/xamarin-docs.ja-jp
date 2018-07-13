---
title: Xamarin.Forms の動作
description: Xamarin.Forms の動作が動作または動作から派生することによって作成された<T>クラス。 この記事では、作成および Xamarin.Forms の動作を使用する方法を示します。
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 7e057567ec0bb72e9bcc016d4a9fef3af78a3ea1
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998896"
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms の動作

_Xamarin.Forms の動作が動作または動作から派生することによって作成された<T>クラス。この記事では、作成および Xamarin.Forms の動作を使用する方法を示します。_

## <a name="overview"></a>概要

Xamarin.Forms の動作を作成するプロセスは次のとおりです。

1. 継承するクラスを作成、 [ `Behavior` ](xref:Xamarin.Forms.Behavior)または[ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)クラス、`T`動作を適用するコントロールの種類です。
1. 上書き、 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))特別な設定を実行するメソッド。
1. 上書き、 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))必要なクリーンアップを実行するメソッド。
1. 動作のコア機能を実装します。

これは、結果、次のコード例に示すように構造体。

```csharp
public class CustomBehavior : Behavior<View>
{
    protected override void OnAttachedTo (View bindable)
    {
        base.OnAttachedTo (bindable);
        // Perform setup
    }

    protected override void OnDetachingFrom (View bindable)
    {
        base.OnDetachingFrom (bindable);
        // Perform clean up
    }

    // Behavior implementation
}
```

[ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))動作がコントロールにアタッチされた後にすぐにメソッドが発生します。 このメソッドは、先がアタッチされている、イベント ハンドラーを登録またはその他の動作の機能をサポートするために必要なセットアップの実行に使用できるコントロールへの参照を受け取ります。 たとえば、コントロール上のイベントにサブスクライブする可能性があります。 動作の機能は、イベントのイベント ハンドラーで実装されます。

[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))メソッドは、動作がコントロールから削除されるときに発生します。 このメソッドは、先がアタッチされているし、必要なクリーンアップを実行するために使用コントロールへの参照を受け取ります。 たとえば、メモリ リークを防ぐためにコントロールのイベントの購読を解除する可能性があります。

動作をアタッチすることで使用できます、 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors)適切なコントロールのコレクション。

## <a name="creating-a-xamarinforms-behavior"></a>Xamarin.Forms の動作を作成します。

サンプル アプリケーションでは、`NumericValidationBehavior`にユーザーが入力した値が強調表示する、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールを赤でない場合、`double`します。 動作は、次のコード例に示されます。

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    protected override void OnAttachedTo(Entry entry)
    {
        entry.TextChanged += OnEntryTextChanged;
        base.OnAttachedTo(entry);
    }

    protected override void OnDetachingFrom(Entry entry)
    {
        entry.TextChanged -= OnEntryTextChanged;
        base.OnDetachingFrom(entry);
    }

    void OnEntryTextChanged(object sender, TextChangedEventArgs args)
    {
        double result;
        bool isValid = double.TryParse (args.NewTextValue, out result);
        ((Entry)sender).TextColor = isValid ? Color.Default : Color.Red;
    }
}
```

`NumericValidationBehavior`から派生した、 [ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)クラス、場所`T`は、 [ `Entry`](xref:Xamarin.Forms.Entry)します。 [ `OnAttachedTo` ](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject))メソッドのイベント ハンドラーの登録、 [ `TextChanged` ](xref:Xamarin.Forms.Entry.TextChanged)イベントで、 [ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) を登録解除メソッド`TextChanged`メモリを防ぐためにイベントのリークが発生します。 動作のコア機能によって提供されます、`OnEntryTextChanged`にユーザーが入力した値を解析するメソッド、 `Entry`、設定と、 [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor)プロパティ値がない場合は赤、 `double`。

> [!NOTE]
> Xamarin.Forms が設定されていない、`BindingContext`動作の動作を共有し、スタイルを介して複数のコントロールに適用されるためです。

## <a name="consuming-a-xamarinforms-behavior"></a>Xamarin.Forms の動作の使用

すべての Xamarin.Forms コントロールは、 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors)追加先となる 1 つまたは複数の動作は、次の XAML コード例に示すようコレクション。

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

相当[ `Entry` ](xref:Xamarin.Forms.Entry) c# では次のコード例で示すようにします。

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

実行時に、動作は動作の実装に応じて、コントロールとの対話に応答します。 次のスクリーン ショットは、無効な入力に応答して動作を示しています。

[![](creating-images/screenshots-sml.png "Xamarin.Forms の動作でアプリケーションをサンプル")](creating-images/screenshots.png#lightbox "Xamarin.Forms の動作とアプリケーションのサンプル")

> [!NOTE]
> 動作は特定のコントロール型 (または多くのコントロールに適用できるスーパークラス) に書き込まれ、互換性のあるコントロールにのみ追加する必要があります。 互換性のないコントロールに動作をアタッチしようとして、スローされる例外が発生します。

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>スタイルと Xamarin.Forms の動作の使用

動作は、明示的または暗黙的なスタイルによっても使用できます。 ただし、設定するスタイルを作成、 [ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors)プロパティが読み取り専用であるために、コントロールのプロパティにことはできません。 ソリューションでは、動作クラスの追加と削除の動作を制御する添付プロパティを追加します。 プロセスは次のとおりです。

1. 添付プロパティを追加または削除がアタッチされている動作では、このコントロールの動作の制御に使用される動作のクラスに追加します。 添付プロパティを登録することを確認、`propertyChanged`プロパティの値が変更されたときに実行されるデリゲート。
1. 作成、`static`添付プロパティのゲッターとセッター。
1. 内のロジックを実装、`propertyChanged`デリゲートを追加して、動作を削除します。

次のコード例の追加と削除を制御する添付プロパティを示しています、 `NumericValidationBehavior`:

```csharp
public class NumericValidationBehavior : Behavior<Entry>
{
    public static readonly BindableProperty AttachBehaviorProperty =
        BindableProperty.CreateAttached ("AttachBehavior", typeof(bool), typeof(NumericValidationBehavior), false, propertyChanged: OnAttachBehaviorChanged);

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
            entry.Behaviors.Add (new NumericValidationBehavior ());
        } else {
            var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
            if (toRemove != null) {
                entry.Behaviors.Remove (toRemove);
            }
        }
    }
    ...
}
```

`NumericValidationBehavior`クラスには、という名前の添付プロパティが含まれています。`AttachBehavior`で、 `static` getter および setter を追加または削除のアタッチはコントロールの動作を制御します。 この添付プロパティに、プロパティの値が変更された時に実行される `OnAttachBehaviorChanged` メソッドを登録します。 このメソッドを追加またはの値に基づいて、コントロールの動作を削除、`AttachBehavior`添付プロパティ。

次のコード例は、*明示的な*のスタイル、`NumericValidationBehavior`を使用して、`AttachBehavior`添付プロパティ、およびに適用できる[ `Entry` ](xref:Xamarin.Forms.Entry)コントロール。

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](xref:Xamarin.Forms.Style)に適用できる、 [ `Entry` ](xref:Xamarin.Forms.Entry)コントロールを設定してその[ `Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティを`Style`インスタンスを使用して、`StaticResource`マークアップ拡張機能で、次のコード例で示した。

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

スタイルの詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/styles/index.md)します。

> [!NOTE]
> 状態にあるバインド可能なプロパティが設定または作成した場合の動作を XAML でクエリを実行する動作を追加できますが、これらは共有できません内のコントロール間を`Style`で、`ResourceDictionary`します。

### <a name="removing-a-behavior-from-a-control"></a>コントロールから、動作を削除します。

[ `OnDetachingFrom` ](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject))動作は、コントロールから削除され、メモリ リークを防ぐために、イベントからサブスクライブを解除するなど、必要なクリーンアップを実行するために使用すると、メソッドが発生します。 ただし、動作は暗黙的に削除されませんコントロールからしない限り、コントロールの[ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors)によってコレクションが変更された、`Remove`または`Clear`メソッド。 次のコード例を示してから、コントロールの特定の動作を削除する`Behaviors`コレクション。

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

または、コントロールの[ `Behaviors` ](xref:Xamarin.Forms.VisualElement.Behaviors)の次のコード例に示す、コレクションをクリアできます。

```csharp
entry.Behaviors.Clear();
```

さらに、ある動作は暗黙的に削除されませんコントロールからページがナビゲーション スタックからポップされます。 注意してください。 代わりに、明示的に削除するページのスコープを外れる前に。

## <a name="summary"></a>まとめ

この記事では、作成し、Xamarin.Forms の動作を使用する方法を示しました。 Xamarin.Forms の動作がから派生することによって作成された、 [ `Behavior` ](xref:Xamarin.Forms.Behavior)または[ `Behavior<T>` ](xref:Xamarin.Forms.Behavior`1)クラス。


## <a name="related-links"></a>関連リンク

- [Xamarin.Forms の動作 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [Xamarin.Forms の動作をスタイル (サンプル) を適用](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [動作](xref:Xamarin.Forms.Behavior)
- [動作<T>](xref:Xamarin.Forms.Behavior`1)
