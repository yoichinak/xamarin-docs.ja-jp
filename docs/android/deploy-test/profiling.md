---
title: Android アプリのプロファイリング
description: このガイドでは、プロファイラー ツールを使用して、Android アプリのパフォーマンスとメモリ使用量を調べる方法について説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 8C823FEE-A6F6-4C31-9EB6-E51407A2FD8E
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/03/2018
ms.openlocfilehash: 44bed11e4d2ccf7baa39734a1b20e49b9ecf5f10
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70753992"
---
# <a name="profiling-android-apps"></a>Android アプリのプロファイリング

アプリ ストアにアプリを配置する前に、パフォーマンスのボトルネック、メモリの過剰使用の問題、またはネットワーク リソースの非効率的な使用を識別して修正することが重要です。 この目的を果たすには、次の 2 つのプロファイラー ツールを利用できます。

- Xamarin Profiler 
- Android Studio 内の Android Profiler

このガイドでは、Xamarin Profiler を紹介し、Android Profiler の使用を開始するための詳細な情報を提供します。

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin Profiler は、IDE 内から Xamarin アプリをプロファイリングするために Visual Studio および Visual Studio for Mac に統合されているスタンドアロン アプリケーションです。 Xamarin Profiler の使用の詳細については、「[Xamarin Profiler](~/tools/profiler/index.md)」を参照してください。

> [!NOTE]
> Windows 用 Visual Studio Enterprise または Visual Studio for Mac のいずれかで Xamarin Profiler 機能のロックを解除するには、[Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) のサブスクライバーである必要があります。

## <a name="android-studio-profiler"></a>Android Studio Profiler

Android Studio 3.0 以降には、Android Profiler ツールが含まれています。 Android Profiler を使用すれば、Visual Studio Enterprise のライセンスがなくても、Visual Studio でビルドされた Xamarin Android アプリのパフォーマンスを測定することができます。 ただし、Android Profiler は、Xamarin Profiler とは異なり、Visual Studio に統合されていないため、あらかじめビルドされ Android Profiler にインポートされた Android アプリケーション パッケージ (APK) をプロファイリングする場合にのみ使用できます。

### <a name="launching-a-xamarin-android-app-in-android-profiler"></a>Android Profiler での Xamarin Android アプリの起動

次の手順では、Android Studio の Android Profiler ツールで Xamarin Android アプリケーションを起動する方法について説明します。 次のスクリーン ショットの例では、Xamarin Forms の [XamagonXuzzle](https://docs.microsoft.com/samples/xamarin/mobile-samples/liveplayer-xamagonxuzzlelp/) アプリがビルドされ、Android Profiler を使用してプロファイリングされています。

1. Android プロジェクトのビルド オプションで、 **[共有ランタイムの使用]** を無効にします。 これで、Android アプリケーション パッケージ (APK) は、開発時の共有 Mono ランタイムとの依存関係なしで、ビルドされるようになります。

    ![[共有ランタイムの使用] の無効化](profiling-images/vswin/01-turn-off-shared-runtime.png)

2. **デバッグ**用のアプリをビルドし、物理デバイスまたはエミュレーターに配置します。 これにより、APK の署名付き**デバッグ** バージョンがビルドされます。
    **XamagonXuzzle** の例では、結果として得られる APK に **com.companyname.XamagonXuzzle Signed.apk** という名前が付けられます。

3. プロジェクト フォルダーを開き、**bin/debug** に移動します。 このフォルダー内でアプリの **Signed.apk** バージョンを見つけ、それを便利にアクセスできる場所 (デスクトップなど) にコピーします。 次のスクリーン ショットの例では、APK **com.companyname.XamagonXuzzle Signed.apk** を見つけて、デスクトップにコピーします。

    [![署名された APK ファイルのデバッグの場所](profiling-images/vswin/02-locating-the-debug-apk-sml.png)](profiling-images/vswin/02-locating-the-debug-apk.png#lightbox)

4. Android Studio を起動し、 **[Profile or debug APK]\(プロファイルまたはデバッグ APK\)** を選択します。

    ![Android Studio 起動画面からのプロファイラーの起動](profiling-images/vswin/03-android-studio.png)

5. **[Select APK File]\(APK ファイルの選択\)** ダイアログ内で、前にビルドしてコピーした APK に移動します。 APK を選択し、 **[OK]** をクリックします。 
    
    ![[Select APK File]\(APK ファイルの選択\) ダイアログでの APK の選択](profiling-images/vswin/04-select-apk-dialog.png)

6. Android Studio によって APK が読み込まれ、**classes.dex** が逆アセンブルされます。

    ![APK の設定](profiling-images/vswin/05-setting-up-the-apk.png)

7. APK が読み込まれると、Android Studio によって APK 用の次のプロジェクト画面が表示されます。 左側のツリー ビューでアプリ名を右クリックして、 **[Open Module Settings]\(モジュール設定を開く\)** を選択します。

    [![メニュー項目 [Open Module Settings]\(モジュール設定を開く\) の場所](profiling-images/vswin/06-open-module-settings-sml.png)](profiling-images/vswin/06-open-module-settings.png#lightbox)

8. **[プロジェクトの設定]、[モジュール]** の順に移動し、アプリの **[-Signed]** ノードを選択し、 **&lt;[No SDK]\(SDK なし\)&gt;** をクリックします。

    [![SDK 設定への移動](profiling-images/vswin/07-project-settings-modules-sml.png)](profiling-images/vswin/07-project-settings-modules.png#lightbox)

9. **[Module SDK]\(モジュール SDK\)** プルダウン メニューで、アプリをビルドするために使用した Android SDK レベルを選択します (この例では、API レベル 26 を使用して **XamagonXuzzle** がビルドされています)。

    [![プロジェクト SDK レベルの設定](profiling-images/vswin/08-project-sdk-level-sml.png)](profiling-images/vswin/08-project-sdk-level.png#lightbox)

    **[Apply]\(適用\)** 、 **[OK]** の順にクリックして、この設定を保存します。

10. ツール バー アイコンからプロファイラーを起動します。

    [![プロファイラー ツールバー アイコンの場所](profiling-images/vswin/09-launch-profiler-sml.png)](profiling-images/vswin/09-launch-profiler.png#lightbox)

11. アプリを実行/プロファイリングするために配置ターゲットを選択して、 **[OK]** をクリックします。 配置ターゲットとしては、物理デバイスまたはエミュレーターで実行されている仮想デバイスを指定できます。 この例では、Nexus 5 X デバイスを使用します。

    ![配置ターゲットの選択](profiling-images/vswin/10-select-deployment-target.png)

12. プロファイラーを起動後、プロファイラーが配置デバイスおよびアプリのプロセスに接続するのに数秒かかります。 APK がインストールされている間、Android Profiler は**接続されているデバイスが存在しない**こと、および**デバッグ可能なプロセスが存在しない**ことをレポートします。

    [![プロファイラーによって APK がインストールされます](profiling-images/vswin/11-no-connected-devices-sml.png)](profiling-images/vswin/11-no-connected-devices.png#lightbox)

13. 数秒後、Android Profiler は APK のインストールを完了して APK を起動します。このときデバイス名とプロファイリングされるアプリ プロセスの名前がレポートされます (この例では、**LGE Nexus 5 X** と **com.companyname.XamagonXuzzle** がそれぞれ該当します)。

    [![起動後のプロファイラー ウィンドウ](profiling-images/vswin/12-profiler-starts-sml.png)](profiling-images/vswin/12-profiler-starts.png#lightbox)

14. デバイスとデバッグ可能なプロセスが特定されると、Android Profiler によってアプリのプロファイリングが開始されます。

    [![実行中のアプリに対するプロファイラーの表示](profiling-images/vswin/13-profiler-running-sml.png)](profiling-images/vswin/13-profiler-running.png#lightbox)

15. **XamagonXuzzle** 上の **[RANDOMIZE]\(ランダム化\)** ボタンをタップすると (これによって、タイルがシフトおよびランダム化されます)、アプリのランダム化の間隔中に CPU の使用率が高くなるのがわかります。

    [![[RANDOMIZE]\(ランダム化\) ボタンをタップしたときの CPU 使用率](profiling-images/vswin/14-tap-randomize-sml.png)](profiling-images/vswin/14-tap-randomize.png#lightbox)

### <a name="using-the-android-profiler"></a>Android Profiler の使用

Android Profiler の使用の詳細については、[Android Studio のドキュメント](https://developer.android.com/studio/profile/android-profiler.html)を参照してください。
次のトピックは、Xamarin Android 開発者を対象としています。

- [CPU Profiler](https://developer.android.com/studio/profile/cpu-profiler.html) &ndash; アプリの CPU 使用率とスレッド アクティビティをリアルタイムで検査する方法について説明します。

- [Memory Profiler](https://developer.android.com/studio/profile/memory-profiler.html) &ndash; アプリのメモリ使用量のリアルタイム グラフを表示します。分析のためにメモリ割り当てを記録するボタンが含まれています。

- [Network Profiler](https://developer.android.com/studio/profile/network-profiler.html) &ndash; アプリによって送受信されるデータのリアルタイムでのネットワーク アクティビティを表示します。
