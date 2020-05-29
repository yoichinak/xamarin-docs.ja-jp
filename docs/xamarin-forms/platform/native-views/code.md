---
title: ''
description: IOS、Android、UWP のネイティブビューは、 Xamarin.Forms C# を使用して作成されたページから直接参照できます。 この記事では、 Xamarin.Forms C# を使用して作成されたレイアウトにネイティブビューを追加する方法と、カスタムビューのレイアウトをオーバーライドして、その測定 API の使用状況を修正する方法について説明します。
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 18cdeccbdff86a6b20aab4b33db259f1f06ee096
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139595"
---
# <a name="native-views-in-c"></a>C のネイティブビュー\#

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)

_IOS、Android、UWP のネイティブビューは、 Xamarin.Forms C# を使用して作成されたページから直接参照できます。この記事では、 Xamarin.Forms C# を使用して作成されたレイアウトにネイティブビューを追加する方法と、カスタムビューのレイアウトをオーバーライドして、その測定 API の使用状況を修正する方法について説明します。_

## <a name="overview"></a>概要

Xamarin.Formsを設定できるコントロール `Content` 、またはコレクションを持つコントロールは `Children` 、プラットフォーム固有のビューを追加できます。 たとえば、iOS は `UILabel` プロパティに直接追加することも、コレクションに追加することもでき [`ContentView.Content`](xref:Xamarin.Forms.ContentView.Content) [`StackLayout.Children`](xref:Xamarin.Forms.Layout`1.Children) ます。 ただし、この機能は共有プロジェクトソリューションでの定義を使用する必要があり、 `#if` Xamarin.Forms .NET Standard ライブラリソリューションからは使用できないことに注意して Xamarin.Forms ください。

次のスクリーンショットは、に追加されたプラットフォーム固有のビューを示してい Xamarin.Forms [`StackLayout`](xref:Xamarin.Forms.StackLayout) ます。

[![](code-images/screenshots-sml.png "StackLayout Containing Platform-Specific Views")](code-images/screenshots.png#lightbox "StackLayout Containing Platform-Specific Views")

プラットフォーム固有のビューをレイアウトに追加する機能 Xamarin.Forms は、各プラットフォームの2つの拡張メソッドによって有効になります。

- `Add`–レイアウトのコレクションにプラットフォーム固有のビューを追加し [`Children`](xref:Xamarin.Forms.Layout`1.Children) ます。
- `ToView`–プラットフォーム固有のビューを取得し、 Xamarin.Forms [`View`](xref:Xamarin.Forms.View) コントロールのプロパティとして設定できるとしてラップし `Content` ます。

共有プロジェクトでこれらのメソッドを使用するには Xamarin.Forms 、プラットフォーム固有の適切な名前空間をインポートする必要があり Xamarin.Forms ます。

- **iOS** – Xamarin.Forms 。Platform. iOS
- **Android** – Xamarin.Forms 。Platform. Android
- **ユニバーサル Windows プラットフォーム (UWP)** – Xamarin.Forms 。プラットフォーム (UWP)

## <a name="adding-platform-specific-views-on-each-platform"></a>プラットフォーム固有のビューを各プラットフォームに追加する

以下のセクションでは、プラットフォーム固有のビューを各プラットフォームのレイアウトに追加する方法について説明し Xamarin.Forms ます。

### <a name="ios"></a>iOS

次のコード例は、をとに追加する方法を示してい `UILabel` [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`ContentView`](xref:Xamarin.Forms.ContentView) ます。

```csharp
var uiLabel = new UILabel {
  MinimumFontSize = 14f,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = originalText,
};
stackLayout.Children.Add (uiLabel);
contentView.Content = uiLabel.ToView();
```

この例では、 `stackLayout` `contentView` インスタンスとインスタンスが XAML または C# で既に作成されていることを前提としています。

### <a name="android"></a>Android

次のコード例は、をとに追加する方法を示してい `TextView` [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`ContentView`](xref:Xamarin.Forms.ContentView) ます。

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

この例では、 `stackLayout` `contentView` インスタンスとインスタンスが XAML または C# で既に作成されていることを前提としています。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

次のコード例は、をとに追加する方法を示してい `TextBlock` [`StackLayout`](xref:Xamarin.Forms.StackLayout) [`ContentView`](xref:Xamarin.Forms.ContentView) ます。

```csharp
var textBlock = new TextBlock
{
    Text = originalText,
    FontSize = 14,
    FontFamily = new FontFamily("HelveticaNeue"),
    TextWrapping = TextWrapping.Wrap
};
stackLayout.Children.Add(textBlock);
contentView.Content = textBlock.ToView();
```

この例では、 `stackLayout` `contentView` インスタンスとインスタンスが XAML または C# で既に作成されていることを前提としています。

## <a name="overriding-platform-measurements-for-custom-views"></a>カスタムビューのプラットフォーム測定値の上書き

多くの場合、各プラットフォームのカスタムビューは、設計されたレイアウトシナリオの測定を正しく実装するだけです。 たとえば、カスタムビューは、デバイスの使用可能な幅の半分だけを占有するように設計されている場合があります。 ただし、他のユーザーと共有した後は、デバイスの使用可能な完全な幅を占有するためにカスタムビューが必要になることがあります。 そのため、レイアウトで再利用するときに、カスタムビューの測定実装をオーバーライドすることが必要になる場合があり Xamarin.Forms ます。 そのため、および拡張メソッドには、測定デリゲートを指定できるオーバーライドが用意されています。これにより、 `Add` `ToView` レイアウトに追加されるときにカスタムビューレイアウトがオーバーライドされる可能性があり Xamarin.Forms ます。

次のセクションでは、カスタムビューのレイアウトをオーバーライドして、その測定 API の使用状況を修正する方法を示します。

### <a name="ios"></a>iOS

次のコード例は、 `CustomControl` から継承するクラスを示してい `UILabel` ます。

```csharp
public class CustomControl : UILabel
{
  public override string Text {
    get { return base.Text; }
    set { base.Text = value.ToUpper (); }
  }

  public override CGSize SizeThatFits (CGSize size)
  {
    return new CGSize (size.Width, 150);
  }
}
```

[`StackLayout`](xref:Xamarin.Forms.StackLayout)次のコード例に示すように、このビューのインスタンスはに追加されます。

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

ただし、 `CustomControl.SizeThatFits` このオーバーライドは常に高さ150を返します。そのため、次のスクリーンショットに示すように、ビューの上と下に空の領域が表示されます。

![](code-images/ios-bad-measurement.png "iOS CustomControl with Bad SizeThatFits Implementation")

この問題を解決するには、 `GetDesiredSizeDelegate` 次のコード例に示すように、実装を提供します。

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, double width, double height)
{
  var uiView = renderer.Control;

  if (uiView == null) {
    return null;
  }

  var constraint = new CGSize (width, height);

  // Let the CustomControl determine its size (which will be wrong)
  var badRect = uiView.SizeThatFits (constraint);

  // Use the width and substitute the height
  return new SizeRequest (new Size (badRect.Width, 70));
}
```

このメソッドは、メソッドによって提供される幅を使用し `CustomControl.SizeThatFits` ますが、高さ70の高さ150に置き換えられます。 インスタンスがに追加されたときに、 `CustomControl` [`StackLayout`](xref:Xamarin.Forms.StackLayout) `FixSize` `GetDesiredSizeDelegate` クラスによって提供される不適切な測定を修正するために、メソッドをとして指定でき `CustomControl` ます。

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

この結果、次のスクリーンショットに示すように、カスタムビューが正しく表示されます。

![](code-images/ios-good-measurement.png "iOS CustomControl with GetDesiredSize Override")

### <a name="android"></a>Android

次のコード例は、 `CustomControl` から継承するクラスを示してい `TextView` ます。

```csharp
public class CustomControl : TextView
{
  public CustomControl (Context context) : base (context)
  {
  }

  protected override void OnMeasure (int widthMeasureSpec, int heightMeasureSpec)
  {
    int width = MeasureSpec.GetSize (widthMeasureSpec);

    // Force the width to half of what's been requested.
    // This is deliberately wrong to demonstrate providing an override to fix it with.
    int widthSpec = MeasureSpec.MakeMeasureSpec (width / 2, MeasureSpec.GetMode (widthMeasureSpec));

    base.OnMeasure (widthSpec, heightMeasureSpec);
  }
}
```

[`StackLayout`](xref:Xamarin.Forms.StackLayout)次のコード例に示すように、このビューのインスタンスはに追加されます。

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

ただし、この `CustomControl.OnMeasure` オーバーライドは常に要求された幅の半分を返すので、次のスクリーンショットに示すように、デバイスの使用可能な幅の半分だけを使用してビューが表示されます。

![](code-images/android-bad-measurement.png "Android CustomControl with Bad OnMeasure Implementation")

この問題を解決するには、 `GetDesiredSizeDelegate` 次のコード例に示すように、実装を提供します。

```csharp
SizeRequest? FixSize (NativeViewWrapperRenderer renderer, int widthConstraint, int heightConstraint)
{
  var nativeView = renderer.Control;

  if ((widthConstraint == 0 && heightConstraint == 0) || nativeView == null) {
    return null;
  }

  int width = Android.Views.View.MeasureSpec.GetSize (widthConstraint);
  int widthSpec = Android.Views.View.MeasureSpec.MakeMeasureSpec (
    width * 2, Android.Views.View.MeasureSpec.GetMode (widthConstraint));
  nativeView.Measure (widthSpec, heightConstraint);
  return new SizeRequest (new Size (nativeView.MeasuredWidth, nativeView.MeasuredHeight));
}
```

このメソッドは、メソッドによって提供される幅を使用し `CustomControl.OnMeasure` ますが、2で乗算します。 インスタンスがに追加されたときに、 `CustomControl` [`StackLayout`](xref:Xamarin.Forms.StackLayout) `FixSize` `GetDesiredSizeDelegate` クラスによって提供される不適切な測定を修正するために、メソッドをとして指定でき `CustomControl` ます。

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

これにより、次のスクリーンショットに示すように、カスタムビューが正しく表示され、デバイスの幅が含まれるようになります。

![](code-images/android-good-measurement.png "Android CustomControl with Custom GetDesiredSize Delegate")

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

次のコード例は、 `CustomControl` から継承するクラスを示してい `Panel` ます。

```csharp
public class CustomControl : Panel
{
  public static readonly DependencyProperty TextProperty =
    DependencyProperty.Register(
      "Text", typeof(string), typeof(CustomControl), new PropertyMetadata(default(string), OnTextPropertyChanged));

  public string Text
  {
    get { return (string)GetValue(TextProperty); }
    set { SetValue(TextProperty, value.ToUpper()); }
  }

  readonly TextBlock textBlock;

  public CustomControl()
  {
    textBlock = new TextBlock
    {
      MinHeight = 0,
      MaxHeight = double.PositiveInfinity,
      MinWidth = 0,
      MaxWidth = double.PositiveInfinity,
      FontSize = 14,
      TextWrapping = TextWrapping.Wrap,
      VerticalAlignment = VerticalAlignment.Center
    };

    Children.Add(textBlock);
  }

  static void OnTextPropertyChanged(DependencyObject dependencyObject, DependencyPropertyChangedEventArgs args)
  {
    ((CustomControl)dependencyObject).textBlock.Text = (string)args.NewValue;
  }

  protected override Size ArrangeOverride(Size finalSize)
  {
      // This is deliberately wrong to demonstrate providing an override to fix it with.
      textBlock.Arrange(new Rect(0, 0, finalSize.Width/2, finalSize.Height));
      return finalSize;
  }

  protected override Size MeasureOverride(Size availableSize)
  {
      textBlock.Measure(availableSize);
      return new Size(textBlock.DesiredSize.Width, textBlock.DesiredSize.Height);
  }
}
```

[`StackLayout`](xref:Xamarin.Forms.StackLayout)次のコード例に示すように、このビューのインスタンスはに追加されます。

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

ただし、この `CustomControl.ArrangeOverride` オーバーライドは常に要求された幅の半分を返すため、次のスクリーンショットに示すように、ビューはデバイスの使用可能な幅の半分にクリップされます。

![](code-images/winrt-bad-measurement.png "UWP CustomControl with Bad ArrangeOverride Implementation")

この問題を解決するには、 `ArrangeOverrideDelegate` [`StackLayout`](xref:Xamarin.Forms.StackLayout) 次のコード例に示すように、ビューをに追加するときに、実装を提供します。

```csharp
stackLayout.Children.Add(fixedControl, arrangeOverrideDelegate: (renderer, finalSize) =>
{
    if (finalSize.Width <= 0 || double.IsInfinity(finalSize.Width))
    {
        return null;
    }
    var frameworkElement = renderer.Control;
    frameworkElement.Arrange(new Rect(0, 0, finalSize.Width * 2, finalSize.Height));
    return finalSize;
});
```

このメソッドは、メソッドによって提供される幅を使用し `CustomControl.ArrangeOverride` ますが、2で乗算します。 これにより、次のスクリーンショットに示すように、カスタムビューが正しく表示され、デバイスの幅が含まれるようになります。

![](code-images/winrt-good-measurement.png "UWP CustomControl with ArrangeOverride Delegate")

## <a name="summary"></a>[概要]

この記事では、 Xamarin.Forms C# を使用して作成されたレイアウトにネイティブビューを追加する方法と、カスタムビューのレイアウトをオーバーライドして、その測定 API の使用を修正する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [すべての埋め込み (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)
- [ネイティブ フォーム](~/xamarin-forms/platform/native-forms.md)
