---
title: Xamarin.Forms で urhosharp の使用
description: この記事では、UrhoSharp を使用して、高度な視覚化用の Xamarin.Forms アプリケーションに 3D グラフィックスを追加する方法について説明します。
ms.prod: xamarin
ms.assetid: 0646B98E-CC04-4537-9715-9F82338FD7FF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/11/2016
ms.openlocfilehash: fc316a9e6ab4261eaa956a987b47aeaf546344a2
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2019
ms.locfileid: "67675269"
---
# <a name="using-urhosharp-in-xamarinforms"></a>Xamarin.Forms で urhosharp の使用

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/urho-samples/tree/master/FormsSample)

## <a name="what-is-urhosharp"></a>UrhoSharp とは何ですか。

[UrhoSharp](~/graphics-games/urhosharp/index.md)強力な 3D エンジンは、Xamarin と .NET の開発者向け。 [概要](~/graphics-games/urhosharp/introduction.md)UrhoSharp ライブラリでは、詳細を説明し、[これらの注意事項](~/graphics-games/urhosharp/using.md)シーンとアクションをプログラミングする方法について説明します。

UrhoSharp は、Xamarin.Forms アプリケーションでグラフィックスを表示するために使用できます。
これは、[サンプル](https://github.com/xamarin/urho-samples/tree/master/FormsSample)UrhoSharp でした使用して、対話型の 3D グラフを作成する方法を示します。

![](urhosharp-images/ios-animation.gif "IOS で 3D 対話的なグラフを UrhoSharp")
![](urhosharp-images/android-animation.gif "UrhoSharp Android で 3D の対話的なグラフ。")

## <a name="adding-the-urhosharp-nuget-packages"></a>UrhoSharp の Nuget パッケージを追加します。

UrhoSharp を使用する前に、開発者は、そのソリューションに UrhoSharp の Nuget パッケージを追加する必要があります。 このガイドでは、Xamarin.Forms プロジェクトで、iOS、Android、および .NET Standard ライブラリ プロジェクト。 すべてのコードは、.NET Standard ライブラリ プロジェクトで出力されます。ただし、UrhoSharp の Nuget をも iOS と Android プロジェクトに追加する必要があります。

UrhoSharp.Forms Nuget パッケージには、すべての UrhoSharp のオブジェクトを作成するために必要なオブジェクトが含まれます。 UrhoSharp.Forms の nuget パッケージに含まれる、`UrhoSurface`クラスは、Xamarin.Forms で UrhoSharp をホストするために使用します。
最初を右クリックし、**パッケージ**フォルダーをクリックし、.NET Standard ライブラリ プロジェクトで**パッケージを追加しています.** .検索語句を入力します。 **UrhoSharp.Forms**を選択します**Xamarin.Forms 用 UrhoSharp**、順にクリックします**パッケージの追加**します。

[![](urhosharp-images/add-package-sml.png "[追加] ダイアログのパッケージ")](urhosharp-images/add-package.png#lightbox "追加パッケージ ダイアログ ボックス")

UrhoSharp.Forms NuGet パッケージをプロジェクトに追加されます。

![](urhosharp-images/packages.png "パッケージ フォルダー")

プラットフォーム固有プロジェクト (iOS と Android) などの上記の手順を繰り返します。

## <a name="walkthrough-adding-urhosharp-to-a-xamarinforms-app"></a>チュートリアル: UrhoSharp の Xamarin.Forms アプリへの追加

次の手順では、Xamarin.Forms UrhoSharp のサンプル コードについて説明します。

1. [Xamarin フォームのページを作成する.](#1)
2. [追加、UrhoSurface](#2)
3. [Urho アプリケーションを構築します。](#3)
4. [グラフ クラス、UrhoSurface を追加します。](#4)
5. [UrhoSharp の対話](#5)

注サンプルで使用されるC#6 機能は、Visual Studio の以前のバージョンでコンパイルしないとします。

<a name="1"/>

### <a name="1-create-a-xamarin-forms-page"></a>1.Xamarin フォーム ページを作成します。

次のコードは、Xamarin.Forms のページを示しています。 `UrhoPage` Urho に関連するコードが追加された前に。

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

### <a name="2-add-the-urhosurface"></a>2.追加、UrhoSurface

UrhoSharp をホストすることができます、`ContentPage`などの他の Xamarin.Forms コントロール。
次のコード スニペットを`UrhoSurface`Xamarin.Forms のページに追加します。

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

参照してください、`Charts`このサンプルで使用されている Urho 3D グラフィックの実装のためのクラス。 基本的なコードのアウトラインを示す次のクラスを実装することに注意してください`Urho.Application`とは別に、`Xamarin.Forms.Application`クラスで実装されている**App.cs**します。

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

[UrhoSharp ドキュメント](~/graphics-games/urhosharp/index.md)3D シーンとアクションを作成する方法の詳細が含まれます。

<a name="4"/>

### <a name="4-add-the-charts-class-to-the-urhosurface"></a>4.グラフ クラス、UrhoSurface を追加します。

使用して、 `UrhoSurface.Show<T>` Urho アプリケーション Xamarin.Forms ページを追加するジェネリック メソッド。 次のコード スニペットを作成するために必要な追加のコードを示しています、`Charts`クラス。

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

注:`Show<T>`メソッドは非同期でありで呼び出す必要があります、`await`キーワード。

<a name="5"/>

### <a name="5-interacting-with-urhosharp"></a>5.UrhoSharp の対話

例では、グラフのバーを選択し、変更を許可します。 `Charts`クラスでは、`Bars`と`SelectedBar`この対話を有効にします。

各"bar"が後に追加の選択範囲のイベント ハンドラー、`Charts`クラスが表示されたらを公開されている反復処理して`Bars`コレクション。

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

イベント ハンドラーは、Xamarin.Forms の値を使用して`Slider`特定のバーの高さを調整するコントロール。

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

最後に、2 つを結び付ける`Slider`コントロール UrhoSharp のキャンバスが影響を受けるの値が変更されたとき。 最初のスライダーが 3D グラフ イメージを回転し、2 つ目のスライダーが選択したバーの高さを調整します。

```csharp
rotationSlider = new Slider(0, 500, 250);
rotationSlider.ValueChanged += (s, e) => urhoApp?.Rotate((float)(e.NewValue - e.OldValue));

selectedBarSlider = new Slider(0, 5, 2.5);
selectedBarSlider.ValueChanged += OnValuesSliderValueChanged;
```

アニメーションを[ページのトップ](#what-is-urhosharp)実行されているサンプルを表示します。

## <a name="summary"></a>まとめ

このページは、UrhoSharp を使用して、Xamarin.Forms の 3D のデータの視覚化を追加する方法を示します。 読み取り、 [UrhoSharp ドキュメント](~/graphics-games/urhosharp/index.md)前に示したメソッドを使用して Xamarin.Forms アプリに含めることができる Urho シーンを構築する方法の詳細について。


## <a name="related-links"></a>関連リンク

- [サンプルのグラフ (C# 6)](https://github.com/xamarin/urho-samples/tree/master/FormsSample)
- [UrhoSharp](~/graphics-games/urhosharp/index.md)
