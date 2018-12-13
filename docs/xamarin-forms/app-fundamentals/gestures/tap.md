---
title: タップ ジェスチャ認識エンジンの追加
description: この記事では、Xamarin.Forms アプリケーションでタップ検出にタップ ジェスチャを使用する方法について説明します。 タップ検出は TapGestureRecognizer クラスで実装されています。
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: a28afb30770f15861aef06643e7f51070199ea9b
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "38994855"
---
# <a name="adding-a-tap-gesture-recognizer"></a>タップ ジェスチャ認識エンジンの追加

_タップ ジェスチャはタップ検出に使用され、TapGestureRecognizer クラスで実装されています。_

ユーザー インターフェース要素をタップ ジェスチャを使用してクリックできるようにするには、[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) インスタンスを作成し、[`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) イベントを処理し、新しいジェスチャ認識エンジンをユーザー インターフェイス要素の [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) コレクションに追加します。 [`Image`](xref:Xamarin.Forms.Image) 要素にアタッチされている `TapGestureRecognizer` のコード例を次に示します。

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

既定で、画像は 1 回のタップに応答します。 ダブルタップ (または必要に応じてより多くのタップ) を待機するように [`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) プロパティを設定します。

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

[`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) を 1 を超える値に設定すると、設定された期間内にタップが発生した場合にのみ、イベント ハンドラーが実行されます (この期間は構成できません)。 2 回目 (またはそれ以降の) タップがその期間内に発生しない場合は、実質的に無視され、'タップのカウント' が再開されます。

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Xaml を使用する

ジェスチャ認識エンジンは、添付プロパティを使用して Xaml のコントロールに追加できます。 画像に [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) を追加する構文を以下に示します (この例では、*ダブル タップ* イベントを定義します)。

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

(このサンプルの) イベント ハンドラーのコードでは、カウンターをインクリメントし、画像をカラーからモノクロに変更します。

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## <a name="using-icommand"></a>ICommand の使用

Model-View-ViewModel (MVVM) パターンを使用するアプリケーションでは、通常、イベント ハンドラーを直接接続するのではなく、`ICommand` を使用します。 [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) で `ICommand` を簡単にサポートするには、次のようにコードにバインドを設定するか、

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

次のように Xaml を使用するします。

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

このビュー モデルの完成したコードについては、サンプルを参照してください。 関連する `Command` 実装の詳細を以下に示します。

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```


## <a name="related-links"></a>関連リンク

- [TapGesture (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
