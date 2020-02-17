---
title: Xamarin のホット再起動
description: このドキュメントでは、Xamarin のホット再起動を設定して使用し、iOS アプリをデバッグする方法について説明します。
ms.prod: xamarin
ms.assetid: 6BC62A88-9368-41BB-8494-760F2A4805DB
ms.technology: xamarin-forms
author: jimmgarrido
ms.author: jigarrid
ms.date: 01/14/2020
ms.openlocfilehash: 2cf925a96e952e6b760da9ca5416e124a3e3716b
ms.sourcegitcommit: ccbf914615c0ce6b3f308d930f7a77418aeb4dbc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2020
ms.locfileid: "77071154"
---
# <a name="xamarin-hot-restart-preview"></a>Xamarin のホット再起動 (プレビュー)

![プレビュー機能](~/media/shared/preview.png)

Xamarin のホット再起動を使用すると、複数ファイルのコード編集、リソース、参照など、開発中のアプリに対する変更を迅速にテストできます。 新しい変更がデバッグ ターゲットの既存のアプリ バンドルにプッシュされ、ビルドと配置のサイクルが大幅に短縮されます。

> [!NOTE]
> Xamarin のホット再起動は現在 Visual Studio 2019 バージョン 16.5 Preview で利用でき、Xamarin.Forms を使用した iOS アプリをサポートしています。 Visual Studio for Mac と Xamarin.Forms 以外のアプリのサポートが計画中です。

## <a name="requirements"></a>必要条件

- Visual Studio 2019 バージョン 16.5 Preview 2 以降
- iTunes (64 ビット)
- Apple 開発者アカウント


## <a name="initial-setup"></a>初期セットアップ

1. iOS プロジェクトがスタートアップ プロジェクトとして設定され、ビルド構成が **[デバッグ | iPhone]** に設定されていることを確認します。

   1. 既存のプロジェクトの場合は、 **[ビルド]、[構成マネージャー]** の順に移動し、 iOS プロジェクトに対して **[配置]** が有効になっていることを確認します。

2. ツール バーの **[ローカル デバイス]** を選択してクリックし、セットアップ ウィザードを起動します。

    [![](hot-restart-images/toolbar.png "Screenshot of the Visual Studio toolbar with local device set as the debug target.")](hot-restart-images/toolbar.png)

3. iTunes がインストールされていない場合は、 **[Download iTunes]\(iTunes をダウンロード\)** をクリックして、インストーラーをダウンロードします。 iTunes のインストールが完了したら **[次へ]** をクリックします。

4. iOS デバイスをお使いのマシンに接続します。 デバイス名が検出されると、ウィザードに表示されます。 **[次へ]** をクリックします。

5. Apple 開発者アカウントの資格情報を入力し、 **[次へ]** をクリックします。

6. プロジェクトで[自動プロビジョニング](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md)を有効にするために、ドロップダウン メニューを使用して開発チームを選択します。 **[完了]** をクリックします。

> [!NOTE]
> 自動プロビジョニングを使用することをお勧めします。これにより、追加の iOS デバイスを簡単に配置用に構成できるようになります。 ただし、適切なプロビジョニング プロファイルが存在する場合は、これを無効にして、手動プロビジョニングの使用を続行することができます。

## <a name="use-xamarin-hot-restart"></a>Xamarin のホット再起動を使用する
初期セットアップの後、接続されているデバイスが [デバッグ ターゲット] ドロップダウン メニューに表示されます。 アプリをデバッグするには、ドロップダウンからデバイスを選択し、 **[実行]** ボタンをクリックします。 デバッグ セッションを開始するため、デバイスでアプリを手動で起動するように求めるメッセージが Visual Studio で表示される場合があります。

デバッグ中にコード ファイルを編集し、[デバッグ] ツール バーの **[再起動]** ボタンを押すか、**Ctrl + Shift + F5** キーを使用して、新しい変更が適用されたデバッグ セッションを再起動することができます。

[![](hot-restart-images/restart.png "Screenshot of the debug toolbar with the restart button highlighted.")](hot-restart-images/toolbar.png)

## <a name="limitations"></a>制限事項
- 現在、Xamarin.Forms および iOS デバイスでビルドされた iOS アプリのみがサポートされています。
- ストーリーボードと XIB のファイルはサポートされていません。これらを実行時に読み込もうとすると、アプリがクラッシュする可能性があります。 将来的にこのシナリオをサポートすることに関心があるため、アプリでこれらを使用している場合はお知らせください。
- 静的な iOS ライブラリとフレームワークはサポートされていません。 アプリでこれらを読み込もうとすると、ランタイム エラーまたはクラッシュが発生することがあります。 ただし、動的な iOS ライブラリはサポートされています。
- Xamarin のホット再起動を使用して、発行用のアプリ バンドルを作成することはできません。 アプリケーションを運用環境に完全にコンパイル、署名、配置するには、引き続き Mac マシンが必要です。

## <a name="troubleshoot"></a>トラブルシューティング
- iTunes が Microsoft Store からインストールされた場合、セットアップ ウィザードでは検出されません。 先にそのバージョンをアンインストールしてから、[Apple のインストーラー](https://go.microsoft.com/fwlink/?linkid=2101014)をダウンロードする必要があります。
- デバイス固有のビルドを有効にすると、アプリがデバッグ モードに移行できない既知のイシューがあります。 回避策は、 **[プロパティ]、[iOS ビルド]** でこれを無効にし、デバッグを再試行することです。 これは今後のリリースで修正される予定です。
- アプリがデバイスに既に存在する場合、ホット再起動を伴う配置を試行すると、`AMDeviceStartHouseArrestService` エラーが発生して失敗することがあります。 この回避策は、デバイスでアプリをアンインストールして、再度配置することです。

その他のイシューを報告するには、[[ヘルプ]、[フィードバックの送信]、[問題の報告]](/visualstudio/ide/feedback-options?view=vs-2019#report-a-problem) から、フィードバック ツールを使用してください。
