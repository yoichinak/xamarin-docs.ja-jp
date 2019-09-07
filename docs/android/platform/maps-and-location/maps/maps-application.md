---
title: Maps アプリケーションの起動
description: Xamarin Android アプリ内から組み込みの Maps アプリケーションを起動する方法。
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: b950326eb5a124d5040caa0044309630a2a53d38
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761672"
---
# <a name="launching-the-maps-application"></a>Maps アプリケーションの起動

Xamarin でマップを操作する最も簡単な方法は、次に示す組み込みの maps アプリケーションを利用することです。

[![組み込みの Google Maps アプリのスクリーンショットの例](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

Maps アプリケーションを使用する場合、マップはアプリケーションの一部にはなりません。 代わりに、アプリケーションは maps アプリケーションを起動し、外部にマップを読み込みます。 次のセクションでは、Xamarin を使用して、上記のようなマップを起動する方法を確認します。

## <a name="creating-the-intent"></a>インテントの作成

Maps アプリケーションでの作業は、適切な URI を使用した意図の作成、アクションの ActionView への設定、および StartActivity メソッドの呼び出しと同じように簡単です。 たとえば、次のコードでは、特定の緯度と経度の中央にある maps アプリケーションを起動します。

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

このコードは、前のスクリーンショットに示されているマップを起動するために必要なすべてのコードです。 緯度と経度を指定するだけでなく、maps の URI スキームでは、他のいくつかのオプションがサポートされています。

## <a name="geo-uri-scheme"></a>Geo URI スキーム

上記のコードでは、geo スキームを使用して URI を作成しています。 この URI スキームでは、次のようないくつかの形式がサポートされています。

- `geo:latitude,longitude`&ndash;マップアプリケーションを lat/lon の中央に開きます。 

- `geo:latitude,longitude?z=zoom`&ndash;マップアプリケーションを lat/lon で中央に開き、指定されたレベルに拡大します。 ズームレベルは、1 ~ 23 の範囲で指定できます。1地球全体を表示し、23は最も近いズームレベルを示します。

- `geo:0,0?q=my+street+address`&ndash;地図アプリケーションを住所の場所に対して開きます。 

- `geo:0,0?q=business+near+city`&ndash; Maps アプリケーションを開き、注釈付き検索結果を表示します。 

クエリを受け取る URI のバージョン (つまり、番地または検索語) は、Google の geocoder サービスを使用して、マップに表示される場所を取得します。 たとえば、URI `geo:0,0?q=coop+Cambridge`は次のようなマップになります。

[![検索用語を含む Google マップを示すスクリーンショットの例](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)

Geo URI スキームの詳細については、「[マップ上の場所の表示](https://developer.android.com/guide/components/intents-common.html#Maps)」を参照してください。

## <a name="street-view"></a>番地ビュー

Geo スキームに加えて、Android では目的からのストリートビューの読み込みもサポートされています。 Xamarin. Android から起動したストリートビューアプリケーションの例を次に示します。

[![ストリートビューのスクリーンショットの例](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

ストリートビューを起動するには、次`google.streetview`のコードに示すように、単に URI スキームを使用します。

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

上記で使用した streetview URI スキームは、次の形式になります。

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

ご覧のように、次に示すように、いくつかのパラメーターがサポートされています。

- `lat`&ndash;番地ビューに表示される場所の緯度。

- `lng`&ndash;番地ビューに表示される位置の経度。

- `pitch`&ndash;ストリートビューパノラマの角度。この角度は、中心から、90°が垂直方向で、-90 °がまっすぐになります。

- `yaw`&ndash;北から時計回りに測定した、ストリートビューパノラマのビューの中心。

- `zoom`&ndash;ストリートビューパノラマのズーム乗数 (1.0 = 通常のズーム、2.0 = 拡大、3.0、拡大した4倍など)。

- `mz`&ndash;ストリートビューから maps アプリケーションに移動するときに使用されるマップのズームレベル。

組み込みの maps アプリケーションまたは [番地] ビューを使用すると、簡単にマッピングサポートを追加することができます。 ただし、Android の Maps API では、マッピングエクスペリエンスをより細かく制御できます。
