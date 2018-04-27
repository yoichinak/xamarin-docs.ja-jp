---
title: C# でネイティブのビュー
description: IOS、Android、および UWP からネイティブのビューは、c# を使用して作成された Xamarin.Forms ページから直接参照できます。 この記事では、c# を使用して作成された Xamarin.Forms レイアウトにネイティブのビューを追加する方法と API の使用状況の測定を解決するカスタム ビューのレイアウトをオーバーライドする方法を示します。
ms.prod: xamarin
ms.assetid: 230F937C-F914-4B21-8EA1-1A2A9E644769
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 46284fd1b0863f904e9f24f125aef75fe3eb8caa
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="native-views-in-c"></a>C# でネイティブのビュー

_IOS、Android、および UWP からネイティブのビューは、c# を使用して作成された Xamarin.Forms ページから直接参照できます。この記事では、c# を使用して作成された Xamarin.Forms レイアウトにネイティブのビューを追加する方法と API の使用状況の測定を解決するカスタム ビューのレイアウトをオーバーライドする方法を示します。_

## <a name="overview"></a>概要

任意 Xamarin.Forms コントロールを許可する`Content`を設定するにまたはを持つ、`Children`コレクションは、プラットフォーム固有のビューを追加できます。 たとえば、iOS`UILabel`に直接追加することができます、 [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/)プロパティ、または、 [ `StackLayout.Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/)コレクション。 ただし、この機能を使用する必要ある`#if`Xamarin.Forms 共有プロジェクトのソリューションで定義し、Xamarin.Forms ポータブル クラス ライブラリ (PCL) ソリューションから使用できません。

次のスクリーン ショットは、プラットフォーム固有のビューを Xamarin.Forms に追加されたことを示す[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/):

[![](code-images/screenshots-sml.png "プラットフォーム固有のビューを含む StackLayout")](code-images/screenshots.png#lightbox "StackLayout プラットフォーム固有のビューを含む")

Xamarin.Forms のレイアウトにプラットフォーム固有のビューを追加する機能は、各プラットフォームでの 2 つの拡張メソッドで有効です。

- `Add` – するプラットフォーム固有のビューを追加、 [ `Children` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Layout%3CT%3E.Children/)レイアウトのコレクション。
- `ToView` – プラットフォームに固有のビューを取得し、Xamarin.Forms としてラップ[ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)として設定できる、`Content`コントロールのプロパティです。

Xamarin.Forms 共有プロジェクトでこれらのメソッドを使用するには、適切なプラットフォーム固有の Xamarin.Forms 名前空間をインポートする必要があります。

- **iOS** – Xamarin.Forms.Platform.iOS
- **Android** – Xamarin.Forms.Platform.Android
- **ユニバーサル Windows プラットフォーム (UWP)** – Xamarin.Forms.Platform.UWP

## <a name="adding-platform-specific-views-on-each-platform"></a>各プラットフォームでプラットフォーム固有のビューを追加します。

次のセクションでは、各プラットフォームで Xamarin.Forms レイアウトにプラットフォーム固有のビューを追加する方法を示します。

### <a name="ios"></a>iOS

次のコード例は、追加する方法を示します、`UILabel`を[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)と[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

例では、`stackLayout`と`contentView`インスタンスは、XAML または c# で既に作成しています。

### <a name="android"></a>Android

次のコード例は、追加する方法を示します、`TextView`を[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)と[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
var textView = new TextView (MainActivity.Instance) { Text = originalText, TextSize = 14 };
stackLayout.Children.Add (textView);
contentView.Content = textView.ToView();
```

例では、`stackLayout`と`contentView`インスタンスは、XAML または c# で既に作成しています。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

次のコード例は、追加する方法を示します、`TextBlock`を[ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)と[ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

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

例では、`stackLayout`と`contentView`インスタンスは、XAML または c# で既に作成しています。

## <a name="overriding-platform-measurements-for-custom-views"></a>カスタム ビューのプラットフォーム測定値をオーバーライドします。

各プラットフォームでのカスタム ビューは、多くの場合のみ正しくデザイン対象のレイアウト シナリオの測定を実装します。 たとえば、カスタム ビューがのみ、デバイスの使用可能な幅の半分を占有するように設計されています可能性があります。 ただし、他のユーザーと共有されているの後に、カスタム ビュー必要があります、デバイスの利用可能な幅を占有するようにします。 したがって、Xamarin.Forms レイアウトで再利用されるときに、カスタム ビューの測定の実装をオーバーライドする必要であることができます。 そのため、`Add`と`ToView`拡張メソッドは、Xamarin.Forms のレイアウトに追加されたとき、カスタム ビューのレイアウトをオーバーライドできます測定デリゲートを指定するをできる上書きします。

次のセクションでは、API の使用状況の測定を解決する、カスタム ビューのレイアウトをオーバーライドする方法を示します。

### <a name="ios"></a>iOS

次のコード例は、`CustomControl`から継承されるクラスが`UILabel`:

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

このビューのインスタンスを追加、 [ `StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)次のコード例に示すように、します。

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

![](code-images/ios-bad-measurement.png "iOS SizeThatFits 実装が不適切なカスタム コントロール")

この問題の解決策が提供することです、`GetDesiredSizeDelegate`実装では、次のコード例で示したようにします。

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

このメソッドによって提供される幅の使用、`CustomControl.SizeThatFits`メソッドでは、70 の高さの 150 の高さが置き換えられます。 ときに、`CustomControl`インスタンスに追加される、 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)、`FixSize`としてメソッドを指定することができます、`GetDesiredSizeDelegate`によって提供される不適切な測定値の修正、`CustomControl`クラス。

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

これは、結果、次のスクリーン ショットに示すようにせず、上下に空の領域が正しく表示されるカスタム ビューに。

![](code-images/ios-good-measurement.png "iOS GetDesiredSize 上書きを使ってカスタム コントロール")

### <a name="android"></a>Android

次のコード例は、`CustomControl`から継承されるクラスが`TextView`:

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

このビューのインスタンスを追加、 [ `StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)次のコード例に示すように、します。

```csharp
var customControl = new CustomControl (MainActivity.Instance) {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device.",
  TextSize = 14
};
stackLayout.Children.Add (customControl);
```

ただし、ため、`CustomControl.OnMeasure`オーバーライドが常に要求された幅の半分を返します、ビューに表示されます、デバイスの使用可能な幅の半分だけを使用していた次のスクリーン ショットに示すようにします。

![](code-images/android-bad-measurement.png "不適切な OnMeasure 実装と android のカスタム コントロール")

この問題の解決策が提供することです、`GetDesiredSizeDelegate`実装では、次のコード例で示したようにします。

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

このメソッドによって提供される幅の使用、`CustomControl.OnMeasure`メソッドが乗算して 2 つです。 ときに、`CustomControl`インスタンスに追加される、 [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)、`FixSize`としてメソッドを指定することができます、`GetDesiredSizeDelegate`によって提供される不適切な測定値の修正、`CustomControl`クラス。

```csharp
stackLayout.Children.Add (customControl, FixSize);
```

これは、結果カスタム ビューの中で正しく表示すると、デバイスの幅を使用していた、次のスクリーン ショットに示すように。

![](code-images/android-good-measurement.png "カスタム GetDesiredSize デリゲートを使用して android のカスタム コントロール")

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

次のコード例は、`CustomControl`から継承されるクラスが`Panel`:

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

このビューのインスタンスを追加、 [ `StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)次のコード例に示すように、します。

```csharp
var brokenControl = new CustomControl {
  Text = "This control has incorrect sizing - it doesn't occupy the available width of the device."
};
stackLayout.Children.Add(brokenControl);
```

ただし、ため、`CustomControl.ArrangeOverride`オーバーライドが常に要求された幅の半分を返します、ビューは、次のスクリーン ショットに示すように、デバイスの使用可能な幅の半分にクリップされます。

![](code-images/winrt-bad-measurement.png "不適切な ArrangeOverride 実装と UWP CustomControl")

この問題を解決が提供するには、`ArrangeOverrideDelegate`実装では、ビューを追加するときに、 [ `StackLayout`](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)次のコード例に示すように。

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

このメソッドによって提供される幅の使用、`CustomControl.ArrangeOverride`メソッドが乗算して 2 つです。 これは、結果カスタム ビューの中で正しく表示すると、デバイスの幅を使用していた、次のスクリーン ショットに示すように。

![](code-images/winrt-good-measurement.png "ArrangeOverride デリゲートを使用して UWP CustomControl")

## <a name="summary"></a>まとめ

この記事では、c# を使用して作成された Xamarin.Forms レイアウトにネイティブのビューを追加する方法と API の使用状況の測定を解決するカスタム ビューのレイアウトをオーバーライドする方法について説明します。


## <a name="related-links"></a>関連リンク

- [NativeEmbedding (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/NativeEmbedding/)
- [ネイティブ フォーム](~/xamarin-forms/platform/native-forms.md)
