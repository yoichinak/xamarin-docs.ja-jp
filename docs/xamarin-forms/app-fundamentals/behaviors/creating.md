---
title: Xamarin.Forms ビヘイビアーの作成
description: Xamarin.Forms のビヘイビアーは、Behavior または Behavior<T> クラスから派生させることで作成されます。 この記事では、Xamarin.Forms のビヘイビアーを作成して使用する方法を示します。
ms.prod: xamarin
ms.assetid: 300C16FE-A7E0-445B-9099-8E93ABB6F73D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/06/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 53df681e79c4c233a8908764d31c8ee416c8a621
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91561171"
---
# <a name="create-no-locxamarinforms-behaviors"></a>Xamarin.Forms ビヘイビアーの作成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/behaviors-numericvalidationbehavior)

_Xamarin.Forms のビヘイビアーは、Behavior または Behavior&lt;T&gt; クラスから派生させることで作成されます。この記事では、Xamarin.Forms のビヘイビアーを作成して使用する方法を示します。_

## <a name="overview"></a>概要

Xamarin.Forms のビヘイビアーを作成するプロセスは次のとおりです。

1. [`Behavior`](xref:Xamarin.Forms.Behavior) または [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスから継承されるクラスを作成します。`T` は、このビヘイビアーを適用するコントロールの種類です。
1. 必要な設定を実行するように [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) メソッドをオーバーライドします。
1. 必要なクリーンアップを実行するように [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) メソッドをオーバーライドします。
1. ビヘイビアーのコア機能を実装します。

これで、次のコード例に示す構造になります。

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

ビヘイビアーがコントロールにアタッチされると、すぐに [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) メソッドが実行されます。 このメソッドがアタッチされているコントロールへの参照を受け取り、イベント ハンドラーの登録やビヘイビアー機能をサポートするために必要なその他の設定を実行するために使用できます。 たとえば、コントロールのイベントをサブスクライブできます。 その後、イベント用のイベント ハンドラー内にビヘイビアー機能を実装します。

ビヘイビアーがコントロールから削除されると、[`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) メソッドが実行されます。 このメソッドがアタッチされているコントロールへの参照を受け取り、必要なクリーンアップを実行するために使用されます。 たとえば、メモリ リークを防ぐためにコントロールのイベントのサブスクライブを解除できます。

その後、適切なコントロールの [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) コレクションにアタッチすることで、ビヘイビアーを使用できます。

## <a name="creating-a-no-locxamarinforms-behavior"></a>Xamarin.Forms ビヘイビアーの作成

サンプル アプリケーションでは、`NumericValidationBehavior` を示します。ここでは、ユーザーが [`Entry`](xref:Xamarin.Forms.Entry) コントロールに入力した値が `double` でない場合に、その値を赤色で強調表示します。 このビヘイビアーを次のコード例に示します。

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

`NumericValidationBehavior` は [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスから派生します。`T` は [`Entry`](xref:Xamarin.Forms.Entry) です。 メモリ リークを防ぐために `TextChanged` の登録を解除する [`OnAttachedTo`](xref:Xamarin.Forms.Behavior`1.OnAttachedTo(Xamarin.Forms.BindableObject)) method registers an event handler for the [`TextChanged`](xref:Xamarin.Forms.InputView.TextChanged) event, with the [`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) メソッド。 `OnEntryTextChanged` メソッドにより、ビヘイビアーのコア機能が提供され、ユーザーが `Entry` に入力した値が解析され、その値が `double` でなければ [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) プロパティが赤に設定されます。

> [!NOTE]
> ビヘイビアーはスタイルを利用して共有し、複数のコントロールに適用できるため、Xamarin.Forms では、ビヘイビアーの `BindingContext` は設定されません。

## <a name="consuming-a-no-locxamarinforms-behavior"></a>Xamarin.Forms ビヘイビアーの使用

次の XAML のコード例に示すように、すべての Xamarin.Forms コントロールには、1 つまたは複数のビヘイビアーを追加できる [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) コレクションがあります。

```xaml
<Entry Placeholder="Enter a System.Double">
    <Entry.Behaviors>
        <local:NumericValidationBehavior />
    </Entry.Behaviors>
</Entry>
```

C# での同等の [`Entry`](xref:Xamarin.Forms.Entry) を次のコード例に示します。

```csharp
var entry = new Entry { Placeholder = "Enter a System.Double" };
entry.Behaviors.Add (new NumericValidationBehavior ());
```

実行時、ビヘイビアーは、ビヘイビアーの実装に従って、コントロールとのやりとりに応答します。 次のスクリーン ショットで、無効な入力に応答しているビヘイビアーを示します。

[![Xamarin.Forms のビヘイビアーを使用したサンプル アプリケーション](creating-images/screenshots-sml.png)](creating-images/screenshots.png#lightbox "Xamarin.Forms のビヘイビアーを使用したサンプル アプリケーション動作")

> [!NOTE]
> ビヘイビアーは特定のコントロールの種類 (または複数のコントロールに適用できるスーパークラス) に対して記述され、互換性のあるコントロールにのみ追加する必要があります。 互換性のないコントロールにビヘイビアーをアタッチしようとすると、例外がスローされます。

### <a name="consuming-a-no-locxamarinforms-behavior-with-a-style"></a>スタイルによる Xamarin.Forms ビヘイビアーの使用

明示的または暗黙的なスタイルによってビヘイビアーを使用することもできます。 ただし、コントロールの [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) プロパティは読み取り専用であるため、このプロパティを設定するスタイルを作成することはできません。 解決策は、ビヘイビアーの追加と削除を制御する添付プロパティをビヘイビアー クラスに追加することです。 プロセスは、次のとおりです。

1. ビヘイビアーがアタッチされるコントロールへのビヘイビアーの追加または削除を制御するために使用するビヘイビアー クラスに添付プロパティを追加します。 この添付プロパティにより、プロパティの値が変更されたときに実行される `propertyChanged` デリゲートが確実に登録されるようにします。
1. 添付プロパティの `static` ゲッターとセッターを作成します。
1. `propertyChanged` デリゲートで、ビヘイビアーを追加または削除するロジックを実装します。

次のコード例で、`NumericValidationBehavior` の追加と削除を制御する添付プロパティを示します。

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

`NumericValidationBehavior` クラスには、`static` ゲッターとセッターがある `AttachBehavior` という名前の添付プロパティが含まれています。このプロパティにより、それが添付されるコントロールへのビヘイビアーの追加または削除が制御されます。 この添付プロパティにより、プロパティの値が変更されたときに実行される `OnAttachBehaviorChanged` デリゲートが登録されます。 このメソッドでは、`AttachBehavior` 添付プロパティの値に基づいて、ビヘイビアーの追加または削除が行われます。

次のコード例で、`AttachBehavior` 添付プロパティを使用する `NumericValidationBehavior` 用の "*明示的な*" スタイルで、[`Entry`](xref:Xamarin.Forms.Entry) コントロールに適用できるものを示します。

```xaml
<Style x:Key="NumericValidationStyle" TargetType="Entry">
    <Style.Setters>
        <Setter Property="local:NumericValidationBehavior.AttachBehavior" Value="true" />
    </Style.Setters>
</Style>
```

次に示すように、`StaticResource` マークアップ拡張を使用して [`Style`](xref:Xamarin.Forms.NavigableElement.Style) プロパティを `Style` インスタンスに設定することで、[`Style`](xref:Xamarin.Forms.Style) を [`Entry`](xref:Xamarin.Forms.Entry) コントロールに適用できます。

```xaml
<Entry Placeholder="Enter a System.Double" Style="{StaticResource NumericValidationStyle}">
```

スタイルについて詳しくは、[スタイル](~/xamarin-forms/user-interface/styles/index.md)に関する記事をご覧ください。

> [!NOTE]
> XAML で設定されているかクエリを実行するビヘイビアーにバインド可能プロパティを追加できますが、状態があるビヘイビアーを作成する場合は、`ResourceDictionary` の `Style` スタイルに含まれるコントロール間でそれらを共有しないでください。

### <a name="removing-a-behavior-from-a-control"></a>コントロールからのビヘイビアーの削除

[`OnDetachingFrom`](xref:Xamarin.Forms.Behavior`1.OnDetachingFrom(Xamarin.Forms.BindableObject)) method is fired when a behavior is removed from a control, and is used to perform any required cleanup such as unsubscribing from an event to prevent a memory leak. However, behaviors are not implicitly removed from controls unless the control's [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) collection is modified by a `Remove` or `Clear` method. The following code example demonstrates removing a specific behavior from a control's `Behaviors` コレクション:

```csharp
var toRemove = entry.Behaviors.FirstOrDefault (b => b is NumericValidationBehavior);
if (toRemove != null) {
    entry.Behaviors.Remove (toRemove);
}
```

または、次のコード例に示すように、コントロールの [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors) コレクションをクリアすることもできます。

```csharp
entry.Behaviors.Clear();
```

さらに、ナビゲーション スタックからページがポップアップする場合、ビヘイビアーがコントロールから暗黙的に削除されることはないことに注意してください。 代わりに、ページがスコープを外れる前に、明示的に削除する必要があります。

## <a name="summary"></a>まとめ

この記事では、Xamarin.Forms のビヘイビアーを作成して使用する方法を示しました。 Xamarin.Forms のビヘイビアーは、[`Behavior`](xref:Xamarin.Forms.Behavior) または [`Behavior<T>`](xref:Xamarin.Forms.Behavior`1) クラスから派生させて作成されます。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms のビヘイビアー (サンプル)](/samples/xamarin/xamarin-forms-samples/behaviors-numericvalidationbehavior)
- [スタイルを使用して適用される Xamarin.Forms のビヘイビアー (サンプル)](/samples/xamarin/xamarin-forms-samples/behaviors-numericvalidationbehaviorstyle)
- [Behavior](xref:Xamarin.Forms.Behavior)
- [Behavior\<T>](xref:Xamarin.Forms.Behavior`1)