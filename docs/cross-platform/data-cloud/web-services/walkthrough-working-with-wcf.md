---
title: チュートリアル - WCF の使用
description: このチュートリアルでは、Xamarin でビルドしたモバイル アプリケーションが、BasicHttpBinding クラスを使用して WCF web サービスを使用する方法について説明します。
ms.prod: xamarin
ms.assetid: D0E83342-2E79-4D25-BD57-43718F5628C4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 02/17/2018
ms.openlocfilehash: 297aac4ba4a564e4506d841d3e11718ad79307e2
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/26/2018
---
# <a name="walkthrough---working-with-wcf"></a>チュートリアル - WCF の使用

_このチュートリアルでは、Xamarin でビルドしたモバイル アプリケーションが、BasicHttpBinding クラスを使用して WCF web サービスを使用する方法について説明します。_


これは、モバイル アプリケーション バックエンド システムと通信できるようにするための一般的な要件です。 多くの選択肢とオプションのうちの 1 つのバックエンド フレームワークがある[Windows Communication Foundation](http://msdn.microsoft.com/library/ms731082.aspx) (WCF)。 このチュートリアルには、Xamarin のモバイル アプリケーションを WCF サービスを使用して、使用方法の例を示します、`BasicHttpBinding`クラスです。 このチュートリアルには、次のトピックが含まれています。

1.  **WCF サービスを作成**-このセクションでは、2 つのメソッドを持つ非常に基本的な WCF サービスを作成しました。 ながら、他のメソッドは c# オブジェクトは、最初のメソッドを文字列パラメーターになります。 このセクションでは、WCF サービスへのリモート アクセスを許可する開発者のワークステーションを構成する方法についても説明されます。
1.  **Xamarin.Android アプリケーションを作成**の WCF サービスを作成すると、WCF サービスを使用する単純な Xamarin.Android アプリケーションが作成されます。 このセクションでは、WCF サービスとの通信を容易にするために WCF サービス プロキシ クラスを作成する方法について説明します。
1.  **Xamarin.iOS アプリケーションを作成する**-このチュートリアルの最後の部分では、WCF サービスを使用する簡単な Xamarin.iOS アプリケーションを作成します。

<a name="Requirements" />

## <a name="requirements"></a>要件

このチュートリアルでは、を作成して、WCF サービスを使用していくつかの知識があることを前提としています。

<a name="Creating_a_WCF_Service" />

## <a name="creating-a-wcf-service"></a>WCF サービスを作成します。

私たちの前に、最初のタスクでは、モバイル アプリケーションと通信するための WCF サービスを作成します。

1. Visual Studio 2017 を起動し、新しいプロジェクトを作成します。
1. **新しいプロジェクト**ダイアログで、選択、 **WCF > WCF サービス ライブラリ**テンプレート、およびソリューション名`HelloWorldService`:

    ![](walkthrough-working-with-wcf-images/new-wcf-service.png "新しい WCF サービス ライブラリを作成します。")

1. **ソリューション エクスプ ローラー**、という名前の新しいクラスを追加`HelloWorldData`をプロジェクトに。

    ```csharp
        using System.Runtime.Serialization;

        namespace HelloWorldService
        {
            [DataContract]
            public class HelloWorldData
            {
                [DataMember]
                public bool SayHello { get; set; }

                [DataMember]
                public string Name { get; set; }

                public HelloWorldData()
                {
                    Name = "Hello ";
                    SayHello = false;
                }
            }
        }
    ```


1. **ソリューション エクスプ ローラー**、名前を変更`IService1.cs`に`IHelloWorldService.cs`の名前を変更し、`Service1.cs`に`HelloWorldService.cs`です。
1. **ソリューション エクスプ ローラー**、開かれている`IHelloWorldService.cs`コードを次のコードに置き換えます。

    ```csharp
        using System.ServiceModel;

        namespace HelloWorldService
        {
            [ServiceContract]
            public interface IHelloWorldService
            {
                [OperationContract]
                string SayHelloTo(string name);

                [OperationContract]
                HelloWorldData GetHelloData(HelloWorldData helloWorldData);
            }
        }
    ```
  
    このサービスは、2 つのメソッドを提供します。 – .NET オブジェクトを受け取るいずれかのパラメーターと別の文字列を使用します。

1. **ソリューション エクスプ ローラー**、開かれている`HelloWorldService.cs`コードを次のコードに置き換えます。

    ```csharp
        using System;

        namespace HelloWorldService
        {
            public class HelloWorldService : IHelloWorldService
            {
                public HelloWorldData GetHelloData(HelloWorldData helloWorldData)
                {
                    if (helloWorldData == null)
                        throw new ArgumentException("helloWorldData");

                    if (helloWorldData.SayHello)
                        helloWorldData.Name = "Hello World to {helloWorldData.Name}";

                    return helloWorldData;
                }

                public string SayHelloTo(string name)
                {
                    return "Hello World to you, {name}";
                }
            }
        }
    ```

1. **ソリューション エクスプ ローラー**、開かれている`App.config`、更新、`name`の属性、 `<service>`  ノード、`contract`の属性、`<endpoint>`ノード、および`baseAddress`の属性`<add>`ノード。

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
            ...
            <services>
              <service name="HelloWorldService.HelloWorldService">
                <endpoint address="" binding="basicHttpBinding" contract="HelloWorldService.IHelloWorldService">
                  <identity>
                    <dns value="localhost" />
                  </identity>
                </endpoint>
                <endpoint address="mex" binding="mexHttpBinding" contract="IMetadataExchange" />
                <host>
                  <baseAddresses>
                    <add baseAddress="http://localhost:8733/Design_Time_Addresses/HelloWorldService/" />
                  </baseAddresses>
                </host>
              </service>
            </services>
            ...
        </configuration>
    ```

1. ビルドして、WCF サービスを実行します。 サービスは、WCF テスト クライアントでホストされます。

    ![](walkthrough-working-with-wcf-images/hosted-wcf-service.png "テスト クライアントで実行されている WCF サービス")

1. 実行して、WCF テスト クライアントでは、ブラウザーを起動し、WCF サービスのエンドポイントに移動します。

    ![](walkthrough-working-with-wcf-images/wcf-service-browser.png "WCF サービスのブラウザー情報 ページ")

> [!IMPORTANT]
> 次のセクションでは、必要なは、Windows 10 のワークステーション上のリモート接続を許可する必要がある場合のみです。 別のプラットフォームを WCF サービスを展開する必要がある場合は、セクションを無視できます。

<a name="Allow_Remote_Access_to_IIS_Express" />

## <a name="configuring-remote-access-to-iis-express"></a>IIS Express にリモート アクセスの構成

接続は、ローカル コンピューターからのみが取得される場合は、ローカルで WCF をホストするいると十分です。 ただし、リモート デバイス (Android デバイスまたは iPhone) などには、ローカルの WCF サービスへのアクセスはありません。 そのため、このセクションでは、リモート接続を受け入れるための Windows 10 と IIS Express を構成する方法について説明します。

1.  **IIS Express を構成を受け入れるリモート接続に**-この手順では、特定のポートでのリモート接続を受け入れるための IIS Express の構成ファイルの編集後の IIS Express で受信トラフィックを許可するルールを設定します。
1.  **Windows ファイアウォールに例外を追加**-リモート アプリケーションが WCF サービスと通信するために使用できる Windows ファイアウォールでポートを開く必要があります。

    ワークステーションの IP アドレスを知っている必要があります。 この例の目的はものと、ワークステーションが IP アドレス 192.168.1.143 を持っています。

1. 外部からの要求をリッスンするように IIS が Express の構成を見てみましょう。 IIS Express の構成ファイルを編集することによってこの作業を行うことができます`[solutiondirectory]\.vs\config\applicationhost.config`の次のスクリーン ショットに示すようにします。

    [![](walkthrough-working-with-wcf-images/image05.png "そのため、solutiondirectory.vsconfigapplicationhost.config に IIS Express の構成ファイルを編集してこのスクリーン ショットに示すように")](walkthrough-working-with-wcf-images/image05.png#lightbox)

    検索、`site`という名前の要素`HelloWorldWcfHost`です。 次の XML スニペットのように表示する必要があります。

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
            </bindings>
        </site>
    ```
 
    もう 1 つ追加する必要があります。`binding`を外部のトラフィックをポート 8734 を開きます。 次の XML を追加、`bindings`要素、IP アドレスを IP アドレスに置き換えます。

    ```xml
    <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
    ```
    
    コンピューターの外部 IP アドレスにポート 8734 でリモート IP アドレスからの HTTP トラフィックを受け入れるように IIS が Express に設定されます。 上のスニペットは、IIS Express を実行しているコンピューターの IP アドレスは 192.168.1.143 前提としています。 変更後に、`bindings`要素は次のようになります。

    ```xml
        <site name="HelloWorldWcfHost" id="2">
            <application path="/" applicationPool="Clr4IntegratedAppPool">
                <virtualDirectory path="/" physicalPath="\\vmware-host\Shared Folders\tom\work\xamarin\code\private-samples\webservices\HelloWorld\HelloWorldWcfHost" />
            </application>
            <bindings>
                <binding protocol="http" bindingInformation="*:8733:localhost" />
                <binding protocol="http" bindingInformation="*:8734:192.168.1.143" />
            </bindings>
        </site>
    ```

1. 次に、IIS Express を構成する必要があります 8734 のポートで着信接続を許可します。 スタートアップの管理コマンド プロンプトのセットアップし、このコマンドを実行します。

    `> netsh http add urlacl url=http://192.168.1.143:9608/ user=everyone`

1. 最後の手順では、ポート 8734 で外部のトラフィックを許可するように Windows ファイアウォールを構成します。 管理コマンド プロンプトから次のコマンドを実行します。

    `> netsh advfirewall firewall add rule name="IISExpressXamarin" dir=in protocol=tcp localport=8734 profile=private remoteip=localsubnet action=allow`

    このコマンドは、Windows 10 のワークステーションと同じサブネットにポート 8734 すべてのデバイスからの着信トラフィックを許可します。

IIS Express を他のデバイスや、サブネット上のコンピューターからの着信接続を受け入れるでホストされている非常に基本的な WCF サービスを作成します。 この出力をテストするにはアプリケーションを実行して、オフィスを訪れたによって`http://localhost:8733/Design_Time_Addresses/HelloWorldService/`ワークステーションと`http://192.168.1.143:8734/Design_Time_Addresses/HelloWorldService/`サブネット上の別のコンピューターからです。

IIS Express で実行されていると、サービスを提供する、オフに、**エディット コンティニュ**オプション*プロジェクト プロパティ > Web > デバッガー*です。

## <a name="creating-a-proxy-for-the-web-service"></a>Web サービスのプロキシを作成します。

アプリケーションは、サービスを利用できる前に、WCF サービスの web サービス プロキシを作成する必要があります。 これは次のようにして実装します。

1. 名前付き .NET の標準的なクラス ライブラリの追加`HelloWorldServiceProxy`、およびプロジェクト内の任意のクラスを削除します。
1. 実行、`HelloWorldService`プロジェクト。
1. `HelloWorldService`プロジェクト実行中に追加する新しい**接続済みサービスの**をプロジェクトを使用して、 **Microsoft WCF Web サービス参照プロバイダー**です。
1. **サービス エンドポイント**のタブ、 **WCF Web サービス参照の構成**ダイアログ ボックスで、をクリックして、 **Discover**ボタンを削除し、`mex`検出されたの末尾から内のエンドポイント、 **URI**ドロップダウン リストで、入力`HelloWorldServiceProxy`として、 **Namespace**、 をクリックし、 **次へ**ボタンをクリックします。
1. **データ型オプション**のタブ、 **WCF Web サービス参照の構成**ダイアログ ボックスで、 をクリックして、既定値のまま、 **次へ**ボタンをクリックします。
1. **クライアント オプション**のタブ、 **WCF Web サービス参照の構成**ダイアログ ボックスで、いることを確認、**パブリック** チェック ボックスを選択して、をクリックして、**完了**ボタンをクリックします。
1. ビルド、`HelloWorldServiceProxy`プロジェクト。

> [!NOTE]
> Visual Studio 2017 で Microsoft WCF Web サービス参照のプロバイダーを使用してプロキシを作成する代わりにでは、ServiceModel メタデータ ユーティリティ ツール (svcutil.exe) を使用します。 詳細については、次を参照してください。 [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)です。

<a name="Creating_a_Xamarin_Android_Application" />

## <a name="creating-a-xamarinandroid-application"></a>Xamarin.Android アプリケーションを作成します。

WCF サービスのプロキシは、Xamarin.Android のアプリケーションで使用することができます。

1. Visual Studio で、新しい空の Android プロジェクトをソリューションに追加し、名前`HelloWorld.Android`です。
1. `HelloWorld.Android`プロジェクトへの参照を追加、`HelloWorldServiceProxy`プロジェクト、およびへの参照、`System.ServiceModel`名前空間。
1. **ソリューション エクスプ ローラー**、開かれている`Resources/layout/main.axml`し既存の XML を次の XML に置き換えます。

    ```xml
        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
                  android:orientation="vertical"
                  android:layout_width="fill_parent"
                  android:layout_height="fill_parent">
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/sayHelloWorldButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/say_hello_world" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/sayHelloWorldTextView" />
            </LinearLayout>
            <LinearLayout
                    android:orientation="vertical"
                    android:layout_width="fill_parent"
                    android:layout_height="0px"
                    android:layout_weight="1">
                <Button
                        android:id="@+id/getHelloWorldDataButton"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:text="@string/get_hello_world_data" />
                <TextView
                        android:text="Large Text"
                        android:textAppearance="?android:attr/textAppearanceLarge"
                        android:layout_width="fill_parent"
                        android:layout_height="wrap_content"
                        android:id="@+id/getHelloWorldDataTextView" />
            </LinearLayout>
        </LinearLayout>
    ```
    
    次のスクリーン ショットは、デザイナーで UI を示しています。

    [![](walkthrough-working-with-wcf-images/image09.png "これは、デザイナーで、この UI はのようになりますのスクリーン ショット")](walkthrough-working-with-wcf-images/image09.png#lightbox)
    
1. **ソリューション エクスプ ローラー**、開かれている`Resources/values/Strings.xml`し、次の XML を追加します。

    ```xml
    <string name="say_hello_world">Say Hello World</string>
    <string name="get_hello_world_data">Get Hello World data</string>
    ```
    
1. **ソリューション エクスプ ローラー**、開かれている`MainActivity.cs`し既存のコードを次のコードに置き換えます。

    ```csharp
        [Activity(Label = "HelloWorld.Android", MainLauncher = true)]
        public class MainActivity : Activity
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");

            HelloWorldServiceClient _client;
            Button _getHelloWorldDataButton;
            TextView _getHelloWorldDataTextView;
            Button _sayHelloWorldButton;
            TextView _sayHelloWorldTextView;
            ...
        }
    ```

    置き換える`<insert_WCF_service_endpoint_here>`WCF エンドポイントのアドレスを使用します。

1. `MainActivity.cs`、変更、`OnCreate`次のコードを格納するためのメソッド。

    ```csharp
        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(bundle);

            SetContentView(Resource.Layout.Main);

            InitializeHelloWorldServiceClient();

            // This button will invoke the GetHelloWorldData - the method that takes a C# object as a parameter.
            _getHelloWorldDataButton = FindViewById<Button>(Resource.Id.getHelloWorldDataButton);
            _getHelloWorldDataButton.Click += GetHelloWorldDataButtonOnClick;
            _getHelloWorldDataTextView = FindViewById<TextView>(Resource.Id.getHelloWorldDataTextView);

            // This button will invoke SayHelloWorld - this method takes a simple string as a parameter.
            _sayHelloWorldButton = FindViewById<Button>(Resource.Id.sayHelloWorldButton);
            _sayHelloWorldButton.Click += SayHelloWorldButtonOnClick;
            _sayHelloWorldTextView = FindViewById<TextView>(Resource.Id.sayHelloWorldTextView);
        }
    ```
    
    上記のコードでは、クラスのインスタンス変数を初期化し、いくつかのイベント ハンドラーに接続します。

1. `MainActivity.cs`、次の 2 つのメソッドを追加することによって、クライアント プロキシ クラスをインスタンス化します。

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
    
    上記のコードは、インスタンス化し、初期化、`HelloWorldServiceClient`オブジェクト。

1. `MainActivity.cs`、2 つのボタンの偶数のハンドラーを追加、 `Activity`:

    ```csharp
        async void GetHelloWorldDataButtonOnClick(object sender, EventArgs e)
        {
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            _getHelloWorldDataTextView.Text = "Waiting for WCF...";
            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                _getHelloWorldDataTextView.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButtonOnClick(object sender, EventArgs e)
        {
            _sayHelloWorldTextView.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                _sayHelloWorldTextView.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```
  
1. アプリケーションを実行することを確認して、WCF サービスが実行されている、2 つのボタンをクリックします。 アプリケーションは、WCF を非同期で呼び出すこと、`Endpoint`フィールドが正しく設定されています。

    [![](walkthrough-working-with-wcf-images/image08.png "30 秒以内は、各 WCF メソッドから応答を受信して、アプリケーションはこのスクリーン ショットのようになります")](walkthrough-working-with-wcf-images/image08.png#lightbox)

<a name="Creating_a_Xamarin_iOS_Application" />

## <a name="creating-a-xamarinios-application"></a>Xamarin.iOS アプリケーションを作成します。

WCF サービス プロキシは、Xamarin.iOS のアプリケーションで使用できることができます。

1. Visual Studio で、追加新しい iPhone **1 つのビュー アプリケーション**をソリューションにプロジェクトおよび名前を付けます`HelloWorld.iOS`です。
1. `HelloWorld.iOS`プロジェクトへの参照を追加、`HelloWorldServiceProxy`プロジェクト、およびへの参照、`System.ServiceModel`名前空間。
1. **ソリューション エクスプ ローラー**をダブルクリックして`Main.storyboard`iOS デザイナーで、ファイルを開きます。 次のコードを追加し、`UIButton`と`UITextView`コントロール。

    ||名前|Title|
    |--- |--- |--- |
    |`UIButton`|`sayHelloWorldButton`|「こんにちは, World」を言う|
    |`UITextView`|`sayHelloWorldText`||
    |`UIButton`|`getHelloWorldDataButton`|「こんにちは, World」を取得するデータ|
    |`UITextView`|`getHelloWorldDataText`||

    コントロールを追加した後、UI は、次のスクリーン ショットをようになります。

    [![](walkthrough-working-with-wcf-images/image12.png "UI は、コントロールを追加すると、このスクリーン ショットのようになります")](walkthrough-working-with-wcf-images/image12.png#lightbox)

1. **ソリューション エクスプ ローラー**、開かれている`ViewController.cs`し、次のコードを追加します。

    ```xml
        public partial class ViewController : UIViewController
        {
            static readonly EndpointAddress Endpoint = new EndpointAddress("<insert_WCF_service_endpoint_here>");
            HelloWorldServiceClient _client;
            ...
        }
    ```
  
    置き換える`<insert_WCF_service_endpoint_here>`WCF エンドポイントのアドレスを使用します。

1. `ViewController.cs`、更新、`ViewDidLoad`メソッドが、次のようになります。

    ```csharp
        public override void ViewDidLoad()
        {
            base.ViewDidLoad();
            InitializeHelloWorldServiceClient();

            getHelloWorldDataButton.TouchUpInside += GetHelloWorldDataButton_TouchUpInside;
            sayHelloWorldButton.TouchUpInside += SayHelloWorldButton_TouchUpInside;
        }
    ```
  
1. `ViewController.cs`、追加、`InitializeHelloWorldServiceClient`と`CreateBasicHttpBinding`メソッド。

    ```csharp
        void InitializeHelloWorldServiceClient()
        {
            BasicHttpBinding binding = CreateBasicHttpBinding();
            _client = new HelloWorldServiceClient(binding, Endpoint);
        }

        static BasicHttpBinding CreateBasicHttpBinding()
        {
            BasicHttpBinding binding = new BasicHttpBinding
            {
                Name = "basicHttpBinding",
                MaxBufferSize = 2147483647,
                MaxReceivedMessageSize = 2147483647
            };

            TimeSpan timeout = new TimeSpan(0, 0, 30);
            binding.SendTimeout = timeout;
            binding.OpenTimeout = timeout;
            binding.ReceiveTimeout = timeout;
            return binding;
        }
    ```
  
1. `ViewController.cs`、イベント ハンドラーを追加、 `TouchUpInside` 、2 つのイベント`UIButton`インスタンス。

    ```csharp
        async void GetHelloWorldDataButton_TouchUpInside(object sender, EventArgs e)
        {
            getHelloWorldDataText.Text = "Waiting for WCF...";
            var data = new HelloWorldData
            {
                Name = "Mr. Chad",
                SayHello = true
            };

            HelloWorldData result;
            try
            {
                result = await _client.GetHelloDataAsync(data);
                getHelloWorldDataText.Text = result.Name;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }

        async void SayHelloWorldButton_TouchUpInside(object sender, EventArgs e)
        {
            sayHelloWorldText.Text = "Waiting for WCF...";
            try
            {
                var result = await _client.SayHelloToAsync("Kilroy");
                sayHelloWorldText.Text = result;
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.Message);
            }
        }
    ```

1. アプリケーションを実行することを確認して、WCF サービスが実行されている、2 つのボタンをクリックします。 アプリケーションは、WCF を非同期で呼び出すこと、`Endpoint`フィールドが正しく設定されています。

    [![](walkthrough-working-with-wcf-images/image10.png "30 秒以内は、各 WCF メソッドから応答を受信して、アプリケーションはこのスクリーン ショットのようになります")](walkthrough-working-with-wcf-images/image10.png#lightbox)

<a name="Summary" />

## <a name="summary"></a>まとめ

このチュートリアルでは、Xamarin.iOS と Xamarin.Android を使用してモバイル アプリケーションで WCF サービスを操作する方法について説明します。 WCF サービスを作成する方法を示しましたし、リモート デバイスから接続を受け入れるための Windows 10 と IIS Express を構成する方法を説明しました。 WCF クライアント プロキシを生成する方法を説明し、Xamarin.iOS と Xamarin.Android の両方のアプリケーションで、プロキシのクライアントを使用する方法を説明します。


## <a name="related-links"></a>関連リンク

- [HelloWorld (サンプル)](https://developer.xamarin.com/samples/mobile/WCF-Walkthrough/)
- [WCF でのサービス指向アプリケーションの開発](https://docs.microsoft.com/dotnet/framework/wcf/index)
- [方法: Windows Communication Foundation クライアントを作成](https://docs.microsoft.com/dotnet/framework/wcf/how-to-create-a-wcf-client)
- [ServiceModel メタデータ ユーティリティ ツール (svcutil.exe)](https://docs.microsoft.com/dotnet/framework/wcf/servicemodel-metadata-utility-tool-svcutil-exe)
