---
title: ライブの再読み込み
description: 別のコンパイルを必要とせず、XAML への変更が実行中、反映を表示し、展開します。
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
ms.date: 04/23/2018
ms.openlocfilehash: bfb53af420b64fb9af994d3fb19293406d3acd7b
ms.sourcegitcommit: 180a8411d912de40545f9624e2127a66ee89e7b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/30/2018
---
# <a name="xamarin-live-reload"></a>Xamarin ライブの再読み込み

![[プレビュー]](~/media/shared/preview.png)

Xamarin のライブの再読み込みを有効にすると、 **、XAML を変更および実行中、別のコンパイルを必要とせずに反映してデプロイ**です。 XAML に加えられた変更は再デプロイで保存され、配置ターゲットに反映されます。

アプリはライブの再読み込みを使用するときにコンパイルされているために、すべてのライブラリおよびサード パーティ製コントロールで使用できます。 Xamarin.Forms は、Android、iOS、UWP などをサポートし、シミュレーター、エミュレーター、および物理デバイスを含む、すべての有効な配置ターゲット上で動作のすべてのプラットフォームで再読み込み動作をライブします。

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

ライブの再読み込みは、現在 Visual Studio 2017 の使用のみ。

## <a name="requirements"></a>要件

* [Visual Studio 2017 15.7 Preview 4](https://www.visualstudio.com/vs/preview/)以上で、 **.NET を使用したモバイル開発**ワークロード。
* [Xamarin.Forms 3.0.354232-pre3](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3)またはそれ以降。

## <a name="getting-started"></a>作業の開始
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1.Visual Studio Marketplace からライブ再読み込みの Xamarin をインストールします。

Xamarin のライブの再読み込みは、Visual Studio Marketplace 経由で分散されます。 拡張機能をインストールするを参照してください。、 [Visual Studio Marketplace での Xamarin のライブの再読み込み](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload) web サイトとクリック**ダウンロード**です。

ダウンロードされ、をクリックする .vsix を開く**インストール**です。

![Visual Studio インストーラー Xamarin Live 再読み込みの確認](images/LiveReloadVSIXInstall.png)

代わりでの情報を検索することができます、**オンライン** タブで、**拡張機能と更新プログラム**Visual Studio 内のダイアログ。

### <a name="2-configure-your-app-to-use-live-reload"></a>2.ライブの再読み込みを使用するアプリを構成します。

既存のモバイル アプリへのライブの再読み込みの追加は、3 つの手順で実行できます。

1. すべてのプロジェクトが更新を使用することを確認[Xamarin.Forms 3.0.354232-pre3](https://www.nuget.org/packages/Xamarin.Forms/3.0.0.354232-pre3)またはそれ以降。
2. インストール、 **Xamarin.LiveReload** NuGet .NET 標準 2.0 ライブラリにします。 これは、プラットフォームのプロジェクトにインストールする必要はありません。 いることを確認、**パッケージ ソース**に設定されている**すべて**です。

![Xamarin ライブの再読み込み NuGet NuGet Package Manager での追加します。](images/addlivereloadnuget.png)

3. 追加`LiveReload.Init();`でコンス トラクターに、`Application`クラスに、次のコード スニペットに示すようにします。

```csharp
public partial class App : Application
{
    public App ()
    {
        // Initialize Live Reload.
        LiveReload.Init();
    
        InitializeComponent();
        MainPage = new MainPage();
    }
}
```

### <a name="3-start-live-reloading"></a>3.ライブの再読み込みを開始します。

コンパイルし、アプリケーションを配置します。 アプリを配置した場合は、XAML ファイルを開く、いくつか変更を行い、ファイルを保存します。 変更内容は、配置ターゲットを再展開します。

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

任意の XAML ファイルへの変更で再読み込み動作をライブします。 C# の場合や、NuGet パッケージの追加/削除の変更は、新しいビルドが必要ですしを有効にする展開します。

## <a name="frequently-asked-questions"></a>よく寄せられる質問 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Xamarin Live 再読み込みは Mac を Visual Studio で使用できますか。 

Xamarin のライブの再読み込みの初期のプレビュー リリースは Visual Studio 2017 をできるだけです。 Mac 用の Visual Studio のサポートは、今後のリリースで予定です。

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>これは、に対してプリズムなど、すべてのライブラリですか。 

アプリがコンパイルされているために、ライブの再読み込みはプリズム、およびサード パーティ製コントロール ライブラリなど、Telerik、Infragistics、Syncfusion、ArcGIS、GrapeCity、およびその他のコントロール ベンダーなどのすべてのライブラリで動作します。

### <a name="what-changes-does-live-reload-redeploy"></a>ライブの再読み込みに再配置どのような変更 

ライブの再読み込みには、XAML に加えられた変更のみ適用されます。 C# ファイルを変更する場合に再コンパイルが必要になります。 再読み込みする (C#) のサポートについては、今後のリリース計画します。

### <a name="what-platforms-are-supported"></a>どのようなプラットフォームがサポートされますか。 

ライブの再読み込みは、Android、iOS、UWP など、Xamarin.Forms でサポートされている任意のプラットフォームで動作します。

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>エミュレーターやシミュレーターが、物理デバイスの操作手順は? 

[はい]、ライブの再読み込みは、Android エミュレーター、iOS シミュレーターでは、物理デバイスなど、すべての有効な配置ターゲットで動作します。 デバイスに展開するには、デバイスとコンピューターが同じ Wi-fi ネットワークに属していることが必要です。

### <a name="does-this-work-with-corporate-networks"></a>これは、に対しては企業ネットワークか。

Android エミュレーターまたは iOS シミュレーターをデバッグしている場合、ライブの再読み込みは、通信するために localhost を使用します。 デバイスに展開する場合は、デバイスとコンピューターを同じの Wi-fi ネットワーク上にある必要があります。 できない場合は、可能なシナリオで実行できる[独自の再読み込みをライブ サーバーを構成する](#live-reload-server)、することができます Live は再読み込みするネットワーク接続の設定に関係なく。

### <a name="does-it-require-debugging-the-app"></a>これは、アプリのデバッグが必要ですか。 

いいえ。 実際には、任意の数のデバイスまたはシミュレーターまたはエミュレーターですべて、サポートされているアプリケーションがターゲット (Android、iOS、および UWP) を開始し、それらすべてを一度に更新を確認することができますも。 

## <a name="limitations"></a>制限事項

* XAML のみが再読み込みはサポートされています。
* Visual Studio でのみサポートされます。
* .NET 標準ライブラリでのみ動作します。
* CSS スタイル シートはサポートされていません。
* MVVM を使用していない限り、UI の状態を再配置します。、間では維持されません可能性があります。
* アプリ全体のリソースを再読み込みする (つまり**App.xaml**またはリソース ディクショナリを共有)、アプリのナビゲーションをリセットします。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="app-doesnt-connect"></a>アプリにアクセスできなかった

アプリケーションのビルド時からの情報**ツール > オプション > Xamarin > ライブの再読み込み**(ホスト名、ポート、および暗号化キー) に埋め込まれているアプリ、そのため時に`LiveReload.Init()`ペアリングまたは構成がありませんが、実行が接続が成功するために必要です。

標準ネットワーク上の問題 (ファイアウォール、別のネットワーク上のデバイス)、以外の主な理由は、アプリが正常に接続できない IDE は、Visual Studio での 1 つの構成が異なるためです。 これは、場合に発生する可能性があります。

* アプリは、別のコンピューターでコンパイルされました。
* アプリケーションがコンパイルされ、別の Visual Studio セッションで展開および**暗号化キーの自動生成**は (既定値) をチェックイン**ツール > オプション > Xamarin > ライブの再読み込み**です。
* Visual Studio の設定には (つまりホスト名、ポート、または暗号化キー) が変更されましたが、アプリがビルドされず、再度展開します。

このような場合は、すべてのビルドと、アプリをもう一度展開して解決されます。

### <a name="uninstalling-preview-1"></a>Preview 1 をアンインストールします。

古いプレビューがあり、これをアンインストールする問題がある場合は、次の手順に従います。

1. フォルダーを削除**C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (注:"Enterprise"をインストールされているエディションに置き換えるし、「プレビュー」と「2017」します。安定した VS にインストール)
2. 開く、**開発者コマンド プロンプト**Visual Studio と実行の`devenv /updateconfiguration`します。 

## <a name="tips--tricks"></a>ヒントとコツ

* ライブの再読み込みの設定を変更しない限り、(場合など、暗号化キーを含むをオフにする**暗号化キーの自動生成**) と同じコンピューターからビルド、ビルドおよび初期の後に、アプリを展開する必要はありませんコードまたは依存関係を変更しない限りを展開します。 だけを実行してもう一度、以前に配置されたアプリと接続を最後に使用するホスト。

* Visual Studio の同じセッションに接続できるデバイスの数に制限はありません。 展開し、必要に応じて、同時にそれらのすべてでライブの再読み込み動作を表示する、同数のデバイスまたはシミュレーターでアプリを起動できます。

* ライブの再読み込みされます、アプリのユーザー インターフェイス部分を再読み込みのみが、*いない*ページを再作成もが、それぞれ置き換えますビュー モデル (またはバインディング コンテキスト)。 つまり、*全体*アプリの状態が再読み込み、挿入された依存関係を含む常に保持されます。

## <a name="live-reload-server"></a>ライブの再読み込みサーバー

シナリオで、実行中のアプリからコンピューターへの接続 (を使用して示された`localhost`または`127.0.0.1`で**ツール > オプション > Xamarin > ライブの再読み込み**) ことはできません (ファイアウォール、別のネットワークなど)できますリモート サーバーを構成する代わりに、IDE と、アプリの両方がこれに接続します。

ライブの再読み込みは、標準を使用して[MQTT プロトコル](http://mqtt.org/)を交換するメッセージ、およびと通信できるため[サード パーティ サーバー](https://github.com/mqtt/mqtt.github.io/wiki/servers)です。 でもは[公開サーバー](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (とも呼ばれる*ブローカー*) を使用することができます。 ライブの再読み込みがテストされて`broker.hivemq.com`と`iot.eclipse.org`によって提供されるサービスと同様に、ホスト名[www.cloudmqtt.com](https://www.cloudmqtt.com)と[www.cloudamqp.com](https://www.cloudamqp.com)です。など、クラウド内の独自の MQTT サーバーを展開することもできます。 [azure HiveMQ](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud)または[AWS でな MQ](http://www.rabbitmq.com/ec2.html)です。 

任意のポートを構成することができますが、リモート サーバーの既定の 1883 ポートを使用するが一般的です。 ライブの再読み込みメッセージは、リモート サーバーに接続しても安全であるために、強力なエンド ツー エンド AES 対称暗号化を使用します。 既定では、暗号化キーと初期化ベクター (IV) の両方が Visual Studio のすべてのセッションに再生成します。

インストールする最も簡単な方法は、おそらく、 [mosquitto](https://mosquitto.org) Azure 内の空白 Ubuntu VM 内のサーバー。

1. Azure ポータルで新しい Ubuntu Server VM を作成します。
2. [ネットワーク] タブで 1883 (MQTT ポートの既定値) に対して新しい受信ポート規則を追加します。
3. 開く、[クラウド シェル](https://docs.microsoft.com/azure/cloud-shell/overview)(bash モード)
4. 入力`ssh [USERNAME]@[PUBLIC_IP]`1 で選択したユーザー名を使用して) と、VM の概要ページに表示されるパブリック IP
5. 実行`sudo apt-get install mosquitto`1 で選択したパスワードを入力)

今すぐ MQTT サーバーへの接続にその IP を使用できます。
