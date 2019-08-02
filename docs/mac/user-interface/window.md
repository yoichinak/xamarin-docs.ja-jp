---
title: Xamarin. Mac のウィンドウ
description: この記事では、Xamarin. Mac アプリケーションでのウィンドウとパネルの使用について説明します。 Xcode と Interface Builder でのウィンドウとパネルの作成、ストーリーボードと xib ファイルからの読み込み、プログラムによる作業について説明します。
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 6d766e74f99e3c69259a41ce13501de80cf0231a
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653113"
---
# <a name="windows-in-xamarinmac"></a>Xamarin. Mac のウィンドウ

_この記事では、Xamarin. Mac アプリケーションでのウィンドウとパネルの使用について説明します。Xcode と Interface Builder でのウィンドウとパネルの作成、ストーリーボードと xib ファイルからの読み込み、プログラムによる作業について説明します。_

Xamarin. Mac C#アプリケーションでおよび .net を使用する場合、開発者が*Xcode と* *で作業*するのと同じウィンドウとパネルにアクセスできます。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用して、ウィンドウとパネルを作成および管理できます (また、 C#必要に応じて、コード内で直接作成することもできます)。

Xamarin アプリケーションでは、その目的に基づいて、画面上に1つまたは複数のウィンドウを表示して、表示および操作する情報を管理および調整できます。 ウィンドウの主な機能は次のとおりです。

1. ビューとコントロールを配置および管理できる領域を提供する。
2. キーボードとマウスの両方を使用したユーザーの操作に応じて、イベントを受け入れて応答する。

ウィンドウは、モードレス状態 (複数のドキュメントを一度に開くことができるテキストエディターなど) またはモーダル (アプリケーションを続行する前に閉じる必要があるエクスポートダイアログなど) で使用できます。

パネルとは、特殊な種類のウィンドウ (基本`NSWindow`クラスのサブクラス) です。通常は、テキスト形式のインスペクターやシステムカラーピッカーなどのユーティリティウィンドウなど、アプリケーションで補助関数を提供します。

[![](window-images/intro01.png "Xcode でのウィンドウの編集")](window-images/intro01.png#lightbox)

この記事では、Xamarin. Mac アプリケーションで Windows とパネルを操作するための基本について説明します。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。この記事をご覧ください。

[Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの「[クラス/ C#メソッドを目的に公開](~/mac/internals/how-it-works.md)する」セクションを参照して、 C#クラスをに接続するために使用`Register`さ`Export`れるコマンドとコマンドについて説明します。目的 C オブジェクトと UI 要素。

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Windows の概要

前述のように、ウィンドウには、ビューとコントロールを配置および管理し、ユーザーの操作 (キーボードまたはマウスを使用) に基づいてイベントに応答する領域が用意されています。

Apple によれば、macOS アプリには次の5つの主要な種類のウィンドウがあります。

- **ドキュメントウィンドウ**-ドキュメントウィンドウには、スプレッドシートやテキストドキュメントなどのファイルベースのユーザーデータが含まれています。
- **アプリウィンドウ**-アプリウィンドウは、ドキュメントベースではないアプリケーションのメインウィンドウです (Mac 上の予定表アプリなど)。
- **パネル**-他のウィンドウの上にフローティングし、ドキュメントが開いている間にユーザーが操作できるツールまたはコントロールが用意されています。 場合によっては、パネルを半透明にすることができます (大規模なグラフィックスを操作する場合など)。
- **ダイアログ**-ユーザーの操作に応答してダイアログが表示され、通常、ユーザーがアクションを完了する方法が示されます。 ダイアログを閉じるには、ユーザーからの応答が必要です。 (「[ダイアログの操作」を](~/mac/user-interface/dialog.md)参照)
- **アラート**-アラートは、重大な問題 (エラーなど) が発生したとき、または警告 (ファイルの削除の準備など) が発生したときに表示される特別な種類のダイアログです。 アラートはダイアログであるため、閉じる前にユーザーの応答も必要です。 (「[アラートの操作」を](~/mac/user-interface/alert.md)参照)

詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「 [About Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) 」セクションを参照してください。

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>メイン、キー、および非アクティブなウィンドウ

Xamarin. Mac アプリケーションのウィンドウは、ユーザーが現在どのように対話しているかに基づいて、外観と動作が異なります。 現在ユーザーの注目に重点を置いている最も重要なドキュメントまたはアプリウィンドウは、_メインウィンドウ_と呼ばれます。 ほとんどの場合、このウィンドウは_キーウィンドウ_(現在ユーザー入力を受け入れているウィンドウ) にもなります。 ただし、これは常にではありません。たとえば、カラーピッカーが開いている場合や、ユーザーが対話して、ドキュメントウィンドウ内の項目の状態を変更するためにユーザーが操作しているキーウィンドウである場合があります (これはメインウィンドウでもあります)。

メインウィンドウとキーウィンドウ (これらが分離されている場合) は常にアクティブであり、_非アクティブウィンドウ_は開いているウィンドウであり、フォアグラウンドにはありません。 たとえば、テキストエディターアプリケーションでは、一度に複数のドキュメントを開くことができ、メインウィンドウだけがアクティブになり、その他はすべて非アクティブになります。 

詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「 [About Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) 」セクションを参照してください。

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Windows の名前付け

ウィンドウにはタイトルバーを表示できます。タイトルが表示されるときは、通常、アプリケーションの名前、作業中のドキュメントの名前、またはウィンドウの機能 (インスペクターなど) が表示されます。 アプリケーションによっては、タイトルバーが表示されない場合があります。これは、表示によって認識され、ドキュメントを使用しないためです。

Apple では、次のガイドラインを提案します。

- ドキュメント以外のメインウィンドウのタイトルには、アプリケーション名を使用します。 
- 新しいドキュメントウィンドウ`untitled`に名前を指定します。 最初の新しいドキュメントでは、タイトルに数字 ( `untitled 1`など) を追加しないでください。 保存する前にユーザーが別の新しいドキュメントを作成し、最初の文書`untitled 2`に`untitled 3`タイトルを追加した場合は、そのウィンドウ、などを呼び出します。

詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「 [Windows の名前付け](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1)」を参照してください。

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>全画面表示ウィンドウ

MacOS では、アプリケーションのウィンドウが全画面表示され、アプリケーションのメニューバー (画面の上部にカーソルを移動すると表示されることがあります) をすべて非表示にすることができます。

Apple では、次のガイドラインが提案されています。

- ウィンドウが全画面表示に適しているかどうかを判断します。 簡単な操作 (電卓など) を提供するアプリケーションは、全画面表示モードを提供してはいけません。
- 全画面表示のタスクで必要な場合は、ツールバーを表示します。 通常、全画面表示モードでは、ツールバーは非表示になります。
- 全画面表示ウィンドウには、ユーザーがタスクを完了するために必要なすべての機能が含まれている必要があります。
- 可能であれば、ユーザーが全画面表示のウィンドウにある間は Finder の対話を避けてください。
- メインタスクからフォーカスを移動せずに、増加した画面領域を活用します。

詳細については、Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)の[全画面ウィンドウ](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1)のセクションを参照してください。

<a name="Panels" />

### <a name="panels"></a>パネル

パネルは、アクティブなドキュメントまたは選択 (システムカラーピッカーなど) に影響を与えるコントロールとオプションを含む補助ウィンドウです。

[![](window-images/panel01.png "カラーパネル")](window-images/panel01.png#lightbox)

パネルには、_アプリ固有_またはシステム_全体_を使用できます。 アプリ固有のパネルは、アプリケーションのドキュメントウィンドウの上部に表示され、アプリケーションがバックグラウンドで表示されると消えます。 システム全体のパネル ( **[フォント]** パネルなど) では、アプリケーションに関係なく、すべての開いているウィンドウの上にフローティングします。 

Apple では、次のガイドラインが提案されています。

- 一般に、標準パネルを使用すると、透明なパネルは控えめに使用する必要があるだけで、グラフィックを多用するタスクにも使用できます。
- ユーザーが自分のタスクに直接影響を与える重要なコントロールや情報に簡単にアクセスできるようにするには、パネルを使用することを検討してください。
- 必要に応じて、パネルを非表示にして表示します。
- パネルには常にタイトルバーを含める必要があります。
- パネルには、アクティブな [最小化] ボタンを含めないでください。

#### <a name="inspectors"></a>インスペクター

最新の macOS アプリケーションでは、パネルウィンドウを使用するのではなく、メインウィンドウの一部である_インスペクター_として、アクティブなドキュメントまたは選択項目に影響を与える補助コントロールとオプションが用意されています。

[![](window-images/panel02.png "インスペクターの例")](window-images/panel02.png#lightbox)

詳細については、Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)の[パネル](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1)セクションと、Xamarin. Mac アプリでの**Inspector インターフェイス**の完全な実装に関する[macinspector](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector)サンプルアプリを参照してください。

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>Xcode でのウィンドウの作成と保守

新しい Xamarin. Mac Cocoa アプリケーションを作成すると、既定で標準の空白のウィンドウが表示されます。 このウィンドウは、プロジェクトに`.storyboard`自動的に含まれるファイルに定義されています。 Windows のデザインを編集するには、**ソリューションエクスプローラー**で、 `Main.storyboard`ファイルをダブルクリックします。

[![](window-images/edit01.png "メインストーリーボードの選択")](window-images/edit01.png#lightbox)

これにより、Xcode の Interface Builder でウィンドウのデザインが開きます。

[![](window-images/edit02.png "Xcode で UI を編集する")](window-images/edit02.png#lightbox)

**属性インスペクター**には、ウィンドウの定義と制御に使用できるプロパティがいくつかあります。

- **Title** -これは、ウィンドウのタイトルバーに表示されるテキストです。
- **[自動保存]** -位置が指定され、設定が自動的に保存されるときにウィンドウを ID するために使用される_キー_です。
- **タイトルバー** -ウィンドウにタイトルバーが表示されます。
- 統合された**タイトルとツールバー** -ウィンドウにツールバーが含まれている場合は、タイトルバーの一部である必要があります。
- **全サイズのコンテンツビュー** -ウィンドウのコンテンツ領域をタイトルバーの下に配置できるようにします。
- **Shadow** -ウィンドウに影が付きます。
- **テクスチャ**が適用されたウィンドウは、vibrancy のような効果を使用して、本体の任意の場所にドラッグすることによって移動できます。
- **閉じる**: ウィンドウに 閉じる ボタンがあります。
- **最小化**-ウィンドウに最小化ボタンが表示されます。
- **[サイズ変更]** -ウィンドウにサイズ変更コントロールがあります。
- **ツールバーボタン**-ウィンドウにツールバーボタンが表示されます。
- **復元**可能-ウィンドウの位置と設定を自動的に保存および復元します。
- **起動時に表示**- `.xib`ファイルが読み込まれるときに、ウィンドウが自動的に表示されます。
- 非**アクティブ化時に非表示**にする-アプリケーションが背景に入ったときにウィンドウが非表示になります。
- **[終了時にリリース]** -ウィンドウが閉じられたときにメモリから削除されます。
- **常にツールヒントを表示**する-ツールヒントが常に表示されます。
- 再**計算ビューループ**-ウィンドウが描画される前に、ビューの順序が再計算されます。
- **スペース**、 **exposé** 、および**循環**-すべての macOS 環境でのウィンドウの動作を定義します。 
- **全画面**-このウィンドウが全画面表示モードに入ることができるかどうかを決定します。 
- **アニメーション**-ウィンドウで使用できるアニメーションの種類を制御します。
- **[外観]** -ウィンドウの外観を制御します。 ここでは、水色だけが表示されます。

詳細については、Apple の Windows と[Nswindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow)の[概要に](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)関するドキュメントを参照してください。

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>既定のサイズと場所の設定

ウィンドウの初期位置を設定し、サイズを制御するには、**サイズインスペクター**に切り替えます。

[![](window-images/edit07.png "既定のサイズと場所")](window-images/edit07.png#lightbox)

ここでは、ウィンドウの初期サイズを設定し、それに最小値と最大サイズを設定し、画面の最初の位置を設定して、ウィンドウの周囲の境界線を制御できます。

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>カスタムメインウィンドウコントローラーの設定

UI 要素をコードにC#公開するためのアウトレットとアクションを作成できるようにするには、Xamarin. Mac アプリでカスタムウィンドウコントローラーを使用する必要があります。

次の手順で行います。

1. Xcode の Interface Builder でアプリのストーリーボードを開きます。
2. `NSWindowController`デザインサーフェイスでを選択します。
3. **[Identity Inspector]** ビューに切り替え、 `WindowController` **クラス名**として「」と入力します。 

    [![](window-images/windowcontroller01.png "クラス名の設定")](window-images/windowcontroller01.png#lightbox)
4. 変更を保存し、Visual Studio for Mac に戻って同期します。
5. ファイルは、Visual Studio for Mac のソリューションエクスプローラーにプロジェクトに追加されます。 `WindowController.cs` 

    [![](window-images/windowcontroller02.png "Windows コントローラーの選択")](window-images/windowcontroller02.png#lightbox)
6. Xcode の Interface Builder でストーリーボードを再度開きます。
7. この`WindowController.h`ファイルは、次のように使用できます。 

    [![](window-images/windowcontroller03.png "WindowController .h ファイルを編集しています")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>UI 要素の追加

ウィンドウのコンテンツを定義するには、**ライブラリインスペクター**から**インターフェイスエディター**にコントロールをドラッグします。 Interface Builder を使用してコントロールを作成および有効にする方法の詳細については、 [Xcode の概要と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)のドキュメントを参照してください。

例として、**ライブラリインスペクター**から**インターフェイスエディター**のウィンドウにツールバーをドラッグしてみましょう。

[![](window-images/edit03.png "ライブラリからのツールバーの選択")](window-images/edit03.png#lightbox)

次に、**テキストビュー**をドラッグして、ツールバーの下の領域に合わせてサイズを変更します。

[![](window-images/edit04.png "テキストビューの追加")](window-images/edit04.png#lightbox)

ウィンドウのサイズが変更されたときに**テキストビュー**を縮小して拡大する必要があるため、**制約エディター**に切り替えて、次の制約を追加してみましょう。

[![](window-images/edit05.png "制約の編集")](window-images/edit05.png#lightbox)

エディターの上部にある**赤の I ビーム**のをクリックし、 **[4 つの制約の追加]** をクリックすると、指定した X、Y 座標に合わせてテキストビューが表示され、ウィンドウのサイズが変更されると、水平方向および垂直方向に拡大または縮小されます。

最後に、**テキストビュー**を**アウトレット**を使用してコードに公開します ( `ViewController.h`ファイルを選択してください)。

[![](window-images/edit06.png "アウトレットの構成")](window-images/edit06.png#lightbox)

変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。

**アウトレット**とアクションの操作の詳細について**は、** [アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)のドキュメントを参照してください。

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>標準ウィンドウワークフロー

Xamarin. Mac アプリケーションで作成して使用するすべてのウィンドウについて、このプロセスは基本的に上記の手順と同じです。

1. プロジェクトに自動的に追加されない新しいウィンドウの場合は、新しいウィンドウ定義をプロジェクトに追加します。 詳細については、以下で詳しく説明します。
1. `Main.storyboard`ファイルをダブルクリックして、Xcode の Interface Builder で編集するためのウィンドウデザインを開きます。
1. 新しいウィンドウをユーザーインターフェイスのデザインにドラッグし、_セグエ_を使用してウィンドウをメインウィンドウにフックします (詳細については、[ストーリーボードの操作](~/mac/platform/storyboards/indepth.md)に関するドキュメントの[セグエ](~/mac/platform/storyboards/indepth.md#Segues)に関するセクションを参照してください)。
1. **属性インスペクター**と**サイズインスペクター**で必要なウィンドウプロパティを設定します。
1. インターフェイスを構築するために必要なコントロールをドラッグして、**属性インスペクター**で構成します。
1. **サイズインスペクター**を使用して、UI 要素のサイズ変更を処理します。
1. ウィンドウの UI 要素を、 C# **コンセント**と**アクション**を使用してコードに公開します。
1. 変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。

基本的なウィンドウが作成されたので、windows を操作するときに、Xamarin アプリケーションが実行する一般的なプロセスを見ていきます。 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>既定のウィンドウを表示する

既定では、新しい Xamarin. Mac アプリケーションは、開始時に`MainWindow.xib`ファイルに定義されているウィンドウを自動的に表示します。

[![](window-images/display01.png "実行中のウィンドウの例")](window-images/display01.png#lightbox)

上のウィンドウのデザインを変更したため、既定のツールバーと**テキストビュー**コントロールが追加されました。 `Info.plist`ファイルの次のセクションでは、このウィンドウを表示します。

[![](window-images/display00.png "情報を編集しています。 plist")](window-images/display00.png#lightbox)

メインの **[インターフェイス]** ドロップダウンを使用して、メインアプリの UI (この場合`Main.storyboard`は) として使用するストーリーボードを選択します。

ビューコントローラーがプロジェクトに自動的に追加され、表示されるメインウィンドウ (およびそのプライマリビュー) が制御されます。 これは、 `ViewController.cs`ファイルで定義され、 **id インスペクター**の Interface Builder の**ファイルの所有者**にアタッチされます。

[![](window-images/display02.png "ファイルの所有者を設定しています")](window-images/display02.png#lightbox)

ウィンドウでは、最初にを開いたときにの`untitled`タイトルが表示されるようにするため、 `ViewWillAppear` `ViewController.cs`のメソッドをオーバーライドして次のようにします。

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
```    

> [!NOTE]
> メソッド`Title` では`ViewDidLoad`なく、メソッドでウィンドウのプロパティの値を設定しています。これは、ビューがメモリに読み込まれても、まだ完全にインスタンス化されていないためです。`ViewWillAppear` メソッドでプロパティ`Title`にアクセスしようとすると、ウィンドウがまだ構築`null`されていないため、例外が発生します。 `ViewDidLoad`

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>プログラムによるウィンドウの終了

ユーザーがウィンドウの **[閉じる]** ボタンをクリックしたり、メニュー項目を使用したりするのではなく、プログラムによって Xamarin. Mac アプリケーションでウィンドウを閉じることが必要になる場合があります。 macOS では、 `NSWindow` `PerformClose`との2つの異なる方法`Close`で、プログラムによってを閉じることができます。

<a name="PerformClose" />

### <a name="performclose"></a>パフォーマンスを閉じる

のメソッドを呼び出すと、ユーザーがウィンドウの [閉じる] ボタンをクリックすると、そのボタンが一時的に強調表示され、ウィンドウが閉じます。 `PerformClose` `NSWindow`

アプリケーションが`NSWindow`の`WillClose`イベントを実装する場合は、ウィンドウが閉じられる前に発生します。 イベントからが返さ`false`れた場合、ウィンドウは閉じられません。 ウィンドウに **[閉じる]** ボタンがない場合、または何らかの理由で閉じることができない場合は、OS によって警告音が出力されます。

例えば:

```csharp
MyWindow.PerformClose(this);
```

はインスタンスを`MyWindow` `NSWindow`終了しようとします。 成功した場合、ウィンドウは閉じられます。それ以外の場合は、警告音が出力され、が開いたままになります。

<a name="Close" />

### <a name="close"></a>閉じる

のメソッドを呼び出すと、ウィンドウを一時的に強調表示してウィンドウの **[閉じる]** ボタンをクリックしても、ウィンドウが閉じられるだけではありません。`NSWindow` `Close`

ウィンドウは閉じ`NSWindowWillCloseNotification`られている必要はなく、閉じられているウィンドウの既定の通知センターに通知が投稿されます。

メソッド`Close`は、 `PerformClose`メソッドの2つの重要な方法で異なります。

1. イベントの`WillClose`発生を試みることはありません。
2. ユーザーが **[閉じる]** ボタンをクリックしても、ボタンを一時的に強調表示することはできません。

例えば:

```csharp
MyWindow.Close();
```

`NSWindow`インスタンスを閉じ`MyWindow`ます。

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>変更された Windows コンテンツ

MacOS では、ユーザーにウィンドウ (`NSWindow`) の内容が変更されていること、および保存する必要があることをユーザーに通知する方法が Apple から提供されています。 ウィンドウに変更されたコンテンツが含まれている場合は、小さい黒い点が**閉じ**たウィジェットに表示されます。

[![](window-images/close01.png "変更されたマーカーを含むウィンドウ")](window-images/close01.png#lightbox)

ウィンドウのコンテンツに未保存の変更があるときに、ユーザーがウィンドウを閉じたり、Mac アプリを終了しようとしたりした場合は、[ダイアログボックス](~/mac/user-interface/dialog.md)または[モーダルシート](~/mac/user-interface/dialog.md)を表示し、ユーザーが最初に変更を保存できるようにする必要があります。

[![](window-images/close02.png "ウィンドウを閉じたときに表示される保存シート")](window-images/close02.png#lightbox)

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

ユーザーがウィンドウを閉じ、変更されたコンテンツを事前に保存できるようにするには、の`NSWindowDelegate`サブクラスを作成し、その`WindowShouldClose`メソッドをオーバーライドする必要があります。 例えば:

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

最後に、Xamarin アプリで、変更されたコンテンツが含まれている Windows があるかどうかを確認し、終了する前にユーザーが変更を保存できるようにする必要があります。 これを行うには、 `AppDelegate.cs`ファイルを編集し`ApplicationShouldTerminate` 、メソッドをオーバーライドして、次のようにします。

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

<a name="Working_with_Multiple_Windows" />

## <a name="working-with-multiple-windows"></a>複数のウィンドウの操作

ほとんどのドキュメントベースの Mac アプリケーションでは、複数のドキュメントを同時に編集できます。 たとえば、テキストエディターでは、複数のテキストファイルを同時に編集することができます。 既定では、新しい Xamarin. Mac アプリケーションには **[ファイル]** メニューがあり、自動的に**新しい**項目が`newDocument:` **アクション**に自動的に接続されます。

この新しい項目をアクティブ化し、ユーザーがメインウィンドウの複数のコピーを開いて、一度に複数のドキュメントを編集できるようにします。

`AppDelegate.cs`ファイルを編集し、次の計算されたプロパティを追加してみましょう。

```csharp
public int UntitledWindowCount { get; set;} =1;
```

これを使用して、保存されていないファイルの数を追跡し、ユーザーにフィードバックを提供できるようにします (前述のように、Apple のガイドラインに従ってください)。

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

このコードは、ウィンドウコントローラーの新しいバージョンを作成し、新しいウィンドウを読み込み、メインウィンドウとキーウィンドウにして、タイトルを設定します。 ここでアプリケーションを実行し、 **[ファイル]** メニューから **[新規]** を選択すると、新しいエディターウィンドウが開き、表示されます。

[![](window-images/display04.png "新しい無題のウィンドウが追加されました")](window-images/display04.png#lightbox)

**Windows**メニューを開くと、アプリケーションが自動的に追跡し、開いているウィンドウを処理していることがわかります。

[![](window-images/display05.png "Windows のメニュー")](window-images/display05.png#lightbox)

Xamarin. Mac アプリケーションでのメニューの操作の詳細については、「[メニューの操作](~/mac/user-interface/menu.md)」のドキュメントを参照してください。

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>現在アクティブなウィンドウを取得しています

複数のウィンドウ (ドキュメント) を開くことができる Xamarin アプリケーションでは、現在の最上位ウィンドウ ([キー] ウィンドウ) を取得する必要がある場合があります。 次のコードは、キーウィンドウを返します。

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

現在のキーウィンドウにアクセスする必要がある任意のクラスまたはメソッドで呼び出すことができます。 現在開いているウィンドウがない場合は、 `null`が返されます。

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>すべてのアプリウィンドウにアクセスする

Xamarin. Mac アプリで現在開いているすべてのウィンドウにアクセスする必要がある場合があります。 たとえば、ユーザーが開こうとしているファイルが既に開いているウィンドウで開かれているかどうかを確認します。

は`NSApplication.SharedApplication` 、アプリ`Windows`内のすべての開いているウィンドウの配列を格納するプロパティを保持します。 この配列を反復処理して、すべてのアプリの現在のウィンドウにアクセスできます。 例えば:

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

コード例では、返された各ウィンドウをアプリの`ViewController`カスタムクラスにキャストし、ユーザーが開こうとして`Path`いるファイルのパスに対してカスタムプロパティの値をテストしています。 ファイルが既に開いている場合は、そのウィンドウを前面に表示します。

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>コードでのウィンドウサイズの調整

アプリケーションでコード内のウィンドウのサイズを変更する必要がある場合もあります。 ウィンドウのサイズと位置を変更するには、 `Frame`そのウィンドウのプロパティを調整します。 ウィンドウのサイズを調整する場合は、通常、macOS の座標系によって同じ場所にウィンドウを維持するために、原点を調整する必要もあります。

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

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>ウィンドウサイズの変更の監視

Xamarin. Mac アプリ内のウィンドウサイズの変更を監視する必要がある場合があります。 たとえば、新しいサイズに合うようにコンテンツを再描画します。

サイズの変更を監視するには、まず、Xcode の Interface Builder でウィンドウコントローラーのカスタムクラスが割り当てられていることを確認します。 たとえば`MasterWindowController` 、次のようにします。

[![](window-images/resize01.png "Id インスペクター")](window-images/resize01.png#lightbox)

次に、カスタムウィンドウコントローラークラスを編集し、 `DidResize`コントローラーのウィンドウでイベントを監視して、ライブサイズの変更が通知されるようにします。 例えば:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

必要に応じて、 `DidEndLiveResize`イベントを使用して、ユーザーがウィンドウのサイズ変更を完了した後にのみ通知を受け取ることができます。 たとえば、次のように入力します。

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

<a name="Setting_a_Window’s_Title_and_Represented_File" />

## <a name="setting-a-windows-title-and-represented-file"></a>ウィンドウのタイトルと表現されたファイルの設定

ドキュメントを表すウィンドウを操作する場合`NSWindow` 、に`DocumentEdited`はプロパティがあります`true` 。これは、をに設定すると、ファイルが変更されたことをユーザーに示すため、閉じる前に保存する必要があることを示します。

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

また、ウィンドウでイベント`WillClose`を監視し、 `DocumentEdited`プロパティの状態を確認します。 その場合は`true` 、変更をファイルに保存する権限をユーザーに付与する必要があります。 アプリを実行してテキストを入力すると、ドットが表示されます。

[![](window-images/file01.png "変更されたウィンドウ")](window-images/file01.png#lightbox)

ウィンドウを閉じようとすると、アラートが表示されます。

[![](window-images/file02.png "保存ダイアログの表示")](window-images/file02.png#lightbox)

ファイルからドキュメントを読み込む場合は、 `window.SetTitleWithRepresentedFilename (Path.GetFileName(path));`メソッドを使用して、ウィンドウのタイトルをファイルの名前に設定できます ( `path`これは、開いているファイルを表す文字列です)。 また、 `window.RepresentedUrl = url;`メソッドを使用して、ファイルの URL を設定することもできます。

URL が OS によって認識されるファイルの種類を指している場合、そのアイコンはタイトルバーに表示されます。 ユーザーがアイコンを右クリックすると、ファイルへのパスが表示されます。

`AppDelegate.cs`ファイルを編集し、次のメソッドを追加してみましょう。

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

ここでアプリを実行する場合は、 **[ファイル]** メニューの **[開く]** を選択し、 **[開く]** ダイアログボックスからテキストファイルを選択して開きます。

[![](window-images/file03.png "[開く] ダイアログボックス")](window-images/file03.png#lightbox)

ファイルが表示され、ファイルのアイコンでタイトルが設定されます。

[![](window-images/file04.png "読み込まれたファイルの内容")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>プロジェクトへの新しいウィンドウの追加

メインのドキュメントウィンドウとは別に、Xamarin. Mac アプリケーションでは、他の種類のウィンドウをユーザーに表示する必要がある場合があります (ユーザー設定やインスペクターパネルなど)。

新しいウィンドウを追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、 `Main.storyboard`ファイルをダブルクリックして、Xcode の Interface Builder で編集するために開きます。
2. 新しい**ウィンドウコントローラー**を**ライブラリ**からドラッグし、**デザインサーフェイス**にドロップします。

    [![](window-images/new01.png "ライブラリで新しいウィンドウコントローラーを選択する")](window-images/new01.png#lightbox)
3. **Id インスペクター**で、 `PreferencesWindow` **ストーリーボード ID**として「」と入力します。 

    [![](window-images/new02.png "ストーリーボード ID の設定")](window-images/new02.png#lightbox)
4. インターフェイスを設計します。 

    [![](window-images/new03.png "UI のデザイン")](window-images/new03.png#lightbox)
5. アプリメニュー (`MacWindows`) を開き、 **[基本設定...]** を選択し、コントロールをクリックして新しいウィンドウにドラッグします。 

    [![](window-images/new05.png "セグエの作成")](window-images/new05.png#lightbox)
6. ポップアップメニューから **[表示]** を選択します。
7. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

コードを実行し、[**アプリケーション] メニュー**の **[基本設定...]** を選択すると、ウィンドウが表示されます。

[![](window-images/new04.png "[基本設定] メニュー")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>パネルの操作

この記事の冒頭で説明したように、パネルは他のウィンドウの上にフローティングし、ドキュメントが開いている間にユーザーが操作できるツールまたはコントロールを提供します。 

Xamarin. Mac アプリケーションで作成して使用する他の種類のウィンドウと同様に、プロセスは基本的に同じです。

1. 新しいウィンドウ定義をプロジェクトに追加します。
2. `.xib`ファイルをダブルクリックして、Xcode の Interface Builder で編集するためのウィンドウデザインを開きます。
3. **属性インスペクター**と**サイズインスペクター**で必要なウィンドウプロパティを設定します。
4. インターフェイスを構築するために必要なコントロールをドラッグして、**属性インスペクター**で構成します。
5. **サイズインスペクター**を使用して、UI 要素のサイズ変更を処理します。
6. ウィンドウの UI 要素を、 C# **コンセント**と**アクション**を使用してコードに公開します。
7. 変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。

**属性インスペクター**には、パネルに固有の次のオプションがあります。

[![](window-images/panel03.png "属性インスペクター")](window-images/panel03.png#lightbox)

- **スタイル**-パネルのスタイルを次のように調整できます。通常のパネル (標準のウィンドウのように見えます)、[ユーティリティ] パネル (タイトルバーが小さい)、[HUD] パネル (半透明、タイトルバーは背景の一部です)。
- **非アクティブ化**-パネル内のがキーウィンドウになるかどうかを判断します。
- **ドキュメントモーダル**-ドキュメントモーダルの場合、パネルはアプリケーションのウィンドウの上にのみ表示され、それ以外の場合はすべてを超えます。


新しいパネルを追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、プロジェクトを右クリックし、[新しいファイルの**追加** >  **..** .] を選択します。
2. [新しいファイル] ダイアログボックスで **、次の**ように選択し**ます。**  > 

    [![](window-images/panels00.png "新しいウィンドウコントローラーの追加")](window-images/panels00.png#lightbox)
3. **[名前]** に「`DocumentPanel`」と入力し、 **[新規]** ボタンをクリックします。
4. `DocumentPanel.xib`ファイルをダブルクリックして、Interface Builder で編集するために開きます。 

    [![](window-images/new02.png "パネルを編集する")](window-images/new02.png#lightbox)
5. 既存のウィンドウを削除し、**インターフェイスエディター**で**ライブラリインスペクター**からパネルをドラッグします。 

    [![](window-images/panels01.png "既存のウィンドウを削除しています")](window-images/panels01.png#lightbox)
6. **ファイルのオーナー** - ウィンドウ - の**アウトレット**にパネルをフックします。 

    [![](window-images/panels02.png "ドラッグしてパネルを接続する")](window-images/panels02.png#lightbox)
7. **Id インスペクター**に切り替えて、パネルのクラスを次の`DocumentPanel`ように設定します。 

    [![](window-images/panels03.png "パネルのクラスを設定する")](window-images/panels03.png#lightbox)
8. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。
9. `DocumentPanel.cs`ファイルを編集し、クラス定義を次のように変更します。 

    `public partial class DocumentPanel : NSPanel`
10. 変更内容をファイルに保存します。

ファイルを編集し、メソッド`DidFinishLaunching`を次のようにします。 `AppDelegate.cs`

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

アプリケーションを実行すると、パネルが表示されます。

[![](window-images/panels04.png "実行中のアプリのパネル")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> パネルウィンドウは Apple によって非推奨とされているため、**インスペクターインターフェイス**に置き換える必要があります。 Xamarin. Mac アプリで**インスペクター**を作成する完全な例については、 [macinspector](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector)サンプルアプリを参照してください。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでのウィンドウとパネルの使用について詳しく説明しました。 Windows とパネルのさまざまな種類と用途、Xcode の Interface Builder でウィンドウとパネルを作成および管理する方法、およびコードでC#ウィンドウとパネルを操作する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacWindows (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [MacInspector (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macinspector)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [メニューの操作](~/mac/user-interface/menu.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
