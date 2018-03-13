---
title: "Xamarin.Forms の動作"
description: "Xamarin.Forms の動作を動作または動作から派生することで作成する<T>クラスです。 この記事では、作成および Xamarin.Forms ビヘイビアーを使用する方法を示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
ms.openlocfilehash: 160dd4b2326529abbb456e77391f0f73ee374f50
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="xamarinforms-behaviors"></a>Xamarin.Forms の動作

_Xamarin.Forms の動作を動作または動作から派生することで作成する<T>クラスです。この記事では、作成および Xamarin.Forms ビヘイビアーを使用する方法を示します。_

## <a name="overview"></a>概要

Xamarin.Forms の動作を作成するプロセスは次のとおりです。

1. 継承するクラスを作成、 [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)または[ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)クラス、場所`T`動作を適用するコントロールの種類です。
1. 上書き、 [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)特別な設定を実行するメソッド。
1. 上書き、 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)必要なクリーンアップを実行するメソッド。
1. 動作のコア機能を実装します。

これは、結果は、次のコード例に示すように構造になります。

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

[ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)動作がコントロールに関連付けられた直後後にメソッドが起動します。 このメソッドは、先がアタッチされている、イベント ハンドラーを登録またはその他の動作の機能をサポートするために必要なセットアップを実行に使用できるコントロールへの参照を受け取ります。 たとえば、コントロールのイベントをサブスクライブする可能性があります。 イベントのイベント ハンドラーの動作の機能を実装する場合とします。

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)メソッドは、動作がコントロールから削除されたときに発生します。 このメソッドは、コントロールへの参照先がアタッチされているし、必要なクリーンアップを実行するために使用を受け取ります。 たとえば、メモリ リークを防止するためのコントロールのイベントからサブスクリプションを解除する可能性があります。

アタッチすることによって動作し、使用できる、 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/)適切なコントロールのコレクション。

## <a name="creating-a-xamarinforms-behavior"></a>Xamarin.Forms の動作を作成します。

サンプル アプリケーションは、`NumericValidationBehavior`にユーザーが入力した値が強調表示されますが、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)にない場合は赤で制御、`double`です。 次のコード例では、動作が表示されます。

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

`NumericValidationBehavior`から派生した、 [ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)クラス、場所`T`は、 [ `Entry`](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)です。 [ `OnAttachedTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnAttachedTo/p/Xamarin.Forms.BindableObject/)メソッドのイベント ハンドラーの登録、 [ `TextChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Entry.TextChanged/)イベントと、 [ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/) を登録解除メソッド`TextChanged`メモリを防ぐためにイベントのリークが発生します。 動作のコア機能が用意されて、`OnEntryTextChanged`にユーザーが入力した値を解析するメソッド、 `Entry`、設定と、 [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/)プロパティ値がない場合は赤を`double`です。

> [!NOTE]
> Xamarin.Forms は設定されません、`BindingContext`動作の動作を共有し、スタイルを介して複数のコントロールに適用されるためです。

## <a name="consuming-a-xamarinforms-behavior"></a>Xamarin.Forms 動作の使用

Xamarin.Forms のすべてのコントロールは、 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/)するを 1 つまたは複数の動作を追加できる、次の XAML コードの例で示したように、コレクション。

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

該当するショートカットは、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) c# では次のコード例で示すようにします。

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

実行時に、動作はに従って動作実装では、コントロールとの対話に応答します。 次のスクリーン ショットは、無効な入力に応答して動作を示しています。

[![](creating-images/screenshots-sml.png "サンプル Xamarin.Forms 動作を持つアプリケーション")](creating-images/screenshots.png#lightbox "Xamarin.Forms 動作を持つアプリケーションのサンプル")

> [!NOTE]
> 動作は特定のコントロール型 (または多くのコントロールに適用できるスーパークラス) に書き込まれ、互換性のあるコントロールにのみ追加する必要があります。 互換性のないコントロールにビヘイビアーをアタッチしようと、スローされた例外が発生します。

### <a name="consuming-a-xamarinforms-behavior-with-a-style"></a>スタイルと Xamarin.Forms の動作の使用

動作は、明示的または暗黙的なスタイルによっても使用できます。 ただし、設定するスタイルを作成する、 [ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/)コントロールのプロパティは、プロパティが読み取り専用であるためです。 ソリューションでは、添付プロパティを追加および削除する動作を制御する behavior クラスに追加します。 プロセスは次のとおりです。

1. 添付プロパティを追加または削除の割り当て動作が、コントロールの動作の制御に使用される動作のクラスに追加します。 添付プロパティを登録することを確認、`propertyChanged`プロパティの値が変更されたときに実行されるデリゲート。
1. 作成、 `static` get アクセス操作子と添付プロパティの set アクセス操作子。
1. ロジックを実装、`propertyChanged`デリゲートを追加して、動作を削除します。

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

`NumericValidationBehavior`クラスには、添付プロパティ名前にはが含まれています。`AttachBehavior`で、 `static` get アクセス操作子および set アクセス操作子を追加またはアタッチがコントロールする動作の削除を制御します。 これは、添付プロパティのレジスタ、`OnAttachBehaviorChanged`プロパティの値が変更されたときに実行されるメソッド。 このメソッドを追加または削除動作の値に基づいて、コントロールを`AttachBehavior`添付プロパティ。

次のコード例は、*明示的な*のスタイル、`NumericValidationBehavior`を使用して、`AttachBehavior`接続されたプロパティ、およびに適用できる[ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロール。

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)に適用できる、 [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/)コントロールを設定してその[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティを`Style`インスタンスを使用して、`StaticResource`マークアップ拡張機能で、次のコード例で示したようにします。

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

スタイルの詳細については、次を参照してください。[スタイル](~/xamarin-forms/user-interface/styles/index.md)です。

> [!NOTE]
> 状態にあるバインド可能なプロパティが設定または作成した場合の動作を XAML では、クエリを実行する動作を追加できますが、それらを共有しない内のコントロール間、`Style`で、`ResourceDictionary`です。

### <a name="removing-a-behavior-from-a-control"></a>コントロールの動作を削除します。

[ `OnDetachingFrom` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Behavior%3CT%3E.OnDetachingFrom/p/Xamarin.Forms.BindableObject/)動作は、コントロールから削除され、メモリ リークを防ぐためにイベントからサブスクライブを解除するなど、必要なクリーンアップを実行するために使用すると、メソッドが発生します。 ただし、動作は暗黙的に削除されませんコントロールからしない限り、コントロールの[ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/)でコレクションが変更、`Remove`または`Clear`メソッドです。 次のコード例に示しますからコントロールの特定の動作を削除する`Behaviors`コレクション。

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

また、コントロールの[ `Behaviors` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Behaviors/)次のコード例で示したように、コレクションをクリアできます。

```csharp
entry.Behaviors.Clear();
```

さらに、動作が削除されなかったことに暗黙的にコントロールからページがナビゲーション スタックからポップされたときに注意してください。 代わりに、明示的に削除するページがスコープ外に移動する前にします。

## <a name="summary"></a>まとめ

この記事では、作成および Xamarin.Forms ビヘイビアーを使用する方法を示しました。 派生することによって Xamarin.Forms 動作を作成する、 [ `Behavior` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)または[ `Behavior<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)クラスです。


## <a name="related-links"></a>関連リンク

- [Xamarin.Forms 動作 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehavior/)
- [(サンプル) のスタイルと Xamarin.Forms の動作が適用されます。](https://developer.xamarin.com/samples/xamarin-forms/behaviors/numericvalidationbehaviorstyle/)
- [動作](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior/)
- [動作<T>](https://developer.xamarin.com/api/type/Xamarin.Forms.Behavior%3CT%3E/)
