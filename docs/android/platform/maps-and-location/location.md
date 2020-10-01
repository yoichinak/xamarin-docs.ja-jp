---
title: Android での位置情報サービス
description: このガイドでは、Android アプリケーションでの位置情報認識について説明し、Android Location Service API と、Google Location Services API で使用可能な Fused Location Provider を使用してユーザーの位置情報を取得する方法について説明します。
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/22/2018
ms.openlocfilehash: 37bfed9a2b49be2d04f58dab70d3185418f0f67d
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91457472"
---
# <a name="location-services-on-android"></a>Android での位置情報サービス

_このガイドでは、Android アプリケーションでの位置情報認識について説明し、Android Location Service API と、Google Location Services API で使用可能な Fused Location Provider を使用してユーザーの位置情報を取得する方法について説明します。_

Android では、基地局、Wi-Fi、GPS など、さまざまな位置情報テクノロジにアクセスできます。 各位置情報テクノロジの詳細は "*位置情報プロバイダー*" によって抽象化されており、アプリケーションでは、使用するプロバイダーに関係なく、同じ方法で位置情報を取得できます。 このガイドでは、Google Play 開発者サービスの一部である Fused Location Provider について説明します。これにより、使用可能なプロバイダーとデバイスの使用方法に基づいて、デバイスの位置情報を取得する最適な方法がインテリジェントに決定されます。 Android Location Service API と、`LocationManager` を使用してシステムの位置情報サービスと通信する方法を示します。 このガイドの 2 番目のパートでは、`LocationManager` を使用する Android Location Services API について説明します。

一般的な経験則として、アプリケーションでは、Fused Location Provider を使用し、必要な場合にのみ古い Android Location Service API にフォールバックすることをお勧めします。

## <a name="location-fundamentals"></a>位置情報の基礎

Android では、位置情報データを操作するためにどの API を選択しても、いくつかの概念は変わりません。 ここでは、位置情報プロバイダーと位置情報関連のアクセス許可について説明します。

### <a name="location-providers"></a>位置情報プロバイダー

ユーザーの場所を特定するために、いくつかのテクノロジが内部的に使用されます。 使用されるハードウェアは、データ収集ジョブ用に選択されている "*位置情報プロバイダー*" の種類によって異なります。 Android では、次の 3 つの位置情報プロバイダーが使用されます。

- **GPS プロバイダー** &ndash; GPS では、最も正確な位置情報が提供され、最も多くの電力が消費され、屋外での動作が最適になります。 このプロバイダーでは、GPS と補助 GPS ([aGPS](https://en.wikipedia.org/wiki/Assisted_GPS)) の組み合わが使用され、基地局によって収集された GPS データが返されます。

- **ネットワーク プロバイダー** &ndash; Wi-Fi と携帯データ ネットワークの組み合わせが提供され、基地局によって収集された aGPS データが含まれます。 GPS プロバイダーより消費電力は少ないですが、返される位置情報データの精度は変化します。

- **パッシブ プロバイダー** &ndash; 他のアプリケーションまたはサービスによって要求されたプロバイダーを使用して、アプリケーションでの位置情報データを生成する "ピギーバック" オプションです。 信頼性は低いですが、位置情報が常に更新されている必要のないアプリケーションに最適な省電力オプションです。

位置情報プロバイダーは、常に使用できるとは限りません。 たとえば、アプリケーションで GPS を使用する必要があっても、GPS が設定でオフになっていたり、デバイスに GPS がまったくない可能性があります。 特定のプロバイダーを使用できない場合、そのプロバイダーを選択すると `null` が返される可能性があります。

### <a name="location-permissions"></a>位置情報のアクセス許可

位置情報認識アプリケーションでは、GPS、Wi-Fi、携帯ネットワークのデータを受信するために、デバイスのハードウェア センサーにアクセスする必要があります。 アクセスは、アプリケーションの Android マニフェストでの適切なアクセス許可によって制御されます。
アプリケーションの要件と API の選択に応じて、利用可能なアクセス許可が 2 つあり、一方を許可します。

- `ACCESS_FINE_LOCATION` &ndash; アプリケーションに GPS へのアクセスを許可します。
    "*GPS プロバイダー*" と "*パッシブ プロバイダー*" のオプションに必要です ("*パッシブ プロバイダーでは、別のアプリケーションまたはサービスによって収集された GPS データにアクセスするためのアクセス許可が必要です*")。 "*ネットワーク プロバイダー*" に対してはオプションのアクセス許可です。

- `ACCESS_COARSE_LOCATION` &ndash; アプリケーションに携帯ネットワークと Wi-Fi の位置情報へのアクセスを許可します。 `ACCESS_FINE_LOCATION` が設定されていない場合、"*ネットワーク プロバイダー*" に必要です。

API バージョン 21 (Android 5.0 Lollipop) 以降を対象とするアプリの場合は、`ACCESS_FINE_LOCATION` を有効にしながら、GPS ハードウェアを搭載していないデバイスで実行することができます。 アプリで GPS ハードウェアが必要な場合は、`android.hardware.location.gps` `uses-feature` 要素を Android マニフェストに明示的に追加する必要があります。 詳細については、Android の [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) 要素のリファレンスを参照してください。

アクセス許可を設定するには、**Solution Pad** で **[プロパティ]** フォルダーを展開し、**AndroidManifest.xml** をダブルクリックします。 アクセス許可が **[必要なアクセス許可]** の下に一覧表示されます。

[![Android マニフェストでの必要なアクセス許可の設定のスクリーンショット](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

これらのアクセス許可のいずれかを設定すると、アプリケーションが位置情報プロバイダーにアクセスするためにユーザーからのアクセス許可が必要であることが Android に通知されます。 API レベル 22 (Android 5.1) 以前を実行するデバイスでは、アプリがインストールされるたびに、ユーザーにこれらのアクセス許可を付与するように求めるメッセージが表示されます。 API レベル 23 (Android 6.0) 以降を実行しているデバイスでは、位置情報プロバイダーに要求を行う前に、アプリで実行時のアクセス許可チェックを実行する必要があります。 

> [!NOTE]
>メモ:`ACCESS_FINE_LOCATION` を設定することは、粗い位置情報データと細かい位置情報データの両方にアクセスすることを暗黙で意味します。 両方のアクセス許可を設定する必要はありません。アプリが動作するために必要な "*最低限の*" アクセス許可のみを設定します。

次のスニペットは、アプリに `ACCESS_FINE_LOCATION` アクセス許可に対するアクセス許可があるかどうかを確認する方法の例です。

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

アプリは、ユーザーがアクセス許可を付与しない (またはアクセス許可を失効させた) シナリオを許容し、その状況を適切に処理する方法を備えている必要があります。 Xamarin.Android での実行時のアクセス許可チェックの実装の詳細については、[アクセス許可ガイド](~/android/app-fundamentals/permissions.md)に関する記事を参照してください。

## <a name="using-the-fused-location-provider"></a>Fused Location Provider の使用

Fused Location Provider は、Android アプリケーションでデバイスから位置情報の更新を受信するための推奨される方法です。これを使用すると、バッテリ効率の高い方法で最善の位置情報を提供するための位置情報プロバイダーが実行時に効率的に選択されます。 たとえば、屋外を歩いているユーザーは、GPS で最善の位置情報を取得します。 その後、GPS が (あったとしても) 正常に機能しない屋内にユーザーが入ると、Fused Location Provider によって自動的に、屋内で適切に機能する Wi-Fi に切り替えることができます。

Fused Location Provider API では、ジオフェンシングやアクティビティ監視など、位置情報対応アプリケーションを支援するためのさまざまなツールが提供されます。 このセクションでは、`LocationClient` の設定、プロバイダーの確立、およびユーザーの位置情報の取得の基本について重点的に説明します。

Fused Location Provider は、[Google Play 開発者サービス](https://developer.android.com/google/play-services/index.html)の一部です。
Fused Location Provider API が機能するには、Google Play 開発者サービス パッケージをインストールし、アプリケーションで適切に構成する必要があり、デバイスに Google Play 開発者サービス APK がインストールされている必要があります。

Xamarin.Android アプリケーションで Fused Location Provider を使用する前に、**Xamarin.GooglePlayServices.Location** パッケージをプロジェクトに追加しておく必要があります。 さらに、次の `using` ステートメントを、以下で説明するクラスを参照するすべてのソース ファイルに追加する必要があります。

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Google Play 開発者サービスがインストールされていることを確認する

Google Play 開発者サービスがインストールされていない (または期限が切れている) ときに、Fused Location Provider を使用しようとすると Xamarin.Android はクラッシュし、ランタイム例外が発生します。  Google Play 開発者サービスがインストールされていない場合、アプリケーションは、前に説明した Android Location Service にフォールバックする必要があります。 Google Play 開発者サービスが古い場合、アプリではユーザーにメッセージを表示し、インストールされている Google Play 開発者サービスのバージョンの更新を求めることができます。

次のスニペットは、Google Play 開発者サービスがインストールされているかどうかを、Android アクティビティのプログラムで確認する方法の例です。

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

Fused Location Provider と対話するには、Xamarin.Android アプリケーションに `FusedLocationProviderClient` のインスタンスが必要です。 このクラスでは、位置情報の更新をサブスクライブし、デバイスの最後の既知の位置情報を取得するために必要なメソッドが公開されています。

次のコード スニペットで示すように、アクティビティの `OnCreate` メソッドは、`FusedLocationProviderClient` への参照を取得するのに適した場所です。

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

### <a name="getting-the-last-known-location"></a>最後の既知の位置情報を取得する

`FusedLocationProviderClient.GetLastLocationAsync()` メソッドでは、最小限のコーディング オーバーヘッドで、デバイスの最後の既知の位置情報をすばやく取得するための、簡単で非ブロッキングの方法が、Xamarin.Android アプリケーションに提供されます。

次のスニペットでは、`GetLastLocationAsync` メソッドを使用してデバイスの位置情報を取得する方法を示します。

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

### <a name="subscribing-to-location-updates"></a>位置情報の更新をサブスクライブする

次のコード スニペットで示すように、Xamarin.Android アプリケーションでは、`FusedLocationProviderClient.RequestLocationUpdatesAsync` メソッドを使用して、Fused Location Provider から位置情報の更新をサブスクライブすることもできます。

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```

このメソッドは、2 つのパラメーターを受け取ります。

- **`Android.Gms.Location.LocationRequest`** &ndash; Xamarin.Android アプリケーションでは、パラメーターで `LocationRequest` オブジェクトを使用して、Fused Location Provider の動作方法を渡します。 `LocationRequest` には、要求を行う頻度や、正確な位置情報の更新がどの程度重要か、といった情報が保持されます。 たとえば、重要な位置情報を要求すると、デバイスでは GPS が使用され、その結果、位置情報を決定するときの電力が多くなります。 次のコード スニペットでは、高い精度の位置情報の更新を約 5 分ごとに確認する場合の、`LocationRequest` の作成方法を示します。場所の更新についてはしています (ただし、要求の間隔は 2 分以上)。 Fused Location Provider では、`LocationRequest` を参考にして、デバイスの位置情報を特定するときに使用する位置情報プロバイダーが選択されます。

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```

- **`Android.Gms.Location.LocationCallback`** &ndash; 位置情報の更新を受信するため、Xamarin.Android アプリケーションでは `LocationProvider` 抽象クラスをサブクラス化する必要があります。 このクラスで公開されている 2 つのメソッドは、位置情報でアプリを更新するために、Fused Location Provider によって呼び出される場合があります。 これについては、以下で詳しく説明します。

位置情報の更新を Xamarin.Android アプリケーションに通知するため、Fused Location Provider では `LocationCallBack.OnLocationResult(LocationResult result)` が呼び出されます。 `Android.Gms.Location.LocationResult` パラメーターには、更新された位置情報が含まれます。

Fused Location Provider で位置情報データの使用可能性の変化が検出されると、`LocationProvider.OnLocationAvailability(LocationAvailability
locationAvailability)` メソッドが呼び出されます。 `LocationAvailability.IsLocationAvailable` プロパティで `true` が返された場合、`OnLocationResult` によって報告されたデバイスの位置情報の結果は、`LocationRequest` によって要求されている正確さと新しさであるものと想定できます。 `IsLocationAvailable` が false の場合、`OnLocationResult` では位置情報の結果は返されません。

次のコード スニペットは、`LocationCallback` オブジェクトの実装例です。

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

## <a name="using-the-android-location-service-api"></a>Android Location Service API を使用する

Android Location Service は、Android で位置情報を使用するための古い API です。 位置情報データは、ハードウェア センサーによって収集され、システム サービスによって収集されて、`LocationManager` クラスと `ILocationListener` を使用してアプリケーションでアクセスされます。

位置情報サービスは、Google Play 開発者サービスがインストールされていないデバイスで実行する必要があるアプリケーションに最適です。

位置情報サービスは、システムによって管理される特別な種類の[サービス](https://developer.android.com/guide/components/services.html)です。 システム サービスは、デバイスのハードウェアと対話し、常に実行されています。 アプリケーションで位置情報の更新を利用するには、`LocationManager` と `RequestLocationUpdates` の呼び出しを使用して、システムの位置情報サービスから位置情報の更新をサブスクライブします。

Android Location Service を使用してユーザーの位置情報を取得するには、いくつかのステップを実行します。

1. `LocationManager` サービスへの参照を取得します。
2. `ILocationListener` インターフェイスを実装し、位置情報が変更されたときのイベントを処理します。
3. `LocationManager` を使用して、指定したプロバイダーに位置情報の更新を要求します。 前のステップの `ILocationListener` を使用して、`LocationManager` からコールバックを受け取ります。
4. アプリケーションで更新を受信する必要がなくなったら、位置情報の更新を停止します。

### <a name="location-manager"></a>Location Manager

システムの位置情報サービスには、`LocationManager` クラスのインスタンスを使用してアクセスできます。 `LocationManager` は、システムの位置情報サービスと対話し、そこでメソッドを呼び出すことができる、特別なクラスです。 アプリケーションでは、次に示すように、`GetSystemService` を呼び出してサービスの種類を渡すことにより、`LocationManager` への参照を取得できます。

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` は、`LocationManager` への参照を取得するのに適した場所です。
アクティビティのライフサイクルのさまざまな時点で呼び出すことができるように、`LocationManager` をクラス変数として保持することをお勧めします。

### <a name="request-location-updates-from-the-locationmanager"></a>LocationManager に位置情報の更新を要求する

アプリケーションでは、`LocationManager` への参照を取得した後、必要な位置情報の種類と、その情報を更新する頻度を、`LocationManager` に通知する必要があります。 これを行うには、`LocationManager` オブジェクトで `RequestLocationUpdates` を呼び出し、更新に関するいくつかの条件と、位置情報の更新を受け取るコールバックを渡します。 このコールバックは、`ILocationListener` インターフェイスを実装する必要がある型です (このガイドで後ほど詳しく説明します)。

`RequestLocationUpdates` メソッドでは、アプリケーションが位置情報の更新の受信を開始することを望んでいることが、システムの位置情報サービスに通知されます。 このメソッドを使用すると、プロバイダーと、更新頻度を制御するための時間と距離のしきい値を、指定することができます。 たとえば、次のメソッドでは、GPS 位置情報プロバイダーに対し、2000 ミリ秒ごとに、位置の変化が 1 メートルより大きい場合にのみ、位置情報を更新するよう要求しています。

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

アプリケーションでは、アプリケーションを正常に実行するために必要な頻度でのみ、位置情報の更新を要求する必要があります。 これにより、バッテリの寿命が維持され、ユーザーのエクスペリエンスが向上します。

### <a name="responding-to-updates-from-the-locationmanager"></a>LocationManager からの更新に応答する

アプリケーションでは、`LocationManager` に更新を要求した後、[`ILocationListener`](xref:Android.Locations.ILocationListener) インターフェイスを実装することによって、サービスから情報を受け取ることができます。 このインターフェイスでは、位置情報サービスと位置情報プロバイダー、`OnLocationChanged` をリッスンする 4 つのメソッドが提供されています。 位置情報の更新を要求するときに設定された条件に従って、ユーザーの位置の変化が位置情報の変化に十分に該当するときは、システムによって `OnLocationChanged` が呼び出されます。 

次のコードでは、`ILocationListener` インターフェイスのメソッドを示します。

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

### <a name="unsubscribing-to-locationmanager-updates"></a>LocationManager の更新のサブスクライブを解除する

システム リソースを節約するため、アプリケーションではできるだけ早く場所の更新のサブスクライブを解除する必要があります。 `RemoveUpdates` メソッドでは、アプリケーションへの更新の送信を停止するよう `LocationManager` に指示されます。  たとえば、アクティビティの `OnPause` メソッドで `RemoveUpdates` を呼び出すことにより、アクティビティが画面上にないときにアプリケーションで位置情報を更新する必要がない場合は、電力を節約できます。

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

バックグラウンドの間もアプリケーションで位置情報の更新を取得する必要がある場合は、システムの位置情報サービスをサブスクライブするカスタム サービスを作成する必要があります。 詳細については、[Android サービスでのバックグラウンド処理](~/android/app-fundamentals/services/index.md)に関するガイドを参照してください。

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>LocationManager に最適な位置情報プロバイダーを決定する

上のアプリケーションでは、位置情報プロバイダーとして GPS を設定しています。 ただし、デバイスが屋内にある場合や GPS レシーバーを備えていない場合など、GPS を使用できないことがあります。 このような場合、プロバイダーとして `null` が返されます。

GPS が使用できないときでもアプリを動作させるには、`GetBestProvider` メソッドを使用して、アプリケーションの起動時に使用できる最適な (デバイスでサポートされ、ユーザーが有効にしている) 位置情報プロバイダーを要求します。 特定のプロバイダーを渡す代わりに、[`Criteria` オブジェクト](xref:Android.Locations.Criteria)を使用して、精度や電力などのプロバイダーに対する要件を、`GetBestProvider` に指定できます。 `GetBestProvider` からは、指定した条件に最適なプロバイダーが返されます。

次のコードでは、使用可能な最善のプロバイダーを取得し、位置情報の更新を要求するときにそれを使用する方法を示します。

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
> ユーザーがすべての位置情報プロバイダーを無効にしている場合は、`GetBestProvider` から `null` が返されます。 実際のデバイスでこのコードがどのように動作するかを確認するには、次のスクリーンショットで示すように、 **[Google 設定] > [場所] > [モード]** で、GPS、Wi-Fi、携帯ネットワークを有効にする必要があります。
>
> [![Android フォンでの場所モード設定画面](location-images/location-02.png)](location-images/location-02.png#lightbox)
>
> 次のスクリーンショットでは、`GetBestProvider` を使用して実行されている位置情報アプリケーションを示します。
>
> [![緯度、経度、プロバイダーを表示する GetBestProvider アプリ](location-images/location-03.png)](location-images/location-03.png#lightbox)
>
> `GetBestProvider` ではプロバイダーが動的に変更されないことに注意してください。 代わりに、アクティビティのライフサイクル中に 1 回だけ、使用可能な最善のプロバイダーが決定されます。 設定された後でプロバイダーの状態が変化する場合は、アプリケーションで `ILocationListener` のメソッド `OnProviderEnabled`、`OnProviderDisabled`、`OnStatusChanged` にコードを追加し、プロバイダーの切り替えに関連するすべての可能性を処理する必要があります。

## <a name="summary"></a>まとめ

このガイドでは、Android Location Service と、Google Location Services API の Fused Location Provider の両方を使用して、ユーザーの位置情報を取得する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [場所 (サンプル)](/samples/xamarin/monodroid-samples/location)
- [FusedLocationProvider (サンプル)](/samples/xamarin/monodroid-samples/fusedlocationprovider)
- [Google Play 開発者サービス](https://developer.android.com/google/play-services/index.html)
- [Criteria クラス](xref:Android.Locations.Criteria)
- [LocationManager クラス](xref:Android.Locations.LocationManager)
- [LocationListener クラス](xref:Android.Locations.ILocationListener)
- [LocationClient API](https://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener API](https://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)