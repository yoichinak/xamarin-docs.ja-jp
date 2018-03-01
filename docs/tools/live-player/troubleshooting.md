---
title: "トラブルシューティング"
description: "Xamarin Live Player、およびそれらの修正方法に関する既知の問題です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 29A97ADA-80E0-40A1-8B26-C68FFABE7D26
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/17/2017
ms.openlocfilehash: d7c5bedb03d7c869be65e3c704bac58a9cdfcbbd
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="troubleshooting"></a>トラブルシューティング

![プレビュー機能](~/media/shared/preview.png)

ここでは、一般的な問題について説明し、それらを解決する手順について説明します。


## <a name="mobile-device-does-not-connect-after-scanning-barcode-or-entering-code"></a>モバイル デバイスがスキャン バーコード (または入力するコード) の後に接続できません。

Xamarin Live Player を実行しているモバイル デバイスは、IDE を実行しているコンピューターと同じネットワーク上にないときに発生します。 次を確認します。

- デバイスとコンピューターの両方が同じ WiFi ネットワークにあることを確認します。
  - コンピューターがワイヤード (有線) ネットワークに接続しても、ワイヤード (有線) 接続のプラグを抜きを再試行してください。
- (によって企業ネットワークなど)、緊密にネットワークを保護することがあります Xamarin Live Player で必要なポートをブロックします。
- Live プレーヤーの Xamarin アプリを閉じて再起動します。


## <a name="error-while-trying-to-deploy-message-in-ide"></a>IDE で「を展開するときにエラー」メッセージ

**"IOException: 転送接続からデータを読み取ることができません: 非ブロッキング ソケットでの操作はブロック"**

Xamarin Live Player を実行しているモバイル デバイスは、IDE を実行しているコンピューターと同じネットワーク上にないときに、多くの場合、このエラーが発生しましたこれは多くの場合、ペアリングされて正常にデバイスに接続するときに発生します。

* デバイスとコンピューターの両方が同じ WiFi ネットワークにあることを確認してください。
* (によって企業ネットワークなど)、緊密にネットワークを保護することがあります Xamarin Live Player で必要なポートをブロックします。 次のポートは、Xamarin Live プレーヤーの必要があります。
  * 37847 – 内部ネットワーク アクセス 
  * 8090-外部のネットワーク アクセス

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
