---
title: "プレーヤーへのビデオ ソースのバインド"
ms.topic: article
ms.prod: xamarin
ms.assetid: 504E0C7E-051A-4AF2-B654-BAB4D0957928
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/12/2018
ms.openlocfilehash: d0842a54f725e5a9504668f977ba06648a96ee6d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="binding-video-sources-to-the-player"></a>プレーヤーへのビデオ ソースのバインド

ときに、`Source`のプロパティ、`VideoPlayer`ビューが新しいビデオ ファイルに設定されている、既存のビデオの再生が停止および新しいビデオを開始します。 示されるこれは、 **Web ビデオの選択**のページ、 [ **VideoPlayerDemos** ](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)サンプルです。 ページが含まれています、`ListView`から参照されている 3 つのビデオのタイトルと、 **App.xaml**ファイル。

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

ビデオを選択すると、`ItemSelected`分離コード ファイル内のイベント ハンドラーを実行します。 ハンドラーは、任意の空白とアポストロフィ タイトルから削除しで定義されているリソースのいずれかを取得するキーとして使用する、 **App.xaml**ファイル。 ある`UriVideoSource`オブジェクトに設定し、`Source`のプロパティ、`VideoPlayer`です。

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

内の項目が選択されていないページが初めて読み込まれるときに、`ListView`ので、再生を開始するビデオのいずれかを選択する必要があります。

[![Web のビデオを選択して](source-bindings-images/selectwebvideo-small.png "Web ビデオを選択して")](source-bindings-images/selectwebvideo-large.png "Web ビデオ を選択")

`Source`プロパティ`VideoPlayer`バインド可能なプロパティは、データ バインディングのターゲットができることを意味によってサポートされます。 示されるこれは、 **VideoPlayer にバインド**ページ。 内のマークアップ、 **BindToVideoPlayer.xaml**ファイルは、次のビデオとそれに対応するタイトルをカプセル化するクラスでサポートされて`VideoSource`オブジェクト。

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

`ListView`で、 **BindToVideoPlayer.xaml**ファイルには、これらの配列が含まれています`VideoInfo`オブジェクト、各ビデオのタイトルを使用して初期化し、`UriVideoSource`オブジェクト内のリソース ディクショナリから**。App.xaml**:

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

`Source`のプロパティ、`VideoPlayer`にバインドされて、`ListView`です。 `Path`として指定されたバインディングの`SelectedItem.VideoSource`、2 つのプロパティで構成される複合パスは:`SelectedItem`のプロパティは、`ListView`です。 選択した項目の型は`VideoInfo`を持つ、`VideoSource`プロパティです。

最初と同様**Web ビデオの選択** ページで、項目が最初に選択されていないから、`ListView`ので、再生を開始する前に、ビデオのいずれかを選択する必要があります。


## <a name="related-links"></a>関連リンク

- [ビデオ プレーヤーのデモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/VideoPlayerDemos/)
