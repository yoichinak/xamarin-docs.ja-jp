---
title: ロケーション サービス
description: このガイドでは、Android アプリケーションの場所の認識を導入し、Google の場所のサービス API で使用可能な余裕が生まれます場所プロバイダーと同様に、Android の場所のサービスの API を使用して、ユーザーの場所を取得する方法について説明します。
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/22/2018
ms.openlocfilehash: b509f6892b27afa053a6ee913826d913d7ad54a8
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/25/2018
ms.locfileid: "34546139"
---
# <a name="location-services"></a>ロケーション サービス

_このガイドでは、Android アプリケーションの場所の認識を導入し、Google の場所のサービス API で使用可能な余裕が生まれます場所プロバイダーと同様に、Android の場所のサービスの API を使用して、ユーザーの場所を取得する方法について説明します。_

## <a name="location-services-overview"></a>サービスの場所の概要

Android では、セル タワー場所、Wi-fi、GPS など、さまざまな場所テクノロジへのアクセスを提供します。 それぞれの場所のテクノロジの詳細を抽象化された*場所プロバイダー*アプリケーションを使用するプロバイダーに関係なく同じ方法での場所を取得できるようにします。 このガイドでは、Google Play サービスをインテリジェントにどのようなプロバイダーが追加され、デバイスの使用方法に基づいてデバイスの場所を取得する最善の方法を決定するの一部である余裕が生まれます場所プロバイダーが導入されています。 Android のロケーション サービス API し、システムの場所と通信する方法を示していますサービスを使用して、`LocationManager`です。 Android のロケーション サービス API を使用して、番組ガイドの 2 番目の部分を検討、`LocationManager`です。
 
全般則として、アプリケーションは、代替で古い Android 場所サービス API に必要な場合にのみ、余裕が生まれます場所プロバイダーを使用する優先必要があります。

## <a name="location-fundamentals"></a>場所の基礎

Android で場所データを操作するために選択するどのような API は重要でない、いくつかの概念は同じままです。このセクションでは、プロバイダーの場所と場所に関連するアクセス許可について説明します。

### <a name="location-providers"></a>プロバイダーの場所

いくつかのテクノロジは、ユーザーの場所を突き止めるに内部的に使用されます。 使用されるハードウェアの種類によって異なります*場所プロバイダー*データを収集する場合のジョブに選択します。 Android では、次の 3 つの場所のプロバイダーを使用します。

-   **GPS プロバイダー** &ndash; GPS は最も正確な場所を指定、最も消費電力を使用して、屋外で最適です。 このプロバイダーは、GPS と支援 GPS の組み合わせを使用して ([aGPS](http://en.wikipedia.org/wiki/Assisted_GPS))、携帯電話塔によって収集された GPS データが返されます。

-   **プロバイダーのネットワーク**&ndash;セル塔によって収集された aGPS データを含む、WiFi および携帯電話のデータの組み合わせを提供します。 GPS プロバイダーよりも消費電力しますが、さまざまな精度の場所データを返します。

-   **パッシブ プロバイダー** &ndash;他のアプリケーションやサービスによって要求されたプロバイダーを使用して、アプリケーションで場所データを生成する「ピギーバック」オプション。 これは、信頼性が低いが一定の場所の更新プログラムが機能を必要としないアプリケーションの省電力オプションに適切です。

プロバイダーの場所は、常に使用できません。 たとえばのアプリケーションでは、GPS を使用する場合がありますの設定で、GPS をオフにすることがあります。 またはデバイスの GPS をまったくがない可能性がします。 特定のプロバイダーが使用できない場合は、そのプロバイダーが返す可能性がありますを選択する`null`です。

### <a name="location-permissions"></a>場所のアクセス許可

場所に対応するアプリケーションでは、GPS、Wi-fi、および携帯電話のデータを受信するデバイスのハードウェア センサーへのアクセスが必要です。 アクセスは、アプリケーションの Android マニフェストで適切なアクセス許可によって制御されます。
2 つの権限がある使用可能な&ndash;によっては、アプリケーションの要件、API の選択が許可するいずれか。

-   `ACCESS_FINE_LOCATION` &ndash; GPS へのアプリケーション アクセスを許可します。
    必要な*GPS プロバイダー*と*パッシブ プロバイダー*オプション (*パッシブ プロバイダーには、別のアプリケーションまたはサービスによって収集された GPS データにアクセスする許可が必要です。*)。 省略可能なアクセス許可、*ネットワーク プロバイダー*です。

-   `ACCESS_COARSE_LOCATION` &ndash; 携帯電話や Wi-fi の場所にアプリケーションのアクセスを許可します。 必要な*ネットワーク プロバイダー*場合`ACCESS_FINE_LOCATION`が設定されていません。

API 21 (Android 5.0 ロリポップ) のバージョンを対象とするアプリの以降では、有効にできますか`ACCESS_FINE_LOCATION`して GPS ハードウェアがないデバイス上で引き続き実行します。 かどうか、アプリには、GPS のハードウェアが必要とする必要があります明示的に追加する、 `android.hardware.location.gps` `uses-feature` Android マニフェストへの要素。 詳細については、Android を参照してください。[使用機能](https://developer.android.com/guide/topics/manifest/uses-feature-element.html)要素への参照。

アクセス許可を設定するには、展開、**プロパティ**内のフォルダー、**ソリューション パッド** をダブルクリック**AndroidManifest.xml**です。 アクセス許可が表示されます**必要なアクセス許可**:

[![Android マニフェストの必要なアクセス許可の設定のスクリーン ショット](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

これらのアクセス許可のいずれかの設定に指示 Android 場所プロバイダーにアクセスするために、ユーザーからのアクセス許可がアプリケーションに必要があること。 デバイスを API レベル 22 (Android 5.1) を実行または低いが、アプリがインストールされるたびにこれらのアクセス許可を付与するユーザーを要求します。 API を実行しているデバイス上のレベル 23 (Android 6.0) または、以降では、アプリが場所プロバイダーの要求を行う前に実行時アクセス許可のチェックを実行する必要があります。 

> [!NOTE]
>注: 設定`ACCESS_FINE_LOCATION`大まか場所データの両方にアクセスを意味します。 両方のアクセス許可、のみを設定することはありません必要、*最小限*アプリが動作に必要なアクセス許可。

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

アプリは、ここで、ユーザー アクセス権が付与されます (または、アクセス許可が失効) シナリオを許容して、正常にそのような状況に対処する手段が用意する必要があります。 参照してください、[権限ガイド](~/android/app-fundamentals/permissions.md)Xamarin.Android チェックイン、実行時アクセス許可の実装の詳細についてはします。


## <a name="using-the-fused-location-provider"></a>余裕が生まれます場所プロバイダーを使用します。

余裕が生まれます場所プロバイダーは、場所プロバイダー バッテリ効率的な方法で最適な場所情報を提供する実行時に効率的に選択されますので、デバイスからの場所の更新プログラムを受信する Android アプリケーションの推奨される方法です。 たとえば、outdoors を歩きながら、ユーザーは、GPS を読み取り、最適な場所を取得します。 場合は、ユーザー、屋内で、ここで GPS 動作が不十分な場合)、余裕が生まれます場所プロバイダーは、WiFi、動作は屋内でより高いに自動的に切り替えることがあります。
 
余裕が生まれます場所プロバイダー API は、さまざまな場所に対応するなど、アプリケーション ジオフェンシング アクティビティの監視を支援するためには、その他のツールを提供します。 このセクションで行うフォーカスの設定の基本の`LocationClient`プロバイダーを確立して、ユーザーの場所を取得します。

余裕が生まれます場所プロバイダーの一部である[Google Play サービス](http://developer.android.com/google/play-services/index.html)です。
Google Play サービス パッケージをインストールし、余裕が生まれます場所プロバイダー API を使用するアプリケーションで正しく構成されているし、デバイスには、Google プレイ サービス APK インストールされている必要があります。

前に、Xamarin.Android アプリケーションに余裕が生まれます場所プロバイダーを使用できますする必要があります、追加、 **Xamarin.GooglePlayServices.Maps**をプロジェクトにパッケージします。 さらに、次`using`ステートメントは、次に示すクラスを参照するソース ファイルに追加する必要があります。

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Google Play サービスがインストールされているかどうかは確認

Google Play サービスがインストールされていないときに余裕が生まれます場所プロバイダーを使用する場合は、クラッシュ (または期限切れ) Xamarin.Android は、ランタイム例外が発生します。  Google Play サービスがインストールされていない場合、アプリケーションする必要があります、Android の場所をサービスに切り替え前述のとおりです。 Google Play サービスが期限切れの場合は、アプリは Google Play サービスのインストールされているバージョンを更新するように求めるユーザーにメッセージを表示でした。

このスニペットは、どのように Android アクティビティ プログラムでチェックできる Google Play サービスがインストールされているかどうかの例を示します。

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

Xamarin.Android アプリケーションに余裕が生まれます場所プロバイダーとの対話のインスタンスを持つ必要があります、`FusedLocationProviderClient`です。 このクラスは、場所の更新をサブスクライブして、デバイスの最後の既知の場所を取得するために必要なメソッドを公開します。

`OnCreate`アクティビティのメソッドは、適切な場所への参照を取得する、`FusedLocationProviderClient`次のコード スニペットで示すように、します。

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

`FusedLocationProviderClient.GetLastLocationAsync()`メソッドは、最小限のコーディングがオーバーヘッドとデバイスの最後の既知の場所を迅速に取得する Xamarin.Android アプリケーションの単純な非ブロッキング手段を提供します。

このスニペットを使用する方法を示しています、`GetLastLocationAsync`デバイスの位置を取得します。

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

### <a name="subscribing-to-location-updates"></a>更新プログラムの場所へのサブスクライブ

Xamarin.Android アプリケーションできますも更新定期受信する場所を使用しての余裕が生まれます場所プロバイダーから、`FusedLocationProviderClient.RequestLocationUpdatesAsync`メソッドを次のコード スニペットに示すようにします。

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
このメソッドは、2 つのパラメーターを受け取ります。

-   **`Android.Gms.Location.LocationRequest`** &ndash; A`LocationRequest`オブジェクトが作業する余裕が生まれます場所プロバイダーで Xamarin.Android アプリケーションがパラメーターを渡します。 `LocationRequest`情報を保持してこのような方法に頻繁に要求を行う必要がありますまたは重要性、正確な位置更新する必要があります。 たとえば、重要な場所要求では、場所を決定するときに、GPS、したがって複数の電源を使用するデバイスが発生します。 このコード スニペットを作成する方法を示しています、`LocationRequest`正確性に優れた、場所の場所の更新 (ただし要求間で 2 分よりも早くされません) の分 5 台に約をチェックします。 余裕が生まれます場所プロバイダーを使用して、`LocationRequest`どの場所プロバイダーをデバイスの場所を決定しようとしているときに使用するためのガイダンスとして。

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; Xamarin.Android アプリケーションの場所の更新プログラムを受信するためにサブクラス化する必要があります、`LocationProvider`抽象クラスです。 このクラスは、場所情報を使って、アプリを更新する余裕が生まれます場所プロバイダーによって呼び出される可能性がありますが 2 つのメソッドを公開します。 これについてで詳しく説明します。

余裕が生まれます場所プロバイダーを呼び出す場所の更新の Xamarin.Android アプリケーションを通知するために、`LocationCallBack.OnLocationResult(LocationResult result)`です。 `Android.Gms.Location.LocationResult`パラメーター更新プログラムの場所情報が含まれます。

余裕が生まれます場所プロバイダーでは、場所データの可用性の変更を検出、ときに呼び出されます、`LocationProvider.OnLocationAvaibility(LocationAvailability
locationAvailability)`メソッドです。 場合、`LocationAvailability.IsLocationAvailable`プロパティから返される`true`でデバイスの場所の結果が報告されることが想定することができますし、`OnLocationResult`正確であり、最新の状態に必要なは、`LocationRequest`です。 場合`IsLocationAvailable`が false の場合、場所の結果はなしで返される`OnLocationResult`です。

このコード スニペットの実装のサンプル、`LocationCallback`オブジェクト。

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

## <a name="using-the-android-location-service-api"></a>Android のロケーション サービス API の使用

Android のロケーション サービスは、Android で場所情報を使用するため古い API です。 場所のデータがハードウェア センサーによって収集され、アプリケーションにアクセスするシステム サービスによって収集、`LocationManager`クラスおよび`ILocationListener`です。

ロケーション サービスは、Google Play サービスがインストールされていないデバイス上で実行する必要があるアプリケーションに最適です。

ロケーション サービスは特殊な種類の[サービス](http://developer.android.com/guide/components/services.html)システムによって管理されます。 システム サービスを使用して、デバイスのハードウェア通信が常に実行されています。 アプリケーションでは場所の更新をタップして、おにサブスクライブする場所の更新プログラム、システムの場所を使用してサービスから、`LocationManager`と`RequestLocationUpdates`呼び出します。

Android のロケーション サービスを使用して、ユーザーの場所を取得するには、手順をいくつか示します。

1.  参照を取得、`LocationManager`サービス。
2.  実装、`ILocationListener`場所が変更されたときに、インターフェイスとハンドル イベント。
3.  使用して、`LocationManager`に指定されたプロバイダーの場所の更新を要求します。 `ILocationListener`前の手順からのコールバックの受信に使用される、`LocationManager`です。
4.  アプリケーションが適切では不要になった更新プログラムを受信場所の更新を停止します。

### <a name="location-manager"></a>Location Manager

インスタンスにシステムのロケーション サービスにアクセスできます、`LocationManager`クラスです。 `LocationManager` 特殊クラスにより、システムの場所のサービスとの対話し、メソッドを呼び出すことです。 アプリケーションへの参照を取得できます、`LocationManager`を呼び出して`GetSystemService`を次に示すようにサービスの種類で渡すこと。

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` 参照を取得することをお勧め、`LocationManager`です。
保持することをお勧め、`LocationManager`クラス変数としてアクティビティのライフ サイクルのさまざまな時点で呼び出すおことができるようにします。

### <a name="request-location-updates-from-the-locationmanager"></a>LocationManager から場所の更新を要求します。

アプリケーションがへの参照を持つ、`LocationManager`に指示する必要がある、`LocationManager`場所情報の種類が必要であり、その情報を更新するのにはどのくらいの頻度。 これを呼び出すことによって行います`RequestionLocationUpdates`上、`LocationManager`オブジェクト、およびいくつかの条件の更新プログラムおよび場所の更新を受信するコールバックに渡すことです。 このコールバックは、型を実装する必要がある、`ILocationListener`インターフェイス (このガイドの後半で詳しく説明します)。

`RequestionLocationUpdates`メソッドは、アプリケーションは受信場所の更新を開始するよう、システム サービスの場所を指示します。 このメソッドでは、プロバイダーに加え、時間との距離のしきい値の更新頻度を制御するを指定することができます。 たとえば、以下の要求の場所の下のメソッドを更新 GPS 場所プロバイダーから 2000 ミリ秒ごとに、場所が 1 メートル以上に変更されたときにのみとします。

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

アプリケーションでは、のみ必要とされる頻度もを実行するアプリケーションの場所の更新を要求する必要があります。 これにより、バッテリの寿命が保持され、ユーザーのエクスペリエンスを向上させるを作成します。

### <a name="responding-to-updates-from-the-locationmanager"></a>LocationManager から更新プログラムへの応答

アプリケーションがから更新プログラムを要求されたら、 `LocationManager`、実装することによって、サービスから情報を受信できる、 [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)インターフェイスです。 このインターフェイスをリッスンするサービスの場所と場所プロバイダーでは、次の 4 つのメソッドを提供`OnLocationChanged`です。 システムが呼び出す`OnLocationChanged`ユーザーの所在地の場所の更新プログラムを要求するときに設定された条件に従って場所の変更と見なされるために十分な数が変更されたとき。 

次のコード内のメソッドを示しています、`ILocationListener`インターフェイス。

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

### <a name="unsubscribing-to-locationmanager-updates"></a>LocationManager 更新アンサブスク ライブ

システム リソースを節約するためにアプリケーションは場所の更新プログラムをできるだけ早く取り消し必要があります。 `RemoveUpdates`方法では、`LocationManager`アプリケーションの更新の送信を停止します。  たとえば、アクティビティを呼び出すことが`RemoveUpdates`で、`OnPause`メソッド場合、アプリケーションは、電力を節約することができるように必要はありません場所の更新中に、そのアクティビティが画面上にありません。

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

場合は、アプリケーションは、バック グラウンドでの場所の更新プログラムを取得する必要があります、システムの場所のサービスをサブスクライブするカスタム サービスを作成します。 参照してください、 [Android サービスと Backgrounding](~/android/app-fundamentals/services/index.md)についてガイドします。

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>最適な場所のプロバイダー、LocationManager を決定します。

上記のアプリケーションは、GPS を場所プロバイダーとして設定します。 ただし、GPS できない場合があります、常になどかどうか、デバイスは屋内でかがない GPS 受信装置です。 そうでは、この場合、結果は、`null`プロバイダーの戻り値。

使用するアプリの GPS が利用できない場合に使用するには、`GetBestProvider`メソッドをアプリケーションの起動時に最適な (デバイスでサポートされているし、ユーザーが有効な) の使用可能な場所プロバイダーに問い合わせてください。 特定のプロバイダーに渡すことは、代わりにあることがわかります`GetBestProvider`- 精度と電源 - などのプロバイダーの要件、 [ `Criteria`オブジェクト](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)です。 `GetBestProvider` 指定された条件の最適なプロバイダーを返します。

次のコードは、最適な使用可能なプロバイダーを取得し、場所の更新を要求するときに使用する方法を示しています。

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
>  ユーザーが、すべての場所のプロバイダーを無効な場合`GetBestProvider`戻ります`null`です。 実際のデバイスでこのコードの動作を確認するには、GPS、Wi-fi、および移動体通信ネットワークで有効にすることを確認する**Google 設定 > の場所 > モード**このスクリーン ショットに示すようにします。

[![Android フォンの設定の配置モード 画面](location-images/location-02.png)](location-images/location-02.png#lightbox)

場所アプリケーション実行を使用して、次のスクリーン ショットに示します`GetBestProvider`:

[![GetBestProvider アプリが緯度、経度、およびプロバイダーの表示](location-images/location-03.png)](location-images/location-03.png#lightbox)

注意してください`GetBestProvider`プロバイダーが動的に変更できません。 代わりに、アクティビティのライフ サイクル中に 1 回、最適な使用可能なプロバイダーを決定します。 設定されている後にプロバイダーの状態が変更された場合、アプリケーションが必要になりますでコードを追加、`ILocationListener`メソッド&ndash; `OnProviderEnabled`、 `OnProviderDisabled`、および`OnStatusChanged`&ndash;に関連するすべての可能性を処理する、プロバイダーのスイッチです。

## <a name="summary"></a>まとめ

このガイドでは、Android のロケーション サービスと Google の場所のサービスの API から余裕が生まれます場所プロバイダーの両方を使用して、ユーザーの場所を取得するについて説明します。


## <a name="related-links"></a>関連リンク

- [場所 (サンプル)](https://developer.xamarin.com/samples/Location/)
- [FusedLocationProvider (サンプル)](https://developer.xamarin.com/samples/FusedLocationProvider/)
- [Google Play サービス](http://developer.android.com/google/play-services/index.html)
- [条件クラス](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)
- [LocationManager クラス](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/)
- [LocationListener クラス](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)
- [LocationClient API](http://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener API](http://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
