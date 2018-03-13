---
title: "Xamarin.Forms で UrhoSharp の使用"
description: "UrhoSharp を使用して、高度な視覚エフェクトのアプリケーションに 3 D グラフィックスを追加すること"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 03/11/2016
ms.openlocfilehash: 9cbc756c5ba61d764404ffabd347232a25dbdc58
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="using-urhosharp-in-xamarinforms"></a>Xamarin.Forms で UrhoSharp の使用

## <a name="what-is-urhosharp"></a>UrhoSharp とは何ですか。

[UrhoSharp](~/graphics-games/urhosharp/index.md)強力な 3D エンジンは、Xamarin と .NET 開発者向けです。 [概要](~/graphics-games/urhosharp/introduction.md)UrhoSharp ライブラリについて説明し、[これらの注意事項](~/graphics-games/urhosharp/using.md)シーンとアクションをプログラミングする方法について説明します。

UrhoSharp は、Xamarin.Forms アプリケーションでグラフィックスを表示するために使用できます。
これは、[サンプル](https://github.com/xamarin/urho-samples/tree/master/FormsSample)UrhoSharp を対話形式の 3D グラフを構築するために使用だった方法を示します。

![](urhosharp-images/ios-animation.gif "IOS で 3D 対話的なグラフを UrhoSharp")
![](urhosharp-images/android-animation.gif "UrhoSharp Android で 3D の対話的なグラフ。")

## <a name="adding-the-urhosharp-nuget-packages"></a>UrhoSharp Nuget パッケージを追加します。

UrhoSharp を使用する前に開発者がそのソリューションに UrhoSharp Nuget パッケージを追加する必要があります。 この前提として、iOS、Android、および PCL Xamarin.Forms プロジェクト プロジェクト。 すべてのコードを書き込む PCL プロジェクトにあります。UrhoSharp Nuget をすぎる iOS および Android のプロジェクトに追加する必要があります。

UrhoSharp.Forms Nuget パッケージには、すべての UrhoSharp オブジェクトを作成するために必要なオブジェクトが含まれています。 UrhoSharp.Forms nuget パッケージに含まれる、 `UrhoSurface` xamarin.forms UrhoSharp のホストに使用されるクラスです。
を開始するには、PCL の上を右クリックして**パッケージ**フォルダーと選択**パッケージを追加しています.**.検索語句を入力**UrhoSharp.Forms**を選択**Xamarin.Forms の UrhoSharp**をクリックし、**パッケージの追加**です。

[![](urhosharp-images/add-package-sml.png "[追加] ダイアログのパッケージ")](urhosharp-images/add-package.png#lightbox "パッケージ ダイアログの追加")

UrhoSharp.Forms NuGet パッケージは、プロジェクトに追加されます。

![](urhosharp-images/packages.png "パッケージ フォルダー")

(IOS および Android) などのプラットフォーム固有のプロジェクトを上記の手順を繰り返します。

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>チュートリアル: UrhoSharp を Xamarin.Forms アプリに追加します。

次の手順では、Xamarin.Forms UrhoSharp サンプルのコードについて説明します。

1. [Xamarin フォーム ページを作成します。](#1)
2. [UrhoSurface を追加します。](#2)
3. [Urho アプリケーションを構築します。](#3)
4. [グラフのクラス、UrhoSurface を追加します。](#4)
5. [UrhoSharp と対話します。](#5)

サンプルは c# 6 の機能を使用し、古いバージョンの Visual Studio ではコンパイルできない可能性がありますに注意してください。

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1.Xamarin フォーム ページを作成します。

次のコードは、Xamarin.Forms ページを示しています。 `UrhoPage` Urho に関連するコードが追加される前に。

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

<a name="2"/>

### <a name="2-add-the-urhosurface"></a>2.UrhoSurface を追加します。

ホストできる UrhoSharp、`ContentPage`などの他の Xamarin.Forms コントロール。
次のコード スニペット、 `UrhoSurface` Xamarin.Forms ページに追加します。

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

<a name="3"/>

### <a name="3-build-a-urho-application"></a>3.Urho アプリケーションを構築します。

参照してください、`Charts`このサンプルで使用される Urho 3 D グラフィックスの実装のクラスです。 基本的なコードのアウトラインを示す次のクラスを実装することに注意してください`Urho.Application`とは別に、`Xamarin.Forms.Application`クラスで実装されている**App.cs**です。

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

[UrhoSharp ドキュメント](~/graphics-games/urhosharp/index.md)3D シーンとアクションを作成する方法の詳細が含まれています。

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4.グラフのクラス、UrhoSurface を追加します。

使用して、 `UrhoSurface.Show<T>` Urho アプリケーション Xamarin.Forms ページを追加するジェネリック メソッドです。 次のコード スニペットを作成するために必要な追加のコードを示しています、`Charts`クラス。

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

注:`Show<T>`メソッドは非同期で呼び出す必要がありますであり、`await`キーワード。

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5.UrhoSharp と対話します。

この例では、グラフ バーを選択し、変更します。 `Charts`クラスが公開、`Bars`と`SelectedBar`この対話を有効にします。

各「バー」の後に追加、選択範囲のイベント ハンドラーには、`Charts`クラスが表示されたら、公開されているに対する繰り返し処理で`Bars`コレクション。

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

イベント ハンドラーは、Xamarin.Forms の値を使用して`Slider`コントロールを指定したバーの高さを調整します。

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

2 つのネットワーク上での最後に、 `Slider` UrhoSharp キャンバスが影響を受けるその値が変更されたときにできるようにを制御します。 最初のスライダーが 3D グラフ イメージを回転し、2 番目のスライダーが選択したバーの高さを調整します。

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

アニメーションを[ページのトップ](#)実行されているサンプルを表示します。

## <a name="summary"></a>まとめ

このページは、UrhoSharp を使用して、Xamarin.Forms を 3D データの視覚エフェクトを追加する方法を示します。 読み取り、 [UrhoSharp ドキュメント](~/graphics-games/urhosharp/index.md)上記の方法を使用してアプリを Xamarin.Forms に含めることができる Urho シーンを構築する方法の詳細。


## <a name="related-links"></a>関連リンク

- [グラフのサンプル (c# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
