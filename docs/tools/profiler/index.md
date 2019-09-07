---
title: Xamarin Profiler
description: このガイドでは、Xamarin Profiler の主な機能について説明します。 ここでは、プロファイラー、プロファイリング、使用する時期、および Xamarin アプリケーションをプロファイリングするための標準的なワークフローについて説明します。
ms.prod: xamarin
ms.assetid: 3247fcee-6acc-470d-ab87-c1c511d67363
author: conceptdev
ms.author: crdun
ms.date: 06/03/2018
ms.openlocfilehash: 745c59ad50f0e8ad50a8ec56549d99b7b5e72228
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70772470"
---
# <a name="xamarin-profiler"></a>Xamarin Profiler

_このガイドでは、Xamarin Profiler の主な機能について説明します。ここでは、プロファイラー、プロファイリング、使用する時期、および Xamarin アプリケーションをプロファイリングするための標準的なワークフローについて説明します。_

アプリケーションの成功は、エンドユーザーエクスペリエンスによって異なります。 開発者はアプリに非常に優れた機能を実装しているかもしれませんが、アプリのクラッシュが遅い場合や、クラッシュが発生する場合は、ユーザーがそれを削除する可能性があります。

Mono は、mono の[ログプロファイラー](https://www.mono-project.com/docs/debug+profile/profile/profiler/)と呼ばれる mono ランタイムで実行されているプログラムに関する情報を収集するための強力なコマンドラインプロファイラーを備えています。 Xamarin Profiler Mono ログプロファイラー用のグラフィカルインターフェイスであり、Mac 上の Android、iOS、tvOS、および Mac の各アプリケーションのプロファイルと、Windows 上の Android、iOS、tvOS アプリケーションをサポートしています。

Xamarin Profiler には、プロファイリング (割り当て、サイクル、タイムプロファイラー) に使用できるさまざまな音色があります。 このガイドでは、これらの音色がどのように測定されるか、および各画面に表示されるデータの意味を明確にする方法について説明します。

このガイドでは、一般的なプロファイルシナリオについて説明し、iOS および Android アプリケーションの分析と最適化を支援するツールとしてプロファイラーを紹介します。

## <a name="download-and-install"></a>ダウンロードしてインストールする

> [!NOTE]
> Windows の Visual Studio Enterprise または Mac 上の Visual Studio for Mac でこの機能のロックを解除するには、 [Visual Studio Enterprise](https://visualstudio.microsoft.com/vs/compare/)サブスクライバーである必要があります。

Xamarin Profiler はスタンドアロンアプリケーションであり、IDE 内からプロファイリングを有効にするために Visual Studio for Mac と Visual Studio に統合されています。

プラットフォームのインストールパッケージをダウンロードします。

- [**macOS**](https://dl.xamarin.com/profiler/profiler-mac.pkg)
- [**Windows**](https://dl.xamarin.com/profiler/profiler-windows.msi)

ダウンロードが完了したら、インストーラーを起動して、システムに Xamarin Profiler を追加します。

## <a name="profilers-and-profiling"></a>プロファイラーとプロファイリング

プロファイリングは、アプリケーション開発の重要で見落とされることが多い手順です。 プロファイリングは、動的な**プログラム分析**の一種であり、プログラムの実行中および使用中に分析されます。 プロファイラーは、時間の複雑さ、特定のメソッドの使用状況、および割り当てられているメモリに関する情報を収集するデータマイニングツールです。 プロファイラーを使用すると、これらのメトリックを掘り下げて分析し、コード内の問題の領域を特定できます。

アプリケーションを設計および開発する際には、事前に最適化しないことが重要です。つまり、アクセス頻度の低い領域でコードを開発するために時間を費やすことになります。 これはプロファイルの機能です。 プロファイラーは、コードベースの最も一般的に使用される部分に関する洞察を提供し、改善のために時間を費やす必要がある領域を特定するのに役立ちます。 開発者は、ほとんどの時間がアプリケーションで費やされている場所と、アプリケーションでメモリがどのように使用されるかを理解する必要があります。

プロファイリングはあらゆる種類の開発に役立ちますが、特にモバイル開発では非常に重要です。 最適化されていないコードは、デスクトップコンピューターよりもモバイルプラットフォームではさらに顕著になります。アプリの成功は、効率的に実行される美しいコードと最適化されたコードに依存します。

## <a name="xamarin-profiler"></a>Xamarin Profiler

Xamarin Profiler を使用すると、開発者は Visual Studio for Mac または Visual Studio 内からアプリケーションをプロファイリングすることができます。 プロファイラーは、アプリに関する情報を収集して表示します。これは、開発者がアプリケーションの動作を分析するために使用できます。 Xamarin Profiler でアプリケーションのプロファイリングを行うには、さまざまな方法があります。つまり、メモリプロファイルと統計サンプリングです。 これらはそれぞれ、割り当てと時間プロファイラーの音色を通じて実行されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

現時点では、Xamarin Profiler を使用して、Mac 上の Xamarin アプリケーション (Visual Studio for Mac 経由) をテストできます。 プロファイラーは IDE とは別のプロセスであるため、Visual Studio for Mac から起動するだけでなく、それをスタンドアロンアプリケーションとして使用して、 `.mlpd` [mono ログプロファイラー](https://www.mono-project.com/docs/debug+profile/profile/profiler/)から生成された .exe とファイルを調べることもできます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

現時点では、Xamarin Profiler を使用して、Windows 上の Xamarin Android アプリをテストすることができます (Visual Studio および Visual Studio for Mac を使用)。 プロファイラーは IDE とは別のプロセスであるため、Visual Studio から起動するだけでなく、それをスタンドアロンアプリケーションとして使用して、 `.mlpd` [mono ログプロファイラー](https://www.mono-project.com/docs/debug+profile/profile/profiler/)から生成された .exe とファイルを調べることもできます。

-----

<a name="Profiler_Support" />

## <a name="profiler-support"></a>プロファイラーのサポート

Xamarin Profiler のサポートは、次のプラットフォームで使用できます。

- Visual Studio for Mac (macOS、エンタープライズライセンス)
  - Android
    - デバイスとエミュレーター
  - iOS
    - デバイスとシミュレーター
  - tvOS (時刻のインストルメント化はサポートされていません)
    - デバイスとシミュレーター
  - Mac

- Visual Studio ( **Enterprise**バージョンのみ)
  - Android
    - デバイスとエミュレーター
  - iOS [試験段階]
    - デバイスとシミュレーター
  - tvOS
    - デバイスとシミュレーター

**デバッグ**構成のプロファイル**のみ**が可能であることに注意してください。

## <a name="profiler-basics"></a>プロファイラーの基礎

このセクションでは、Xamarin Profiler の一部を紹介し、その機能について説明します。

### <a name="allow-profiling-in-your-app"></a>アプリでのプロファイリングを許可する

アプリを正常にプロファイリングするには、アプリのプロジェクトオプションでプロファイリングを許可する必要があります。

- IOS

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  **> IOS デバッグ > プロファイリングを有効にする**

  ![Visual Studio for Mac の [iOS オプション] ダイアログ](images/ios-options-mac.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **プロパティ > iOS ビルド > プロファイリングを有効にする**

  ![Visual Studio の [iOS オプション] ダイアログ](images/ios-project-options-vs.png)

-----

- Android

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  **Android デバッグ > 構築 > 開発者向けインストルメンテーションを有効にする**

  ![Visual Studio for Mac の [Android オプション] ダイアログ](images/android-project-options.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  **Android デバッグ > 構築 > 開発者向けインストルメンテーションを有効にする**

  ![Visual Studio for Mac の [Android オプション] ダイアログ](images/android-project-options-vs-sml.png)

-----

### <a name="launching-the-profiler"></a>プロファイラーの起動

Xamarin Profiler は、iOS または Android アプリケーションをプロファイルするときに IDE から起動することも、スタンドアロンアプリケーションとして起動することもできます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

#### <a name="launching-from-visual-studio-for-mac"></a>起動 (Visual Studio for Mac から)

1. 最初に、Visual Studio for Mac にアプリケーションが読み込まれていることを確認し、(既定) のデバッグ構成を選択します。
2. 次の図に示すように、[参照] を選択して Visual Studio for Mac で**プロファイリングを開始**するか、Visual Studio で **> Xamarin Profiler を分析**して、プロファイラーを開き > ます。

  ![Visual Studio for Mac からのプロファイラーの起動](images/start-profiling-xs.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

#### <a name="launching-from-visual-studio"></a>起動 (Visual Studio から)

1. まず、アプリケーションが Visual Studio に読み込まれていることを確認し、上で指定したように (既定の) デバッグ構成を選択します。
2. 次の図に示すように、Visual Studio で **> Xamarin Profiler**を参照して、プロファイラーを開きます。

![Visual Studio からのプロファイラーの起動](images/start-profiling-vs.png)

-----

メニュー項目が表示されない場合は、[トラブルシューティングガイド](~/tools/profiler/troubleshooting.md)を参照してください。

これにより、プロファイラーが起動し、アプリケーションのプロファイリングが自動的に開始されます。

プロファイラーを使用して、メモリとパフォーマンスを測定できます。 これは、次のセクションで詳しく説明する割り当てと時間プロファイラーの音色によって実現されます。

#### <a name="saving-and-loading-profiler-sessions"></a>プロファイラーセッションの保存と読み込み

プロファイリングセッションをいつでも保存するには、プロファイラーのメニューバーから **[ファイル > 名前を付けて保存...]** を選択します。 これにより、ファイルは、プロファイリングデータ用の特殊で高度に圧縮された形式で _.mlpd_形式で保存されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

インストールが完了すると、次のスクリーンショットに示すように、アプリケーションフォルダーに Xamarin Profiler があります。

![Mac からスタンドアロンのプロファイラーを開く](images/applications.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

インストールが完了すると、アプリケーションディレクトリに Xamarin Profiler アプリケーションがあります。

![Windows からスタンドアロンのプロファイラーを開く](images/applications-vs.png)

-----

スタンドアロンアプリケーションを開き、 **[ターゲットの選択]** を選択し、ファイルを読み込むことによって、プロファイラーに*mlpd*ファイルを読み込むことができます。

詳細については、「 [mlpd ファイルの生成](~/tools/profiler/troubleshooting.md#gen_mlpd)」を参照してください。

## <a name="profiler-features"></a>プロファイラーの機能

Xamarin Profiler は、次に示す5つのセクションで構成されています。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![Visual Studio for Mac のプロファイラーセクション](images/profiler-mac-sml.png)](images/profiler-mac.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Visual Studio のプロファイラーセクション](images/profiler-vs.png)](images/profiler-vs.png#lightbox)

-----

- **ツールバー** –プロファイラーの上部にあり、プロファイリングの開始/停止、ターゲットプロセスの選択、アプリの実行時間の表示、およびプロファイラーアプリケーションを構成する分割ビューの選択を行うためのオプションが用意されています。
- **[インストルメントの一覧]** –プロファイルセッション用に読み込まれたすべての音色が一覧表示されます。
- **プロットグラフ**–これらのグラフは、インストルメントリスト内の関連する音色に水平に関連付けられています。 スライダー (タイムプロファイラーの下に表示) を使用して、スケールを変更できます。
- **[インストルメントの詳細領域]** -現在のインストルメントの選択したビューで表示されるデータが含まれます。 これらのビューについては、以下のセクションで詳しく説明します。
- **Inspector View** –セグメント化されたコントロールで選択できるセクションが含まれます。 これらのセクションは、選択したインストルメントに依存し、次のものが含まれます。構成設定、統計情報、スタックトレース情報、およびルートへのパス。

### <a name="allocations"></a>割り当て

割り当てインストルメントは、アプリケーション内のオブジェクトの作成時およびガベージコレクション時の詳細情報を提供します。

プロファイラーの上部には、割り当てグラフが表示されます。このグラフには、プロファイリング中に一定間隔で割り当てられたメモリ量が表示されます。 現在、割り当てグラフは、割り当ての合計数であり、その時点でのヒープのサイズではありません。 意味があるので、このようなことは決して増加しません。 これには、スタック上に割り当てられたオブジェクトが含まれます。 使用されているランタイムのバージョンによっては、同じアプリであってもグラフが異なる外観になる場合があります。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![割り当てインストルメント](images/allocations1.png)](images/allocations1.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![割り当てインストルメント](images/allocations1-vs.png)](images/allocations1-vs.png#lightbox)

-----

割り当てインストルメントにはさまざまなデータビューがあります。これにより、開発者は、アプリケーションがどのようにメモリを使用してメモリを解放しているかを分析できます。 これらのビューについて以下に説明します。

- **割り当て**–すべての割り当ての一覧を表示し、クラス名でグループ化します。 これにより、使用されるクラスとメソッドの概要、使用頻度、および使用されるクラスの集合サイズが提供されます。 クラスをダブルクリックすると、割り当てられたメモリが表示されます。 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![[割り当て] タブ](images/allocations3.png)](images/allocations3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![[割り当て] タブ](images/allocations2-vs.png)](images/allocations2-vs.png#lightbox)

-----

割り当て用のインスペクタービューには、オブジェクトのフィルター処理とグループ化のオプション、割り当てられたメモリの統計情報、上位の割り当て、スタックトレースとルートへのパスのビューがあります。

- **[コールツリー]** –アプリケーション内のすべてのスレッドのコールツリー全体が表示され、各ノードに割り当てられたメモリに関する情報が含まれます。 一覧で要素が選択されると、すべての兄弟ノードが灰色で表示されます。 ツリーを展開するか、要素をダブルクリックしてドリルダウンすることができます。このデータビューを表示するときに、表示設定インスペクタービューを使用して、表示方法を変更できます。 現在、次の2つのオプションがあります。
    1. [反転された**コールツリー** ] –スタックトレースは上から下へと見なされます。 これは、CPU が時間を費やしている最も深い方法を示す、便利な表示オプションです。
    2. **スレッドで分離**-このオプションは、呼び出しツリーをスレッドごとに整理します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![[コールツリー] タブ](images/allocations2.png)](images/allocations2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![[コールツリー] タブ](images/allocations3-vs.png)](images/allocations3-vs.png#lightbox)

-----

- **スナップショット**–このペインには、メモリスナップショットに関する情報が表示されます。 ライブアプリケーションのプロファイリング中にこれらを生成するには、保持および解放されたメモリを確認する各ポイントで、ツールバーの [_カメラ_] ボタンをクリックします。 各スナップショットをクリックすると、内部で何が起こっているかを調べることができます。 スナップショットは、アプリをライブプロファイリングする場合にのみ実行できます。 

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![[スナップショット] タブ](images/allocations4.png)](images/allocations4.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![[スナップショット] タブ](images/allocations4-vs.png)](images/allocations4-vs.png#lightbox)

-----

### <a name="time-profiler"></a>時間プロファイラー

時間プロファイラーのインストルメントは、アプリケーションの各メソッドで費やされた時間を正確に測定します。 アプリケーションは一定の間隔で一時停止し、アクティブな各スレッドでスタックトレースが実行されます。 [インストルメントの詳細] 領域の各行には、その後に続く実行パスが表示されます。

次のスクリーンショットに示すように、プロットチャートには、アプリの実行中に受信したサンプルの数が表示されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![時間プロファイラーのインストルメント](images/time1.png)](images/time1.png#lightbox) 

[![時間プロファイラーインストルメント-サンプル一覧](images/time3.png)](images/time3.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![時間プロファイラーのインストルメント](images/time1-vs.png)](images/time1-vs.png#lightbox) 

[![時間プロファイラーインストルメント-サンプル一覧](images/time3-vs.png)](images/time3-vs.png#lightbox) 

-----

- **[コールツリー]** –各メソッドで費やされた時間が表示されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

  [![時間プロファイラーインストルメント-コールツリー](images/time2.png)](images/time2.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

  [![時間プロファイラーインストルメント-コールツリー](images/time2-vs.png)](images/time2-vs.png#lightbox) 

-----

### <a name="cycles"></a>サイクル

およびF#マネージコードをC#使用すると、非常に一般的になることがあります。また、残念ながら、破棄されないオブジェクトへの参照を作成するのは非常に簡単です。 このインストルメントを使用すると、これらのオブジェクトを特定し、アプリケーションで参照されているサイクルを表示できます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![サイクルの音色](images/cycles.m751-sml.png)](images/cycles.m751.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![サイクルの音色](images/cycles-vs-sml.png)](images/cycles-vs.png#lightbox) 

-----

## <a name="profiling-applications"></a>アプリケーションのプロファイリング

現時点では、プロファイルできるのは既定のデバッグ構成のみです。

他の構成でアプリをプロファイルすると、次のメッセージダイアログが表示されます。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![プロファイルエラーダイアログ](images/image001.png)](images/image001.png#lightbox) 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![プロファイルエラーダイアログ](images/image1vs.png)](images/image1vs.png#lightbox) 

-----

**[更新]** を選択して続行します。

### <a name="sgen-garbage-collector-and-profiling"></a>SGen ガベージコレクターとプロファイリング

[SGen](https://www.mono-project.com/docs/advanced/garbage-collector/sgen/)ガベージコレクターは、すべての Xamarin プラットフォームに使用されます。

SGen は、アプリケーションのオブジェクトを3つのヒープ (新世代、主要ヒープ、大きなオブジェクト領域) に割り当てる、世代の GC です。 これにより、ガベージコレクションの実行を処理できます。 現在、SGen は、Xamarin Android と Xamarin の統合アプリケーションの既定の GC です。

Classic API を使用する Xamarin アプリケーションでは、Boehm GC が使用されています。これは、控えめな非世代ガベージコレクターです。 控えめなことですが、使用可能なメモリが解放される可能性は低く、プロファイラーを使用した場合の結果が不正確になる可能性があります。 このため、割り当てインストルメントは Boehm ガベージコレクターでは使用できません。

アプリで Boehm GC が使用されている場合、メッセージダイアログが表示されますが、Xamarin では、Boehm を使用する既存の iOS アプリケーションを、慎重に調査して徹底的なテストを行わずに、SGen に切り替えることはお勧めしません。 Xamarin では、プロファイリング用に SGen に切り替えてから戻すことは推奨されません。これらの結果はメモリ使用量の正確なベンチマークを提供しないためです。

メモリ管理の詳細については、「[メモリとパフォーマンスのベストプラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)ガイド」を参照してください。

## <a name="summary"></a>まとめ

このガイドでは、プロファイルとは何か、および開発者にとってどのような利点があるかを見てきました。 次に、Xamarin Profiler を導入し、その動作についていくつかの履歴と情報を提供します。 最後に、Xamarin Profiler の機能をがし、割り当てと時間プロファイラーの音色について説明します。

## <a name="related-links"></a>関連リンク

- [メモリとパフォーマンスのベストプラクティス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [リリース ノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/profiler/preview/index.md)
