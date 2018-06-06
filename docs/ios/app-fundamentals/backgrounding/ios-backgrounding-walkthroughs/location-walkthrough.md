---
title: チュートリアル - Xamarin.iOS でバック グラウンドの場所
description: このドキュメントでは、backgrounded Xamarin.iOS アプリケーションで場所情報を使用する方法のチュートリアルを提供します。 これは、必要なセットアップ、ユーザー インターフェイス、およびアプリケーションの状態について説明します。
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: aef39ef435bbbad6f643b2376832d8f8132d6a4c
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784095"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>チュートリアル - Xamarin.iOS でバック グラウンドの場所

この例では、ここを現在の場所に関する情報を出力する場所のアプリケーションに、iOS をビルド: 緯度、経度、およびその他のパラメーターを画面にします。 このアプリケーションは、アプリケーションがアクティブまたは Backgrounded のいずれかの場所の更新プログラムが正しく実行する方法をデモンストレーションします。

このチュートリアルに必要なバック グラウンド アプリケーションとしてアプリを登録するとき、アプリが backgrounded、UI の更新を中断して、処理などの概念を backgrounding いくつかのキーがについて説明します、`WillEnterBackground`と`WillEnterForeground``AppDelegate`メソッド.

## <a name="application-set-up"></a>アプリケーション設定


1. 最初に、新しい作成**iOS > アプリ > 1 つのビュー アプリケーション (c#)** です。 それを呼び出す_場所_iPad および iPhone の両方が選択されていることを確認してください。

1. 場所のアプリケーションは、iOS で必要なバック グラウンド アプリケーションとして修飾します。 編集することによって、場所アプリケーションとしてアプリケーションを登録、 **Info.plist**のプロジェクト ファイルです。

    ソリューション エクスプ ローラーで、をダブルクリックして、 **Info.plist**ファイルを開くと、一覧の一番下までスクロールします。 両方で、チェックを配置、**バック グラウンド モードを有効にする**と**場所の更新**チェック ボックスをオンします。

    Mac 用 Visual Studio のように表示されます。

    [![](location-walkthrough-images/image7.png "バック グラウンド モードを有効にして、場所の更新プログラムのチェック ボックスの両方でチェック ボックスをオンします。")](location-walkthrough-images/image7.png#lightbox)

    Visual Studio で、 **Info.plist**次のキー/値ペアを追加することによって手動で更新する必要があります。

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. これでアプリケーションを登録すると、場所データ、デバイスからアクセスできます。 IOS、`CLLocationManager`クラスの場所の情報にアクセスするために使用し、場所の更新を提供するイベントを発生させることができます。

1. コードという新しいクラスを作成する`LocationManager`さまざまな画面および場所の更新をサブスクライブするコードの 1 つの場所を提供します。 `LocationManager`クラスのインスタンスを作成、`CLLocationManager`と呼ばれる`LocMgr`:

    ```csharp
    public class LocationManager
    {
        protected CLLocationManager locMgr;

        public LocationManager () {
            this.locMgr = new CLLocationManager();
            this.locMgr.PausesLocationUpdatesAutomatically = false;

            // iOS 8 has additional permissions requirements
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                locMgr.RequestAlwaysAuthorization (); // works in background
                //locMgr.RequestWhenInUseAuthorization (); // only in foreground
            }

            if (UIDevice.CurrentDevice.CheckSystemVersion (9, 0)) {
                locMgr.AllowsBackgroundLocationUpdates = true;
            }
        }

        public CLLocationManager LocMgr {
            get { return this.locMgr; }
        }
    }
    ```

    上記のコードでは、さまざまなプロパティとアクセス許可を設定で、 [CLLocationManager](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/)クラス。

    - `PausesLocationUpdatesAutomatically` – これは、システムの場所の更新プログラムを一時停止が許可されたかどうかに応じて設定可能なブール値です。 一部のデバイスで、既定値は`true`デバイスを停止するバック グラウンドに約 15 分後の場所の更新プログラムが取得される可能性があります。
    - `RequestAlwaysAuthorization` -ユーザーに提供する、アプリに、バック グラウンドでアクセスできる場所を許可するオプションは、このメソッドを渡す必要があります。 `RequestWhenInUseAuthorization` アプリがフォア グラウンドである場合にのみアクセスできる場所を許可するオプションをユーザーに付与する場合も渡されることができます。
    - `AllowsBackgroundLocationUpdates` – これは、iOS 9 を中断されている場合、場所の更新を受け取るようアプリを許可する設定できるで導入された、ブール型プロパティです。

    > [!IMPORTANT]
    > iOS 8 (以降) では、内のエントリも必要です、 **Info.plist**承認要求の一部として、ユーザーを表示するファイル。

1. キーを追加する`NSLocationAlwaysUsageDescription`または`NSLocationWhenInUseUsageDescription`場所データへのアクセスを要求する警告のユーザーに表示される文字列を使用します。

1. iOS 9 する必要がありますを使用する場合`AllowsBackgroundLocationUpdates`、 **Info.plist**は、キーを含む`UIBackgroundModes`値を持つ`location`します。 このチュートリアルの手順 2、これは既に完了している場合、Info.plist のファイルが使用されます。


1. 内部、`LocationManager`クラス、メソッドを作成`StartLocationUpdates`を次のコード。 このコードは、受信場所からの更新を開始する、 `CLLocationManager`:

    ```csharp
    if (CLLocationManager.LocationServicesEnabled) {
        //set the desired accuracy, in meters
        LocMgr.DesiredAccuracy = 1;
        LocMgr.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) =>
        {
            // fire our custom Location Updated event
            LocationUpdated (this, new LocationUpdatedEventArgs (e.Locations [e.Locations.Length - 1]));
        };
        LocMgr.StartUpdatingLocation();
    }
    ```

    このメソッドで行われているいくつかの重要な点があります。 最初に、アプリケーションがデバイス上の場所のデータへのアクセスを含まれているかどうか、チェックを実行します。 これを呼び出すことによって確認お`LocationServicesEnabled`上、`CLLocationManager`です。 このメソッドは**false**場合は、ユーザーがアプリケーションの場所の情報へのアクセスを拒否しました。

1. 次に、更新する場所のマネージャーをどのくらいの頻度を伝えます。 `CLLocationManager` フィルター処理して、更新の頻度を含め、場所データを構成するためには、多くのオプションを提供します。 この例では、設定、`DesiredAccuracy`によって、メーター値の場所を変更するたびに更新します。 場所の更新頻度やその他の設定を構成する方法についてを参照してください、 [CLLocationManager クラス参照](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html)Apple のドキュメントでします。

1. 最後に、呼び出す`StartUpdatingLocation`上、`CLLocationManager`インスタンス。 こうことを現在の場所に初期の修正プログラムを取得および更新の送信を開始する場所マネージャー

これまでに location manager が作成されたら、受信するデータの種類を使用して構成し、最初の場所が決定されます。 今すぐコードは、ユーザー インターフェイスに、場所データを表示するために必要があります。 カスタム イベントを受け取るにこの作業を行うことができます、`CLLocation`を引数として。

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

場所の更新をサブスクライブする次の手順では、 `CLLocationManager`、カスタムを発生させると`LocationUpdated`イベントを新しい場所データ利用可能になったら、場所を引数として渡します。 これを行うには、新しいクラスを作成**LocationUpdateEventArgs.cs**です。 このコードは、メイン アプリケーション内でアクセス可能であり、イベントが発生したときに、デバイスの場所を返します。

```csharp
public class LocationUpdatedEventArgs : EventArgs
{
    CLLocation location;

    public LocationUpdatedEventArgs(CLLocation location)
    {
       this.location = location;
    }

    public CLLocation Location
    {
       get { return location; }
    }
}
```

## <a name="user-interface"></a>ユーザー インターフェイス

1. IOS デザイナーを使用すると、位置情報を表示する画面を構築します。 ダブルクリックして、 **Main.storyboard**を開始するファイル。

    ストーリー ボード上の複数のラベルを場所情報のプレース ホルダーとして機能する画面にドラッグします。 この例では、緯度、経度、高度、コース、および速度のラベルがあります。

    レイアウトは、次のようになります。

    ![](location-walkthrough-images/image8.png "IOS デザイナーで UI レイアウトの例")

1. パッドでは、ソリューション、ダブルクリック、 `ViewController.cs` LocationManager と呼び出しの新しいインスタンスを作成するために編集してください。`StartLocationUpdates`にします。
  次のようにコードを変更します。

    ```csharp
    #region Computed Properties
    public static bool UserInterfaceIdiomIsPhone {
                get { return UIDevice.CurrentDevice.UserInterfaceIdiom == UIUserInterfaceIdiom.Phone; }
            }

    public static LocationManager Manager { get; set;}
    #endregion

    #region Constructors
    public ViewController (IntPtr handle) : base (handle)
    {
    // As soon as the app is done launching, begin generating location updates in the location manager
        Manager = new LocationManager();
        Manager.StartLocationUpdates();
    }

    #endregion
    ```

    場所の更新データは表示されませんが、アプリケーションの起動時に開始されます。

1. これで、場所の更新は、受信した場所情報を使用して画面を更新します。 次のメソッドが元の場所を取得、`LocationUpdated`イベントされ、UI に表示します。

    ```csharp
    #region Public Methods
    public void HandleLocationChanged (object sender, LocationUpdatedEventArgs e)
    {
        // Handle foreground updates
        CLLocation location = e.Location;

        LblAltitude.Text = location.Altitude + " meters";
        LblLongitude.Text = location.Coordinate.Longitude.ToString ();
        LblLatitude.Text = location.Coordinate.Latitude.ToString ();
        LblCourse.Text = location.Course.ToString ();
        LblSpeed.Text = location.Speed.ToString ();

        Console.WriteLine ("foreground updated");
    }
    #endregion
    ```

サブスクライブする必要があります、 `LocationUpdated` AppDelegate と UI を更新するには、新しいメソッドを呼び出してイベント。 次のコードを追加`ViewDidLoad,`直後に、`StartLocationUpdates`呼び出し。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


ここで、アプリケーションの実行時になりますが次のようにします。

[![](location-walkthrough-images/image5.png "実行のサンプル アプリ")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>アクティブおよびバック グラウンドの状態の処理

1. アプリケーションがフォア グラウンドでとアクティブになっている場所の更新を出力します。 アプリは、バック グラウンドになりときの動作を示すためには、上書き、`AppDelegate`フォア グラウンドとバック グラウンドの間を遷移するときに、アプリケーションがコンソールに出力できるようにアプリケーションを追跡する方法状態が変化します。

    ```csharp
    public override void DidEnterBackground (UIApplication application)
    {
        Console.WriteLine ("App entering background state.");
    }

    public override void WillEnterForeground (UIApplication application)
    {
        Console.WriteLine ("App will enter foreground");
    }
    ```

    次のコードを追加、`LocationManager`を継続的に更新された場所を印刷するを場所情報を確認する、アプリケーションの出力データは、バック グラウンドで引き続き使用できます。

    ```csharp
    public class LocationManager
    {
        public LocationManager ()
        {
        ...
        LocationUpdated += PrintLocation;
        }
        ...

        //This will keep going in the background and the foreground
        public void PrintLocation (object sender, LocationUpdatedEventArgs e) {
        CLLocation location = e.Location;
        Console.WriteLine ("Altitude: " + location.Altitude + " meters");
        Console.WriteLine ("Longitude: " + location.Coordinate.Longitude);
        Console.WriteLine ("Latitude: " + location.Coordinate.Latitude);
        Console.WriteLine ("Course: " + location.Course);
        Console.WriteLine ("Speed: " + location.Speed);
        }
    }
    ```

1. コードを使用して、残り 1 つの問題があります: アプリが backgrounded ときに、UI を更新しようとは原因 iOS は終了します。 アプリは、バック グラウンドになったら、コードは、場所の更新の登録を解除し、停止、UI を更新する必要があります。

    iOS により、通知、アプリが別のアプリケーションに移行するときの状態。 この例を購読できます、`ObserveDidEnterBackground`通知します。

    次のコード スニペットは、通知を使用して、ビューに UI の更新プログラムを停止するタイミングを認識する方法を示しています。 これは、予定`ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    アプリが実行中は、出力は次のようになります。

    ![](location-walkthrough-images/image6.png "コンソール内の場所の出力の例")

1. アプリケーションは、フォア グラウンドで動作しているときに画面に場所の更新を出力し、バック グラウンドで動作しているときにアプリケーションの出力ウィンドウにデータを印刷する続行します。

1 つだけの未解決の問題が残ります。 画面が、アプリが最初に読み込まれたが、アプリは再入力されている場合、前景色を知ることがないときに、UI の更新を開始します。 Backgrounded アプリケーションがフォア グラウンドに戻る場合は、UI の更新は再開されません。

これを解決するには、アプリケーションがアクティブな状態になったときに起動されますが、別の通知内の UI の更新プログラムを起動する呼び出しを入れ子にします。

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

今すぐ UI では、更新、アプリケーションが最初に起動、および再開をいつでも、アプリの更新は、フォア グラウンドに戻ったときに開始されます。

このチュートリアルで、画面とアプリケーションは、出力ウィンドウの両方に、場所データを出力する適切に動作、バック グラウンドに対応する iOS アプリケーションを構築します。


## <a name="related-links"></a>関連リンク

- [場所 (パート 4) (サンプル)](https://developer.xamarin.com/samples/monotouch/Location/)
- [コアの場所のフレームワーク参照](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
