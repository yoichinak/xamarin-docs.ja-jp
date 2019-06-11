---
title: 位置情報サービス
description: このガイドでは、Android アプリケーションの場所の認識を紹介し、Google の場所のサービス API で使用可能な場所の融合型プロバイダーと同様に、Android の場所のサービスの API を使用して、ユーザーの場所を取得する方法を示しています。
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/22/2018
ms.openlocfilehash: 76f98f1e660f22ec25c48407f2e87cec60ff12ef
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827687"
---
# <a name="location-services"></a>位置情報サービス

_このガイドでは、Android アプリケーションの場所の認識を紹介し、Google の場所のサービス API で使用可能な場所の融合型プロバイダーと同様に、Android の場所のサービスの API を使用して、ユーザーの場所を取得する方法を示しています。_

## <a name="location-services-overview"></a>サービスの場所の概要

Android では、セル tower 場所、Wi-fi、GPS などのさまざまな場所テクノロジへのアクセスを提供します。 各場所のテクノロジの詳細が抽象化される*場所プロバイダー*をアプリケーションで使用されるプロバイダーに関係なく同じ方法での場所を取得することができます。 このガイドでは、Google play 開発者サービス、インテリジェントにどのようなプロバイダーが追加され、デバイスの使用方法に基づいてデバイスの場所を取得する最善の方法を決定するの一部を融合型場所プロバイダーが導入されています。 Android の場所のサービス API し、システムの場所と通信する方法を示しています。 サービスを使用して、`LocationManager`します。 このガイドの 2 番目の部分では、Android の場所のサービス API を使用して、について説明します、`LocationManager`します。
 
一般則としてアプリケーションはフォールバックして、古い Android 場所サービス API に必要な場合にのみ、場所の融合型プロバイダーを使用する優先する必要があります。

## <a name="location-fundamentals"></a>場所の基礎

場所のデータを操作するために選択するどのような API に関係なく、Android では、いくつかの概念が同じになります。 このセクションでは、場所プロバイダーと場所に関連する権限について説明します。

### <a name="location-providers"></a>場所プロバイダー

いくつかのテクノロジは、ユーザーの場所を正確に内部的に使用されます。 使用されるハードウェアの種類によって異なります*場所プロバイダー*のデータ収集ジョブに選択します。 Android では、次の 3 つの場所プロバイダーを使用します。

-   **GPS プロバイダー** &ndash; GPS 最も正確な場所は、最大限の電源を使用して、屋外で最適に動作します。 このプロバイダーは、GPS とアシスト型 GPS の組み合わせを使用して ([aGPS](https://en.wikipedia.org/wiki/Assisted_GPS))、携帯電話の塔によって収集された GPS データが返されます。

-   **ネットワーク プロバイダー** &ndash;セル towers によって収集された aGPS データを含む、WiFi および携帯電話のデータの組み合わせを提供します。 GPS プロバイダーよりも少ない電力を使用しているが、さまざまな精度の場所データを返します。

-   **パッシブ プロバイダー** &ndash;他のアプリケーションまたはサービスによって要求されたプロバイダーを使用して、アプリケーションで場所データを生成する「ピギーバック」オプション。 これは、定数の場所の更新プログラムが機能を必要としないアプリケーション オプションの省電力に最適ですが、小さい信頼性が高くします。

場所プロバイダーは、常に使用できません。 などのアプリケーションでは、GPS を使用する可能性がまたはの設定で、GPS をオフにすることがありますが、デバイスの GPS をまったくがない可能性があります。 特定のプロバイダーが使用できない場合は、そのプロバイダーが返す可能性がありますを選択`null`します。

### <a name="location-permissions"></a>場所のアクセス許可

位置認識アプリケーションでは、GPS、Wi-fi、および携帯電話のデータを受信するデバイスのハードウェア センサーへのアクセスが必要です。 アクセスは、アプリケーションの Android マニフェストで適切なアクセス許可によって制御されます。
2 つのアクセス許可がある使用可能な&ndash;をアプリケーションの要件と API の好みに応じてができるようにする 1 つ。

-   `ACCESS_FINE_LOCATION` &ndash; GPS をアプリケーションのアクセスを許可します。
    必要な*GPS プロバイダー*と*パッシブ プロバイダー*オプション (*パッシブ プロバイダーには、別のアプリケーションまたはサービスによって収集された GPS データにアクセスする許可が必要です。* )。 省略可能なアクセス許可、*ネットワーク プロバイダー*します。

-   `ACCESS_COARSE_LOCATION` &ndash; 携帯電話と Wi-fi の場所にアプリケーションのアクセスを許可します。 必要な*ネットワーク プロバイダー*場合`ACCESS_FINE_LOCATION`が設定されていません。

API 21 (Android 5.0 Lollipop) のバージョンを対象とするアプリ以降では、有効にできますか`ACCESS_FINE_LOCATION`して GPS ハードウェアがないデバイス上でまだ実行します。 かどうか、アプリには、GPS ハードウェアが必要とする必要があります明示的に追加する、 `android.hardware.location.gps` `uses-feature` Android マニフェストに要素。 詳細については、Android を参照してください。[使用機能](https://developer.android.com/guide/topics/manifest/uses-feature-element.html)要素への参照。

アクセス許可を設定するには、展開、**プロパティ**フォルダーで、 **Solution Pad**  をダブルクリックします**AndroidManifest.xml**します。 アクセス許可が表示されます**必要なアクセス許可**:

[![Android マニフェストに必要なアクセス許可の設定のスクリーン ショット](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

これらのアクセス許可のいずれかの設定は Android アプリケーションでは、場所プロバイダーにアクセスするには、ユーザーからのアクセス許可が必要なします。 デバイスを API レベル 22 (Android 5.1) を実行または低く、アプリがインストールされるたびにこれらのアクセス許可を付与をユーザーに尋ねます。 API を実行するデバイスでは、レベル 23 (Android 6.0) または以降では、アプリが場所プロバイダーの要求を行う前に実行時のアクセス許可のチェックを実行する必要があります。 

> [!NOTE]
>メモ:設定`ACCESS_FINE_LOCATION`大まか場所データの両方にアクセスを意味します。 のみ両方の権限を設定することはありません必要、*最小限*アプリの動作に必要なアクセス許可。

このスニペットは、アプリにアクセス許可があることを確認する方法の例、`ACCESS_FINE_LOCATION`権限。

```csharp
 if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.AccessFineLocation) == Permission.Granted)
{
    StartRequestingLocationUpdates();
    isRequestingLocationUpdates = true;
}
else
{
    // The app does not have permission ACCESS_FINE_LOCATION 
}
```

アプリは、場所、ユーザー アクセス権が付与されます (または、アクセス許可が失効) シナリオに対応できるし、そのような状況に適切に対処する方法を持つ必要があります。 参照してください、[アクセス許可ガイド](~/android/app-fundamentals/permissions.md)実行時のアクセス許可の実装の詳細については、Xamarin.Android でチェックします。


## <a name="using-the-fused-location-provider"></a>場所の融合型プロバイダーを使用します。

融合型場所プロバイダーは、場所プロバイダー バッテリ効率の高い方法で最適な場所情報を提供する実行時に効率的に選択されますので、デバイスからの場所の更新プログラムを受信する Android アプリケーションのことをお勧めします。 たとえば、屋外を歩きながら、ユーザーは、gps を読み取る最適な場所を取得します。 ユーザーは、屋内でし場合、場所 GPS 機能が不十分な場合)、融合型場所プロバイダーは、WiFi、動作の向上屋内に自動的に切り替えることがあります。
 
融合型場所プロバイダー API は、さまざまな場所に対応するアプリケーション、geofencing 機能と動作の監視などを支援するその他のツールを提供します。 このセクションで行うフォーカスの設定の基本、`LocationClient`プロバイダーを確立し、ユーザーの位置を取得します。

場所の融合型プロバイダーの一部である[Google play 開発者サービス](https://developer.android.com/google/play-services/index.html)します。
Google play 開発者サービス パッケージをインストールして作業をする場所の融合型プロバイダーの API のアプリケーションで適切に構成する必要があり、デバイスには、Google Play Services APK インストールされている必要があります。

前に、Xamarin.Android アプリケーションは、場所の融合型プロバイダーを使用できますする必要があります追加、 **Xamarin.GooglePlayServices.Maps**をプロジェクトにパッケージします。 さらに、次`using`ステートメントは、以下で説明するクラスを参照するすべてのソース ファイルに追加する必要があります。

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Google play 開発者サービスがインストールされているかどうかは確認

クラッシュの場合は Google play 開発者サービスがインストールされていないときに、場所の融合型プロバイダーを使用する (または期限切れ) は、Xamarin.Android は、ランタイム例外が発生します。  Google play 開発者サービスがインストールされていない場合、アプリケーションする必要があります戻る Android のロケーション サービスが前述のようにします。 Google play 開発者サービスが最新でない場合は、アプリは Google play 開発者サービスのインストールされているバージョンを更新するかを確認するユーザーにメッセージを表示できます。

このスニペットでは、どの Android Activity をプログラムによってチェックできます Google play 開発者サービスがインストールされているかどうかの例を示します。

```csharp
bool IsGooglePlayServicesInstalled()
{
    var queryResult = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
    if (queryResult == ConnectionResult.Success)
    {
        Log.Info("MainActivity", "Google Play Services is installed on this device.");
        return true;
    }

    if (GoogleApiAvailability.Instance.IsUserResolvableError(queryResult))
    {
        // Check if there is a way the user can resolve the issue
        var errorString = GoogleApiAvailability.Instance.GetErrorString(queryResult);
        Log.Error("MainActivity", "There is a problem with Google Play Services on this device: {0} - {1}",
                  queryResult, errorString);
                  
        // Alternately, display the error to the user.
    }

    return false;
}
```

### <a name="fusedlocationproviderclient"></a>FusedLocationProviderClient

Xamarin.Android アプリケーションの場所の融合型プロバイダーをやり取りするのインスタンスをいる必要があります、`FusedLocationProviderClient`します。 このクラスは、位置情報の更新をサブスクライブして、デバイスの最後の既知の場所を取得するために必要なメソッドを公開します。

`OnCreate`アクティビティのメソッドは、適切な場所への参照を取得する、`FusedLocationProviderClient`次のコード スニペットに示すように、します。

```csharp
public class MainActivity: AppCompatActivity
{
    FusedLocationProviderClient fusedLocationProviderClient;

    protected override void OnCreate(Bundle bundle) 
    {
        fusedLocationProviderClient = LocationServices.GetFusedLocationProviderClient(this);
    }
}
```

### <a name="getting-the-last-known-location"></a>最後の既知の場所を取得します。

`FusedLocationProviderClient.GetLastLocationAsync()`メソッドは、最小限のコーディングのオーバーヘッドとデバイスの最後の既知の場所をすばやく取得する Xamarin.Android アプリケーションの非ブロッキングの単純な方法を提供します。

このスニペットを使用する方法を示します、`GetLastLocationAsync`デバイスの場所を取得します。

```csharp
async Task GetLastLocationFromDevice()
{
    // This method assumes that the necessary run-time permission checks have succeeded.
    getLastLocationButton.SetText(Resource.String.getting_last_location);
    Android.Locations.Location location = await fusedLocationProviderClient.GetLastLocationAsync();

    if (location == null)
    {
        // Seldom happens, but should code that handles this scenario
    }
    else
    {
        // Do something with the location 
        Log.Debug("Sample", "The latitude is " + location.Latitude);
    }
}
```

### <a name="subscribing-to-location-updates"></a>更新プログラムの場所にサブスクライブします。

Xamarin.Android アプリケーションも購読できる場所の更新プログラムを使用して融合型場所プロバイダーから、`FusedLocationProviderClient.RequestLocationUpdatesAsync`メソッドを次のコード スニペットに示すようにします。

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
このメソッドは、2 つのパラメーターを受け取ります。

-   **`Android.Gms.Location.LocationRequest`** &ndash; A`LocationRequest`オブジェクトが融合型場所プロバイダーの動作方法の Xamarin.Android アプリケーションがパラメーターを渡します。 `LocationRequest`情報を保持するこのような方法に頻繁に要求を行う必要がありますまたは正確な場所の更新プログラムが重要かあります。 たとえば、場所を決定するときに、GPS、したがって複数の電源を使用するデバイス、重要な場所の要求になります。 このコード スニペットを作成する方法を示しています、`LocationRequest`高精度での場所、場所の更新 (ただし要求間で 2 分よりも早くされません) の分 5 台に約をチェックします。 場所の融合型プロバイダーを使用して、`LocationRequest`しようとしたときに使用して、デバイスの場所を決定するには、どの場所プロバイダーのガイダンスとして。

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; 場所の更新プログラムを受信するには、Xamarin.Android アプリケーション サブクラスする必要があります、`LocationProvider`抽象クラス。 このクラスは、位置情報でアプリを更新する場所の融合型プロバイダーによって呼び出されるかもしれませんが 2 つのメソッドを公開します。 これについては、以下で詳しく説明します。

場所の融合型プロバイダーを呼び出す場所の更新の Xamarin.Android アプリケーションを通知する、`LocationCallBack.OnLocationResult(LocationResult result)`します。 `Android.Gms.Location.LocationResult`パラメーター更新場所情報が含まれます。

呼び出すことが融合型場所プロバイダーは、場所データの可用性の変更を検出すると、`LocationProvider.OnLocationAvailability(LocationAvailability
locationAvailability)`メソッド。 場合、`LocationAvailability.IsLocationAvailable`プロパティが返す`true`、によってデバイスの場所の結果が報告されたことが想定することができますし、`OnLocationResult`は正確でと、最新の状態によって必要に応じて、`LocationRequest`します。 場合`IsLocationAvailable`が false の場合、場所の結果はありませんで返される`OnLocationResult`します。

このコード スニペットは、サンプルの実装の`LocationCallback`オブジェクト。

```csharp
public class FusedLocationProviderCallback : LocationCallback
{
    readonly MainActivity activity;

    public FusedLocationProviderCallback(MainActivity activity)
    {
        this.activity = activity;
    }

    public override void OnLocationAvailability(LocationAvailability locationAvailability)
    {
        Log.Debug("FusedLocationProviderSample", "IsLocationAvailable: {0}",locationAvailability.IsLocationAvailable);
    }

    public override void OnLocationResult(LocationResult result)
    {
        if (result.Locations.Any())
        {
            var location = result.Locations.First();
            Log.Debug("Sample", "The latitude is :" + location.Latitude);
        }
        else
        {
            // No locations to work with.
        }
    }
}
```

## <a name="using-the-android-location-service-api"></a>Android の場所のサービス API の使用

Android のロケーション サービスは、Android で位置情報を使用するための古い API です。 場所データがハードウェア センサーによって収集され、アプリケーションでアクセスされるシステム サービスによって収集された、`LocationManager`クラスおよび`ILocationListener`します。

ロケーション サービスがインストールされている Google play 開発者サービスがないデバイスで実行する必要があるアプリケーションに最適です。

ロケーション サービスは特殊な種類の[サービス](https://developer.android.com/guide/components/services.html)システムによって管理されます。 System サービスは、デバイスのハードウェアとやり取りしは常に実行されています。 使用して、システムのロケーション サービスから、アプリケーションでの更新プログラムの場所にはタップ、位置情報の更新を購読は、`LocationManager`と`RequestLocationUpdates`呼び出します。

Android の場所サービスを使用して、ユーザーの場所を取得するには、いくつかの手順が含まれます。

1.  参照を取得、`LocationManager`サービス。
2.  実装、`ILocationListener`場所が変更されたときに、インターフェイスとハンドル イベント。
3.  使用して、`LocationManager`に指定されたプロバイダーの場所の更新を要求します。 `ILocationListener` 、前の手順からのコールバックの受信に使用される、`LocationManager`します。
4.  アプリケーションが適切では不要になった更新プログラムを受信場所の更新プログラムを停止します。

### <a name="location-manager"></a>Location Manager

システムのロケーション サービスのインスタンスにアクセスできる、`LocationManager`クラス。 `LocationManager` システム サービスの場所との対話し、メソッドを呼び出しにより、特別なクラスです。 アプリケーションへの参照を取得できます、`LocationManager`呼び出して`GetSystemService`サービスの種類では、次に示すように渡すとします。

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` 参照を取得することをお勧め、`LocationManager`します。
保持することをお勧め、`LocationManager`クラスの変数としてアクティビティのライフ サイクルのさまざまな時点で呼び出せるようにします。

### <a name="request-location-updates-from-the-locationmanager"></a>要求の場所の更新プログラム、LocationManager から

アプリケーションがへの参照を持つ、 `LocationManager`、確認する必要がある、`LocationManager`場所情報の種類が必要であり、その情報を更新するのにはどのくらいの頻度。 これを呼び出すことによって行います`RequestLocationUpdates`上、`LocationManager`オブジェクト、およびいくつかの条件の更新プログラムおよび場所の更新プログラムを受信するコールバックを渡すことです。 このコールバックは型を実装する必要があります、`ILocationListener`インターフェイス (このガイドの後半で詳しく説明します)。

`RequestLocationUpdates`メソッドは、受信場所の更新を開始する、アプリケーションが希望をシステムの場所のサービスに指示します。 このメソッドでは、プロバイダーと、時間との距離のしきい値の更新頻度を制御するを指定できます。 たとえば、以下のメソッドは 2000 ミリ秒ごとに、GPS の場所プロバイダーから更新プログラムの場所を要求し、場所が 1 メートル以上に変更されたときにのみ。

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

アプリケーションでは、のみ必要とされる頻度もを実行するアプリケーションの場所の更新を要求する必要があります。 これは、バッテリの寿命を保持し、ユーザーのエクスペリエンスを向上させるを作成します。

### <a name="responding-to-updates-from-the-locationmanager"></a>LocationManager から更新プログラムへの応答

アプリケーションがから更新プログラムを要求されたら、 `LocationManager`、実装することで、サービスから情報を受信できる、 [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)インターフェイス。 このインターフェイスは、サービスの場所と、場所プロバイダーをリッスンする 4 つのメソッドを提供します。`OnLocationChanged`します。 システムが呼び出す`OnLocationChanged`ユーザーの場所の場所の更新プログラムを要求するときに設定されている条件に従って場所の変更として修飾するために十分なが変更されたとき。 

次のコードは、メソッド、`ILocationListener`インターフェイス。

```csharp
public class MainActivity : AppCompatActivity, ILocationListener
{
    TextView latitude;
    TextView longitude;
    
    public void OnLocationChanged (Location location)
    {
        // called when the location has been updated.
    }
    
    public OnProviderDisabled(string locationProvider)
    {
        // called when the user disables the provider
    }
    
    public OnProviderEnabled(string locationProvider)
    {
        // called when the user enables the provider
    }
    
    public OnStatusChanged(string locationProvider, Availability status, Bundle extras)
    {
        // called when the status of the provider changes (there are a variety of reasons for this)
    }
}
```

### <a name="unsubscribing-to-locationmanager-updates"></a>LocationManager 更新プログラムに登録を解除します。

システム リソースを節約するためにアプリケーションは場所の更新プログラムをできるだけ早く解除する必要があります。 `RemoveUpdates`方法では、`LocationManager`アプリケーションへの更新プログラムの送信を停止します。  たとえば、アクティビティが呼び出すことができます`RemoveUpdates`で、`OnPause`メソッド場合、アプリケーションに電源を節約することができるように必要はありません場所の更新中に、そのアクティビティが画面上にありません。

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

を、アプリケーションがバック グラウンドで位置情報の更新を取得する必要がある場合、システムの場所のサービスをサブスクライブするカスタム サービスを作成します。 参照してください、 [Android サービスとバック グラウンド処理](~/android/app-fundamentals/services/index.md)詳細情報をガイドします。

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>LocationManager の最適な場所プロバイダーを決定します。

上記のアプリケーションは、場所プロバイダーとして、GPS を設定します。 ただし、GPS 可能性がありますまたは利用できませんすべてのケースでなどかどうか、デバイスは屋内 GPS 受信機はありません。 この場合する場合、結果は、`null`プロバイダーの戻り値。

使用して GPS が利用できない場合に動作するアプリを取得する、`GetBestProvider`アプリケーションの起動時に最適な使用可能な (デバイスでサポートされているし、ユーザーが有効な) 場所プロバイダーを要求するメソッド。 確認する特定のプロバイダーではなく、`GetBestProvider`精度と電源のなどのプロバイダーの要件、 [ `Criteria`オブジェクト](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)します。 `GetBestProvider` 特定の条件の最適なプロバイダーを返します。

次のコードは、使用可能な最適なプロバイダーを取得し、場所の更新を要求するときに使用する方法を示しています。

```csharp
Criteria locationCriteria = new Criteria();   
locationCriteria.Accuracy = Accuracy.Coarse;
locationCriteria.PowerRequirement = Power.Medium;

locationProvider = locationManager.GetBestProvider(locationCriteria, true);

if(locationProvider != null)
{
    locationManager.RequestLocationUpdates (locationProvider, 2000, 1, this);
}
else
{
    Log.Info(tag, "No location providers available");
}
```

> [!NOTE]
>  ユーザーがすべての場所プロバイダーを無効にした場合`GetBestProvider`戻ります`null`します。 実際のデバイスでこのコードの動作を確認するには、GPS、Wi-fi、および 移動体通信ネットワークを有効にすることを確認する**Google 設定 > 位置 > モード**このスクリーン ショットで示すようにします。

[![Android フォンで設定の配置モード 画面](location-images/location-02.png)](location-images/location-02.png#lightbox)

次のスクリーン ショットは、アプリケーション実行を使用して場所を示します`GetBestProvider`:

[![GetBestProvider アプリが緯度、経度、およびプロバイダーを表示します。](location-images/location-03.png)](location-images/location-03.png#lightbox)

注意`GetBestProvider`プロバイダーが動的に変更できません。 代わりに、アクティビティのライフ サイクル中に 1 回、最適な使用可能なプロバイダーを決定します。 設定された後にプロバイダーの状態が変更された場合、アプリケーションが必要になりますでコードを追加、`ILocationListener`メソッド&ndash; `OnProviderEnabled`、 `OnProviderDisabled`、および`OnStatusChanged`&ndash;に関連するあらゆる可能性を処理するために、プロバイダーのスイッチです。

## <a name="summary"></a>まとめ

このガイドでは、Android のロケーション サービスと Google の場所のサービスの API からの融合型場所プロバイダーの両方を使用して、ユーザーの場所の取得について説明します。


## <a name="related-links"></a>関連リンク

- [場所 (サンプル)](https://developer.xamarin.com/samples/monodroid/Location/)
- [FusedLocationProvider (サンプル)](https://developer.xamarin.com/samples/monodroid/FusedLocationProvider/)
- [Google play 開発者サービスします。](https://developer.android.com/google/play-services/index.html)
- [条件のクラス](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)
- [LocationManager クラス](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/)
- [LocationListener クラス](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)
- [LocationClient API](https://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener API](https://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
