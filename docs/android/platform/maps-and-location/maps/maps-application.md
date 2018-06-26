---
title: マップのアプリケーションの起動
description: Xamarin.Android アプリから組み込みマップのアプリケーションを起動する方法。
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: d15b6e544f58f03272c711236b579ca568e09539
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935023"
---
# <a name="launching-the-maps-application"></a>マップのアプリケーションの起動

Xamarin.Android 内の対応付けを使用する最も簡単な方法では、次に示す組み込みマップ アプリケーションを活用します。

[![Google Maps の組み込みアプリのサンプルのスクリーン ショット](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

マップのアプリケーションを使用するときに、マップには、アプリケーションの一部はできません。 代わりに、アプリケーションでは、マップのアプリケーションを起動して、外部でマップを読み込めません。 次のセクションでは、上記のようなマップを起動する Xamarin.Android を使用する方法を調べます。


## <a name="creating-the-intent"></a>目的の作成

アプリケーションが適切な URI を持つインテントを作成し、ActionView にアクションを設定し、StartActivity メソッドを呼び出すことと同じくらい簡単では、マップを使用します。 たとえば、次のコードは、特定の緯度と経度を中心とマップ アプリケーションを起動します。

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

このコードは、前のスクリーン ショットに示すように、マップを起動するために必要なすべてです。 緯度と経度を指定すると、他は、マップの URI スキームは、その他のいくつかのオプションをサポートします。


## <a name="geo-uri-scheme"></a>Geo URI スキーム

上記のコードは、URI を作成するのに geo スキームを使用します。 この URI スキームは、次に示すように、いくつかの形式をサポートします。

-   `geo:latitude,longitude` &ndash; Lat/lon を中心とマップ アプリケーションが開きます。 

-   `geo:latitude,longitude?z=zoom` &ndash; アプリケーションが lat/lon を中心し、指定されたレベルにズーム イン マップを開きます。 ズーム レベル範囲は 1 ~ 23: 1 のディスプレイ全体の地球、23、最も近いズーム レベル。

-   `geo:0,0?q=my+street+address` &ndash; 住所の場所にマップ アプリケーションが開きます。 

-   `geo:0,0?q=business+near+city` &ndash; マップのアプリケーションを開き、注釈付きの検索結果を表示します。 


URI のクエリ (つまり、番地アドレスや検索条件) を受け取るバージョンでは、Google の geocoder サービスを使用して、マップに表示される場所を取得します。 たとえば、URI`geo:0,0?q=coop+Cambridge`下図のマップにおいて。

[![検索語句で Google Maps を示すサンプルのスクリーン ショット](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)



Geo の URI スキームの詳細については、次を参照してください。[マップ上の場所を表示する](http://developer.android.com/guide/components/intents-common.html#Maps)です。


## <a name="street-view"></a>番地ビュー

Geo スキームだけでなく、インテントから street ビューの読み込みを Android もサポートします。 Xamarin.Android から起動する street ビュー アプリケーションの例を次に示します。

[![番地ビューの例のスクリーン ショット](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

Street ビューを起動するだけで使用、 `google.streetview` URI スキームは、次のコードに示すようにします。

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

上記で使用 google.streetview URI スキームは、次の形式になります。

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

わかります以下に示すとおりにサポートされている、いくつかのパラメーターにはあります。

-   `lat` &ndash; 住所のビューに表示される場所の緯度。

-   `lng` &ndash; 住所のビューに表示される場所の経度です。

-   `pitch` &ndash; 番地ビュー パノラマ、ここで 90 ° はダウン直線度および向きで中心からの角度は、まっすぐです。

-   `yaw` &ndash; 番地ビュー パノラマの中心はビューは、北部から角度の時計回りに測定しました。

-   `zoom` &ndash; 番地ビュー パノラマの乗数をズームすると、1.0 が標準の拡大または縮小、2.0 拡大 2 = 3.0 x 拡大 4 を = x, などです。

-   `mz` &ndash; 住所のビューからマップ アプリケーションに移動したときに使用されるマップのズーム レベル。


組み込みの作業は、アプリケーションをマップまたは street ビューはすばやくマッピングのサポートを追加する簡単な方法です。 ただし、Android のマップの API は、マッピングのエクスペリエンスをより細かく制御を提供します。
