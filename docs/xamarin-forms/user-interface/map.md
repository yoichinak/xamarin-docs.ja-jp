---
title: Xamarin.Forms のマップ
description: この記事では、各プラットフォームでネイティブ マップ Api を使用して、使い慣れたマップ ユーザーのエクスペリエンスを提供する Xamarin.Forms Map クラスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/13/2019
ms.openlocfilehash: 60d78797406f2e69c435fb597e36775d906852f9
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2019
ms.locfileid: "67513109"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms のマップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithMaps/)

_Xamarin.Forms は、各プラットフォームでネイティブ マップ Api を使用します。_

Xamarin.Forms.Maps は、各プラットフォームでネイティブ マップ Api を使用します。 これにより、ユーザーは、マップの高速で使い慣れたエクスペリエンスを提供しますが、各プラットフォーム API の要件に準拠するいくつかの構成手順が必要であることを意味します。
構成すると、`Map`共通コードでその他の Xamarin.Forms 要素と同様の動作を制御します。

マップ コントロールが使用されて、 [MapsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithMaps/)サンプルは、次のとおりです。

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

-  **iOS** -AppDelegate.cs ファイルで、`FinishedLaunching`メソッド。
-  **Android** -MainActivity.cs ファイルで、`OnCreate`メソッド。
-  **UWP** -MainPage.xaml.cs ファイルで、`MainPage`コンストラクター。

NuGet パッケージが追加され、各アプリケーション内で初期化メソッドが呼び出された後`Xamarin.Forms.Maps`Api は、一般的な .NET Standard ライブラリ プロジェクトまたは共有プロジェクトのコードで使用できます。

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

有効な API キーがない場合は、マップ コントロールは、Android で灰色のボックスとして表示されます。

> [!NOTE]
> 注意してください、APK を Google Maps にアクセスするためには、する必要があります sha-1 指紋が含まれますすべてキーストアの (デバッグとリリース)、APK の署名に使用する名前をパッケージ化します。 たとえば、デバッグおよびリリース APK を生成するための別のコンピュータを 1 台のコンピューターを使用する場合を含める必要がありますデバッグ キーストアを最初のコンピューターから、sha-1 証明書フィンガー プリントのリリース キーストアから sha-1 証明書フィンガー プリント2 番目のコンピューター。 場合は、キーの資格情報を編集することも忘れないでアプリの**パッケージ名**変更します。 参照してください[v2 Google マップ API キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)します。

Android プロジェクトを右クリックして適切なアクセス許可を有効にする必要も**オプション > ビルド > Android アプリケーション**と、次の時間を刻みします。

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

これらの一部は、次のスクリーン ショットに示します。

![Android 用のアクセス許可を必要な](map-images/android-map-permissions.png "Android 用の必要なアクセス許可")

アプリケーションにマップ データのダウンロードへのネットワーク接続が必要なために、最後の 2 つが必要です。 Android について[権限](https://developer.android.com/reference/android/Manifest.permission.html)詳細。

さらに、Android 9 では、Apache HTTP クライアント ライブラリは、bootclasspath から削除し、ないので以上 API 28 を対象とするアプリケーションを使用できます。 次の行を追加する必要があります、`application`のノード、 **AndroidManifest.xml**は引き続き 28 またはそれ以降の API を対象とするアプリケーションでの Apache HTTP クライアントを使用するファイル。

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
map.MapType == MapType.Street;
```

有効な`MapType`値は。

-  ハイブリッド
-  サテライト
-  番地 (既定値)

### <a name="map-region-and-mapspan"></a>マップの領域と MapSpan

上記のコード スニペットに示すように指定して、`MapSpan`インスタンス マップ コンストラクターに初期ビューの設定 (ポイントを中心し、ズーム レベル) が読み込まれるときに、マップの。 `MoveToRegion`マップ クラスのメソッドは、マップの位置やズーム レベルを変更し使用できます。 新たに作成する 2 つの方法がある`MapSpan`インスタンス。

-  **MapSpan.FromCenterAndRadius()** -からのスパンを作成する静的メソッド、`Position`を指定して、`Distance`します。
-  **新しい MapSpan ()** -コンストラクターを使用する、`Position`と緯度と経度を表示する角度。


場所を変更することがなく、マップのズーム レベルを変更するには、新しい作成`MapSpan`から現在の場所を使用して、`VisibleRegion.Center`マップ コントロールのプロパティ。 A `Slider` (ただし、マップ コントロールに直接ズーム スライダーの値を更新できません現在) は、このようなマップのズームを制御するされる可能性があります。

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![マップと zoom](map-images/maps-zoom-sml.png "マップ コントロールのズーム")](map-images/maps-zoom.png#lightbox "マップ コントロールのズーム")

### <a name="map-pins"></a>マップ ピン

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

`PinType` (プラットフォーム) に応じて、暗証番号 (pin) の表示方法に影響を与える可能性があります、次の値のいずれかに設定できます。

-  ジェネリック
-  場所
-  SavedPin
-  SearchResult

### <a name="map-clicks"></a>マップをクリックします。

`Map` 定義、`MapClicked`マップがタップされたときに発生するイベントです。 `MapClickedEventArgs`に付属しているオブジェクト、`MapClicked`イベントという名前の 1 つのプロパティには、 `Position`、型の`Position`します。 ときにイベントが発生しての値、`Position`プロパティがタップされたマップの地域に設定されます。

次のコード例のイベント ハンドラーを示しています、`MapClicked`イベント。

```csharp
map.MapClicked += OnMapClicked;

void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

この例で、`OnMapClicked`イベント ハンドラーは、緯度と経度マップがタップされた位置を表すを出力します。

<a name="Using_Xaml" />

### <a name="create-a-map-in-xaml"></a>XAML でマップを作成します。

この例で示すように、XAML でマップを作成もできます。

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
> 追加`xmlns`Xamarin.Forms.Maps コントロールを参照する名前空間の定義が必要です。

`MapRegion`と`Pins`の名前付き参照を使用してコードで設定することができます、 `Map`:

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="populate-a-map-with-data-using-data-binding"></a>データ バインディングを使用してデータをマップを設定します。

[ `Map` ](xref:Xamarin.Forms.Maps.Map)クラスには、次のプロパティも公開します。

- `ItemsSource` – のコレクションを指定`IEnumerable`表示する項目。
- `ItemTemplate` – を指定します、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)表示されている項目のコレクション内の各項目に適用します。
- `ItemTemplateSelector` – を指定します、 [ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector)選択に使用される、 [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)のランタイムにある項目。

> [!NOTE]
> `ItemTemplate`プロパティが優先と両方、`ItemTemplate`と`ItemTemplateSelector`プロパティを設定します。

A [ `Map` ](xref:Xamarin.Forms.Maps.Map)にバインドするデータ バインディングを使用してデータを設定することができます、`ItemsSource`プロパティを`IEnumerable`コレクション。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
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

`ItemsSource`プロパティ データにバインド、`Locations`を返す接続されているビュー モデルのプロパティ、`ObservableCollection`の`Location`、カスタム型であるオブジェクト。 各`Location`オブジェクトを定義します`Address`と`Description`型のプロパティ、 `string`、および`Position`型のプロパティ、 [ `Position` ](xref:Xamarin.Forms.Maps.Position)。

内の各項目の外観、`IEnumerable`を設定してコレクションが定義されている、`ItemTemplate`プロパティを[ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)を格納している、 [ `Pin` ](xref:Xamarin.Forms.Maps.Pin)オブジェクトにデータをバインドします。適切なプロパティです。

次のスクリーン ショットに示す、 [ `Map` ](xref:Xamarin.Forms.Maps.Map)を表示する、 [ `Pin` ](xref:Xamarin.Forms.Maps.Pin)データ バインディングを使用してコレクション。

[![マップにデータのスクリーン ショットには、iOS と Android でのピンがバインドされている](map-images/pins-itemssource.png "ピンがバインドされたデータとマップ")](map-images/pins-itemssource-large.png#lightbox "ピンがバインドされたデータとマップ")

### <a name="choose-item-appearance-at-runtime"></a>実行時に項目の外観を選択します。

内の各項目の外観、`IEnumerable`コレクションを設定して、項目の値に基づいて、実行時に選択できる、`ItemTemplateSelector`プロパティを[ `DataTemplateSelector` ](xref:Xamarin.Forms.DataTemplateSelector):

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

次の例は、`MapItemTemplateSelector`クラス。

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

`MapItemTemplateSelector`クラス定義`DefaultTemplate`と`XamarinTemplate` [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate)さまざまなデータ テンプレートに設定されているプロパティ。 `OnSelectTemplate`メソッドを返します。、 `XamarinTemplate`、ときにラベルとして"Xamarin"を表示する、`Pin`アイテムが"サンフランシスコ"を含むアドレスがあるときに、タップします。 項目は、「サンフランシスコ」を含むアドレスを持っていない場合、`OnSelectTemplate`メソッドが返す、`DefaultTemplate`します。

データ テンプレート セレクターの詳細については、次を参照してください。[作成 Xamarin.Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)します。

## <a name="related-links"></a>関連リンク

- [MapsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithMaps/)
- [カスタム レンダラーをマップします。](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Xamarin.Forms DataTemplateSelector を作成します。](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
