---
title: Xamarin. Mac のトラブルシューティングのヒント
description: このドキュメントでは、Xamarin アプリケーションの開発時に発生した問題を解決するための方法について説明します。 また、サポートを受ける方法についても説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 8714297c4948dbb65c521d6a32bac3e437b40733
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725441"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Xamarin. Mac のトラブルシューティングのヒント

## <a name="overview"></a>の概要

場合によっては、API を手に入れたり、バグに対処したりすることができないときに、プロジェクトの作業中に行き詰まってしまうことがあります。 Xamarin での目標は、モバイルアプリケーションとデスクトップアプリケーションの作成に成功し、役に立ちます。

これらのリソースのいずれかについて、問題を迅速に解決するための準備手順がいくつかあります。

- クラッシュを報告するのに最適な問題の根本原因を特定します。

  - "アプリケーションのクラッシュ" は診断が困難です。 "この呼び出しに空の配列を返すと、アプリケーションがクラッシュします。" は、修正の作業がはるかに簡単になります。

  - "NSTable to work を使用できません" は、"私の NSTableDelegate にはどのメソッドも呼び出されませんでした。" とはあまり役に立ちません。

- 可能な場合は、問題を示す小さなサンプルプログラムを提供します。 問題を探すソースコードのページを掘り下げていくと、時間と労力が大幅に増加します。

- 問題の原因を特定するためにアプリケーションに対して行った変更を把握しておくと、問題の原因をすばやく絞り込むことができます。 最近バージョンの Xamarin. Mac をアップグレードした場合、問題の原因となっている部分を見つけるためにアプリケーションのセクションをトリミングする場合、または以前のビルドをテストして問題が発生した変更内容を見つけることが非常に便利な場合に注意してください。

### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>出力なしでアプリがクラッシュした場合の対処方法

ほとんどの場合、Visual Studio for Mac のデバッガーは例外をキャッチし、アプリケーションでクラッシュし、根本原因を追跡するのに役立ちます。 ただし、アプリケーションがドッキングにバウンスし、出力がほとんどまたはまったくない状態で終了する場合もあります。 次のようなものがあります。

- コード署名に問題があります。
- 特定の mono ランタイムがクラッシュした場合。
- いくつかの目的 c の例外とクラッシュ。
- 一部のクラッシュは、プロセスの有効期間のごく早い段階で発生します。
- スタックオーバーフローが発生しています。
- お使いの**情報 plist**に記載されている macos のバージョンが、現在インストールされている macos のバージョンよりも新しいものであるか、または無効です。

これらのプログラムのデバッグは困難になる可能性があります。必要な情報を見つけるのは難しい場合があるためです。 次のようないくつかの方法があります。

- **情報 plist**に記載されている macos のバージョンが、コンピューターに現在インストールされている macos のバージョンと同じであることを確認します。
- スタックトレースの場合は**Visual Studio for Mac アプリケーション**の ** ->  -> 出力**を確認し、出力を記述する場合は cocoa から赤で出力します。
- コマンドラインからアプリケーションを実行し、次のコマンドを使用して (**ターミナル**アプリ内の) 出力を確認します。

  `MyApp.app/Contents/MacOS/MyApp` (`MyApp` はアプリケーションの名前です)
- コマンドラインでコマンドに "MONO_LOG_LEVEL" を追加すると、出力を増やすことができます。次に例を示します。

  `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- ネイティブデバッガー (`lldb`) をプロセスにアタッチして、それ以上の情報が提供されているかどうかを確認できます (これには有料ライセンスが必要です)。 たとえば、次の手順を行います。

  1. ターミナルで `lldb MyApp.app/Contents/MacOS/MyApp` を入力します。
  2. ターミナルで `run` を入力します。
  3. ターミナルで `c` を入力します。
  4. デバッグが完了したら終了します。
- 最後の手段として、`Main` メソッド (または必要に応じてその他の場所) で `NSApplication.Init` を呼び出す前に、既知の場所のファイルにテキストを記述して、問題が発生している起動手順を追跡することができます。

## <a name="known-issues"></a>既知の問題

以下のセクションでは、既知の問題とその解決策について説明します。

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>サンドボックスアプリのデバッガーに接続できません

デバッガーは、TCP を通じて Xamarin. Mac アプリに接続します。つまり、サンドボックスを有効にすると、既定ではアプリに接続できないため、適切なアクセス許可を有効にせずにアプリを実行しようとすると、 *"デバッガーに接続できません"* というエラーが表示されます。

[![権利の編集](troubleshooting-images/debug01.png "権利の編集")](troubleshooting-images/debug01-large.png#lightbox)

**[発信ネットワーク接続 (クライアント) を許可する]** アクセス許可は、デバッガーに必要なアクセス許可です。これを有効にすると、デバッグが正常に実行できるようになります。 デバッグを行わずにデバッグすることはできないため、`msbuild` の `CompileEntitlements` ターゲットを更新して、デバッグビルド用にサンドボックスされているすべてのアプリの権利にそのアクセス許可を自動的に追加します。 リリースビルドでは、権利ファイルで指定されている権利を変更せずに使用する必要があります。

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>NotSupportedException: エンコード437で使用できるデータがありません

Xamarin. Mac アプリにサードパーティ製のライブラリを含めると、アプリをコンパイルして実行しようとすると、"NotSupportedException: encoding 437 のデータは使用できません" という形式のエラーが表示されることがあります。 たとえば、`Ionic.Zip.ZipFile`などのライブラリでは、操作中にこの例外がスローされる場合があります。

これを解決するには、Xamarin プロジェクトのオプションを開き、[ **Mac Build** > **国際化**] に移動して、[**西**国際化] をオンにします。

[![ビルド オプションの編集](troubleshooting-images/issue01.png "ビルド オプションの編集")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>コンパイルに失敗しました (mm5103)

このエラーは通常、新しいバージョンの Xcode がリリースされていて、新しいバージョンをインストールしたが、まだ実行していない場合に発生します。 新しいバージョンの Xcode を使用してコンパイルする前に、そのバージョンを少なくとも1回は実行する必要があります。

新しいバージョンの Xcode を初めて実行するときは、Xamarin. Mac で必要ないくつかのコマンドラインツールがインストールされます。 また、Xcode または Xamarin. Mac バージョンを更新した後で、クリーンビルドを実行する必要があります。

この問題を解決できない場合は、[バグを報告](#filing-a-bug)してください。

### <a name="missing-entitlementsplist"></a>権利がありません。 plist

最新バージョンの Visual Studio for Mac により、**情報**エディターから権利セクションが削除され、別の権利 ( **plist** ) エディターに配置されました (Xamarin でのクロスプラットフォームサポートを強化)。

新しい Visual Studio for Mac がインストールされた状態で、新しい Xamarin. Mac アプリプロジェクトを作成すると、次のような**権利の plist**ファイルがプロジェクトツリーに自動的に追加されます。

![権利の選択](troubleshooting-images/entitlements01.png "権利の選択")

**権利の plist**ファイルをダブルクリックすると、権利エディターが表示されます。

[![権利の編集](troubleshooting-images/entitlements02.png "権利の編集")](troubleshooting-images/entitlements02-large.png#lightbox)

既存の Xamarin. Mac プロジェクトの場合は、 **Solution Pad**でプロジェクトを右クリックし、[ > **新しいファイル**の**追加**] を選択して、**権利の plist**ファイルを手動で作成する必要があります。次に、[ **Xamarin. Mac** > **空のプロパティリスト**] を選択します。

![新しいプロパティリストの追加](troubleshooting-images/entitlements03.png "新しいプロパティリストの追加")

名前として「`Entitlements`」と入力し、 **[新規]** ボタンをクリックします。 プロジェクトに既に権利ファイルが含まれている場合は、新しいファイルを作成するのではなく、プロジェクトに追加するように求められます。

[![ファイルの上書きの確認](troubleshooting-images/entitlements04.png "ファイルの上書きの確認")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="community-support-on-the-forums"></a>フォーラムのコミュニティサポート

Xamarin 製品を使用する開発者コミュニティはすばらしいものであり、多くの場合、経験と専門知識を共有するための[xamarin. Mac フォーラム](https://forums.xamarin.com/categories/xamarin-mac)にアクセスします。 さらに、Xamarin エンジニアは、フォーラムに定期的にアクセスしてヘルプを提供します。

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>バグを提出する

皆様からのご意見をお待ちしております。 Xamarin. Mac で問題が見つかった場合は、次のようにします。

- [問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues)を検索する
- GitHub の問題に切り替わる前に、Xamarin の問題は [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) で追跡されていました。 そこで一致する問題を検索してください。
- 一致する問題が見つからない場合は、[GitHub の問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues/new)に新しい問題を提出してください。

GitHub の問題はすべて公開されています。 コメントまたは添付ファイルを非表示にすることはできません。

次の情報について、できるだけ多くを含めてください。

- 問題を再現する簡単な例。 これは**重要**です (可能な場合)。
- クラッシュの完全なスタック トレース。
- クラッシュの周囲の C# コード。
