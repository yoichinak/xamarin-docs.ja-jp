---
title: Xamarin. フォームマップの初期化と構成
description: この記事では、Xamarin. Forms Map クラスを使用して、各プラットフォームでネイティブマップ Api を使用して、ユーザーに使い慣れたマップエクスペリエンスを提供する方法について説明します。
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/07/2019
ms.openlocfilehash: d9b1b93b0667415281bb04e2c4264f03be19bd83
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697711"
---
# <a name="xamarinforms-map-initialization-and-configuration"></a>Xamarin. フォームマップの初期化と構成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

_Xamarin は、各プラットフォームでネイティブマップ Api を使用します。_

Xamarin は、各プラットフォームでネイティブマップ Api を使用します。 これにより、ユーザーのための高速で使い慣れたマップエクスペリエンスが提供されますが、各プラットフォーム API の要件に準拠するためにはいくつかの構成手順が必要になります。
構成が完了すると、`Map` コントロールは、共通コードの他の Xamarin. Forms 要素と同じように動作します。

マップコントロールは、次に示す map[サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)サンプルで使用されています。

 [![MobileCRM サンプルのマップ](map-images/maps-zoom-sml.png "マップコントロールの例")](map-images/maps-zoom.png#lightbox "マップコントロールの例")

マップの[カスタムレンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)を作成することにより、マップの機能をさらに拡張できます。

<a name="Maps_Initialization" />

## <a name="map-initialization"></a>マップの初期化

Xamarin. フォームアプリケーションにマップを追加すると **、別**の NuGet パッケージとして、ソリューション内のすべてのプロジェクトに追加する必要があります。
Android では、これも GooglePlayServices (別の NuGet) に依存しています。これは、Xamarin を追加すると自動的にダウンロードされます。

NuGet パッケージをインストールした後、`Xamarin.Forms.Forms.Init` メソッドの呼び出し*後*に、各アプリケーションプロジェクトでいくつかの初期化コードが必要になります。 IOS の場合は、次のコードを使用します。

```csharp
Xamarin.FormsMaps.Init();
```

Android では、`Forms.Init` と同じパラメーターを渡す必要があります。

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

ユニバーサル Windows プラットフォーム (UWP) の場合は、次のコードを使用します。

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

プラットフォームごとに次のファイルにこの呼び出しを追加します。

- `FinishedLaunching` メソッドの**iOS** -AppDelegate.cs file。
- @No__t_1 メソッドの**Android** -MainActivity.cs ファイル。
- @No__t_1 コンストラクター内の MainPage.xaml.cs ファイル。

NuGet パッケージが追加され、各アプリケーション内部で初期化メソッドが呼び出されると、共通 .NET Standard ライブラリプロジェクトまたは共有プロジェクトコードで `Xamarin.Forms.Maps` Api を使用できるようになります。

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>プラットフォームの構成

一部のプラットフォームでは、マップを表示する前に追加の構成手順が必要です。

### <a name="ios"></a>iOS

IOS 上のロケーションサービスにアクセスするには、**情報 plist**で次のキーを設定する必要があります。

- iOS 11
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) –アプリを使用しているときにロケーションサービスを使用する場合
  - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) –位置情報サービスを常に使用する場合
- iOS 10 以前
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) –アプリを使用しているときにロケーションサービスを使用する場合
  - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) –位置情報サービスを常に使用する場合    

IOS 11 以前をサポートするには、`NSLocationWhenInUseUsageDescription`、`NSLocationAlwaysAndWhenInUseUsageDescription`、`NSLocationAlwaysUsageDescription` の3つのキーすべてを含めることができます。

次に、これらのキーの XML 表現を**情報 plist**で示します。 アプリケーションが場所情報をどのように使用しているかを反映するように、`string` の値を更新する必要があります。

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

また **、情報の plist ファイルを**編集しているときに、**ソース**ビューで**情報**を追加することもできます。

![IOS 8 用情報 plist](map-images/ios8-map-permissions.png "iOS 8 必須情報. plist エントリ")

### <a name="android"></a>Android

Android で[Google MAPS api v2](https://developers.google.com/maps/documentation/android/)を使用するには、api キーを生成して android プロジェクトに追加する必要があります。
[Google MAPS API v2 キーを取得する](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)方法については、Xamarin ドキュメントの手順に従ってください。
これらの手順に従った後、API キーを**Properties/AndroidManifest**ファイルに貼り付けます (ソースを表示し、次の要素を検索/更新します)。

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

有効な API キーがない場合、maps コントロールは Android で灰色のボックスとして表示されます。

> [!NOTE]
> APK が Google Maps にアクセスできるようにするには、APK に署名するために使用するすべてのキーストア (デバッグとリリース) に SHA-1 指紋とパッケージ名を含める必要があることに注意してください。 たとえば、デバッグに1台のコンピューターを使用し、リリース APK を生成する別のコンピューターを使用する場合は、最初のコンピューターのデバッグキーストアから SHA-1 証明書のフィンガープリントを指定し、次のリリースキーストアから SHA-1 証明書のフィンガープリントを含める必要があります。2番目のコンピューター。 また、アプリの**パッケージ名**が変更された場合は、キーの資格情報を編集することも忘れないでください。 「 [Google MAPS API v2 キーの取得」を](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)参照してください。

また、適切なアクセス許可を有効にする必要があります。そのためには、Android プロジェクトを右クリックし、[オプション] を選択して **> > Android アプリケーションをビルド**し、次のように設定します。

- `AccessCoarseLocation`
- `AccessFineLocation`
- `AccessLocationExtraCommands`
- `AccessMockLocation`
- `AccessNetworkState`
- `AccessWifiState`
- `Internet`

これらの一部を次のスクリーンショットに示します。

![Android に必要なアクセス許可](map-images/android-map-permissions.png "Android に必要なアクセス許可")

最後の2つは、マップデータをダウンロードするためにアプリケーションがネットワーク接続を必要とするために必要です。 詳細については、Android の[アクセス許可](https://developer.android.com/reference/android/Manifest.permission.html)に関するページを参照してください。

さらに、Android 9 は bootclasspath から Apache HTTP クライアントライブラリを削除したため、API 28 以上を対象とするアプリケーションでは使用できません。 API 28 以降を対象とするアプリケーションで Apache HTTP クライアントを引き続き使用するには、次の行を**Androidmanifest .xml**ファイルの `application` ノードに追加する必要があります。

```xml
<application ...>
    ...
    <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォームでマップを使用するには、認証トークンを生成する必要があります。 詳細については、MSDN の「 [Request a maps authentication key](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) 」を参照してください。

その後、`FormsMaps.Init("AUTHORIZATION_TOKEN")` メソッドの呼び出しで認証トークンを指定し、Bing Maps でアプリを認証する必要があります。

<a name="Using_Maps" />

## <a name="map-configuration"></a>マップの構成

コードでマップコントロールを使用する方法の例については、MobileCRM サンプルの[MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs)を参照してください。 単純な `MapPage` クラスは次のようになります。新しい `MapSpan` が作成され、マップのビューを配置します。

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

マップコンテンツは、`MapType` プロパティを設定して、通常の道路地図 (既定)、衛星画像、またはその両方の組み合わせを表示することによって変更することもできます。

```csharp
map.MapType = MapType.Street;
```

有効な `MapType` 値は次のとおりです。

- `Hybrid`
- `Satellite`
- `Street` (既定値)

### <a name="map-region-and-mapspan"></a>マップ領域と MapSpan

上記のコードスニペットに示されているように、マップコンストラクターに `MapSpan` インスタンスを指定すると、マップの読み込み時にマップの初期ビュー (中心点とズームレベル) が設定されます。 新しい `MapSpan` インスタンスを作成するには、次の2つの方法があります。

- **Mapspan. FromCenterAndRadius ()** -静的メソッド。 `Position` からスパンを作成し、`Distance` を指定します。
- **新しい MapSpan ()** -`Position` と、表示する緯度と経度の角度を使用するコンストラクター。

マップクラスの `MoveToRegion` メソッドを使用して、マップの位置またはズームレベルを変更できます。 位置を変更せずにマップのズームレベルを変更するには、マップコントロールの `VisibleRegion.Center` プロパティの現在の場所を使用して、新しい `MapSpan` を作成します。 @No__t_0 を使用すると、次のようにマップのズームを制御できます (ただし、マップコントロールに直接ズームすると、スライダーの値を更新することはできません)。

```csharp
Slider slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) =>
{
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

[![ズーム付きのマップ](map-images/maps-zoom-sml.png "マップコントロールズーム")](map-images/maps-zoom.png#lightbox "マップコントロールズーム")

さらに、 [`Map`](xref:Xamarin.Forms.Maps.Map)クラスには、バインド可能なプロパティによってサポートされる型 `bool` の `MoveToLastRegionOnLayoutChange` プロパティがあります。 既定では、このプロパティは `true` です。これは、デバイスの回転など、レイアウトの変更が発生したときに、表示されているマップ領域が現在の領域から以前に設定された領域に移動することを示します。 このプロパティが `false` に設定されている場合、レイアウトの変更が発生しても、表示されているマップ領域は中央のままになります。 次の例は、このプロパティを設定する方法を示しています。

```csharp
map.MoveToLastRegionOnLayoutChange = false;
```

### <a name="map-clicks"></a>マップのクリック

`Map` は、マップがタップされたときに発生する `MapClicked` イベントを定義します。 @No__t_1 イベントに付随する `MapClickedEventArgs` オブジェクトには、`Position` 型の `Position` という名前のプロパティが1つあります。 イベントが発生すると、`Position` プロパティの値が、タップされたマップの場所に設定されます。

次のコード例は、`MapClicked` イベントのイベントハンドラーを示しています。

```csharp
map.MapClicked += OnMapClicked;

void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

この例では、`OnMapClicked` イベントハンドラーが、タップされたマップの場所を表す緯度と経度を出力します。

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
> 追加の `xmlns` 名前空間の定義は、Xamarin. Forms. マップコントロールを参照するために必要です。 前の例では、`Xamarin.Forms.Maps` 名前空間が `maps` キーワードを通じて参照されています。

@No__t_0 は、`Map` の名前付き参照を使用してコードで設定できます。

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="related-links"></a>関連リンク

- [Maps サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin. Forms. マップの pin](~/xamarin-forms/user-interface/map/pins.md)。
- [Maps API](xref:Xamarin.Forms.Maps)
- [カスタムレンダラーのマップ](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
