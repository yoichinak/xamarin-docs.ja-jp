---
title: Xamarin.Mac トラブルシューティングのヒント
description: このドキュメントでは、Xamarin.Mac アプリケーションを開発するときに発生した問題を解決するための方法について説明します。 サポートを受ける方法についても説明します。
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5CBC6822-BCD7-4DAD-8468-6511250D41C4
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 5e6cd5b338034cfa9956244015d4585b4f005793
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792968"
---
# <a name="xamarinmac-troubleshooting-tips"></a>Xamarin.Mac トラブルシューティングのヒント

## <a name="overview"></a>概要

場合があります、希望方法を使用する API を取得することができない、またはバグを回避しようとしています。 で、プロジェクトで作業中にスタックしている全員を取得します。 Xamarin の目標は、ユーザーが、モバイル、デスクトップ アプリケーションの作成に成功して、支援何らかのリソースが提供されています。

これらのリソースのいずれかとは、準備する、問題を迅速に解決するために役立つためのいくつかの手順があります。

- レポートがクラッシュするベスト考えられる問題の根本原因を判断するには。
 
     - 「マイ アプリケーションがクラッシュする」診断は困難です。 「マイ アプリケーションには、この呼び出しに空の配列を返すとがクラッシュする」の解決に使用する方が簡単です。

     - 「My NSTableDelegate のメソッドのようにここでは呼び出せません」よりも小さいと便利では「NSTable 動作を取得できません」

- 可能であれば、問題を引き起こしている小さなサンプル プログラムを提供します。 物置ソース コードの問題を探してのページからは、多くの時間と労力桁違いを受け取ります。

- 新機能に表示される問題が発生するアプリケーションに加えた変更できますすばやく絞り込むため、問題の原因を把握します。 最近 Xamarin.Mac、セクションでは、問題を引き起こしているパーツを検索するアプリケーションのトリミングのバージョンをアップグレードしたかどうか、またはどのような変更に導入された問題を検索するビルド前のテストを記録すると非常に役立ちます。


### <a name="what-to-do-when-your-app-crashes-with-no-output"></a>出力なしでアプリがクラッシュした場合の対処方法

ほとんどの場合、Mac 用の Visual Studio のデバッガーの例外をキャッチするアプリケーションのクラッシュおよびヘルプ根本原因を追跡します。 ただし場合によっては、アプリケーションは、ドッキング ステーションでバウンドし、終了位置でほとんどまたはまったくない出力があります。 これらを含めることができます。

- コードの問題を署名します。
- 特定の mono ランタイムがクラッシュします。
- Objective c の例外やクラッシュします。
- 一部には、初期段階のプロセスの有効期間がクラッシュします。
- いくつかはスタック オーバーフローです。
- 記載 macOS バージョン、 **Info.plist**は新しいものよりも、現在インストールされている macOS バージョンまたはそれが無効です。

これらのプログラムのデバッグと手間が、検索としては、必要な情報を困難に指定できます。 ここでは、いくつかの方法が役立つ場合があります。

- 記載 macOS バージョンを確認してください、 **Info.plist** macOS コンピューターに現在インストールされているのバージョンと同じです。
- Visual Studio を Mac アプリケーションの出力の確認 (**ビュー** -> **パッド** -> **アプリケーション出力**) のスタック トレースをまたはから赤で出力出力を記述できます Cocoa です。
- コマンドラインからアプリケーションを実行し、結果を調べます (で、**ターミナル**アプリ) を使用しています。 

     `MyApp.app/Contents/MacOS/MyApp` (ここで`MyApp`アプリケーションの名前を指定)
- たとえば、コマンドラインでコマンドに"MONO_LOG_LEVEL"を追加することで、出力を増やすことができます。 

     `MONO_LOG_LEVEL=debug MyApp.app/Contents/MacOS/MyApp`
- ネイティブ デバッガーをアタッチする可能性があります (`lldb`)、プロセスに表示するかどうか (有料のライセンスが必要) その他の情報を提供します。 たとえば、次の操作を行います。

    1. 入力`lldb MyApp.app/Contents/MacOS/MyApp`端末にします。
    2. 入力`run`端末にします。
    3. 入力`c`端末にします。
    4. デバッグが完了したらを終了します。
- 最後の手段としてを呼び出す前に`NSApplication.Init`で、`Main`メソッド (または必要に応じてその他の場所) で起動のどのような手順を実行している問題を追跡する既知の場所にファイルにテキストを記述することもできます。

## <a name="known-issues"></a>既知の問題

次のセクションでは、既知の問題とその解決方法について説明します。

### <a name="unable-to-connect-to-the-debugger-in-sandboxed-apps"></a>サンド ボックス化されたアプリにデバッガーに接続できません。

既定では、サンド ボックス化を有効にすることができないことアプリへの接続には、TCP を介して Xamarin.Mac アプリにデバッガーが接続するため、エラーが発生した場合は有効になっている適切な権限がない場合、アプリを実行しようとすると、 *"に接続できません。デバッガー"* です。 

[![権利を編集](troubleshooting-images/debug01.png "と権利の編集")](troubleshooting-images/debug01-large.png#lightbox)

**送信ネットワーク接続を許可する (クライアント)** 権限は、デバッガーのために必要な 1 つは、この許可すると、通常どおりにデバッグします。 更新したなしでデバッグすることはできませんので、`CompileEntitlements`対象の`msbuild`ビルドでのみデバッグのセキュリティで保護されているすべてのアプリの権利にそのアクセス許可を自動的に追加します。 リリース ビルドでは、未変更の状態、権利ファイルで指定された権限を使用してください。

### <a name="systemnotsupportedexception-no-data-is-available-for-encoding-437"></a>System.NotSupportedException: データがない 437 をエンコードするため
 
Xamarin.Mac アプリでは、サード パーティ製ライブラリを含む、ときに、フォームでエラーが発生した可能性があります"System.NotSupportedException: 437 をエンコードするために使用可能なデータがない"、アプリをコンパイルして実行しようとしているときにします。 たとえば、ライブラリなど`Ionic.Zip.ZipFile`操作中にこの例外をスローする可能性があります。

これは、問題を Xamarin.Mac プロジェクトのオプションを開くことによって解決できます**Mac をビルド** > **国際化**をチェックし、**西部**国際化:

[![ビルド オプションの編集](troubleshooting-images/issue01.png "ビルド オプションの編集")](troubleshooting-images/issue01-large.png#lightbox)

### <a name="failed-to-compile-mm5103"></a>(Mm5103) のコンパイルに失敗しました

このエラーは通常、まだ実行されてではありません Xcode の新しいバージョンがリリースし、新しいバージョンをインストールしているときに発生します。 Xcode の新しいバージョンでコンパイルする前に、まずそのバージョンを少なくとも 1 回実行する必要があります。

初めて Xcode の新しいバージョンを実行する Xamarin.Mac で必要ないくつかのコマンド ライン ツールをインストールします。 さらに、Xcode または Xamarin.Mac バージョンを更新した後、クリーン ビルドを行う必要があります。

この問題を解決できない場合は場合、次のようにしてください。[バグ](#filing-a-bug)です。

### <a name="missing-entitlementsplist"></a>不足している entitlements.plist

Visual Studio for Mac の最新バージョンから権利セクションが削除、 **Info.plist**エディターされ、個別に配置**Entitlements.plist** (優れたクロスプラット フォーム サポート用のエディターで Xamarin.iOS)。

新しい Visual Studio for Mac にインストールされている、ときにプロジェクトを作成する新しい Xamarin.Mac アプリ、 **Entitlements.plist**プロジェクト ツリーには、ファイルが自動的に追加されます。

![権利を選択すると](troubleshooting-images/entitlements01.png "権利を選択します。")

ダブルクリックする場合、 **Entitlements.plist**ファイル、権利エディターが表示されます。

[![権利を編集](troubleshooting-images/entitlements02.png "と権利の編集")](troubleshooting-images/entitlements02-large.png#lightbox)

既存の Xamarin.Mac プロジェクトに、手動で作成する必要があります、 **Entitlements.plist**ファイルでプロジェクトを右クリックして、**ソリューション パッド**を選択して**追加**  > **新しいファイル.**.次に、選択**Xamarin.Mac** > **空のプロパティ リスト**:

![新しいプロパティ リストを追加する](troubleshooting-images/entitlements03.png "新しいプロパティ リストを追加します。")

入力`Entitlements`クリックと名前の**新規**ボタンをクリックします。 プロジェクトには、権利ファイルが含まれていた場合、は、新しいファイルを作成する代わりにプロジェクトに追加するよう求められます。

[![ファイルの上書きを確認する](troubleshooting-images/entitlements04.png "ファイルの上書きを確認しています")](troubleshooting-images/entitlements04-large.png#lightbox)

## <a name="contacting-support-business-or-enterprise-licenses"></a>(Business または enterprise のライセンス) のサポートに連絡

Business または enterprise のライセンスがある場合は、サポート チケットを使用して Xamarin エンジニアから直接支援を依頼する権限があります。 参照してください[xamarin.com/support](http://xamarin.com/support)詳細についてはします。

## <a name="community-support-on-the-forums"></a>フォーラムでコミュニティ サポート

Xamarin の製品を使用する開発者コミュニティとは、すばらしいとの多くを参照してください、 [Xamarin.Mac フォーラム](http://forums.xamarin.com/categories/mac)エクスペリエンスと専門知識を共有します。 さらに、Xamarin エンジニアには、フォーラムのヘルプを定期的にアクセスしてください。

<a name="filing-a-bug"/>

## <a name="filing-a-bug"></a>バグをファイリング

お客様のフィードバックは弊社にとって重要です。 : Xamarin.Mac で問題が見つかった場合

- [問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues)を検索する 
- GitHub の問題に切り替わる前に、Xamarin の問題は [Bugzilla](https://bugzilla.xamarin.com/describecomponents.cgi) で追跡されていました。 そこで一致する問題を検索してください。
- 一致する問題が見つからない場合は、[GitHub の問題リポジトリ](https://github.com/xamarin/xamarin-macios/issues/new)に新しい問題を提出してください。

GitHub の問題はすべて公開されています。 コメントまたは添付ファイルを非表示にすることはできません。 

次の情報について、できるだけ多くを含めてください。                                                                                                                                          

- 問題を再現する簡単な例。 これは**重要**です (可能な場合)。 
- クラッシュの完全なスタック トレース。
- クラッシュの周囲の C# コード。 
