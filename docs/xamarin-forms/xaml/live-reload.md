---
title: Xamarin Live Reload (プレビュー)
description: 別のコンパイルとデプロイを必要とせずに、XAML に対する変更をライブで反映します。
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
robots: noindex
ms.date: 10/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b67594e2675c774512f3bf64f2e91ef10dbff444
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84134213"
---
# <a name="xamarin-live-reload-preview"></a>Xamarin Live Reload (プレビュー)

> [!NOTE]
> Xamarin Live Reload のプレビューが終了しました。フィードバックとコメントについては、すべてのユーザーに感謝します。 
>
> アプリの実行中に XAML を編集するには、[の Xamarin.Forms Xaml ホットリロード](~/xamarin-forms/xaml/hot-reload.md)を使用します。
>

Xamarin Live Reload を使用する**と、XAML を変更してライブに反映することができ、別のコンパイルとデプロイは必要**ありません。 XAML に加えられたすべての変更は保存時に再デプロイされ、デプロイターゲットに反映されます。

## <a name="requirements"></a>要件

* .NET ワークロード**を使用したモバイル開発**のある[Visual Studio 2017 バージョン15.7 以降](https://visualstudio.microsoft.com/vs/)。
* [ Xamarin.Forms 3.0.0 以上](https://www.nuget.org/packages/Xamarin.Forms/)。

## <a name="getting-started"></a>作業の開始
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1. Visual Studio Marketplace から Xamarin Live Reload をインストールする

Xamarin Live Reload は、Visual Studio Marketplace 経由で配布されます。 拡張機能をインストールするには、Visual Studio Marketplace web サイトの[Xamarin ライブリロードページに](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload)アクセスし、[**ダウンロード**] をクリックします。

ダウンロードされた .vsix を開き、[**インストール**] をクリックします。

![Visual Studio インストーラー Xamarin ライブ再読み込みの確認](images/LiveReloadVSIXInstall.png)

または、Visual Studio 内の [**拡張機能と更新プログラム**] ダイアログの [**オンライン**] タブで検索することもできます。

### <a name="2-configure-your-app-to-use-live-reload"></a>2. ライブリロードを使用するようにアプリを構成する

既存のモバイルアプリにライブ再読み込みを追加するには、次の3つの手順を実行します。

1. すべてのプロジェクトが[ Xamarin.Forms 3.0.0 以上](https://www.nuget.org/packages/Xamarin.Forms/)を使用するように更新されていることを確認します。

2. **LiveReload** NuGet パッケージを追加します。

    a. **.NET Standard** – .NET Standard 2.0 ライブラリに**LiveReload** NuGet をインストールします。 これは、プラットフォームプロジェクトにインストールする必要はありません。 **パッケージソース**が**All**に設定されていることを確認します。
    
    b. **共有プロジェクト**–すべてのプラットフォームプロジェクト (Android、IOS、UWP など) に**LiveReload** NuGet をインストールします。 **パッケージソース**が**All**に設定されていることを確認します。

    [![NuGet パッケージマネージャーを使用して Xamarin Live Reload NuGet を追加する](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. `LiveReload.Init();` `Application` 次のコードスニペットに示すように、クラスのコンストラクターにを追加します。

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        #if DEBUG
        LiveReload.Init();
        #endif
        
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3. ライブリロードを開始する

アプリケーションをコンパイルして配置します。 アプリが展開されたら、XAML ファイルを開き、いくつかの変更を行い、ファイルを保存します。 変更は配置ターゲットに再展開されます。

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

ライブ再読み込みは、XAML ファイルに対する変更と共に機能します。 C# に変更を加えたり、NuGet パッケージを追加または削除したりするには、新しいビルドと配置を有効にする必要があります。

## <a name="frequently-asked-questions"></a>よく寄せられる質問 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Xamarin Live Reload は Visual Studio for Mac で利用できますか? 

いいえ、Xamarin Live Reload のプレビューリリースは、Visual Studio 2017 でのみ使用できます。

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>これは、Prism などのすべてのライブラリで動作しますか。 

アプリがコンパイルされるため、ライブ再読み込みは、Prism などのすべてのライブラリと、Telerik、Infragistics、Syncfusion、ArcGIS、GrapeCity、およびその他のコントロールベンダーなどのサードパーティ製のコントロールライブラリと共に機能します。

### <a name="what-changes-does-live-reload-redeploy"></a>ライブリロードによって再デプロイされる変更 

ライブ再読み込みは、XAML または CSS に加えられた変更のみを適用します。 C# ファイルに変更を加える場合は、再コンパイルが必要になります。 

### <a name="what-platforms-are-supported"></a>サポートされているプラットフォーム 

ライブリロードは、でサポートされているすべてのプラットフォーム Xamarin.Forms (Android、iOS、UWP を含む) で動作します。

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>これはエミュレーター、シミュレーター、および物理デバイスで動作しますか。 

はい。ライブリロードは、Android エミュレーター、iOS シミュレーター、物理デバイスなど、すべての有効なデプロイターゲットで動作します。 デバイスへの展開では、デバイスとコンピューターが同じ Wi-fi ネットワーク上にある必要があります。

### <a name="does-this-work-with-corporate-networks"></a>これは企業ネットワークで動作しますか。

Android エミュレーターまたは iOS シミュレーターにデバッグしている場合は、ライブ再読み込みで localhost を使用して通信します。 デバイスに展開する場合は、デバイスとコンピューターが同じ Wi-fi ネットワーク上にある必要があります。 これが不可能なシナリオでは、独自の[ライブ再読み込みサーバーを構成](#live-reload-server)することができます。これにより、ネットワーク接続の設定に関係なく、ライブ再読み込みが可能になります。

### <a name="does-it-require-debugging-the-app"></a>アプリのデバッグが必要ですか。 

いいえ。 実際には、サポートされているすべてのアプリケーションターゲット (Android、iOS、UWP) を任意の数のデバイスまたはシミュレーター/エミュレーターで起動し、一度にすべての更新を確認することもできます。 

## <a name="limitations"></a>制限事項

* XAML の再読み込みのみがサポートされています。
* MVVM を使用しない限り、再デプロイ間で UI の状態を維持することはできません。

## <a name="known-issues"></a>の既知の問題

* Visual Studio でのみサポートされています。
* リンクは、リンクし**ない**か、**フレームワーク Sdk のみをリンク**しないように設定する必要があります 
* アプリ全体のリソース (つまり、 **app.xaml**または共有リソースディクショナリ) を再読み込みすると、アプリのナビゲーションがリセットされます。 
* 現在、ContentView を再読み込みするには、含んでいるページを再読み込みする必要があります。
* AutomationId を含む要素によって、再読み込みエラーが発生する可能性があります。
* UWP のデバッグ中に XAML を編集すると、ランタイムクラッシュが発生する可能性があります。 回避策:**デバッグの開始 (F5**キー) ではなく、**デバッグなしで開始 (Ctrl + F5)** を使用します。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="error-codes"></a>エラー コード

* **XLR001**:*現在のプロジェクトは ' xamarin. LiveReload ' NuGet パッケージバージョン ' [version] ' を参照していますが、Xamarin Live Reload 拡張機能にはバージョン ' [version] ' が必要です。*

  ライブリロード機能を迅速に反復して進化させるには、NuGet パッケージと Visual Studio 拡張機能が完全に一致している必要があります。 NuGet パッケージを、インストールした拡張機能と同じバージョンに更新します。

* **XLR002**:*ライブ再読み込みには、コマンドラインからビルドするときに少なくとも ' MqttHostname ' プロパティが必要です。または、' EnableLiveReload ' を ' false ' に設定して、この機能を無効にします。*

  ライブ再読み込みに必要なプロパティは、コマンドラインからのビルド (または継続的インテグレーション) では使用できません。したがって、を明示的に指定する必要があります。 

* **XLR003**:*ライブリロード NuGet パッケージをインストールするには、Xamarin Live reload Visual Studio 拡張機能をインストールする必要があります。*

  ライブリロード NuGet パッケージを参照するプロジェクトをビルドしようとしましたが、ビジュアル拡張機能がインストールされていません。  

* *アセンブリの読み込み中に例外が発生しました: FileNotFoundException: アセンブリ ' 0.3.27.0, Version =, Culture = ニュートラル, PublicKeyToken = ' を読み込むことができませんでした。*

  ホストプロジェクトは、 `PackageReference` の代わりにを使用する必要があります。`packages.config`

### <a name="app-doesnt-connect"></a>アプリが接続していません

アプリケーションがビルドされると、ツール > オプションからの情報 ( **Xamarin > ライブ再読み込み >** (ホスト名、ポート、暗号化キー) がアプリに埋め込まれます。これにより、 `LiveReload.Init()` 接続を正常に実行するために、ペアリングや構成は必要ありません。

通常のネットワークの問題 (ファイアウォール、別のネットワーク上のデバイス) 以外は、アプリが IDE に正常に接続できないという主な理由は、その構成が Visual Studio の構成と異なるためです。 これは、次の場合に発生する可能性があります。

* アプリが別のコンピューターでコンパイルされました。
* アプリは別の Visual Studio セッションでコンパイルおよび展開されており、[ツール > オプション] の [**暗号化キーの自動生成**] がオンになっています (既定)。 **Xamarin > ライブ再読み込み >** ます。
* Visual Studio の設定が変更されました (ホスト名、ポート、または暗号化キー) が、アプリがビルドされずに再展開されませんでした。

これらのケースはすべて、アプリを再度ビルドしてデプロイすることで解決されます。

### <a name="uninstalling-preview-1"></a>プレビュー1をアンインストールしています

以前のプレビューを使用していて、アンインストールで問題が発生した場合は、次の手順を実行します。

1. フォルダー **C:\Program files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload**を削除します (注: "Enterprise" をインストールされているエディションに置き換え、"Preview" を "2017" に置き換えます (安定した VS にインストールした場合)。
2. その Visual Studio の**開発者コマンドプロンプト**を開き、を実行 `devenv /updateconfiguration` します。 

## <a name="tips--tricks"></a>ヒント & コツ

* ライブ再読み込みの設定が変更されない限り (暗号化キーの**自動生成**をオフにした場合など)、同じコンピューターからビルドする場合は、コードまたは依存関係を変更しない限り、初期デプロイ後にアプリをビルドして展開する必要はありません。 以前にデプロイされたアプリを再起動するだけで、最後に使用したホストに接続できます。

* 同じ Visual Studio セッションに接続できるデバイスの数に制限はありません。 アプリは、必要な数のデバイス/シミュレーターでデプロイおよび開始できます。これにより、ライブ再読み込みがすべて同時に実行されます。

* ライブ再読み込みでは、アプリのユーザーインターフェイス部分のみが再読み込みされますが、ページは再作成され*ません*。また、ビューモデル (またはバインドコンテキスト) を置き換えることもありません。 つまり、挿入された依存関係を含め、アプリ*全体*の状態は常に再読み込み中に保持されます。

## <a name="live-reload-server"></a>ライブ再読み込みサーバー

実行中のアプリからコンピューターへの接続 ( `localhost` または `127.0.0.1` [**ツール > オプション] の [Xamarin > ライブ再読み込み >**) を使用できない場合 (ファイアウォールや別のネットワークなど) は、代わりに、IDE とアプリの両方が使用するリモートサーバーを構成することができます。

ライブ再読み込みでは、標準の[MQTT プロトコル](https://mqtt.org/)を使用してメッセージを交換するため、[サードパーティサーバー](https://github.com/mqtt/mqtt.github.io/wiki/servers)と通信できます。 使用可能な[パブリックサーバー](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (*ブローカー*とも呼ばれます) もあります。 ライブ再読み込みは、 `broker.hivemq.com` および `iot.eclipse.org` ホスト名と、 [www.cloudmqtt.com](https://www.cloudmqtt.com)および[www.cloudamqp.com](https://www.cloudamqp.com)によって提供されるサービスを使用してテストされています。 また、 [Azure 上の Hivemq](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud)など、独自の MQTT サーバーをクラウドにデプロイすることもできます。

任意のポートを構成できますが、リモートサーバーに既定の1883ポートを使用するのが一般的です。 ライブ再読み込みメッセージでは、強力なエンドツーエンドの AES 対称暗号化が使用されるため、リモートサーバーに安全に接続できます。 既定では、暗号化キーと初期化ベクター (IV) は、すべての Visual Studio セッションで再生成されます。

最も簡単な方法は、Azure の空の Ubuntu VM に[mosquitto](https://mosquitto.org)サーバーをインストールすることです。

1. Azure Portal での新しい Ubuntu Server VM の作成
2. [ネットワーク] タブで 1883 (既定の MQTT ポート) の新しい受信ポートの規則を追加します。
3. [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview)を開きます (bash モード)
4. 「 `ssh [USERNAME]@[PUBLIC_IP]` 1」で選択したユーザー名を使用) と、VM の [概要] ページに表示されるパブリック IP を入力します。
5. `sudo apt-get install mosquitto`を実行し、1で選択したパスワードを入力します)

これで、その IP を使用して、独自の MQTT サーバーに接続できるようになりました。
