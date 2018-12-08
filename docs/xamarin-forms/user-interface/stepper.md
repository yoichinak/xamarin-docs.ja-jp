---
title: Xamarin.Forms ステッパ
description: Xamarin.Forms ステッパ値の範囲から数値を選択できます。 ラベルが付いた 2 つのボタンで構成されますマイナス、プラス記号。 2 つのボタンの操作、選択した値を増分変更します。
ms.prod: xamarin
ms.assetid: 62571B3E-D84B-4F52-9FC7-C105D6733B16
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/17/2018
ms.openlocfilehash: a224d82ed7bb993f51be6cca6ccf09b5331cfac0
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53052207"
---
# <a name="xamarinforms-stepper"></a>Xamarin.Forms ステッパ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos)

_ステッパを使用して、値の範囲から数値を選択するためです。_

Xamarin.Forms [ `Stepper` ](xref:Xamarin.Forms.Stepper)ラベルが付いた 2 つのボタンから成るマイナス、プラス記号。 これらのボタンは、増分を選択するユーザーが操作できる、`double`値の範囲からの値。

[ `Stepper` ](xref:Xamarin.Forms.Stepper)型の 4 つのプロパティを定義します`double`:。

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) 既定値は 1 で、選択した値を変更する量です。
- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 既定値が 0 で、範囲の最小値です。
- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) 既定値は 100 の範囲の最大となります。
- [`Value`](xref:Xamarin.Forms.Stepper.Value) ステッパの値は、の範囲では、`Minimum`と`Maximum`あり、既定値は 0 です。

これらすべてのプロパティに支えは[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクト。 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)プロパティの既定のバインド モードは、 [ `BindingMode.TwoWay` ](xref:Xamarin.Forms.BindingMode.TwoWay)、使用するアプリケーションでバインディング ソースとして適切であることを意味する、 [モデル-ビュー-ビューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャ。

> [!WARNING]
> 内部的には、 [ `Stepper` ](xref:Xamarin.Forms.Stepper)により[ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)がより小さい[ `Maximum`](xref:Xamarin.Forms.Stepper.Maximum)します。 場合`Minimum`または`Maximum`がこれまでに設定できるように`Minimum`はより小さくない`Maximum`例外が発生します。 設定の詳細については、`Minimum`と`Maximum`プロパティを参照してください[予防策](#precautions)セクション。

[ `Stepper` ](xref:Xamarin.Forms.Stepper)型に変換、 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)プロパティ間なるように[ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)と[ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum)までの値。 場合、`Minimum`プロパティがより大きい値に設定、`Value`プロパティ、`Stepper`設定、`Value`プロパティを`Minimum`します。 同様に場合、`Maximum`値に設定されているより小さい`Value`、し`Stepper`設定、`Value`プロパティを`Maximum`します。

[`Stepper`](xref:Xamarin.Forms.Stepper) 定義、 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged)ときに発生するイベント、 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)いずれかのユーザー操作によって、変更、`Stepper`アプリケーションを設定した場合、または、 `Value`プロパティで直接使用します。 A`ValueChanged`イベントがときに発生することはまた、`Value`プロパティは、前の段落で説明したように強制変換されます。

[ `ValueChangedEventArgs` ](xref:Xamarin.Forms.ValueChangedEventArgs)に付属しているオブジェクト、 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged)イベントには 2 つのプロパティが両方の種類の`double`: [ `OldValue` ](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue)と[`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue). 時にイベントが発生して、値の`NewValue`と同じ、 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)のプロパティ、 [ `Stepper` ](xref:Xamarin.Forms.Stepper)オブジェクト。

## <a name="basic-stepper-code-and-markup"></a>基本的なステッパ コードとマークアップ

[ **StepperDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos)サンプルには、機能的に同等ですが、さまざまな方法で実装されている 3 つのページが含まれています。 最初のページを使用してのみC#コード、コード、および 3 番目のイベント ハンドラーと XAML を XAML ファイル内のデータ バインディングを使用して、イベント ハンドラーを回避することは、2 つ目を使用します。

### <a name="creating-a-stepper-in-code"></a>コードで、ステッパの作成

**ステッパの基本的なコード**ページで、 [ **StepperDemos** ](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos)サンプルを作成する方法を示しています、 [ `Stepper` ](xref:Xamarin.Forms.Stepper)と 2 つ[`Label` ](xref:Xamarin.Forms.Label)コード内のオブジェクト。

```csharp
public class BasicStepperCodePage : ContentPage
{
    public BasicStepperCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Stepper stepper = new Stepper
        {
            Maximum = 360,
            Increment = 30,
            HorizontalOptions = LayoutOptions.Center
        };
        stepper.ValueChanged += (sender, e) =>
        {
            rotationLabel.Rotation = stepper.Value;
            displayLabel.Text = string.Format("The Stepper value is {0}", e.NewValue);
        };

        Title = "Basic Stepper Code";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = { rotationLabel, stepper, displayLabel }
        };
    }
}
```

[ `Stepper` ](xref:Xamarin.Forms.Stepper)が初期化されて、 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 360 のプロパティと[ `Increment` ](xref:Xamarin.Forms.Stepper.Increment) 30 のプロパティ。 操作、 `Stepper` 、選択した値を増分間変更[ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)に`Maximum`の値に基づいて、`Increment`プロパティ。 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged)のハンドラー、`Stepper`を使用して、 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)のプロパティ、`stepper`を設定するオブジェクト、 [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)最初の[ `Label` ](xref:Xamarin.Forms.Label)を使用して、`string.Format`メソッドを`NewValue`設定へのイベント引数のプロパティ、 [ `Text` ](xref:Xamarin.Forms.Label.Text)のプロパティ、2 番目`Label`します。 現在の値を取得する 2 つの方法、`Stepper`を交換できます。

次のスクリーン ショットに示す、**ステッパの基本的なコード**ページ。

[![基本的なステッパ コード](stepper-images/basic-stepper-code.png "ステッパの基本的なコード")](stepper-images/basic-stepper-code-large.png#lightbox)

2 番目の[ `Label` ](xref:Xamarin.Forms.Label)まで「(初期化されていない)」のテキストが表示されます、 [ `Stepper` ](xref:Xamarin.Forms.Stepper)操作は、これにより、最初[ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged)イベント起動します。

### <a name="creating-a-stepper-in-xaml"></a>XAML でのステッパの作成

**ステッパの基本的な XAML**ページは機能的に同じ**ステッパの基本的なコード**は実装が XAML でほとんどの場合。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperXAMLPage"
             Title="Basic Stepper XAML">
    <StackLayout Margin="20">
        <Label x:Name="_rotatingLabel"
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center"
                 ValueChanged="OnStepperValueChanged" />
        <Label x:Name="_displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />        
    </StackLayout>
</ContentPage>
```

分離コード ファイルにはハンドラーが含まれています、 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged)イベント。

```csharp
public partial class BasicStepperXAMLPage : ContentPage
{
    public BasicStepperXAMLPage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs e)
    {
        double value = e.NewValue;
        _rotatingLabel.Rotation = value;
        _displayLabel.Text = string.Format("The Stepper value is {0}", value);
    }
}
```

イベント ハンドラーを取得することも、 [ `Stepper` ](xref:Xamarin.Forms.Stepper)を介してイベントを発生させるですが、`sender`引数。 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)プロパティには、現在の値が含まれています。

```csharp
double value = ((Stepper)sender).Value;
```

場合、 [ `Stepper` ](xref:Xamarin.Forms.Stepper)オブジェクトが使用して、XAML ファイルの名前が付与された、`x:Name`属性 (たとえば、「ステッパ」)、イベント ハンドラーがそのオブジェクトを直接参照します。

```csharp
double value = stepper.Value;
```

### <a name="data-binding-the-stepper"></a>データ バインディング ステッパ

**ステッパの基本的なバインディング**ページを排除するほぼ同等のアプリケーションを記述する方法を示しています、 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)イベント ハンドラーを使用して[データ バインディングの](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperBindingsPage"
             Title="Basic Stepper Bindings">
    <StackLayout Margin="20">
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference _stepper}, Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper x:Name="_stepper"
                 Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center" />
        <Label Text="{Binding Source={x:Reference _stepper}, Path=Value, StringFormat='The Stepper value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

[ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation)最初の[ `Label` ](xref:Xamarin.Forms.Label)にバインドされて、 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)のプロパティ、 [ `Stepper` ](xref:Xamarin.Forms.Stepper)現状、 [ `Text` ](xref:Xamarin.Forms.Label.Text)プロパティは、2 つ目の`Label`で、`StringFormat`仕様。 **ステッパの基本的なバインディング**ページ関数を少し異なる方法で 2 つの前のページから: ページが最初に表示される、2 番目の`Label`値を持つテキスト文字列が表示されます。 データ バインディングを使用すると便利です。 データ バインドせずにテキストを表示する、具体的には初期化する必要があるが、`Text`のプロパティ、`Label`またはの起動処理をシミュレートする、 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged)クラス コンス トラクターからイベント ハンドラーを呼び出すことによってイベント.

## <a name="precautions"></a>注意事項

値、 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)プロパティの値より小さくする必要があります常に、 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum)プロパティ。 次のコード スニペットの原因、 [ `Stepper` ](xref:Xamarin.Forms.Stepper)例外が発生します。

```csharp
// Throws an exception!
Stepper stepper = new Stepper
{
    Minimum = 180,
    Maximum = 360
};
```

C#コンパイラ順番では、これら 2 つのプロパティを設定するコードの生成とタイミング、 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 180 に設定されて、既定値より大きい[ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 100 の値。 設定してここで、例外を回避することができます、`Maximum`プロパティ最初。

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

設定[ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 360 に問題には、既定値より大きいため、 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum) 0 の値。 ときに`Minimum`が設定された場合、値がより小さい`Maximum`360 の値。

XAML で同じ問題が存在します。 確実な順序でプロパティを設定[ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum)よりも大きいは常に`Minimum`:

```xaml
<Stepper Maximum="360"
         Minimum="180" ... />
```

設定することができます、 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)と[ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum)値の順序でのみが、負の数値を`Minimum`は常により小さい`Maximum`:

```xaml
<Stepper Minimum="-360"
         Maximum="-180" ... />
```

[ `Value` ](xref:Xamarin.Forms.Stepper.Value)より大きいまたは等しい、プロパティは常に、 [ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)値以下と等しい、 [ `Maximum`](xref:Xamarin.Forms.Stepper.Maximum)します。 場合`Value`設定されている範囲外の値に値が、範囲でなければに強制変換されますが、例外は発生しません。 このコードは、たとえば、*いない*例外を発生させます。

```csharp
Stepper stepper = new Stepper
{
    Value = 180
};
```

代わりに、 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)プロパティに強制変換、 [ `Maximum` ](xref:Xamarin.Forms.Stepper.Maximum) 100 の値。

上記のコード スニペットを次に示します。

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

ときに[ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)を 180 に設定し、 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)も 180 に設定されます。

場合、 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged)時にイベント ハンドラーがアタッチされている、 [ `Value` ](xref:Xamarin.Forms.Stepper.Value)プロパティが 0 の場合の既定値以外に強制変換、`ValueChanged`イベントが発生します。 次の XAML スニペットに示します。

```xaml
<Stepper ValueChanged="OnStepperValueChanged"
         Maximum="360"
         Minimum="180" />
```

ときに[ `Minimum` ](xref:Xamarin.Forms.Stepper.Minimum)を 180 に設定されている[ `Value` ](xref:Xamarin.Forms.Stepper.Value)を 180 に設定されても、 [ `ValueChanged` ](xref:Xamarin.Forms.Stepper.ValueChanged)イベントが発生します。 これは、エラーは、ページの残りの部分が作成されると、ハンドラーは、まだ作成されていない他の要素 ページを参照しよう前に発生する可能性があります。 いくつかのコードを追加したい場合があります、`ValueChanged`をチェックするハンドラー`null`ページ上の他の要素の値。 または、設定することができます、`ValueChanged`後のイベント ハンドラー、 [ `Stepper` ](xref:Xamarin.Forms.Stepper)値が初期化されています。

## <a name="related-links"></a>関連リンク

- [ステッパ デモのサンプル](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/StepperDemos)
- [ステッパ API](xref:Xamarin.Forms.Stepper)
