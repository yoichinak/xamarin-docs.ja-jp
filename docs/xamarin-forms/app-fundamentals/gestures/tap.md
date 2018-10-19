---
title: タップ ジェスチャ認識エンジンを追加します。
description: この記事では、Xamarin.Forms アプリケーションでのタップ検出のタップ ジェスチャを使用する方法について説明します。 Tap の検出は、TapGestureRecognizer クラスで実装されます。
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: a28afb30770f15861aef06643e7f51070199ea9b
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2018
ms.locfileid: "38994855"
---
# <a name="adding-a-tap-gesture-recognizer"></a>タップ ジェスチャ認識エンジンを追加します。

_タップ ジェスチャでは、tap の検出に使用し、TapGestureRecognizer クラスを使用して実装されます。_

ユーザー インターフェイス要素をタップ ジェスチャでクリック可能にするために、作成、 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer)インスタンスを処理、 [ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped)イベントに新しいジェスチャ レコグナイザーを追加し、 [`GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers)ユーザー インターフェイスの要素のコレクション。 次のコード例は、`TapGestureRecognizer`にアタッチされている、 [ `Image` ](xref:Xamarin.Forms.Image)要素。

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

既定では、イメージを 1 つタップに応答します。 設定、 [ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired)ダブルタップします (または複数回のタップに必要な場合) を待機するプロパティ。

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

ときに[ `NumberOfTapsRequired` ](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired)設定は、1 を超えるイベント ハンドラーのみ場合に実行される、タップは、一定の時間 (この期間は構成できません) 内で発生します。 2 番目 (またはそれ以降) のタップは、その期間内は発生しない場合は、それらは実質的に無視され、タップ数を再起動します。

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Xaml を使用してください。

ジェスチャ レコグナイザーは、添付プロパティを使用して、Xaml でコントロールに追加できます。 追加するための構文を[ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer)イメージを以下に示します (ここで定義する、*ダブルタップ*イベント)。

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

(サンプル) のイベント ハンドラーのコードは、カウンターをインクリメントして、色からイメージを黒に変更&amp;白。

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

## <a name="using-icommand"></a>ICommand を使用します。

通常、モデル-ビュー-ビューモデル (MVVM) パターンを使用するアプリケーションを使用して、`ICommand`イベント ハンドラーを直接接続ではなく。 [ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer)簡単にサポートできる`ICommand`をコードでバインディングを設定します。

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

または、Xaml を使用します。

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

このサンプルでは、このビュー モデルの完全なコードを確認できます。 関連する`Command`実装の詳細を以下に示します。

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
