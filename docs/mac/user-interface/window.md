---
title: Xamarin. Mac のウィンドウ
description: この記事では、Xamarin. Mac アプリケーションでのウィンドウとパネルの使用について説明します。 Xcode と Interface Builder でのウィンドウとパネルの作成、ストーリーボードと xib ファイルからの読み込み、プログラムによる作業について説明します。
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 65ebefef0f03e2b4abd8c36fc1e0a68812e48218
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939517"
---
# <a name="windows-in-xamarinmac"></a>Xamarin. Mac のウィンドウ

_この記事では、Xamarin. Mac アプリケーションでのウィンドウとパネルの使用について説明します。Xcode と Interface Builder でのウィンドウとパネルの作成、ストーリーボードと xib ファイルからの読み込み、プログラムによる作業について説明します。_

Xamarin. Mac アプリケーションで C# と .NET を使用している場合、開発者が*目的の C*および*Xcode*で作業しているのと同じウィンドウとパネルにアクセスできます。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用して、ウィンドウとパネルを作成および管理できます (または、必要に応じて C# コードで直接作成することもできます)。

Xamarin アプリケーションでは、その目的に基づいて、画面上に1つまたは複数のウィンドウを表示して、表示および操作する情報を管理および調整できます。 ウィンドウの主な機能は次のとおりです。

1. ビューとコントロールを配置および管理できる領域を提供する。
2. キーボードとマウスの両方を使用したユーザーの操作に応じて、イベントを受け入れて応答する。

ウィンドウは、モードレス状態 (複数のドキュメントを一度に開くことができるテキストエディターなど) またはモーダル (アプリケーションを続行する前に閉じる必要があるエクスポートダイアログなど) で使用できます。

パネルとは、特殊な種類のウィンドウ (基本クラスのサブクラス `NSWindow` ) です。通常は、テキスト形式のインスペクターやシステムカラーピッカーなどのユーティリティウィンドウなど、アプリケーションで補助関数を提供します。

[![Xcode でのウィンドウの編集](window-images/intro01.png)](window-images/intro01.png#lightbox)

この記事では、Xamarin. Mac アプリケーションで Windows とパネルを操作するための基本について説明します。 この記事で使用する主要な概念と手法について説明しているように、最初に[Hello, Mac](~/mac/get-started/hello-mac.md)の記事「 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)」と「[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)」セクションをご覧になることを強くお勧めします。

「 [C# のクラス/メソッドを](~/mac/internals/how-it-works.md) [Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの前に公開する」セクションを参照して `Register` `Export` ください。 c# クラスを目的の c オブジェクトと UI 要素に接続するために使用されるコマンドとコマンドについても説明します。

## <a name="introduction-to-windows"></a>Windows の概要

前述のように、ウィンドウには、ビューとコントロールを配置および管理し、ユーザーの操作 (キーボードまたはマウスを使用) に基づいてイベントに応答する領域が用意されています。

Apple によれば、macOS アプリには次の5つの主要な種類のウィンドウがあります。

- **ドキュメントウィンドウ**-ドキュメントウィンドウには、スプレッドシートやテキストドキュメントなどのファイルベースのユーザーデータが含まれています。
- **アプリウィンドウ**-アプリウィンドウは、ドキュメントベースではないアプリケーションのメインウィンドウです (Mac 上の予定表アプリなど)。
- **パネル**-他のウィンドウの上にフローティングし、ドキュメントが開いている間にユーザーが操作できるツールまたはコントロールが用意されています。 場合によっては、パネルを半透明にすることができます (大規模なグラフィックスを操作する場合など)。
- **ダイアログ**-ユーザーの操作に応答してダイアログが表示され、通常、ユーザーがアクションを完了する方法が示されます。 ダイアログを閉じるには、ユーザーからの応答が必要です。 (「[ダイアログの操作」を](~/mac/user-interface/dialog.md)参照)
- **アラート**-アラートは、重大な問題 (エラーなど) が発生したとき、または警告 (ファイルの削除の準備など) が発生したときに表示される特別な種類のダイアログです。 アラートはダイアログであるため、閉じる前にユーザーの応答も必要です。 (「[アラートの操作」を](~/mac/user-interface/alert.md)参照)

詳細については、Apple の[macOS デザインテーマ](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)の「 [About Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) 」セクションを参照してください。

### <a name="main-key-and-inactive-windows"></a>メイン、キー、および非アクティブなウィンドウ

Xamarin. Mac アプリケーションのウィンドウは、ユーザーが現在どのように対話しているかに基づいて、外観と動作が異なります。 現在ユーザーの注目に重点を置いている最も重要なドキュメントまたはアプリウィンドウは、_メインウィンドウ_と呼ばれます。 ほとんどの場合、このウィンドウは_キーウィンドウ_(現在ユーザー入力を受け入れているウィンドウ) にもなります。 ただし、これは常にではありません。たとえば、カラーピッカーが開いている場合や、ユーザーが対話して、ドキュメントウィンドウ内の項目の状態を変更するためにユーザーが操作しているキーウィンドウである場合があります (これはメインウィンドウでもあります)。

メインウィンドウとキーウィンドウ (これらが分離されている場合) は常にアクティブであり、_非アクティブウィンドウ_は開いているウィンドウであり、フォアグラウンドにはありません。 たとえば、テキストエディターアプリケーションでは、一度に複数のドキュメントを開くことができ、メインウィンドウだけがアクティブになり、その他はすべて非アクティブになります。 

詳細については、Apple の[macOS デザインテーマ](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)の「 [About Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) 」セクションを参照してください。

### <a name="naming-windows"></a>Windows の名前付け

ウィンドウにはタイトルバーを表示できます。タイトルが表示されるときは、通常、アプリケーションの名前、作業中のドキュメントの名前、またはウィンドウの機能 (インスペクターなど) が表示されます。 アプリケーションによっては、タイトルバーが表示されない場合があります。これは、表示によって認識され、ドキュメントを使用しないためです。

Apple では、次のガイドラインを提案します。

- ドキュメント以外のメインウィンドウのタイトルには、アプリケーション名を使用します。 
- 新しいドキュメントウィンドウに名前を指定 `untitled` します。 最初の新しいドキュメントでは、タイトルに数字 (など) を追加しないで `untitled 1` ください。 保存する前にユーザーが別の新しいドキュメントを作成し、最初の文書にタイトルを追加した場合は、そのウィンドウ、などを呼び出し `untitled 2` `untitled 3` ます。

詳細については、Apple の[macOS デザインテーマ](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)に関する[Windows の名前付け](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1)に関するセクションを参照してください。

### <a name="full-screen-windows"></a>全画面表示ウィンドウ

MacOS では、アプリケーションのウィンドウが全画面表示され、アプリケーションのメニューバー (画面の上部にカーソルを移動すると表示されることがあります) をすべて非表示にすることができます。

Apple では、次のガイドラインが提案されています。

- ウィンドウが全画面表示に適しているかどうかを判断します。 簡単な操作 (電卓など) を提供するアプリケーションは、全画面表示モードを提供してはいけません。
- 全画面表示のタスクで必要な場合は、ツールバーを表示します。 通常、全画面表示モードでは、ツールバーは非表示になります。
- 全画面表示ウィンドウには、ユーザーがタスクを完了するために必要なすべての機能が含まれている必要があります。
- 可能であれば、ユーザーが全画面表示のウィンドウにある間は Finder の対話を避けてください。
- メインタスクからフォーカスを移動せずに、増加した画面領域を活用します。

詳細については、Apple の[macOS デザインテーマ](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)の[全画面ウィンドウ](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1)のセクションを参照してください。

### <a name="panels"></a>パネル

パネルは、アクティブなドキュメントまたは選択 (システムカラーピッカーなど) に影響を与えるコントロールとオプションを含む補助ウィンドウです。

[![カラーパネル](window-images/panel01.png)](window-images/panel01.png#lightbox)

パネルには、_アプリ固有_またはシステム_全体_を使用できます。 アプリ固有のパネルは、アプリケーションのドキュメントウィンドウの上部に表示され、アプリケーションがバックグラウンドで表示されると消えます。 システム全体のパネル ([**フォント**] パネルなど) では、アプリケーションに関係なく、すべての開いているウィンドウの上にフローティングします。 

Apple では、次のガイドラインが提案されています。

- 一般に、標準パネルを使用すると、透明なパネルは控えめに使用する必要があるだけで、グラフィックを多用するタスクにも使用できます。
- ユーザーが自分のタスクに直接影響を与える重要なコントロールや情報に簡単にアクセスできるようにするには、パネルを使用することを検討してください。
- 必要に応じて、パネルを非表示にして表示します。
- パネルには常にタイトルバーを含める必要があります。
- パネルには、アクティブな [最小化] ボタンを含めないでください。

#### <a name="inspectors"></a>インスペクター

最新の macOS**アプリケーションでは**、パネルウィンドウを使用するのではなく、メインウィンドウの一部である_インスペクター_として、アクティブなドキュメントまたは選択項目に影響を与える補助コントロールとオプションが用意されています。

[![インスペクターの例](window-images/panel02.png)](window-images/panel02.png#lightbox)

詳細については、「Apple の[macOS デザインテーマ](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)」と「 [macinspector](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector)サンプルアプリ」の「[パネル](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1)」セクションを参照して、Xamarin. Mac アプリで**インスペクターインターフェイス**を完全に実装してください。

## <a name="creating-and-maintaining-windows-in-xcode"></a>Xcode でのウィンドウの作成と保守

新しい Xamarin. Mac Cocoa アプリケーションを作成すると、既定で標準の空白のウィンドウが表示されます。 このウィンドウは `.storyboard` 、プロジェクトに自動的に含まれるファイルに定義されています。 Windows のデザインを編集するには、**ソリューションエクスプローラー**で、ファイルをダブルクリックし `Main.storyboard` ます。

[![メインストーリーボードの選択](window-images/edit01.png)](window-images/edit01.png#lightbox)

これにより、Xcode の Interface Builder でウィンドウのデザインが開きます。

[![Xcode で UI を編集する](window-images/edit02.png)](window-images/edit02.png#lightbox)

**属性インスペクター**には、ウィンドウの定義と制御に使用できるプロパティがいくつかあります。

- **Title** -これは、ウィンドウのタイトルバーに表示されるテキストです。
- [**自動保存**]-位置が指定され、設定が自動的に保存されるときにウィンドウを ID するために使用される_キー_です。
- **タイトルバー** -ウィンドウにタイトルバーが表示されます。
- 統合された**タイトルとツールバー** -ウィンドウにツールバーが含まれている場合は、タイトルバーの一部である必要があります。
- **全サイズのコンテンツビュー** -ウィンドウのコンテンツ領域をタイトルバーの下に配置できるようにします。
- **Shadow** -ウィンドウに影が付きます。
- **テクスチャ**が適用されたウィンドウは、vibrancy のような効果を使用して、本体の任意の場所にドラッグすることによって移動できます。
- [**閉じる**]: ウィンドウに [閉じる] ボタンがあります。
- **最小化**-ウィンドウに最小化ボタンが表示されます。
- [**サイズ変更**]-ウィンドウにサイズ変更コントロールがあります。
- **ツールバーボタン**-ウィンドウにツールバーボタンが表示されます。
- **復元**可能-ウィンドウの位置と設定を自動的に保存および復元します。
- **起動時に表示**-ファイルが読み込まれるときに、ウィンドウが自動的に表示され `.xib` ます。
- 非**アクティブ化時に非表示**にする-アプリケーションが背景に入ったときにウィンドウが非表示になります。
- [**終了時にリリース**]-ウィンドウが閉じられたときにメモリから削除されます。
- **常にツールヒントを表示**する-ツールヒントが常に表示されます。
- 再**計算ビューループ**-ウィンドウが描画される前に、ビューの順序が再計算されます。
- **スペース**、 **exposé** 、および**循環**-すべての macOS 環境でのウィンドウの動作を定義します。 
- **全画面**-このウィンドウが全画面表示モードに入ることができるかどうかを決定します。 
- **アニメーション**-ウィンドウで使用できるアニメーションの種類を制御します。
- [**外観**]-ウィンドウの外観を制御します。 ここでは、水色だけが表示されます。

詳細については、Apple の Windows と[Nswindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow)の[概要に](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)関するドキュメントを参照してください。

### <a name="setting-the-default-size-and-location"></a>既定のサイズと場所の設定

ウィンドウの初期位置を設定し、サイズを制御するには、**サイズインスペクター**に切り替えます。

[![既定のサイズと場所](window-images/edit07.png)](window-images/edit07.png#lightbox)

ここでは、ウィンドウの初期サイズを設定し、それに最小値と最大サイズを設定し、画面の最初の位置を設定して、ウィンドウの周囲の境界線を制御できます。

### <a name="setting-a-custom-main-window-controller"></a>カスタムメインウィンドウコントローラーの設定

UI 要素を C# コードに公開するためのアウトレットとアクションを作成できるようにするには、Xamarin. Mac アプリでカスタムウィンドウコントローラーを使用する必要があります。

次の手順を実行します。

1. Xcode の Interface Builder でアプリのストーリーボードを開きます。
2. デザインサーフェイスでを選択し `NSWindowController` ます。
3. [ **Identity Inspector** ] ビューに切り替え、 `WindowController` **クラス名**として「」と入力します。 

    [![クラス名の設定](window-images/windowcontroller01.png)](window-images/windowcontroller01.png#lightbox)
4. 変更を保存し、Visual Studio for Mac に戻って同期します。
5. ファイルは、 `WindowController.cs` Visual Studio for Mac の**ソリューションエクスプローラー**にプロジェクトに追加されます。 

    [![Windows コントローラーの選択](window-images/windowcontroller02.png)](window-images/windowcontroller02.png#lightbox)
6. Xcode の Interface Builder でストーリーボードを再度開きます。
7. この `WindowController.h` ファイルは、次のように使用できます。 

    [![WindowController .h ファイルを編集しています](window-images/windowcontroller03.png)](window-images/windowcontroller03.png#lightbox)

### <a name="adding-ui-elements"></a>UI 要素の追加

ウィンドウのコンテンツを定義するには、**ライブラリインスペクター**から**インターフェイスエディター**にコントロールをドラッグします。 Interface Builder を使用してコントロールを作成および有効にする方法の詳細については、 [Xcode の概要と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)のドキュメントを参照してください。

例として、**ライブラリインスペクター**から**インターフェイスエディター**のウィンドウにツールバーをドラッグしてみましょう。

[![ライブラリからのツールバーの選択](window-images/edit03.png)](window-images/edit03.png#lightbox)

次に、**テキストビュー**をドラッグして、ツールバーの下の領域に合わせてサイズを変更します。

[![テキストビューの追加](window-images/edit04.png)](window-images/edit04.png#lightbox)

ウィンドウのサイズが変更されたときに**テキストビュー**を縮小して拡大する必要があるため、**制約エディター**に切り替えて、次の制約を追加してみましょう。

[![制約の編集](window-images/edit05.png)](window-images/edit05.png#lightbox)

エディターの上部にある4つの**赤の I ビーム**をクリックし、[4 つの**制約の追加**] をクリックすると、指定された X、Y 座標に収まるようにテキストビューが表示され、ウィンドウのサイズが変更されると、水平方向および垂直方向に拡大または縮小されます。

最後に、**アウトレット**を使用して**テキストビュー**をコードに公開します (ファイルを確実に選択し `ViewController.h` ます)。

[![アウトレットの構成](window-images/edit06.png)](window-images/edit06.png#lightbox)

変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。

**アウトレット****とアクション**の操作の詳細については、[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)のドキュメントを参照してください。

### <a name="standard-window-workflow"></a>標準ウィンドウワークフロー

Xamarin. Mac アプリケーションで作成して使用するすべてのウィンドウについて、このプロセスは基本的に上記の手順と同じです。

1. プロジェクトに自動的に追加されない新しいウィンドウの場合は、新しいウィンドウ定義をプロジェクトに追加します。 詳細については、以下で詳しく説明します。
1. ファイルをダブルクリックして `Main.storyboard` 、Xcode の Interface Builder で編集するためのウィンドウデザインを開きます。
1. 新しいウィンドウをユーザーインターフェイスのデザインにドラッグし、_セグエ_を使用してウィンドウをメインウィンドウにフックします (詳細については、[ストーリーボードの操作](~/mac/platform/storyboards/indepth.md)に関するドキュメントの[セグエ](~/mac/platform/storyboards/indepth.md#Segues)に関するセクションを参照してください)。
1. **属性インスペクター**と**サイズインスペクター**で必要なウィンドウプロパティを設定します。
1. インターフェイスを構築するために必要なコントロールをドラッグして、**属性インスペクター**で構成します。
1. **サイズインスペクター**を使用して、UI 要素のサイズ変更を処理します。
1. ウィンドウの UI 要素を、**コンセント**と**アクション**を使用して C# コードに公開します。
1. 変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。

基本的なウィンドウが作成されたので、windows を操作するときに、Xamarin アプリケーションが実行する一般的なプロセスを見ていきます。 

## <a name="displaying-the-default-window"></a>既定のウィンドウを表示する

既定では、新しい Xamarin. Mac アプリケーションは、 `MainWindow.xib` 開始時にファイルに定義されているウィンドウを自動的に表示します。

[![実行中のウィンドウの例](window-images/display01.png)](window-images/display01.png#lightbox)

上のウィンドウのデザインを変更したため、既定のツールバーと**テキストビュー**コントロールが追加されました。 ファイルの次のセクションで `Info.plist` は、このウィンドウを表示します。

[![情報を編集しています。 plist](window-images/display00.png)](window-images/display00.png#lightbox)

メインの [**インターフェイス**] ドロップダウンを使用して、メインアプリの UI (この場合は) として使用するストーリーボードを選択し `Main.storyboard` ます。

ビューコントローラーがプロジェクトに自動的に追加され、表示されるメインウィンドウ (およびそのプライマリビュー) が制御されます。 これは、ファイルで定義され、 `ViewController.cs` **id インスペクター**の Interface Builder の**ファイルの所有者**にアタッチされます。

[![ファイルの所有者を設定しています](window-images/display02.png)](window-images/display02.png#lightbox)

ウィンドウでは、最初にを開いたときにのタイトルが表示されるようにするため `untitled` 、のメソッドをオーバーライドして `ViewWillAppear` `ViewController.cs` 次のようにします。

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
```    

> [!NOTE]
> ウィンドウの `Title` プロパティは、メソッドの代わりにメソッドで設定され `ViewWillAppear` `ViewDidLoad` ます。これは、ビューがメモリに読み込まれても、まだ完全にインスタンス化されていないためです。 メソッド内のプロパティにアクセスする `Title` `ViewDidLoad` `null` と、ウィンドウがまだ構築されていないため、例外が発生します。

## <a name="programmatically-closing-a-window"></a>プログラムによるウィンドウの終了

ユーザーがウィンドウの [**閉じる**] ボタンをクリックしたり、メニュー項目を使用したりするのではなく、プログラムによって Xamarin. Mac アプリケーションでウィンドウを閉じることが必要になる場合があります。 macOS では、との2つの異なる方法で、プログラムによってを閉じることが `NSWindow` `PerformClose` できます。 `Close`

### <a name="performclose"></a>パフォーマンスを閉じる

`PerformClose`のメソッドを呼び出す `NSWindow` と、ユーザーがウィンドウの [**閉じる**] ボタンをクリックすると、そのボタンが一時的に強調表示され、ウィンドウが閉じます。

アプリケーションがのイベントを実装する場合は、 `NSWindow` `WillClose` ウィンドウが閉じられる前に発生します。 イベントからが返された場合、ウィンドウは閉じられ `false` ません。 ウィンドウに [**閉じる**] ボタンがない場合、または何らかの理由で閉じることができない場合は、OS によって警告音が出力されます。

次に例を示します。

```csharp
MyWindow.PerformClose(this);
```

はインスタンスを終了しようとし `MyWindow` `NSWindow` ます。 成功した場合、ウィンドウは閉じられます。それ以外の場合は、警告音が出力され、が開いたままになります。

### <a name="close"></a>閉じる

のメソッドを呼び出すと、 `Close` `NSWindow` ウィンドウを一時的に強調表示してウィンドウの [**閉じる**] ボタンをクリックしても、ウィンドウが閉じられるだけではありません。

ウィンドウは閉じられている必要はなく、 `NSWindowWillCloseNotification` 閉じられているウィンドウの既定の通知センターに通知が投稿されます。

メソッドは、 `Close` メソッドの2つの重要な方法で異なり `PerformClose` ます。

1. イベントの発生を試みることはありません `WillClose` 。
2. ユーザーが [**閉じる**] ボタンをクリックしても、ボタンを一時的に強調表示することはできません。

次に例を示します。

```csharp
MyWindow.Close();
```

インスタンスを閉じ `MyWindow` `NSWindow` ます。

## <a name="modified-windows-content"></a>変更された windows コンテンツ

MacOS では、ユーザーにウィンドウ () の内容が変更されていること、および保存する必要があることをユーザーに通知する方法が Apple から提供されてい `NSWindow` ます。 ウィンドウに変更されたコンテンツが含まれている場合は、小さい黒い点が**閉じ**たウィジェットに表示されます。

[![変更されたマーカーを含むウィンドウ](window-images/close01.png)](window-images/close01.png#lightbox)

ウィンドウのコンテンツに未保存の変更があるときに、ユーザーがウィンドウを閉じたり、Mac アプリを終了しようとしたりした場合は、[ダイアログボックス](~/mac/user-interface/dialog.md)または[モーダルシート](~/mac/user-interface/dialog.md)を表示し、ユーザーが最初に変更を保存できるようにする必要があります。

[![ウィンドウを閉じたときに表示される保存シート](window-images/close02.png)](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>ウィンドウを変更済みとしてマークする

ウィンドウを変更されたコンテンツとしてマークするには、次のコードを使用します。

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

変更が保存されたら、次のように変更したフラグをクリアします。

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>ウィンドウを閉じる前に変更を保存する

ユーザーがウィンドウを閉じ、変更されたコンテンツを事前に保存できるようにするには、のサブクラスを作成し、そのメソッドをオーバーライドする必要があり `NSWindowDelegate` `WindowShouldClose` ます。 次に例を示します。

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWindowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWindowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            // is the window dirty?
            if (Window.DocumentEdited) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Critical,
                    InformativeText = "Save changes to document before closing window?",
                    MessageText = "Save Document",
                };
                alert.AddButton ("Save");
                alert.AddButton ("Lose Changes");
                alert.AddButton ("Cancel");
                var result = alert.RunSheetModal (Window);

                // Take action based on result
                switch (result) {
                case 1000:
                    // Grab controller
                    var viewController = Window.ContentViewController as ViewController;

                    // Already saved?
                    if (Window.RepresentedUrl != null) {
                        var path = Window.RepresentedUrl.Path;

                        // Save changes to file
                        File.WriteAllText (path, viewController.Text);
                        return true;
                    } else {
                        var dlg = new NSSavePanel ();
                        dlg.Title = "Save Document";
                        dlg.BeginSheet (Window, (rslt) => {
                            // File selected?
                            if (rslt == 1) {
                                var path = dlg.Url.Path;
                                File.WriteAllText (path, viewController.Text);
                                Window.DocumentEdited = false;
                                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                                viewController.View.Window.RepresentedUrl = dlg.Url;
                                Window.Close();
                            }
                        });
                        return true;
                    }
                    return false;
                case 1001:
                    // Lose Changes
                    return true;
                case 1002:
                    // Cancel
                    return false;
                }
            }

            return true;
        }
        #endregion
    }
}
```

次のコードを使用して、このデリゲートのインスタンスをウィンドウにアタッチします。

```csharp
// Set delegate
Window.Delegate = new EditorWindowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>アプリを閉じる前に変更を保存する

最後に、Xamarin アプリで、変更されたコンテンツが含まれている Windows があるかどうかを確認し、終了する前にユーザーが変更を保存できるようにする必要があります。 これを行うには、ファイルを編集し、 `AppDelegate.cs` メソッドをオーバーライドして、 `ApplicationShouldTerminate` 次のようにします。

```csharp
public override NSApplicationTerminateReply ApplicationShouldTerminate (NSApplication sender)
{
    // See if any window needs to be saved first
    foreach (NSWindow window in NSApplication.SharedApplication.Windows) {
        if (window.Delegate != null && !window.Delegate.WindowShouldClose (this)) {
            // Did the window terminate the close?
            return NSApplicationTerminateReply.Cancel;
        }
    }

    // Allow normal termination
    return NSApplicationTerminateReply.Now;
}
```

## <a name="working-with-multiple-windows"></a>複数のウィンドウの操作

ほとんどのドキュメントベースの Mac アプリケーションでは、複数のドキュメントを同時に編集できます。 たとえば、テキストエディターでは、複数のテキストファイルを同時に編集することができます。 既定では、新しい Xamarin. Mac アプリケーションには [**ファイル**] メニューがあり、自動的に**新しい**項目がアクションに自動的に接続され `newDocument:` **Action**ます。

次のコードでは、この新しい項目をアクティブ化し、ユーザーがメインウィンドウの複数のコピーを開いて、一度に複数のドキュメントを編集できるようにします。

ファイルを編集 `AppDelegate.cs` し、次の計算されたプロパティを追加します。

```csharp
public int UntitledWindowCount { get; set;} =1;
```

これを使用して、保存されていないファイルの数を追跡し、ユーザーにフィードバックを提供することができます (前述のように、Apple のガイドラインに従ってください)。

次に、次のメソッドを追加します。

```csharp
[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

このコードは、ウィンドウコントローラーの新しいバージョンを作成し、新しいウィンドウを読み込み、メインウィンドウとキーウィンドウにして、タイトルを設定します。 ここでアプリケーションを実行し、[**ファイル**] メニューから [**新規**] を選択すると、新しいエディターウィンドウが開き、表示されます。

[![新しい無題のウィンドウが追加されました](window-images/display04.png)](window-images/display04.png#lightbox)

**Windows**メニューを開くと、アプリケーションが自動的に追跡し、開いているウィンドウを処理していることがわかります。

[![Windows のメニュー](window-images/display05.png)](window-images/display05.png#lightbox)

Xamarin. Mac アプリケーションでのメニューの操作の詳細については、「[メニューの操作](~/mac/user-interface/menu.md)」のドキュメントを参照してください。

### <a name="getting-the-currently-active-window"></a>現在アクティブなウィンドウを取得しています

複数のウィンドウ (ドキュメント) を開くことができる Xamarin アプリケーションでは、現在の最上位ウィンドウ ([キー] ウィンドウ) を取得する必要がある場合があります。 次のコードは、キーウィンドウを返します。

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

現在のキーウィンドウにアクセスする必要がある任意のクラスまたはメソッドで呼び出すことができます。 現在開いているウィンドウがない場合は、が返され `null` ます。

### <a name="accessing-all-app-windows"></a>すべてのアプリウィンドウにアクセスする

Xamarin. Mac アプリで現在開いているすべてのウィンドウにアクセスする必要がある場合があります。 たとえば、ユーザーが開こうとしているファイルが既に開いているウィンドウで開かれているかどうかを確認します。

は、 `NSApplication.SharedApplication` `Windows` アプリ内のすべての開いているウィンドウの配列を格納するプロパティを保持します。 この配列を反復処理して、すべてのアプリの現在のウィンドウにアクセスできます。 次に例を示します。

```csharp
// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

コード例では、返された各ウィンドウを `ViewController` アプリのカスタムクラスにキャストし、ユーザーが開こうとしている `Path` ファイルのパスに対してカスタムプロパティの値をテストしています。 ファイルが既に開いている場合は、そのウィンドウを前面に表示します。

## <a name="adjusting-the-window-size-in-code"></a>コードでのウィンドウサイズの調整

アプリケーションでコード内のウィンドウのサイズを変更する必要がある場合もあります。 ウィンドウのサイズと位置を変更するには、そのウィンドウのプロパティを調整し `Frame` ます。 ウィンドウのサイズを調整する場合は、通常、macOS の座標系によって同じ場所にウィンドウを維持するために、原点を調整する必要もあります。

左上隅が (0, 0) を表す iOS の場合とは異なり、macOS では、画面の左下隅が (0, 0) を表す数学的座標系が使用されます。 IOS では、右方向へ移動すると座標が増加します。 MacOS では、座標は右に向かって値を大きくします。 

次のコード例では、ウィンドウのサイズを変更します。

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> コード内で windows のサイズと位置を調整する場合は、Interface Builder で設定した最小サイズと最大サイズを確実に調整する必要があります。 この設定は自動的には受け付けられず、ウィンドウをこれらの制限よりも大きくしたり小さくしたりすることができます。

## <a name="monitoring-window-size-changes"></a>ウィンドウサイズの変更の監視

Xamarin. Mac アプリ内のウィンドウサイズの変更を監視する必要がある場合があります。 たとえば、新しいサイズに合うようにコンテンツを再描画します。

サイズの変更を監視するには、まず、Xcode の Interface Builder でウィンドウコントローラーのカスタムクラスが割り当てられていることを確認します。 たとえば、 `MasterWindowController` 次のようにします。

[![Id インスペクター](window-images/resize01.png)](window-images/resize01.png#lightbox)

次に、カスタムウィンドウコントローラークラスを編集し、 `DidResize` コントローラーのウィンドウでイベントを監視して、ライブサイズの変更が通知されるようにします。 次に例を示します。

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

必要に応じて、イベントを使用して、 `DidEndLiveResize` ユーザーがウィンドウのサイズ変更を完了した後にのみ通知を受け取ることができます。 たとえば次のようになります。

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

        Window.DidEndLiveResize += (sender, e) => {
        // Do something after the user's finished resizing
        // the window
    };
}
```

## <a name="setting-a-windows-title-and-represented-file"></a>ウィンドウのタイトルと表現されたファイルの設定

ドキュメントを表すウィンドウを操作する場合、に `NSWindow` はプロパティがあります。これは、 `DocumentEdited` をに設定すると、 `true` ファイルが変更されたことをユーザーに示すため、閉じる前に保存する必要があることを示します。

`ViewController.cs`ファイルを編集し、次のように変更してみましょう。

```csharp
public bool DocumentEdited {
    get { return View.Window.DocumentEdited; }
    set { View.Window.DocumentEdited = value; }
}
...

public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";

    View.Window.WillClose += (sender, e) => {
        // is the window dirty?
        if (DocumentEdited) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to give the user the ability to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    };
}

public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Show when the document is edited
    DocumentEditor.TextDidChange += (sender, e) => {
        // Mark the document as dirty
        DocumentEdited = true;
    };

    // Overriding this delegate is required to monitor the TextDidChange event
    DocumentEditor.ShouldChangeTextInRanges += (NSTextView view, NSValue[] values, string[] replacements) => {
        return true;
    };

}
```

また `WillClose` 、ウィンドウでイベントを監視し、プロパティの状態を確認し `DocumentEdited` ます。 その場合は、 `true` 変更をファイルに保存する権限をユーザーに付与する必要があります。 アプリを実行してテキストを入力すると、ドットが表示されます。

[![変更されたウィンドウ](window-images/file01.png)](window-images/file01.png#lightbox)

ウィンドウを閉じようとすると、アラートが表示されます。

[![保存ダイアログの表示](window-images/file02.png)](window-images/file02.png#lightbox)

ファイルからドキュメントを読み込む場合は、メソッドを使用して、ウィンドウのタイトルをファイルの名前に設定し `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));` ます (これ `path` は、開いているファイルを表す文字列です)。 また、メソッドを使用して、ファイルの URL を設定することもでき `window.RepresentedUrl = url;` ます。

URL が OS によって認識されるファイルの種類を指している場合、そのアイコンはタイトルバーに表示されます。 ユーザーがアイコンを右クリックすると、ファイルへのパスが表示されます。

ファイルを編集 `AppDelegate.cs` し、次のメソッドを追加します。

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = true;
    dlg.CanChooseDirectories = false;

    if (dlg.RunModal () == 1) {
        // Nab the first file
        var url = dlg.Urls [0];

        if (url != null) {
            var path = url.Path;

            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow(this);

            // Load the text into the window
            var viewController = controller.Window.ContentViewController as ViewController;
            viewController.Text = File.ReadAllText(path);
                    viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
            viewController.View.Window.RepresentedUrl = url;

        }
    }
}
```

ここでアプリを実行する場合は、[**ファイル**] メニューの [**開く**] を選択し、[**開く**] ダイアログボックスからテキストファイルを選択して開きます。

[![[開く] ダイアログボックス](window-images/file03.png)](window-images/file03.png#lightbox)

ファイルが表示され、ファイルのアイコンでタイトルが設定されます。

[![読み込まれたファイルの内容](window-images/file04.png)](window-images/file04.png#lightbox)

## <a name="adding-a-new-window-to-a-project"></a>プロジェクトへの新しいウィンドウの追加

メインのドキュメントウィンドウとは別に、Xamarin. Mac アプリケーションでは、他の種類のウィンドウをユーザーに表示する必要がある場合があります (ユーザー設定やインスペクターパネルなど)。

新しいウィンドウを追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、ファイルをダブルクリックし `Main.storyboard` て、Xcode の Interface Builder で編集するために開きます。
2. 新しい**ウィンドウコントローラー**を**ライブラリ**からドラッグし、**デザインサーフェイス**にドロップします。

    [![ライブラリで新しいウィンドウコントローラーを選択する](window-images/new01.png)](window-images/new01.png#lightbox)
3. **Id インスペクター**で、 `PreferencesWindow` **ストーリーボード ID**として「」と入力します。 

    [![ストーリーボード ID の設定](window-images/new02.png)](window-images/new02.png#lightbox)
4. インターフェイスを設計します。 

    [![UI のデザイン](window-images/new03.png)](window-images/new03.png#lightbox)
5. アプリメニュー ( `MacWindows` ) を開き、[**基本設定...**] を選択し、コントロールをクリックして新しいウィンドウにドラッグします。 

    [![セグエの作成](window-images/new05.png)](window-images/new05.png#lightbox)
6. ポップアップメニューから [**表示**] を選択します。
7. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

コードを実行し、[**アプリケーション] メニュー**の [**基本設定...** ] を選択すると、ウィンドウが表示されます。

[![[基本設定] メニュー](window-images/new04.png)](window-images/new04.png#lightbox)

## <a name="working-with-panels"></a>パネルの操作

この記事の冒頭で説明したように、パネルは他のウィンドウの上にフローティングし、ドキュメントが開いている間にユーザーが操作できるツールまたはコントロールを提供します。 

Xamarin. Mac アプリケーションで作成して使用する他の種類のウィンドウと同様に、プロセスは基本的に同じです。

1. 新しいウィンドウ定義をプロジェクトに追加します。
2. ファイルをダブルクリックして `.xib` 、Xcode の Interface Builder で編集するためのウィンドウデザインを開きます。
3. **属性インスペクター**と**サイズインスペクター**で必要なウィンドウプロパティを設定します。
4. インターフェイスを構築するために必要なコントロールをドラッグして、**属性インスペクター**で構成します。
5. **サイズインスペクター**を使用して、UI 要素のサイズ変更を処理します。
6. ウィンドウの UI 要素を、**コンセント**と**アクション**を使用して C# コードに公開します。
7. 変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。

**属性インスペクター**には、パネルに固有の次のオプションがあります。

[![属性インスペクター](window-images/panel03.png)](window-images/panel03.png#lightbox)

- **スタイル**-[標準] パネル (標準ウィンドウのように表示される)、[ユーティリティ] パネル (小さいタイトルバー)、[HUD] パネル (半透明で、タイトルバーが背景の一部である) からパネルのスタイルを調整できます。
- **非アクティブ化**-パネル内のがキーウィンドウになるかどうかを判断します。
- **ドキュメントモーダル**-ドキュメントモーダルの場合、パネルはアプリケーションのウィンドウの上にのみ表示され、それ以外の場合はすべてを超えます。

新しいパネルを追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[ **Add**  >  **新しいファイル**の追加] を選択します。
2. [新しいファイル] ダイアログボックスで、次のように選択し**ます。**  >  **Cocoa Window with Controller**

    [![新しいウィンドウコントローラーの追加](window-images/panels00.png)](window-images/panels00.png#lightbox)

3. **[名前]** に「`DocumentPanel`」と入力し、 **[新規]** ボタンをクリックします。
4. ファイルをダブルクリックし `DocumentPanel.xib` て、Interface Builder で編集するために開きます。 

    [![パネルを編集する](window-images/new02.png)](window-images/new02.png#lightbox)

5. 既存のウィンドウを削除し、**インターフェイスエディター**で**ライブラリインスペクター**からパネルをドラッグします。 

    [![既存のウィンドウを削除しています](window-images/panels01.png)](window-images/panels01.png#lightbox)

6. **ファイルのオーナー**ウィンドウのアウトレットにパネルをフックし  -  **window**  -  **Outlet**ます。 

    [![ドラッグしてパネルを接続する](window-images/panels02.png)](window-images/panels02.png#lightbox)

7. **Id インスペクター**に切り替えて、パネルのクラスを次のように設定し `DocumentPanel` ます。 

    [![パネルのクラスを設定する](window-images/panels03.png)](window-images/panels03.png#lightbox)

8. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。
9. ファイルを編集 `DocumentPanel.cs` し、クラス定義を次のように変更します。 

    `public partial class DocumentPanel : NSPanel`

10. 変更をファイルに保存します。

ファイルを編集 `AppDelegate.cs` し、 `DidFinishLaunching` メソッドを次のようにします。

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

アプリケーションを実行すると、パネルが表示されます。

[![実行中のアプリのパネル](window-images/panels04.png)](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> パネルウィンドウは Apple によって非推奨とされているため、**インスペクターインターフェイス**に置き換える必要があります。 Xamarin. Mac アプリで**インスペクター**を作成する完全な例については、 [macinspector](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector)サンプルアプリを参照してください。

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでのウィンドウとパネルの使用について詳しく説明しました。 Windows とパネルのさまざまな種類と用途、Xcode の Interface Builder でウィンドウとパネルを作成および管理する方法、C# コードでウィンドウとパネルを操作する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacWindows (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [MacInspector (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [メニューの作業](~/mac/user-interface/menu.md)
- [macOS デザインテーマ (Apple)](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/)
- [ウィンドウ、パネル、画面 (Apple)](https://developer.apple.com/documentation/appkit/windows_panels_and_screens)
