---
title: Xamarin.Forms のマップ
description: この記事では、各プラットフォームでネイティブ マップ Api を使用して、使い慣れたマップ ユーザーのエクスペリエンスを提供する Xamarin.Forms Map クラスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 3d031489fe71c580b309bedba30c524dfe6666db
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057945"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms のマップ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/WorkingWithMaps/)

_Xamarin.Forms は、各プラットフォームでネイティブ マップ Api を使用します。_

Xamarin.Forms.Maps は、各プラットフォームでネイティブ マップ Api を使用します。 これにより、ユーザーは、マップの高速で使い慣れたエクスペリエンスを提供しますが、各プラットフォーム固有の API 要件に準拠するいくつかの構成手順が必要であることを意味します。
構成すると、`Map`共通コードでその他の Xamarin.Forms 要素と同様の動作を制御します。

* [初期化のマップ](#Maps_Initialization)- を使用して`Map`起動時に追加の初期化コードが必要です。
* [プラットフォーム構成](#Platform_Configuration)-各プラットフォームには、作業にマップの一部の構成が必要です。
* [C# でマップを使用して](#Using_Maps)-マップの表示と c# を使用してピン留めします。
* [XAML でマップを使用して](#Using_Xaml)-XAML を使用してマップを表示します。

マップ コントロールが使用されて、 [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)サンプルは、次のとおりです。

 [![MobileCRM サンプル内の対応付け](map-images/maps-zoom-sml.png "マップ コントロールの例")](map-images/maps-zoom.png#lightbox "マップ コントロールの例")

作成することによって、マップ機能をさらに拡張できます、[カスタム レンダラーをマップ](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)します。

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>マップの初期化

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
-  **UWP** -MainPage.xaml.cs ファイルで、`MainPage`コンス トラクター。

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

アプリケーションにマップ データのダウンロードへのネットワーク接続が必要なために、最後の 2 つが必要です。 Android について[権限](http://developer.android.com/reference/android/Manifest.permission.html)詳細。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォームでマップを使用するには、認証トークンを生成する必要があります。 詳細については、次を参照してください。[マップ認証キーを要求](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx)msdn です。

認証トークンを指定し、必要があります、`FormsMaps.Init("AUTHORIZATION_TOKEN")`メソッドの呼び出し、Bing Maps を使用したアプリを認証します。

<a name="Using_Maps" />

## <a name="using-maps"></a>マップの使用

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

上記のコード スニペットに示すように指定して、`MapSpan`インスタンス マップ コンス トラクターに初期ビューの設定 (ポイントを中心し、ズーム レベル) が読み込まれるときに、マップの。 `MoveToRegion`マップ クラスのメソッドは、マップの位置やズーム レベルを変更し使用できます。 新たに作成する 2 つの方法がある`MapSpan`インスタンス。

-  **MapSpan.FromCenterAndRadius()** -からのスパンを作成する静的メソッド、`Position`を指定して、`Distance`します。
-  **新しい MapSpan ()** -コンス トラクターを使用する、`Position`と緯度と経度を表示する角度。


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

 `PinType` pin が (プラットフォーム) によってレンダリングされる方法に影響を与える可能性があります、次の値のいずれかに設定できます。

-  ジェネリック
-  場所
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>Xaml を使用してください。

このスニペットで示すように、マップを Xaml レイアウトの配置もできます。

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
    x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
            x:Name="MyMap"
            IsShowingUser="true"
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

`MapRegion`と`Pins`を使用してコードで設定できる、`MyMap`参照 (または、マップの名前は任意)。 なお、追加`xmlns`Xamarin.Forms.Maps コントロールを参照する名前空間の定義が必要です。

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>まとめ

Xamarin.Forms.Maps は、別個の NuGet Xamarin.Forms ソリューション内の各プロジェクトに追加する必要があります。 追加の初期化コードは、iOS、Android、および UWP 用やその構成手順として、必要があります。

1 回構成されたマップ API は、わずか数行のコードでピン留めするマーカーのマップを表示するために使用できます。 マップをさらに強調することができます、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)します。


## <a name="related-links"></a>関連リンク

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [カスタム レンダラーをマップします。](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
