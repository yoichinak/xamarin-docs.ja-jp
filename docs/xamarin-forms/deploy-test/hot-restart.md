---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 45b7d0d20c43aa22ebde3a17552f10ceea77a48b
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/28/2020
ms.locfileid: "84139140"
---
# <a name="xamarin-hot-restart-preview"></a>Xamarin のホット再起動 (プレビュー)

![プレビュー機能](~/media/shared/preview.png)

Xamarin のホット再起動を使用すると、複数ファイルのコード編集、リソース、参照など、開発中のアプリに対する変更を迅速にテストできます。 新しい変更がデバッグ ターゲットの既存のアプリ バンドルにプッシュされ、ビルドと配置のサイクルが大幅に短縮されます。

> [!IMPORTANT]
> Xamarin のホット再起動は現在 Visual Studio 2019 バージョン 16.5 で安定的に利用でき、Xamarin.Forms を使用する iOS アプリをサポートしています。 Visual Studio for Mac と Xamarin.Forms 以外のアプリのサポートが計画中です。

## <a name="requirements"></a>必要条件

- Visual Studio 2019 バージョン 16.5
- iTunes (64 ビット)
- Apple 開発者アカウントと有料 [Apple Developer Program](https://developer.apple.com/programs) 登録


## <a name="initial-setup"></a>初期セットアップ

> [!NOTE]
> Xamarin のホット再起動は、プレビュー段階の間は既定で無効になっています。 **[ツール] > [オプション] > [環境] > [プレビュー機能] > [Xamarin ホット リスタートを有効にする]** で有効にすることができます。

1. iOS プロジェクトがスタートアップ プロジェクトとして設定され、ビルド構成が **[デバッグ | iPhone]** に設定されていることを確実にします。

   1. 既存のプロジェクトの場合は、 **[ビルド]、[構成マネージャー]** の順に移動し、 iOS プロジェクトに対して **[配置]** が有効になっていることを確認します。

2. ツール バーの **[ローカル デバイス]** を選択してクリックし、セットアップ ウィザードを起動します。

    [![](hot-restart-images/toolbar.png "Screenshot of the Visual Studio toolbar with local device set as the debug target.")](hot-restart-images/toolbar.png)

3. iTunes がインストールされていない場合は、 **[Download iTunes]\(iTunes をダウンロード\)** をクリックして、インストーラーをダウンロードします。 iTunes のインストールが完了したら **[次へ]** をクリックします。

4. iOS デバイスをお使いのマシンに接続します。 デバイスが既に接続されている場合は、プラグを抜いてから再接続します。 デバイス名が検出されると、ウィザードに表示されます。 **[次へ]** をクリックします。

5. Apple 開発者アカウントの資格情報を入力し、 **[次へ]** をクリックします。

6. プロジェクトで[自動プロビジョニング](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md)を有効にするために、ドロップダウン メニューを使用して開発チームを選択します。 **[完了]** をクリックします。

> [!NOTE]
> 自動プロビジョニングを使用することをお勧めします。これにより、追加の iOS デバイスを簡単に配置用に構成できるようになります。 ただし、適切なプロビジョニング プロファイルが存在する場合は、これを無効にして、手動プロビジョニングの使用を続行することができます。

## <a name="use-xamarin-hot-restart"></a>Xamarin のホット再起動を使用する
初期セットアップの後、接続されているデバイスが [デバッグ ターゲット] ドロップダウン メニューに表示されます。 アプリをデバッグするには、ドロップダウンからデバイスを選択し、 **[実行]** ボタンをクリックします。 デバッグ セッションを開始するため、デバイスでアプリを手動で起動するように求めるメッセージが Visual Studio で表示される場合があります。

デバッグ中にコード ファイルを編集し、[デバッグ] ツール バーの **[再起動]** ボタンを押すか、**Ctrl + Shift + F5** キーを使用して、新しい変更が適用されたデバッグ セッションを再起動することができます。

[![](hot-restart-images/restart.png "Screenshot of the debug toolbar with the restart button highlighted.")](hot-restart-images/toolbar.png)

また、`HOTRESTART` プリプロセッサ シンボルを使用して、Xamarin のホット再起動でデバッグするときに特定のコードが実行されないようにすることもできます。

## <a name="limitations"></a>制限事項

- 現在、Xamarin.Forms および iOS デバイスでビルドされた iOS アプリのみがサポートされています。
- 64 ビットの iOS デバイスのみがサポートされています。 iOS 11 以降では、32 ビット アーキテクチャ (iPhone 5s よりも前のデバイス) で iOS アプリを実行することが Apple によって許可されなくなりました。
- ストーリーボードと XIB のファイルはサポートされていません。これらを実行時に読み込もうとすると、アプリがクラッシュする可能性があります。 `HOTRESTART` プリプロセッサ シンボルを使用して、このコードが実行されないようにします。
- 静的な iOS ライブラリとフレームワークはサポートされていません。アプリがこれらを読み込もうとすると、実行時エラーまたはクラッシュが発生する可能性があります。 `HOTRESTART` プリプロセッサ シンボルを使用して、このコードが実行されないようにします。 動的な iOS ライブラリはサポートされています。
- Xamarin のホット再起動を使用して、発行用のアプリ バンドルを作成することはできません。 アプリケーションを運用環境に完全にコンパイル、署名、配置するには、引き続き Mac マシンが必要です。

## <a name="troubleshoot"></a>トラブルシューティング

- iTunes が Microsoft Store からインストールされた場合、セットアップ ウィザードでは検出されません。 先にそのバージョンをアンインストールしてから、[Apple のインストーラー](https://go.microsoft.com/fwlink/?linkid=2101014)をダウンロードする必要があります。
- デバイス固有のビルドを有効にすると、アプリがデバッグ モードに移行できない既知のイシューがあります。 回避策は、 **[プロパティ]、[iOS ビルド]** でこれを無効にし、デバッグを再試行することです。 これは今後のリリースで修正される予定です。
- アプリがデバイスに既に存在する場合、ホット再起動を伴う配置を試行すると、`AMDeviceStartHouseArrestService` エラーが発生して失敗することがあります。 この回避策は、デバイスでアプリをアンインストールして、再度配置することです。
- Apple Developer Program に含まれていない Apple ID を入力すると、次のエラーが発生する可能性があります: `Authentication Error. Xcode 7.3 or later is required to continue developing with your Apple ID`。 iOS デバイスで Xamarin のホット再起動を使用するには、有効な Apple Developer アカウントが必要です。 

その他のイシューを報告するには、[[ヘルプ]、[フィードバックの送信]、[問題の報告]](/visualstudio/ide/feedback-options?view=vs-2019#report-a-problem) から、フィードバック ツールを使用してください。
