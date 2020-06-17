---
title:"プレーヤーへのビデオ ソースのバインド" description:"この記事では、Xamarin.Forms を使用してビデオ ソースをビデオ プレーヤーにバインドする方法について説明します。"
ms.prod: xamarin ms.assetid:504E0C7E-051A-4AF2-B654-BAB4D0957928 ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date:02/12/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="binding-video-sources-to-the-player"></a>プレーヤーへのビデオ ソースのバインド

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)

`VideoPlayer` ビューの `Source` プロパティが新しいビデオ ファイルに設定されると、既存のビデオの再生が停止し、新しいビデオが開始されます。 この実例は、[**VideoPlayerDemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos) サンプルの **[Select Web Video]\(Web ビデオの選択\)** ページで示されています。 このページには、**App.xaml** ファイルから参照された 3 つのビデオのタイトルを示す `ListView` が含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.SelectWebVideoPage"
             Title="Select Web Video">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0" />

        <ListView Grid.Row="1"
                  ItemSelected="OnListViewItemSelected">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type x:String}">
                    <x:String>Elephant's Dream</x:String>
                    <x:String>Big Buck Bunny</x:String>
                    <x:String>Sintel</x:String>
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

ビデオが選択されると、分離コード ファイルの `ItemSelected` イベント ハンドラーが実行されます。 ハンドラーによってタイトルから空白とアポストロフィが削除され、それがキーとして使用され、**App.xaml** ファイルに定義されているリソースの 1 つが取得されます。 その `UriVideoSource` オブジェクトは `VideoPlayer` の `Source` プロパティに設定されます。

```csharp
namespace VideoPlayerDemos
{
    public partial class SelectWebVideoPage : ContentPage
    {
        public SelectWebVideoPage()
        {
            InitializeComponent();
        }

        void OnListViewItemSelected(object sender, SelectedItemChangedEventArgs args)
        {
            if (args.SelectedItem != null)
            {
                string key = ((string)args.SelectedItem).Replace(" ", "").Replace("'", "");
                videoPlayer.Source = (UriVideoSource)Application.Current.Resources[key];
            }
        }
    }
}
```

ページが初めて読み込まれたときには `ListView` で項目が選択されていないため、ビデオの再生を開始するためにいずれかを選択する必要があります。

[![Web ビデオの選択](source-bindings-images/selectwebvideo-small.png "Web ビデオの選択")](source-bindings-images/selectwebvideo-large.png#lightbox "Web ビデオの選択")

`VideoPlayer` の `Source` プロパティはバインド可能なプロパティによってサポートされています。つまり、データ バインディングのターゲットにすることができます。 この実例は、 **[Bind to VideoPlayer]\(VideoPlayer へのバインド\)** ページで示されています。 **BindToVideoPlayer.xaml** ファイルのマークアップは、ビデオのタイトルと対応する `VideoSource` オブジェクトをカプセル化する次のクラスによってサポートされています。

```csharp
namespace VideoPlayerDemos
{
    public class VideoInfo
    {
        public string DisplayName { set; get; }

        public VideoSource VideoSource { set; get; }

        public override string ToString()
        {
            return DisplayName;
        }
    }
}
```

**BindToVideoPlayer.xaml** ファイルの `ListView` には、これらの `VideoInfo` オブジェクトの配列が含まれており、それぞれ、ビデオ タイトルと、**App.xaml** のリソース ディレクトリの `UriVideoSource` オブジェクトを使用して初期化されています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VideoPlayerDemos"
             xmlns:video="clr-namespace:FormsVideoLibrary"
             x:Class="VideoPlayerDemos.BindToVideoPlayerPage"
             Title="Bind to VideoPlayer">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="2*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <video:VideoPlayer x:Name="videoPlayer"
                           Grid.Row="0"
                           Source="{Binding Source={x:Reference listView},
                                            Path=SelectedItem.VideoSource}" />

        <ListView x:Name="listView"
                  Grid.Row="1">
            <ListView.ItemsSource>
                <x:Array Type="{x:Type local:VideoInfo}">
                    <local:VideoInfo DisplayName="Elephant's Dream"
                                     VideoSource="{StaticResource ElephantsDream}" />

                    <local:VideoInfo DisplayName="Big Buck Bunny"
                                     VideoSource="{StaticResource BigBuckBunny}" />

                    <local:VideoInfo DisplayName="Sintel"
                                     VideoSource="{StaticResource Sintel}" />
                </x:Array>
            </ListView.ItemsSource>
        </ListView>
    </Grid>
</ContentPage>
```

`VideoPlayer` の `Source` プロパティは `ListView` にバインドされています。 バインドの `Path` は `SelectedItem.VideoSource` として指定されます。これは、2 つのプロパティから構成される複合パスです。`SelectedItem` は `ListView` のプロパティです。 選択されている項目は、種類が `VideoInfo` でプロパティが `VideoSource` です。

最初の **[Select Web Video]\(Web ビデオの選択\)** ページと同様に、最初は `ListView` から項目が選択されていないため、再生を開始する前にいずれかのビデオを選択する必要があります。

## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-videoplayerdemos)
