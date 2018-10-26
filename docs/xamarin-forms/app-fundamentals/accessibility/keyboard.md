---
title: キーボード ナビゲーション
description: 既定のタブ順序を使用するのではなく、TabIndex および IsTapStop プロパティの組み合わせでタブの順序を指定することによって、UI を調整する必要があります。
ms.prod: xamarin
ms.assetid: 8be8f498-558a-4894-a01f-91a0d3ef927e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: f703dff56d2947c35a9bc76e0eb909bfe9023bac
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131556"
---
# <a name="keyboard-navigation-in-xamarinforms"></a>Xamarin.Forms でのキーボード ナビゲーション

一部のユーザーには、適切なキーボード アクセスを提供しないアプリケーションの使用が困難なをことができます。 コントロールのタブ オーダーを指定するキーボード ナビゲーションを有効にし、特定の順序で入力を受信するアプリケーション ページを準備します。

既定では、コントロールのタブ オーダーは、同じ順序で XAML に記載またはプログラムで子コレクションに追加するです。 この順序は、順序をコントロールでナビゲートをキーボードを使用して、多くの場合、この既定の順序は、最適な順序。 ただし、既定の順序は常に、予想される順序と同じ次の XAML コード例に示すように。

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

次のスクリーン ショットは、このコード例については、既定のタブ オーダーを示しています。

![](keyboard-images/default-tab-order.png "既定の行ベースのタブ オーダー")

タブ オーダーをここでは行ベースであり、XAML でコントロールが表示順序を使います。 そのため、自分をたどって、Tab キーを押して[ `Entry` ](xref:Xamarin.Forms.Entry)インスタンス、surname 続けて`Entry`インスタンス。 ただしより直感的な経験では Tab キーを押して自分 surname のペア間を移動するため、列目のタブ ナビゲーションを使用すること。 これは、入力コントロールのタブの順序を指定することで実現できます。

> [!NOTE]
> ユニバーサル Windows プラットフォームで、キーボード ショートカットがタッチまたはマウスを使用してキーボードの代わりに、アプリケーションの表示の UI を使用したユーザーにすばやく移動したり、操作の直感的な方法を提供するを定義することができます。 詳細については、次を参照してください。[設定 VisualElement アクセスキー](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys)します。

## <a name="setting-the-tab-order"></a>タブ オーダーの設定

`VisualElement.TabIndex`プロパティを使用する順序を示す[ `VisualElement` ](xref:Xamarin.Forms.VisualElement)インスタンスは、コントロール間を Tab キーを押して、ユーザーを移動するときにフォーカスを受け取る。 プロパティの既定値は 0 で、いずれかに設定できます`int`値。

ときに、次の規則が適用または設定する既定のタブ オーダーを使用して、`TabIndex`プロパティ。

 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) インスタンス、 `TabIndex` XAML または子のコレクション内の宣言の順序に基づいてタブ オーダーに 0 が追加されます。
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) インスタンス、`TabIndex`タブ オーダーに 0 が追加されますよりも大きいに基づいてその`TabIndex`値。
 - [`VisualElement`](xref:Xamarin.Forms.VisualElement) インスタンスを`TabIndex`より 0 は、タブ オーダーに追加され、前に、値 0 が表示されます。
 - 競合を`TabIndex`宣言の順序で解決されます。

タブ オーダーを定義した後、Tab キーを押してが循環昇順でのコントロール間でフォーカス`TabIndex`順序、最終的なコントロールに達すると、先頭にラップします。

次の XAML の例は、`TabIndex`列目のタブのナビゲーションを有効にする入力コントロールの設定のプロパティ。

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

次のスクリーン ショットは、このコード例については、タブ オーダーを示しています。

![](keyboard-images/correct-tab-order.png "列ベースのタブ オーダー")

タブ オーダーは、ここでは、列ベースです。 そのため、自分の姓をナビゲート Tab キーを押して[ `Entry` ](xref:Xamarin.Forms.Entry)ペア。

## <a name="excluding-controls-from-the-tab-order"></a>タブ オーダーからコントロールを除外します。

コントロールのタブ オーダーを設定するだけでなく、タブ オーダーからコントロールを除外する必要がある場合があります。 設定によってはこれを実現するための 1 つの方法、 [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement)にコントロールのプロパティ`false`、無効になっているコントロールがタブ オーダーから除外されます。

ただしは無効になっている場合でも、タブ オーダーからコントロールを除外するために必要な場合があります。 これを実現する、`VisualElement.IsTapStop`プロパティを示すかどうかを[ `VisualElement` ](xref:Xamarin.Forms.VisualElement)タブ ナビゲーションに含まれています。 既定値は`true`、その値がの場合と`false`場合かにかかわらず、タブ ナビゲーション インフラストラクチャによって、コントロールは無視されます、`TabIndex`設定されています。

## <a name="supported-controls"></a>サポートされているコントロール

`TabIndex`と`IsTapStop`プロパティは、次のコントロールは、1 つまたは複数のプラットフォームでのキーボード入力をそのまま使用でサポートされます。

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
> これらの各コントロールは、すべてのプラットフォームにフォーカスを設定できるタブがありません。

## <a name="related-links"></a>関連リンク

- [ユーザー補助 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Accessibility/)
