---
title: Visual Studio Emulator for Android
description: "このガイドでは、Visual Studio 2015 で Xamarin.Android アプリを開発するため、Visual Studio Emulator for Android を構成および使用する方法について説明します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: CD128CB9-499F-4558-B49F-77248824EFDF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: a31d90e6d5abd574eb6187953082e1b70f66a113
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="visual-studio-android-emulator"></a>Visual Studio Emulator for Android

_このガイドでは、Visual Studio 2015 で Xamarin.Android アプリを開発するため、Visual Studio Emulator for Android を構成および使用する方法について説明します。_

## <a name="visual-studio-android-emulator-overview"></a>Visual Studio Android Emulator の概要

Microsoft Visual Studio 2015 には、Xamarin.Android アプリのデバッグのターゲットとして使用できる Android エミュレーター、*Visual Studio Emulator for Android* が含まれています。 このエミュレーターは、開発用コンピューターの Hyper-V 機能を使用して、Android SDK に付属の既定のエミュレーターよりも速い起動と短い実行時間を実現します。 Visual Studio Emulator for Android は、Xamarin.Android アプリケーションを開発するときに、既定の Android SDK エミュレーターの代わりに使用できます。

このガイドでは、Visual Studio から Microsoft Android エミュレーターを起動してアプリをテストする方法と、エミュレーターで使用できるさまざまな機能について説明します。 (既定の Android SDK エミュレーターのデバイス定義に似た) *デバイス プロファイル* を選択してさまざまな種類の Android デバイスをシミュレートする方法を学習します。 最後のトラブルシューティングのセクションでは、よくある落とし穴とその回避策について説明します。

## <a name="requirements"></a>必要条件

エミュレーターを実行するには、コンピューターが Hyper-V の実行要件を満たしている必要があります。 Hyper-V には、Windows 8、Windows 8.1、Windows10 以上の Pro エディションの 64 ビット バージョンが必要です。 要件の詳細については、「[Visual Studio Emulator for Android のシステム要件](https://msdn.microsoft.com/en-us/library/mt228280.aspx)」を参照してください。

> [!NOTE]
> Hyper-V が有効になっている場合は、HAXM (Android SDK エミュレーターで使用) を使用することはできません。 HAXM の制限事項と潜在的な問題の詳細については、[HAXM 仮想化の競合](~/android/deploy-test/debugging/android-sdk-emulator/troubleshooting.md#virt-conflicts)に関するページを参照してください。


## <a name="running-the-emulator"></a>エミュレーターの実行

Visual Studio は、**[デバッグ ターゲット]** ドロップダウン メニュー (次のスクリーン ショット) で複数の事前構成済みのターゲット デバイス プロファイルを使用できるようにします。 Microsoft Android Emulator ターゲットには、**VS Emulator** が前に付きます。

[![事前構成済みのターゲット デバイス プロファイル](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs-sml.png)](visual-studio-android-emulator-images/01-vs-emulator-defaults-vs.png#lightbox)

Visual Studio が Xamarin.Android アプリケーションを起動すると、エミュレーターは選択したデバイスのターゲットで起動され、アプリがエミュレーターに展開されます。 Visual Studio の左下隅にエミュレーターが起動していることを示すメッセージが表示されます。

[![VS エミュレーターが起動中](visual-studio-android-emulator-images/02-emulator-starting-vs-sml.png)](visual-studio-android-emulator-images/02-emulator-starting-vs.png#lightbox)

スタートアップの遅延の後に、左下に示されているようなエミュレーター画面が表示されます。 画面のロック アイコンを上にドラッグして、デバイスのロックを解除します。
Xamarin.Android アプリは、右に示すように、エミュレーターで実行されているはずです。

[![エミュレーターのスクリーンショット](visual-studio-android-emulator-images/03-first-screen-vs-sml.png)](visual-studio-android-emulator-images/03-first-screen-vs.png#lightbox)

既定の Android SDK エミュレーターと同じく、コードにブレークポイントを設定する、変数を調査する、および呼び出し履歴を表示することができます。 エミュレーターの右側の垂直ツールバーからエミュレーターの機能にアクセスできます。

[![垂直ツールバーのボタン](visual-studio-android-emulator-images/04-vertical-toolbar-vs-sml.png)](visual-studio-android-emulator-images/04-vertical-toolbar-vs.png#lightbox)

次のリストは、垂直ツールバーの各ボタンの機能をまとめたものです。

-   **閉じる** &ndash; エミュレーター アプリケーションをシャットダウンします。 このボタンはあまり使用されません。&ndash; 通常、エミュレーターは最初に起動した後、(エミュレーターの再起動の遅延を回避するために) 実行したままになっており、不要になった場合にのみ閉じられます。

-   **最小化** &ndash; エミュレーターを実行したまま、タスクバーに最小化します。

-   **電源** &ndash; デバイスのオン/オフをシミュレートします  (エミュレーターは実行したままです)。

-   **マルチタッチ** &ndash; ピンチやズームのタッチ ポイントとして機能する複数のドットをデバイス ディスプレイにオーバーレイします。
    1 つのドットをドラッグすると、他のドットが反対方向に移動し、2 本の指の動きをシミュレートします。

-   **シングル ポイント マウス入力** &ndash; (マルチ タッチ入力を使用した後に) デバイスをシングル ポイント入力に戻します。

-   **左に回転/右に回転** &ndash; アプリが向きの変更にどのように反応するかのテストを支援します。 たとえば、初めて *[左に回転]* ボタンをクリックすると、エミュレーターは横モードに切り替わります。 *[右に回転]* ボタンを押すと、エミュレーターは縦モードに戻ります。

-   **画面に合わせる** &ndash; デスクトップ画面に収まるように、エミュレーター画面のサイズを拡大します。

-   **ズーム** &ndash; エミュレーター画面を 33%、50%、66%、100%、またはカスタム比率で拡大縮小します。


*[その他のツール]* ボタンは、エミュレーターの追加の機能を表示するダイアログを開きます。

[![[その他のツール] ダイアログ](visual-studio-android-emulator-images/05-additional-tools-vs-sml.png)](visual-studio-android-emulator-images/05-additional-tools-vs.png#lightbox)


追加機能はそれぞれ、ダイアログの上部にあるタブの行から入手できます。

-   **加速度計** &ndash; デバイスの動きを 3D 空間でシミュレートします。

-   **位置情報** &ndash; GPS の位置の選択およびシミュレートするために使用できるマップを表示します。 このマップ上に、位置間の移動をシミュレートするための*マップ ポイント*を作成することができます。

-   **バッテリ** &ndash; バッテリの残量をシミュレートするためのスライダーを提供します。

-   **スクリーンショット** &ndash; このタブには、スクリーンショットを取得し、インスタント プレビューを表示する **[キャプチャ]** ボタンがあります。 *[保存]* ボタンはスクリーンショットを保存します。

-   **カメラ** &ndash; 固定のアニメーション化されたイメージを通じて、ファイルから、またはホスト コンピューターに接続されている Web カメラからの画像を使用して写真撮影をシミュレートします。 フロンント カメラまたはリア カメラのいずれかを選択できます。

-   **SD カード** &ndash; エミュレーターは、ホスト コンピューター上のフォルダーを SD カードとしてデバイスが使用できるようにすることができます。
    アプリがファイルを読み取り、シミュレートした SD カードに書き込むときに、`adb` コマンドを使用せずにデスクトップから直接ファイルにアクセスできます。

-   **ネットワーク** &ndash; エミュレーターのネットワーク設定の概要を表示します (エミュレーターはホスト コンピューターのネットワーク接続を再利用します)。

これらの機能を使用する方法の詳細については、「[Introducing Visual Studio's Emulator for Android](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/)」 (Visual Studio Emulator for Android の概要) を参照してください。



## <a name="configuring-device-profiles"></a>デバイス プロファイルを構成する

Microsoft Android Emulator には、市場で最も人気のある Android バージョン、画面サイズ、および Android デバイスのハードウェア プロパティを表す一連のデバイス プロファイルが含まれています。 さらに、これらのデバイス プロファイルは KitKat、ロリポップ、Marshmallow などのさまざまな Android バージョン用に既に構成されています。

デバイス プロファイルのインストール、アンインストール、および開始には、*エミュレーター マネージャー*が使用されます。 次のスクリーンショットに示されているように、**[ツール]** メニューから **[Visual Studio Emulator for Android]** を選択します。

[![[ツール] メニューからエミュレーターを起動する](visual-studio-android-emulator-images/06-launch-emulator-manager-vs-sml.png)](visual-studio-android-emulator-images/06-launch-emulator-manager-vs.png#lightbox)

これにより **[Device Profiles]\(デバイス プロファイル\)** ダイアログが開きます。 インストールされたプロファイルは、デバイス プロファイル リストの上部で強調表示されます。 インストールされていない (ただし、インストール可能な) プロファイルはグレー表示されます。

[![[Device Profiles]\(デバイス プロファイル\) アイコン](visual-studio-android-emulator-images/07-device-profiles-vs-sml.png)](visual-studio-android-emulator-images/07-device-profiles-vs.png#lightbox)

新しいプロファイルをインストールするには、プロファイルのインストール アイコン (上記のスクリーンショットに示されているように、下向きの矢印) をクリックします。 たとえば、**5.7"Marshmallow (6.0.0) XHDPI Phone** のプロファイルのインストール アイコンをクリックすると、次に示すように、エミュレーター マネージャーがプロファイルをダウンロードします。

[![プロファイルのダウンロードの例](visual-studio-android-emulator-images/08-downloading-profile-vs-sml.png)](visual-studio-android-emulator-images/08-downloading-profile-vs.png#lightbox)

デバイス プロファイルをダウンロードすると、プロファイルが正常にインストールされたことを示すために強調表示されます。 *[詳細の表示]* アイコンをクリックすると、デバイスで使用可能なプラットフォームの種類、CPU アーキテクチャ、画面のサイズと解像度、およびメモリが表示されます。

[![デバイス プロファイルの詳細の表示](visual-studio-android-emulator-images/09-show-details-vs-sml.png)](visual-studio-android-emulator-images/09-show-details-vs.png#lightbox)

Visual Studio の **[デバッグ ターゲット]** ドロップダウン メニューを開くと、新しくインストールしたデバイス プロファイルがターゲットとして使用できるようになります。

[![ターゲット ドロップダウン メニューの新しいプロファイル](visual-studio-android-emulator-images/10-debug-target-vs-sml.png)](visual-studio-android-emulator-images/10-debug-target-vs.png#lightbox)

このリストを短くするには、*エミュレーター マネージャー*で **[Uninstall this profile]\(このプロファイルをアンインストールします\)** をクリックして、使用していないデバイス プロファイルを削除します。 現時点では、このエミュレーターで、カスタマイズしたデバイス プロファイルを作成する方法がないことに注意してください。


## <a name="troubleshooting"></a>トラブルシューティング

このセクションでは、*Visual Studio Emulator for Android* と Xamarin.Android を使用する際の一般的なエラーと回避策について説明します。

<a name="cant_connect" />

### <a name="emulator-will-not-start"></a>エミュレーターが起動しない

ホスト プロセッサと Hyper-V 仮想マシンとの非互換性がある場合、エミュレーターが起動しないことがあります。 この問題を回避するには、仮想マシンが持つことができるプロセッサの機能を制限するように Hyper-V を構成することです。 &ndash; これにより、仮想マシンと異なるホスト プロセッサのバージョンとの互換性が向上します。
次の手順に従って、この変更を行います。

1.  **[スタート]** ボタンをクリックして、「**MMC**」と入力し、**Enter** キーを押します。 次に示すように、**[Hyper-V マネージャー]** をクリックします。

    [![Hyper-V マネージャー](visual-studio-android-emulator-images/15-launch-hyperv-manager.png)](visual-studio-android-emulator-images/15-launch-hyperv-manager.png#lightbox)

2.  Hyper-V マネージャーの **[仮想マシン]** ウィンドウで、編集して使用するエミュレーターを右クリックし、**[設定...]** をクリックします。

    [![仮想マシンの設定のメニュー項目](visual-studio-android-emulator-images/16-vm-settings.png)](visual-studio-android-emulator-images/16-vm-settings.png#lightbox)

3.  [設定] ウィンドウで、**[互換性]** セクション (**[ハードウェア]、[プロセッサ]** の下) を見つけ、**[プロセッサ バージョンが異なる物理コンピューターへ移行する]** を有効にします。

    [![オンになっている移行オプション](visual-studio-android-emulator-images/17-set-compatibility-vs-sml.png)](visual-studio-android-emulator-images/17-set-compatibility-vs.png#lightbox)

4.  **[OK]** をクリックして、[Hyper-V マネージャー] ウィンドウを閉じます。



### <a name="app-deploys-and-starts-but-fails-immediately"></a>アプリが展開されて起動するが、すぐに失敗する

この状況では、エミュレーターが起動し、アプリがエミュレーターに正常に展開され起動されます。 しかし、アプリはすぐに失敗します。
多くの場合、これもホスト プロセッサと Hyper-V 仮想マシン間の非互換性によって発生します。 このエラーを解決するには、上記の「[エミュレーターが起動しない](#cant_connect)」の指示に従います。


### <a name="emulator-stops-with-the-diagnostic-message-libaot-mscorlibdllso-not-found"></a>エミュレーターが診断メッセージ "**libaot-mscorlib.dll.so not found**" (libaot-mscorlib.dll.so が見つかりません) で停止する

このエラーを解決するには、次の手順に従って Fast Deployment を無効にします。

1.  上記の「[エミュレーターが起動しない](#cant_connect)」の指示に従います。

2.  プロジェクトの **[プロパティ]** をダブルクリックします。

3.  **[Android オプション]** をクリックして、**[Fast Deployment の使用 (デバッグ モードのみ)]** を選択解除します。

    [![オフにした [Fast Deployment の使用] オプション](visual-studio-android-emulator-images/18-fast-deployment-vs-sml.png)](visual-studio-android-emulator-images/18-fast-deployment-vs.png#lightbox)



### <a name="drag-and-drop-does-not-work"></a>ドラッグ アンド ドロップが機能しない

管理者として *Visual Studio Emulator for Android* を起動した場合 (または Visual Studio を管理者特権で実行しているときに Visual Studio からこれを起動した場合)、.APK ファイルまたは .ZIP ファイルのドラッグ アンド ドロップが機能しないことがあります。 この問題を回避するには、より高いレベルのアクセス許可を使用せずに (つまり管理者としてではなく) *Visual Studio Emulator for Android* を実行します。


### <a name="other-errors"></a>その他のエラー

上記のトラブルシューティングのヒントは、Visual Studio Emulator for Android と Xamarin.Android を使用する際の最も一般的な問題を対象としています。 Visual Studio Emulator for Android のトラブルシューティングのより詳細なガイドについては、「[Visual Studio Emulator for Android のトラブルシューティング](https://msdn.microsoft.com/en-us/library/mt228282.aspx)」を参照してください。



## <a name="summary"></a>まとめ

*Visual Studio Emulator for Android* を紹介したこの記事では、エミュレーターを使用して Visual Studio で Xamarin.Android アプリをデバッグする方法について説明し、垂直ツールバーのボタンの機能を説明し、**[その他のツール]** ダイアログで使用可能な機能の概要を提供しました。 また、*エミュレーター マネージャー*を使用してデバイス プロファイルをインストール、アンインストール、および開始する方法についても説明しました。 「*トラブルシューティング*」セクションでは、エミュレーターを使用する際の一般的な問題と回避策を説明しました。


## <a name="related-links"></a>関連リンク

- [Visual Studio Emulator for Android](https://www.visualstudio.com/en-us/explore/msft-android-emulator-vs.aspx)
- [Visual Studio Emulator for Android の概要](https://blogs.msdn.microsoft.com/visualstudioalm/2014/11/12/introducing-visual-studios-emulator-for-android/)
