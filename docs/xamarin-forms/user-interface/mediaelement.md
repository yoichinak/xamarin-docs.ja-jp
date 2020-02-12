---
title: Xamarin. フォーム MediaElement
description: この記事では、MediaElement を使用して、Xamarin. フォームアプリケーションでビデオとオーディオを再生する方法について説明します。
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/7/2020
ms.openlocfilehash: 67e4e9eec38f9dd23ebc3efe503d4b2a34ea8b39
ms.sourcegitcommit: 07941cf9704ff88cf4087de5ebdea623ff54edb1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/11/2020
ms.locfileid: "77145719"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin. フォーム MediaElement

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)

`MediaElement` は、ビデオとオーディオの再生用にメディアプレーヤーをレンダリングします。 トランスポートコントロールと呼ばれるオペレーティングシステムの再生コントロールを使用できます。また、これらを非表示にして、独自のコントロールをデザイン要件に合わせて提供することもできます。

この新しいプレビューコントロールを使用する前に、`App.xaml.xs`に適切なフラグを設定して使用することを選択する必要があります。

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

サポートされているプラットフォーム:

- Android
- iOS
- UWP
- WPF
- macOS
- Tizen

## <a name="set-media-source"></a>メディアソースの設定

`MediaElement` プロパティ `Source` は URI またはローカルファイルパスを受け取ることができます。 再生は、メディアを開くとすぐに開始されます。

```xaml
<MediaElement Source="http://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4" />
```

![](mediaelement-images/mediaelement-android.png "MediaElement on Android")

プラットフォームプロジェクトからローカルアセットにソースを設定するには、"ms appx" URI スキームを使用します。

```xaml
<MediaElement Source="ms-appx://XamarinShow_mid.mp4" />
```

データバインディングを使用する場合は、値コンバーターを使用してこの URI スキームを適用することができます。

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (String.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }

    public object ConvertBack(object value, Type targetType, object parameter, CultureInfo culture)
    {
        throw new NotImplementedException();
    }
}
```

```xaml
<MediaElement
    Source="{Binding VideoSource, Converter={StaticResource VideoSourceConverter}}" />
```

## <a name="control-playback"></a>コントロールの再生

既定では、`MediaElement` は、再生/一時停止、ボリュームとミュート、シーク、時刻の表示に、プラットフォームのネイティブ再生コントロールを使用します。

独自のコントロールをオーバーライドして実装するには、まず `ShowsPlaybackControls="False"`を設定してプラットフォームコントロールを無効にします。 次のプロパティ、メソッド、およびイベントを使用して、独自のコントロールを使用して再生ステータスとコントロール再生を反映できるようになりました。

- 再生を制御する `Play()`、`Pause()`、`Stop()` メソッド
- 初期動作を設定する `AutoPlay`
- シークを有効または無効にする `CanSeek`
- time の `Position` と `Duration` のプロパティ
- 音量とミュートを変更するための `Volume` プロパティ
- 再生の完了時に繰り返す `IsLooping` プロパティ
- UI 更新を処理するための `StateRequested`、`PositionRequested`、`VolumeRequested`

### <a name="handle-additional-media-events"></a>追加のメディアイベントを処理する

`MediaOpened`、`MediaEnded`、`MediaFailed`、`CurrentStateChanged` イベントなどの一般的なメディアイベントに応答できます。 常に MediaFailed イベントを処理することをお勧めします。
CurrentStateChanged は、再生の状態に関する情報を提供します。 `MediaElementState` 列挙型の値を使用します。

- **Closed**: メディアが含まれていないため、透明なフレームが表示されます
- **開く**: 指定されたソースの検証中に、読み込みを試行しています
- **バッファリング**: メディアの再生を読み込んでいます
  - その `Position` は、この状態では処理を進めません。
  - 既にビデオを再生している場合は、最後に表示したフレームが引き続き表示されます。
- **再生**中: 現在のメディアソースを再生しています
- **一時停止**: `Position` を事前に進めません
  - ビデオを再生している場合は、現在のフレームが引き続き表示されます。
- **停止**: メディアが含まれていますが、再生または一時停止していません
  - この `Position` は0であり、先行しません。
  - 読み込まれたメディアがビデオの場合、`MediaElement` に最初のフレームが表示されます。

## <a name="control-display"></a>コントロールの表示

[`Image`](xref:Xamarin.Forms.Image)と同様に、`MediaElement` コントロールには `VideoHeight`、`VideoWidth`、および `Aspect`があります。 アスペクトには3つの異なるオプションがあります。

- **Aspectfill**: ビデオは、コントロールの幅と高さ全体を占めるようになります。また、ソースの縦横比を維持したまま、多くの場合、コントロールの範囲外に漏れることがあります。
- **Aspfit**: ビデオは、ソースの縦横比を維持したまま、コントロールの幅と高さの範囲内に収まる
- **Fill**: ビデオがコントロールの幅と高さに合わせて表示されます。

## <a name="additional-properties"></a>追加のプロパティ

- `VideoWidth`: コントロールの幅
- `VideoHeight`: コントロールの高さ
- `KeepScreenOn`: 再生中にデバイスの画面を維持する必要がある場合

## <a name="related-links"></a>関連リンク

- [MediaElement (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/pre-release/WorkingWithMediaElement)
