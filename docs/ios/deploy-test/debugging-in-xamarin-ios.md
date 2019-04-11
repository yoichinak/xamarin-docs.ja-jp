---
title: Xamarin.iOS アプリのデバッグ
description: このドキュメントでは、Visual Studio for Mac や Visual Studio 2019 のデバッガーを使用して、Xamarin.iOS アプリケーションをデバッグする方法について説明します。これには、ブレークポイントの設定やワイヤレス デバッグなどが含まれます。
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 59df461c52cd01187ca3a9fc25fe741342910061
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854536"
---
# <a name="debugging-xamarinios-apps"></a>Xamarin.iOS アプリのデバッグ

_Xamarin.iOS アプリケーションは Visual Studio for Mac や Visual Studio に内蔵されているデバッガーでデバッグできます。_

C# やその他のマネージド言語コードのデバッグには、Visual Studio for Mac のネイティブ デバッグ サポートを利用します。Xamarin.iOS プロジェクトにリンクする可能性がある C、C++、Objective C コードのデバッグには、[LLDB](http://lldb.llvm.org/tutorial.html) を利用します。


> [!NOTE]
> デバッグ モードでアプリケーションをコンパイルすると、Xamarin.iOS は動作が遅く、大容量のアプリケーションを生成します。すべてのコード行をインストルメント化しなければならないためです。 リリース前に、リリース ビルドを行ってください。

Xamarin.iOS デバッガーは IDE に統合されており、開発者はシミュレーターまたはデバイスで、Xamarin.iOS が対応している管理対象言語でビルドされた Xamarin.iOS アプリケーションをデバッグできます。

Xamarin.iOS デバッガーは [Mono Soft Debugger](https://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/) を利用します。つまり、生成されたコードと Mono ランタイムが IDE と連動してデバッグを行います。 これが LLDB や MDB のようなハード デバッガーとの違いです。ハード デバッガーは、デバッグされるプログラムから情報や協力を得ることなくプログラムを制御します。

## <a name="setting-breakpoints"></a>ブレークポイントの設定

アプリケーションのデバッグを始める準備ができたら、まずアプリケーションの[ブレークポイントを設定](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)します。 エディターの余白領域で、ブレークポイントにするコードの行番号の隣をクリックします。

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

[![](debugging-in-xamarin-ios-images/debugging1.png "ブレークポイントの設定")](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

[![](debugging-in-xamarin-ios-images/debugging1a.png "ブレークポイントの設定")](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

コードに設定したすべてのブレークポイントを表示するには、**[ブレークポイント] パッド**を開きます。

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

[![](debugging-in-xamarin-ios-images/image0a.png "[ブレークポイント] パッド")](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 [ブレークポイント] パッドが自動的に表示されない場合、_[表示]、[デバッグ ウィンドウ]、[ブレークポイント]_ の順に選択すると表示されます。
 
# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

[![](debugging-in-xamarin-ios-images/image0.png "[ブレークポイント] パッド")](debugging-in-xamarin-ios-images/image0.png#lightbox)

 [ブレークポイント] パッドが自動的に表示されない場合、_[デバッグ]、[ウィンドウ]、[ブレークポイント]_ の順に選択すると表示されます。
 
-----

アプリケーションのデバッグを始める前に、構成が **[デバッグ]** に設定されていることを必ず確認してください。この構成には、ブレークポイント、データ ビジュアライザーの使用、コール スタックの表示など、デバッグに役立つ便利なツールが揃っています。

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

[![](debugging-in-xamarin-ios-images/debugging7.png "シミュレーターでのデバッグ")](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7a.png "物理デバイスでのデバッグ")](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

[![](debugging-in-xamarin-ios-images/debugging7c.png "シミュレーターでのデバッグ")](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[![](debugging-in-xamarin-ios-images/debugging7d.png "物理デバイスでのデバッグ")](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>デバッグの開始
デバッグを開始するには、IDE でターゲット デバイスまたは類似するデバイスを選択します。

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

[![](debugging-in-xamarin-ios-images/debugging7b.png "ターゲット デバイスを選択する")](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

[![](debugging-in-xamarin-ios-images/debugging7e.png "ターゲット デバイスを選択する")](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----



**[再生]** ボタンを押してアプリケーションを展開します。

ブレークポイントにヒットすると、コードは黄色で強調表示されます。

[![](debugging-in-xamarin-ios-images/image2.png "コードが黄色で強調表示される")](debugging-in-xamarin-ios-images/image2.png#lightbox)

オブジェクト値の検査ツールなど、デバッグ ツールをこの時点で使用すると、コードの状態を詳しく確認できます。

[![](debugging-in-xamarin-ios-images/image3.png "色の値の表示")](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>条件付きブレークポイント

ブレークポイントが発生する状況を指示した規則を設定することもできます。これは*条件付きブレークポイント*の追加とも呼ばれます。

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

条件付きブレークポイントを設定するには、**[ブレークポイントのプロパティ] ウィンドウ**にアクセスします。アクセスする方法は 2 つあります。


- 新しい条件付きブレークポイントを追加するには、ブレークポイントを設定するコードの行番号の左にあるエディターの余白を右クリックし、[ブレークポイントの作成] を選択します。

    [![](debugging-in-xamarin-ios-images/image4.png "[ブレークポイントの作成] を選択する")](debugging-in-xamarin-ios-images/image4.png#lightbox)

- 既存のブレークポイントに条件を追加するには、ブレークポイントを右クリックし、**[ブレークポイントのプロパティ]** を選択します。あるいは**ブレークポイント パッド**で、次の画像にあるプロパティを選択します。

    [![](debugging-in-xamarin-ios-images/image5.png "[ブレークポイント] パッド")](debugging-in-xamarin-ios-images/image5.png#lightbox)


この画面で、ブレークポイントが発生する条件を入力できます。

[![](debugging-in-xamarin-ios-images/image6.png "ブレークポイントが発生する条件を入力する")](debugging-in-xamarin-ios-images/image6.png#lightbox)

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

Visual Studio で条件付きブレークポイントを設定するには、最初に[普通のブレークポイントを設定](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)します。 そのブレークポイントを右クリックし、コンテキスト メニューを表示します。

 [![](debugging-in-xamarin-ios-images/image4vs.png "ブレークポイント コンテキスト メニュー")](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

**[条件...]** を選択し、_[ブレークポイントの設定]_ メニューを表示します。

 [![](debugging-in-xamarin-ios-images/image6vs.png "[ブレークポイントの設定] メニュー")](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

ここで、ブレークポイントが発生する条件を入力できます。

以前のバージョンの Visual Studio でブレークポイント条件を利用する方法については、本トピックの [Visual Studio ドキュメント](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints)をご覧ください。

-----

## <a name="navigating-through-code"></a>コード間を移動する

デバッグ ツールでは、ブレークポイントに達すると、プログラムの実行を制御できます。 IDE には、コードの実行とステップ実行に使用できるボタンが 4 つ表示されます。

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

Visual Studio for Mac では次のように表示されます。

 [![](debugging-in-xamarin-ios-images/image7.png "開発者はデバッグ ツールを使用してプログラムの実行を制御できる")](debugging-in-xamarin-ios-images/image7.png#lightbox)

これらの数値は、次のとおりです。

- **再生/停止** - コードの実行が開始され、次のブレークポイントまで実行されます。あるいは、コードの実行を停止します。
- **ステップ オーバー** - 次のコード行が実行されます。 次行が関数呼び出しの場合、[ステップ オーバー] をクリックするとその関数が実行され、関数の_後_の次のコード行で停止します。
- **ステップ イン** - このボタンの場合も次のコード行が実行されます。 次行が関数呼び出しの場合、[ステップ イン] をクリックすると関数の最初の行で停止するので、関数のデバッグを 1 行ずつ続行できます。 次行が関数ではない場合の動作は、[ステップ オーバー] と同じです。
- **ステップ アウト** - 現在の関数が呼び出された行に戻ります。

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

Visual Studio では次のように表示されます。

[![](debugging-in-xamarin-ios-images/image7vs.png "開発者はデバッグ ツールを使用してプログラムの実行を制御できる")](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

これらの数値は、次のとおりです。

- **再生/停止** - コードの実行が開始され、次のブレークポイントまで実行されます。あるいは、コードの実行を停止します。
- **ステップ オーバー (F11)** - 次のコード行が実行されます。 次行が関数呼び出しの場合、[ステップ オーバー] をクリックするとその関数が実行され、関数の_後_の次のコード行で停止します。
- **ステップ イン (F10)** - このボタンの場合も次のコード行が実行されます。 次行が関数呼び出しの場合、[ステップ イン] をクリックすると関数の最初の行で停止するので、関数のデバッグを 1 行ずつ続行できます。 次行が関数ではない場合の動作は、[ステップ オーバー] と同じです。
- **ステップ アウト (Shift + F11)** - 現在の関数が呼び出された行に戻ります。

デバッグの詳しいドキュメントについては、「[Navigate Code with the Visual Studio Debugger](https://docs.microsoft.com/visualstudio/debugger/navigating-through-code-with-the-debugger)」(Visual Studio デバッガーでコード内を移動する) をご覧ください。

-----

### <a name="breakpoints"></a>ブレークポイント

これは重要なことですが、アプリケーションが起動し、アプリケーション デリゲートで `FinishedLaunching` メソッドを完了するための時間として iOS はアプリケーションに 10 秒だけ与えます。 アプリケーションが 10 秒以内にこのメソッドを完了しない場合、iOS はプロセスを終了します。

つまり、プログラムの起動コードにブレークポイントを設定することはほぼ不可能です。 起動コードをデバッグする場合、その初期化の一部を遅らせ、それをタイマーで呼び出されるメソッドか、FinishedLaunching の終了後に実行される他の形態のコールバック メソッドに入れてください。

## <a name="device-diagnostics"></a>デバイス診断

デバッガーの設定中にエラーが発生した場合、プロジェクト オプションの追加 mtouch 引数に "-v -v -v" を追加すれば、詳しく診断できます。 デバイスのコンソールに詳しいエラー情報が出力されます。

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>ワイヤレス デバッグ

Xamarin.iOS の初期設定では、USB で接続されているデバイスのアプリケーションをデバッグします。 ExternalAccessory で動くアプリケーションの開発で、ケーブルを差し込んだり、抜いたりしてテストするとき、USB デバイスが必要になることもあります。 そのような場合、無線ネットワークでデバッグできます。

ワイヤレス展開およびデバッグについて詳しくは、「[ワイヤレス展開](~/ios/deploy-test/wireless-deployment.md)」ガイドをご覧ください。

<a name="Technical_Details" />

## <a name="technical-details"></a>技術詳細

Xamarin.iOS は新しい Mono Soft Debugger を使用します。 オペレーティング システムのインターフェイスで個々のプロセスを制御するプログラムである標準の Mono Debugger とは異なり、このソフト デバッガーは Mono ランタイムにワイヤー プロトコル経由でデバッグ機能を公開させることで機能します。

起動時、デバッグ対象のアプリケーションはデバッガーに接触し、デバッガーは動作を開始します。 Xamarin.iOS for Visual Studio の場合、Xamarin Mac Agent は (Visual Studio の) アプリケーションとデバッガーの間の仲介として機能します。

このソフト デバッガーは、デバイスで実行時、連携するデバッグ スキームを必要とします。 つまり、デバッグのためにコードがインストルメント化され、すべてのシーケンス ポイントで余計なコードが含まれるため、デバッグ時のバイナリ ビルドが大きくなります。

<a name="Accessing_the_Console" />


## <a name="accessing-the-console"></a>コンソールにアクセスする

クラッシュ ログとコンソール クラスの出力は iPhone コンソールに送信されます。 このコンソールには Xcode でアクセスできます。"オーガナイザー" を利用し、オーガナイザーからデバイスを選択します。

あるいは、Xcode を起動しない場合、Apple の [iPhone Configuration Utility](http://www.apple.com/support/iphone/enterprise/) を利用してコンソールに直接アクセスできます。 これにはさらに良いことがあります。現場である問題をデバッグしているとき、Windows マシンからコンソール ログにアクセスできます。

Visual Studio をご利用の場合、出力ウィンドウにもログがありますが、Mac に切り替え、完全なログを参照してください。

-----

<a name="Debugging_Mono's_Class_Libraries" />


## <a name="debugging-monos-class-libraries"></a>Mono のクラス ライブラリのデバッグ

Xamarin.iOS には Mono のクラス ライブラリのソース コードが付属しているため、そのソース コードを利用してデバッガーからシングル ステップ実行し、内部でどのような処理が実行されているかを確認できます。

# [<a name="visual-studio-for-mac"></a>Visual Studio for Mac](#tab/macos)

この機能はデバッグ中に大量のメモリを使用するため、既定ではオフになっています。


この機能を有効にするには、下の画像のように、_[Visual Studio for Mac]、[環境設定]、[デバッガー]_ メニューで **[プロジェクト コードのみをデバッグします。フレームワーク コードにはステップ インしません。]** オプションの選択を解除します。

[![](debugging-in-xamarin-ios-images/debugging6.png "Mono のクラス ライブラリのデバッグ")](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

Visual Studio でクラス ライブラリをデバッグするには、_[デバッグ] の [オプション]_ メニューで **[マイ コードのみ]** を無効にする必要があります。 _[デバッグ]、[全般]_ ノードで、**[マイ コードのみを有効にする]** チェック ボックスをオフにします。

[![](debugging-in-xamarin-ios-images/debugging6vs.png "Mono のクラス ライブラリのデバッグ")](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

完了したら、アプリケーションを起動し、Mono のコア クラス ライブラリをシングル ステップ実行できます。


## <a name="related-links"></a>関連リンク

- [Xamarin を使ったデバッグ](/visualstudio/mac/debugging/)
- [データの視覚化](/visualstudio/mac/data-visualizations/)
- [ブレークポイントの設定](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)
- [コードのステップ実行](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)
- [ログ ウィンドウに情報を出力する](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)
