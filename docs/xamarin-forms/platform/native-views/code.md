---
title: C# でのネイティブ ビュー
description: IOS、Android、および UWP からのネイティブ ビューは、c# を使用して作成された Xamarin.Forms ページから直接参照できます。 この記事では、c# を使用して作成された Xamarin.Forms レイアウトにネイティブ ビューを追加する方法と API の使用状況の測定を修正するカスタム ビューのレイアウトを上書きする方法を示します。
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: ebaf5bf2fcb82dd98147819ea9e089dcd427affc
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644453"
---
# <a name="native-views-in-c"></a>C# でのネイティブ ビュー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)

_IOS、Android、および UWP からのネイティブ ビューは、c# を使用して作成された Xamarin.Forms ページから直接参照できます。この記事では、c# を使用して作成された Xamarin.Forms レイアウトにネイティブ ビューを追加する方法と API の使用状況の測定を修正するカスタム ビューのレイアウトを上書きする方法を示します。_

## <a name="overview"></a>概要

により、任意の Xamarin.Forms コントロール`Content`を設定するかを持つ、`Children`コレクションは、プラットフォーム固有のビューを追加できます。 たとえば、iOS`UILabel`に直接追加することができます、 [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content)プロパティ、または、 [ `StackLayout.Children` ](xref:Xamarin.Forms.Layout`1.Children)コレクション。 ただし、この機能を使用する必要ある`#if`Xamarin.Forms 共有プロジェクトのソリューションで定義し、Xamarin.Forms .NET Standard ライブラリのソリューションから使用できません。

次のスクリーン ショットは、プラットフォーム固有のビューを Xamarin.Forms に追加されたことを示す[ `StackLayout` ](xref:Xamarin.Forms.StackLayout):

[![](code-images/screenshots-sml.png "プラットフォーム固有のビューを含む StackLayout")](code-images/screenshots.png#lightbox "プラットフォームに固有のビューを含む StackLayout")

Xamarin.Forms のレイアウトにプラットフォーム固有のビューを追加する機能は、各プラットフォームでの 2 つの拡張メソッドで有効です。

- `Add` – するプラットフォームに固有のビューを追加、 [ `Children` ](xref:Xamarin.Forms.Layout`1.Children)レイアウトのコレクション。
- `ToView` – プラットフォームに固有のビューを受け取り、Xamarin.Forms としてラップ[ `View` ](xref:Xamarin.Forms.View)として設定できる、`Content`コントロールのプロパティ。

Xamarin.Forms 共有プロジェクトでこれらのメソッドを使用するには、適切なプラットフォーム固有の Xamarin.Forms 名前空間をインポートする必要があります。

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **ユニバーサル Windows プラットフォーム (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>各プラットフォームでプラットフォーム固有のビューを追加します。

次のセクションでは、各プラットフォームで Xamarin.Forms のレイアウトにプラットフォーム固有のビューを追加する方法を示します。

### <a name="ios"></a>iOS

次のコード例は、追加する方法を示します、`UILabel`を[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)と[ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

例では、`stackLayout`と`contentView`XAML または c# で作成したインスタンス。

### <a name="android"></a>Android

次のコード例は、追加する方法を示します、`TextView`を[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)と[ `ContentView` ](xref:Xamarin.Forms.ContentView):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

例では、`stackLayout`と`contentView`XAML または c# で作成したインスタンス。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

次のコード例は、追加する方法を示します、`TextBlock`を[ `StackLayout` ](xref:Xamarin.Forms.StackLayout)と[ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

例では、`stackLayout`と`contentView`XAML または c# で作成したインスタンス。

## <a name="overriding-platform-measurements-for-custom-views"></a>カスタム ビューのプラットフォーム測定値をオーバーライドします。

各プラットフォームでのカスタム ビューは、多くの場合のみ正しくデザイン対象のレイアウト シナリオの測定を実装します。 たとえば、カスタム ビューがのみ、デバイスの使用可能な幅の半分を占有するよう設計されています可能性があります。 ただし後、他のユーザーと共有されている、カスタム ビューが必要、デバイスの利用可能な幅を占有するように可能性があります。 そのため、Xamarin.Forms のレイアウトで再利用されるときに、カスタム ビューの測定の実装をオーバーライドする必要があります。 そのため、`Add`と`ToView`拡張メソッドは、Xamarin.Forms のレイアウトに追加されたときに、カスタム ビュー レイアウトをオーバーライドできます測定デリゲートを指定するを許可する上書きを提供します。

次のセクションでは、API の使用状況の測定を修正する、カスタム ビューのレイアウトをオーバーライドする方法を示します。

### <a name="ios"></a>iOS

次のコード例は、`CustomControl`クラスから継承`UILabel`:

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

このビューのインスタンスを追加、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)次のコード例に示すように。

```csharp
var customControl = new CustomControl {
  MinimumFontSize = 14,
  Lines = 0,
  LineBreakMode = UILineBreakMode.WordWrap,
  Text = "This control has incorrect sizing - there's empty space above and below it."
};
stackLayout.Children.Add (customControl);
```

ただし、ため、`CustomControl.SizeThatFits`オーバーライドは常に 150 の高さを返します、次のスクリーン ショットに示すように、上下に空の領域で、ビューが表示されます。

![](code-images/ios-bad-measurement.png "iOS SizeThatFits の不適切な実装でのカスタム コントロール")

この問題を解決が提供するには、`GetDesiredSizeDelegate`実装は、次のコード例で示した。

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

このメソッドによって提供される幅の使用、`CustomControl.SizeThatFits`メソッドは、高さ 70 の場合は 150 の高さが置換されます。 ときに、`CustomControl`インスタンスに追加される、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)、`FixSize`としてメソッドを指定することができます、`GetDesiredSizeDelegate`によって提供される無効な測定値を修正する、`CustomControl`クラス。

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

これは、結果、次のスクリーン ショットに示すように、上と下、空のスペースのないが正しく表示されているカスタム ビュー。

![](code-images/ios-good-measurement.png "iOS GetDesiredSize 上書きでカスタム コントロール")

### <a name="android"></a>Android

次のコード例は、`CustomControl`クラスから継承`TextView`:

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

このビューのインスタンスを追加、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)次のコード例に示すように。

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

ただし、ため、`CustomControl.OnMeasure`オーバーライドが常に要求された幅の半分を返します、ビューが表示されます、デバイスの使用可能な幅の半分だけを占有している次のスクリーン ショットに示すようにします。

![](code-images/android-bad-measurement.png "不適切な OnMeasure 実装で android カスタム コントロール")

この問題を解決が提供するには、`GetDesiredSizeDelegate`実装は、次のコード例で示した。

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

このメソッドによって提供される幅の使用、`CustomControl.OnMeasure`メソッドが乗算して 2 つです。 ときに、`CustomControl`インスタンスに追加される、 [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)、`FixSize`としてメソッドを指定することができます、`GetDesiredSizeDelegate`によって提供される無効な測定値を修正する、`CustomControl`クラス。

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

これにより、カスタム ビューが正しく表示されたら、デバイスの幅を占有している次のスクリーン ショットに示すように。

![](code-images/android-good-measurement.png "カスタム GetDesiredSize デリゲートを使用して android カスタム コントロール")

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

次のコード例は、`CustomControl`クラスから継承`Panel`:

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

このビューのインスタンスを追加、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)次のコード例に示すように。

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

ただし、ため、`CustomControl.ArrangeOverride`オーバーライドが常に要求された幅の半分を返します、次のスクリーン ショットで示すようにそのビューが、デバイスの使用可能な幅の半分に切り取られます。

![](code-images/winrt-bad-measurement.png "不適切な ArrangeOverride 実装で、UWP のカスタム コントロール")

この問題を解決が提供するには、`ArrangeOverrideDelegate`にビューを追加するときに、実装、 [ `StackLayout`](xref:Xamarin.Forms.StackLayout)次のコード例に示すように。

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

このメソッドによって提供される幅の使用、`CustomControl.ArrangeOverride`メソッドが乗算して 2 つです。 これにより、カスタム ビューが正しく表示されたら、デバイスの幅を占有している次のスクリーン ショットに示すように。

![](code-images/winrt-good-measurement.png "ArrangeOverride デリゲートを使用して、UWP のカスタム コントロール")

## <a name="summary"></a>まとめ

この記事では、c# を使用して作成された Xamarin.Forms レイアウトにネイティブ ビューを追加する方法と API の使用状況の測定を修正するカスタム ビューのレイアウトを上書きする方法について説明します。


## <a name="related-links"></a>関連リンク

- [NativeEmbedding (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-nativeembedding)
- [ネイティブ フォーム](~/xamarin-forms/platform/native-forms.md)
