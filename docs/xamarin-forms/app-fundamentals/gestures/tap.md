---
title: タップ ジェスチャのジェスチャ レコグナイザーを追加します。
description: この記事では、Xamarin.Forms アプリケーションでの tap 検出のタップ ジェスチャを使用する方法について説明します。 Tap の検出は、TapGestureRecognizer クラスで実装されます。
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: bbe4ca7a1080459b8aeb33640be5158b15e97715
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240666"
---
# <a name="adding-a-tap-gesture-gesture-recognizer"></a>タップ ジェスチャのジェスチャ レコグナイザーを追加します。

_タップ ジェスチャでは、タップ検出のために使用され、TapGestureRecognizer クラスで実装されます。_

## <a name="overview"></a>概要

ユーザー インターフェイス要素のタップ ジェスチャのクリック可能な作成、 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)インスタンス、処理、 [ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/)イベントに新しいジェスチャ レコグナイザーを追加し、 [`GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/)ユーザー インターフェイス要素のコレクション。 次のコード例は、`TapGestureRecognizer`にアタッチされている、 [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/)要素。

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

既定では、イメージは、シングル タップに応答します。 設定、 [ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/)ダブルタップ (または必要な場合は複数回のタップ) まで待機するプロパティです。

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

ときに[ `NumberOfTapsRequired` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired/)設定は、1 を超えるイベント ハンドラーのみ実行されます (この期間は構成できません) 時間の設定時間内に 2 回のタップが発生した場合。 その期間内に 2 つ目 (またはそれ以降) のタップが発生しない場合は実質的に無視され、'タップ数' を再起動します。

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Xaml を使用してください。

ジェスチャ レコグナイザーは、アタッチされるプロパティを使用して、Xaml 内のコントロールに追加できます。 追加するための構文、 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)をイメージには、次に示す (ここで定義する、*ダブルタップ*イベント)。

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

(サンプル) のイベント ハンドラーのコードをカウンターをインクリメントして色からを黒にイメージを変更&amp;白です。

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

通常、Mvvm パターンを使用するアプリケーションを使用して`ICommand`直接イベント ハンドラーを配線ではなくです。 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)簡単にサポートできます`ICommand`コードで、バインドを設定するか。

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

このビューのモデルの完全なコードは、このサンプルで確認できます。 関連する`Command`実装の詳細を以下に示します。

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

## <a name="summary"></a>まとめ

タップ ジェスチャがタップ検出に使用され、使用して実装されて、 [ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)クラスです。 タップ数をダブルタップを認識するように指定することができます (またはトリプル タップ以上がタップ) の動作です。


## <a name="related-links"></a>関連リンク

- [TapGesture (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)
- [TapGestureRecognizer](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)
