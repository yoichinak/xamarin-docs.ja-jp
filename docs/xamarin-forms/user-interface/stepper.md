---
title: Xamarin.Formsステッパ
description: Xamarin.Formsステッパを使用すると、ユーザーは値の範囲から数値を選択できます。 これは、マイナス記号とプラス記号の付いた2つのボタンで構成されています。 2つのボタンを操作すると、選択した値が徐々に変化します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4f071530fb17de44d8ede786ca1b42f5e11f4f7c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84130547"
---
# <a name="xamarinforms-stepper"></a>Xamarin.Formsステッパ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)

_値の範囲から数値を選択するには、ステッパを使用します。_

は、 Xamarin.Forms [`Stepper`](xref:Xamarin.Forms.Stepper) マイナス記号とプラス記号の付いた2つのボタンで構成されています。 これらのボタンは、値の範囲から値を段階的に選択するためにユーザーが操作でき `double` ます。

は、 [`Stepper`](xref:Xamarin.Forms.Stepper) 型の4つのプロパティを定義し `double` ます。

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment)選択した値を変更する量を指定します。既定値は1です。
- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum)範囲の最小値を指定します。既定値は0です。
- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)範囲の最大値を指定します。既定値は100です。
- [`Value`](xref:Xamarin.Forms.Stepper.Value)ステッパの値を指定します。この値はとの間で、 `Minimum` `Maximum` 既定値は0です。

これらのプロパティはすべて、オブジェクトによってバックアップされ [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) ます。 [`Value`](xref:Xamarin.Forms.Stepper.Value)プロパティには、の既定のバインディングモードがあります。これは、 [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) [モデルビュービューモデル (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md)アーキテクチャを使用するアプリケーションでバインディングソースとして適切であることを意味します。

> [!WARNING]
> 内部的には、 [`Stepper`](xref:Xamarin.Forms.Stepper) [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) はよりも小さいことを保証し [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) ます。 またはが `Minimum` `Maximum` より小さい値に設定されている場合は `Minimum` `Maximum` 、例外が発生します。 プロパティとプロパティの設定の詳細につい `Minimum` `Maximum` ては、「[予防策](#precautions)」を参照してください。

は、 [`Stepper`](xref:Xamarin.Forms.Stepper) [`Value`](xref:Xamarin.Forms.Stepper.Value) との間であるように、プロパティを強制し [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) ます。 プロパティ `Minimum` がプロパティよりも大きい値に設定されている場合、 `Value` は `Stepper` `Value` プロパティをに設定し `Minimum` ます。 同様に、 `Maximum` がよりも小さい値に設定されている場合は、 `Value` `Stepper` プロパティをに設定し `Value` `Maximum` ます。

[`Stepper`](xref:Xamarin.Forms.Stepper)の [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) [`Value`](xref:Xamarin.Forms.Stepper.Value) ユーザー操作によって、 `Stepper` またはアプリケーションによってプロパティが直接設定されたときに、が変更されたときに発生するイベントを定義し `Value` ます。 `ValueChanged` `Value` 前の段落で説明したように、プロパティが強制変換された場合にもイベントが発生します。

[`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs)イベントに付随するオブジェクトに [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) は、型と型の2つのプロパティがあり `double` [`OldValue`](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) [`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue) ます。 イベントが発生した時点で、の値 `NewValue` は [`Value`](xref:Xamarin.Forms.Stepper.Value) オブジェクトのプロパティと同じです [`Stepper`](xref:Xamarin.Forms.Stepper) 。

## <a name="basic-stepper-code-and-markup"></a>基本的なステッパコードとマークアップ

[**Stepperdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)サンプルには、機能的には異なる3つのページが含まれていますが、さまざまな方法で実装されています。 最初のページでは C# コードのみを使用し、2番目のページでは XAML をコード内のイベントハンドラーと共に使用します。3番目のページでは、XAML ファイルのデータバインディングを使用してイベントハンドラーを回避できます。

### <a name="creating-a-stepper-in-code"></a>コードでのステッパの作成

[**Stepperdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)サンプルの「**基本的なステッパコード**」ページでは、 [`Stepper`](xref:Xamarin.Forms.Stepper) コード内にとの2つのオブジェクトを作成する方法を示してい [`Label`](xref:Xamarin.Forms.Label) ます。

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

は、 [`Stepper`](xref:Xamarin.Forms.Stepper) [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) プロパティが360、プロパティが30であるように初期化され [`Increment`](xref:Xamarin.Forms.Stepper.Increment) ます。 を操作 `Stepper` すると、 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) `Maximum` プロパティの値に基づいて、選択した値がに変更され `Increment` ます。 [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged)のハンドラーは、 `Stepper` [`Value`](xref:Xamarin.Forms.Stepper.Value) オブジェクトのプロパティを使用し `stepper` [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) て最初ののプロパティを設定 [`Label`](xref:Xamarin.Forms.Label) し、メソッドを `string.Format` イベント引数のプロパティと共に使用して `NewValue` [`Text`](xref:Xamarin.Forms.Label.Text) 、2番目のプロパティを設定し `Label` ます。 の現在の値を取得するための2つの方法 `Stepper` は、交換可能です。

次のスクリーンショットは、**基本的なステッパコード**ページを示しています。

[![基本的なステッパコード](stepper-images/basic-stepper-code.png "基本的なステッパコード")](stepper-images/basic-stepper-code-large.png#lightbox)

2番目のは [`Label`](xref:Xamarin.Forms.Label) 、が操作されるまで "(初期化されていません)" というテキストを表示し [`Stepper`](xref:Xamarin.Forms.Stepper) ます。これにより、最初の [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) イベントが発生します。

### <a name="creating-a-stepper-in-xaml"></a>XAML でのステッパの作成

**基本的なステッパ xaml**ページは、**基本的なステッパコード**と機能的には同じですが、ほとんどの xaml で実装されています。

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

分離コードファイルには、イベントのハンドラーが含まれてい [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) ます。

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

また、イベントハンドラーは、引数を使用してイベントを発生させるを取得することもでき [`Stepper`](xref:Xamarin.Forms.Stepper) `sender` ます。 プロパティには [`Value`](xref:Xamarin.Forms.Stepper.Value) 、現在の値が含まれます。

```csharp
double value = ((Stepper)sender).Value;
```

オブジェクトに [`Stepper`](xref:Xamarin.Forms.Stepper) 属性を持つ XAML ファイル内の名前が指定されている場合 `x:Name` (たとえば、"ステッパ")、イベントハンドラーはそのオブジェクトを直接参照できます。

```csharp
double value = stepper.Value;
```

### <a name="data-binding-the-stepper"></a>ステッパのデータバインディング

[**基本的なステッパのバインド**] ページには、 [`Value`](xref:Xamarin.Forms.Stepper.Value) [データバインディング](~/xamarin-forms/app-fundamentals/data-binding/index.md)を使用してイベントハンドラーを削除するほぼ同等のアプリケーションを作成する方法が示されています。

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

最初ののプロパティはのプロパティ [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) [`Label`](xref:Xamarin.Forms.Label) にバインドされ [`Value`](xref:Xamarin.Forms.Stepper.Value) [`Stepper`](xref:Xamarin.Forms.Stepper) ます。これは、 [`Text`](xref:Xamarin.Forms.Label.Text) 2 番目のプロパティが `Label` 仕様であるためです `StringFormat` 。 **基本ステッパバインド**ページは、前の2つのページとは少し異なる方法で機能します。最初にページが表示されたときに、2番目のページに `Label` 値を含むテキスト文字列が表示されます。 これは、データバインディングを使用する利点です。 データバインディングなしでテキストを表示するには、のプロパティを明示的に初期化する `Text` `Label` か、 [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) クラスコンストラクターからイベントハンドラーを呼び出すことによってイベントの発生をシミュレートする必要があります。

## <a name="precautions"></a>予防

プロパティの値は、 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 常にプロパティの値よりも小さくする必要があり [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) ます。 次のコードスニペットでは、によって [`Stepper`](xref:Xamarin.Forms.Stepper) 例外が発生します。

```csharp
// Throws an exception!
Stepper stepper = new Stepper
{
    Minimum = 180,
    Maximum = 360
};
```

C# コンパイラは、これらの2つのプロパティを順番に設定するコードを生成し [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) ます。また、プロパティが180に設定されている場合は、既定値の100よりも大きくなり [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) ます。 この場合、この例外を回避するには、最初にプロパティを設定し `Maximum` ます。

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

[`Maximum`](xref:Xamarin.Forms.Stepper.Maximum)を360に設定することは、既定値の0よりも大きいため、問題にはなりません [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 。 `Minimum`が設定されている場合、値は `Maximum` 360 の値よりも小さくなります。

XAML にも同じ問題があります。 が常により大きいことを保証する順序でプロパティを設定し [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) `Minimum` ます。

```xaml
<Stepper Maximum="360"
         Minimum="180" ... />
```

との値は、負の数値に設定できますが、 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) `Minimum` が常により小さい順序でのみ使用でき `Maximum` ます。

```xaml
<Stepper Minimum="-360"
         Maximum="-180" ... />
```

プロパティは、 [`Value`](xref:Xamarin.Forms.Stepper.Value) 常に値以上で、以下の値以上 [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) です。 `Value`がその範囲外の値に設定されている場合、値はその範囲内に強制的に変換されますが、例外は発生しません。 たとえば、次のコードでは例外が発生し*ません*。

```csharp
Stepper stepper = new Stepper
{
    Value = 180
};
```

代わりに、 [`Value`](xref:Xamarin.Forms.Stepper.Value) プロパティは100の値に強制変換され [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) ます。

上記のコードスニペットを次に示します。

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

を [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 180 に設定すると、 [`Value`](xref:Xamarin.Forms.Stepper.Value) も180に設定されます。

[`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) [`Value`](xref:Xamarin.Forms.Stepper.Value) プロパティが既定値の0以外に強制的に変換されるときにイベントハンドラーがアタッチされている場合は、 `ValueChanged` イベントが発生します。 XAML のスニペットを次に示します。

```xaml
<Stepper ValueChanged="OnStepperValueChanged"
         Maximum="360"
         Minimum="180" />
```

を [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) 180 に設定すると、 [`Value`](xref:Xamarin.Forms.Stepper.Value) も180に設定され、 [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) イベントが発生します。 これは、ページの残りの部分が構築される前に発生し、ハンドラーは、まだ作成されていないページ上の他の要素を参照しようとする場合があります。 `ValueChanged` `null` ページ上の他の要素の値を確認するコードをハンドラーに追加することができます。 または、 `ValueChanged` [`Stepper`](xref:Xamarin.Forms.Stepper) 値が初期化された後にイベントハンドラーを設定できます。

## <a name="related-links"></a>関連リンク

- [ステッパデモのサンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stepperdemos)
- [ステッパ API](xref:Xamarin.Forms.Stepper)
