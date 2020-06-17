---
title:"キーボード アクセシビリティ" の説明: "既定のタブ シーケンスを使用するのではなく、TabIndex プロパティと IsTabStop プロパティの組み合わせでタブ シーケンスを指定することにより、UI のアクセシビリティを調整しなければならない場合があります。"
ms.prod: xamarin ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 05/09/2019 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="keyboard-accessibility-in-xamarinforms"></a>Xamarin.Forms でのキーボード アクセシビリティ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)

スクリーン リーダーを使用するユーザーや身体の不自由なユーザーの場合、適切なキーボード アクセスが提供されていないアプリケーションでは使いづらいことがあります。 Xamarin.Forms アプリケーションでは、望ましいタブ オーダーを指定して、ユーザビリティとアクセシビリティを改善することができます。 コントロールのタブ オーダーを指定することで、キーボード ナビゲーションが有効になり、アプリケーション ページが特定の順序で入力を受け取れるようになり、スクリーン リーダーがフォーカス可能な要素をユーザーに対して読み上げられるようになります。

既定では、コントロールのタブ オーダーは、XAML に列記されている順序、またはプログラムで子コレクションに追加された順序と同じです。 この順序はキーボードでコントロールをナビゲーションし、スクリーン リーダーが読み上げる順序であり、多くの場合、この既定の順序が最善の順序です。 ただし、次の XAML コードの例で示すように、既定の順序が望ましい順序と同じではないこともあります。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname" />
</Grid>
```

次のスクリーンショットは、このコード例での既定のタブ オーダーです。

![](keyboard-images/default-tab-order.png "Default Row-based Tab Order")

このタブ オーダーは行ベースであり、XAML でコントロールが列記されている順序です。 したがって、Tab キーを押すと、名の 2 つの [`Entry`](xref:Xamarin.Forms.Entry) インスタンス、姓の 2 つの `Entry` インスタンスの順に移動します。 しかし、列優先のタブ ナビゲーションを使用して、Tab キーを押すと名と姓のペアの順に移動するようにした方が、エクスペリエンスがわかりやすくなります。 これは、入力コントロールのタブ オーダーを指定することで実現できます。

> [!NOTE]
> ユニバーサル Windows プラットフォームでは、キーボード ショートカットを定義して、ユーザーがタッチやマウスではなくキーボードを使用してアプリケーションの表示されている UI 間をすばやく移動してやり取りするわかりやすい方法を提供することができます。 詳しくは、「[VisualElement アクセス キーの設定](~/xamarin-forms/platform/windows/visualelement-access-keys.md)」をご覧ください。

## <a name="setting-the-tab-order"></a>タブ オーダーを設定する

ユーザーが Tab キーを押してコントロール間を移動するときに [`VisualElement`](xref:Xamarin.Forms.VisualElement) インスタンスがフォーカスを受け取る順序を指定するには、`VisualElement.TabIndex` プロパティを使用します。 プロパティの既定値は 0 で、任意の `int` 値に設定できます。

既定のタブ オーダーを使用するとき、または `TabIndex` プロパティを設定するときは、次の規則が適用されます。

- `TabIndex` が 0 に設定されている [`VisualElement`](xref:Xamarin.Forms.VisualElement) インスタンスは、XAML または子コレクションでの宣言の順序に基づいて、タブ オーダーに追加されます。
- `TabIndex` に 0 より大きい値が設定されている [`VisualElement`](xref:Xamarin.Forms.VisualElement) インスタンスは、`TabIndex` の値に基づいてタブ オーダーに追加されます。
- `TabIndex` に 0 より小さい値が設定されている [`VisualElement`](xref:Xamarin.Forms.VisualElement) インスタンスは、値が 0 のすべてのものより前のタブ オーダーに追加されて表示されます。
- `TabIndex` が競合する場合は、宣言の順序によって解決されます。

タブ オーダーが定義された後、Tab キーを押すと、フォーカスは `TabIndex` の昇順でコントロール間を移動し、最後のコントロールに達すると先頭に戻ります。

次の XAML の例では、列優先のナビゲーションになるように入力コントロールに設定された `TabIndex` プロパティを示します。

```xaml
<Grid>
    <Grid.ColumnDefinitions>
        <ColumnDefinition Width="0.5*" />
        <ColumnDefinition Width="0.5*" />
    </Grid.ColumnDefinitions>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
        <RowDefinition Height="Auto" />
    </Grid.RowDefinitions>
    <Label Text="You"
           HorizontalOptions="Center" />
    <Label Grid.Column="1"
           Text="Manager"
           HorizontalOptions="Center" />
    <Entry Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="1" />
    <Entry Grid.Column="1"
           Grid.Row="1"
           Placeholder="Enter forename"
           TabIndex="3" />
    <Entry Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="2" />
    <Entry Grid.Column="1"
           Grid.Row="2"
           Placeholder="Enter surname"
           TabIndex="4" />
</Grid>
```

次のスクリーンショットは、このコード例でのタブ オーダーです。

![](keyboard-images/correct-tab-order.png "Column-based Tab Order")

ここでのタブ オーダーは列ベースです。 したがって、Tab キーを押すと、名と姓の [`Entry`](xref:Xamarin.Forms.Entry) ペアの順に移動します。

> [!IMPORTANT]
> iOS と Android のスクリーン リーダーは画面上のアクセシビリティ対応の要素を読み上げるときに [`VisualElement`](xref:Xamarin.Forms.VisualElement) の `TabIndex` を考慮します。

## <a name="excluding-controls-from-the-tab-order"></a>タブ オーダーからコントロールを除外する

コントロールのタブ オーダーを設定するだけでなく、タブ オーダーからコントロールを除外することが必要な場合があります。 これを実現するための 1 つの方法は、コントロールの [`IsEnabled`](xref:Xamarin.Forms.VisualElement) プロパティを `false` に設定することです。無効になっているコントロールはタブ オーダーから除外されます。

ただし、無効になっていなくても、タブ オーダーからコントロールを除外することが必要になることがあります。 これは、`VisualElement.IsTabStop` プロパティで実現できます。このプロパティは、[`VisualElement`](xref:Xamarin.Forms.VisualElement) がタブ ナビゲーションに含まれるかどうかを示します。 既定値は `true` で、値を `false` にすると、そのコントロールは、`TabIndex` が設定されているかどうかにかかわらず、タブ ナビゲーション インフラストラクチャによって無視されます。

## <a name="supported-controls"></a>サポートされているコントロール

`TabIndex` プロパティと `IsTabStop` プロパティは、1 つ以上のプラットフォームでキーボード入力を受け付ける次のコントロールでサポートされています。

- [`Button`](xref:Xamarin.Forms.Button)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`NavigationPage`](xref:Xamarin.Forms.NavigationPage)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`SearchBar`](xref:Xamarin.Forms.SearchBar)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)
- [`Switch`](xref:Xamarin.Forms.Switch)
- [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)

> [!NOTE]
> これらの各コントロールは、すべてのプラットフォームにおいてタブでフォーカスを設定できるわけではありません。

## <a name="related-links"></a>関連リンク

- [アクセシビリティ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-accessibility)
