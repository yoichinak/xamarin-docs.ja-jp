---
title: SkiaSharp のビットマップをファイルに保存します。
description: ユーザーのフォト ライブラリでのビットマップの保存の SkiaSharp でサポートされるさまざまなファイル形式について説明します。
ms.prod: xamarin
ms.technology: xamarin-forms++++++
ms.assetid: 2D696CB6-B31B-42BC-8D3B-11D63B1E7D9C
author: charlespetzold
ms.author: chape
ms.date: 07/10/2018
ms.openlocfilehash: ee9690d52d18c1b0da2b35144b41ae856d64a85d
ms.sourcegitcommit: 7f2e44e6f628753e06a5fe2a3076fc2ec5baa081
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/18/2018
ms.locfileid: "39131524"
---
# <a name="saving-skiasharp-bitmaps-to-files"></a>SkiaSharp のビットマップをファイルに保存します。

SkiaSharp アプリケーションを作成または変更ビットマップが後、アプリケーションがユーザーのフォト ライブラリをビットマップを保存すると。

![ビットマップの保存](saving-images/SavingSample.png "のビットマップの保存")

このタスクには、2 つの手順が含まれます。

- SkiaSharp のビットマップに JPEG または PNG などの特定のファイル形式のデータを変換します。
- プラットフォーム固有のコードを使用して、フォト ライブラリには、結果を保存しています。

## <a name="file-formats-and-codecs"></a>ファイル形式とコーデック

今日の一般的なビットマップ ファイルのほとんどでは、圧縮を使用して記憶域スペースを削減する書式設定します。 圧縮方法の 2 つのカテゴリが呼び出される_損失を伴う_と_ロスレス_します。 これらの用語では、圧縮アルゴリズムの結果データが失われるかどうかを示します。

最も人気のあるデータ損失の形式を使用する、Joint Photographic Experts Group が開発した JPEG と呼びます。 JPEG 圧縮アルゴリズムと呼ばれる数学的なツールを使用して画像を分析し、_不連続コサイン変換_画像のビジュアルの忠実性を維持するために重要でないデータを削除しようとします。 圧縮の度合いと通常呼ばれる設定で制御できる_品質_します。 高い品質の設定のより大きなファイルの結果。

これに対し、無損失圧縮アルゴリズムは、画像の繰り返しとデータが減少しますが、すべての情報の損失につながらないようにエンコードできるピクセルのパターンを分析します。 元のビットマップ データは、圧縮されたファイルの完全復元できます。 使用されるプライマリ無損失圧縮ファイル形式は、ポータブル ネットワーク グラフィックス (PNG) を今すぐです。

一般に、アルゴリズムまたは手動で生成されたイメージに PNG を使用中に JPEG が写真に使用されます。 いくつかのファイルのサイズを縮小する任意の無損失圧縮アルゴリズムは、他のユーザーのサイズを大きく必要がありますとは限りません。 さいわい、このサイズの増加通常のみに大量のランダムな (またはランダム) の情報に含まれるデータのします。

圧縮アルゴリズムは、2 つの用語を必要とするほど複雑では、圧縮と圧縮解除のプロセスについて説明します。

- _デコード_&mdash;ビットマップ ファイルの形式を読み取って、圧縮解除して
- _エンコード_&mdash;ビットマップを圧縮し、ビットマップ ファイル形式への書き込み

[ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/)クラスには、という名前のいくつかのメソッドが含まれています。`Decode`を作成する、`SKBitmap`圧縮ソースからです。 必須では、ファイル名、ストリーム、またはバイト配列を指定するだけです。 デコーダーは、ファイル形式を特定し、適切な内部デコードの関数に渡します。

さらに、 [ `SKCodec` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCodec/)クラスという 2 つのメソッドには`Create`作成できる、`SKCodec`圧縮ソースからオブジェクトし、デコードの処理でより複雑なを取得するアプリケーションを許可します。 (、`SKCodec`クラスは、記事に記載[ **SkiaSharp ビットマップをアニメーション化**](animating.md#gif-animation)アニメーション GIF ファイルのデコードに関連します)。

ビットマップをエンコードするときに詳細が必要です。 エンコーダーは、アプリケーションが (JPEG または PNG、または別のもの) を使用する、特定のファイル形式を知る必要があります。 データ損失の形式を使用する場合は、エンコードは、必要な品質レベルも知る必要があります。 

`SKBitmap`クラスは、1 つ定義[ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Encode/p/SkiaSharp.SKWStream/SkiaSharp.SKEncodedImageFormat/System.Int32/)メソッドで次の構文。

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

このメソッドはすぐについて詳しく説明します。 エンコードされたビットマップは、書き込み可能なストリームに書き込まれます。 (で 'W' `SKWStream` 「書き込み」の略します)。2 番目と 3 番目の引数は、ファイル形式を指定し、(データ損失の形式) の 0 から 100 まで必要な品質。

さらに、 [ `SKImage` ](https://developer.xamarin.com/api/type/SkiaSharp.SKImage/)と[ `SKPixmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKPixmap/)クラスも定義`Encode`メソッドより用途が広くにあるとすることをお勧めします。 簡単に作成することができます、`SKImage`オブジェクトから、`SKBitmap`オブジェクト、静的なを使用して[ `SKImage.FromBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.FromBitmap/p/SkiaSharp.SKBitmap/)メソッド。 取得することができます、`SKPixmap`オブジェクトから、`SKBitmp`オブジェクトを使用して、 [ `PeekPixels` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.PeekPixels()/)メソッド。

1 つ、 [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.Encode()/)によって定義されたメソッド`SKImage`パラメーターを持たないおよび PNG 形式に自動的に保存します。 パラメーターなしのメソッドが非常に簡単に使用します。

## <a name="platform-specific-code-for-saving-bitmap-files"></a>ビットマップ ファイルを保存するためのプラットフォーム固有のコード

エンコードするとき、`SKBitmap`オブジェクトを特定のファイルに書式設定、一般になります、何らかのストリーム オブジェクト、またはデータの配列になっています。 いくつかの`Encode`メソッド (によって定義されたパラメーターのない 1 つを含む`SKImage`) を返す、 [ `SKData` ](https://developer.xamarin.com/api/type/SkiaSharp.SKData/)オブジェクトを使用しているバイトの配列に変換できる、 [ `ToArray` ](https://developer.xamarin.com/api/member/SkiaSharp.SKData.ToArray()/)メソッド。 このデータをファイルに保存しする必要があります。 

標準を使用できるため、アプリケーションのローカル ストレージ内のファイルへの保存は非常に簡単`System.IO`クラスと、このタスクのメソッド。 この手法の説明については、記事の[ **SkiaSharp ビットマップをアニメーション化**](animating.md#bitmap-animation)マンデルブロ集合のビットマップの一連のアニメーション化に関連します。

他のアプリケーションで共有するファイルを設定する場合は、ユーザーのフォト ライブラリに保存する必要があります。 このタスクは、プラットフォーム固有のコードと Xamarin.Forms の使用が必要です。 [ `DependencyService`](xref:Xamarin.Forms.DependencyService)します。

**SkiaSharpFormsDemo**プロジェクト、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)アプリケーション定義、`IPhotoLibrary`で使用されるインターフェイス、`DependencyService`クラス。 定義の構文を`SavePhotoAsync`メソッド。

```csharp
public interface IPhotoLibrary
{
    Task<Stream> PickPhotoAsync();

    Task<bool> SavePhotoAsync(byte[] data, string folder, string filename);
}
```

このインターフェイスを定義も、`PickPhotoAsync`メソッドで、デバイスのフォト ライブラリのプラットフォームに固有のファイル ピッカーを開くために使用します。

`SavePhotoAsync`、最初の引数が既に JPEG または PNG などの特定のファイル形式にエンコードされたビットマップを含むバイト配列。 アプリケーションがファイル名を続けて、次のパラメーターで指定された特定のフォルダーに作成しますすべてのビットマップを分離することができます。 メソッドは、成功をかどうかを示すブール値を返します。

ここではどのように`SavePhotoAsync`は、3 つのプラットフォームに実装します。

### <a name="the-ios-implementation"></a>IOS の実装

IOS のカスタム実装の`SavePhotoAsync`を使用して、 [ `SaveToPhotosAlbum` ](https://developer.xamarin.com/api/member/UIKit.UIImage.SaveToPhotosAlbum/)メソッドの`UIImage`:

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

残念ながら、ファイル名またはイメージのフォルダーを指定する方法はありません。 

**Info.plist** iOS プロジェクトのファイルには、フォト ライブラリにイメージが追加されることを示すキーが必要です。

```xml
<key>NSPhotoLibraryAddUsageDescription</key>
<string>SkiaSharp Forms Demos adds images to your photo library</string>
```

注意して下さい！ 単にフォト ライブラリにアクセスするためのアクセス許可のキーは同じではありませんとよく似ています。

```xml
<key>NSPhotoLibraryUsageDescription</key>
<string>SkiaSharp Forms Demos accesses your photo library</string>
```

### <a name="the-android-implementation"></a>Android の実装

Android の実装の`SavePhotoAsync`場合はまず、`folder`引数が`null`または空の文字列。 そうである場合、ビットマップは、フォト ライブラリのルート ディレクトリに保存されます。 それ以外の場合、フォルダーを取得し、存在しない場合は作成します。

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

呼び出し`MediaScannerConnection.ScanFile`は厳密に必要ありませんが、ライブラリのギャラリー ビューを更新することで多く役立ちますフォト ライブラリをすぐにチェックして、プログラムをテストする場合。

**AndroidManifest.xml**ファイルには、次のアクセス許可のタグが必要です。

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

### <a name="the-uwp-implementation"></a>UWP の実装

UWP 実装`SavePhotoAsync`Android の実装に構造によく似ています。

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

**機能**のセクション、 **Package.appxmanifest**ファイルが必要な**ピクチャ ライブラリ**します。

## <a name="exploring-the-image-formats"></a>イメージ形式の調査

ここでは、 [ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Encode/p/SkiaSharp.SKWStream/SkiaSharp.SKEncodedImageFormat/System.Int32/)メソッドの`SKImage`もう一度。

```csharp
public Boolean Encode (SKWStream dst, SKEncodedImageFormat format, Int32 quality)
```

[`SKEncodedImageFormat`](https://developer.xamarin.com/api/type/SkiaSharp.SKEncodedImageFormat/) これらの一部はあいまいではなく、11 個のビットマップ ファイル形式を参照するメンバーを持つ列挙型を示します。

- `Astc` &mdash; 適応型のスケーラブルなテクスチャの圧縮
- `Bmp` &mdash; Windows ビットマップ
- `Dng` &mdash; Adobe デジタル負の値
- `Gif` &mdash; グラフィックス インターチェンジ形式
- `Ico` &mdash; Windows アイコン イメージ
- `Jpeg` &mdash; Joint Photographic Experts Group
- `Ktx` &mdash; OpenGL の Khronos テクスチャ形式
- `Pkm` &mdash; ファイルを保存 Pokémon
- `Png` &mdash; ポータブル ネットワーク グラフィックス
- `Wbmp` &mdash; ワイヤレス アプリケーション プロトコル ビットマップ形式 (1 ピクセルあたり 1 ビット)
- `Webp` &mdash; Google WebP 形式

後ほど、唯一の 3 つのファイル形式 (`Jpeg`、 `Png`、および`Webp`) SkiaSharp で実際にサポートされています。

保存する、`SKBitmap`という名前のオブジェクト`bitmap`のメンバーの必要も、ユーザーの写真のライブラリに、`SKEncodedImageFormat`という名前の列挙`imageFormat`と (データ損失の形式) の整数`quality`変数。 次のコードを使用するには、名前のファイルにそのビットマップを保存する`filename`で、`folder`フォルダー。

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

`SKManagedWStream`クラスから派生`SKWStream`(「書き込み可能なストリーム」の略)。 `Encode`メソッドは、そのストリームにエンコードされたビットマップ ファイルを書き込みます。 そのコード内のコメントは、エラー チェックを実行する必要がありますを参照してください。 

**ファイル形式の保存**ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)アプリケーションでは、同様のコードを使用して、さまざまな形式で、ビットマップの保存に実験できます。

XAML ファイルが含まれています、`SKCanvasView`ビットマップを表示する、アプリケーションを呼び出す必要があるページの残りの部分がすべて含まれている、`Encode`メソッドの`SKBitmap`します。 `Picker`のメンバーの`SKEncodedImageFormat`列挙型で、`Slider`損失を伴うビットマップ形式の場合、品質の引数の 2 つ`Entry`ファイル名とフォルダー名では、ビュー、`Button`ファイルを保存します。

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

分離コード ファイルはビットマップ リソースを読み込み、使用して、`SKCanvasView`表示します。 ビットマップの変更のことはありません。 `SelectedIndexChanged`のハンドラー、`Picker`列挙型のメンバーと同じである拡張機能とファイル名を変更します。

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

`Clicked`のハンドラー、`Button`はすべて、実際の動作します。 2 つの引数を取得`Encode`から、`Picker`と`Slider`、しを作成する前に示したコードを使用して、`SKManagedWStream`の`Encode`メソッド。 2 つ`Entry`ビューのフォルダーとファイル名を提供する、`SavePhotoAsync`メソッド。

問題やエラーの処理には、このメソッドのほとんどを占めています。 場合`Encode`空の配列を作成します。 特定のファイル形式がサポートされないことを意味します。 場合`SavePhotoAsync`返します`false`、ファイルが正常に保存されませんでした。 

次の 3 つのプラットフォームで実行されるプログラムを次に示します。

[![ファイル形式を保存](saving-images/SaveFileFormats.png "ファイル形式の保存")](saving-images/SaveFileFormats-Large.png#lightbox)

そのスクリーン ショットは、これらのプラットフォームでサポートされている 3 つだけの形式を示しています。

- JPEG
- PNG
- WebP

その他のすべての形式、`Encode`メソッドは、ストリームに書き込みます nothing と結果のバイト配列が空です。

ビットマップを**ファイル形式の保存**ページの保存は 600 ピクセルの四角形。 1 ピクセルあたり 4 バイトのメモリの 1,440,000 バイト数の合計です。 次の表は、ファイル形式と品質のさまざまな組み合わせのファイル サイズを示しています。

|形式|品質|サイズ|
|------|------:|---:|
| PNG | N/A | 492 K |
| JPEG | 0 | 横 2.95 K |
|      | 50 | 22.1 K |
|      | 100 | 206 K |
| WebP | 0 | 2.71 K |
|      | 50 | 11.9 K |
|      | 100 | 101 K |

さまざまな品質設定で実験でき、結果を確認することができます。

## <a name="saving-finger-paint-art"></a>Finger-paint アートを保存しています

ビットマップの一般的な用途の 1 つは、描画プログラムの機能と呼ばれるよう、_シャドウ ビットマップ_します。 すべての描画は、プログラムによって表示されるビットマップに保持されます。 ビットマップにも便利図面を保存するためです。

[ **SkiaSharp の指による描画**](../paths/finger-paint.md)記事は、タッチ プリミティブ フィンガーペインティング プログラムを実装するために追跡を使用する方法を示しました。 プログラムには、1 つだけの色と 1 つだけのストロークの幅がサポートされていますのコレクション全体の描画を保持すること`SKPath`オブジェクト。

**指ペイント保存**ページで、 [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)のコレクション全体の描画にも保持サンプル`SKPath`オブジェクトですが、またフォト ライブラリに保存できますが、ビットマップの描画をレンダリングします。

このプログラムの多くは、元のような**指ペイント**プログラム。 1 つの拡張機能は、XAML ファイルにラベルの付いたボタンがインスタンス化今すぐ**クリア**と**保存**:

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

分離コード ファイルの種類のフィールドを保持する`SKBitmap`という`saveBitmap`します。 このビットマップが作成または再作成、`PaintSurface`ハンドラーの表示のサイズは画面の変更されるたびにします。 ビットマップを再作成する必要がある場合、既存のビットマップの内容は、サイズ、表示画面がどのように変化するかに関係なくすべてのものを保持できるように、新しいビットマップにコピーされます。

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

描画、`PaintSurface`ハンドラーが最後に、発生し、ビットマップをレンダリングするだけで構成されます。

タッチ処理は、以前のバージョンのプログラムに似ています。 プログラムは、2 つのコレクションを保持`inProgressPaths`と`completedPaths`、前回表示が消去された後、ユーザーが描画された内容が含まれています。 各タッチ イベントの`OnTouchEffectAction`ハンドラー呼び出し`UpdateBitmap`:

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

`UpdateBitmap`メソッドの再描画`saveBitmap`新しい`SKCanvas`、オフにすること、およびビットマップ上のすべてのパスをレンダリングします。 無効にすることを断定`canvasView`ディスプレイ上のビットマップを描画できるようにします。

ここでは、2 つのボタンのハンドラーです。 **クリア**ボタンは、両方のパスのコレクション、更新プログラムをクリアします`saveBitmap`(結果は、ビットマップをクリアする) を無効にし、 `SKCanvasView`:。

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

**保存** ボタンのハンドラーを使用して、簡略化された[ `Encode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKImage.Encode()/)メソッドから`SKImage`します。 このメソッドは、PNG 形式を使用してエンコードします。 `SKImage`に基づいてオブジェクトを作成`saveBitmap`、および`SKData`オブジェクトには、エンコード済みの PNG ファイルが含まれています。 

`ToArray`メソッドの`SKData`バイトの配列を取得します。 これに渡される内容が、`SavePhotoAsync`メソッドと固定のフォルダー名では、および現在の日付と時刻から構築された一意のファイル名。

アクションで、プログラムを示します。

[![本の指で保存ペイント](saving-images/FingerPaintSave.png "本の指でペイント保存")](saving-images/FingerPaintSave-Large.png#lightbox)

非常に同様の手法が使用される、 [ **SpinPaint** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/)サンプル。 ユーザーが、その他の 4 つの象限に基づいて設計を再現するスピン ディスクに描画する点を除いてはもフィンガーペインティング プログラムになります。 本の指のペイント変更の色をディスクが回転している場合。

[![ペイントをスピン](saving-images/SpinPaint.png "ペイントの回転")](saving-images/SpinPaint-Large.png#lightbox)

**保存**のボタン`SpinPaint`クラスと似ています**指ペイント**固定フォルダー名に、イメージを保存するで (**SpainPaint**) ファイル名から構成されます日付と時刻。

## <a name="related-links"></a>関連リンク

- [SkiaSharp の Api](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [SpinPaint (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/SpinPaint/)
