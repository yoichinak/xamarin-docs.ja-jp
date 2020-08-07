---
title: Xamarin.FormsMediaElement
description: この記事では、MediaElement を使用してアプリケーションでビデオとオーディオを再生する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4a8ca74fc12b59100cc60b72d3c2287cffadfd18
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918111"
---
# <a name="no-locxamarinforms-mediaelement"></a>Xamarin.FormsMediaElement

![プレリリース API](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png) サンプルをダウンロードします](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、ビデオやオーディオを再生するためのビューです。 基になるプラットフォームでサポートされているメディアは、次のソースから再生できます。

- URI (HTTP または HTTPS) を使用した web。
- URI スキームを使用して、プラットフォームアプリケーションに埋め込まれたリソース `ms-appx:///` 。
- URI スキームを使用して、アプリのローカルフォルダーと一時データフォルダーから取得されたファイル `ms-appdata:///` 。
- デバイスのライブラリ。

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、トランスポートコントロールと呼ばれるプラットフォーム再生コントロールを使用できます。 ただし、これらは既定で無効になっており、独自のトランスポートコントロールに置き換えることができます。 次のスクリーンショットは、 `MediaElement` プラットフォームのトランスポートコントロールでビデオを再生する方法を示しています。

[![IOS と Android でビデオを再生している MediaElement のスクリーンショット](mediaelement-images/playback-controls.png "ビデオを再生する MediaElement")](mediaelement-images/playback-controls-large.png#lightbox "ビデオを再生する MediaElement")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は4.5 で使用でき Xamarin.Forms ます。 ただし、現在は実験的であり、 *App.xaml.cs*ファイルに次のコード行を追加することによってのみ使用できます。

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)は、iOS、Android、ユニバーサル Windows プラットフォーム (UWP)、macOS、Windows Presentation Foundation、Tizen で使用できます。

[`MediaElement`](xref:Xamarin.Forms.MediaElement)では、次のプロパティが定義されています。

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)型のは、 [`Aspect`](xref:Xamarin.Forms.Aspect) 表示領域に合わせてメディアがどのように拡大縮小されるかを決定します。 このプロパティの既定値は `AspectFit` です。
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)型のは、 `bool` [`Source`](xref:Xamarin.Forms.MediaElement.Source) プロパティが設定されたときにメディアの再生を自動的に開始するかどうかを示します。 このプロパティの既定値は `true` です。
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)型のは、 `double` 現在のバッファリングの進行状況を示します。 このプロパティの既定値は0.0 です。
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)型のは、 `bool` プロパティの値を設定することによってメディアを再配置できるかどうかを示し [`Position`](xref:Xamarin.Forms.MediaElement.Position) ます。 これは、読み取り専用プロパティです。
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)型のは、 [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) コントロールの現在の状態を示します。 これは読み取り専用プロパティで、既定値は `MediaElementState.Closed` です。
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)型のは、 `TimeSpan?` 現在開いているメディアの期間を示します。 これは、既定値がである読み取り専用プロパティです `null` 。
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)型の。は、現在読み込まれている `bool` メディアソースが最後に到達した後に、最初から再生を再開するかどうかを示します。 このプロパティの既定値は `false` です。
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn)型のは、 `bool` メディアの再生中にデバイスの画面を維持するかどうかを決定します。 このプロパティの既定値は `false` です。
- [`Position`](xref:Xamarin.Forms.MediaElement.Position)型のは、 `TimeSpan` メディアの再生時間における現在の進行状況を示します。 このプロパティの既定値は `TimeSpan.Zero` です。
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)型のは、 `bool` プラットフォームの再生コントロールを表示するかどうかを決定します。 このプロパティの既定値は `false` です。
- [`Source`](xref:Xamarin.Forms.MediaElement.Source)型のは、 [`MediaSource`](xref:Xamarin.Forms.MediaSource) コントロールに読み込まれるメディアのソースを示します。
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight)型のは、 `int` コントロールの高さを示します。 これは、読み取り専用プロパティです。
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)型のは、 `int` コントロールの幅を示します。 これは、読み取り専用プロパティです。
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume)型のは、 `double` メディアのボリュームを決定します。これは、0と1の間の線形スケールで表されます。 このプロパティはバインディングを使用 `TwoWay` し、既定値は1です。

これらのプロパティは、プロパティを除き、 `CanSeek` オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。これは、データバインディングのターゲットとスタイルを設定できることを意味します。

クラスは、 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 次の4つのイベントも定義します。

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)メディアストリームが検証され、開いたときに発生します。
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)が、メディアの再生を終了すると発生 `MediaElement` します。
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)メディアソースに関連付けられたエラーが発生したときに発生します。
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)は、要求されたシーク操作のシークポイントが再生できる状態になったときに発生します。

さらに、には、、、 [`MediaElement`](xref:Xamarin.Forms.MediaElement) およびメソッドが含まれてい [`Play`](xref:Xamarin.Forms.MediaElement.Play) [`Pause`](xref:Xamarin.Forms.MediaElement.Pause) [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) ます。

Android でサポートされているメディア形式の詳細については、「developer.android.com で[サポートされているメディア形式](https://developer.android.com/guide/topics/media/media-formats)」を参照してください。 ユニバーサル Windows プラットフォーム (UWP) でサポートされているメディア形式の詳細については、「[サポートされているコーデック](/windows/uwp/audio-video-camera/supported-codecs)」を参照してください。

## <a name="play-remote-media"></a>リモートメディアを再生する

は、 [`MediaElement`](xref:Xamarin.Forms.MediaElement) HTTP および HTTPS URI スキームを使用してリモートメディアファイルを再生できます。 これを行うには、 [`Source`](xref:Xamarin.Forms.MediaElement.Source) プロパティをメディアファイルの URI に設定します。

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

既定では、プロパティによって定義されるメディアは、 [`Source`](xref:Xamarin.Forms.MediaElement.Source) メディアが開かれた直後に再生されます。 自動メディア再生を抑制するには、プロパティをに設定し [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay) `false` ます。

既定では、メディア再生コントロールは無効になっており、プロパティをに設定することによって有効になってい [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls) `true` ます。 [`MediaElement`](xref:Xamarin.Forms.MediaElement)は、プラットフォームの再生コントロールを使用します。

## <a name="play-local-media"></a>ローカルメディアを再生する

ローカルメディアは、次のソースから再生できます。

- URI スキームを使用して、プラットフォームアプリケーションに埋め込まれたリソース `ms-appx:///` 。
- URI スキームを使用して、アプリのローカルフォルダーと一時データフォルダーから取得されたファイル `ms-appdata:///` 。
- デバイスのライブラリ。

これらの URI スキームの詳細については、「 [uri スキーム](/windows/uwp/app-resources/uri-schemes)」を参照してください。

### <a name="play-media-embedded-in-the-app-package"></a>アプリパッケージに埋め込まれたメディアを再生する

は、 [`MediaElement`](xref:Xamarin.Forms.MediaElement) URI スキームを使用して、アプリパッケージに埋め込まれているメディアファイルを再生でき `ms-appx:///` ます。 メディアファイルは、プラットフォームプロジェクトに配置することによって、アプリパッケージに埋め込まれます。

プラットフォームプロジェクトにメディアファイルを格納することは、プラットフォームによって異なります。

- IOS では、メディアファイルは**resources フォルダーに**保存するか、 **resources**フォルダーのサブフォルダーに格納する必要があります。 メディアファイルには、のが含まれている必要があり `Build Action` `BundleResource` ます。
- Android では、メディアファイルは**raw**という名前の**リソース**のサブフォルダーに格納されている必要があります。 **raw** フォルダーにサブフォルダーを含めることはできません。 メディアファイルには、のが含まれている必要があり `Build Action` `AndroidResource` ます。
- UWP では、メディアファイルはプロジェクト内の任意のフォルダーに格納できます。 メディアファイルには、のが含まれている必要があり `BuildAction` `Content` ます。

これらの条件を満たすメディアファイルは、URI スキームを使用して再生でき `ms-appx:///` ます。

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

次に、のインスタンスを使用して、 `VideoSourceConverter` `ms-appx:///` 埋め込みメディアファイルに URI スキームを適用できます。

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

Ms appx URI スキームの詳細については、「 [ms appx と ms appx-web](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web)」を参照してください。

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>アプリのローカルフォルダーと一時フォルダーからメディアを再生する

は、 [`MediaElement`](xref:Xamarin.Forms.MediaElement) URI スキームを使用して、アプリのローカルまたは一時データフォルダーにコピーされたメディアファイルを再生でき `ms-appdata:///` ます。

次の例は、 [`Source`](xref:Xamarin.Forms.MediaElement.Source) アプリケーションのローカルデータフォルダーに格納されているメディアファイルに設定されたプロパティを示しています。

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

次の例では、 [`Source`](xref:Xamarin.Forms.MediaElement.Source) アプリの一時データフォルダーに格納されているメディアファイルのプロパティを示します。

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> UWP は、アプリのローカルまたは一時データフォルダーに格納されているメディアファイルを再生するだけでなく、アプリのローミングフォルダーにあるメディアファイルを再生することもできます。 これは、メディアファイルをにプレフィックスとして付けることで実現でき `ms-appdata:///roaming/` ます。

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

次に、のインスタンスを使用して、 `VideoSourceConverter` `ms-appdata:///` アプリのローカルまたは一時データフォルダー内のメディアファイルに URI スキームを適用できます。

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
> 上記のコード例では、 `FileSystem` に含まれているクラスを使用して Xamarin.Essentials います。 詳細については、「 [ Xamarin.Essentials ファイルシステムヘルパー](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android)」を参照してください。

### <a name="play-media-from-the-device-library"></a>デバイスライブラリからメディアを再生する

最新のモバイルデバイスやデスクトップコンピューターのほとんどは、デバイスのカメラとマイクを使用してビデオやオーディオを録画する機能を備えています。 作成されたメディアは、デバイスにファイルとして保存されます。 これらのファイルはライブラリから取得でき、によって再生さ [`MediaElement`](xref:Xamarin.Forms.MediaElement) れます。

各プラットフォームには、ユーザーがデバイスのライブラリからメディアを選択できる機能が含まれています。 では Xamarin.Forms 、プラットフォームプロジェクトはこの機能を呼び出すことができ、クラスによって呼び出すことができ [`DependencyService`](xref:Xamarin.Forms.DependencyService) ます。

サンプルアプリケーションで使用されているビデオの選択の依存関係サービスは、オブジェクトではなくファイル名を返すことを除けば、[画像ライブラリからの写真の選択](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)によく似てい `Stream` ます。 共有コードプロジェクトでは、という名前の `IVideoPicker` 1 つのメソッドを定義するという名前のインターフェイスを定義し `GetVideoFileAsync` ます。 次に、各プラットフォームは、このインターフェイスをクラスに実装し `VideoPicker` ます。

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

ビデオの選択依存サービスは、 `DependencyService.Get` プラットフォームプロジェクトでインターフェイスの実装を取得するためにメソッドを呼び出すことによって呼び出され `IVideoPicker` ます。 その `GetVideoFileAsync` 後、そのインスタンスでメソッドが呼び出され、返されたファイル名を使用してオブジェクトが作成され、 [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) のプロパティに設定され [`Source`](xref:Xamarin.Forms.MediaElement.Source) [`MediaElement`](xref:Xamarin.Forms.MediaElement) ます。

## <a name="change-video-aspect-ratio"></a>ビデオの縦横比の変更

プロパティは、 [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect) 表示領域に合わせてビデオメディアがどのように拡大縮小されるかを決定します。 既定では、このプロパティは列挙体のメンバーに設定されて `AspectFit` いますが、列挙体のメンバーのいずれかに設定でき [`Aspect`](xref:Xamarin.Forms.Aspect) ます。

- `AspectFit`縦横比を維持したまま、必要に応じて、表示領域に合わせるためにビデオが letterboxed されることを示します。
- `AspectFill`縦横比を維持したまま、表示領域を塗りつぶすためにビデオがクリップされることを示します。
- `Fill`ビデオを拡大して表示領域を塗りつぶすことを示します。

## <a name="poll-for-position-data"></a>位置データのポーリング

バインド可能なプロパティのプロパティ変更通知は、 [`Position`](xref:Xamarin.Forms.MediaElement.Position) 再生の開始と終了、および一時停止の発生など、キーの瞬間にのみ発生します。 したがって、プロパティへのデータバインディングで `Position` は、正確な位置データは生成されません。 代わりに、タイマーを設定し、プロパティをポーリングする必要があります。

これを行うには、 `OnAppearing` メディアの再生時に位置データを必要とするページの上書きを使用することをお勧めします。

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

この例では、 `OnAppearing` オーバーライドにより、 `positionLabel` 1 秒ごとに値を更新するタイマーが開始され `Position` ます。 タイマーコールバックは、コールバックが戻るまで、1秒ごとに呼び出され `false` ます。 ページナビゲーションが発生すると、 `OnDisappearing` オーバーライドが実行され、呼び出されるタイマーコールバックが停止します。

## <a name="understand-mediasource-types"></a>MediaSource の種類について

は、 [`MediaElement`](xref:Xamarin.Forms.MediaElement) [`Source`](xref:Xamarin.Forms.MediaElement.Source) プロパティをリモートまたはローカルのメディアファイルに設定することにより、メディアを再生できます。 `Source`プロパティは型 [`MediaSource`](xref:Xamarin.Forms.MediaSource) で、このクラスは2つの静的メソッドを定義します。

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)は、 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 引数からインスタンスを返し `string` ます。
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)は、 [`MediaSource`](xref:Xamarin.Forms.MediaSource) 引数からインスタンスを返し `Uri` ます。

また、クラスに [`MediaSource`](xref:Xamarin.Forms.MediaSource) は、 `MediaSource` 引数と引数からインスタンスを返す暗黙の演算子もあり `string` `Uri` ます。

> [!NOTE]
> [`Source`](xref:Xamarin.Forms.MediaElement.Source)プロパティが XAML で設定されている場合、型コンバーターが呼び出され、 [`MediaSource`](xref:Xamarin.Forms.MediaSource) またはからインスタンスが返され `string` `Uri` ます。

[`MediaSource`](xref:Xamarin.Forms.MediaSource)クラスには、次の2つの派生クラスもあります。

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource)。 URI からリモートメディアファイルを指定するために使用されます。 このクラスには [`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri) 、に設定できるプロパティがあり `Uri` ます。
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)。からローカルメディアファイルを指定するために使用され `string` ます。 このクラスには [`File`](xref:Xamarin.Forms.FileMediaSource.File) 、に設定できるプロパティがあり `string` ます。 さらに、このクラスには、をオブジェクトに、オブジェクトをに変換するための暗黙的な演算子があり `string` `FileMediaSource` `FileMediaSource` `string` ます。

> [!NOTE]
> [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)オブジェクトが XAML で作成されると、型コンバーターが呼び出され、からインスタンスが返され [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource) `string` ます。

## <a name="determine-mediaelement-status"></a>MediaElement ステータスの決定

[`MediaElement`](xref:Xamarin.Forms.MediaElement)クラスは、型の、という名前の読み取り専用のバインド可能なプロパティを定義 [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) [`MediaElementState`](xref:Xamarin.Forms.MediaElementState) します。 このプロパティは、メディアを再生するか一時停止するか、メディアを再生する準備がまだ整っていないかなど、コントロールの現在の状態を示します。

[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)列挙体は、次のメンバーを定義します。

- `Closed`にメディアが `MediaElement` 含まれていないことを示します。
- `Opening`が検証中で、指定されたソースを読み込もうとしていることを示し `MediaElement` ます。
- `Buffering`が再生用にメディアを読み込んでいることを示し `MediaElement` ます。 [`Position`](xref:Xamarin.Forms.MediaElement.Position)この状態では、プロパティの処理は進められません。 がビデオを再生していた場合は、 `MediaElement` 最後に表示されたフレームが引き続き表示されます。
- `Playing`がメディアソースを再生していることを示し `MediaElement` ます。
- `Paused`がプロパティを進めることがないことを示し `MediaElement` [`Position`](xref:Xamarin.Forms.MediaElement.Position) ます。 が `MediaElement` ビデオを再生していた場合は、現在のフレームが引き続き表示されます。
- `Stopped`に `MediaElement` メディアが含まれているが、再生または一時停止されていないことを示します。 この [`Position`](xref:Xamarin.Forms.MediaElement.Position) プロパティは0であり、事前にはありません。 読み込まれたメディアがビデオの場合、によって `MediaElement` 最初のフレームが表示されます。

一般に、 [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState) トランスポートコントロールを使用する場合は、プロパティを調べる必要はありません [`MediaElement`](xref:Xamarin.Forms.MediaElement) 。 ただし、独自のトランスポートコントロールを実装する場合は、このプロパティが重要になります。

## <a name="implement-custom-transport-controls"></a>カスタムトランスポートコントロールを実装する

Media player のトランスポートコントロールには、**再生**、**一時停止**、および**停止**の機能を実行するボタンが含まれています。 これらのボタンは一般的に、テキストではなく使い慣れたアイコンで識別されます。また、**再生**と**一時停止**機能は一般的に、1 つのボタンに結合されています。

既定では、 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 再生コントロールは無効になっています。 これにより、をプログラムで制御したり、独自のトランスポートコントロールを指定したりすることができ `MediaElement` ます。 このサポートでは、、、 `MediaElement` およびメソッドが含まれ [`Play`](xref:Xamarin.Forms.MediaElement.Play) [`Pause`](xref:Xamarin.Forms.MediaElement.Pause) [`Stop`](xref:Xamarin.Forms.MediaElement.Stop) ます。

次の XAML の例は、およびカスタムトランスポートコントロールを含むページを示してい [`MediaElement`](xref:Xamarin.Forms.MediaElement) ます。

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

この例では、カスタムトランスポートコントロールはオブジェクトとして定義されてい [`Button`](xref:Xamarin.Forms.Button) ます。 ただし、オブジェクトは2つだけです `Button` 。最初のオブジェクトは `Button` **Play**と**Pause**、2番目は `Button` **停止**を表します。 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)オブジェクトは、ボタンを有効または無効にしたり、**再生**と**一時停止**の間で最初のボタンを切り替えたりするために使用されます。 データトリガーの詳細については、「 [ Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

分離コードファイルには、イベントのハンドラーがあり [`Clicked`](xref:Xamarin.Forms.Button.Clicked) ます。

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

[**一時停止**] ボタンを押すと、再生が一時停止します。

[![再生が一時停止されている MediaElement のスクリーンショット (iOS と Android)](mediaelement-images/custom-transport-paused.png "一時停止中のビデオを含む MediaElement")](mediaelement-images/custom-transport-paused-large.png#lightbox "一時停止中のビデオを含む MediaElement")

[**停止**] ボタンを押すと、再生が停止し、メディアファイルの開始位置が返されます。

## <a name="implement-a-custom-position-bar"></a>カスタムの位置バーを実装する

各プラットフォームで実装されているトランスポート コントロールには、位置バーが含まれます。 このバーは、スライダーに似ており、その合計期間内のメディアの現在の場所を示しています。 さらに、位置バーを操作して、ビデオ内の新しい位置に前方または後方に移動できます。

カスタムの位置バーを実装するには、メディアの期間と現在の再生位置を把握しておく必要があります。 このデータは、 [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) プロパティとプロパティで使用でき [`Position`](xref:Xamarin.Forms.MediaElement.Position) ます。

> [!IMPORTANT]
> [`Position`](xref:Xamarin.Forms.MediaElement.Position)正確な位置データを取得するには、をポーリングする必要があります。 詳細については、「[位置データのポーリング](#poll-for-position-data)」を参照してください。

次の例に示すように、を使用してカスタムの位置バーを実装でき [`Slider`](xref:Xamarin.Forms.Slider) ます。

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

クラスは、 `PositionSlider` 独自の `Duration` バインド可能 `Position` なプロパティと、バインド可能なプロパティを定義し `TimeToEnd` ます。 3つのプロパティはすべて型 `TimeSpan` です。 プロパティのプロパティ変更ハンドラーは `Duration` `Maximum` 、のプロパティ [`Slider`](xref:Xamarin.Forms.Slider) を `TotalSeconds` 値のプロパティに設定し `TimeSpan` ます。 プロパティは、 `TimeToEnd` プロパティとプロパティの変更に基づいて計算され、 `Duration` `Position` メディアの期間から開始し、再生が進むと0に減少します。

が移動されると、は `PositionSlider` 基になるから更新され [`Slider`](xref:Xamarin.Forms.Slider) ます。これは、メディアを高度にするか、 `Slider` 新しい位置に戻す必要があることを示します。 これは、コンストラクターのハンドラーで検出され `PropertyChanged` `PositionSlider` ます。 このハンドラーでは `Value` プロパティの変更が確認され、`Position` プロパティと異なる場合は、`Position` プロパティが `Value` プロパティから設定されます。 の使用方法の詳細については [`Slider`](xref:Xamarin.Forms.Slider) 、「」を参照[ Xamarin.Forms ](~/xamarin-forms/user-interface/slider.md)してください。

> [!NOTE]
> Android では、 [`Slider`](xref:Xamarin.Forms.Slider) との設定に関係なく、には1000の個別の手順しかありませ `Minimum` `Maximum` ん。 メディアの長さが1000秒を超える場合、2つ `Position` の異なる値がの同じ値に対応し `Value` `Slider` ます。 このため、上記のコードでは、新しい位置と既存の位置が全体の期間の 1 ~ 100 を超えていることを確認しています。

次の例は、 `PositionSlider` ページで使用されているを示しています。

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

この例では、の `Duration` プロパティは `PositionSlider` 、のプロパティにデータバインドされてい [`Duration`](xref:Xamarin.Forms.MediaElement.Duration) [`MediaElement`](xref:Xamarin.Forms.MediaElement) ます。 [`Value`](xref:Xamarin.Forms.Slider.Value)のプロパティが変更されると、 [`Slider`](xref:Xamarin.Forms.Slider) イベントが発生し、 `ValueChanged` `OnPositionSliderValueChanged` ハンドラーが実行されます。 このハンドラーは、プロパティを [`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position) プロパティの値に設定し `PositionSlider.Position` ます。 そのため、をドラッグすると、 `Slider` 次のように変更されます。

[![カスタム位置バーがある MediaElement のスクリーンショット (iOS と Android)](mediaelement-images/custom-position-bar.png "カスタム位置バーを持つ MediaElement")](mediaelement-images/custom-position-bar-large.png#lightbox "カスタム位置バーを持つ MediaElement")

また、オブジェクトを [`DataTrigger`](xref:Xamarin.Forms.DataTrigger) 使用して、 `PositionSlider` メディアがバッファリングされるときにを無効にします。 データトリガーの詳細については、「 [ Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="implement-a-custom-volume-control"></a>カスタムボリュームコントロールを実装する

各プラットフォームによって実装されるメディア再生コントロールには、ボリュームバーがあります。 このバーは、スライダーに似ており、メディアのボリュームを示しています。 さらに、ボリュームバーを操作してボリュームを増減することもできます。

カスタムのボリュームバーは、 [`Slider`](xref:Xamarin.Forms.Slider) 次の例に示すように、を使用して実装できます。

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

この例では、 [`Slider`](xref:Xamarin.Forms.Slider) データは `Value` プロパティをのプロパティにバインドし [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) [`MediaElement`](xref:Xamarin.Forms.MediaElement) ます。 これが可能なのは、 `Volume` プロパティがバインディングを使用するためです `TwoWay` 。 したがって、プロパティを変更すると、 `Value` プロパティが変更され `Volume` ます。

> [!NOTE]
> プロパティには、 [`Volume`](xref:Xamarin.Forms.MediaElement.Volume) その値が0.0 以上で1.0 以下であることを保証する、vlidation コールバックが含まれています。

の使用方法の詳細については [`Slider`](xref:Xamarin.Forms.Slider) 、「」を参照[ Xamarin.Forms ](~/xamarin-forms/user-interface/slider.md)してください。

## <a name="related-links"></a>関連リンク

- [MediaElementDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)
- [URI スキーム](/windows/uwp/app-resources/uri-schemes)
- [Xamarin.Forms のトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.FormsSlider](~/xamarin-forms/user-interface/slider.md)
- [Android: サポートされているメディア形式](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: サポートされているコーデック](/windows/uwp/audio-video-camera/supported-codecs)
