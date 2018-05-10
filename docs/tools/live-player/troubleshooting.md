---
title: トラブルシューティング
description: Xamarin Live Player、およびそれらの修正方法に関する既知の問題です。
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: 147ce43d3fe764f71f27dce46b699142dfb99872
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="troubleshooting"></a>トラブルシューティング

![プレビュー機能](~/media/shared/preview.png)

ここでは、一般的な問題について説明し、それらを解決する手順について説明します。


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>モバイル デバイスがスキャン バーコード (または入力するコード) の後に接続できません。

Xamarin Live Player を実行しているモバイル デバイスは、IDE を実行しているコンピューターと同じネットワーク上にないときに発生します。 次を確認します。

- デバイスとコンピューターの両方が同じの Wi-fi ネットワークにあることを確認します。
  - コンピューターがワイヤード (有線) ネットワークに接続しても、ワイヤード (有線) 接続のプラグを抜きを再試行してください。
- (によって企業ネットワークなど)、緊密にネットワークを保護することがあります Xamarin Live Player で必要なポートをブロックします。
- Live プレーヤーの Xamarin アプリを閉じて再起動します。


## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE で「を展開するときにエラー」メッセージ

**"IOException: 転送接続からデータを読み取ることができません: 非ブロッキング ソケットでの操作はブロック"**

Xamarin Live Player を実行しているモバイル デバイスが Visual Studio は; を実行しているコンピューターと同じネットワーク上にない場合に、多くの場合、このエラーが発生しましたこれは多くの場合、ペアリングされて正常にデバイスに接続するときに発生します。

* デバイスとコンピューターの両方が同じの Wi-fi ネットワークにあることを確認してください。
* (によって企業ネットワークなど)、緊密にネットワークを保護することがあります Xamarin Live Player で必要なポートをブロックします。 次のポートは、Xamarin Live プレーヤーの必要があります。
  * 37847 – 内部ネットワーク アクセス 
  * 8090-外部のネットワーク アクセス

## <a name="manually-configure-device"></a>デバイスを手動で構成します。

、Wi-fi 経由で、デバイスに接続できる場合、次の手順で、構成ファイルを使用したデバイスを手動で構成しようとすることができません。

**手順 1: 構成ファイルを開く**

アプリケーション データ フォルダーにヘッドします。

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

このフォルダーで見つかります**PlayerDeviceList.xml**が存在しない場合は、1 つを作成する必要があります。

**手順 2: IP アドレスを取得します。**

Xamarin Player のライブ アプリに移動**に関する > 接続テスト > 接続テストの開始**です。

メモの IP アドレスは、デバイスを構成するときに表示されている IP アドレス必要があります。

**手順 3: は、コードのペアを取得します。**

Xamarin Player のライブ tap の内部で**ペア**または**ペア再度**、キーを押します**手動で入力**です。 数値のコードが表示されます、これは、構成ファイルを更新する必要があります。

**手順 4: GUID を生成します。**

移動:https://www.guidgenerator.com/online-guid-generator.aspx新しい guid を生成しには、大文字に変換するかどうかを確認します。


**手順 5: デバイスを構成します。**

開き、 **PlayerDeviceList.xml**など、Visual Studio または Visual Studio Code エディターでセットアップします。 このファイルで、デバイスを手動で構成する必要があります。 既定では、ファイルが次の空白を含める必要があります`Devices`XML 要素。

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
```

**IOS デバイスを追加します。**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>iPhone Player</Name>
<Platform>iOS</Platform>
<AndroidApiLevel>0</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:36:03.9492291Z</LastConnectTimeUtc>
</PlayerDevice>
```


**Android デバイスを追加します。**

```xml
<PlayerDevice>
<SecretCode>ENTER-PAIR-CODE-HERE</SecretCode>
<UniqueIdentifier>ENTER-GUID-HERE</UniqueIdentifier>
<Name>Android Player</Name>
<Platform>Android</Platform>
<AndroidApiLevel>24</AndroidApiLevel>
<DebuggerEndPoint>ENTER-IP-HERE:37847</DebuggerEndPoint>
<HostEndPoint />
<NeedsAppInstall>false</NeedsAppInstall>
<IsSimulator>false</IsSimulator>
<SimulatorIdentifier />
<LastConnectTimeUtc>2018-01-08T20:34:42.2332328Z</LastConnectTimeUtc>
</PlayerDevice>
```

**閉じて、再び Visual Studio を開きます。** デバイスが一覧に表示する必要があります。


## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>IDE で「型または名前空間が見つかりません」メッセージ

選択したことを確認して、**スタートアップ プロジェクト**デバイスの種類 (iOS または Android) に一致して、そのデバイスの種類 (の構成と一致 **デバッグ | iPhone シミュレーター** iOS 用)。

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Player でメッセージを「コンス トラクターで型 'InterpretedXamarin.Forms.Button' が見つかりません。」

一部のシステム クラスはオーバーライドできません、たとえば。

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: 'Resource.Layout' に 'Main' の定義が含まれていません"

このエラーは、AXML ファイルで定義されているユーザー インターフェイスでは、Android プロジェクト用に発生します。
AXML ファイルは、Xamarin Live プレーヤーで現在サポートされていません。

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android のツールバーとタブは、Xamarin.Forms の使用を正しくを表示します。

Xamarin.Forms Android プロジェクトでは、関連するレイアウト ファイルの名前の"Toolbar.axml"と"Tabbar.axml"を使用する必要があります。 これらの名前を使用する既定のテンプレート名前を変更すると、レンダリングの問題が発生します。


その他の問題を報告してください[bugzilla](https://aka.ms/live-player-report-issue)です。


## <a name="related-links"></a>関連リンク

- [制限事項](~/tools/live-player/limitations.md)
- [セットアップ](~/tools/live-player/install.md)
