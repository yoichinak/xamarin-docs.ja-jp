---
title: Xamarin Profiler
description: "このガイドでは、Xamarin プロファイラーの主な機能について説明します。 Xamarin アプリケーションをプロファイリングするため、プロファイラー、プロファイリング、および、使用すべき状況、および標準的なワークフローを探します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 9c95a1b71f83ee810b775420aab3ceafeeca0379
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_このガイドでは、Xamarin プロファイラーの主な機能について説明します。Xamarin アプリケーションをプロファイリングするため、プロファイラー、プロファイリング、および、使用すべき状況、および標準的なワークフローを探します。_

アプリケーションの成功した場合は、エンド ユーザー エクスペリエンスに依存します。 開発者としてする可能性がありますが実装本当に素晴らしいの一部の機能、アプリが、アプリが動作が遅くまたはクラッシュのフルの場合は、ユーザーは可能性がありますを取り除くこと。

従来、モノラルが機能を備えた強力なコマンド ライン プロファイラー モノのランタイムで実行されているプログラムに関する情報の収集と呼ばれる、[モノラル ログ プロファイラー](http://www.mono-project.com/docs/debug+profile/profile/profiler/)です。 Xamarin プロファイラー モノラル ログ プロファイラーと Android、iOS、tvOS、し、Mac、および Android、ios、Mac アプリケーションおよび Windows 上の tvOS アプリケーションのプロファイリングをサポートするためのグラフィカル インターフェイスです。

Xamarin プロファイラーは、プロファイルの使用可能な機器の数-割り当て、サイクル、および時間プロファイラーです。 このガイドでは、これらの機器のメジャーし、アプリケーションを分析する方法について説明し、各画面に表示されるデータの意味を明確化します。

このガイドでは、プロファイリングの一般的なシナリオを調査し、分析し、iOS および Android アプリケーションを最適化するためのツールとして、プロファイラーを紹介します。

## <a name="contents"></a>目次

- [ダウンロードとインストール](#Download_and_Install)
- [プロファイラーおよびプロファイリング](#Profilers_and_Profiling)
- [Xamarin Profiler](#Xamarin_Profiler)
- [プロファイラーのサポート](#Profiler_Support)
- [プロファイラーの基礎](#Profiler_Basics)
    - [アプリのプロファイリングで許可します。](#Allowing_Profiling_in_your_App)
    - [プロファイラーを起動します。](#Launching_the_Profiler)
        - [Mac 用 Visual Studio から起動します。](#Launching_from_Xamarin_Studio)
        - [Visual Studio から起動します。](#Launching_from_Visual_Studio)
        - [保存と読み込みプロファイラー セッション](#Saving_and_Loading_Profiler_Sessions)
        - [プロファイラーの機能および機器](#Profiler_Features)
    - [割り当て](#Allocations)
    - [時間プロファイラー](#Time_Profiler)
    - [Cycles](#Cycles)
- [アプリケーションのプロファイリング](#Profiling_Applications)
- [まとめ](#Summary)

## <a name="download-and-install"></a>ダウンロードとインストール

> [!NOTE]
> **注:** mac 上の Mac の Windows 上のいずれかの Visual Studio Enterprise または Visual Studio でこの機能のロックを解除する Visual Studio Enterprise のサブスクライバーである必要があります

Xamarin プロファイラーは、スタンドアロン アプリケーションされ、IDE 内からのプロファイリングを有効にするには、Visual Studio for Mac と Visual Studio と統合されます。

### <a name="download"></a>ダウンロード

お使いのプラットフォーム用のインストール パッケージをダウンロードするには。

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

ダウンロードされると、Xamarin プロファイラーをシステムに追加するためにインストーラーを起動します。

IDE の統合は、Xamarin のすべてのリリース バージョンで使用可能です。
ただし、 [Visual Studio Enterprise](https://www.xamarin.com/compare-visual-studio)のプロファイルが必要です。



## <a name="profilers-and-profiling"></a>プロファイラーおよびプロファイリング

プロファイリングは、アプリケーションの開発で重要であり、見落とされがちなステップです。 形式は、プロファイリング**動的プログラム analysis** -は実行されている、使用中に、プログラムを分析します。 プロファイラーは、時間の複雑さ、特定のメソッドの使用法および割り当てられているメモリに関する情報を収集するデータ マイニング ツールです。 プロファイラーを使用すると、深層ドリル ダウンおよびをコード内の問題領域を特定するこれらのメトリックを分析できます。

設計とアプリケーションを開発、ときにすることが重要最適化処理の途中でです。つまり、費やしている時間はめったにアクセスするための領域で、コードを作成します。 これは、プロファイリングの機能です。 プロファイラーは、洞察が最もよく使用される基本、コードの部分と時刻のための強化を費やす必要があります領域を特定するのに役立ちますを提供します。 開発者は、アプリケーションでのほとんどの時間のかかった箇所と、アプリケーションによるメモリの使用方法を理解する注意する必要があります。

プロファイリングは、すべての種類の開発に便利ですがモバイル開発で特に重要です。 最適化されていないコードよりも、デスクトップ コンピューター上のモバイル プラットフォームで顕著は、アプリの成功が効率的に実行する美しいと最適化されたコードによって異なります。

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin プロファイラーからアプリケーションをプロファイルする方法を開発者に提供 Mac または Visual Studio の Visual Studio 内です。 プロファイラーは、収集し、し、使用できる開発者がアプリケーションの動作を分析すると、アプリに関する情報が表示されます。 Xamarin プロファイラーによるアプリケーションのプロファイリングをさまざまな方法の数があるメモリのプロファイリングおよび統計のサンプリングつまりです。 これらが実行されます。 割り当てと時間のプロファイラーをそれぞれをインストルメント化します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

現時点では、Xamarin プロファイラーは、Mac (を使用して Visual Studio for Mac) で、Xamarin.iOS、Xamarin.Android、および Xamarin.Mac のアプリケーションのテストに使用できます。 プロファイラーは、IDE から別のプロセスであり、に加えて、Mac を Visual Studio から起動する場合で利用できますスタンドアロン アプリケーションとして .exe を検査して`.mlpd`から生成されたファイル、[モノラル ログ プロファイラー](http://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

現時点では、Xamarin プロファイラーは、(Visual Studio と Visual Studio for Mac) 経由での Windows でアプリを Xamarin.Android テストに使用できます。 プロファイラーは、IDE から別のプロセスであり、に加えて、Visual Studio から起動する場合で利用できますスタンドアロン アプリケーションとして .exe を調べると`.mlpd`から生成されたファイル、[モノラル ログ プロファイラー](http://www.mono-project.com/docs/debug+profile/profile/profiler/).

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>プロファイラーのサポート

Xamarin プロファイラーのサポートは、次のプラットフォームで入手できます。

 - Visual Studio for Mac (macOS エンタープライズ ライセンス)
    - Android
        - デバイスおよびエミュレーター
    - iOS
        - デバイスおよびシミュレーターでは
    - tvOS (時間のインストルメント化がサポートされていません)
        - デバイスおよびシミュレーターでは
    - Mac

 - Visual Studio (だけ**エンタープライズ**バージョン)
    - Android
        - デバイスおよびエミュレーター
    - iOS の [試験的]
        - デバイスおよびシミュレーターでは
    - tvOS
        - デバイスおよびシミュレーターでは

可能なメモ**のみ**プロファイル**デバッグ**構成します。

## <a name="profiler-basics"></a>プロファイラーの基礎

このセクションでは、Xamarin プロファイラーの部分を紹介し、その機能を巡業です。

### <a name="allow-profiling-in-your-app"></a>アプリのプロファイリングで許可します。

正常にアプリをプロファイリングすることができます、前に、アプリのプロジェクトのオプションでプロファイルを使用できるようにする必要があります。

 - iOS:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  **ビルド > iOS デバッグ > プロファイリングを有効にします。**

  ![](images/ios-options-mac.png "iOS Mac 用の Visual Studio でのオプション ダイアログ ボックス")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **プロパティ > iOS ビルド > プロファイリングを有効にします。**

  ![](images/ios-project-options-vs.png "iOS Visual Studio でのオプション ダイアログ ボックス")

-----

 - Android:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  **ビルド > Android のデバッグ > 開発者インストルメンテーションを有効にします。**

  ![Android オプション ダイアログ ボックスで、Visual Studio for Mac](images/android-project-options.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  **ビルド > Android のデバッグ > 開発者インストルメンテーションを有効にします。**

  ![Android オプション ダイアログ ボックスで、Visual Studio for Mac](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>プロファイラーを起動します。

Xamarin プロファイラーは、iOS または Android のアプリケーションをプロファイリングする場合は、IDE からまたはスタンドアロン アプリケーションとして起動できます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

#### <a name="launching-from-visual-studio-for-mac"></a>Mac 用 Visual Studio から起動します。

1. まず、for Mac を Visual Studio に読み込まれて、アプリケーションがあるし、(既定) のデバッグ構成を選択することを確認してください。
2. 参照**実行 > プロファイリングの開始**for Mac を Visual Studio でまたは**分析 > Xamarin プロファイラー** Visual Studio で、次の図に示すように、プロファイラーを開きます。

  ![](images/start-profiling-xs.png "Mac 用 Visual Studio からプロファイラーを起動します。")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

#### <a name="launching-from-visual-studio"></a>Visual Studio から起動します。

1.  まず、上で指定した (既定) のデバッグ構成を選択し、Visual Studio に読み込まれているアプリケーションをしたことを確認してください。
2.  参照**分析 > Xamarin プロファイラー** Visual Studio で、次の図に示すように、プロファイラーを開きます。

![Visual Studio からプロファイラーを起動します。](images/start-profiling-vs.png)

-----

メニュー項目が表示されない場合を参照してください、[トラブルシューティング ガイド](~/tools/profiler/troubleshooting.md)です。

これにより、プロファイラーを起動し、アプリケーションのプロファイリングを自動的に開始します。

プロファイラーは、メモリおよびパフォーマンスの測定に使用できます。 割り当てと時間プロファイラーで、これを行う方法で、次のセクションで詳しく学びます。

#### <a name="saving-and-loading-profiler-sessions"></a>保存と読み込みプロファイラー セッション

いつでも、プロファイル セッションを保存する**ファイル > 名前を付けて保存しています.**プロファイラーのメニュー バーからです。 これでファイルが保存_mlpd_形式、プロファイリング データの圧縮率の高いは、特別な形式です。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

インストールされた後、Xamarin プロファイラーは含まれて、アプリケーション フォルダー下のスクリーン ショットに示すように。

![](images/applications.png "Mac からスタンドアロン プロファイラーを開く")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

インストールされた後、Xamarin プロファイラー アプリケーションは含まれて、アプリケーション ディレクトリ。

![](images/applications-vs.png "Windows からスタンドアロン プロファイラーを開く ")

-----

読み込むことができます*.mlpd*スタンドアロン アプリケーションを開くことによってプロファイラーにファイルを選択すると**選択対象**ファイルの読み込みとします。

詳細については、次を参照してください。 [.mlpd ファイルを生成する](~/tools/profiler/troubleshooting.md#gen_mlpd)です。


## <a name="profiler-features"></a>プロファイラーの機能

Xamarin プロファイラーは、以下に示すように 5 つのセクションで構成されます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](images/profiler-mac-sml.png "Mac 用の Visual Studio でのセクションではプロファイラー")](images/profiler-mac.png) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/profiler-vs.png "Visual Studio でのセクションではプロファイラー")](images/profiler-vs.png)

-----

- **ツールバー** – プロファイラーの上部にある、このオプション、プロファイリングの開始/停止をターゲット プロセスを選択、アプリの実行時間を表示および提供 profiler アプリケーションを構成する分割されたビューを選択します。
- **一覧をインストルメント化**– プロファイリング セッションに読み込まれるすべての機器が表示されます。
- **チャートのプロット**– これらのグラフがインストルメント化リストに関連する機器に水平方向に関連します。 スケールを変更する (時間のプロファイラーの下に表示)、スライダーを使用できます。
- **詳細領域をインストルメント化**-現在の機器の選択したビューによって表示されるデータが含まれています。 これらのビューを以下のセクションで詳しく取り上げます。
- **インスペクター ビュー** – このセクションでには、セグメント化されたコントロールで選択できるが含まれています。 セクションでは、選択すると、インストルメント化に依存するためが含まれています。 構成設定、統計、スタック トレース情報、およびルートへのパス。

### <a name="allocations"></a>割り当て

作成して、ガベージ コレクションの対象として、割り当て方法はアプリケーション内のオブジェクトに関する詳細な情報を提供します。

プロファイラーの上部にある、一定の間隔で、プロファイリング中に割り当てられたメモリの量を表示する割り当てチャートです。 現在割り当てグラフは割り当ての合計数と、ヒープのサイズではなく、その時点でです。 意味では接続されません、増加するだけです。 これには、スタックに割り当てられたオブジェクトが含まれます。 ランタイム バージョンによって使用される、グラフを異なる: 同じアプリにも確認できます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](images/allocations1.png "割り当てのインストルメント化")](images/allocations1.png) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/allocations1-vs.png "割り当てのインストルメント化")](images/allocations1-vs.png)

-----

開発者が、アプリケーションを使用して、メモリを解放する方法を分析できるように割り当て機器で別のデータ ビューがあります。 これらのビューを以下に示します。

 -   **割り当て**– これのすべての割り当ての一覧に表示され、クラス名別にグループ化します。 これは、クラスとメソッドが使用されている、使用する頻度と使用されるクラスの集合的なサイズの大きな概要を説明します。 表示は、クラスをダブルクリックすると、割り当てられたメモリ。 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations3.png "割り当て タブ")](images/allocations3.png) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations2-vs.png "割り当て タブ")](images/allocations2-vs.png)

-----

割り当てのインスペクター ビューはスタック トレースとパスのルートにフィルター処理し、オブジェクトをグループ化、およびオプションを割り当て済みメモリ上の統計情報を提供する、最上位の割り当て、およびビューを示します。

 -   **コール ツリー** – これには、アプリケーションでのすべてのスレッド全体のコール ツリーに表示され、各ノードに割り当てられたメモリに関する情報が含まれています。 すべての兄弟ノードが表示されます、リストに要素を選択すると、灰色。 ツリーを展開したり、要素にドリル ダウンをダブルクリックしてできます。このデータ ビューを表示するときが表示される方法を変更するインスペクター ビューの表示設定を使用できます。 現在、2 つのオプションがあります。
    1.  **コール ツリーは逆に**– 上から下へのスタック トレースをこのと見なします。 これは便利な表示オプション、最下位の方法を示すように、CPU がされた時間を費やしているです。
    2.  **別のスレッドによって**– このオプションは、スレッドによって、コール ツリーを整理します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations2.png "コール ツリー タブ")](images/allocations2.png) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations3-vs.png "コール ツリー タブ")](images/allocations3-vs.png)

-----

 -   **スナップショット**– このペインは、メモリのスナップショットについての情報を表示します。 ライブ アプリケーションのプロファイリング中にこれらを生成する をクリックして、_カメラ_どのようなメモリが保持され、リリースを表示したい各ポイントにあるツールバーでボタンをクリックします。 内部で何が起こっているかを調べるには、各スナップショットをクリックできます。 アプリをプロファイリング実行中はスナップショットを取得のみことに注意してください。 

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/allocations4.png "スナップショット タブ")](images/allocations4.png) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/allocations4-vs.png "スナップショット タブ")](images/allocations4-vs.png)

-----

### <a name="time-profiler"></a>時間プロファイラー

時間のプロファイラーのインストルメント化では、正確にどの程度の時間、アプリケーションの各メソッドでを測定します。 一定の間隔で、アプリケーションが一時停止し、アクティブな各スレッドのスタック トレースを実行します。 インストルメント化詳細領域内の各行が守られている実行パスを示します。

プロット グラフ、下のスクリーン ショットに示すように実行するときに、アプリで受信されたサンプルの数が表示されます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![時間のプロファイラーのインストルメント化](images/time1.png)](images/time1.png) 

[![プロファイラーのインストルメント化 – のサンプル一覧を時刻します。](images/time3.png)](images/time3.png) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![時間のプロファイラーのインストルメント化](images/time1-vs.png)](images/time1-vs.png) 

[![プロファイラーのインストルメント化 – のサンプル一覧を時刻します。](images/time3-vs.png)](images/time3-vs.png) 

-----


- **コール ツリー** – 各メソッドに費やされた時間数を示します。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

  [![](images/time2.png "時間プロファイラー インストルメント – コール ツリー")](images/time2.png) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

  [![](images/time2-vs.png "時間プロファイラー インストルメント – コール ツリー")](images/time2-vs.png) 

-----

### <a name="cycles"></a>サイクル

C# および f# マネージ コードを使用して、非常に一般的でおよびが破棄されることはありませんがオブジェクトへの参照を作成する残念ながら非常に簡単にできます。 この intrument を使用すると、それらのオブジェクトを特定して、アプリケーション内で参照サイクルを表示できます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![サイクルのインストルメント化](images/cycles-vs.png)](images/time1-vs.png) 

-----

## <a name="profiling-applications"></a>アプリケーションのプロファイリング

現時点では、既定のデバッグ構成のみをプロファイルできます。

その他の構成でアプリをプロファイリングする場合、次のメッセージ ダイアログが表示されます。


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![エラー ダイアログのプロファイリング](images/image001.png)](images/image001.png) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](images/image1vs.png "エラー ダイアログのプロファイリング")](images/image1vs.png) 

-----


選択**更新**を続行します。

<!---
##Profiling Android Applications


Due to the recent inclusion of the profiling libraries into any new Android project template, you will find that when profiling any legacy applications you are greeted with the message dialog above.

You will need to enable this to make sure that the profiling libraries are included in your Android application, for debug builds. This should not be checked for release builds as it creates overhead.


##Profiling iOS Applications

### Profiling tvOS

## Profiling Mac Applications
-->

### <a name="sgen-garbage-collector-and-profiling"></a>SGen ガベージ コレクターとプロファイリング

[SGen](http://www.mono-project.com/docs/advanced/garbage-collector/sgen/)ガベージ コレクターはすべて Xamarin プラットフォームを使用します。

SGen は、次の 3 つのヒープにアプリケーションのオブジェクトによって割り当てられた世代 GC — 収集家、主要なヒープと大きなオブジェクトの容量。 これにより、ガベージ コレクションの処理が高速実行します。 SGen が Xamarin.Android の既定の GC では現在、Xamarin.iOS 統合アプリケーション。

従来の API を使用して Xamarin.iOS アプリケーションには、Boehm GC – 保守的な非世代のガベージ コレクターが使用されます。 保守的なは、可能性は低くなります、プロファイラーを使用する場合、不正確な結果につながることが、使用可能なメモリを解放します。 このため、割り当て方法、Boehm ガベージ コレクターで使用できません。

求められますメッセージ ダイアログ ボックスでアプリが Boehm GC を使用する場合、中に Xamarin を推奨しません慎重に検討し、徹底的なテストにせずに SGen Boehm を使用する既存の iOS アプリケーションを切り替えます。 また、Xamarin はのプロファイリングとこれらの結果はメモリ使用量の正確なベンチマーク実行できないため、元に戻す切り替え SGen への切り替えを推奨していません。

メモリ管理の詳細についてを参照してください、[メモリおよびパフォーマンスのベスト プラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)ガイドです。

## <a name="summary"></a>まとめ

このガイドではどのようなプロファイルとの開発者にとって便利な方法について説明しました。 Xamarin プロファイラーは、いくつかの履歴とそのしくみに情報の提供を紹介します。 最後に、Xamarin プロファイラーの機能を付けましたし、割り当ておよび時間プロファイラー機器について説明します。


## <a name="related-links"></a>関連リンク

- [メモリおよびパフォーマンスのベスト プラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [リリース ノート](https://developer.xamarin.com/releases/profiler/preview/)
