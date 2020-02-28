---
title: Xamarin. フォーム MediaElement
description: この記事では、MediaElement を使用して、Xamarin. フォームアプリケーションでビデオとオーディオを再生する方法について説明します。
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
ms.openlocfilehash: 601a2884cb9ca90ab6681e48afda4c9f1f467932
ms.sourcegitcommit: 5d22f37dfc358678df52a4d17c57261056a72cb7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2020
ms.locfileid: "77674547"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin. フォーム MediaElement

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/MediaElementDemos)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、ビデオやオーディオを再生するためのビューです。 基になるプラットフォームでサポートされているメディアは、次のソースから再生できます。

- URI (HTTP または HTTPS) を使用した web。
- `ms-appx:///` URI スキームを使用して、プラットフォームアプリケーションに埋め込まれたリソース。
- `ms-appdata:///` URI スキームを使用して、アプリのローカルフォルダーと一時データフォルダーから取得されたファイル。
- デバイスのライブラリ。

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、トランスポートコントロールと呼ばれるプラットフォーム再生コントロールを使用できます。 ただし、これらは既定で無効になっており、独自のトランスポートコントロールに置き換えることができます。 次のスクリーンショットは、プラットフォームのトランスポートコントロールでビデオを再生 `MediaElement` を示しています。

[![IOS と Android でビデオを再生している MediaElement のスクリーンショット](mediaelement-images/playback-controls.png "ビデオを再生する MediaElement")](mediaelement-images/playback-controls-large.png#lightbox "ビデオを再生する MediaElement")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、Xamarin. Forms 4.5 で使用できます。 ただし、現在は実験的であり、 *App.xaml.cs*ファイルに次のコード行を追加することによってのみ使用できます。

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)は、IOS、Android、ユニバーサル WINDOWS プラットフォーム (UWP)、およびその他のプラットフォームで利用できます。

[`MediaElement`](xref:Xamarin.Forms.MediaElement) は次の特性を定義します。

- 種類が[`Aspect`](xref:Xamarin.Forms.Aspect)の[`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)は、表示領域に合わせてメディアがどのように拡大縮小されるかを決定します。 このプロパティの既定値は `AspectFit` です。
- 種類が `bool`の[`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)は、 [`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティが設定されているときにメディアの再生を自動的に開始するかどうかを示します。 このプロパティの既定値は `true` です。
- `double`型の[`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)は、現在のバッファリングの進行状況を示します。 このプロパティの既定値は0.0 です。
- `bool`型の[`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)は、 [`Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティの値を設定してメディアを再配置できるかどうかを示します。 これは読み取り専用プロパティです。
- [`MediaElementState`](xref:Xamarin.Forms.MediaElementState)型の[`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)は、コントロールの現在の状態を示します。 これは読み取り専用プロパティで、既定値は `MediaElementState.Closed`です。
- `TimeSpan?`型の[`Duration`](xref:Xamarin.Forms.MediaElement.Duration)は、現在開いているメディアの期間を示します。 これは読み取り専用プロパティで、既定値は `null`です。
- `bool`型の[`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)は、現在読み込まれているメディアソースが最後に到達した後に、最初から再生を再開するかどうかを示します。 このプロパティの既定値は `false` です。
- 種類が `bool`の[`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn)は、メディアの再生中にデバイスの画面を維持するかどうかを決定します。 このプロパティの既定値は `false` です。
- 種類が `TimeSpan`の[`Position`](xref:Xamarin.Forms.MediaElement.Position)は、メディアの再生時間における現在の進行状況を示します。 このプロパティの既定値は `TimeSpan.Zero` です。
- `bool`型の[`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)は、プラットフォームの再生コントロールを表示するかどうかを決定します。 このプロパティの既定値は `false` です。
- [`MediaSource`](xref:Xamarin.Forms.MediaSource)型の[`Source`](xref:Xamarin.Forms.MediaElement.Source)は、コントロールに読み込まれるメディアのソースを示します。
- `int`型の[`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight)は、コントロールの高さを示します。 これは読み取り専用プロパティです。
- `int`型の[`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)は、コントロールの幅を示します。 これは読み取り専用プロパティです。
- `double`型の[`Volume`](xref:Xamarin.Forms.MediaElement.Volume)は、メディアのボリュームを決定します。これは、0 ~ 1 の線形スケールで表されます。 このプロパティは `TwoWay` バインドを使用し、既定値は1です。

これらのプロパティは、`CanSeek` プロパティを除き、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えられています。つまり、データバインディングのターゲットとスタイルを設定できます。

[`MediaElement`](xref:Xamarin.Forms.MediaElement)クラスは、次の4つのイベントも定義します。

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)は、メディアストリームが検証され、開いたときに発生します。
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)は、`MediaElement` がメディアの再生を終了したときに発生します。
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)は、メディアソースに関連付けられたエラーが発生した場合に発生します。
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)は、要求されたシーク操作のシークポイントが再生できる状態になったときに発生します。

さらに、 [`MediaElement`](xref:Xamarin.Forms.MediaElement)には、 [`Play`](xref:Xamarin.Forms.MediaElement.Play)、 [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)、および[`Stop`](xref:Xamarin.Forms.MediaElement.Stop)のメソッドが含まれています。

Android でサポートされているメディア形式の詳細については、「developer.android.com で[サポートされているメディア形式](https://developer.android.com/guide/topics/media/media-formats)」を参照してください。 ユニバーサル Windows プラットフォーム (UWP) でサポートされているメディア形式の詳細については、「[サポートされているコーデック](/windows/uwp/audio-video-camera/supported-codecs)」を参照してください。

## <a name="play-remote-media"></a>リモートメディアを再生する

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、HTTP および HTTPS URI スキームを使用してリモートメディアファイルを再生できます。 これを行うには、 [`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティをメディアファイルの URI に設定します。

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

既定では、 [`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティによって定義されるメディアは、メディアが開かれた直後に再生されます。 自動メディア再生を抑制するには、 [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)プロパティを `false`に設定します。

メディア再生コントロールは、既定で無効になっています。また、[ [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) ] プロパティを [`true`] に設定すると有効になります。 [`MediaElement`](xref:Xamarin.Forms.MediaElement)は、プラットフォームの再生コントロールを使用します。

## <a name="play-local-media"></a>ローカルメディアを再生する

ローカルメディアは、次のソースから再生できます。

- `ms-appx:///` URI スキームを使用して、プラットフォームアプリケーションに埋め込まれたリソース。
- `ms-appdata:///` URI スキームを使用して、アプリのローカルフォルダーと一時データフォルダーから取得されたファイル。
- デバイスのライブラリ。

これらの URI スキームの詳細については、「 [uri スキーム](/windows/uwp/app-resources/uri-schemes)」を参照してください。

### <a name="play-media-embedded-in-the-app-package"></a>アプリパッケージに埋め込まれたメディアを再生する

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、`ms-appx:///` URI スキームを使用して、アプリパッケージに埋め込まれているメディアファイルを再生できます。 メディアファイルは、プラットフォームプロジェクトに配置することによって、アプリパッケージに埋め込まれます。

プラットフォームプロジェクトにメディアファイルを格納することは、プラットフォームによって異なります。

- IOS では、メディアファイルは**resources フォルダーに**保存するか、 **resources**フォルダーのサブフォルダーに格納する必要があります。 メディアファイルには、`BundleResource`の `Build Action` が必要です。
- Android では、メディアファイルは**raw**という名前の**リソース**のサブフォルダーに格納されている必要があります。 **raw** フォルダーにサブフォルダーを含めることはできません。 メディアファイルには、`AndroidResource`の `Build Action` が必要です。
- UWP では、メディアファイルはプロジェクト内の任意のフォルダーに格納できます。 メディアファイルには、`Content`の `BuildAction` が必要です。

これらの条件を満たすメディアファイルは、`ms-appx:///` URI スキームを使用して再生できます。

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

データバインディングを使用する場合は、値コンバーターを使用して、この URI スキームを適用できます。

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        if (Device.RuntimePlatform == Device.UWP)
            return new Uri($"ms-appx:///Assets/{value}");
        else
            return new Uri($"ms-appx:///{value}");
    }
    // ...
}
```

次に、`VideoSourceConverter` のインスタンスを使用して、`ms-appx:///` の URI スキームを埋め込みメディアファイルに適用できます。

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Ms appx URI スキームの詳細については、「 [ms appx と ms appx-web](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web)」を参照してください。

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>アプリのローカルフォルダーと一時フォルダーからメディアを再生する

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、`ms-appdata:///` URI スキームを使用して、アプリのローカルまたは一時データフォルダーにコピーされたメディアファイルを再生できます。

次の例では、アプリのローカルデータフォルダーに格納されているメディアファイルに設定された[`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティを示します。

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

次の例では、アプリの一時データフォルダーに格納されているメディアファイルの[`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティを示します。

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> UWP は、アプリのローカルまたは一時データフォルダーに格納されているメディアファイルを再生するだけでなく、アプリのローミングフォルダーにあるメディアファイルを再生することもできます。 これは、メディアファイルに `ms-appdata:///roaming/`でプレフィックスを付けることで実現できます。

データバインディングを使用する場合は、値コンバーターを使用して、この URI スキームを適用できます。

```csharp
public class VideoSourceConverter : IValueConverter
{
    public object Convert(object value, Type targetType, object parameter, CultureInfo culture)
    {
        if (value == null)
            return null;

        if (string.IsNullOrWhiteSpace(value.ToString()))
            return null;

        return new Uri($"ms-appdata:///{value}");
    }
    // ...
}
```

その後、`VideoSourceConverter` のインスタンスを使用して、アプリのローカルまたは一時データフォルダー内のメディアファイルに `ms-appdata:///` URI スキームを適用できます。

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Ms appdata URI スキームの詳細については、「 [ms appdata](/windows/uwp/app-resources/uri-schemes#ms-appdata)」を参照してください。

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>アプリのローカルまたは一時データフォルダーへのメディアファイルのコピー

アプリのローカルまたは一時データフォルダーに格納されているメディアファイルを再生するには、アプリによってメディアファイルがコピーされている必要があります。 これは、たとえば、アプリパッケージからメディアファイルをコピーすることによって実現できます。

```csharp
// This method copies the video from the app package to the app data
// directory for your app. To copy the video to the temp directory
// for your app, comment out the first line of code, and uncomment
// the second line of code.
public static async Task CopyVideoIfNotExists(string filename)
{
    string folder = FileSystem.AppDataDirectory;
    //string folder = Path.GetTempPath();
    string videoFile = Path.Combine(folder, "XamarinVideo.mp4");

    if (!File.Exists(videoFile))
    {
        using (Stream inputStream = await FileSystem.OpenAppPackageFileAsync(filename))
        {
            using (FileStream outputStream = File.Create(videoFile))
            {
                await inputStream.CopyToAsync(outputStream);
            }
        }
    }
}
```

> [!NOTE]
> 上記のコード例では、Xamarin. Essentials に含まれている `FileSystem` クラスを使用しています。 詳細については、「 [Xamarin. Essentials: ファイルシステムヘルパー](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android)」を参照してください。

### <a name="play-media-from-the-device-library"></a>デバイスライブラリからメディアを再生する

最新のモバイルデバイスやデスクトップコンピューターのほとんどは、デバイスのカメラとマイクを使用してビデオやオーディオを録画する機能を備えています。 作成されたメディアは、デバイスにファイルとして保存されます。 これらのファイルは、ライブラリから取得し、 [`MediaElement`](xref:Xamarin.Forms.MediaElement)で再生できます。

各プラットフォームには、ユーザーがデバイスのライブラリからメディアを選択できる機能が含まれています。 Xamarin. Forms では、プラットフォームプロジェクトはこの機能を呼び出すことができ、 [`DependencyService`](xref:Xamarin.Forms.DependencyService)クラスによって呼び出すことができます。

サンプルアプリケーションで使用されるビデオの選択の依存関係サービスは、[画像ライブラリからの写真の選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)によく似ています。ただし、ピッカーが `Stream` オブジェクトではなくファイル名を返す点が異なります。 共有コードプロジェクトは、`GetVideoFileAsync`という名前の1つのメソッドを定義する `IVideoPicker`という名前のインターフェイスを定義します。 各プラットフォームは、このインターフェイスを `VideoPicker` クラスに実装します。

次のコード例は、デバイスライブラリからメディアファイルを取得する方法を示しています。

```csharp
string filename = await DependencyService.Get<IVideoPicker>().GetVideoFileAsync();
if (!string.IsNullOrWhiteSpace(filename))
{
    mediaElement.Source = new FileMediaSource
    {
        File = filename
    };
}
```

ビデオの選択依存サービスは、プラットフォームプロジェクトで `IVideoPicker` インターフェイスの実装を取得するために `DependencyService.Get` メソッドを呼び出すことによって呼び出されます。 そのインスタンスで `GetVideoFileAsync` メソッドが呼び出され、返されたファイル名を使用して[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)オブジェクトを作成し、 [`MediaElement`](xref:Xamarin.Forms.MediaElement)の[`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティに設定します。

## <a name="change-video-aspect-ratio"></a>ビデオの縦横比の変更

[`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)プロパティは、表示領域に合わせてビデオメディアをどのように拡大縮小するかを決定します。 既定では、このプロパティは `AspectFit` 列挙型のメンバーに設定されますが、 [`Aspect`](xref:Xamarin.Forms.Aspect)列挙型のメンバーのいずれかに設定できます。

- `AspectFit` は、縦横比を維持したまま、必要に応じて、表示領域に合わせるためにビデオが letterboxed されることを示します。
- `AspectFill` は、縦横比を維持したまま、表示領域を塗りつぶすためにビデオがクリップされることを示します。
- `Fill` は、ビデオを拡大して表示領域を塗りつぶすことを示します。

## <a name="poll-for-position-data"></a>位置データのポーリング

[`Position`](xref:Xamarin.Forms.MediaElement.Position)バインド可能なプロパティのプロパティ変更通知は、再生の開始と終了、一時停止の発生など、キーの瞬間にのみ発生します。 したがって、`Position` プロパティへのデータバインディングでは、正確な位置データは生成されません。 代わりに、タイマーを設定し、プロパティをポーリングする必要があります。

これを行うには、メディアの再生時に位置データを必要とするページの `OnAppearing` オーバーライドを使用することをお勧めします。

```csharp
bool polling = true;

protected override void OnAppearing()
{
    base.OnAppearing();

    Device.StartTimer(TimeSpan.FromMilliseconds(1000), () =>
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            positionLabel.Text = mediaElement.Position.ToString("hh\\:mm\\:ss");
        });
        return polling;
    });
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    polling = false;
}
```

この例では、`OnAppearing` オーバーライドは `Position` 値を1秒ごとに `positionLabel` を更新するタイマーを開始します。 タイマーコールバックは、コールバックが `false`を返すまで、1秒ごとに呼び出されます。 ページナビゲーションが発生すると `OnDisappearing` オーバーライドが実行され、呼び出されるタイマーコールバックが停止します。

## <a name="understand-mediasource-types"></a>MediaSource の種類について

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、 [`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティをリモートまたはローカルのメディアファイルに設定することにより、メディアを再生できます。 `Source` プロパティは[`MediaSource`](xref:Xamarin.Forms.MediaSource)型で、このクラスは2つの静的メソッドを定義します。

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)は、`string` 引数から[`MediaSource`](xref:Xamarin.Forms.MediaSource)インスタンスを返します。
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)は、`Uri` 引数から[`MediaSource`](xref:Xamarin.Forms.MediaSource)インスタンスを返します。

さらに、 [`MediaSource`](xref:Xamarin.Forms.MediaSource)クラスには、`string` および `Uri` 引数から `MediaSource` インスタンスを返す暗黙の演算子もあります。

> [!NOTE]
> XAML で[`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティが設定されている場合、`string` または `Uri`から[`MediaSource`](xref:Xamarin.Forms.MediaSource)インスタンスを返すために、型コンバーターが呼び出されます。

[`MediaSource`](xref:Xamarin.Forms.MediaSource)クラスには、次の2つの派生クラスもあります。

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource)。 URI からリモートメディアファイルを指定するために使用されます。 このクラスには、`Uri`に設定できる[`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri)プロパティがあります。
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)。 `string`からローカルメディアファイルを指定するために使用されます。 このクラスには、`string`に設定できる[`File`](xref:Xamarin.Forms.FileMediaSource.File)プロパティがあります。 さらに、このクラスには、`string` を `FileMediaSource` オブジェクトに、`FileMediaSource` オブジェクトを `string`に変換するための暗黙的な演算子があります。

> [!NOTE]
> [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)オブジェクトが XAML で作成されると、型コンバーターが呼び出され、`string`から[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)インスタンスが返されます。

## <a name="determine-mediaelement-status"></a>MediaElement ステータスの決定

[`MediaElement`](xref:Xamarin.Forms.MediaElement)クラスは、 [`MediaElementState`](xref:Xamarin.Forms.MediaElementState)型の[`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)という名前の読み取り専用のバインド可能なプロパティを定義します。 このプロパティは、メディアを再生するか一時停止するか、メディアを再生する準備がまだ整っていないかなど、コントロールの現在の状態を示します。

[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)列挙体は、次のメンバーを定義します。

- `Closed` は、`MediaElement` にメディアが含まれていないことを示します。
- `Opening` は、`MediaElement` が検証中で、指定されたソースを読み込もうとしていることを示します。
- `Buffering` は、`MediaElement` が再生用にメディアを読み込んでいることを示します。 この状態では、 [`Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティが事前にはありません。 `MediaElement` がビデオを再生していた場合は、最後に表示されたフレームが引き続き表示されます。
- `Playing` は、`MediaElement` がメディアソースを再生していることを示します。
- `Paused` は、`MediaElement` が[`Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティを進めないことを示します。 `MediaElement` がビデオを再生していた場合は、引き続き現在のフレームが表示されます。
- `Stopped` は、`MediaElement` にメディアが含まれているが、再生または一時停止されていないことを示します。 [`Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティは0であり、事前にはありません。 読み込まれたメディアがビデオの場合、`MediaElement` に最初のフレームが表示されます。

[`MediaElement`](xref:Xamarin.Forms.MediaElement)トランスポートコントロールを使用する場合は、通常、 [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)プロパティを調べる必要はありません。 ただし、独自のトランスポートコントロールを実装する場合は、このプロパティが重要になります。

## <a name="implement-custom-transport-controls"></a>カスタムトランスポートコントロールを実装する

Media player のトランスポートコントロールには、**再生**、**一時停止**、および**停止**の機能を実行するボタンが含まれています。 これらのボタンは一般的に、テキストではなく使い慣れたアイコンで識別されます。また、**再生**と**一時停止**機能は一般的に、1 つのボタンに結合されています。

既定では、 [`MediaElement`](xref:Xamarin.Forms.MediaElement)再生コントロールは無効になっています。 これにより、プログラムによって `MediaElement` を制御したり、独自のトランスポートコントロールを提供したりできます。 これをサポートするために、`MediaElement` には[`Play`](xref:Xamarin.Forms.MediaElement.Play)、 [`Pause`](xref:Xamarin.Forms.MediaElement.Pause)、および[`Stop`](xref:Xamarin.Forms.MediaElement.Stop)のメソッドが含まれています。

次の XAML の例は、 [`MediaElement`](xref:Xamarin.Forms.MediaElement)とカスタムトランスポートコントロールを含むページを示しています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MediaElementDemos.CustomTransportPage"
             Title="Custom transport">
    <Grid>
        ...
        <MediaElement x:Name="mediaElement"
                      AutoPlay="False"
                      ... />
        <StackLayout BindingContext="{x:Reference mediaElement}"
                     ...>
            <Button Text="&#x25B6;&#xFE0F; Play"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnPlayPauseButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Playing}">
                        <Setter Property="Text"
                                Value="&#x23F8; Pause" />
                    </DataTrigger>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Buffering}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
            <Button Text="&#x23F9; Stop"
                    HorizontalOptions="CenterAndExpand"
                    Clicked="OnStopButtonClicked">
                <Button.Triggers>
                    <DataTrigger TargetType="Button"
                                 Binding="{Binding CurrentState}"
                                 Value="{x:Static MediaElementState.Stopped}">
                        <Setter Property="IsEnabled"
                                Value="False" />
                    </DataTrigger>
                </Button.Triggers>
            </Button>
        </StackLayout>
    </Grid>
</ContentPage>
```

この例では、カスタムトランスポートコントロールは[`Button`](xref:Xamarin.Forms.Button)オブジェクトとして定義されています。 ただし、`Button` オブジェクトは2つだけです。最初の `Button` は**Play**と**Pause**を表し、2番目の `Button` は**Stop**を表します。 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)オブジェクトを使用して、ボタンを有効または無効にしたり、**再生**と**一時停止**の間で最初のボタンを切り替えることができます。 データトリガーの詳細については、「 [Xamarin. Forms triggers](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

分離コードファイルには、 [`Clicked`](xref:Xamarin.Forms.Button.Clicked)イベントのハンドラーがあります。

```csharp
void OnPlayPauseButtonClicked(object sender, EventArgs args)
{
    if (mediaElement.CurrentState == MediaElementState.Stopped ||
        mediaElement.CurrentState == MediaElementState.Paused)
    {
        mediaElement.Play();
    }
    else if (mediaElement.CurrentState == MediaElementState.Playing)
    {
        mediaElement.Pause();
    }
}

void OnStopButtonClicked(object sender, EventArgs args)
{
    mediaElement.Stop();
}
```

再生ボタンが有効になったら、**再生ボタンを**押して再生を開始できます。

[![IOS と Android でのカスタムトランスポートコントロールを使用した MediaElement のスクリーンショット](mediaelement-images/custom-transport-playback.png "ビデオを再生する MediaElement")](mediaelement-images/custom-transport-playback-large.png#lightbox "ビデオを再生する MediaElement")

**[一時停止]** ボタンを押すと、再生が一時停止します。

[![再生が一時停止されている MediaElement のスクリーンショット (iOS と Android)](mediaelement-images/custom-transport-paused.png "一時停止中のビデオを含む MediaElement")](mediaelement-images/custom-transport-paused-large.png#lightbox "一時停止中のビデオを含む MediaElement")

**[停止]** ボタンを押すと、再生が停止し、メディアファイルの開始位置が返されます。

## <a name="implement-a-custom-position-bar"></a>カスタムの位置バーを実装する

各プラットフォームで実装されているトランスポート コントロールには、位置バーが含まれます。 このバーは、スライダーに似ており、その合計期間内のメディアの現在の場所を示しています。 さらに、位置バーを操作して、ビデオ内の新しい位置に前方または後方に移動できます。

カスタムの位置バーを実装するには、メディアの期間と現在の再生位置を把握しておく必要があります。 このデータは、 [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)プロパティと[`Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティで使用できます。

> [!IMPORTANT]
> 正確な位置データを取得するには、 [`Position`](xref:Xamarin.Forms.MediaElement.Position)をポーリングする必要があります。 詳細については、「[位置データのポーリング](#poll-for-position-data)」を参照してください。

次の例に示すように、カスタムの位置バーは[`Slider`](xref:Xamarin.Forms.Slider)を使用して実装できます。

```csharp
public class PositionSlider : Slider
{
    public static readonly BindableProperty DurationProperty =
        BindableProperty.Create(nameof(Duration), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(1),
                                propertyChanged: (bindable, oldValue, newValue) =>
                                {
                                    ((PositionSlider)bindable).SetTimeToEnd();
                                    double seconds = ((TimeSpan)newValue).TotalSeconds;
                                    ((Slider)bindable).Maximum = seconds <= 0 ? 1 : seconds;
                                });

    public TimeSpan Duration
    {
        get { return (TimeSpan)GetValue(DurationProperty); }
        set { SetValue(DurationProperty, value); }
    }

    public static readonly BindableProperty PositionProperty =
        BindableProperty.Create(nameof(Position), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan(0),
                                propertyChanged: (bindable, oldValue, newValue) => ((PositionSlider)bindable).SetTimeToEnd());

    public TimeSpan Position
    {
        get { return (TimeSpan)GetValue(PositionProperty); }
        set { SetValue(PositionProperty, value); }
    }

    static readonly BindablePropertyKey TimeToEndPropertyKey =
        BindableProperty.CreateReadOnly(nameof(TimeToEnd), typeof(TimeSpan), typeof(PositionSlider), new TimeSpan());

    public static readonly BindableProperty TimeToEndProperty = TimeToEndPropertyKey.BindableProperty;

    public TimeSpan TimeToEnd
    {
        get { return (TimeSpan)GetValue(TimeToEndProperty); }
        private set { SetValue(TimeToEndPropertyKey, value); }
    }

    public PositionSlider()
    {
        PropertyChanged += (sender, args) =>
        {
            if (args.PropertyName == "Value")
            {
                TimeSpan newPosition = TimeSpan.FromSeconds(Value);
                if (Math.Abs(newPosition.TotalSeconds - Position.TotalSeconds) / Duration.TotalSeconds > 0.01)
                {
                    Position = newPosition;
                }
            }
        };
    }

    void SetTimeToEnd()
    {
        TimeToEnd = Duration - Position;
    }
}
```

`PositionSlider` クラスは、独自の `Duration` と `Position` バインド可能なプロパティ、および `TimeToEnd` のバインド可能なプロパティを定義します。 3つのプロパティはすべて `TimeSpan`型です。 `Duration` プロパティのプロパティ変更ハンドラーによって、 [`Slider`](xref:Xamarin.Forms.Slider)の `Maximum` プロパティが `TimeSpan` 値の `TotalSeconds` プロパティに設定されます。 `TimeToEnd` プロパティは `Duration` と `Position` プロパティの変更に基づいて計算され、メディアの期間から開始し、再生が進むと0に減少します。

`Slider` が移動されたときに、`PositionSlider` が基になる[`Slider`](xref:Xamarin.Forms.Slider)から更新され、メディアを新しい位置に進めたり、逆にしたりする必要があることを示します。 これは、`PositionSlider` コンストラクターの `PropertyChanged` ハンドラーで検出されます。 このハンドラーでは `Value` プロパティの変更が確認され、`Position` プロパティと異なる場合は、`Position` プロパティが `Value` プロパティから設定されます。 [`Slider`](xref:Xamarin.Forms.Slider)の使用方法の詳細については、「 [Xamarin. フォームスライダー](~/xamarin-forms/user-interface/slider.md) 」を参照してください。

> [!NOTE]
> Android では、 [`Slider`](xref:Xamarin.Forms.Slider)には、`Minimum` と `Maximum` の設定に関係なく、1000の個別の手順しかありません。 メディアの長さが1000秒を超える場合は、2つの異なる `Position` 値が `Slider`の同じ `Value` に対応します。 このため、上記のコードでは、新しい位置と既存の位置が全体の期間の 1 ~ 100 を超えていることを確認しています。

次の例は、ページで使用されている `PositionSlider` を示しています。

```xaml
<controls:PositionSlider x:Name="positionSlider"
                         BindingContext="{x:Reference mediaElement}"
                         Duration="{Binding Duration}"
                         ValueChanged="OnPositionSliderValueChanged">
    <controls:PositionSlider.Triggers>
        <DataTrigger TargetType="controls:PositionSlider"
                     Binding="{Binding CurrentState}"
                     Value="{x:Static MediaElementState.Buffering}">
            <Setter Property="IsEnabled" Value="False" />
        </DataTrigger>
    </controls:PositionSlider.Triggers>
</controls:PositionSlider>
```

この例では、`PositionSlider` の `Duration` プロパティは[`MediaElement`](xref:Xamarin.Forms.MediaElement)の[`Duration`](xref:Xamarin.Forms.MediaElement.Duration)プロパティにデータバインドされています。 [`Slider`](xref:Xamarin.Forms.Slider)の[`Value`](xref:Xamarin.Forms.Slider.Value)プロパティが変更されると、`ValueChanged` イベントが発生し、`OnPositionSliderValueChanged` ハンドラーが実行されます。 このハンドラーは、 [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティを `PositionSlider.Position` プロパティの値に設定します。 そのため、`Slider` をドラッグすると、メディア再生位置が変更されます。

[![カスタム位置バーがある MediaElement のスクリーンショット (iOS と Android)](mediaelement-images/custom-position-bar.png "カスタム位置バーを持つ MediaElement")](mediaelement-images/custom-position-bar-large.png#lightbox "カスタム位置バーを持つ MediaElement")

さらに、 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)オブジェクトを使用して、メディアがバッファリングされているときに `PositionSlider` を無効にします。 データトリガーの詳細については、「 [Xamarin. Forms triggers](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="implement-a-custom-volume-control"></a>カスタムボリュームコントロールを実装する

各プラットフォームによって実装されるメディア再生コントロールには、ボリュームバーがあります。 このバーは、スライダーに似ており、メディアのボリュームを示しています。 さらに、ボリュームバーを操作してボリュームを増減することもできます。

カスタムボリュームバーは、次の例に示すように、 [`Slider`](xref:Xamarin.Forms.Slider)を使用して実装できます。

```xaml
<StackLayout>
    <MediaElement AutoPlay="False"
                  Source="{StaticResource AdvancedAsync}" />
    <Slider Maximum="1.0"
            Minimum="0.0"
            Value="{Binding Volume}"
            Rotation="270"
            WidthRequest="100" />
</StackLayout>
```

この例では、 [`Slider`](xref:Xamarin.Forms.Slider)データは、`Value` プロパティを[`MediaElement`](xref:Xamarin.Forms.MediaElement)の[`Volume`](xref:Xamarin.Forms.MediaElement.Volume)プロパティにバインドします。 これが可能なのは、`Volume` プロパティで `TwoWay` バインディングが使用されているためです。 したがって、`Value` プロパティを変更すると、`Volume` プロパティが変更されます。

> [!NOTE]
> [`Volume`](xref:Xamarin.Forms.MediaElement.Volume)プロパティには、その値が0.0 以上で1.0 以下であることを保証する、vlidation コールバックが含まれています。

[`Slider`](xref:Xamarin.Forms.Slider)の使用方法の詳細については、「 [Xamarin. フォームスライダー](~/xamarin-forms/user-interface/slider.md) 」を参照してください。

## <a name="related-links"></a>関連リンク

- [MediaElementDemos (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/MediaElementDemos)
- [URI スキーム](/windows/uwp/app-resources/uri-schemes)
- [Xamarin. フォームトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin. フォームスライダー](~/xamarin-forms/user-interface/slider.md)
- [Android: サポートされているメディア形式](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: サポートされているコーデック](/windows/uwp/audio-video-camera/supported-codecs)
