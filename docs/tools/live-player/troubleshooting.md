---
title: Xamarin Live Player のトラブルシューティング
description: このドキュメントでは、Xamarin Live Player と考えられる修正内容に関する既知の問題について説明します。 これは、接続の問題や、構成の問題について説明します。
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
author: lobrien
ms.author: laobri
ms.date: 08/08/2018
ms.openlocfilehash: eb9d758d72febe0fc0b705d66246c99ade1fc80f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109693"
---
# <a name="troubleshooting-xamarin-live-player"></a>Xamarin Live Player のトラブルシューティング

![プレビュー機能](~/media/shared/preview.png)

この記事では、一般的な問題を説明し、それらを修正する手順を示します。

## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>モバイル デバイスがバーコードのスキャン (または入力するコード) の後に接続できません。

Xamarin Live Player を実行しているモバイル デバイスは、IDE を実行しているコンピューターと同じネットワーク上にないときに発生します。 次を確認します。

- デバイスとコンピューターの両方を同じ Wi-fi ネットワークでは確認します。
  - コンピューターがワイヤード (有線) ネットワークに接続しても、ワイヤード (有線) 接続のプラグを抜くを再試行してください。
- (いくつか企業ネットワークなど)、ネットワークが緊密にセキュリティで保護 Xamarin Live Player で必要なポートをブロックします。
- Xamarin Live Player アプリを閉じてから再起動します。

## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE で「デプロイするときにエラー」メッセージ

**"IOException: 転送接続からデータを読み取ることができません: 非ブロッキング ソケットでの操作を妨げる"**

Xamarin Live Player を実行しているモバイル デバイスが Visual Studio は; を実行しているコンピューターと同じネットワーク上にないときに、多くの場合、このエラーが発生しましたこれは多くの場合、正常にペアリングが以前デバイスに接続するときに発生します。

* デバイスとコンピューターの両方が同じ Wi-fi ネットワークことを確認します。
* (いくつか企業ネットワークなど)、ネットワークが緊密にセキュリティで保護 Xamarin Live Player で必要なポートをブロックします。 次のポートは、Xamarin Live Player の必要があります。
  * 37847 – 内部ネットワークへのアクセス 
  * 8090 – 外部ネットワークへのアクセス

## <a name="manually-configure-device"></a>デバイスを手動で構成します。

いない Wi-fi 経由でデバイスに接続できる場合、次の手順で、構成ファイルを使用してデバイスを手動で構成しようとすることができます。

**手順 1: 構成ファイルを開く**

アプリケーション データ フォルダーに移動します。

* Windows: **%userprofile%\AppData\Roaming**
* macOS: **~/Users/$USER/.config**

このフォルダーでは紹介**PlayerDeviceList.xml**が存在しない場合は、1 つを作成する必要があります。

**手順 2: IP アドレスを取得します。**

Xamarin Live Player アプリに移動**について > 接続テスト > 接続テストの開始**します。

メモ、IP アドレスは、IP アドレスが、デバイスを構成するときに表示されている必要があります。

**手順 3: は、コードのペアを取得します。**

Xamarin Live Player タップ内**ペア**または**ペアをもう一度**、キーを押します**手動で入力**します。 数値のコードが表示されます、これは、構成ファイルを更新する必要があります。

**手順 4: GUID を生成します。**

移動: https://www.guidgenerator.com/online-guid-generator.aspx 新しい guid を生成して、大文字のことを確認します。

**手順 5: デバイスを構成します。**

開き、 **PlayerDeviceList.xml**など、Visual Studio または Visual Studio Code エディターでセットアップします。 このファイルに手動でデバイスを構成する必要があります。 既定では、ファイルは次の空白を含める必要があります`Devices`XML 要素。

```xml
<DeviceList xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
<Devices>

</Devices>
</DeviceList>
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

**閉じて、再度 Visual Studio を開きます。** デバイスが一覧に表示する必要があります。

## <a name="type-or-namespace-cannot-be-found-message-in-ide"></a>IDE で「型または名前空間が見つかりません」メッセージ

選択したことを確認、**スタートアップ プロジェクト**デバイスの種類 (例: に一致します。 Android) し、構成と一致するデバイスの種類 (例。 **デバッグ**Android 用)。

## <a name="constructor-on-type-interpretedxamarinformsbutton-not-found-message-in-player"></a>Player での「型 'InterpretedXamarin.Forms.Button' が見つかりません。 コンス トラクター」メッセージ

一部のシステム クラスはオーバーライドできません、たとえば。

```csharp
public class SomeCustomButton : Xamarin.Forms.Button { ... }
```

## <a name="mainactivitycs-resourcelayout-does-not-contain-a-definition-for-main"></a>"MainActivity.cs: 'Resource.Layout' に 'Main' の定義が含まれていません"

このエラーは、AXML ファイルで定義されたユーザー インターフェイスは、Android プロジェクト用に発生します。
AXML ファイルは Xamarin Live Player で現在サポートされていません。

### <a name="android-toolbar-and-tabs-render-incorrectly-using-xamarinforms"></a>Android のツールバーとタブ表示 Xamarin.Forms を正しく使用します。

Xamarin.Forms の Android プロジェクトでは、関連するレイアウト ファイルの名前に"Toolbar.axml"と"Tabbar.axml"を使用する必要があります。 これらの名前を使用する既定のテンプレートそれらの名前を変更すると、レンダリングの問題が発生されます。

その他の問題を報告してください[bugzilla](https://aka.ms/live-player-report-issue)します。

## <a name="related-links"></a>関連リンク

- [制限事項](~/tools/live-player/limitations.md)
- [セットアップ](~/tools/live-player/install.md)
