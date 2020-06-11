---
title: "SkiaSharp ビットマップをファイルに保存する" 説明: "ユーザーの写真ライブラリにビットマップを保存するために、SkiaSharp でサポートされているさまざまなファイル形式を調べます。"
ms. 製品: xamarin ms テクノロジ: skiasharp: 2D696CB6-B31B-42BC-8D3B-11D63B1E7D9C author: davidbritch dabritch: ms. date: 07/10/2018 no loc: [ Xamarin.Forms ,] を指定します。 Xamarin.Essentials
---

# <a name="saving-skiasharp-bitmaps-to-files"></a>SkiaSharp ビットマップをファイルに保存しています

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

SkiaSharp アプリケーションでビットマップを作成または変更した後、アプリケーションはビットマップをユーザーの写真ライブラリに保存することが必要になる場合があります。

![ビットマップの保存](saving-images/SavingSample.png "ビットマップの保存")

このタスクでは、次の2つの手順を実行します。

- SkiaSharp ビットマップを、JPEG や PNG などの特定のファイル形式のデータに変換します。
- プラットフォーム固有のコードを使用して、結果をフォトライブラリに保存します。

## <a name="file-formats-and-codecs"></a>ファイル形式とコーデック

今日の一般的なビットマップファイル形式のほとんどは、圧縮を使用して記憶域スペースを削減します。 圧縮手法の2つの広範なカテゴリは、"_損失_" と "_損失_なし" です。 これらの用語は、圧縮アルゴリズムによってデータが失われるかどうかを示します。

最も人気のある可逆形式は、Joint Photographic Experts Group によって開発されたもので、JPEG と呼ばれています。 JPEG 圧縮アルゴリズムは、_離散余弦変換_と呼ばれる数学ツールを使用してイメージを分析し、画像の視覚的な忠実性を維持するために重要ではないデータを削除しようとします。 圧縮の程度は、一般に_品質_と呼ばれる設定で制御できます。 品質設定が高いほど、ファイルが大きくなります。

これに対して、ロスレス圧縮アルゴリズムでは、イメージを分析して、データを減らす方法でエンコードできるピクセルの繰り返しとパターンを分析しますが、情報の損失は発生しません。 元のビットマップデータは、圧縮されたファイルから完全に復元できます。 現在使用されているロスレス圧縮ファイル形式は、ポータブルネットワークグラフィックス (PNG) です。

一般に、JPEG は写真に使用されますが、手動またはアルゴリズムで生成された画像には PNG が使用されます。 一部のファイルのサイズを小さくする無損失の圧縮アルゴリズムでは、他のファイルのサイズを大きくする必要があります。 幸いなことに、このサイズの増加は一般に、ランダム (または一見ランダム) の情報が大量に含まれるデータに対してのみ発生します。

圧縮アルゴリズムは、圧縮と圧縮解除のプロセスを記述する2つの用語を保証するのに十分に複雑です。

- _デコード_ &mdash;ビットマップファイル形式の読み取りと圧縮解除
- _エンコード_ &mdash;ビットマップを圧縮し、ビットマップファイル形式に書き込みます。

クラスには、 [`SKBitmap`](xref:SkiaSharp.SKBitmap) 圧縮されたソースからを作成するという名前のメソッドがいくつか含まれてい `Decode` `SKBitmap` ます。 必要なのは、ファイル名、ストリーム、またはバイト配列を指定することだけです。 デコーダーは、ファイル形式を特定し、適切な内部デコード関数に渡すことができます。

また、クラスに [`SKCodec`](xref:SkiaSharp.SKCodec) は、という2つのメソッドがあります。これは、圧縮された `Create` `SKCodec` ソースからオブジェクトを作成し、アプリケーションがデコードプロセスにさらに関与できるようにします。 (クラスは、 `SKCodec` アニメーション gif ファイルのデコードと接続での[**SkiaSharp ビットマップのアニメーション**](animating.md#gif-animation)化に関する記事に記載されています)。

ビットマップをエンコードする場合は、詳細な情報が必要です。エンコーダーは、アプリケーションで使用する特定のファイル形式 (JPEG または PNG など) を認識している必要があります。 損失の多い形式が必要な場合は、エンコードで必要な品質レベルも把握している必要があります。

クラスは、 `SKBitmap` [`Encode`](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32)) 次の構文を使用して1つのメソッドを定義します。

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

このメソッドの詳細については、後で詳しく説明します。 エンコードされたビットマップは書き込み可能なストリームに書き込まれます。 (の "W" は `SKWStream` "書き込み可能" を意味します)。2番目と3番目の引数は、ファイル形式を指定し、(可逆形式の場合) 必要な品質を 0 ~ 100 の範囲で指定します。

さらに、 [`SKImage`](xref:SkiaSharp.SKImage) クラスと [`SKPixmap`](xref:SkiaSharp.SKPixmap) クラスは、 `Encode` やや汎用性が高く、好みに応じたメソッドも定義します。 オブジェクト `SKImage` のオブジェクトは `SKBitmap` 、静的メソッドを使用して簡単に作成でき [`SKImage.FromBitmap`](xref:SkiaSharp.SKImage.FromBitmap(SkiaSharp.SKBitmap)) ます。 オブジェクトからオブジェクトを取得するには `SKPixmap` `SKBitmap` 、メソッドを使用し [`PeekPixels`](xref:SkiaSharp.SKBitmap.PeekPixels) ます。

[`Encode`](xref:SkiaSharp.SKImage.Encode)によって定義されたメソッドの1つに `SKImage` はパラメーターがなく、PNG 形式に自動的に保存されます。 このパラメーターなしのメソッドは、非常に簡単に使用できます。

## <a name="platform-specific-code-for-saving-bitmap-files"></a>ビットマップファイルを保存するためのプラットフォーム固有のコード

`SKBitmap`オブジェクトを特定のファイル形式にエンコードすると、通常、なんらかのストリームオブジェクト、またはデータの配列が残されます。 いくつかの `Encode` メソッド (で定義されているパラメーターがないものを含む) は、 `SKImage` [`SKData`](xref:SkiaSharp.SKData) メソッドを使用してバイトの配列に変換できるオブジェクトを返し [`ToArray`](xref:SkiaSharp.SKData.ToArray) ます。 その後、このデータをファイルに保存する必要があります。

`System.IO`このタスクには標準のクラスとメソッドを使用できるため、アプリケーションのローカルストレージ内のファイルに保存するのは非常に簡単です。 この手法については、マンデルブロセットの一連のビットマップをアニメーション化する接続での[**SkiaSharp ビットマップのアニメーション**](animating.md#bitmap-animation)化に関する記事で説明されています。

ファイルを他のアプリケーションで共有する場合は、ユーザーの写真ライブラリに保存する必要があります。 このタスクでは、プラットフォーム固有のコードとを使用する必要があり Xamarin.Forms [`DependencyService`](xref:Xamarin.Forms.DependencyService) ます。

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)アプリケーションの**SkiaSharpFormsDemo**プロジェクトでは、 `IPhotoLibrary` クラスで使用されるインターフェイスを定義し `DependencyService` ます。 これにより、メソッドの構文が定義され `SavePhotoAsync` ます。

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

また、このインターフェイスでは、 `PickPhotoAsync` デバイスのフォトライブラリ用のプラットフォーム固有のファイルピッカーを開くために使用されるメソッドも定義されています。

`SavePhotoAsync`では、最初の引数は、JPEG や PNG など、特定のファイル形式に既にエンコードされているビットマップを含むバイト配列です。 アプリケーションでは、作成したすべてのビットマップを、次のパラメーターで指定される特定のフォルダーに、ファイル名の後に指定することができます。 メソッドは、成功したかどうかを示すブール値を返します。

以下のセクションで `SavePhotoAsync` は、を各プラットフォームに実装する方法について説明します。

### <a name="the-ios-implementation"></a>IOS の実装

の iOS 実装で `SavePhotoAsync` は、 [`SaveToPhotosAlbum`](xref:UIKit.UIImage.SaveToPhotosAlbum*) 次のメソッドを使用し `UIImage` ます。

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        NSData nsData = NSData.FromArray(data);
        UIImage image = new UIImage(nsData);
        TaskCompletionSource<bool> taskCompletionSource = new TaskCompletionSource<bool>();

        image.SaveToPhotosAlbum((UIImage img, NSError error) =>
        {
            taskCompletionSource.SetResult(error == null);
        });

        return taskCompletionSource.Task;
    }
}
```

残念ながら、イメージのファイル名またはフォルダーを指定する方法はありません。

IOS プロジェクトの**情報の plist**ファイルには、写真ライブラリに画像を追加することを示すキーが必要です。

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

注意して下さい！ フォトライブラリにアクセスするためのアクセス許可キーは非常に似ていますが、同じではありません。

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Android の実装

の Android 実装では、 `SavePhotoAsync` 引数がであるか空の文字列であるかが最初にチェック `folder` され `null` ます。 その場合は、ビットマップがフォトライブラリのルートディレクトリに保存されます。 それ以外の場合はフォルダーが取得され、存在しない場合は作成されます。

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        try
        {
            File picturesDirectory = Environment.GetExternalStoragePublicDirectory(Environment.DirectoryPictures);
            File folderDirectory = picturesDirectory;

            if (!string.IsNullOrEmpty(folder))
            {
                folderDirectory = new File(picturesDirectory, folder);
                folderDirectory.Mkdirs();
            }

            using (File bitmapFile = new File(folderDirectory, filename))
            {
                bitmapFile.CreateNewFile();

                using (FileOutputStream outputStream = new FileOutputStream(bitmapFile))
                {
                    await outputStream.WriteAsync(data);
                }

                // Make sure it shows up in the Photos gallery promptly.
                MediaScannerConnection.ScanFile(MainActivity.Instance,
                                                new string[] { bitmapFile.Path },
                                                new string[] { "image/png", "image/jpeg" }, null);
            }
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

の呼び出しは `MediaScannerConnection.ScanFile` 厳密には必須ではありませんが、フォトライブラリをすぐに確認することでプログラムをテストする場合は、ライブラリギャラリービューを更新することで、非常に役立ちます。

**AndroidManifest.xml**ファイルには、次のアクセス許可タグが必要です。

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>UWP 実装

の UWP 実装 `SavePhotoAsync` は、構造体と Android の実装で非常によく似ています。

```csharp
public class PhotoLibrary : IPhotoLibrary
{
    ···
    public async Task<bool> SavePhotoAsync(byte[] data, string folder, string filename)
    {
        StorageFolder picturesDirectory = KnownFolders.PicturesLibrary;
        StorageFolder folderDirectory = picturesDirectory;

        // Get the folder or create it if necessary
        if (!string.IsNullOrEmpty(folder))
        {
            try
            {
                folderDirectory = await picturesDirectory.GetFolderAsync(folder);
            }
            catch
            { }

            if (folderDirectory == null)
            {
                try
                {
                    folderDirectory = await picturesDirectory.CreateFolderAsync(folder);
                }
                catch
                {
                    return false;
                }
            }
        }

        try
        {
            // Create the file.
            StorageFile storageFile = await folderDirectory.CreateFileAsync(filename,
                                                CreationCollisionOption.GenerateUniqueName);

            // Convert byte[] to Windows buffer and write it out.
            IBuffer buffer = WindowsRuntimeBuffer.Create(data, 0, data.Length, data.Length);
            await FileIO.WriteBufferAsync(storageFile, buffer);
        }
        catch
        {
            return false;
        }

        return true;
    }
}
```

**Package.appxmanifest**ファイルの**Capabilities**セクションには、**画像ライブラリ**が必要です。

## <a name="exploring-the-image-formats"></a>イメージ形式の検証

[`Encode`](xref:SkiaSharp.SKBitmap.Encode(SkiaSharp.SKWStream,SkiaSharp.SKEncodedImageFormat,System.Int32))もう一度の方法を `SKImage` 次に示します。

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](xref:SkiaSharp.SKEncodedImageFormat)は、11個のビットマップファイル形式を参照するメンバーを含む列挙体です。その中には、次のように見えにくいものがあります。

- `Astc`&mdash;アダプティブスケーラブルなテクスチャ圧縮
- `Bmp`&mdash;Windows ビットマップ
- `Dng`&mdash;Adobe デジタルネガ
- `Gif`&mdash;グラフィックスのインターチェンジ形式
- `Ico`&mdash;Windows アイコンイメージ
- `Jpeg`&mdash;Joint Photographic Experts Group
- `Ktx`&mdash;OpenGL の Khronos テクスチャ形式
- `Pkm`&mdash;Pokémon ファイルの保存
- `Png`&mdash;ポータブルネットワークグラフィックス
- `Wbmp`&mdash;ワイヤレスアプリケーションプロトコルビットマップ形式 (1 ピクセルあたり1ビット)
- `Webp`&mdash;Google WebP 形式

すぐにわかるように、これらのファイル形式 (、、および) のうち3つだけ `Jpeg` `Png` が、 `Webp` SkiaSharp で実際にサポートされています。

と `SKBitmap` いう名前のオブジェクトを `bitmap` ユーザーのフォトライブラリに保存するには、 `SKEncodedImageFormat` という名前の列挙体のメンバー `imageFormat` と (可逆形式の場合) 整数変数も必要 `quality` です。 次のコードを使用して、フォルダー内のという名前のファイルにビットマップを保存でき `filename` `folder` ます。

```csharp
using (MemoryStream memStream = new MemoryStream())
using (SKManagedWStream wstream = new SKManagedWStream(memStream))
{
    bitmap.Encode(wstream, imageFormat, quality);
    byte[] data = memStream.ToArray();

    // Check the data array for content!

    bool success = await DependencyService.Get<IPhotoLibrary>().SavePhotoAsync(data, folder, filename);

    // Check return value for success!
}
```

`SKManagedWStream`クラスはから派生 `SKWStream` します ("書き込み可能なストリーム" を意味します)。 メソッドは、エンコードされ `Encode` たビットマップファイルをそのストリームに書き込みます。 そのコード内のコメントは、実行する必要のあるエラーチェックを指しています。

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)アプリケーションの [**ファイル形式の保存**] ページでは、同様のコードを使用して、さまざまな形式でビットマップを保存する方法を試すことができます。

XAML ファイルにはビットマップを表示するが含まれていますが、 `SKCanvasView` ページの残りの部分には、アプリケーションがメソッドを呼び出すために必要なすべてが含まれてい `Encode` `SKBitmap` ます。 これには、 `Picker` 列挙体のメンバーのがあります。これには、 `SKEncodedImageFormat` `Slider` 高損失のビットマップ形式の quality 引数の、 `Entry` ファイル名とフォルダー名の2つのビュー、およびファイルを保存するためのがあり `Button` ます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Bitmaps.SaveFileFormatsPage"
             Title="Save Bitmap Formats">

    <StackLayout Margin="10">
        <skiaforms:SKCanvasView PaintSurface="OnCanvasViewPaintSurface"
                                VerticalOptions="FillAndExpand" />

        <Picker x:Name="formatPicker"
                Title="image format"
                SelectedIndexChanged="OnFormatPickerChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKEncodedImageFormat}">
                    <x:Static Member="skia:SKEncodedImageFormat.Astc" />
                    <x:Static Member="skia:SKEncodedImageFormat.Bmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Dng" />
                    <x:Static Member="skia:SKEncodedImageFormat.Gif" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ico" />
                    <x:Static Member="skia:SKEncodedImageFormat.Jpeg" />
                    <x:Static Member="skia:SKEncodedImageFormat.Ktx" />
                    <x:Static Member="skia:SKEncodedImageFormat.Pkm" />
                    <x:Static Member="skia:SKEncodedImageFormat.Png" />
                    <x:Static Member="skia:SKEncodedImageFormat.Wbmp" />
                    <x:Static Member="skia:SKEncodedImageFormat.Webp" />
                </x:Array>
            </Picker.ItemsSource>
        </Picker>

        <Slider x:Name="qualitySlider"
                Maximum="100"
                Value="50" />

        <Label Text="{Binding Source={x:Reference qualitySlider},
                              Path=Value,
                              StringFormat='Quality = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <StackLayout Orientation="Horizontal">
            <Label Text="Folder Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="folderNameEntry"
                   Text="SaveFileFormats"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="File Name: "
                   VerticalOptions="Center" />

            <Entry x:Name="fileNameEntry"
                   Text="Sample.xxx"
                   HorizontalOptions="FillAndExpand" />
        </StackLayout>

        <Button Text="Save"
                Clicked="OnButtonClicked">
            <Button.Triggers>
                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference formatPicker},
                                               Path=SelectedIndex}"
                             Value="-1">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>

                <DataTrigger TargetType="Button"
                             Binding="{Binding Source={x:Reference fileNameEntry},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Button.Triggers>
        </Button>

        <Label x:Name="statusLabel"
               Text="OK"
               Margin="10, 0" />
    </StackLayout>
</ContentPage>
```

分離コードファイルは、ビットマップリソースを読み込み、を使用し `SKCanvasView` てそれを表示します。 そのビットマップは変更されません。 の `SelectedIndexChanged` ハンドラーは、 `Picker` 列挙体メンバーと同じ拡張子を持つファイル名を変更します。

```csharp
public partial class SaveFileFormatsPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(typeof(SaveFileFormatsPage),
        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    public SaveFileFormatsPage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        args.Surface.Canvas.DrawBitmap(bitmap, args.Info.Rect, BitmapStretch.Uniform);
    }

    void OnFormatPickerChanged(object sender, EventArgs args)
    {
        if (formatPicker.SelectedIndex != -1)
        {
            SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
            fileNameEntry.Text = Path.ChangeExtension(fileNameEntry.Text, imageFormat.ToString());
            statusLabel.Text = "OK";
        }
    }

    async void OnButtonClicked(object sender, EventArgs args)
    {
        SKEncodedImageFormat imageFormat = (SKEncodedImageFormat)formatPicker.SelectedItem;
        int quality = (int)qualitySlider.Value;

        using (MemoryStream memStream = new MemoryStream())
        using (SKManagedWStream wstream = new SKManagedWStream(memStream))
        {
            bitmap.Encode(wstream, imageFormat, quality);
            byte[] data = memStream.ToArray();

            if (data == null)
            {
                statusLabel.Text = "Encode returned null";
            }
            else if (data.Length == 0)
            {
                statusLabel.Text = "Encode returned empty array";
            }
            else
            {
                bool success = await DependencyService.Get<IPhotoLibrary>().
                    SavePhotoAsync(data, folderNameEntry.Text, fileNameEntry.Text);

                if (!success)
                {
                    statusLabel.Text = "SavePhotoAsync return false";
                }
                else
                {
                    statusLabel.Text = "Success!";
                }
            }
        }
    }
}
```

の `Clicked` ハンドラーは、 `Button` すべての実際の動作を行います。 とからの2つの引数を取得 `Encode` `Picker` し、 `Slider` 前に示したコードを使用して、メソッドのを作成し `SKManagedWStream` `Encode` ます。 2つのビューは、 `Entry` メソッドのフォルダーとファイル名を表示し `SavePhotoAsync` ます。

この方法のほとんどは、問題やエラーを処理するために費やされています。 が `Encode` 空の配列を作成する場合は、特定のファイル形式がサポートされていないことを意味します。 がを返した場合 `SavePhotoAsync` `false` 、ファイルは正常に保存されませんでした。

次のプログラムが実行されています。

[![ファイル形式を保存する](saving-images/SaveFileFormats.png "ファイル形式を保存する")](saving-images/SaveFileFormats-Large.png#lightbox)

このスクリーンショットは、これらのプラットフォームでサポートされている3つの形式のみを示しています。

- JPEG
- PNG
- WebP

他のすべての形式では、 `Encode` メソッドはストリームに何も書き込みません。結果のバイト配列は空になります。

[**ファイル形式の保存**] ページに保存されるビットマップは、600ピクセルの四角形です。 1ピクセルあたり4バイトで、メモリ内の合計は144万バイトです。 次の表は、ファイル形式と品質のさまざまな組み合わせのファイルサイズを示しています。

|フォーマット|[品質]|サイズ|
|------|------:|---:|
| PNG | 該当なし | 492K |
| JPEG | 0 | 2.95 k |
|      | 50 | 22.1 k |
|      | 100 | 206K |
| WebP | 0 | 2.71 k |
|      | 50 | 11.9 k |
|      | 100 | 101K |

さまざまな品質設定を試して、結果を調べることができます。

## <a name="saving-finger-paint-art"></a>フィンガーペイントアートを保存する

ビットマップの一般的な用途の1つは、プログラムの描画です。この場合、_シャドウビットマップ_と呼ばれるものとして機能します。 すべての描画はビットマップに保持され、その後プログラムによって表示されます。 ビットマップは、描画を保存するのにも便利です。

[**SkiaSharp 記事のフィンガーペイントで**](../paths/finger-paint.md)は、タッチ追跡を使用して、基本的な指描画プログラムを実装する方法を示しています。 このプログラムでサポートされるのは、1つの色と1つのストローク幅のみですが、オブジェクトのコレクションに描画全体を保持し `SKPath` ます。

[**SkiaSharpFormsDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)サンプルの [ **Save を使用したフィンガーペイント**] ページでは、描画全体がオブジェクトのコレクションにも保持され `SKPath` ますが、ビットマップで描画がレンダリングされ、写真ライブラリに保存することができます。

このプログラムの多くは、オリジナルの**フィンガーペイント**プログラムに似ています。 1つの拡張機能として、XAML ファイルで**Clear**および**Save**というラベルのボタンがインスタンス化されるようになりました。

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             xmlns:tt="clr-namespace:TouchTracking"
             x:Class="SkiaSharpFormsDemos.Bitmaps.FingerPaintSavePage"
             Title="Finger Paint Save">

    <StackLayout>
        <Grid BackgroundColor="White"
              VerticalOptions="FillAndExpand">
            <skia:SKCanvasView x:Name="canvasView"
                               PaintSurface="OnCanvasViewPaintSurface" />
            <Grid.Effects>
                <tt:TouchEffect Capture="True"
                                TouchAction="OnTouchEffectAction" />
            </Grid.Effects>
        </Grid>

        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>
        </Grid>

        <Button Text="Clear"
                Grid.Row="0"
                Margin="50, 5"
                Clicked="OnClearButtonClicked" />

        <Button Text="Save"
                Grid.Row="1"
                Margin="50, 5"
                Clicked="OnSaveButtonClicked" />

    </StackLayout>
</ContentPage>
```

分離コードファイルは、という名前の型のフィールドを保持し `SKBitmap` `saveBitmap` ます。 このビットマップは、 `PaintSurface` 表示サーフェイスのサイズが変更されるたびにハンドラーで作成または再作成されます。 ビットマップを再作成する必要がある場合、既存のビットマップの内容は新しいビットマップにコピーされるので、表示サーフェイスのサイズがどのように変化するかにかかわらず、すべてが保持されます。

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    SKBitmap saveBitmap;

    public FingerPaintSavePage ()
    {
        InitializeComponent ();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        // Create bitmap the size of the display surface
        if (saveBitmap == null)
        {
            saveBitmap = new SKBitmap(info.Width, info.Height);
        }
        // Or create new bitmap for a new size of display surface
        else if (saveBitmap.Width < info.Width || saveBitmap.Height < info.Height)
        {
            SKBitmap newBitmap = new SKBitmap(Math.Max(saveBitmap.Width, info.Width),
                                              Math.Max(saveBitmap.Height, info.Height));

            using (SKCanvas newCanvas = new SKCanvas(newBitmap))
            {
                newCanvas.Clear();
                newCanvas.DrawBitmap(saveBitmap, 0, 0);
            }

            saveBitmap = newBitmap;
        }

        // Render the bitmap
        canvas.Clear();
        canvas.DrawBitmap(saveBitmap, 0, 0);
    }
    ···
}
```

ハンドラーによって実行される描画は、最後に発生し、 `PaintSurface` ビットマップをレンダリングするだけで構成されます。

タッチ処理は、以前のプログラムに似ています。 このプログラムは、とという2つのコレクションを保持し `inProgressPaths` `completedPaths` ています。これには、最後に表示がクリアされてからユーザーが描画したすべてのものが含まれます。 各タッチイベントに対して、ハンドラーはを `OnTouchEffectAction` 呼び出し `UpdateBitmap` ます。

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    Dictionary<long, SKPath> inProgressPaths = new Dictionary<long, SKPath>();
    List<SKPath> completedPaths = new List<SKPath>();

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ···
    void OnTouchEffectAction(object sender, TouchActionEventArgs args)
    {
        switch (args.Type)
        {
            case TouchActionType.Pressed:
                if (!inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = new SKPath();
                    path.MoveTo(ConvertToPixel(args.Location));
                    inProgressPaths.Add(args.Id, path);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Moved:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    SKPath path = inProgressPaths[args.Id];
                    path.LineTo(ConvertToPixel(args.Location));
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Released:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    completedPaths.Add(inProgressPaths[args.Id]);
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;

            case TouchActionType.Cancelled:
                if (inProgressPaths.ContainsKey(args.Id))
                {
                    inProgressPaths.Remove(args.Id);
                    UpdateBitmap();
                }
                break;
        }
    }

    SKPoint ConvertToPixel(Point pt)
    {
        return new SKPoint((float)(canvasView.CanvasSize.Width * pt.X / canvasView.Width),
                            (float)(canvasView.CanvasSize.Height * pt.Y / canvasView.Height));
    }

    void UpdateBitmap()
    {
        using (SKCanvas saveBitmapCanvas = new SKCanvas(saveBitmap))
        {
            saveBitmapCanvas.Clear();

            foreach (SKPath path in completedPaths)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }

            foreach (SKPath path in inProgressPaths.Values)
            {
                saveBitmapCanvas.DrawPath(path, paint);
            }
        }

        canvasView.InvalidateSurface();
    }
    ···
}
```

`UpdateBitmap`メソッドは、 `saveBitmap` 新しいを作成して消去し、ビットマップのすべてのパスを表示することによって再描画し `SKCanvas` ます。 `canvasView`画面上にビットマップを描画できるように、無効にすることで終了します。

2つのボタンのハンドラーを次に示します。 [**クリア**] ボタンをクリックすると、パスコレクションと更新 `saveBitmap` (ビットマップがクリアされます) の両方がクリアされ、が無効になり `SKCanvasView` ます。

```csharp
public partial class FingerPaintSavePage : ContentPage
{
    ···
    void OnClearButtonClicked(object sender, EventArgs args)
    {
        completedPaths.Clear();
        inProgressPaths.Clear();
        UpdateBitmap();
        canvasView.InvalidateSurface();
    }

    async void OnSaveButtonClicked(object sender, EventArgs args)
    {
        using (SKImage image = SKImage.FromBitmap(saveBitmap))
        {
            SKData data = image.Encode();
            DateTime dt = DateTime.Now;
            string filename = String.Format("FingerPaint-{0:D4}{1:D2}{2:D2}-{3:D2}{4:D2}{5:D2}{6:D3}.png",
                                            dt.Year, dt.Month, dt.Day, dt.Hour, dt.Minute, dt.Second, dt.Millisecond);

            IPhotoLibrary photoLibrary = DependencyService.Get<IPhotoLibrary>();
            bool result = await photoLibrary.SavePhotoAsync(data.ToArray(), "FingerPaint", filename);

            if (!result)
            {
                await DisplayAlert("FingerPaint", "Artwork could not be saved. Sorry!", "OK");
            }
        }
    }
}
```

**保存**ボタンハンドラーは、の簡略化されたメソッドを使用し [`Encode`](xref:SkiaSharp.SKImage.Encode) `SKImage` ます。 このメソッドは、PNG 形式を使用してエンコードします。 `SKImage`オブジェクトは、に基づいて作成され、オブジェクトにはエンコードされた `saveBitmap` `SKData` PNG ファイルが含まれます。

`ToArray`のメソッドは、 `SKData` バイトの配列を取得します。 これは、固定フォルダー名と共にメソッドに渡され、 `SavePhotoAsync` 現在の日付と時刻から構築された一意のファイル名です。

動作中のプログラムを次に示します。

[![フィンガーペイントによる保存](saving-images/FingerPaintSave.png "フィンガーペイントによる保存")](saving-images/FingerPaintSave-Large.png#lightbox)

[**SpinPaint**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint)サンプルでは、非常によく似た手法が使用されています。 これは、ユーザーが回転するディスク上に描画し、その他の4つの作業領域に設計を再現する点を除いて、指描画プログラムでもあります。 指の描画の色は、ディスクのスピン中に変わります。

[![スピン描画](saving-images/SpinPaint.png "スピン描画")](saving-images/SpinPaint-Large.png#lightbox)

クラスの [**保存**] ボタン `SpinPaint` は、イメージを固定フォルダー名 (**spainpaint**) に保存し、日付と時刻から構築されたファイル名を保存するという点で、**フィンガーペイント**に似ています。

## <a name="related-links"></a>関連リンク

- [SkiaSharp Api](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
- [SpinPaint (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-spinpaint)
