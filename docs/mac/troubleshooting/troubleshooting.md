---
title: Xamarin.Mac のトラブルシューティングのヒント
description: このドキュメントでは、Xamarin.Mac アプリケーションを開発するときに発生する問題を解決するための方法について説明します。 サポートを受ける方法も説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: f498aab5bfaffc08a22f62a318f8f9f73ab0afca
ms.sourcegitcommit: d294c967a18e6d91f3909c052eeff98ede1a21f6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/19/2018
ms.locfileid: "53609910"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Xamarin.Mac のトラブルシューティングのヒント

## <a name="overview"></a>概要

希望方法を操作する API を取得することができない、またはバグを回避しようとしています。 で、プロジェクトで作業中にスタックでしょう。 Xamarin の目標は、モバイルおよびデスクトップ アプリケーションの作成に成功することですし、リソースについてご紹介します。

これらのリソースは、すばやく問題を解決するためにかかる準備のいくつかの手順があります。

- レポートがクラッシュするベストできるだけの問題の根本原因を特定するには。
 
     - 「マイ アプリケーションがクラッシュする」診断は困難です。 「この呼び出しに空の配列を返すと、アプリケーションがクラッシュする」がずっと簡単に修正します。

     - 「マイ NSTableDelegate のメソッドのいずれもないように見えるここでは呼び出せません」よりもあまり役に立ちませんが「動作する NSTable は取得できません」

- 可能であれば、問題を引き起こしている小規模のサンプル プログラムを提供します。 ソース コードの問題お探しのページは掘り出すでは、多くの時間と労力桁違いがかかります。

- 何に表示される問題を生じさせるをアプリケーションに行った変更はすばやく絞り問題の原因を把握します。 最近、アプリケーションを問題の原因で一部のセクションをトリミングして、Xamarin.Mac のバージョンをアップグレードしたかどうか、またはどのような変更に導入された問題を検索するビルド前のテストを記録は、非常に役立ちます。


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>出力なしで、アプリがクラッシュした場合の対処方法

ほとんどの場合は、Visual Studio for Mac でデバッガーは例外をキャッチ、アプリケーションでのクラッシュと根本原因を追跡するため。 ただし場合によっては、アプリケーションがドック バウンドされ、終了し、ほとんどまたはまったくない出力があります。 これらを含めることができます。

- コード署名の問題。
- 特定の mono ランタイムがクラッシュします。
- Objective c の例外やクラッシュします。
- 一部には、非常に早い段階のプロセス有効期間がクラッシュします。
- いくつかはスタック オーバーフローです。
- 記載されている macOS バージョン、 **Info.plist**現在インストールされている macOS バージョンまたはそれが無効より新しいものです。

これらのプログラムをデバッグ、いらいらすること、検索としてに必要な情報を困難にすることができます。 役立つ可能性のあるいくつかの方法を次に示します。

- MacOS バージョンを記載することを確認、 **Info.plist** macOS コンピューターに現在インストールされているのバージョンと 1 つと同じです。
- Visual Studio を Mac アプリケーションの出力の確認 (**ビュー** -> **パッド** -> **アプリケーション出力**) のスタック トレースまたは赤からの出力出力の記述が Cocoa します。
- コマンドラインからアプリケーションを実行し、結果を調べます (で、**ターミナル**アプリ) を使用しています。 

     `MyApp.app/Contents/MacOS/MyApp` (場所`MyApp`アプリケーションの名前を指定)
- たとえば、コマンドラインでコマンドに"MONO_LOG_LEVEL"を追加することで、出力を増やすことができます。 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- ネイティブ デバッガーをアタッチする可能性があります (`lldb`)、プロセスに表示するかどうか (有料ライセンスが必要) その他の情報を提供します。 たとえば、次の操作を行います。

    1. 入力`lldb MyApp.app/Contents/MacOS/MyApp`ターミナル。
    2. 入力`run`ターミナル。
    3. 入力`c`ターミナル。
    4. デバッグが完了したらを終了します。
- 最後の手段としてを呼び出す前に`NSApplication.Init`で、`Main`メソッド (または必要に応じてその他の場所) での起動には、どのような手順を実行している問題があるを追跡する既知の場所にファイルにテキストを書き込む可能性があります。

## <a name="known-issues"></a>既知の問題

次のセクションでは、既知の問題とその解決方法について説明します。

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>サンド ボックス化されたアプリにデバッガーに接続できません。

デバッガーは、既定ではサンド ボックスは、有効にするとそれができないことをアプリに接続するには、TCP 経由の Xamarin.Mac アプリに接続するため、エラーが発生した場合は有効になっている適切なアクセス許可がないアプリを実行しようとすると、 *"に接続できませんデバッガー"* します。 

[![権利の編集](troubleshooting-images/debug01.png "権利の編集")](troubleshooting-images/debug01-large.png#lightbox)

**発信ネットワーク接続を許可する (クライアント)** 権限は、デバッガーに必要な 1 つは、この 1 つ許可すると、通常のデバッグします。 更新したなしでデバッグすることはできませんので、`CompileEntitlements`対象の`msbuild`デバッグのセキュリティで保護されているすべてのアプリのビルドのみの場合は、権利をそのアクセス許可を自動的に追加します。 リリース ビルドでは、未変更の状態、権利ファイルで指定された資格を使用する必要があります。

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: データは利用できません 437 のエンコード
 
Xamarin.Mac アプリでは、サード パーティ製ライブラリを含む、ときにフォームでエラーが発生する可能性があります"System.NotSupportedException:エンコード 437"をコンパイルして、アプリを実行しようとするときはデータは有効ではありません。 たとえば、ライブラリなど`Ionic.Zip.ZipFile`操作中にこの例外をスローする可能性があります。

これは、問題を Xamarin.Mac プロジェクトのオプションを開くことで解決できます**Mac ビルド** > **国際化**とチェック、**西部**国際化:

[![ビルド オプションの編集](troubleshooting-images/issue01.png "ビルド オプションの編集")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>(Mm5103) のコンパイルに失敗しました

Xcode の新しいバージョンがリリースと新しいバージョンをインストールしたときに、いいえまだ実行されて、このエラーは通常発生します。 Xcode の新しいバージョンを使用してコンパイルする前に、まずそのバージョンを少なくとも 1 回実行する必要があります。

初めて、Xcode の新しいバージョンを実行するには、Xamarin.Mac で必要ないくつかのコマンド ライン ツールがインストールされます。 さらに、Xcode または、Xamarin.Mac のバージョンを更新した後、クリーン ビルドを行う必要があります。

この問題を解決できない場合は、ください[バグ](#filing-a-bug)します。

### <a name="missing-entitlementsplist"></a>不足している entitlements.plist

Visual Studio for Mac の最新バージョンからの権利のセクションが削除、 **Info.plist**エディターに個別の配置と**Entitlements.plist** (クロス プラットフォーム サポートの向上に対するエディターで Xamarin.iOS)。

新しい Visual Studio for Mac をインストール、ときに作成する新しい Xamarin.Mac アプリ プロジェクトでは、 **Entitlements.plist**プロジェクト ツリーには、ファイルが自動的に追加されます。

![権利を選択](troubleshooting-images/entitlements01.png "権利を選択")

ダブルクリックする場合、 **Entitlements.plist**ファイル、権利エディターが表示されます。

[![権利の編集](troubleshooting-images/entitlements02.png "権利の編集")](troubleshooting-images/entitlements02-large.png#lightbox)

既存の Xamarin.Mac プロジェクトでは、手動で作成する必要があります、 **Entitlements.plist**ファイルでプロジェクトを右クリックして、 **Solution Pad**選択**追加**  > **新しいファイル.**.次に、選択**Xamarin.Mac** > **空のプロパティ リスト**:

![新しいプロパティ リストを追加する](troubleshooting-images/entitlements03.png "新しいプロパティ リストを追加します。")

入力`Entitlements` をクリックして、**新規**ボタンをクリックします。 プロジェクトには、権利ファイルが含まれていた場合、は、新しいファイルを作成する代わりに、プロジェクトに追加するよう求められます。

[![ファイルの上書きを確認する](troubleshooting-images/entitlements04.png "ファイルの上書きを確認しています")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="community-support-on-the-forums"></a>フォーラムでコミュニティ サポート

Xamarin 製品を使用している開発者のコミュニティは驚くべきことと、多くを参照してください、 [Xamarin.Mac フォーラム](http://forums.xamarin.com/categories/mac)エクスペリエンスや、専門知識を共有します。 さらに、Xamarin のエンジニアに役立つフォーラムに定期的にアクセスします。

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>バグをファイリング

お客様のフィードバックは弊社にとって重要です。 Xamarin.Mac を使って問題を検出する: 場合

- [問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues)を検索する 
- GitHub の問題に切り替わる前に、Xamarin の問題は [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) で追跡されていました。 そこで一致する問題を検索してください。
- 一致する問題が見つからない場合は、[GitHub の問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues/new)に新しい問題を提出してください。

GitHub の問題はすべて公開されています。 コメントまたは添付ファイルを非表示にすることはできません。 

次の情報について、できるだけ多くを含めてください。                                                                                                                                          

- 問題を再現する簡単な例。 これは**重要**です (可能な場合)。 
- クラッシュの完全なスタック トレース。
- クラッシュの周囲の C# コード。 
