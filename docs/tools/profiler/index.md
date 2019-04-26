---
title: Xamarin Profiler
description: このガイドでは、Xamarin Profiler の主な機能について説明します。 これにより、プロファイラーがプロファイリング、およびときに使用する必要があります、および標準的なワークフローが Xamarin アプリケーションのプロファイリングを探します。
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: lobrien
ms.author: laobri
ms.date: 06/03/2018
ms.openlocfilehash: 15739ff191953e4730d44c6ad9f63dccb9a0017e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61377109"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_このガイドでは、Xamarin Profiler の主な機能について説明します。これにより、プロファイラーがプロファイリング、およびときに使用する必要があります、および標準的なワークフローが Xamarin アプリケーションのプロファイリングを探します。_

アプリケーションの成功は、エンド ユーザー エクスペリエンスに依存します。 開発者として可能性がありますに実装したいくつかの非常に魅力的な機能、アプリが、アプリが動作の遅いまたはフル クラッシュの場合は、ユーザーは可能性がありますを取り除くことです。

従来は、Mono が機能を備えた強力なコマンド ライン プロファイラーと呼ばれる、Mono ランタイムで実行されているプログラムに関する情報を収集、 [Mono ログ プロファイラー](https://www.mono-project.com/docs/debug+profile/profile/profiler/)します。 Xamarin Profiler Mono ログ プロファイラーでは、および Android、iOS、tvOS、および Mac、および Android、ios、Mac アプリケーションおよび Windows 上の tvOS アプリケーションのプロファイリングをサポートするためのグラフィカル インターフェイスです。

Xamarin Profiler がプロファイリングの使用可能な instruments 数-割り当て、サイクル、および時間 Profiler します。 このガイドでは、これらのインストルメントで測定し、アプリケーションの分析方法について説明し、各画面に表示されるデータの意味を明確に。

このガイドは、一般的なプロファイリング シナリオにおけるを調べ、分析し、iOS および Android アプリケーションを最適化するためのツールとして、プロファイラーが導入されています。

## <a name="download-and-install"></a>ダウンロードとインストール

> [!NOTE]
> する必要があります、 [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/) for mac で Mac に Windows のいずれかの Visual Studio Enterprise または Visual Studio でこの機能のロックを解除するサブスクライバー

Xamarin Profiler は、スタンドアロン アプリケーションでは、され、IDE 内からのプロファイリングを有効にするには、Visual Studio for Mac と Visual Studio と統合されます。

お使いのプラットフォームのインストール パッケージをダウンロードするには。

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

ダウンロードされると、Xamarin Profiler をシステムに追加するインストーラーを起動します。

## <a name="profilers-and-profiling"></a>プロファイラーとプロファイリング

プロファイリングは、アプリケーションの開発で重要で見落とされがちなステップです。 形式は、プロファイリング**プログラムの動的分析**-は実行されている、使用中に、プログラムを分析します。 プロファイラーは、時間の複雑さ、特定のメソッドの使用状況、および割り当てられているメモリに関する情報を収集するデータ マイニング ツールです。 プロファイラーを使用すると、詳細なドリルダウンし、コードの問題領域を正確にこれらのメトリックを分析できます。

設計と、アプリケーションを開発、ことができません。 途中で最適化するために重要です。アクセス頻度の少ないがある領域で、コードの開発の時間を費やしています。 これは、プロファイリングの機能です。 プロファイラーは、洞察、最も一般的に使用される、コード ベースの部分と時刻の強化機能のことを費やす必要がありますな領域を特定するのに役立ちますを提供します。 開発者は、アプリケーションでは、ほとんどの時間を要した箇所と、アプリケーションによるメモリの使用方法を理解する注意する必要があります。

プロファイルは、開発のすべての種類で便利ですが、モバイル開発で特に重要です。 最適化されていないコードがよりも、デスクトップ コンピューター上のモバイル プラットフォームで顕著にあり、アプリの成功が効率的に実行する美しいと最適化されたコードによって異なります。

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin Profiler からアプリケーションをプロファイルする方法を開発者に提供 Visual Studio for Mac または Visual Studio 内で。 プロファイラーが収集し、アプリケーションの動作を分析する開発者で、使用すると、アプリに関する情報を表示します。 Xamarin Profiler を使用したアプリケーションをプロファイルするさまざまな方法がいくつかメモリのプロファイリングと統計サンプリング namely します。 割り当てと時間 Profiler を介して実行これらそれぞれをインストルメント化します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

現時点では、Xamarin Profiler を使用して、Mac (を使用して Visual Studio for Mac) で、Xamarin.iOS、Xamarin.Android、Xamarin.Mac アプリケーションをテストすることができます。 プロファイラーは、IDE から別のプロセスしから Visual Studio for Mac を起動、だけでなく、使えますスタンドアロン アプリケーションとして .exe を確認して`.mlpd`から生成されているファイル、 [mono ログ プロファイラー](https://www.mono-project.com/docs/debug+profile/profile/profiler/).

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

現時点では、Xamarin Profiler を使用して、(Visual Studio と Visual Studio for Mac) 経由での Windows で Xamarin.Android アプリをテストできます。 プロファイラーは、IDE から別のプロセスと、だけでなく、Visual Studio から起動できますスタンドアロン アプリケーションとして .exe を調べると`.mlpd`から生成されているファイル、 [mono ログ プロファイラー](https://www.mono-project.com/docs/debug+profile/profile/profiler/).

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>Profiler のサポート

Xamarin Profiler のサポートは、次のプラットフォームで入手できます。

 - Visual Studio for Mac (macOS エンタープライズ ライセンス)
    - Android
        - デバイスとエミュレーター
    - iOS
        - デバイスとシミュレーター
    - tvOS (時間のインストルメント化がサポートされていません)
        - デバイスとシミュレーター
    - Mac

 - Visual Studio (だけ**Enterprise**バージョン)
    - Android
        - デバイスとエミュレーター
    - iOS の [試験的]
        - デバイスとシミュレーター
    - tvOS
        - デバイスとシミュレーター

できます**のみ**プロファイル**デバッグ**構成します。

## <a name="profiler-basics"></a>Profiler の基礎

このセクションでは、Xamarin Profiler の一部を紹介し、その機能を紹介します。

### <a name="allow-profiling-in-your-app"></a>アプリのプロファイリングで許可します。

正常にアプリをプロファイリングすることができます、前に、アプリのプロジェクト オプションでプロファイルを可能にする必要があります。

 - iOS の場合:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  **ビルド > iOS デバッグ > プロファイルの有効化**

  ![](images/ios-options-mac.png "iOS では、Visual Studio for Mac のオプション ダイアログ ボックス")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **プロパティ > iOS ビルド > プロファイルの有効化**

  ![](images/ios-project-options-vs.png "iOS では、Visual Studio のオプション ダイアログ ボックス")

-----

 - Android:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  **ビルド > Android のデバッグ > 開発者のインストルメンテーションを有効にします。**

  ![Visual studio for Mac の android オプション ダイアログ](images/android-project-options.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **ビルド > Android のデバッグ > 開発者のインストルメンテーションを有効にします。**

  ![Visual studio for Mac の android オプション ダイアログ](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>Profiler を起動します。

IOS または Android のアプリケーションをプロファイリングする場合、IDE から、またはスタンドアロン アプリケーションとして、Xamarin Profiler を起動できます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="launching-from-visual-studio-for-mac"></a>Visual Studio for Mac を起動します。

1. まず、for Mac、Visual Studio に読み込まれて、アプリケーションがあるし、(既定値) のデバッグ構成を選択することを確認してください。
2. 参照**実行 > プロファイリングの開始**Visual studio for Mac、または**分析 > Xamarin Profiler** Visual studio で次の図に示すように、Profiler を開きます。

  ![](images/start-profiling-xs.png "Mac を Visual Studio から Profiler を起動します。")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

#### <a name="launching-from-visual-studio"></a>Visual Studio から起動します。

1.  まず、Visual Studio に読み込まれたアプリケーションを選択し、上で指定した (既定) のデバッグ構成を選択することを確認してください。
2.  参照**分析 > Xamarin Profiler** Visual studio で次の図に示すように、Profiler を開きます。

![Visual Studio から Profiler を起動します。](images/start-profiling-vs.png)

-----

メニュー項目が表示されない場合を参照してください、[トラブルシューティング ガイド](~/tools/profiler/troubleshooting.md)します。

これは、Profiler を起動し、アプリケーションのプロファイリングを自動的に開始されます。

メモリとパフォーマンスを測定する、Profiler を使用できます。 これを割り当てと時間 Profiler 行います instruments で、次のセクションで詳しく紹介します。

#### <a name="saving-and-loading-profiler-sessions"></a>保存と読み込み Profiler セッション

いつでも、プロファイル セッションを保存する**ファイル > 名前を付けて保存.** Profiler メニュー バーから。 これでファイルが保存_mlpd_形式、プロファイリング データの圧縮率の高い、特別な形式です。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

インストールされた後、次のスクリーン ショットに示すように、Xamarin Profiler は、アプリケーション フォルダーに格納されたことができます。

![](images/applications.png "Mac からスタンドアロン Profiler を開く")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

インストールされた後、アプリケーション ディレクトリ内と、Xamarin Profiler アプリケーションはあります。

![](images/applications-vs.png "Windows からスタンドアロン Profiler を開く ")

-----

読み込むことができます *.mlpd* 、スタンドアロン アプリケーションを開くことで Profiler にファイルを選択すると**ターゲットの選択**ファイルの読み込みとします。

詳細については、次を参照してください。 [.mlpd ファイルを生成する](~/tools/profiler/troubleshooting.md#gen_mlpd)します。

## <a name="profiler-features"></a>Profiler の機能

Xamarin Profiler は、以下に示す 5 つのセクションで構成されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](images/profiler-mac-sml.png "Visual Studio for Mac での Profiler セクション")](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](images/profiler-vs.png "Visual Studio での Profiler セクション")](images/profiler-vs.png#lightbox)

-----

- **ツールバー** – プロファイラーの上部にある、このオプションのプロファイリングを開始/停止するターゲット プロセスの選択、アプリの実行時間を表示と提供 profiler アプリケーションを構成する分割ビューを選択します。
- **一覧をインストルメント化**– プロファイル セッションに読み込まれたすべての機器が一覧表示されます。
- **グラフのプロット**– これらのグラフは、インストルメント化リストに関連する instruments に水平方向に関連します。 スケールを変更します (時間の Profiler の下に表示される) のスライダーを使用できます。
- **詳細領域をインストルメント化**-現在のインストルメント化の選択したビューによって表示されているデータが含まれています。 これらのビューを以下のセクションで詳しく取り上げます。
- **インスペクター ビュー** – セグメント付きコントロールで選択可能なセクションが含まれます。 セクションは、選択すると、インストルメント化に依存するためが含まれています。構成設定、統計、スタック トレース情報、およびルートへのパス。

### <a name="allocations"></a>割り当て

割り当てのインストルメント化が作成されると、ガベージ コレクションのアプリケーション内のオブジェクトに関する詳細な情報を提供します。

プロファイラーの上部にある割り当てグラフでは、一定の間隔で、プロファイリング中に割り当てられたメモリの量を表示するにです。 現在、割り当てグラフは、割り当ての合計数と、ヒープのサイズではなく、その時点で。 ある意味は決してダウンがだけ大きくなります。 これには、スタックに割り当てられたオブジェクトが含まれます。 使用されるバージョンのランタイムに応じてグラフを異なる – 同じアプリの場合でも確認できます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![](images/allocations1.png "割り当てのインストルメント化")](images/allocations1.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](images/allocations1-vs.png "割り当てのインストルメント化")](images/allocations1-vs.png#lightbox)

-----

これにより、アプリケーションを使用して、メモリを解放する方法を分析する開発者割り当てインストルメント化で別のデータ ビューがあります。 これらのビューを次に示します。

 -   **割り当て**– これのすべての割り当ての一覧に表示され、クラス名ごとにグループ化します。 これは、クラスとメソッドが使用されている、使用頻度と使用されるクラスの集合的なサイズの概要を提供します。 クラスをダブルクリックすると、割り当てられたメモリが表示されます。 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![](images/allocations3.png "[割り当て] タブ")](images/allocations3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/allocations2-vs.png "[割り当て] タブ")](images/allocations2-vs.png#lightbox)

-----

割り当てのインスペクター ビューはスタック トレースとパスのルートにフィルター処理し、オブジェクトをグループ化、およびオプションを割り当て済みメモリの統計情報を提供する、上位の割り当て、およびビューを示します。

 -   **コール ツリー** – このアプリケーションのすべてのスレッド全体のコール ツリーを表示および各ノードに割り当てられたメモリに関する情報が含まれます。 すべての兄弟ノードが表示されます、リストの要素を選択すると、灰色。 ツリーを展開したりにドリルダウンする要素をダブルクリックします。このデータ ビューを表示するときに、提示される方法を変更する表示設定のインスペクター ビューを使用できます。 現在、2 つのオプションがあります。
    1.  **コール ツリーを反転**– 上から下へのスタック トレースをこれと見なします。 これは、便利な表示オプション、最下位の方法を示すため、CPU がされている時間を費やしています。
    2.  **別のスレッドによって**– このオプションは、スレッドがコール ツリーを整理します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![](images/allocations2.png "呼び出しツリー タブ")](images/allocations2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/allocations3-vs.png "呼び出しツリー タブ")](images/allocations3-vs.png#lightbox)

-----

 -   **スナップショット**– このペインは、メモリのスナップショットに関する情報を表示します。 ライブ アプリケーションのプロファイリング中にこれらを生成する をクリックして、_カメラ_をどのようなメモリが保持され、リリースを確認したい各ポイントにあるツールバーでボタンをクリックします。 内部的には何が起こっているかを確認するには、各スナップショットをクリックできます。 アプリをプロファイリング実行中はスナップショットを取得のみことに注意してください。 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![](images/allocations4.png "スナップショット タブ")](images/allocations4.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/allocations4-vs.png "スナップショット タブ")](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>時間の Profiler

時間の Profiler のインストルメント化は、アプリケーションの各メソッドにどれ時間だけが正確に費やされたを測定します。 一定の間隔で、アプリケーションが一時停止し、アクティブな各スレッドでスタック トレースを実行します。 インストルメント化の詳細領域内の各行が守られている実行パスを示します。

プロット グラフで、次のスクリーン ショットに示すように実行するときに、アプリで受信されたサンプルの数が表示されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![時間の Profiler のインストルメント化](images/time1.png)](images/time1.png#lightbox) 

[![インストルメント化 Profiler 時間 – のサンプル一覧](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![時間の Profiler のインストルメント化](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![インストルメント化 Profiler 時間 – のサンプル一覧](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **コール ツリー** – 各メソッドで費やされた時間数を示しています。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![](images/time2.png "インストルメント化 Profiler 時間 – コール ツリー")](images/time2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![](images/time2-vs.png "インストルメント化 Profiler 時間 – コール ツリー")](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>サイクル

使用によりC#とF#マネージ コードでは、非常に一般的と破棄されることはありませんが、オブジェクトへの参照を作成する残念ながら非常に簡単であることができます。 この機器を使用すると、それらのオブジェクトを特定し、アプリケーションで参照されているサイクルを表示できます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![サイクルのインストルメント化](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![サイクルのインストルメント化](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>アプリケーションのプロファイリング

現時点では、既定のデバッグ構成のみをプロファイルすることができます。

その他の構成を使用してアプリをプロファイリングする場合、次のメッセージ ダイアログが表示されます。


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![エラー ダイアログのプロファイリング](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](images/image1vs.png "エラー ダイアログのプロファイリング")](images/image1vs.png#lightbox) 

-----

選択**Update**を続行します。

### <a name="sgen-garbage-collector-and-profiling"></a>SGen ガベージ コレクターとプロファイリング

[SGen](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/)ガベージ コレクターはすべての Xamarin プラットフォームを使用します。

SGen 世代別の GC でアプリケーションを 3 つのヒープのオブジェクトによって割り当てられたは、新世代、メジャー ヒープと大きなオブジェクトの容量。 これにより、ガベージ コレクションの処理が高速に実行できます。 SGen は、Xamarin.Android 向けの既定の GC では現在と Xamarin.iOS Unified アプリケーション。

従来の API を使用して Xamarin.iOS アプリケーションでは、Boehm GC – 保守的な非世代別ガベージ コレクターを使用します。 控えめなは、プロファイラーを使用する場合、不正確な結果につながることが、使用可能なメモリを解放する可能性が低くなります。 このため、Boehm ガベージ コレクターを使用した割り当て方法を使用できません。

求められますメッセージ ダイアログ ボックスで、アプリが Boehm GC を使用している場合、中に Xamarin をお勧めしません Boehm SGen を慎重に検討し、徹底的なテストを使用して、既存の iOS アプリケーションを切り替えます。 また、Xamarin はプロファイリングと、これらの結果は正確なベンチマークのメモリ使用量を指定しないため、切り替えの SGen への切り替えを推奨していません。

メモリ管理の詳細についてを参照してください、[メモリとパフォーマンスのベスト プラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)ガイド。

## <a name="summary"></a>まとめ

このガイドではどのようなプロファイルでは開発者にとって便利な方法について説明しました。 Xamarin Profiler は、一部の履歴とそのしくみに情報を提供することを紹介します。 最後に、Xamarin Profiler の機能に遠征し、割り当てと Profiler Instruments の時間について説明しました。

## <a name="related-links"></a>関連リンク

- [メモリとパフォーマンスのベスト プラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [リリース ノート](https://developer.xamarin.com/releases/profiler/preview/)
