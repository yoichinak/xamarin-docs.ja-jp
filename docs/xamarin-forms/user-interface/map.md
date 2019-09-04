---
title: Xamarin.Forms のマップ
description: この記事では、各プラットフォームでネイティブ マップ Api を使用して、使い慣れたマップ ユーザーのエクスペリエンスを提供する Xamarin.Forms Map クラスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2019
ms.openlocfilehash: 242673efb38931eb678432a28f24db0ad9b8cb7d
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228219"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms のマップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

_Xamarin.Forms は、各プラットフォームでネイティブ マップ Api を使用します。_

Xamarin.Forms.Maps は、各プラットフォームでネイティブ マップ Api を使用します。 これにより、ユーザーのための高速で使い慣れたマップエクスペリエンスが提供されますが、各プラットフォーム API の要件に準拠するためにはいくつかの構成手順が必要になります。
構成すると、`Map`共通コードでその他の Xamarin.Forms 要素と同様の動作を制御します。

マップ コントロールが使用されて、 [MapsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)サンプルは、次のとおりです。

 [![MobileCRM サンプル内の対応付け](map-images/maps-zoom-sml.png "マップ コントロールの例")](map-images/maps-zoom.png#lightbox "マップ コントロールの例")

作成することによって、マップ機能をさらに拡張できます、[カスタム レンダラーをマップ](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)します。

<a name="Maps_Initialization" />

## <a name="map-initialization"></a>マップの初期化

Xamarin.Forms アプリケーションにマップを追加するときに**Xamarin.Forms.Maps**個別の NuGet パッケージ、ソリューション内のすべてのプロジェクトに追加する必要があります。
Android では、これもに依存している Xamarin.Forms.Maps を追加するときに自動的にダウンロードされる GooglePlayServices (別の NuGet)。

各アプリケーション プロジェクトで NuGet パッケージをインストールすると、初期化コードが必要な*後*、`Xamarin.Forms.Forms.Init`メソッドの呼び出し。 IOS 用には、次のコードを使用します。

```csharp
Xamarin.FormsMaps.Init();
```

Android と同じパラメーターを渡す必要があります`Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

ユニバーサル Windows プラットフォーム (UWP) のでは、次のコードを使用します。

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

この呼び出しは、プラットフォームごとに、次のファイルに追加します。

- **iOS** -AppDelegate.cs ファイルで、`FinishedLaunching`メソッド。
- **Android** -MainActivity.cs ファイルで、`OnCreate`メソッド。
- **UWP** -MainPage.xaml.cs ファイルで、`MainPage`コンストラクター。

NuGet パッケージが追加され、各アプリケーション内部で初期化メソッドが呼び出される`Xamarin.Forms.Maps`と、共通の .NET Standard ライブラリプロジェクトまたは共有プロジェクトコードで api を使用できるようになります。

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>プラットフォームの構成

マップを表示する前に、一部のプラットフォームで追加の構成手順が必要です。

### <a name="ios"></a>iOS

IOS で位置情報サービスにアクセスする、次のキーを設定する必要があります**Info.plist**:

- iOS 11
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – 使用中のアプリが位置情報サービスを使用します。
  - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – 常に位置情報サービスを使用します。
- iOS 10 以降
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – 使用中のアプリが位置情報サービスを使用します。
  - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – 常に位置情報サービスを使用します。    

IOS 11 以降をサポートするには、3 つのキーを含めることができます: `NSLocationWhenInUseUsageDescription`、 `NSLocationAlwaysAndWhenInUseUsageDescription`、および`NSLocationAlwaysUsageDescription`します。

これらのキーでの XML 表現**Info.plist**を次に示します。 更新する必要があります、`string`アプリケーションが場所情報を使用する方法を反映するように値。

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist**でエントリを追加することも**ソース**ビューの編集中に、 **Info.plist**ファイル。

![IOS 8 の Info.plist](map-images/ios8-map-permissions.png "Info.plist に必要なエントリを iOS 8")

### <a name="android"></a>Android

使用する、 [Google マップ API v2](https://developers.google.com/maps/documentation/android/) Android で API キーを生成し、Android プロジェクトに追加する必要があります。
Xamarin ドキュメントの指示に従って[v2 Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)します。
これらの手順を実行した後で API キーを貼り付け、 **Properties/AndroidManifest.xml**ファイル (ソースの表示と次の要素を検索/更新)。

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

有効な API キーがない場合、maps コントロールは Android で灰色のボックスとして表示されます。

> [!NOTE]
> 注意してください、APK を Google Maps にアクセスするためには、する必要があります sha-1 指紋が含まれますすべてキーストアの (デバッグとリリース)、APK の署名に使用する名前をパッケージ化します。 たとえば、デバッグおよびリリース APK を生成するための別のコンピュータを 1 台のコンピューターを使用する場合を含める必要がありますデバッグ キーストアを最初のコンピューターから、sha-1 証明書フィンガー プリントのリリース キーストアから sha-1 証明書フィンガー プリント2 番目のコンピューター。 場合は、キーの資格情報を編集することも忘れないでアプリの**パッケージ名**変更します。 参照してください[v2 Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)します。

Android プロジェクトを右クリックして適切なアクセス許可を有効にする必要も**オプション > ビルド > Android アプリケーション**と、次の時間を刻みします。

- `AccessCoarseLocation`
- `AccessFineLocation`
- `AccessLocationExtraCommands`
- `AccessMockLocation`
- `AccessNetworkState`
- `AccessWifiState`
- `Internet`

これらの一部は、次のスクリーン ショットに示します。

![Android 用のアクセス許可を必要な](map-images/android-map-permissions.png "Android 用の必要なアクセス許可")

アプリケーションにマップ データのダウンロードへのネットワーク接続が必要なために、最後の 2 つが必要です。 Android について[権限](https://developer.android.com/reference/android/Manifest.permission.html)詳細。

さらに、Android 9 は bootclasspath から Apache HTTP クライアントライブラリを削除したため、API 28 以上を対象とするアプリケーションでは使用できません。 API 28 以降を対象とするアプリケーション`application`で Apache HTTP クライアントを引き続き使用するには、次の行を**androidmanifest .xml**ファイルのノードに追加する必要があります。

```xml
<application ...>
    ...
    <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォームでマップを使用するには、認証トークンを生成する必要があります。 詳細については、[マップ認証キーを要求](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx)msdn を参照してください。

認証トークンを指定し、必要があります、`FormsMaps.Init("AUTHORIZATION_TOKEN")`メソッドの呼び出し、Bing Maps を使用したアプリを認証します。

<a name="Using_Maps" />

## <a name="map-configuration"></a>マップの構成

参照してください、 [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) MobileCRM サンプル コードで、マップ コントロールの使用方法の例についてはします。 単純な`MapPage`クラスは、この通知のようになりますが、新しい`MapSpan`マップのビューを配置が作成されます。

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>マップの種類

マップのコンテンツを設定して変更することも、`MapType`正規ストリート マップ (既定)、衛星画像、または両方の組み合わせを表示するプロパティ。

```csharp
map.MapType = MapType.Street;
```

有効な`MapType`値は。

- `Hybrid`
- `Satellite`
- `Street` (既定値)

### <a name="map-region-and-mapspan"></a>マップ領域と MapSpan

上記のコード スニペットに示すように指定して、`MapSpan`インスタンス マップ コンストラクターに初期ビューの設定 (ポイントを中心し、ズーム レベル) が読み込まれるときに、マップの。 新たに作成する 2 つの方法がある`MapSpan`インスタンス。

- **MapSpan.FromCenterAndRadius()** -からのスパンを作成する静的メソッド、`Position`を指定して、`Distance`します。
- **新しい MapSpan ()** -コンストラクターを使用する、`Position`と緯度と経度を表示する角度。

`MoveToRegion`マップ クラスのメソッドは、マップの位置やズーム レベルを変更し使用できます。 場所を変更することがなく、マップのズーム レベルを変更するには、新しい作成`MapSpan`から現在の場所を使用して、`VisibleRegion.Center`マップ コントロールのプロパティ。 を`Slider`使用すると、次のようにマップのズームを制御できます (ただし、マップコントロールに直接ズームすると、スライダーの値を現在更新することはできません)。

```csharp
Slider slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) =>
{
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

[![マップと zoom](map-images/maps-zoom-sml.png "マップ コントロールのズーム")](map-images/maps-zoom.png#lightbox "マップ コントロールのズーム")

さら[`Map`](xref:Xamarin.Forms.Maps.Map)に、クラスに`MoveToLastRegionOnLayoutChange`は、バインド可能な`bool`プロパティによってサポートされる型のプロパティがあります。 既定では、この`true`プロパティはです。これは、デバイスの回転など、レイアウトの変更が発生したときに、表示されているマップ領域が現在の領域から以前に設定された領域に移動することを示します。 このプロパティがに`false`設定されている場合、レイアウトの変更が発生しても、表示されているマップ領域は中央のままになります。 次の例は、このプロパティを設定する方法を示しています。

```csharp
map.MoveToLastRegionOnLayoutChange = false;
```

### <a name="map-pins"></a>ピンのマップ

マップ上の場所をマークできます`Pin`オブジェクト。

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

`PinType`は、次のいずれかの値に設定できます。これは、プラットフォームによっては、pin のレンダリング方法に影響を与える可能性があります。

- ジェネリック
- 場所
- SavedPin
- SearchResult

### <a name="map-clicks"></a>マップのクリック

`Map`マップが`MapClicked`タップされたときに発生するイベントを定義します。 イベントに`Position` `Position`付随する`MapClickedEventArgs`オブジェクトには、型のという名前のプロパティが1つあります。 `MapClicked` イベントが発生すると、 `Position`プロパティの値が、タップされたマップの場所に設定されます。

次のコード例は、 `MapClicked`イベントのイベントハンドラーを示しています。

```csharp
map.MapClicked += OnMapClicked;

void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

この例では、 `OnMapClicked`イベントハンドラーは、タップされたマップの場所を表す緯度と経度を出力します。

<a name="Using_Xaml" />

### <a name="create-a-map-in-xaml"></a>XAML でマップを作成する

次の例に示すように、XAML でマップを作成することもできます。

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map x:Name="MyMap"
                  Clicked="OnMapClicked"
                  WidthRequest="320"
                  HeightRequest="200"                  
                  IsShowingUser="true"
                  MapType="Hybrid" />
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> 追加`xmlns`の名前空間定義は、Xamarin. Forms. マップコントロールを参照するために必要です。

と`MapRegion` は`Pins` 、`Map`の名前付き参照を使用してコードで設定できます。

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="populate-a-map-with-data-using-data-binding"></a>データバインディングを使用してマップにデータを設定する

クラス[`Map`](xref:Xamarin.Forms.Maps.Map)では、次のプロパティも公開されます。

- `ItemsSource`–表示する項目の`IEnumerable`コレクションを指定します。
- `ItemTemplate`–表示さ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)れている項目のコレクション内の各項目に適用するを指定します。
- `ItemTemplateSelector`–実行時[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)に項目のを[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)選択するために使用されるを指定します。

> [!NOTE]
> プロパティ`ItemTemplate` `ItemTemplate`とプロパティ`ItemTemplateSelector`の両方が設定されている場合は、プロパティが優先されます。

データ[`Map`](xref:Xamarin.Forms.Maps.Map)バインディングを使用してデータを設定し、その`ItemsSource`プロパティを`IEnumerable`コレクションにバインドすることができます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  MoveToLastRegionOnLayoutChange="false"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

プロパティ`ItemsSource`データは、接続さ`Locations`れたビュー `ObservableCollection`モデルのプロパティにバインドされます`Location` 。これは、カスタム型のオブジェクトのを返します。 各`Location`オブジェクトは`Address` 、 `Description`型のプロパティ`string`とプロパティ`Position` 、および型[`Position`](xref:Xamarin.Forms.Maps.Position)のプロパティを定義します。

`IEnumerable`コレクション内の各項目の外観を定義するには、 `ItemTemplate`適切なプロパティにデータを[`Pin`](xref:Xamarin.Forms.Maps.Pin)バインドするオブジェクトを含むにプロパティ[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)を設定します。

次のスクリーンショットは[`Map`](xref:Xamarin.Forms.Maps.Map) 、データ[`Pin`](xref:Xamarin.Forms.Maps.Pin)バインディングを使用してコレクションを表示する方法を示しています。

データバインドされ[![た pin を使用したマップのスクリーンショット (](map-images/pins-itemssource.png "データバインドされた pin を使用")した iOS および Android マップ)](map-images/pins-itemssource-large.png#lightbox "データバインドされた pin を使用したマップ")

### <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択する

`IEnumerable`コレクション内の各項目の外観は、 `ItemTemplateSelector`プロパティ[`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)をに設定することにより、項目の値に基づいて実行時に選択できます。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

クラスの`MapItemTemplateSelector`例を次に示します。

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

クラス`MapItemTemplateSelector`は、さまざま`XamarinTemplate`なデータテンプレートに設定されるプロパティと[`DataTemplate`](xref:Xamarin.Forms.DataTemplate)プロパティを定義`DefaultTemplate`します。 メソッド`OnSelectTemplate`はを`XamarinTemplate`返します。このメソッドは、 `Pin`がタップされたときに "Xamarin" をラベルとして表示し、項目に "サンフランシスコ" を含むアドレスがある場合はそれを表示します。 "サンフランシスコ" を含むアドレスが項目に含まれていない`OnSelectTemplate`場合、メソッド`DefaultTemplate`はを返します。

データテンプレートセレクターの詳細については、「 [DataTemplateSelector の作成](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [MapsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [カスタム レンダラーをマップします。](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms のサンプル](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [DataTemplateSelector を作成する](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
- [Maps API](xref:Xamarin.Forms.Maps)
