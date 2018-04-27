---
title: マップ
description: Xamarin.Forms は各プラットフォームでネイティブ マップ Api を使用します。
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: e296ca79ee03e7fc61532758219b65946a8d4381
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2018
---
# <a name="map"></a>マップ

_Xamarin.Forms は各プラットフォームでネイティブ マップ Api を使用します。_

Xamarin.Forms.Maps は、各プラットフォームでネイティブ マップ Api を使用します。 これにより、ユーザー、マップの高速で使い慣れたエクスペリエンスを提供しますが、各プラットフォーム固有の API 要件に準拠するいくつかの構成手順が必要なことを意味します。
構成後、`Map`一般的なコードでその他の Xamarin.Forms 要素と同じように動作を制御します。

* [初期化をマップ](#Maps_Initialization)を使用して -`Map`起動時に追加の初期化コードが必要です。
* [プラットフォーム構成](#Platform_Configuration)-各プラットフォームには、作業にマップの一部の構成が必要です。
* [C# でマップを使用して](#Using_Maps)-マップの表示と c# を使用してピン留めします。
* [XAML でマップを使用して](#Using_Xaml)-XAML でマップを表示します。

マップ コントロールがで使用されて、 [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)サンプルでは、次に示します。

 [![MobileCRM サンプル内の対応付け](map-images/maps-zoom-sml.png "マップ コントロールの例")](map-images/maps-zoom.png#lightbox "マップ コントロールの例")

作成することで、マップ機能をさらに向上することができます、[カスタム レンダラーをマップ](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)です。

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>マップの初期化

Xamarin.Forms のアプリケーションにマップを追加するときに**Xamarin.Forms.Maps**は、個別の NuGet パッケージ、ソリューション内のすべてのプロジェクトに追加する必要があります。
Android でこれもに依存している Xamarin.Forms.Maps を追加するときに自動的にダウンロードされる GooglePlayServices (別の NuGet)。

各アプリケーション プロジェクトでは、NuGet パッケージをインストールすると、初期化コードが必要な*後*、`Xamarin.Forms.Forms.Init`メソッドの呼び出しです。 IOS 用には、次のコードを使用します。

```csharp
Xamarin.FormsMaps.Init();
```

Android では、同じパラメーターを渡す必要があります`Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

ユニバーサル Windows プラットフォーム (UWP) には、次のコードを使用します。

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

プラットフォームごとに次のファイルでは、この呼び出しを追加します。

-  **iOS** -<code>appdelegate.cs</code> ファイルに、`FinishedLaunching`メソッドです。
-  **Android** -MainActivity.cs ファイルで、`OnCreate`メソッドです。
-  **UWP** -MainPage.xaml.cs ファイルで、`MainPage`コンス トラクターです。

NuGet パッケージが追加されており、各アプリケーションの内部初期化メソッドが呼び出された後`Xamarin.Forms.Maps`Api は、一般的な PCL または共有プロジェクトのコードで使用できます。

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>プラットフォームの構成

マップを表示する前に、一部のプラットフォームで追加の構成手順が必要です。

### <a name="ios"></a>iOS

IOS でロケーション サービスにアクセスする、次のキーを設定する必要があります**Info.plist**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) -アプリを使用するときに、ロケーション サービスを使用します。
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – 常時ロケーション サービスを使用します。
- iOS 10 およびそれ以前
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) -アプリを使用するときに、ロケーション サービスを使用します。
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – 常時ロケーション サービスを使用します。    
    
11 およびそれ以前の iOS をサポートするには、3 つのキーを含めることができます: `NSLocationWhenInUseUsageDescription`、 `NSLocationAlwaysAndWhenInUseUsageDescription`、および`NSLocationAlwaysUsageDescription`です。

これらのキーで XML 形式を**Info.plist**を次に示します。 更新する必要があります、`string`アプリで場所情報を使用する方法を反映するように値。

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

**Info.plist**でエントリを追加することも**ソース**ビューの編集中に、 **Info.plist**ファイル。

![IOS 8 の Info.plist](map-images/ios8-map-permissions.png "iOS 8 のために必要な Info.plist エントリ")


### <a name="android"></a>Android

使用する、 [Google Maps API v2](https://developers.google.com/maps/documentation/android/) Android で API キーを生成して、Android のプロジェクトに追加する必要があります。
指示に従って、Xamarin のドキュメント[Google Maps API v2 キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)です。
これらの手順に従うと後で API キーを貼り付けます、 **Properties/AndroidManifest.xml**ファイル (ソースの表示と、次の要素を検索/更新)。

```xml
<meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="YOUR_API_KEY"/>
```

有効な API キーがない場合は、マップ コントロールが Android 上の灰色のボックスとして表示されます。

> [!NOTE]
> アップロードされた任意のアプリケーションのリリース バージョンの署名に使用されるキーストア ファイルを使用して別のキーを生成する、Google Play ストアにします。 開発のキーを生成して、デバッグは機能しません Google Play からダウンロードしたアプリはマップの表示できなくなっていません。 アプリの場合はキーを再生成することも忘れないで**パッケージ名**変更します。

Android プロジェクトを右クリックしを選択すると、適切なアクセス許可を有効にする必要もあります**オプション > ビルド > Android アプリケーション**し、次に時間刻み。

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

次のスクリーン ショットに、これらのいくつか示します。

![Android 用のアクセス許可を必要な](map-images/android-map-permissions.png "Android 用の必要なアクセス許可")

アプリケーションにマップ データをダウンロードするネットワーク接続が必要なために、最後の 2 つが必要です。 Android についてお読み[権限](http://developer.android.com/reference/android/Manifest.permission.html)詳細についてはします。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォームでマップを使用するには、承認トークンを生成する必要があります。 詳細については、次を参照してください。[マップ認証キーを要求](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx)msdn です。

認証トークンを指定する必要があります、`FormsMaps.Init("AUTHORIZATION_TOKEN")`メソッドの呼び出し、Bing マップでアプリを認証します。

<a name="Using_Maps" />

## <a name="using-maps"></a>マップを使用します。

参照してください、 [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) MobileCRM サンプル コードでマップ コントロールを使用する方法の例についてはします。 単純な`MapPage`クラス - この通知のようになりますを新しい`MapSpan`マップのビューを配置するために作成は。

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

設定してマップ コンテンツを変更することも、`MapType`プロパティ、標準の道路地図 (既定)、サテライト画像または両方の組み合わせを表示します。

```csharp
map.MapType == MapType.Street;
```

有効な`MapType`値は。

-  ハイブリッド
-  サテライト
-  番地 (既定)


### <a name="map-region-and-mapspan"></a>マップ領域および MapSpan

上記のコード スニペットに示すように、次のように指定すること、`MapSpan`マップ コンス トラクターへのインスタンスが初期表示を設定 (ポイントを中心し、ズーム レベル) が読み込まれると、マップのです。 `MoveToRegion`マップの位置やズーム レベルを変更する、マップ クラスのメソッドを使用することができます。 新しいを作成する 2 つの方法がある`MapSpan`インスタンス。

-  **MapSpan.FromCenterAndRadius()** -からのスパンを作成する静的メソッド、`Position`を指定して、`Distance`です。
-  **新しい MapSpan ()** -コンス トラクターを使用する、`Position`と degress の緯度と経度を表示します。


場所を変更することがなく、マップのズーム レベルを変更するには、新規作成`MapSpan`から現在の場所を使用して、`VisibleRegion.Center`マップ コントロールのプロパティです。 A `Slider` (ただし、マップ コントロールに直接ズーム スライダーの値を更新できません現在) は、次のようにマップのズームを制御する可能性があります。

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![使って、ズーム マップ](map-images/maps-zoom-sml.png "マップ コントロールを拡大表示")](map-images/maps-zoom.png#lightbox "マップ コントロールを拡大表示")

### <a name="map-pins"></a>マップのピン

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

 `PinType` (プラットフォームによっては、) の暗証番号 (pin) の表示方法に影響を与える可能性があります、次の値のいずれかに設定できます。

-  ジェネリック
-  場所
-  SavedPin
-  検索


<a name="Using_Xaml" />

## <a name="using-xaml"></a>Xaml を使用してください。

このスニペットで示すように、Xaml のレイアウトでマップを配置もできます。

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

`MapRegion`と`Pins`を使用してコードで設定することができます、`MyMap`参照 (または、マップの名前は任意)。 なお、追加`xmlns`Xamarin.Forms.Maps コントロールを参照する名前空間の定義が必要です。

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>まとめ

Xamarin.Forms.Maps は、個別の NuGet Xamarin.Forms ソリューション内の各プロジェクトに追加する必要があります。 追加の初期化コードは、iOS、Android、および UWP もいくつかの構成手順として必要です。

一度設定したコードのはわずか数行で暗証番号 (pin)、マーカー付きマップを表示するために、マップの API を使用できます。 マップをさらに向上することができます、[カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)です。


## <a name="related-links"></a>関連リンク

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [カスタム レンダラーをマップします。](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms のサンプル](https://developer.xamarin.com/samples/xamarin-forms/all/)
