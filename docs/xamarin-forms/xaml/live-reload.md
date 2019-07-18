---
title: Xamarin のライブの再読み込み (プレビュー)
description: XAML への変更はもう 1 つのコンパイルを必要とせず、ライブ、反映を確認し、デプロイします。
ms.prod: xamarin
ms.assetid: 4917273d-32f9-401a-a52c-5cfb53a2170d
ms.technology: xamarin-forms
author: pierceboggan
ms.author: piboggan
robots: noindex
ms.date: 10/26/2018
ms.openlocfilehash: 21ff09f2af93ee46578b959111bf744ba05a74d7
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61384934"
---
# <a name="xamarin-live-reload-preview"></a>Xamarin のライブの再読み込み (プレビュー)

> [!NOTE]
> Xamarin Live Reload のプレビューが終了しの意見やフィードバックは、すべてのユーザーに感謝いたします。 通してください、[ロードマップ](https://docs.microsoft.com/visualstudio/productinfo/vs-roadmap)の詳細については、Xamarin.Forms に取り組んでいますが、新しい生産性機能は Visual Studio 2019 します。 この拡張機能では、Visual Studio 2017 で利用可能な残りますが、今後の更新プログラムを受信するされません。

Xamarin Live の再読み込みを使用する **、XAML に変更を加えると別のコンパイルを必要とせず、ライブ、反映それらを表示してデプロイ**します。 XAML に加えられた変更が再デプロイで保存し、配置ターゲットに反映されます。

## <a name="requirements"></a>必要条件

* [Visual Studio 2017 バージョン 15.7 以降](https://visualstudio.microsoft.com/vs/)で、 **.NET によるモバイル開発**ワークロード。
* [Xamarin.Forms 3.0.0 以降](https://www.nuget.org/packages/Xamarin.Forms/)します。

## <a name="getting-started"></a>作業の開始
### <a name="1-install-xamarin-live-reload-from-the-visual-studio-marketplace"></a>1.Visual Studio Marketplace からライブの再読み込みの Xamarin をインストールします。

Xamarin Live の再読み込みは、Visual Studio Marketplace 経由で分散されます。 拡張機能をインストールするを参照してください。、 [Visual Studio Marketplace での Xamarin Live Reload ページ](https://marketplace.visualstudio.com/items?itemName=Xamarin.XamarinLiveReload)web サイトをクリックします**ダウンロード**します。

ダウンロードされ、をクリックすると、.vsix を開く**インストール**します。

![Visual Studio インストーラーを Xamarin Live の再読み込みの確認](images/LiveReloadVSIXInstall.png)

検索する代わりに、**オンライン** タブで、**拡張機能と更新**Visual Studio 内でのダイアログ。

### <a name="2-configure-your-app-to-use-live-reload"></a>2.ライブの再読み込みを使用するアプリを構成します。

既存のモバイル アプリにライブの再読み込みを追加することは、3 つの手順で実行できます。

1. 使用するすべてのプロジェクトが更新されるよう[Xamarin.Forms 3.0.0 以降](https://www.nuget.org/packages/Xamarin.Forms/)以降。

2. 追加、 **Xamarin.LiveReload** NuGet パッケージ。

    a.  **.NET standard** – インストール、 **Xamarin.LiveReload** NuGet を .NET Standard 2.0 ライブラリにします。 これは、プラットフォーム プロジェクトにインストールする必要はありません。 いることを確認、**パッケージ ソース**に設定されている**すべて**します。
    
    b.  **共有プロジェクト**– インストール、 **Xamarin.LiveReload**プラットフォームのすべてのプロジェクトに NuGet (Android、iOS、UWP などなど)。 いることを確認、**パッケージ ソース**に設定されている**すべて**します。

    [![Xamarin のライブの再読み込み NuGet では、NuGet パッケージ マネージャーに追加します。](images/addlivereloadnuget.w157-sml.png)](images/addlivereloadnuget.w157.png#lightbox)

3. 追加`LiveReload.Init();`コンス トラクターに、`Application`クラスに、次のコード スニペットに示すようにします。

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

### <a name="3-start-live-reloading"></a>3.ライブの再読み込みを開始します。

コンパイルし、アプリケーションをデプロイします。 アプリが、デプロイされていると、XAML ファイルを開き、いくつかの変更を加えるおよびファイルを保存します。 変更は、配置ターゲットを再デプロイします。

> [!Video https://www.youtube.com/embed/-5WJZpeXlC8]

任意の XAML ファイルへの変更で再読み込み動作をライブにします。 (C#) または NuGet パッケージの追加/削除の変更は、新しいビルドを必要とし、デプロイを有効にします。

## <a name="frequently-asked-questions"></a>よく寄せられる質問 
### <a name="is-xamarin-live-reload-available-on-visual-studio-for-mac"></a>Mac の Xamarin Live Reload Visual Studio で使用できるは、ですか。 

いいえ、Xamarin Live Reload のプレビュー リリースは、Visual Studio 2017 のできるだけです。

### <a name="does-this-work-with-all-libraries-such-as-prism"></a>これで、Prism など、すべてのライブラリは動作はしますか。 

アプリがコンパイルされるため、ライブの再読み込みは、Prism、およびサード パーティ製のコントロール ライブラリなど、Telerik、Infragistics、Syncfusion、ArcGIS、GrapeCity、およびその他のコントロール ベンダーなどのすべてのライブラリで動作します。

### <a name="what-changes-does-live-reload-redeploy"></a>ライブの再読み込みに再デプロイの変更点 

ライブの再読み込みには、XAML や CSS に加えられた変更のみ適用されます。 C# ファイルを変更する場合、再コンパイルが必要になります。 

### <a name="what-platforms-are-supported"></a>どのようなプラットフォームがサポートされますか。 

ライブの再読み込みは、Android、iOS、UWP など、Xamarin.Forms でサポートされる任意のプラットフォームで動作します。

### <a name="does-this-work-on-emulators-simulators-and-physical-devices"></a>エミュレーターやシミュレーター、物理デバイスの操作手順は? 

はい、ライブの再読み込みは、Android エミュレーター、iOS シミュレーターでは、物理デバイスなど、すべての有効なデプロイ ターゲットで動作します。 デバイスへのデプロイでは、デバイスとコンピューターが同じ Wi-fi ネットワークにあることが必要です。

### <a name="does-this-work-with-corporate-networks"></a>これの企業ネットワークでは動作はしますか。

Android エミュレーターまたは iOS シミュレーターをデバッグする場合、ライブの再読み込みは通信するために localhost を使用します。 デバイスに展開する場合は、デバイスとコンピューターを同じ Wi-fi ネットワークに配置する必要があります。 できない場合は、考えられるシナリオで[Live Reload サーバー構成](#live-reload-server)、することができますをライブの再読み込みでは、ネットワーク接続の設定に関係なく。

### <a name="does-it-require-debugging-the-app"></a>これは、アプリのデバッグが必要ですか。 

いいえ。 実際には、でも任意の数のデバイスまたはシミュレーターまたはエミュレーターですべてサポートされているアプリケーションの対象 (Android、iOS、および UWP) を開始して、すべてを一度に更新できます。 

## <a name="limitations"></a>制限事項

* XAML の再読み込みだけがサポートされています。
* UI の状態は可能性があります MVVM を使用していない場合の再デプロイの間で保持されません。

## <a name="known-issues"></a>既知の問題

* Visual Studio でのみサポートされます。
* 設定する必要がありますリンク**リンクしない**または**リンク フレームワーク Sdk のみ** 
* アプリ全体にわたるリソースを再読み込み (つまり**App.xaml**またはリソース ディクショナリの共有)、アプリのナビゲーションをリセットします。 
* 現在 ContentView の再読み込みするには、格納されているページを再読み込みする必要があります。
* AutomationId を含む要素の再読み込みエラーが発生する可能性があります。
* ランタイム クラッシュが発生する可能性があります UWP のデバッグ中には、XAML を編集します。 回避策:使用 **(Ctrl + F5) をデバッグなしで開始**の代わりに**デバッグ (F5) で開始**します。

## <a name="troubleshooting"></a>トラブルシューティング

### <a name="error-codes"></a>エラー コード

* **XLR001**:*現在のプロジェクト参照 'Xamarin.LiveReload' NuGet パッケージのバージョン [バージョン] が、Xamarin Live Reload 拡張機能には、バージョン '[VERSION]' が必要です。*

  迅速な反復処理しをライブの再読み込み機能の進化を許可するには、nuget パッケージと Visual Studio 拡張機能に一致します。 Nuget パッケージをインストールした拡張機能の同じバージョンに更新します。

* **XLR002**:*ライブの再読み込みに必要な最小 'MqttHostname' プロパティ、コマンドラインからビルドするときにします。また、この機能を無効にするには、'false' に 'EnableLiveReload' を設定します。*

  ときにライブの再読み込みに必要なプロパティは使用できません、コマンドラインから (または継続的インテグレーション) でのビルドとため明示的に指定する必要があります。 

* **XLR003**:*ライブの再読み込みの nuget パッケージでは、Xamarin Live Reload Visual Studio 拡張機能をインストールする必要があります。*

  ライブの再読み込みの nuget パッケージを参照するプロジェクトをビルドしようとしていますが、ビジュアルの拡張機能がインストールされていません。  

* *アセンブリの読み込み中に例外:System.IO.FileNotFoundException:アセンブリを読み込むことができません ' Xamarin.Live.Reload、バージョン = 0.3.27.0、Culture = neutral, PublicKeyToken ='。*

  ホスト プロジェクトを使用する必要があります`PackageReference`の代わりに `packages.config`

### <a name="app-doesnt-connect"></a>接続していないアプリ

ときに、アプリケーションがビルドからの情報**ツール > オプション > Xamarin > ライブの再読み込み**ので (ホスト名、ポート、および暗号化キー) をアプリでは、埋め込まれたときに`LiveReload.Init()`ペアリングや構成はありませんが、実行が接続が成功するために必要です。

通常ネットワークの問題 (ファイアウォール、別のネットワーク上のデバイス)、以外主な理由は、アプリが IDE を正常に接続できませんが、Visual Studio で 1 つの構成が異なるためです。 これは、場合に発生する可能性があります。

* アプリは、別のコンピューターでコンパイルされました。
* アプリがコンパイルされ、別の Visual Studio セッションでのデプロイと**暗号化キーの自動生成**は (既定値) をチェックイン**ツール > オプション > Xamarin > Live Reload**。
* Visual Studio の設定は (つまりホスト名、ポート、または暗号化キー) が変更されたが、アプリが構築され再度配置されません。

このような場合は、すべてのビルドと、アプリをもう一度展開して解決されます。

### <a name="uninstalling-preview-1"></a>Preview 1 をアンインストールします。

以前のプレビューがあり、アンインストールの問題がある場合は、次の手順に従います。

1. フォルダーを削除**C:\Program Files (x86) \Microsoft Visual Studio\Preview\Enterprise\Common7\IDE\Extensions\Xamarin\LiveReload** (注:"Enterprise"をインストールされているエディションで置き換え、"Preview"場合は「2017」にします。安定した VS にインストール)
2. 開く、**開発者コマンド プロンプト**Visual Studio と実行の`devenv /updateconfiguration`します。 

## <a name="tips--tricks"></a>ヒントと秘訣

* ライブの再読み込みの設定を変更しない限り、(場合など、暗号化キーなどをオフにする**暗号化キーの自動生成**) と同じコンピューターからビルドする、ビルドおよび初期の後に、アプリをデプロイする必要はありませんコードまたは依存関係を変更しない限りにデプロイします。 だけを表示するもう一度以前展開したアプリと接続、最後に使用されるホスト。

* 同じ Visual Studio セッションに接続できるデバイスの数に制限はありません。 展開し、それらのすべてで同時にライブの再読み込み動作を確認する必要に応じて多くのデバイスまたはシミュレーターでアプリを起動できます。

* ライブの再読み込みは、アプリのユーザー インターフェイス部分を再読み込みだけが*いない*ページを再作成もしませんが、ビュー モデル (または交換バインディング コンテキスト)。 つまり、*全体*アプリの状態が再読み込み、挿入された、依存関係も含めて常に保持されます。

## <a name="live-reload-server"></a>ライブの再読み込みサーバー

シナリオで、実行中のアプリから、コンピューターへの接続 (を使用して表される`localhost`または`127.0.0.1`で**ツール > オプション > Xamarin > Live Reload**) ことはできません (つまりファイアウォール、異なるネットワーク)できるサーバーを構成するリモート代わりに、IDE とアプリの両方がこれに接続します。

ライブの再読み込みは、標準を使用して[MQTT プロトコル](http://mqtt.org/)に、メッセージ交換し、で通信できるため[サード パーティのサーバー](https://github.com/mqtt/mqtt.github.io/wiki/servers)します。 あるでも[パブリック サーバー](https://github.com/mqtt/mqtt.github.io/wiki/public_brokers) (とも呼ばれます*ブローカー*) 使用できます。 ライブの再読み込みがテストされている`broker.hivemq.com`と`iot.eclipse.org`によって提供されるサービスと同様に、ホスト名[www.cloudmqtt.com](https://www.cloudmqtt.com)と[www.cloudamqp.com](https://www.cloudamqp.com)します。 など、クラウドで独自の MQTT サーバーをデプロイすることもできます。 [azure HiveMQ](https://www.hivemq.com/blog/hivemq-on-windows-azure-mqtt-microsoft-cloud)します。

任意のポートを構成できますが、リモート サーバーの既定の 1883 ポートを使用するが一般的です。 ライブの再読み込みメッセージは、安全なリモート サーバーに接続するために強力なエンド ツー エンド AES 対称暗号化を使用します。 既定では、暗号化キーおよび初期化ベクター (IV) の両方が Visual Studio のすべてのセッションで再生成します。

インストールする最も簡単な方法は、おそらく、 [mosquitto](https://mosquitto.org)空白の Ubuntu VM を Azure でのサーバー。

1. Azure Portal で新しい Ubuntu Server VM を作成します。
2. [ネットワーク] タブで 1883 (MQTT ポートの既定値) の新しい受信ポートの規則を追加します。
3. 開く、 [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) (bash モード)
4. 入力`ssh [USERNAME]@[PUBLIC_IP]`1 で選択したユーザー名を使用して) と、VM の概要ページに表示されるパブリック IP
5. 実行`sudo apt-get install mosquitto`1 で選択したパスワードを入力)

今すぐその IP を使用すると、MQTT サーバーに接続します。
