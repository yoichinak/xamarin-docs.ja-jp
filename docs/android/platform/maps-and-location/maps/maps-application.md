---
title: マップ アプリケーションの起動
description: Xamarin.Android アプリ内から組み込みの地図アプリケーションを起動する方法。
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: cd80154602cc22668768fe217da7371b77ded003
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112384"
---
# <a name="launching-the-maps-application"></a>マップ アプリケーションの起動

Xamarin.Android でマップを使用する最も簡単な方法は、次に示す組み込みの地図アプリケーションで利用するのには。

[![Google Maps の組み込みアプリのスクリーン ショットの例](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

マップ アプリケーションを使用すると、マップには、アプリケーションの一部はできません。 代わりに、アプリケーションは、マップ アプリを起動し、外部でマップを読み込みます。 次のセクションでは、Xamarin.Android を使用して、上記のようなマップを起動する方法を調べます。


## <a name="creating-the-intent"></a>インテントを作成します。

アプリケーションは、マップの操作は、適切な URI を持つ、インテントを作成し、ActionView にアクションを設定し、StartActivity メソッドを呼び出すことと同じくらい簡単です。 たとえば、次のコードは、特定の緯度と経度を中心とマップ アプリケーションを起動します。

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

このコードは、すべての起動前のスクリーン ショットに示すようにマップするために必要です。 緯度と経度を指定するだけでなく、マップの URI スキームはその他のいくつかのオプションをサポートします。


## <a name="geo-uri-scheme"></a>Geo の URI スキーム

上記のコードでは、URI の作成に geo スキームを使用します。 この URI スキームは、次に示すように、いくつかの形式をサポートします。

-   `geo:latitude,longitude` &ndash; 中央に緯度/経度、マップ アプリを開きます。 

-   `geo:latitude,longitude?z=zoom` &ndash; アプリケーションは、緯度/経度の中心し、指定されたレベルに拡大されているマップを開きます。 ズーム レベルの範囲は 1 から 23: 1 の表示全体の地球と 23 は最も近いズーム レベル。

-   `geo:0,0?q=my+street+address` &ndash; 住所の場所に、マップ アプリを開きます。 

-   `geo:0,0?q=business+near+city` &ndash; マップ アプリケーションを開き、注釈付きの検索結果を表示します。 


クエリ (つまり、ストリート アドレスまたは検索条件) を取得する URI のバージョンでは、Google の geocoder サービスを使用して、マップに表示される場所を取得します。 たとえば、URI`geo:0,0?q=coop+Cambridge`結果、次に示すマップ。

[![検索用語に Google マップを示す例のスクリーン ショット](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)



Geo の URI スキームの詳細については、次を参照してください。[マップ上の場所を表示する](http://developer.android.com/guide/components/intents-common.html#Maps)します。


## <a name="street-view"></a>番地のビュー

Geo スキームに加えて、インテントから番地ビューの読み込みを Android もサポートします。 Xamarin.Android から起動する番地ビュー アプリケーションの例は、以下に示します。

[![番地のビューのスクリーン ショットの例](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

番地のビューを起動するを使用して単に、 `google.streetview` URI スキームを次のコードで示した。

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

上記で使用 google.streetview URI スキームでは、次の形式をとります。

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

ご覧のように、次に示すように、サポートされているいくつかのパラメーターにがあります。

-   `lat` &ndash; 番地のビューに表示される場所の緯度。

-   `lng` &ndash; 番地のビューに表示される位置の経度。

-   `pitch` &ndash; 番地ビュー パノラマ、角度が 90 度がダウン直線および-90 度で、中心から測定の角度は、まっすぐです。

-   `yaw` &ndash; 番地のビューのパノラマのセンターはビューは、北米からの角度で時計回りに測定しました。

-   `zoom` &ndash; 番地ビュー パノラマの乗数のズーム、1.0、2.0、通常のズームを = = 拡大 2 3.0 x 拡大 4 = x, などです。

-   `mz` &ndash; 番地のビューから、マップ アプリに移動したときに使用されるマップのズーム レベル。


組み込みの操作はアプリケーションを割り当てたり、ストリート ビューはすばやくマッピングのサポートを追加する簡単な方法です。 ただし、Android のマップの API は、マッピングのエクスペリエンスをより細かく制御を提供します。
