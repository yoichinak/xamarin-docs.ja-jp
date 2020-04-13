---
title: Xamarin.フォームメディア要素
description: この記事では、Xamarin.Forms アプリケーションでビデオとオーディオを再生する MediaElement を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: e65f1e56-a80d-46c7-9ff4-7ae6650a3165
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/18/2020
ms.openlocfilehash: 6f6c51c428de569ceb09ed6a26cfc36881c86dc5
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628337"
---
# <a name="xamarinforms-mediaelement"></a>Xamarin.フォームメディア要素

![](~/media/shared/preview.png "This API is currently pre-release")

[![サンプルの](~/media/shared/download.png)ダウンロード サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)

[`MediaElement`](xref:Xamarin.Forms.MediaElement)はビデオとオーディオを再生するためのビューです。 基盤となるプラットフォームでサポートされているメディアは、次のソースから再生できます。

- URI (HTTP または HTTPS) を使用した Web。
- URI スキームを使用して、プラットフォーム アプリケーション`ms-appx:///`に埋め込まれたリソース。
- URI スキームを使用して、アプリのローカルおよび一時データ フォルダー`ms-appdata:///`から取得されるファイル。
- デバイスのライブラリ。

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、トランスポート コントロールと呼ばれるプラットフォーム再生コントロールを使用できます。 ただし、既定では無効になっており、独自のトランスポート コントロールに置き換えることができます。 次のスクリーンショットは、`MediaElement`プラットフォームトランスポートコントロールでビデオを再生する様子を示しています。

[![iOSとアンドロイドでビデオを再生するメディア要素のスクリーンショット](mediaelement-images/playback-controls.png "ビデオを再生するメディア要素")](mediaelement-images/playback-controls-large.png#lightbox "ビデオを再生するメディア要素")

[`MediaElement`](xref:Xamarin.Forms.MediaElement)は Xamarin.Forms 4.5 で入手できます。 ただし、現在は試験的なコードであり *、App.xaml.cs*ファイルに次のコード行を追加することによってのみ使用できます。

```csharp
Device.SetFlags(new string[]{ "MediaElement_Experimental" });
```

> [!NOTE]
> [`MediaElement`](xref:Xamarin.Forms.MediaElement)iOS、アンドロイド、ユニバーサル Windows プラットフォーム (UWP)、macOS、Windows プレゼンテーションファウンデーション、および Tizen で利用できます。

[`MediaElement`](xref:Xamarin.Forms.MediaElement)では、次のプロパティを定義します。

- [`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)の種類[`Aspect`](xref:Xamarin.Forms.Aspect)は、表示領域に合わせてメディアを拡大縮小する方法を決定します。 このプロパティの既定値は `AspectFit` です。
- [`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)の種類`bool`は、プロパティが設定されたときにメディアの再生を自動的[`Source`](xref:Xamarin.Forms.MediaElement.Source)に開始するかどうかを示します。 このプロパティの既定値は `true` です。
- [`BufferingProgress`](xref:Xamarin.Forms.MediaElement.BufferingProgress)の種類`double`は、現在のバッファリングの進行状況を示します。 このプロパティの既定値は 0.0 です。
- [`CanSeek`](xref:Xamarin.Forms.MediaElement.CanSeek)の種類`bool`は、[`Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティの値を設定することによってメディアを再配置できるかどうかを示します。 これは、読み取り専用プロパティです。
- [`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)の種類[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)は、コントロールの現在の状態を示します。 これは読み取り専用プロパティで、既定値は`MediaElementState.Closed`です。
- [`Duration`](xref:Xamarin.Forms.MediaElement.Duration)の種類`TimeSpan?`は、現在開いているメディアの継続時間を示します。 これは読み取り専用プロパティで、既定値は`null`です。
- [`IsLooping`](xref:Xamarin.Forms.MediaElement.IsLooping)の種類`bool`は、現在ロードされているメディア ソースが、最後に到達した後に再生を開始から再開するかどうかを示します。 このプロパティの既定値は `false` です。
- [`KeepScreenOn`](xref:Xamarin.Forms.MediaElement.KeepScreenOn)の種類`bool`は、メディアの再生中にデバイス画面をオンにするかどうかを決定します。 このプロパティの既定値は `false` です。
- [`Position`](xref:Xamarin.Forms.MediaElement.Position)の種類`TimeSpan`は、メディアの再生時間を通じて現在の進行状況を表します。 このプロパティの既定値は `TimeSpan.Zero` です。
- [`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)の種類`bool`は、プラットフォームの再生コントロールを表示するかどうかを決定します。 このプロパティの既定値は `false` です。
- [`Source`](xref:Xamarin.Forms.MediaElement.Source)の種類[`MediaSource`](xref:Xamarin.Forms.MediaSource)は、コントロールに読み込まれたメディアのソースを示します。
- [`VideoHeight`](xref:Xamarin.Forms.MediaElement.VideoHeight)の種類`int`は、コントロールの高さを示します。 これは、読み取り専用プロパティです。
- [`VideoWidth`](xref:Xamarin.Forms.MediaElement.VideoWidth)の種類`int`は、コントロールの幅を示します。 これは、読み取り専用プロパティです。
- [`Volume`](xref:Xamarin.Forms.MediaElement.Volume)の種類`double`は、メディアのボリュームを決定します。 このプロパティはバインディング`TwoWay`を使用し、既定値は 1 です。

これらのプロパティは、`CanSeek`プロパティを除き、オブジェクトによって[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)バックアップされるため、データ バインディングのターゲットとなり、スタイルが設定されます。

この[`MediaElement`](xref:Xamarin.Forms.MediaElement)クラスは、次の 4 つのイベントも定義します。

- [`MediaOpened`](xref:Xamarin.Forms.MediaElement.MediaOpened)は、メディア ストリームが検証されて開かれたときに発生します。
- [`MediaEnded`](xref:Xamarin.Forms.MediaElement.MediaEnded)は、メディアの`MediaElement`再生が終了すると起動されます。
- [`MediaFailed`](xref:Xamarin.Forms.MediaElement.MediaFailed)は、メディア ソースに関連付けられたエラーが発生したときに発生します。
- [`SeekCompleted`](xref:Xamarin.Forms.MediaElement.SeekCompleted)は、要求されたシーク操作のシーク ポイントが再生の準備ができたときに発生します。

また、 [`MediaElement`](xref:Xamarin.Forms.MediaElement) 、[`Play`](xref:Xamarin.Forms.MediaElement.Play)[`Pause`](xref:Xamarin.Forms.MediaElement.Pause)および メソッド[`Stop`](xref:Xamarin.Forms.MediaElement.Stop)も含みます。

Android でサポートされるメディア形式については、「developer.android.comで[サポートされるメディア形式](https://developer.android.com/guide/topics/media/media-formats)」を参照してください。 ユニバーサル Windows プラットフォーム (UWP) でサポートされるメディア形式については、「[サポートされているコーデック](/windows/uwp/audio-video-camera/supported-codecs)」を参照してください。

## <a name="play-remote-media"></a>リモート メディアを再生する

は[`MediaElement`](xref:Xamarin.Forms.MediaElement)、HTTP および HTTPS URI スキームを使用してリモート メディア ファイルを再生できます。 これは、プロパティを[`Source`](xref:Xamarin.Forms.MediaElement.Source)メディア ファイルの URI に設定することで実現されます。

```xaml
<MediaElement Source="https://sec.ch9.ms/ch9/5d93/a1eab4bf-3288-4faf-81c4-294402a85d93/XamarinShow_mid.mp4"
              ShowsPlaybackControls="True" />
```

デフォルトでは、プロパティによって定義されたメディアは、[`Source`](xref:Xamarin.Forms.MediaElement.Source)メディアが開かれた直後に再生されます。 メディアの自動再生を抑制するには、[`AutoPlay`](xref:Xamarin.Forms.MediaElement.AutoPlay)プロパティを`false`に設定します。

メディア再生コントロールは既定で無効になっており、プロパティを[`ShowsPlaybackControls`](xref:Xamarin.Forms.MediaElement.ShowsPlaybackControls)に設定`true`して有効にします。 [`MediaElement`](xref:Xamarin.Forms.MediaElement)その後、プラットフォームの再生コントロールを使用します。

## <a name="play-local-media"></a>地元メディアを再生する

ローカル メディアは、次のソースから再生できます。

- URI スキームを使用して、プラットフォーム アプリケーション`ms-appx:///`に埋め込まれたリソース。
- URI スキームを使用して、アプリのローカルおよび一時データ フォルダー`ms-appdata:///`から取得されるファイル。
- デバイスのライブラリ。

これらの URI スキームの詳細については[、「URI スキーム](/windows/uwp/app-resources/uri-schemes)」を参照してください。

### <a name="play-media-embedded-in-the-app-package"></a>アプリ パッケージに埋め込まれたメディアを再生する

URI[`MediaElement`](xref:Xamarin.Forms.MediaElement)スキームを使用して、アプリ パッケージに埋め込まれている`ms-appx:///`メディア ファイルを再生できます。 メディア ファイルは、プラットフォーム プロジェクトに配置することで、アプリ パッケージに埋め込まれます。

プラットフォームプロジェクトにメディアファイルを保存する方法は、プラットフォームごとに異なります。

- iOS では、メディア ファイルは**Resources**フォルダ、またはリソース**フォルダの**サブフォルダに保存する必要があります。 メディア ファイルには、`Build Action`の`BundleResource`が必要です。
- Android では、メディア ファイルは**raw**という名前の**リソース**のサブフォルダに格納する必要があります。 **raw** フォルダーにサブフォルダーを含めることはできません。 メディア ファイルには、`Build Action`の`AndroidResource`が必要です。
- UWP では、メディア ファイルはプロジェクト内の任意のフォルダーに格納できます。 メディア ファイルには、`BuildAction`の`Content`が必要です。

これらの基準を満たすメディア ファイルは、URI スキーム`ms-appx:///`を使用して再生できます。

```xaml
<MediaElement Source="ms-appx:///XamarinForms101UsingEmbeddedImages.mp4"
              ShowsPlaybackControls="True" />
```

データ バインディングを使用する場合、値コンバーターを使用してこの URI スキームを適用できます。

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

その後、の`VideoSourceConverter`インスタンスを使用して、埋め`ms-appx:///`込みメディア ファイルに URI スキームを適用できます。

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

ms-appx URI スキームの詳細については[、「ms-appx および ms-appx-web](/windows/uwp/app-resources/uri-schemes#ms-appx-and-ms-appx-web)」を参照してください。

### <a name="play-media-from-the-apps-local-and-temporary-folders"></a>アプリのローカルフォルダーと一時フォルダーからメディアを再生する

URI[`MediaElement`](xref:Xamarin.Forms.MediaElement)スキームを使用して、アプリのローカルまたは一時データ フォルダーにコピーされたメディア ファイル`ms-appdata:///`を再生できます。

次の例は、[`Source`](xref:Xamarin.Forms.MediaElement.Source)アプリのローカル データ フォルダーに格納されているメディア ファイルに設定されたプロパティを示しています。

```xaml
<MediaElement Source="ms-appdata:///local/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

次の例は、[`Source`](xref:Xamarin.Forms.MediaElement.Source)アプリの一時データ フォルダーに格納されているメディア ファイルに対するプロパティを示しています。

```xaml
<MediaElement Source="ms-appdata:///temp/XamarinVideo.mp4"
              ShowsPlaybackControls="True" />
```

> [!IMPORTANT]
> アプリのローカル または一時データ フォルダーに保存されているメディア ファイルを再生するだけでなく、UWP では、アプリのローミング フォルダーにあるメディア ファイルを再生することもできます。 これは、メディア ファイルの先頭に`ms-appdata:///roaming/`.

データ バインディングを使用する場合、値コンバーターを使用してこの URI スキームを適用できます。

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

その後`VideoSourceConverter`、のインスタンスを使用して、アプリの`ms-appdata:///`ローカルまたは一時データ フォルダー内のメディア ファイルに URI スキームを適用できます。

```xaml
<MediaElement Source="{Binding MediaSource, Converter={StaticResource VideoSourceConverter}}"
              ShowsPlaybackControls="True" />
```

ms-appdata URI スキームの詳細については、「 [ms-appdata](/windows/uwp/app-resources/uri-schemes#ms-appdata)」を参照してください。

#### <a name="copying-a-media-file-to-the-apps-local-or-temporary-data-folder"></a>メディア ファイルをアプリのローカル または一時データ フォルダーにコピーする

アプリのローカルまたは一時データ フォルダーに保存されているメディア ファイルを再生するには、アプリによってメディア ファイルをコピーする必要があります。 これは、たとえば、アプリ パッケージからメディア ファイルをコピーすることによって実現できます。

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
> 上記のコード例では、Xamarin.Essentials`FileSystem`に含まれているクラスを使用しています。 詳細については、「 [Xamarin.Essentials: ファイル システム ヘルパー](~/essentials/file-system-helpers.md?context=xamarin%2Fxamarin-forms&tabs=android)」を参照してください。

### <a name="play-media-from-the-device-library"></a>デバイス ライブラリからメディアを再生する

最近のモバイル デバイスやデスクトップ コンピュータの多くは、デバイスのカメラとマイクを使用してビデオやオーディオを録画できます。 作成されたメディアは、デバイス上のファイルとして保存されます。 これらのファイルは、ライブラリから取得し、 によって[`MediaElement`](xref:Xamarin.Forms.MediaElement)再生できます。

各プラットフォームには、ユーザーがデバイスのライブラリからメディアを選択できる機能が含まれています。 Xamarin.Forms では、プラットフォーム プロジェクトは、この機能を呼び出すことができます、[`DependencyService`](xref:Xamarin.Forms.DependencyService)クラスによって呼び出すことができます。

サンプル アプリケーションで使用されるビデオ 選択依存関係サービスは、ピッカーが`Stream`オブジェクトではなくファイル名を返す点を除いて、[画像ライブラリから写真を選ぶ](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)で定義されている依存関係とよく似ています。 共有コード プロジェクトでは、 という`IVideoPicker`名前の単一のメソッドを`GetVideoFileAsync`定義する インターフェイスを定義します。 各プラットフォームは、このインターフェイスをクラスに`VideoPicker`実装します。

次のコード例は、デバイス ライブラリからメディア ファイルを取得する方法を示しています。

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

ビデオ ピッキング依存関係サービスは、メソッドを`DependencyService.Get`呼び出してプラットフォーム プロジェクトのインターフェイス`IVideoPicker`の実装を取得することによって呼び出されます。 その`GetVideoFileAsync`後、メソッドがそのインスタンスで呼び出され、返されたファイル名を使用[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)してオブジェクトを作成し、[`Source`](xref:Xamarin.Forms.MediaElement.Source)のプロパティに[`MediaElement`](xref:Xamarin.Forms.MediaElement)設定します。

## <a name="change-video-aspect-ratio"></a>ビデオ縦横比を変更する

この[`Aspect`](xref:Xamarin.Forms.MediaElement.Aspect)プロパティは、ビデオメディアを表示領域に合わせて拡大縮小する方法を決定します。 既定では、このプロパティは`AspectFit`列挙型のメンバーに設定されますが、列挙型のメンバーに[`Aspect`](xref:Xamarin.Forms.Aspect)設定できます。

- `AspectFit`は、縦横比を維持したまま、必要に応じてビデオが表示領域に収まるようにレターボックス化されることを示します。
- `AspectFill`は、縦横比を維持したまま、ビデオが表示領域全体に表示されるようにクリップされることを示します。
- `Fill`は、ビデオが表示領域全体に拡大されることを示します。

## <a name="poll-for-position-data"></a>職位データのポーリング

バインド可能プロパティの[`Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティ変更通知は、再生の開始と終了、一時停止などの重要な瞬間にのみ発生します。 したがって、`Position`プロパティへのデータ バインディングでは、正確な位置データが得られるわけではありません。 代わりに、タイマーを設定してプロパティをポーリングする必要があります。

これを行うには、メディアの`OnAppearing`再生時に位置データを必要とするページの上書きが適しています。

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

この例では、オーバーライド`OnAppearing`は、1 秒ごとに`positionLabel`値を`Position`使用して更新するタイマーを開始します。 タイマー コールバックは、コールバックが返されるまで毎秒呼び出`false`されます。 ページ ナビゲーションが発生すると`OnDisappearing`、オーバーライドが実行され、タイマー コールバックが呼び出されなくなります。

## <a name="understand-mediasource-types"></a>メディアソースの種類について

A[`MediaElement`](xref:Xamarin.Forms.MediaElement)は、リモート メディア[`Source`](xref:Xamarin.Forms.MediaElement.Source)ファイルまたはローカル メディア ファイルにプロパティを設定することで、メディアを再生できます。 プロパティ`Source`は 型[`MediaSource`](xref:Xamarin.Forms.MediaSource)で、このクラスは 2 つの静的メソッドを定義します。

- [`FromFile`](xref:Xamarin.Forms.MediaSource.FromFile*)は、引数[`MediaSource`](xref:Xamarin.Forms.MediaSource)からインスタンスを`string`返します。
- [`FromUri`](xref:Xamarin.Forms.MediaSource.FromUri*)は、引数[`MediaSource`](xref:Xamarin.Forms.MediaSource)からインスタンスを`Uri`返します。

また[`MediaSource`](xref:Xamarin.Forms.MediaSource)、このクラスには、`MediaSource`インスタンス`string`と`Uri`引数を返す暗黙の演算子もあります。

> [!NOTE]
> プロパティが[`Source`](xref:Xamarin.Forms.MediaElement.Source)XAML で設定されると、型コンバーターが呼び出され、 [`MediaSource`](xref:Xamarin.Forms.MediaSource) `string`または`Uri`からインスタンスを返す。

この[`MediaSource`](xref:Xamarin.Forms.MediaSource)クラスには、2 つの派生クラスもあります。

- [`UriMediaSource`](xref:Xamarin.Forms.UriMediaSource)を使用して、URI からリモート メディア ファイルを指定します。 このクラスには、[`Uri`](xref:Xamarin.Forms.UriMediaSource.Uri)に設定できるプロパティがあります`Uri`。
- [`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)のローカル メディア ファイルを指定するために使用されます`string`。 このクラスには、[`File`](xref:Xamarin.Forms.FileMediaSource.File)に設定できるプロパティがあります`string`。 さらに、`string`このクラスには、 オブジェクトに変換する暗黙の`FileMediaSource`演算子と、オブジェクト`FileMediaSource`を`string`.

> [!NOTE]
> XAML[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)でオブジェクトが作成されると、型コンバーターが呼び出され、インスタンス[`FileMediaSource`](xref:Xamarin.Forms.FileMediaSource)が返されます`string`。

## <a name="determine-mediaelement-status"></a>メディア要素のステータスを決定する

この[`MediaElement`](xref:Xamarin.Forms.MediaElement)クラスは、 という名前[`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)の読み取り専用の[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)バインド可能なプロパティを定義します。 このプロパティは、メディアの再生中または一時停止中かどうか、メディアを再生する準備ができていないかどうかなど、コントロールの現在の状態を示します。

列挙[`MediaElementState`](xref:Xamarin.Forms.MediaElementState)体は、次のメンバーを定義します。

- `Closed`は、メディア`MediaElement`が含まれていることを示します。
- `Opening`は`MediaElement`、指定されたソースを検証し、読み込み中であることを示します。
- `Buffering`は、再生`MediaElement`用にメディアをロードしていることを示します。 この[`Position`](xref:Xamarin.Forms.MediaElement.Position)状態では、そのプロパティは進みません。 ビデオを`MediaElement`再生していた場合は、最後に表示されたフレームが表示され続けます。
- `Playing`はメディア`MediaElement`ソースを再生中であることを示します。
- `Paused`は、その`MediaElement`プロパティを[`Position`](xref:Xamarin.Forms.MediaElement.Position)進めないことを示します。 ビデオを`MediaElement`再生している場合は、現在のフレームが表示され続けます。
- `Stopped`は、メディア`MediaElement`が含まれているが、再生または一時停止していないことを示します。 その[`Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティは 0 で、進みません。 ロードされたメディアがビデオの場合、`MediaElement`最初のフレームが表示されます。

トランスポート コントロールを使用する場合、通常[`CurrentState`](xref:Xamarin.Forms.MediaElement.CurrentState)はプロパティを調べる必要はありません。 [`MediaElement`](xref:Xamarin.Forms.MediaElement) ただし、このプロパティは、独自のトランスポート コントロールを実装するときに重要になります。

## <a name="implement-custom-transport-controls"></a>カスタム トランスポート コントロールを実装する

メディア プレーヤーのトランスポート コントロールには、**再生**、**一時停止**、**および停止**機能を実行するボタンがあります。 これらのボタンは一般的に、テキストではなく使い慣れたアイコンで識別されます。また、**再生**と**一時停止**機能は一般的に、1 つのボタンに結合されています。

既定では、[`MediaElement`](xref:Xamarin.Forms.MediaElement)再生コントロールは無効になっています。 これにより、プログラムによって、または`MediaElement`独自のトランスポート コントロールを提供することによって制御できます。 これをサポート`MediaElement`するために、 、[`Play`](xref:Xamarin.Forms.MediaElement.Play)[`Pause`](xref:Xamarin.Forms.MediaElement.Pause)および メソッド[`Stop`](xref:Xamarin.Forms.MediaElement.Stop)が含まれます。

次の XAML の例は、カスタム[`MediaElement`](xref:Xamarin.Forms.MediaElement)トランスポート コントロールを含むページを示しています。

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

この例では、カスタム トランスポート コントロールがオブジェクト[`Button`](xref:Xamarin.Forms.Button)として定義されています。 `Button`ただし、2 つのオブジェクトのみが存在し、最初`Button`のオブジェクトは**再生**と**一時停止**を表`Button`し、2 つ目は**Stop**を表します。 [`DataTrigger`](xref:Xamarin.Forms.DataTrigger)オブジェクトは、ボタンを有効または無効にしたり、最初のボタンを **[再生**] と [**一時停止**] の間で切り替えたりするために使用されます。 データ トリガの詳細については、「 [Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

分離コード ファイルには、イベントのハンドラーがあります[`Clicked`](xref:Xamarin.Forms.Button.Clicked)。

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

**再生**ボタンを押すと、有効になってしまいます。

[![iOS と Android で、カスタム トランスポート コントロールを持つ MediaElement のスクリーンショット](mediaelement-images/custom-transport-playback.png "ビデオを再生するメディア要素")](mediaelement-images/custom-transport-playback-large.png#lightbox "ビデオを再生するメディア要素")

**[一時停止**]ボタンを押すと、再生が一時停止します。

[![iOSとアンドロイドで、再生が一時停止したMediaElementのスクリーンショット](mediaelement-images/custom-transport-paused.png "一時停止したビデオを含むメディア要素")](mediaelement-images/custom-transport-paused-large.png#lightbox "一時停止したビデオを含むメディア要素")

**停止**ボタンを押すと再生が停止し、メディアファイルの位置が先頭に戻ります。

## <a name="implement-a-custom-position-bar"></a>カスタム位置バーを実装する

各プラットフォームで実装されているトランスポート コントロールには、位置バーが含まれます。 このバーはスライダーに似ており、メディアの現在の位置をその合計期間内に表示します。 さらに、位置バーを操作して、ビデオ内の新しい位置に前後に移動することもできます。

カスタム位置バーを実装するには、メディアの継続時間と現在の再生位置を知る必要があります。 このデータは[`Duration`](xref:Xamarin.Forms.MediaElement.Duration)、 および[`Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティで使用できます。

> [!IMPORTANT]
> 正確[`Position`](xref:Xamarin.Forms.MediaElement.Position)な位置データを取得するには、 をポーリングする必要があります。 詳細については、「ポジション[データのポーリング](#poll-for-position-data)」を参照してください。

カスタム位置バーは、次の[`Slider`](xref:Xamarin.Forms.Slider)例に示すように を使用して実装できます。

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

この`PositionSlider`クラスは、独自`Duration`の`Position`バインド可能なプロパティと、`TimeToEnd`バインド可能なプロパティを定義します。 3 つのプロパティはすべて、`TimeSpan`の種類です。 プロパティのプロパティ変更ハンドラーは、`Duration`のプロパティ`Maximum`[`Slider`](xref:Xamarin.Forms.Slider)を値のプロパティに`TotalSeconds`設定します`TimeSpan`。 プロパティ`TimeToEnd`は`Duration`、 プロパティ`Position`の変更に基づいて計算され、メディアの再生時間から開始され、再生が進むにつれてゼロに減少します。

メディアを新しい位置に[`Slider`](xref:Xamarin.Forms.Slider)進める`Slider`か、または反転する必要があることを示すために、 が移動されたときに、基になるから更新されます。 `PositionSlider` これは、コンストラクターの`PropertyChanged`ハンドラーで検出されます`PositionSlider`。 このハンドラーでは `Value` プロパティの変更が確認され、`Position` プロパティと異なる場合は、`Position` プロパティが `Value` プロパティから設定されます。 を使用する方法の[`Slider`](xref:Xamarin.Forms.Slider)詳細については[、Xamarin.Forms スライダー](~/xamarin-forms/user-interface/slider.md)

> [!NOTE]
> Androidでは、[`Slider`](xref:Xamarin.Forms.Slider)設定に関係なく、1000の個別の`Minimum``Maximum`ステップしかありません。 メディアの長さが 1000 秒を超える場合、2 つの異なる`Position`値`Value`が`Slider`同じ . このため、上記のコードでは、新しいポジションと既存のポジションが全期間の100分の1を超えているかどうかをチェックしています。

次の例は、`PositionSlider`ページで使用されている例を示しています。

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

この例では、`Duration`のプロパティは`PositionSlider`、 のプロパティに[`Duration`](xref:Xamarin.Forms.MediaElement.Duration)データ バインドされます[`MediaElement`](xref:Xamarin.Forms.MediaElement)。 プロパティが[`Value`](xref:Xamarin.Forms.Slider.Value)[`Slider`](xref:Xamarin.Forms.Slider)変更されると、イベントが`ValueChanged`発生し、`OnPositionSliderValueChanged`ハンドラーが実行されます。 このハンドラーは、[`MediaElement.Position`](xref:Xamarin.Forms.MediaElement.Position)プロパティをプロパティの値に`PositionSlider.Position`設定します。 したがって、メディア再生`Slider`位置の結果をドラッグすると、次のようになります。

[![iOS と Android で、カスタムの位置バーを持つ MediaElement のスクリーンショット](mediaelement-images/custom-position-bar.png "カスタム位置バーを持つメディア要素")](mediaelement-images/custom-position-bar-large.png#lightbox "カスタム位置バーを持つメディア要素")

また、メディアが[`DataTrigger`](xref:Xamarin.Forms.DataTrigger)バッファリングされているときにオブジェクトを`PositionSlider`使用して を無効にします。 データ トリガの詳細については、「 [Xamarin.Forms トリガー](~/xamarin-forms/app-fundamentals/triggers.md)」を参照してください。

## <a name="implement-a-custom-volume-control"></a>カスタム ボリューム コントロールを実装する

各プラットフォームによって実装されるメディア再生コントロールには、ボリュームバーが含まれます。 このバーはスライダーに似ており、メディアのボリュームを示します。 また、ボリュームバーを操作して、ボリュームを増減することもできます。

カスタム ボリューム バーは、次の[`Slider`](xref:Xamarin.Forms.Slider)例に示すように を使用して実装できます。

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

この例では、データ[`Slider`](xref:Xamarin.Forms.Slider)はその`Value`プロパティを のプロパティに[`Volume`](xref:Xamarin.Forms.MediaElement.Volume)バインドします[`MediaElement`](xref:Xamarin.Forms.MediaElement)。 プロパティが`TwoWay`バインディングを使用`Volume`するため、これは可能です。 したがって、プロパティを`Value`変更すると、プロパティが`Volume`変更されます。

> [!NOTE]
> プロパティ[`Volume`](xref:Xamarin.Forms.MediaElement.Volume)には、値が 0.0 以上、1.0 以下であることを保証する、vlidation コールバックがあります。

を使用する方法の[`Slider`](xref:Xamarin.Forms.Slider)詳細については[、Xamarin.Forms スライダー](~/xamarin-forms/user-interface/slider.md)

## <a name="related-links"></a>関連リンク

- [デモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-mediaelementdemos/)
- [URI スキーム](/windows/uwp/app-resources/uri-schemes)
- [Xamarin.Forms のトリガー](~/xamarin-forms/app-fundamentals/triggers.md)
- [Xamarin.フォームスライダー](~/xamarin-forms/user-interface/slider.md)
- [アンドロイド:サポートされているメディア形式](https://developer.android.com/guide/topics/media/media-formats)
- [UWP: サポートされているコーデック](/windows/uwp/audio-video-camera/supported-codecs)
