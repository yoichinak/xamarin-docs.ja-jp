---
title: Maps アプリケーションの起動
description: Xamarin Android アプリ内から組み込みの Maps アプリケーションを起動する方法。
ms.prod: xamarin
ms.assetid: 929EACB8-8950-50E1-093C-43FB5F1F1CD5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/25/2018
ms.openlocfilehash: 7b74f564f2b6e9613874a774258a7e999002e61a
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027076"
---
# <a name="launching-the-maps-application"></a>Maps アプリケーションの起動

Xamarin.Android でマップを操作する最も簡単な方法は、次に示す組み込みの Maps アプリケーションを利用することです。

[![組み込みの Google Maps アプリのスクリーンショットの例](maps-application-images/01-mapsapplication.png)](maps-application-images/01-mapsapplication.png#lightbox)

Maps アプリケーションを使用すると、マップはアプリケーションの一部になりません。 代わりに、アプリケーションで Maps アプリケーションを起動し、外部でマップを読み込みます。 次のセクションでは、Xamarin.Android を使用して、上記のようなマップを起動する方法を説明します。

## <a name="creating-the-intent"></a>意図の作成

Maps アプリケーションの使用は、適切な URI で意図を作成し、アクションを ActionView に設定して、StartActivity メソッドを呼び出すだけの、簡単なものです。 たとえば、次のコードでは、指定した緯度と経度を中心として Maps アプリケーションが起動されます。

```csharp
var geoUri = Android.Net.Uri.Parse ("geo:42.374260,-71.120824");
var mapIntent = new Intent (Intent.ActionView, geoUri);
StartActivity (mapIntent);
```

前のスクリーンショットで示されているマップを起動するために必要なコードはこれだけです。 緯度と経度を指定するだけでなく、Maps の URI スキームでは、他にもいくつかのオプションがサポートされています。

## <a name="geo-uri-scheme"></a>geo URI スキーム

上記のコードでは、geo スキームを使用して URI を作成しています。 この URI スキームでは、次のようないくつかの形式がサポートされています。

- `geo:latitude,longitude` &ndash; 緯度と経度を中央にして Maps アプリケーションを開きます。 

- `geo:latitude,longitude?z=zoom` &ndash; 緯度と経度を中央にして、指定したズーム レベルで、Maps アプリケーションを開きます。 ズーム レベルは、1 から 23 の範囲で指定できます。1 は地球全体を表示し、23 は最も拡大されたズーム レベルです。

- `geo:0,0?q=my+street+address` &ndash; 番地の場所で Maps アプリケーションを開きます。 

- `geo:0,0?q=business+near+city` &ndash; Maps アプリケーションを開き、注釈付きの検索結果を表示します。 

クエリ (つまり、番地または検索語) を受け取る URI のバージョンでは、Google のジオコーダー サービスを使用して、マップに表示される場所を取得します。 たとえば、URI `geo:0,0?q=coop+Cambridge` では、次のようなマップが表示されます。

[![検索用語を含む Google Maps を示すスクリーンショットの例](maps-application-images/02-mapsearch.png)](maps-application-images/02-mapsearch.png#lightbox)

geo URI スキームの詳細については、「[地図上の場所を表示する](https://developer.android.com/guide/components/intents-common.html#Maps)」を参照してください。

## <a name="street-view"></a>ストリート ビュー

geo スキームに加えて、Android では意図からのストリート ビューの読み込みもサポートされています。 Xamarin.Android から起動されたストリート ビュー アプリケーションの例を次に示します。

[![ストリート ビューのスクリーンショットの例](maps-application-images/03-streetview.png)](maps-application-images/03-streetview.png#lightbox)

ストリート ビューを起動するには、次のコードに示すように、単に `google.streetview` URI スキームを使用します。

```csharp
var streetViewUri = Android.Net.Uri.Parse (
       "google.streetview:cbll=42.374260,-71.120824&cbp=1,90,,0,1.0&mz=20");  
var streetViewIntent = new Intent (Intent.ActionView, streetViewUri);  
StartActivity (streetViewIntent);
```

上で使用した google.streetview URI スキームでは、次の形式が使用されます。

```csharp
google.streetview:cbll=lat,lng&cbp=1,yaw,,pitch,zoom&mz=mapZoom
```

ご覧のように、次に示すいくつかのパラメーターがサポートされています。

- `lat` &ndash; ストリート ビューで表示する場所の緯度。

- `lng` &ndash; ストリート ビューで表示する場所の経度。

- `pitch` &ndash; ストリート ビュー パノラマの角度。中心からの角度で測定され、90 度は真下、-90 度は真上になります。

- `yaw` &ndash; ストリート ビュー パノラマのビューの中心。北から時計回りに角度で測定されます。

- `zoom` &ndash; ストリート ビュー パノラマのズーム乗数 (1.0 = 通常のズーム、2.0 = 2 倍ズーム、3.0 = 4 倍ズームなど)。

- `mz` &ndash; ストリート ビューから Maps アプリケーションに移動するときに使用されるマップのズーム レベル。

組み込みの Maps アプリケーションまたはストリート ビューを使用すると、マッピングのサポートを簡単に追加することができます。 ただし、Android の Maps API では、マッピング エクスペリエンスをさらに細かく制御できます。
