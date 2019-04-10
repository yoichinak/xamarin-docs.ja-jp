---
title: チュートリアル - Xamarin.iOS でのバック グラウンド場所
description: このドキュメントでは、backgrounded Xamarin.iOS アプリケーションで場所情報を使用する方法のチュートリアルを提供します。 これは、必要なセットアップ、ユーザー インターフェイス、およびアプリケーションの状態について説明します。
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: fa8a48e165764a449af4bc5414d2e66aecea8269
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870145"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>チュートリアル - Xamarin.iOS でのバック グラウンド場所

この例では、iOS に現在の場所に関する情報を出力する場所のアプリケーションを構築するつもりが: 緯度、経度、および画面の他のパラメーター。 このアプリケーションでは、アプリケーションの アクティブ または Backgrounded 中に、場所の更新プログラムを正しく実行する方法について説明します。

このチュートリアルで説明キーが必要なバック グラウンド アプリケーションとしてアプリの登録、アプリが backgrounded、ときに、UI の更新を中断して、処理などの概念をバック グラウンド処理によって、`WillEnterBackground`と`WillEnterForeground``AppDelegate`メソッド.

## <a name="application-set-up"></a>アプリケーションを設定します。


1. 最初に、作成、新しい**iOS > アプリ > Single View Application (C#)** します。 呼び出す_場所_iPad および iPhone の両方が選択されていることを確認してください。

1. 場所のアプリケーションは、iOS での必要なバック グラウンド アプリケーションとして修飾します。 編集して、アプリケーションを場所として、アプリケーションを登録、 **Info.plist**プロジェクトのファイル。

    ソリューション エクスプ ローラーをダブルクリックします。、 **Info.plist**ファイルを開き、一覧の一番下までスクロールします。 両方で、チェックを配置、**バック グラウンド モードを有効にする**と**場所の更新**チェック ボックスをオンします。

    Visual studio for Mac では、このようなもののように表示されます。

    [![](location-walkthrough-images/image7.png "バック グラウンド モードを有効にして、位置情報の更新のチェック ボックスの両方でチェック ボックスをオンします。")](location-walkthrough-images/image7.png#lightbox)

    Visual Studio で、 **Info.plist**次のキー/値ペアを追加することで手動で更新する必要があります。

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. これで、アプリケーションが登録されたデバイスから場所データを取得ができます。 Ios で、`CLLocationManager`クラス場所の情報にアクセスするために使用し、場所の更新プログラムを提供するイベントを発生させることができます。

1. コードという新しいクラスを作成`LocationManager`さまざまな画面と場所の更新をサブスクライブするコードの 1 つの場所を提供します。 `LocationManager`クラスのインスタンス、`CLLocationManager`と呼ばれる`LocMgr`:

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

    上記のコードでのさまざまなプロパティとアクセス許可の設定、 [CLLocationManager](xref:CoreLocation.CLLocationManager)クラス。

    - `PausesLocationUpdatesAutomatically` – これは、ブール値に応じて、システムの場所の更新プログラムを一時停止が許可されたかどうかを設定できます。 一部のデバイスを既定`true`、バック グラウンド約 15 分後に位置情報の更新の取得を停止するデバイスが発生することができます。
    - `RequestAlwaysAuthorization` -アプリのユーザーに、バック グラウンドでアクセスできる場所を許可するオプションを提供するには、このメソッドを渡す必要があります。 `RequestWhenInUseAuthorization` アプリがフォア グラウンドである場合にのみアクセスする場所を許可するオプションをユーザーに付与する場合も渡されることができます。
    - `AllowsBackgroundLocationUpdates` – これは、受信場所の更新プログラムが中断されたときにアプリを許可する設定できる iOS 9 で導入された、ブール型プロパティです。

    > [!IMPORTANT]
    > iOS 8 (以降) では、内のエントリも必要です、 **Info.plist**承認要求の一部として、ユーザーを表示するファイル。

1. キーを追加する`NSLocationAlwaysUsageDescription`または`NSLocationWhenInUseUsageDescription`場所データへのアクセスを要求するアラートでユーザーに表示される文字列を使用します。

1. iOS 9 いる必要がありますを使用する場合`AllowsBackgroundLocationUpdates`、 **Info.plist** 、キーが含まれて`UIBackgroundModes`値を持つ`location`します。 このチュートリアルの手順 2 完了している場合は、既にある Info.plist file にされています。


1. 内で、`LocationManager`クラス、メソッドを作成`StartLocationUpdates`を次のコード。 このコードは、受信場所からの更新を開始する、 `CLLocationManager`:

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

    このメソッドで行われているいくつかの重要な点があります。 最初に、アプリケーションがデバイス上の場所のデータへのアクセスにはが含まれているのチェックを実行します。 これが呼び出すことによって確認`LocationServicesEnabled`上、`CLLocationManager`します。 このメソッドが返す**false**場合は、ユーザーがアプリケーションの位置情報へのアクセスを拒否します。

1. 次に、更新するロケーション マネージャーをどのくらいの頻度を伝えます。 `CLLocationManager` フィルター処理および更新の頻度を含む、場所データを構成するためには、多くのオプションを提供します。 この例では、設定、`DesiredAccuracy`メーターによって場所が変更されるたびに更新します。 場所の更新の頻度およびその他の設定の構成の詳細についてを参照してください、 [CLLocationManager クラス参照](https://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html)Apple のドキュメントで。

1. 最後に、呼び出す`StartUpdatingLocation`上、`CLLocationManager`インスタンス。 これにより、location manager の更新プログラムの送信を開始して、現在の場所に初期の修正プログラムを取得するには

これまでにロケーション マネージャーが作成されたら、受け取るには、必要なデータの種類で構成されている最初の場所を決定しました。 これで、コードは、場所データをユーザー インターフェイスを表示するために必要があります。 カスタム イベントを受け取るとそのため、`CLLocation`を引数として。

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

位置情報の更新をサブスクライブする次の手順では、 `CLLocationManager`、させてカスタム`LocationUpdated`場所データを新しい場所を引数として渡して使用可能になるときにイベント。 これを行うには、新しいクラスを作成**LocationUpdateEventArgs.cs**します。 このコードは、メイン アプリケーション内でアクセス可能であり、イベントが発生したときに、デバイスの場所を返します。

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

1. IOS デザイナーを使用すると、位置情報を表示する画面を作成します。 ダブルクリックして、 **Main.storyboard**を開始するファイル。

    、ストーリー ボード上の複数のラベルを場所情報のプレース ホルダーとして機能する画面にドラッグします。 この例では、緯度、経度、高度、コース、および速度のラベルです。

    レイアウトは、次のようになります。

    ![](location-walkthrough-images/image8.png "IOS Designer の UI レイアウトの例")

1. Solution Pad でダブルクリックして、`ViewController.cs`ファイルを開き、LocationManager と呼び出しの新しいインスタンスを作成するように編集`StartLocationUpdates`にします。
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

    データは表示されませんが、アプリケーションの起動時に、位置情報の更新が開始されます。

1. これで、場所の更新プログラムを受信した場所情報を画面を更新します。 次のメソッドが元の場所を取得、`LocationUpdated`イベントと、UI に表示。

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

購読を依頼する必要があります、 `LocationUpdated` AppDelegate と UI を更新するには、新しいメソッドを呼び出してイベント。 次のコードを追加`ViewDidLoad,`直後に、`StartLocationUpdates`呼び出し。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


ここで、アプリケーションを実行するになりますこのようなもの。

[![](location-walkthrough-images/image5.png "アプリの実行例")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>アクティブおよびバック グラウンドの状態の処理

1. アプリケーションがフォア グラウンドでとアクティブになっている場所の更新を出力します。 アプリがバック グラウンドに入ったときの動作を示すためには、オーバーライド、`AppDelegate`アプリケーションを追跡するメソッド状態が変化するため、アプリケーションがフォア グラウンドとバック グラウンドの間を遷移するとき、コンソールに書き込みます。

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

    次のコードを追加、`LocationManager`継続的に更新された場所を印刷するデータをアプリケーション出力の場所情報を確認するには、バック グラウンドで引き続き使用できます。

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

1. コードを使用して、残り 1 つの問題があります。 アプリが backgrounded ときに、UI を更新しようとしていますが原因の iOS が終了します。 アプリは、バック グラウンドになったら、コードは、位置情報の更新の登録を解除し、UI の更新を停止する必要があります。

    iOS により、通知、アプリが別のアプリケーションに移行するときの状態。 この場合を購読できる、`ObserveDidEnterBackground`通知します。

    次のコード スニペットでは、通知を使用して、ビューの UI の更新を中止する時期を把握する方法を示します。 これは、予定`ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    アプリが実行されているときに、このようなものを出力になります。

    ![](location-walkthrough-images/image6.png "コンソール内の場所の出力の例")

1. アプリケーションでは、フォア グラウンドで動作しているときに、画面に位置情報の更新を出力し、引き続きバック グラウンドで動作中に、アプリケーションの出力ウィンドウにデータを印刷します。

未解決の問題が 1 つだけが残ります。 画面が、アプリが最初に読み込まれるが、ときに、アプリがフォア グラウンド再入力を知る方法がないときに、UI の更新を開始します。 Backgrounded アプリケーションは、フォア グラウンドに戻されましたが、UI の更新は再開されません。

これを解決するには、アプリケーションがアクティブな状態になるとアラートが発生する別の通知内の UI の更新プログラムを開始する呼び出しを入れ子に。

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

これで、UI、アプリケーションが最初に起動し、アプリを更新するには、いつでも再開がフォア グラウンドに戻ったときに更新が開始されます。

このチュートリアルでは、画面と、アプリケーションの出力ウィンドウの両方に場所データを出力する適切に動作、バック グラウンドに対応した iOS アプリケーションを構築しました。


## <a name="related-links"></a>関連リンク

- [場所 (パート 4) (サンプル)](https://developer.xamarin.com/samples/monotouch/Location/)
- [中核となる場所のフレームワーク参照](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
