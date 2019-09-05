---
title: Android 上のロケーションサービス
description: このガイドでは、Android アプリケーションでの位置情報認識について説明し、Android ロケーションサービス API を使用してユーザーの場所を取得する方法と、Google Location Services API で使用可能なヒューズ位置プロバイダーについて説明します。
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/22/2018
ms.openlocfilehash: 8366796af47e8915bf0bd9ba680e6144e1cfaebc
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280605"
---
# <a name="location-services-on-android"></a>Android 上のロケーションサービス

_このガイドでは、Android アプリケーションでの位置情報認識について説明し、Android ロケーションサービス API を使用してユーザーの場所を取得する方法と、Google Location Services API で使用可能なヒューズ位置プロバイダーについて説明します。_

Android では、セルタワーの場所、Wi-fi、GPS など、さまざまな場所のテクノロジにアクセスできます。 各場所テクノロジの詳細は*場所プロバイダー*によって抽象化されており、使用されているプロバイダーに関係なく、同じ方法で場所を取得することができます。 このガイドでは、Google Play 開発者サービスの一部である、ヒューズのある場所プロバイダーについて説明します。これにより、使用可能なプロバイダーとデバイスの使用方法に基づいて、デバイスの場所を最適に取得する方法がインテリジェントに決定されます。 Android ロケーションサービス API と、を`LocationManager`使用してシステムロケーションサービスと通信する方法を示します。 このガイドの2番目のパートでは、 `LocationManager`を使用した Android Location Services API について説明します。

一般的な経験則として、アプリケーションでは、必要な場合にのみ古い Android ロケーションサービス API を使用することをお勧めします。

## <a name="location-fundamentals"></a>場所の基礎

Android では、場所データを操作するためにどの API が選択されていても、いくつかの概念は変わりません。 ここでは、場所プロバイダーと場所に関連するアクセス許可について説明します。

### <a name="location-providers"></a>場所プロバイダー

ユーザーの場所を特定するために、いくつかのテクノロジが内部的に使用されます。 使用されるハードウェアは、データの収集ジョブに対して選択された*場所プロバイダー*の種類によって異なります。 Android では、次の3つの場所プロバイダーを使用します。

- **GPS プロバイダー**&ndash; GPS は、最も正確な場所を提供し、最も多くの電力を消費し、屋外で最適に動作します。 このプロバイダーは、GPS と支援型 GPS ([Agps](https://en.wikipedia.org/wiki/Assisted_GPS)) を組み合わせて使用します。これは、携帯電話の塔によって収集された gps データを返します。

- **ネットワークプロバイダー**&ndash;携帯ネットワークによって収集された agps データを含む、WiFi と携帯データの組み合わせを提供します。 GPS プロバイダーよりも電力消費量は少なくても、精度が異なる場所データを返します。

- **パッシブプロバイダー**&ndash;アプリケーションで場所データを生成するために他のアプリケーションまたはサービスによって要求されたプロバイダーを使用する "便乗" オプション。 これは信頼性が低く、省電力のオプションであり、固定の場所の更新を必要としないアプリケーションに最適です。

場所プロバイダーは、常に使用できるとは限りません。 たとえば、アプリケーションで GPS を使用する場合がありますが、GPS が設定でオフになっているか、デバイスに GPS がまったくない可能性があります。 特定のプロバイダーを使用できない場合は、そのプロバイダーを`null`選択するとが返される可能性があります。

### <a name="location-permissions"></a>場所のアクセス許可

場所を認識するアプリケーションは、GPS、Wi-fi、携帯電話のデータを受信するために、デバイスのハードウェアセンサーにアクセスする必要があります。 アクセスは、アプリケーションの Android マニフェストで適切なアクセス許可を使用して制御されます。
アプリケーションの要件と API &ndash;の選択に応じて、次の2つのアクセス許可を使用できます。

- `ACCESS_FINE_LOCATION`&ndash;アプリケーションが GPS にアクセスできるようにします。
    *Gps プロバイダー*オプションと*パッシブプロバイダー*オプションに必要です (*パッシブプロバイダーには、別のアプリケーションまたはサービスによって収集された GPS データにアクセスするためのアクセス許可が必要*です)。 *ネットワークプロバイダー*のオプションのアクセス許可。

- `ACCESS_COARSE_LOCATION`&ndash;携帯電話と wi-fi の場所へのアプリケーションのアクセスを許可します。 が設定されて`ACCESS_FINE_LOCATION`いない場合、*ネットワークプロバイダー*に必要です。

API バージョン 21 (Android 5.0 ロリポップ) 以降を対象とするアプリでは、GPS `ACCESS_FINE_LOCATION`ハードウェアが搭載されていないデバイスでを有効にし、引き続き実行できます。 アプリに GPS ハードウェアが必要な`android.hardware.location.gps` `uses-feature`場合は、明示的に Android マニフェストに要素を追加する必要があります。 詳細については、「Android の[使用-機能](https://developer.android.com/guide/topics/manifest/uses-feature-element.html)要素のリファレンス」を参照してください。

アクセス許可を設定するには、 **Solution Pad**の **[プロパティ]** フォルダーを展開し、 **[androidmanifest .xml]** をダブルクリックします。 アクセス許可は、 **[必要なアクセス許可]** の下に一覧表示されます。

[![Android マニフェストに必要なアクセス許可の設定のスクリーンショット](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

これらのアクセス許可のいずれかを設定すると、アプリケーションが場所プロバイダーにアクセスするためにユーザーからのアクセス許可が必要であることが Android に通知されます。 API レベル 22 (Android 5.1) 以降を実行するデバイスでは、アプリがインストールされるたびに、ユーザーにこれらのアクセス許可を付与するように求めるメッセージが表示されます。 API レベル 23 (Android 6.0) 以降を実行しているデバイスでは、アプリは場所プロバイダーの要求を行う前に実行時のアクセス許可チェックを実行する必要があります。 

> [!NOTE]
>メモ:を`ACCESS_FINE_LOCATION`設定すると、粗いデータと細かい場所データの両方にアクセスできます。 両方のアクセス許可を設定する必要はありません。アプリが動作するために必要な*最小限*のアクセス許可のみを設定します。

このスニペットは、アプリが`ACCESS_FINE_LOCATION`アクセス許可に対するアクセス許可を持っているかどうかを確認する方法の例です。

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

アプリは、ユーザーがアクセス許可を付与しない (またはアクセス許可を失効させた) シナリオを許容し、その状況を適切に処理する方法を備えている必要があります。 Xamarin Android での実行時のアクセス許可チェックの実装の詳細については、[アクセス許可ガイド](~/android/app-fundamentals/permissions.md)を参照してください。


## <a name="using-the-fused-location-provider"></a>ヒューズを持つ場所プロバイダーの使用

デバイスから場所の更新を受信するために、デバイスから場所の更新を受信するために、ヒューズを使用することをお勧めします。これは、ベストロケーション情報をバッテリ効率の高い方法で提供するために、実行時に場所プロバイダーを効率的に選択するためです。 たとえば、屋外を歩いているユーザーが GPS を使用した最適な場所を取得します。 その後、GPS が正常に機能しない (ある場合は) 屋内でにステップ実行すると、屋内での機能が向上します。
 
ジオフェンシングとアクティビティの監視など、場所に対応したアプリケーションを支援するためのさまざまなツールを提供します。 このセクションでは、を設定`LocationClient`する方法、プロバイダーを確立する方法、およびユーザーの場所を取得する方法について重点的に説明します。

[Google Play 開発者サービス](https://developer.android.com/google/play-services/index.html)の一部としては、ヒューズが組み込まれています。
Google Play 開発者サービスパッケージをインストールし、アプリケーション内で適切に構成して、ヒューズの場所プロバイダー API が機能するようにする必要があります。また、デバイスに Google Play 開発者サービス APK がインストールされている必要があります。

Xamarin Android アプリケーションでは、 **GooglePlayServices**パッケージをプロジェクトに追加する必要がありますが、そのためには、そのプロバイダーを使用する必要があります。 さらに、以下に`using`示すクラスを参照するソースファイルには、次のステートメントを追加する必要があります。

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Google Play 開発者サービスがインストールされているかどうかを確認する

Google Play 開発者サービスがインストールされていない (または期限が切れている) ときに、使用していない場所プロバイダーを使用しようとすると、Xamarin はクラッシュします。  Google Play 開発者サービスがインストールされていない場合は、前に説明した Android ロケーションサービスにアプリケーションがフォールバックします。 Google Play 開発者サービスが古い場合は、インストールされているバージョンの Google Play 開発者サービスを更新するように求めるメッセージがアプリに表示される可能性があります。

このスニペットは、Google Play 開発者サービスがインストールされているかどうかを、Android アクティビティがプログラムによって確認する方法の例です。

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

ヒューズを持つ場所プロバイダーと対話するには、Xamarin アプリケーションにのインスタンス`FusedLocationProviderClient`が必要です。 このクラスは、位置情報の更新をサブスクライブするために必要なメソッドを公開し、デバイスの最後の既知の場所を取得します。

アクティビティの`FusedLocationProviderClient`メソッドは、次のコードスニペットに示すように、への参照を取得するための適切な場所です。 `OnCreate`

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

### <a name="getting-the-last-known-location"></a>最後の既知の場所を取得する

メソッド`FusedLocationProviderClient.GetLastLocationAsync()`は、最小限のコードオーバーヘッドで、デバイスの最後の既知の場所をすばやく取得する、単純な非ブロッキングの方法を提供します。

このスニペットは、メソッドを使用`GetLastLocationAsync`してデバイスの場所を取得する方法を示しています。

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

### <a name="subscribing-to-location-updates"></a>場所の更新のサブスクライブ

次のコードスニペットに示すように、Xamarin Android アプリケーションでは、 `FusedLocationProviderClient.RequestLocationUpdatesAsync`メソッドを使用して、ヒューズの場所プロバイダーからの場所の更新をサブスクライブすることもできます。

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```

このメソッドは、次の2つのパラメーターを受け取ります。

- **`Android.Gms.Location.LocationRequest`** オブジェクトと`LocationRequest`は、どのようにして、どのようにして、どのようにして Xamarin. Android アプリケーションがどのように機能するかについてのパラメーターです。 &ndash; は`LocationRequest` 、要求を頻繁に実行する方法や、正確な場所の更新が重要であるかなどの情報を保持します。 たとえば、重要な場所を要求すると、デバイスは GPS を使用し、その結果、場所を決定するときに電力が多くなります。 このコードスニペットは、高い精度で`LocationRequest`場所のを作成する方法を示しています。場所の更新については約5分ごとに確認しています (ただし、要求間の間隔が2分を超えていません)。 デバイスの場所を特定するとき`LocationRequest`に使用する場所プロバイダーのガイダンスとして、ヒューズが使用されます。

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```

- **`Android.Gms.Location.LocationCallback`** 場所の更新を受け取るために、Xamarin アプリケーションは`LocationProvider`抽象クラスをサブクラス化する必要があります。 &ndash; このクラスは2つのメソッドを公開しました。これは、場所情報を使用してアプリを更新するために、置き換えられる場所プロバイダーによって呼び出されることが 詳細については、以下で詳しく説明します。

場所の更新を Xamarin Android アプリケーションに通知するために、ヒューズの場所プロバイダーはを`LocationCallBack.OnLocationResult(LocationResult result)`呼び出します。 パラメーター `Android.Gms.Location.LocationResult`には、更新プログラムの場所の情報が含まれます。

ヒューズの位置情報プロバイダーが場所データの可用性の変化を検出すると、 `LocationProvider.OnLocationAvailability(LocationAvailability
locationAvailability)`メソッドが呼び出されます。 プロパティからが返さ`true`れた場合は、に`OnLocationResult`よって報告されたデバイスの場所の結果が、の`LocationRequest`必要に応じて正確かつ最新の状態であると見なすことができます。 `LocationAvailability.IsLocationAvailable` が`IsLocationAvailable` false の場合、場所の結果はによって`OnLocationResult`返されません。

このコードスニペットは、 `LocationCallback`オブジェクトの実装例です。

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

## <a name="using-the-android-location-service-api"></a>Android ロケーションサービス API の使用

Android ロケーションサービスは、Android で位置情報を使用するための古い API です。 場所データは、ハードウェアセンサーによって収集され、システムサービスによって収集され`LocationManager` `ILocationListener`ます。このサービスは、クラスとを使用してアプリケーション内でアクセスされます。

ロケーションサービスは、Google Play 開発者サービスがインストールされていないデバイスで実行する必要があるアプリケーションに最適です。

ロケーションサービスは、システムによって管理される特別な種類の[サービス](https://developer.android.com/guide/components/services.html)です。 システムサービスは、デバイスハードウェアと対話し、常に実行されます。 アプリケーションで位置情報の更新をタップするには、 `LocationManager` `RequestLocationUpdates`との呼び出しを使用して、システムロケーションサービスからの場所の更新をサブスクライブします。

Android ロケーションサービスを使用してユーザーの場所を取得するには、いくつかの手順を実行します。

1. `LocationManager`サービスへの参照を取得します。
2. 場所が変更されたときに、インターフェイスを実装し、イベントを処理します。`ILocationListener`
3. 指定し`LocationManager`たプロバイダーの場所の更新を要求するには、を使用します。 前`ILocationListener`の手順のは、 `LocationManager`からコールバックを受信するために使用されます。
4. アプリケーションが更新プログラムの受信に適していない場合に、場所の更新を停止します。

### <a name="location-manager"></a>ロケーションマネージャー

システムロケーションサービスには、 `LocationManager`クラスのインスタンスを使用してアクセスできます。 `LocationManager`は、システムロケーションサービスとやり取りし、そこでメソッドを呼び出すことができる特別なクラスです。 アプリケーションは、次に示すように`LocationManager` 、を`GetSystemService`呼び出し、サービスの種類を渡すことによってへの参照を取得できます。

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate`は、への`LocationManager`参照を取得するのに適した場所です。
をクラス変数として保持する`LocationManager`ことをお勧めします。これにより、アクティビティのライフサイクルのさまざまな時点でを呼び出すことができます。

### <a name="request-location-updates-from-the-locationmanager"></a>LocationManager からの場所の更新を要求する

アプリケーションにへの`LocationManager`参照がある場合、必要な場所情報の種類と、その情報が更新される頻度を`LocationManager`通知する必要があります。 これを行うに`RequestLocationUpdates`は、 `LocationManager`オブジェクトでを呼び出して、更新の条件と、場所の更新を受け取るコールバックを渡します。 このコールバックは、インターフェイスを実装する`ILocationListener`必要がある型です (詳細については、このガイドの後半で説明します)。

メソッド`RequestLocationUpdates`は、アプリケーションが位置情報の更新の受信を開始するようにシステムロケーションサービスに指示します。 このメソッドを使用すると、更新頻度を制御するための時間と距離のしきい値だけでなく、プロバイダーを指定することができます。 たとえば、次のメソッドでは、GPS ロケーションプロバイダーからの位置情報の更新を2000ミリ秒ごとに要求し、場所が複数のメートルに変更された場合にのみ、場所の更新を要求します。

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

アプリケーションでは、アプリケーションを正常に実行するために必要な場合にのみ、場所の更新を要求する必要があります。 これにより、バッテリの寿命が維持され、ユーザーのエクスペリエンスが向上します。

### <a name="responding-to-updates-from-the-locationmanager"></a>LocationManager からの更新への応答

アプリケーションがから`LocationManager`更新を要求すると、 [`ILocationListener`](xref:Android.Locations.ILocationListener)インターフェイスを実装することによって、サービスから情報を受信できます。 このインターフェイスには、ロケーションサービスとロケーションプロバイダー `OnLocationChanged`をリッスンするための4つのメソッドが用意されています。 場所の更新を`OnLocationChanged`要求したときに設定された条件に従って、ユーザーの場所が変更された場合、システムはを呼び出します。 

次のコードは、 `ILocationListener`インターフェイスのメソッドを示しています。

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

### <a name="unsubscribing-to-locationmanager-updates"></a>LocationManager の更新のサブスクライブを解除します

システムリソースを節約するために、アプリケーションはできるだけ早く場所の更新をサブスクライブ解除する必要があります。 メソッド`RemoveUpdates`は、アプリケーション`LocationManager`への更新プログラムの送信を停止するようにに指示します。  例として、アクティビティは`RemoveUpdates` `OnPause`メソッドでを呼び出すことができます。これにより、アプリケーションでは、アクティビティが画面上にないときに位置情報の更新が不要な場合に、電力を節約できるようになります。

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

バックグラウンドでアプリケーションが位置情報の更新を取得する必要がある場合は、システムロケーションサービスをサブスクライブするカスタムサービスを作成することをお勧めします。 詳細については、[バックグラウンド処理 With Android Services](~/android/app-fundamentals/services/index.md)ガイドを参照してください。

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>LocationManager の最適な場所プロバイダーの決定

上のアプリケーションは、GPS を場所プロバイダーとして設定します。 ただし、デバイスが屋内でである場合や GPS レシーバーがない場合など、どのような場合でも GPS は使用できないことがあります。 この場合、結果`null`はプロバイダーの戻り値になります。

GPS が使用できないときにアプリを動作させるには、 `GetBestProvider`メソッドを使用して、アプリケーションの起動時に最適な (デバイスでサポートされる) 場所プロバイダーに対して要求を行います。 特定のプロバイダーを渡す代わりに、プロバイダーの要件`GetBestProvider` (精度や電源[ `Criteria` ](xref:Android.Locations.Criteria)など) をオブジェクトに伝えることができます。 `GetBestProvider`指定された条件に最適なプロバイダーを返します。

次のコードは、使用可能なプロバイダーを取得し、場所の更新を要求するときに使用する方法を示しています。

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
> ユーザーがすべての場所プロバイダーを無効に`GetBestProvider`した`null`場合、はを返します。 実際のデバイスでこのコードがどのように動作するかを確認するには、次のスクリーンショットに示すように、[ **Google 設定 > 場所] > モード**で GPS、wi-fi、および携帯ネットワークを有効にする必要があります。
>
> [![Android フォンの設定場所モード画面](location-images/location-02.png)](location-images/location-02.png#lightbox)
>
> 次のスクリーンショットは、を使用し`GetBestProvider`て実行されている場所アプリケーションを示しています。
>
> [![緯度、経度、およびプロバイダーを表示する GetBestProvider アプリ](location-images/location-03.png)](location-images/location-03.png#lightbox)
>
> プロバイダーは動的に`GetBestProvider`変更されないことに注意してください。 代わりに、アクティビティのライフサイクル中に1回だけ、最適なプロバイダーを決定します。 プロバイダーの状態が設定された後で変更された場合、アプリケーションでは`ILocationListener` 、 &ndash; 、 `OnProviderDisabled`、および`OnStatusChanged` &ndash;の各メソッド`OnProviderEnabled`に関連するすべての可能性を処理するために、追加のコードが必要になります。プロバイダースイッチ。

## <a name="summary"></a>Summary

このガイドでは、Android ロケーションサービスと、Google Location Services API のヒューズを持つ場所プロバイダーの両方を使用したユーザーの場所の取得について説明します。

## <a name="related-links"></a>関連リンク

- [場所 (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/location)
- [FusedLocationProvider (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fusedlocationprovider)
- [Google Play 開発者サービス](https://developer.android.com/google/play-services/index.html)
- [Criteria クラス](xref:Android.Locations.Criteria)
- [LocationManager クラス](xref:Android.Locations.LocationManager)
- [LocationListener クラス](xref:Android.Locations.ILocationListener)
- [LocationClient API](https://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener API](https://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
