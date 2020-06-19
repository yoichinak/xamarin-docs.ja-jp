---
title: で UrhoSharp を使用するXamarin.Forms
description: この記事では、高度な視覚化のために、UrhoSharp を使用してアプリケーションに3D グラフィックスを追加する方法について説明 Xamarin.Forms します。
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/11/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: fbf717092da7f77e265803fae87efb5bf0e9876f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84574367"
---
# <a name="using-urhosharp-in-xamarinforms"></a>で UrhoSharp を使用するXamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/urho-samples/tree/master/FormsSample)

## <a name="what-is-urhosharp"></a>UrhoSharp とは

[Urhosharp](~/graphics-games/urhosharp/index.md)は、Xamarin および .net 開発者向けの強力な3d エンジンです。 この[概要](~/graphics-games/urhosharp/introduction.md)では、UrhoSharp ライブラリの詳細について説明します。[これらのノート](~/graphics-games/urhosharp/using.md)では、シーンとアクションをプログラミングする方法について説明しています。

UrhoSharp は、アプリケーションでグラフィックスをレンダリングするために使用でき Xamarin.Forms ます。
この[サンプル](https://github.com/xamarin/urho-samples/tree/master/FormsSample)では、UrhoSharp を使用して対話型3d グラフを構築する方法を示します。

![](urhosharp-images/ios-animation.gif "UrhoSharp 3D Interactive Chart on iOS")
![](urhosharp-images/android-animation.gif "UrhoSharp 3D Interactive Chart on Android")

## <a name="adding-the-urhosharp-nuget-packages"></a>UrhoSharp NuGet パッケージの追加

UrhoSharp を使用する前に、開発者は UrhoSharp NuGet パッケージをソリューションに追加する必要があります。 このガイドでは、 Xamarin.Forms iOS、Android、および .NET Standard ライブラリプロジェクトを含むプロジェクトを想定しています。 すべてのコードは、.NET Standard library プロジェクトに記述されます。ただし、UrhoSharp NuGet も iOS および Android プロジェクトに追加する必要があります。

UrhoSharp 形式の NuGet パッケージには、UrhoSharp オブジェクトを作成するために必要なすべてのオブジェクトが含まれています。 UrhoSharp 形式の NuGet パッケージには、 `UrhoSurface` で UrhoSharp をホストするために使用されるクラスが含まれてい Xamarin.Forms ます。
開始するには、.NET Standard ライブラリプロジェクトの**packages**フォルダーを右クリックし、[**パッケージの追加**] を選択します。検索語句を入力し、**に Urhosharp Xamarin.Forms **を選択して、[パッケージの**追加**] をクリック**します。**

[![](urhosharp-images/add-package-sml.png "Add Packages Dialog")](urhosharp-images/add-package.png#lightbox "Add Packages Dialog")

UrhoSharp 形式の NuGet パッケージがプロジェクトに追加されます。

![](urhosharp-images/packages.png "Packages Folder")

プラットフォーム固有のプロジェクト (iOS や Android など) に対して上記の手順を繰り返します。

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>チュートリアル: アプリへの UrhoSharp の追加 Xamarin.Forms

次の手順では、urhosharp サンプルのコードについて説明し Xamarin.Forms ます。

1. [Xamarin フォームページを作成する](#1-create-a-xamarin-forms-page)
2. [UrhoSurface を追加する](#2-add-the-urhosurface)
3. [Urho アプリケーションを構築する](#3-build-an-urho-application)
4. [UrhoSurface にグラフクラスを追加する](#4-add-the-charts-class-to-the-urhosurface)
5. [UrhoSharp との対話](#5-interacting-with-urhosharp)

このサンプルでは C# 6 の機能が使用されており、以前のバージョンの Visual Studio ではコンパイルできない場合があることに注意してください。

### <a name="1-create-a-xamarin-forms-page"></a>1. Xamarin フォームページを作成する

次のコードは、 Xamarin.Forms `UrhoPage` Urho に関連するコードが追加される前のページを示しています。

```csharp
public class UrhoPage : ContentPage
{
  Slider selectedBarSlider, rotationSlider;

  public UrhoPage()
  {
    // we'll add Urho later

    rotationSlider = new Slider(0, 500, 250);

    selectedBarSlider = new Slider(0, 5, 2.5);

    Title = " UrhoSharp + Xamarin.Forms";
    Content = new StackLayout {
      Padding = new Thickness (12, 12, 12, 40),
      VerticalOptions = LayoutOptions.FillAndExpand,
      Children = {
        rotationSlider,
        new Label { Text = "SELECTED VALUE:" },
        selectedBarSlider,
      }
    };
  }
```

### <a name="2-add-the-urhosurface"></a>2. UrhoSurface を追加する

UrhoSharp は、 `ContentPage` 他のコントロールと同様にホストでき Xamarin.Forms ます。
次のコードスニペットは、 `UrhoSurface` ページに追加されたを示してい Xamarin.Forms ます。

```csharp
using Urho;
using Urho.Forms;
...
public class UrhoPage : ContentPage
{
  UrhoSurface urhoSurface;

  public UrhoPage()
  {
    urhoSurface = new UrhoSurface();
    urhoSurface.VerticalOptions = LayoutOptions.FillAndExpand;
...
    Content = new StackLayout {
    Padding = new Thickness (12, 12, 12, 40),
    VerticalOptions = LayoutOptions.FillAndExpand,
    Children = {
      urhoSurface,  // added
      new Label { Text = "ROTATION:" },
      rotationSlider,
      new Label { Text = "SELECTED VALUE:" },
      selectedBarSlider,
    }
  };
```

### <a name="3-build-an-urho-application"></a>3. Urho アプリケーションをビルドする

`Charts`このサンプルで使用される Urho 3d グラフィックスの実装については、クラスを参照してください。 基本的なコードの概要を次に示します。クラスは、 `Urho.Application` `Xamarin.Forms.Application` **App.cs**で実装されているクラスとは異なるを実装していることに注意してください。

```csharp
using Urho;
using Urho.Actions;
using Urho.Gui;
using Urho.Shapes;

namespace FormsSample
{
    public class Charts : Urho.Application
    {
    public Charts (ApplicationOptions options = null) : base(options) { }
    protected override void Start ()
    {
      ...
    }
    protected override void OnUpdate(float timeStep)
    {
      ...
    }
```

[Urhosharp ドキュメント](~/graphics-games/urhosharp/index.md)には、3d シーンとアクションを構築する方法についての詳細が記載されています。

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4. グラフクラスを UrhoSurface に追加する

`UrhoSurface.Show<T>`Urho アプリケーションをページに追加するには、ジェネリックメソッドを使用し Xamarin.Forms ます。 次のコードスニペットは、クラスを作成するために必要な追加のコードを示してい `Charts` ます。

```csharp
public class UrhoPage : ContentPage
{
  Charts urhoApp;
  ...
  protected override async void OnAppearing ()
  {
    urhoApp = await urhoSurface.Show<Charts> (new ApplicationOptions(assetsFolder: null)
      { Orientation = ApplicationOptions.OrientationType.Portrait });
  }
```

メモ: メソッドは非同期であり、 `Show<T>` キーワードを使用して呼び出す必要があり `await` ます。

### <a name="5-interacting-with-urhosharp"></a>5. UrhoSharp との対話

この例では、グラフバーを選択して変更できます。 `Charts`クラスは、 `Bars` `SelectedBar` この相互作用を可能にするために、およびを公開します。

各 "bar" には、 `Charts` 公開されたコレクションを反復処理することで、クラスのレンダリング後に追加された選択イベントハンドラーがあり `Bars` ます。

```csharp
protected override async void OnAppearing ()
{
  urhoApp = await urhoSurface.Show<Charts>(new ApplicationOptions(assetsFolder: null) { Orientation = ApplicationOptions.OrientationType.Portrait });
  foreach (var bar in urhoApp.Bars)
  {
    bar.Selected += OnBarSelection;
  }
}
```

イベントハンドラーは、コントロールの値を使用して、 Xamarin.Forms `Slider` 指定されたバーの高さを調整します。

```csharp
private void OnBarSelection(Bar bar)
{
  //reset value
  selectedBarSlider.ValueChanged -= OnValuesSliderValueChanged;
  selectedBarSlider.Value = bar.Value;
  selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
}

void OnValuesSliderValueChanged(object sender, ValueChangedEventArgs e)
{  // C# 6
  if (urhoApp?.SelectedBar != null)
  {
    urhoApp.SelectedBar.Value = (float)e.NewValue;
  }
}
```

最後に、2つのコントロールを接続して、 `Slider` 値が変更されたときに UrhoSharp キャンバスが影響を受けるようにします。 最初のスライダーは3D グラフのイメージを回転し、2番目のスライダーは選択したバーの高さを調整します。

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

[ページの上部](#what-is-urhosharp)にあるアニメーションは、実行中のサンプルを示しています。

## <a name="summary"></a>まとめ

このページでは、UrhoSharp を使用して、3D データビジュアライゼーションをに追加する方法を示し Xamarin.Forms ます。 前に示したメソッドを使用してアプリに含めることができる Urho シーンを構築する方法の詳細については、 [Urhosharp のドキュメント](~/graphics-games/urhosharp/index.md)を参照して Xamarin.Forms ください。

## <a name="related-links"></a>関連リンク

- [グラフのサンプル (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
