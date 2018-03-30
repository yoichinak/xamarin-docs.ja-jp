---
title: Windows
description: この記事では、windows と Xamarin.Mac アプリケーション内のパネルでの作業について説明します。 これには、作成元の windows と Xcode とストーリー ボードや .xib ファイルから読み込むと、それらをプログラムによって操作のインターフェイス ビルダー内のパネルがについて説明します。
ms.topic: article
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4b8de30cecb738fecb13616a3b796c0b4fa5a51a
ms.sourcegitcommit: 7b88081a979381094c771421253d8a388b2afc16
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/30/2018
---
# <a name="windows"></a>Windows

_この記事では、windows と Xamarin.Mac アプリケーション内のパネルでの作業について説明します。これには、作成元の windows と Xcode とストーリー ボードや .xib ファイルから読み込むと、それらをプログラムによって操作のインターフェイス ビルダー内のパネルがについて説明します。_

同じウィンドウにアクセスし、で作業する開発者をパネル Xamarin.Mac アプリケーションでは、c# と .NET で作業するとき*OBJECTIVE-C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成し、ウィンドウとパネルの管理 (または必要に応じて c# コードで直接作成すること)。

目的に基づき、Xamarin.Mac アプリケーションが画面上の Windows を管理および情報が表示されますで動作を調整する 1 つまたは複数を提供します。 ウィンドウの主要な機能は次のとおりです。

1. どのビュー、およびコントロール内の領域を提供するには、配置および管理できます。
2. 受け入れ、キーボードとマウスの両方でユーザーとの対話に応答内のイベントに応答します。

Windows は、(アプリケーションを続行する前に閉じる必要があります、エクスポート ダイアログ) などのモーダルまたはモードレスの状態 (などを同時に開いて複数のドキュメントを持つことができるテキスト エディター) で使用することができます。

パネルは、特殊なウィンドウ (ベースのサブクラス`NSWindow`クラス)、テキスト形式のインスペクターとシステム カラー ピッカーなどのユーティリティ ウィンドウなどのアプリケーションで補助的な関数に通常使用します。

[![](window-images/intro01.png "Xcode でウィンドウの編集")](window-images/intro01.png#lightbox)

この記事でしれませんには Windows とパネル Xamarin.Mac アプリケーションで操作の基本について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Windows の概要

前述のように、ウィンドウをビューとコントロールを配置および管理できる領域を提供し、ユーザーとの対話 (もキーボードまたはマウスを使用して) に基づいてイベントを応答します。

Apple にに従って macOS アプリで、メインの種類のウィンドウと 5 つがあります。

- **ドキュメント ウィンドウ**-ドキュメント ウィンドウには、スプレッドシートやテキスト ドキュメントなどのファイル ベースのユーザー データが含まれています。
- **アプリ ウィンドウ**-アプリ ウィンドウは、(Mac で予定表アプリ) のようなドキュメント ベースではないアプリケーションのメイン ウィンドウです。
- **パネル**ドキュメントは、中にユーザーを使用するコントロールを開く - か、パネルの他のウィンドウから浮いてし、ツールを提供します。 場合によっては、パネルを半透明にすることができます (場合などの大規模なグラフィックス操作)。
- **ダイアログ**-、ダイアログは、ユーザーの操作への応答が表示され、通常の方法のユーザーが操作を完了できますを提供します。 ダイアログ ボックスを終了する前に、ユーザーからの応答が必要です。 (を参照してください[ダイアログで操作](~/mac/user-interface/dialog.md))
- **アラート**-警告は、特殊な種類 (エラー) など、深刻な問題が発生したときに表示されるダイアログ ボックスのまたは (ファイルを削除する準備をしています) などの警告として。 アラートは、ダイアログ ボックスであるため、必要もありますユーザーからの応答を終了する前にします。 (を参照してください[アラート](~/mac/user-interface/alert.md))

詳細については、次を参照してください、[に関する Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple のセクション[OS X のヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>メイン、キー、および非アクティブなウィンドウ

Xamarin.Mac アプリケーションでの Windows では、検索でき、ユーザーが現在それらと対話する方法に応じて異なる動作することができます。 最も重要なドキュメントまたはフォーカスが置かれて現在ユーザーの注目を集めるアプリ ウィンドウを呼び出す、_のメイン ウィンドウ_します。 ほとんどの場合にこのウィンドウもされます、_キー ウィンドウ_(ユーザー入力を受け付けると現在のウィンドウ)。 これが当てはまらない場合、たとえば、カラー ピッカーでしたが開いている (これは、メイン ウィンドウもよい) のドキュメント ウィンドウ内の項目の状態を変更すると、ユーザーが対話するキー ウィンドウします。

メイン レポートとキーの Windows (個別ある) 場合は、アクティブ、常に_非アクティブなウィンドウ_が開いているウィンドウをフォア グラウンドに含まれていません。 たとえば、テキスト エディターのアプリケーションが複数のドキュメントを持つことが、一度に開くだけで、メイン ウィンドウがアクティブになります、他のすべていないアクティブになります。 

詳細については、次を参照してください、[に関する Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1) Apple のセクション[OS X のヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Windows の名前を付ける

ウィンドウ タイトル バーを表示でき、タイトルが表示される場合は通常、アプリケーションの名前、作業中でドキュメントの名前またはウィンドウ (インスペクター) などの関数。 一部のアプリケーションは、視認で認識可能なは、ドキュメントとでは動作しないために、タイトル バーを表示しません。

Apple では、次のガイドラインを提案します。

- アプリケーション名、ドキュメント以外のメイン ウィンドウのタイトルを使用します。 
- 新しいドキュメント ウィンドウの名前を付けます`untitled`です。 最初の新しいドキュメントにない番号を追加、タイトルに (など`untitled 1`)。 ユーザーは、保存して、最初のタイトルの前に別の新しいドキュメントを作成する場合は、そのウィンドウを呼び出して`untitled 2`、 `untitled 3`, などです。

詳細については、次を参照してください、[名前付けの Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1) Apple のセクション[OS X のヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>全画面表示ウィンドウ

MacOS などのアプリケーションのウィンドウ進むことができます全画面表示の非表示にする (これは、画面の上部にカーソルを移動することによって参照される可能性が) アプリケーションのメニュー バーを含むすべての式と無料のやり取りはコンテンツを邪魔を提供します。

Apple は、次のガイドラインを示しています。

- 全画面表示を移動するウィンドウの理にかなっているかどうかを決定します。 (電卓) などの簡単な相互作用を提供するアプリケーションでは、全画面表示モードを提供しないでください。
- 全画面表示タスク必要なかどうか、ツールバーを表示します。 通常、ツールバーは全画面表示モードでは表示されません。
- 全画面表示ウィンドウでは、タスクを完了する必要があるすべての機能のユーザーが必要です。
- 可能であれば、ユーザーが全画面表示ウィンドウで中には、ファインダー相互作用をされないようにします。
- 主なタスクからフォーカスを移動せず増大画面スペースを利用します。

詳細については、次を参照してください、[全画面表示 Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1) Apple のセクション[OS X のヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>パネル

パネルは、コントロールと、作業中の文書または (システム カラー ピッカー) などの選択に影響するオプションを含む補助ウィンドウには。

[![](window-images/panel01.png "カラー パネル")](window-images/panel01.png#lightbox)

パネルには、いずれかを指定できる_アプリ固有_または_システム全体_です。 アプリ固有のパネルでは、アプリケーションのドキュメント ウィンドウの上に float し、アプリケーションがバック グラウンドでは、表示されなくなります。 システム全体のパネル (など、**フォント**パネル)、アプリケーションに関係なくすべての開いているウィンドウの上に浮動小数点数。 

Apple は、次のガイドラインを示しています。

- 一般に、標準のパネルを使用して、透過的なパネルは、慎重および視覚的に処理を要するタスクにのみ使用する必要があります。
- パネルを使用してユーザーが重要なコントロールや、タスクに直接影響する情報に簡単にアクセスできるようにすることを検討してください。
- 非表示にし、必要に応じてパネルを表示します。
- パネルは、タイトル バーを常に含める必要があります。
- パネルには、アクティブな最小化ボタンは含めないでください。

#### <a name="inspectors"></a>インスペクター

最近のほとんどの macOS アプリケーションは、補助コントロールと、作業中の文書またはとして選択に影響するオプションを提示_インスペクター_メイン ウィンドウの一部である (と同様に、**ページ**アプリ下図)、パネルの Windows ではなく。

[![](window-images/panel02.png "例インスペクター")](window-images/panel02.png#lightbox)

詳細については、次を参照してください、[パネル](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1)Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)と当社[MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) 、の完全な実装のサンプルアプリ**。インスペクター インターフェイス**Xamarin.Mac アプリでします。

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>作成し、Xcode での Windows の管理

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、`.storyboard`プロジェクトに自動的に含まれるファイル。 Windows のデザインを編集する、**ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイル。

[![](window-images/edit01.png "メインのストーリー ボードを選択します。")](window-images/edit01.png#lightbox)

Xcode のインターフェイスのビルダーで、ウィンドウのデザインが開きます。

[![](window-images/edit02.png "Xcode で UI を編集")](window-images/edit02.png#lightbox)

**属性インスペクター**、いくつかのプロパティを定義して、ウィンドウを制御に使用できます。

- **タイトル**-これは、ウィンドウのタイトル バーに表示されるテキストです。
- **自動保存**-これは、_キー_の位置、および設定が自動的に保存されるときに、ウィンドウを ID に使用されます。
- **タイトル バー**のウィンドウは、タイトル バーを表示します。
- **統合されたタイトルおよびツールバー** - 場合は、ウィンドウには、ツールバー、タイトル バーの一部にする必要があります。
- **完全な規模のコンテンツ ビュー** -タイトル バーの下にあるウィンドウのコンテンツ領域を使用します。
- **シャドウ**-には、ウィンドウが影付きです。
- **テクスチャ**-質感の windows (活気) のような効果を使用できるし、本文に任意の場所にドラッグして移動することができます。
- **閉じる**-には、ウィンドウが閉じるボタンをクリックします。
- **最小限に抑える**-には、ウィンドウが最小化ボタンをクリックします。
- **サイズを変更する**-には、ウィンドウがサイズ変更コントロールです。
- **ツール バー ボタン**-には、ウィンドウが非表示/表示 ツールバーのボタンをクリックします。
- **復元可能な**-はウィンドウの位置と設定が自動的に保存および復元します。
- **表示で起動**-ウィンドウに自動的に時に表示される、`.xib`ファイルが読み込まれています。
- **非アクティブ化で非表示に**-は、ウィンドウを非表示、アプリケーションはバック グラウンドに入るとします。
- **ときに閉じられたリリース**-は、ウィンドウが閉じられたときに、メモリから削除します。
- **常に表示ツールヒント**-は常に表示されるツールヒント。
- **ビューのループを再計算**-表示順序をウィンドウが描画される前に再計算されます。
- **スペース**、 **expos ・**と**Cycling** -すべて macOS 環境で、ウィンドウの動作を定義します。 
- **完全な画面**のかどうか、このウィンドウは、全画面表示モードを入力できますを決定します。 
- **アニメーション**のウィンドウの使用できるアニメーションの種類を制御します。
- **外観**のウィンドウの外観を制御します。 ここでは、1 つだけの外観、アクアがあります。

Apple を参照してください[Introduction to Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)と[NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow)詳細についてはドキュメントです。

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>既定のサイズと場所の設定

ウィンドウの最初の位置を設定してのサイズを制御するには、スイッチを**サイズ インスペクター**:

[![](window-images/edit07.png "既定のサイズと場所")](window-images/edit07.png#lightbox)

ここから、ウィンドウの初期サイズを設定、最小値と最大サイズを指定、画面の最初の場所を設定し、制御できますウィンドウの周囲の罫線。

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>カスタムのメイン ウィンドウのコント ローラーを設定

C# コードを UI 要素を公開するには、コンセントとアクションを作成できるようにするには、Xamarin.Mac アプリは、カスタム ウィンドウ コント ローラーを使用する必要があります。

次の手順で行います。

1. Xcode のインターフェイスのビルダーで、アプリのストーリー ボードを開きます。
2. 選択、`NSWindowController`デザイン画面にします。
3. 切り替えて、 **Identity インスペクター**を表示および入力`WindowController`として、**クラス名**: 

    [![](window-images/windowcontroller01.png "クラス名を設定します。")](window-images/windowcontroller01.png#lightbox)
4. 変更内容を保存し、同期する Mac 用の Visual Studio に戻ります。
5. A`WindowController.cs`ファイルは、プロジェクトに追加する、**ソリューション エクスプ ローラー** Mac 用の Visual Studio で。 

    [![](window-images/windowcontroller02.png "Windows のコント ローラーを選択します。")](window-images/windowcontroller02.png#lightbox)
6. Xcode のインターフェイスのビルダーでストーリー ボードを再度開きます。
7. `WindowController.h`ファイルが使用できるようになります。 

    [![](window-images/windowcontroller03.png "WindowController.h ファイルの編集")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>UI 要素を追加します。

ウィンドウの内容を定義するからコントロールをドラッグ、**ライブラリ インスペクター**上に、**インターフェイス エディター**です。 参照してください、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)のドキュメントの詳細については、インターフェイスのビルダーを使用して作成し、コントロールを有効にします。

たとえば、ドラッグしてからツールバー、**ライブラリ インスペクター**でウィンドウの上に、**インターフェイス エディター**:

[![](window-images/edit03.png "ライブラリから、ツールバーを選択します。")](window-images/edit03.png#lightbox)

次に、ドラッグ、**テキスト ビュー**し、ツールバーの下の領域に合わせてサイズします。

[![](window-images/edit04.png "テキスト ビューを追加します。")](window-images/edit04.png#lightbox)

**テキスト ビュー**にウィンドウのサイズの変化に応じて拡大および縮小、ここに切り替えて、**制約エディター**し、次の制約を追加。

[![](window-images/edit05.png "制約の編集")](window-images/edit05.png#lightbox)

クリックしての**赤い I ビーム**エディターの上部と順にクリックで**4 制約の追加**、テキスト ビューで指定した X、Y 座標にこだわると拡大または縮小水平方向および垂直方向に指示することとしてウィンドウをサイズします。

最後に、公開みましょう、**テキスト ビュー**を使用してコードを**コンセント**(選択することを確認、`ViewController.h`ファイル)。

[![](window-images/edit06.png "コンセントを構成します。")](window-images/edit06.png#lightbox)

変更を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

操作の詳細については**コンセント**と**アクション**を参照してください、[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)ドキュメント。

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>標準的なウィンドウのワークフロー

作成および Xamarin.Mac アプリケーションで使用する任意のウィンドウのプロセスは基本的に何だけしたら上と同じ。

1. 新しいウィンドウをプロジェクトに自動的に追加の既定ではありませんが、プロジェクトに新しいウィンドウの定義を追加します。 これについては、以下で詳しく説明します。
2. ダブルクリックして、 `Main.storyboard` Xcode のインターフェイスのビルダーで編集するためのウィンドウのデザインを開くファイル。
3. ユーザー インターフェイスの設計に新しいウィンドウをドラッグし、ウィンドウのメイン ウィンドウを使用してフック_Segues_ (詳細については、次を参照してください、 [Segues](~/mac/platform/storyboards/indepth.md#Segues)のセクションで、 [のストーリーボードの使用。](~/mac/platform/storyboards/indepth.md)ドキュメント)。
3. 必要なウィンドウのプロパティを設定、**属性インスペクター**と**サイズ インスペクター**です。
4. インターフェイスを構築および構成に必要なコントロールでドラッグして、**属性インスペクター**です。
5. 使用して、**サイズ インスペクター** UI 要素のサイズ変更を処理します。
6. C# を使用してコード ウィンドウの UI 要素を公開**コンセント**と**アクション**です。
7. 変更を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

これでが作成された基本的なウィンドウ、見ていきます一般的なプロセス アプリケーションが windows を使用する場合に Xamarin.Mac します。 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>既定のウィンドウを表示します。

既定では、新しい Xamarin.Mac アプリケーションが自動的に表示ウィンドウで定義されている、`MainWindow.xib`は起動時にファイルします。

[![](window-images/display01.png "実行している例ウィンドウ")](window-images/display01.png#lightbox)

既定のツールバーは、そのウィンドウの上部のデザインを変更したため、**テキスト ビュー**コントロール。 次のセクションで、`Info.plist`ファイルは、このウィンドウを表示するため。

[![](window-images/display00.png "Info.plist の編集")](window-images/display00.png#lightbox)

**Main インターフェイス**ドロップダウンを使用して、メイン アプリケーション UI として使用されるストーリー ボードを選択 (ここでは`Main.storyboard`)。

ビュー コント ローラーは、(プライマリ ビュー) と共に表示されているメイン ウィンドウをプロジェクトに自動的に追加されます。 定義されている、`ViewController.cs`ファイルおよびにアタッチされている、**ファイルの所有者**インターフェイスのビルダーで、 **Identity インスペクター**:

[![](window-images/display02.png "ファイルの所有者の設定")](window-images/display02.png#lightbox)

ウィンドウのようにタイトルの`untitled`ときに最初に起動では上書き、`ViewWillAppear`メソッドで、`ViewController.cs`次のようになります。

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> ウィンドウの値を設定お`Title`プロパティに、`ViewWillAppear`メソッドの代わりに、`ViewDidLoad`メソッドのため、ビューは、メモリに読み込まれる可能性があります、中に、まだ完全にインスタンス化されていません。 アクセスしようとした場合、`Title`プロパティに、`ViewDidLoad`メソッドのようになります、`null`例外ウィンドウいないされて構築およびワイヤード (有線) アップ プロパティにまだためです。

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>プログラムでウィンドウを閉じる

プログラムによって Xamarin.Mac アプリケーションのウィンドウを閉じますか時間がある可能性があります、ユーザーこと以外には、ウィンドウのをクリックして**閉じる**ボタンまたはメニュー項目を使用します。 macOS を閉じるの 2 つの異なる方法を提供する、`NSWindow`プログラムから:`PerformClose`と`Close`です。

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

呼び出す、`PerformClose`のメソッド、`NSWindow`ウィンドウをクリックすると、ユーザーをシミュレート**閉じる**すぐに、ボタンを強調表示し、ウィンドウを閉じるボタンをクリックします。

アプリケーションを実装する場合、`NSWindow`の`WillClose`イベント、ウィンドウを閉じる前に生成されます。 イベントを返す場合`false`ウィンドウがない閉じられます。 ウィンドウがない場合、**閉じる**ボタンまたは閉じることができません何らかの理由で、OS は、アラートのサウンドを出力します。

例えば:

```csharp
MyWindow.PerformClose(this);
```

終了しようとして、 `MyWindow` `NSWindow`インスタンス。 成功した場合、ウィンドウは閉じられます、それ以外の場合警告音は出力されは開いた状態になります。

<a name="Close" />

### <a name="close"></a>閉じる

呼び出す、`Close`のメソッド、`NSWindow`ウィンドウをクリックすると、ユーザーをシミュレートできません**閉じる**ボタンすぐに、ボタンが強調表示され、それだけでウィンドウを閉じます。

ウィンドウが終了するために見られるする必要はありません、`NSWindowWillCloseNotification`通知は閉じられているウィンドウの既定の通知センターに書き込まれます。

`Close`メソッドから 2 つの重要な点が異なり、`PerformClose`メソッド。

1. させる試みません、`WillClose`イベント。
2. クリックすると、ユーザーはシミュレートしません、**閉じる**によってすぐに、ボタンを強調表示ボタンをクリックします。

例えば:

```csharp
MyWindow.Close();
```

閉じると、 `MyWindow` `NSWindow`インスタンス。

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>変更された Windows コンテンツ

MacOS などで Apple がユーザーに通知する方法を指定する、ウィンドウの内容 (`NSWindow`) ユーザーによって変更されており、保存する必要があります。 内の小さな黒いドットが表示されます、ウィンドウに、変更内容が含まれている場合**閉じる**ウィジェット。

[![](window-images/close01.png "変更されたマーカーを使用して、ウィンドウ")](window-images/close01.png#lightbox)

ユーザーがウィンドウを閉じるか中止を試みるとウィンドウのコンテンツが保存されているときに、Mac アプリに変更、提示する必要があります、 [ ダイアログ ボックス](~/mac/user-interface/dialog.md)または[モーダル シート](~/mac/user-interface/dialog.md)し、その変更を保存するアクセス許可まずは：

[![](window-images/close02.png "ウィンドウが閉じているときに表示されているシート保存")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>ウィンドウを変更済みとしてマークします。

コンテンツを変更することと、ウィンドウをマークするには、次のコードを使用します。

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

変更が保存されると、変更されたフラグを使用して、オフにします。

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>ウィンドウを閉じる前に変更を保存

ユーザーのウィンドウを閉じるように変更されたコンテンツを事前に保存するは、こちらからのサブクラスを作成する必要があります`NSWindowDelegate`オーバーライドとその`WindowShouldClose`メソッドです。 例えば:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class EditorWidowDelegate : NSWindowDelegate
    {
        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public EditorWidowDelegate (NSWindow window)
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

                // Take action based on resu;t
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

このデリゲートのインスタンスを現在のウィンドウにアタッチするのにには、次のコードを使用します。

```csharp
// Set delegate
Window.Delegate = new EditorWidowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>アプリを閉じる前に変更を保存

最後に、Xamarin.Mac アプリは、そのウィンドウのいずれかの変更されたコンテンツが含まれ、終了する前に、変更を保存するユーザーを許可するかどうかを確認する必要があります。 これを行うには、編集、`AppDelegate.cs`ファイルを上書き、`ApplicationShouldTerminate`メソッドされ次のようになります。

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

ほとんどのドキュメントのベース Mac アプリケーションは、同時に複数のドキュメントを編集できます。 たとえば、テキスト エディターでは、複数のテキスト ファイルを同時に編集用に開くを持つことができます。 既定では、新しい Xamarin.Mac アプリケーションが、**ファイル**メニューには、**新規**項目に自動的にワイヤード (有線) にする、 `newDocument:` **アクション**です。

この新しい項目をアクティブにし、一度に複数のドキュメントを編集するには、当社のメイン ウィンドウの複数のコピーを開くことを許可するでしょう。

編集しましょう、`AppDelegate.cs`ファイルし、次の計算プロパティを追加します。

```csharp
public int UntitledWindowCount { get; set;} =1;
```

このフィードバック (前述の Apple のガイドライン) ごとのユーザーに発行できるように保存されていないファイルの数を追跡するために使用されます。

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

このコード、ウィンドウのコント ローラーの新しいバージョンを作成、新しいウィンドウ、メイン レポートとキーのウィンドウは、読み込んでタイトルを設定します。 ここで、アプリケーションを実行すると、選択**新規**から、**ファイル**新しいエディター ウィンドウが開かれたし、表示されるメニュー。

[![](window-images/display04.png "新しい無題ウィンドウが追加されました")](window-images/display04.png#lightbox)

開くことがある場合、 **Windows** ] メニューの [このアプリケーションは自動的に追跡され、開いているウィンドウの処理を確認できます。

[![](window-images/display05.png "Widows メニュー")](window-images/display05.png#lightbox)

Xamarin.Mac アプリケーションでメニューと操作の詳細についてを参照してください、[メニューの作業](~/mac/user-interface/menu.md)ドキュメント。

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>現在アクティブなウィンドウを取得します。

複数のウィンドウ (ドキュメント) を開くことができる、Xamarin.Mac アプリケーションでは、必要がある生じる場合は、現在、最上位のウィンドウ (キーのウィンドウ) を取得するがあります。 次のコードには、キーのウィンドウが返されます。

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

あるは、任意のクラスまたは現在、キーのウィンドウにアクセスする必要があるメソッドで呼び出すことができます。 かどうかウィンドウが現在開いていないが返されます`null`です。

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>すべてのアプリ ウィンドウへのアクセス

あります Xamarin.Mac アプリが現在開いてでいる windows のすべてにアクセスする必要。 たとえば、ファイルを開くには、ユーザーが必要があるかどうかを既存のウィンドウで既に開かれているがします。

`NSApplication.SharedApplication`保持、`Windows`アプリで開いているすべての windows の配列を格納するプロパティです。 すべてのアプリの現在のウィンドウにアクセスするには、この配列を反復処理することができます。 例えば:

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

おが、カスタムに返される各ウィンドウのキャストのコード例に`ViewController`、アプリケーションの動作とカスタムの値のテスト クラス`Path`プロパティを開くには、ユーザーがファイルのパスを比較します。 ファイルを既に開いている場合は、そのウィンドウを前面に移動はおです。

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>コード ウィンドウのサイズを調整します。

アプリケーションがコード内のウィンドウのサイズを変更する必要がある場合もあります。 調整のサイズを変更し、ウィンドウの位置を変更の`Frame`プロパティです。 ウィンドウのサイズを調整するときに通常も macOS の座標システムのため、同じ場所にウィンドウを保持するの原点を調整する必要があります。

左上隅が (0, 0) を表す、iOS とは異なり、macOS でとして渡された座標系を使用して、画面の左下隅が (0, 0) を表します。 IOS では、座標は、下、右方向に移動するようを増やします。 MacOS などに、座標の下方右側にある値に増加します。 

次のコード例がウィンドウをサイズします。

```csharp
nfloat y = 0;

// Calculate new origin
y = Frame.Y - (768 - Frame.Height);

// Resize and position window
CGRect frame = new CGRect (Frame.X, y, 1024, 768);
SetFrame (frame, true);
```

> [!IMPORTANT]
> Windows のサイズとコード内の位置を調整するときに、インターフェイス ビルダーで設定した最小および最大サイズを考慮するかどうかを確認する必要があります。 これは自動的に無視され、大きい、またはこれらの制限よりも小さいウィンドウを作成することができます。

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>ウィンドウ サイズ変更の監視

あります Xamarin.Mac アプリ内でのウィンドウのサイズの変更を監視する必要。 たとえば、新しいサイズに合わせてコンテンツを再描画するには、です。

サイズ変更を監視するには、まず Xcode のインターフェイスのビルダーのウィンドウ コント ローラーのカスタム クラスを割り当てるようにことを確認します。 たとえば、`MasterWindowController`次に。

[![](window-images/resize01.png "Id 検査")](window-images/resize01.png#lightbox)

カスタムのウィンドウのコント ローラー クラスとモニターを次に、編集、`DidResize`ライブ サイズの変更の通知を受信するコント ローラーのウィンドウでイベントをします。 例えば:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

必要に応じて、使用することができます、`DidEndLiveResize`のみ通知を受けるユーザーが終わったら、ウィンドウのサイズを変更した後イベントです。 たとえば、次のように入力します。

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

## <a name="setting-a-windows-title-and-represented-file"></a>ウィンドウの設定のタイトルと表されるファイル

ドキュメントを表す windows を使用するときに`NSWindow`が、`DocumentEdited`プロパティに設定された場合`true`閉じる ボタンを目安にユーザー ファイルが変更されており、閉じる前に保存することで小規模のドットが表示されます。

編集しましょう、`ViewController.cs`ファイルし、次の変更します。

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

監視も、`WillClose`ウィンドウおよびチェックの状態のイベント、`DocumentEdited`プロパティです。 場合は`true`ファイルに変更を保存する機能をユーザーに付与する必要があります。 アプリケーションの実行、いくつかのテキストを入力すると、ドットが表示されます。

[![](window-images/file01.png "変更したウィンドウ")](window-images/file01.png#lightbox)

ウィンドウを閉じるしようと、アラートが表示されます。

[![](window-images/file02.png "保存を表示するダイアログ")](window-images/file02.png#lightbox)

場合ファイルからドキュメントを読み込んでお設定ウィンドウのタイトル、ファイルの名前を使用して、`window.SetTitleWithRepresentedFilename (Path.GetFileName(path));`メソッド (ある`path`開いているファイルを表す文字列です)。 使用して、ファイルの URL を設定してさらに、`window.RepresentedUrl = url;`メソッドです。

URL は、オペレーティング システムによって把握されているファイルの種類に向いている場合のアイコンがタイトル バーに表示されます。 ユーザーが右のアイコンをクリックすると、ファイルへのパスが表示されます。

編集しましょう、`AppDelegate.cs`ファイルし、次のメソッドを追加します。

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

場合は、アプリを実行してを選択するようになりました**オープンしています.**から、**ファイル**メニューの [テキスト ファイルから、**開く**] ダイアログ ボックスを開きます。

[![](window-images/file03.png "[開く] ダイアログ ボックス")](window-images/file03.png#lightbox)

ファイルが表示され、タイトルは、ファイルのアイコンが設定されます。

[![](window-images/file04.png "読み込まれたファイルの内容")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>新しいウィンドウをプロジェクトに追加します。

メイン ドキュメント ウィンドウとは別に Xamarin.Mac アプリケーションは、設定や検査パネルなどのユーザーにその他の種類のウィンドウを表示する必要があります。

新しいウィンドウを追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**をダブルクリックして、`Main.storyboard`編集のため Xcode のインターフェイスのビルダーで開くファイル。
2. 新しいドラッグ**ウィンドウ コント ローラー**から、**ライブラリ**上にドロップし、**デザイン サーフェイス**:

    [![](window-images/new01.png "ライブラリに新しいウィンドウのコント ローラーを選択します。")](window-images/new01.png#lightbox)
3. **Identity インスペクター**、入力`PreferencesWindow`の**ストーリー ボード ID**: 

    [![](window-images/new02.png "ストーリー ボード ID を設定")](window-images/new02.png#lightbox)
5. インターフェイスを設計するには。 

    [![](window-images/new03.png "UI の設計")](window-images/new03.png#lightbox)
6. アプリのメニューを開き (`MacWindows`) を選択**設定しています.**コントロールを右クリックして新しいウィンドウにドラッグします。 

    [![](window-images/new05.png "Segue を作成します。")](window-images/new05.png#lightbox)
7. 選択**表示**ポップアップ メニューからです。
6. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

コードを実行すると、選択、**設定しています.**から、**アプリケーション メニュー**ウィンドウが表示されます。

[![](window-images/new04.png "サンプルの基本設定 メニュー")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>パネルの使用

この記事の先頭に説明したように、パネルは他のウィンドウから浮いてし、ツールやドキュメントが開いている間のユーザーが使用できるようにコントロールを提供します。 

他の種類 ウィンドウを作成し、Xamarin.Mac アプリケーションで操作するのと同じように、プロセス、基本的に同じです。

1. 新しいウィンドウの定義をプロジェクトに追加します。
2. ダブルクリックして、 `.xib` Xcode のインターフェイスのビルダーで編集するためのウィンドウのデザインを開くファイル。
2. 必要なウィンドウのプロパティを設定、**属性インスペクター**と**サイズ インスペクター**です。
4. インターフェイスを構築および構成に必要なコントロールでドラッグして、**属性インスペクター**です。
5. 使用して、**サイズ インスペクター** UI 要素のサイズ変更を処理します。
6. C# を使用してコード ウィンドウの UI 要素を公開**コンセント**と**アクション**です。
7. 変更を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

**属性インスペクター**パネルに固有の次のオプションがあります。

[![](window-images/panel03.png "属性の検査")](window-images/panel03.png#lightbox)

- **スタイル**-からパネルのスタイルを調整することを許可する: 正規パネル (標準のウィンドウのように見える)、ユーティリティのパネル (より小さいタイトル バーを持つ) HUD パネル (半透明タイトル バーの一部となって、バック グラウンド)。
- **非アクティブ化中**-で決定パネル キー ウィンドウになります。
- **ドキュメントのモーダル**-場合ドキュメント モーダル、アプリケーションのウィンドウの上、パネルは float のみ、それ以外の場合、フローティング状態になったすべての上です。


新しいパネルを追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.**.
2. 新しいファイル] ダイアログ ボックスで、[ **Xamarin.Mac** > **コント ローラーと Cocoa ウィンドウ**:

    [![](window-images/panels00.png "新しいウィンドウ コント ローラーの追加")](window-images/panels00.png#lightbox)
3. **[名前]** に「`DocumentPanel`」と入力し、**[新規]** ボタンをクリックします。
4. ダブルクリックして、`DocumentPanel.xib`編集のためインターフェイス ビルダーで開くファイル。 

    [![](window-images/new02.png "Pannel の編集")](window-images/new02.png#lightbox)
5. 既存のウィンドウを削除してからパネルをドラッグして、**ライブラリ インスペクター**では、**インターフェイス エディター**: 

    [![](window-images/panels01.png "既存のウィンドウを削除します。")](window-images/panels01.png#lightbox)
6. パネルにフックするため、**ファイルの所有者*-**ウィンドウ*- **コンセント**: 

    [![](window-images/panels02.png "パネルをネットワーク上にドラッグします。")](window-images/panels02.png#lightbox)
7. 切り替えて、 **Identity インスペクター**パネルのクラスを設定および`DocumentPanel`: 

    [![](window-images/panels03.png "設定すると、パネルのクラス")](window-images/panels03.png#lightbox)
6. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。
7. 編集、`DocumentPanel.cs`ファイルを開き、次に、クラス定義を変更します。 

    `public partial class DocumentPanel : NSPanel`
8. 変更内容をファイルに保存します。

編集、`AppDelegate.cs`ファイルし、`DidFinishLaunching`次のようなメソッドの検索。

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

アプリケーションを実行する場合、パネルが表示されます。

[![](window-images/panels04.png "実行中のアプリ内のパネル")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> パネルの Windows は、Apple が廃止されに置き換える必要が**インスペクター インターフェイス**です。 完全な例を作成する、**インスペクター** Xamarin.Mac アプリで、次を参照してください、 [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/)サンプル アプリケーションです。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Windows とパネル Xamarin.Mac アプリケーションでの操作についての詳細を取得しました。 おにより、さまざまな種類を説明し、Windows とパネル、Windows とパネル Xcode のインターフェイスのビルダーで作成および管理する方法、および c# コードで Windows とパネルを使用する方法の使用します。

## <a name="related-links"></a>関連リンク

- [MacWindows (サンプル)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (サンプル)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [メニューの作業](~/mac/user-interface/menu.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
