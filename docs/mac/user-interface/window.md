---
title: Xamarin.Mac での Windows
description: この記事では、ウィンドウとパネルは、Xamarin.Mac アプリケーションでの操作について説明します。 作成のウィンドウとパネルで、Xcode と Interface Builder、ストーリー ボードと .xib ファイルから読み込むと、それらをプログラムで操作がについて説明します。
ms.prod: xamarin
ms.assetid: 4F6C67E9-BBFF-44F7-B29E-AB47D7F44287
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: ec907e71074a97bd5d1714e79dd504013f5c8a4b
ms.sourcegitcommit: 7eed80186e23e6aff3ddbbf7ce5cd1fa20af1365
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/11/2018
ms.locfileid: "51526976"
---
# <a name="windows-in-xamarinmac"></a>Xamarin.Mac での Windows

_この記事では、ウィンドウとパネルは、Xamarin.Mac アプリケーションでの操作について説明します。作成のウィンドウとパネルで、Xcode と Interface Builder、ストーリー ボードと .xib ファイルから読み込むと、それらをプログラムで操作がについて説明します。_

使用する場合C#および .NET、Xamarin.Mac アプリケーションで同じ Windows へのアクセスし、で作業する開発者のパネルを*Objective C*と*Xcode*は。 Xamarin.Mac は直接 Xcode と統合、ためには、Xcode を使用して_Interface Builder_を作成し、Windows とパネルを維持 (またはに応じて c# コードで直接作成する)。

その目的に基づき、Xamarin.Mac アプリケーションは、管理および情報を表示し、連携を調整する画面に 1 つまたは複数の Windows に表示できます。 ウィンドウのプリンシパルの関数は次のとおりです。

1. ビューとコントロール内の領域を提供するには、配置および管理できます。
2. そのまま使用し、キーボードとマウスの両方でユーザーとの対話に応答でイベントに応答します。

Windows は、(エクスポート ダイアログ アプリケーションを続行する前に閉じる必要がありますが) などのモーダルまたはモードレスの状態 (など複数のドキュメントを一度に開くことができますが、テキスト エディター) で使用できます。

パネルは、ウィンドウの特殊な (ベースのサブクラス`NSWindow`クラス)、通常、テキスト形式のインスペクターとシステム カラー ピッカーのようなユーティリティ ウィンドウなどのアプリケーションで、補助的な関数を処理します。

[![](window-images/intro01.png "Xcode で、ウィンドウの編集")](window-images/intro01.png#lightbox)

この記事では、Windows とパネル、Xamarin.Mac アプリケーションで操作の基本を説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

見てしたい場合があります、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)、について説明します、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素を c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Windows" />

## <a name="introduction-to-windows"></a>Windows の概要

前述のように、ウィンドウをビューとコントロールを配置および管理できる領域を提供し、(、キーボードまたはマウスを使用して) ユーザーの操作に基づいてイベントに応答します。

に従って、Apple は、macos アプリを Windows の 5 つの主な種類があります。

- **ドキュメント ウィンドウ**-ドキュメント ウィンドウには、スプレッドシートやテキスト ドキュメントなどのファイル ベースのユーザー データが含まれています。
- **アプリのウィンドウ**-アプリのウィンドウは (Mac で予定表アプリ) のようなドキュメント ベースでないアプリケーションのメイン ウィンドウ。
- **パネル**ドキュメントは、中にユーザーを使用するコントロールを開く - または、パネルが他のウィンドウから浮いてし、ツールを提供します。 場合によっては、パネルを半透明にすることができます (場合などの大規模なグラフィックス操作)。
- **ダイアログ**-ダイアログは、ユーザー アクションへの応答に表示され、通常の方法のユーザーは、操作を完了できますを提供します。 ダイアログ ボックスを閉じる前に、ユーザーからの応答が必要です。 (を参照してください[ダイアログの操作](~/mac/user-interface/dialog.md))
- **アラート**-アラートは特殊な種類 (エラー) など、深刻な問題が発生したときに表示されるダイアログ ボックスのまたは (ファイルを削除する準備をしています) などの警告として。 アラートは、ダイアログ ボックスであるためも必要です、ユーザーの応答を閉じる前にします。 (を参照してください[アラートを操作する](~/mac/user-interface/alert.md))

詳細については、次を参照してください、[について Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Main_Key_and_Inactive_Windows" />

### <a name="main-key-and-inactive-windows"></a>Main、キー、および非アクティブな Windows

Xamarin.Mac アプリケーションでの Windows では、検索でき、ユーザーが現在それらと対話する方法に応じて異なる動作することができます。 最も重要なドキュメントまたはアプリのウィンドウは、現在のユーザーの注意のフォーカスが呼び出される、_メイン ウィンドウ_します。 ほとんどの状況でこのウィンドウをすることはまた、_キー ウィンドウ_(ユーザー入力を募集中のウィンドウ)。 常には発生しません、たとえば、カラー ピッカーでした開けませんキー ウィンドウ (これが、メイン ウィンドウにある) ドキュメント ウィンドウ内の項目の状態を変更するユーザーが対話します。

メイン レポートとキーの Windows (独立した場合) は、アクティブな場合は、常に_非アクティブな Windows_はフォア グラウンドではありません。 開いているウィンドウ。 たとえば、テキスト エディター アプリケーションでは、複数のドキュメントがある、一度に開くだけで、メイン ウィンドウがアクティブになります、他のすべてのユーザーがアクティブになりません。 

詳細については、次を参照してください、[について Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowAppearanceBehavior.html#//apple_ref/doc/uid/20000957-CH33-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Naming_Windows" />

### <a name="naming-windows"></a>Windows の名前を付ける

ウィンドウがタイトル バーを表示し、ことは通常、アプリケーションの名前、作業中のドキュメントの名前または関数 (インスペクター) など、ウィンドウのタイトルが表示されたら。 一部のアプリケーションは、視認で認識されていて、ドキュメントでは動作しないために、タイトル バーを表示しません。

Apple では、次のガイドラインをお勧めします。

- メインのドキュメント以外のウィンドウのタイトルのアプリケーション名を使用します。 
- ドキュメント ウィンドウを新しい名前を付けて`untitled`します。 最初の新しいドキュメントにない番号を追加、タイトルに (など`untitled 1`)。 ユーザーは、保存および 1 つ目のタイトルを作成する前に別の新しいドキュメントを作成する場合は、そのウィンドウを呼び出す`untitled 2`、`untitled 3`など。

詳細については、次を参照してください、[名前付けの Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowNaming.html#//apple_ref/doc/uid/20000957-CH35-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Full-Screen_Windows" />

### <a name="full-screen-windows"></a>Windows の全画面表示

MacOS でのアプリケーション ウィンドウ進んで全画面表示の非表示 (これは、画面の上部にカーソルを移動することによって明らかになる) アプリケーションのメニュー バーを含め、すべてとのやり取りを無料のコンテンツが邪魔を提供します。

Apple は、次のガイドラインを示しています。

- 全画面表示を移動するウィンドウの理にかなっているかどうかを決定します。 (電卓) などの簡単な相互作用を提供するアプリケーションでは、全画面表示モードを指定しないでください。
- 全画面表示タスクに必要なかどうかは、ツールバーを表示します。 通常、ツールバーは全画面表示モードでは表示されません。
- 全画面表示のウィンドウには、タスクを完了する必要があるすべての機能のユーザーが必要です。
- ユーザーが全画面表示 ウィンドウでは、ファインダーの相互作用が可能であれば、しないでください。
- 主なタスクからフォーカスを移動せず、増加の画面スペースを利用します。

詳細については、次を参照してください、[全画面表示 Windows](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/FullScreen.html#//apple_ref/doc/uid/20000957-CH61-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Panels" />

### <a name="panels"></a>パネル

パネルは、コントロールとアクティブなドキュメントや、システム カラー ピッカー) などの選択に影響するオプションを含む補助のウィンドウには。

[![](window-images/panel01.png "カラー パネル")](window-images/panel01.png#lightbox)

パネルには、いずれかを指定できる_アプリ固有_または_システム全体_します。 アプリ固有のパネルでは、アプリケーションのドキュメント ウィンドウの上に float し、アプリケーションがバック グラウンドでは、表示されなくなります。 システム全体のパネル (など、**フォント**パネル)、アプリケーションに関係なくすべての開いているウィンドウの上に浮動小数点数。 

Apple は、次のガイドラインを示しています。

- 一般に、標準のパネルを使用して、透過的なパネルは、慎重に行いの視覚的に処理を要するタスクのみ使用できます。
- 重要なコントロールまたはそのタスクに直接影響する情報に簡単にアクセスをユーザーに付与するパネルを使用してください。
- 非表示にし、必要に応じてパネルを表示します。
- パネルは、タイトル バーを常に含める必要があります。
- パネルは、アクティブな最小化ボタンを含めないでください。

#### <a name="inspectors"></a>インスペクター

ほとんどの最新の macOS アプリケーション補助的なコントロールとアクティブなドキュメントや何も選択に影響するオプションを提示_インスペクター_はメイン ウィンドウの一部である (など、**ページ**次に示すアプリ)、代わりに、パネルの Windows を使用します。

[![](window-images/panel02.png "例のインスペクター")](window-images/panel02.png#lightbox)

詳細については、次を参照してください、[パネル](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowPanels.html#//apple_ref/doc/uid/20000957-CH42-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)と[MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/) 、の完全な実装のサンプルアプリ **。インスペクター インターフェイス**Xamarin.Mac アプリでします。

<a name="Creating_and_Maintaining_Windows_in_Xcode" />

## <a name="creating-and-maintaining-windows-in-xcode"></a>作成して、Xcode での Windows の保守

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、`.storyboard`プロジェクトに自動的に含まれるファイル。 Windows のデザインを編集する、**ソリューション エクスプ ローラー**、ダブルクリック、`Main.storyboard`ファイル。

[![](window-images/edit01.png "メインのストーリー ボードを選択します。")](window-images/edit01.png#lightbox)

これが、Xcode の Interface Builder でウィンドウのデザインを開きます。

[![](window-images/edit02.png "Xcode では、UI の編集")](window-images/edit02.png#lightbox)

**属性インスペクター**を定義し、ウィンドウの制御に使用できるいくつかのプロパティします。

- **タイトル**-これは、ウィンドウのタイトル バーに表示されるテキスト。
- **自動保存**-これは、_キー_の位置と設定が自動的に保存されるときに、ウィンドウを ID に使用されます。
- **タイトル バー** -ウィンドウは、タイトル バーを表示します。
- **統合されたタイトルとツールバー** - 場合は、ウィンドウには、ツールバー、タイトル バーの一部にする必要があります。
- **コンテンツ ビューのサイズを完全な**-タイトル バーの下にあるウィンドウのコンテンツ エリアを使用します。
- **シャドウ**-は、ウィンドウがシャドウします。
- **テクスチャ**-テクスチャ windows (活気) などの効果を使用することができ、本体の任意の場所にドラッグして移動できます。
- **閉じる**-は、ウィンドウが閉じるボタンをクリックします。
- **最小限に抑える**-は、ウィンドウが最小化ボタン。
- **サイズ変更**-は、ウィンドウがサイズ変更コントロール。
- **ツール バー ボタン**-は、ウィンドウが非表示/表示ツールバー ボタンをクリックします。
- **復元可能な**-がウィンドウの位置で自動的に保存および復元の設定。
- **起動に表示される**-ウィンドウに自動的に時に表示される、`.xib`ファイルが読み込まれます。
- **非アクティブ化で非表示に**-は、ウィンドウを非表示、アプリケーションがバック グラウンドに入ったとき。
- **閉じられたときにリリース**-は、ウィンドウが閉じているときに、メモリから消去します。
- **ツールヒントを常に表示**-は常に表示されるツールヒント。
- **ビューのループを再計算**-は、ウィンドウが描画される前に再計算された注文を表示します。
- **スペース**、 **expos ・** と**Cycling** -macOS 環境で、ウィンドウの動作方法すべてを定義します。 
- **完全な画面**のかどうか、このウィンドウは、全画面表示モードを入力できますを決定します。 
- **アニメーション**-ウィンドウの使用可能なアニメーションの種類を制御します。
- **外観**-ウィンドウの外観を制御します。 現時点では水色、1 つだけの外観です。

Apple を参照してください。 [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)と[NSWindow](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSWindow_Class/index.html#//apple_ref/occ/cl/NSWindow)詳細についてはドキュメントです。

<a name="Setting_the_Default_Size_and_Location" />

### <a name="setting-the-default-size-and-location"></a>既定のサイズと場所の設定

ウィンドウの初期位置を設定して、そのサイズを制御するスイッチを**サイズ インスペクター**:

[![](window-images/edit07.png "既定のサイズと場所")](window-images/edit07.png#lightbox)

ここから、ウィンドウの初期サイズを設定、最小値と最大サイズを指定、画面の最初の場所を設定してコントロールのウィンドウの周囲の境界線。

<a name="Setting-a-Custom-Main-Window-Controller" />

### <a name="setting-a-custom-main-window-controller"></a>カスタムのメイン ウィンドウのコント ローラーを設定

C# コードに UI 要素を公開する Outlet と Action を作成できるようにするには、Xamarin.Mac アプリは、カスタム ウィンドウ コント ローラーを使用する必要があります。

次の手順で行います。

1. Xcode の Interface Builder では、アプリのストーリー ボードを開きます。
2. 選択、`NSWindowController`デザイン画面でします。
3. 切り替えて、 **Identity Inspector**を表示および入力`WindowController`として、**クラス名**: 

    [![](window-images/windowcontroller01.png "クラス名を設定します。")](window-images/windowcontroller01.png#lightbox)
4. 変更を保存し、Visual Studio for Mac を同期に戻ります。
5. A`WindowController.cs`ファイルは、プロジェクトに追加する、**ソリューション エクスプ ローラー** Visual Studio for Mac で。 

    [![](window-images/windowcontroller02.png "Windows のコント ローラーを選択します。")](window-images/windowcontroller02.png#lightbox)
6. Xcode の Interface Builder のストーリー ボードを再度開きます。
7. `WindowController.h`ファイルが使用できるようにします。 

    [![](window-images/windowcontroller03.png "WindowController.h ファイルの編集")](window-images/windowcontroller03.png#lightbox)

<a name="Adding_UI_Elements" />

### <a name="adding-ui-elements"></a>UI 要素を追加します。

ウィンドウのコンテンツを定義するからコントロールをドラッグ、**ライブラリ インスペクター**上に、**インターフェイス エディター**します。 参照してください、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)Interface Builder を使用して作成し、コントロールを有効にする方法の詳細についてはドキュメントです。

たとえば、ドラッグ、ツールバー、**ライブラリ インスペクター**でウィンドウの上に、**インターフェイス エディター**:

[![](window-images/edit03.png "ライブラリからツールバーを選択します。")](window-images/edit03.png#lightbox)

次に、ドラッグ、**テキスト ビュー**し、ツールバーの下の領域をいっぱいになるようにサイズします。

[![](window-images/edit04.png "テキスト ビューを追加します。")](window-images/edit04.png#lightbox)

たいので、**テキスト ビュー**ウィンドウのサイズの変化に応じて拡大および縮小に切り替えてみましょう、**制約エディター**し、次の制約を追加します。

[![](window-images/edit05.png "制約の編集")](window-images/edit05.png#lightbox)

クリックしての**赤い I ビーム**エディターの上部とクリック**4 制約の追加**座標を指定した X、Y を引き続き使用し、拡大または縮小水平方向および垂直方向にするには、テキスト ビューを示してとしてウィンドウをサイズします。

最後に、公開しましょう、**テキスト ビュー**を使用してコードを**アウトレット**(選択することを確認、`ViewController.h`ファイル)。

[![](window-images/edit06.png "アウトレットを構成します。")](window-images/edit06.png#lightbox)

変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

操作の詳細については**Outlet**と**アクション**を参照してください、[コンセントとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)ドキュメント。

<a name="Standard_Window_Workflow" />

### <a name="standard-window-workflow"></a>標準的なウィンドウのワークフロー

作成し、Xamarin.Mac アプリケーションで使用されるウィンドウで、プロセスは基本的にだけ作業上と同じ。

1. プロジェクトに自動的に追加の既定ではない新しい windows の場合は、プロジェクトに新しいウィンドウの定義を追加します。 これについては、以下で詳しく説明します。
2. ダブルクリックして、 `Main.storyboard` Xcode の Interface Builder で編集するためのウィンドウのデザインを開くファイル。
3. ユーザー インターフェイスの設計に新しいウィンドウをドラッグし、ウィンドウのメイン ウィンドウを使用してフック_Segues_ (詳細については、次を参照してください、 [Segues](~/mac/platform/storyboards/indepth.md#Segues)のセクション、 [ストーリーボードの使用。](~/mac/platform/storyboards/indepth.md)ドキュメント)。
3. ウィンドウの必要なプロパティを設定、**属性インスペクター**と**サイズ インスペクター**します。
4. インターフェイスを構築および構成に必要なコントロールでドラッグして、**属性インスペクター**します。
5. 使用して、**サイズ インスペクター** UI 要素のサイズ変更を処理します。
6. C# を使用してコードに、ウィンドウの UI 要素を公開**Outlet**と**アクション**します。
7. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

作成された基本的なウィンドウが作成できた見て一般的なプロセス、Xamarin.Mac アプリケーションで windows を使用する場合。 

<a name="Displaying_the_Default_Window" />

## <a name="displaying-the-default-window"></a>既定のウィンドウを表示します。

既定では、新しい Xamarin.Mac アプリケーションは自動的にウィンドウを表示で定義されている、`MainWindow.xib`ファイルが開始されている場合に。

[![](window-images/display01.png "実行しているウィンドウの例")](window-images/display01.png#lightbox)

既定のツールバーも含まれていますのででそのウィンドウの上部のデザインを変更し、**テキスト ビュー**コントロール。 次のセクションで、`Info.plist`ファイルがこのウィンドウを表示する責任を負います。

[![](window-images/display00.png "Info.plist を編集")](window-images/display00.png#lightbox)

**メイン インターフェイス**ドロップダウンを使用して、メイン アプリケーション UI として使用されるストーリー ボードを選択 (ここで`Main.storyboard`)。

ビュー コント ローラーは、(そのプライマリ ビュー) と共に表示されるそのメインの Windows を制御するためのプロジェクトに自動的に追加されます。 定義されている、`ViewController.cs`にアタッチされているファイルを開き、**ファイルの所有者**Interface Builder の下で、 **Identity Inspector**:

[![](window-images/display02.png "ファイルの所有者の設定")](window-images/display02.png#lightbox)

ウィンドウでは、今回は、タイトルが必要であること`untitled`ときに最初に起動しましょうオーバーライド、`ViewWillAppear`メソッドで、`ViewController.cs`に、次のようになります。

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set Window Title
    this.View.Window.Title = "untitled";
}
``` 

> [!NOTE]
> ウィンドウの値を設定しています`Title`プロパティ、`ViewWillAppear`メソッドの代わりに、`ViewDidLoad`メソッド ビューは、メモリに読み込まれることが、中に、まだ完全にインスタンス化されないためです。 アクセスしようとすると、`Title`プロパティ、`ViewDidLoad`メソッドは、`null`ウィンドウいないされて構築およびワイヤード (有線) アップ プロパティにまだため例外。

<a name="Programmatically_Closing_a_Window" />

## <a name="programmatically-closing-a-window"></a>プログラムでウィンドウを閉じる

プログラムで、Xamarin.Mac アプリケーションでウィンドウを閉じますか時間がある可能性があります、ユーザーを持つこと以外には、ウィンドウのをクリックして**閉じます**ボタンまたはメニュー項目を使用します。 macOS が終了する 2 つのさまざまな方法を提供します、`NSWindow`プログラム:`PerformClose`と`Close`します。

<a name="PerformClose" />

### <a name="performclose"></a>PerformClose

呼び出す、`PerformClose`のメソッド、`NSWindow`クリックすると、ウィンドウのユーザーをシミュレート**閉じる**を一時的に、ボタンを強調表示し、ウィンドウを閉じるボタンをクリックします。

アプリケーションが実装されている場合、`NSWindow`の`WillClose`イベントがウィンドウを閉じる前に生成されます。 イベントが返された場合`false`、ウィンドウが閉じられていません。 ウィンドウがない場合、**閉じる**ボタンまたは閉じることができません、OS、何らかの理由で警告音が生成されます。

例えば:

```csharp
MyWindow.PerformClose(this);
```

終了しようとして、 `MyWindow` `NSWindow`インスタンス。 ウィンドウは閉じられますが成功した場合は、それ以外の場合、警告音が出力されるは開いたままにします。

<a name="Close" />

### <a name="close"></a>閉じる

呼び出す、`Close`のメソッド、`NSWindow`ユーザーがウィンドウのクリックをシミュレートしない**閉じる**ボタン ウィンドウを閉じるだけを一時的に、ボタンを強調表示、します。

Closed に表示されるウィンドウがないと`NSWindowWillCloseNotification`通知は閉じられているウィンドウの既定の通知センターに投稿されます。

`Close`メソッドが 2 つの重要な点で異なります、`PerformClose`メソッド。

1. 発生させるのには再試行されません、`WillClose`イベント。
2. ユーザーのクリックをシミュレートしません、**閉じる**を一時的に、ボタンを強調表示ボタンをクリックします。

例えば:

```csharp
MyWindow.Close();
```

閉じるには、 `MyWindow` `NSWindow`インスタンス。

<a name="Modified-Windows-Content" />

## <a name="modified-windows-content"></a>変更された Windows コンテンツ

MacOS、Apple は、ユーザーに通知する方法を提供するウィンドウの内容 (`NSWindow`)、ユーザーが変更されており、保存する必要があります。 内の小さな黒い点が表示されますが、ウィンドウに変更されたコンテンツが含まれている場合**閉じる**ウィジェット。

[![](window-images/close01.png "変更されたマーカーを使用して、ウィンドウ")](window-images/close01.png#lightbox)

ウィンドウのコンテンツ変更が保存されているときに、Mac アプリ、ユーザーが、ウィンドウを閉じる、または終了を試みる場合は、提示する必要があります、 [ ダイアログ ボックス](~/mac/user-interface/dialog.md)または[モーダル シート](~/mac/user-interface/dialog.md)し、その変更を保存するユーザーを許可します。まずは：

[![](window-images/close02.png "ウィンドウを閉じるときに表示されているシートの保存")](window-images/close02.png#lightbox)

### <a name="marking-a-window-as-modified"></a>変更されたウィンドウをマークします。

ウィンドウのコンテンツを変更することをマークするには、次のコードを使用します。

```csharp
// Mark Window content as modified
Window.DocumentEdited = true;
```

変更が保存されると、オフに変更フラグを使用して。

```csharp
// Mark Window content as not modified
Window.DocumentEdited = false;
```

### <a name="saving-changes-before-closing-a-window"></a>ウィンドウを閉じる前に変更を保存しています

変更されたコンテンツを事前に保存するユーザー、ウィンドウを閉じるとできるをウォッチするのサブクラスを作成する必要があります`NSWindowDelegate`オーバーライドとその`WindowShouldClose`メソッド。 例えば:

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

ウィンドウにこのデリゲートのインスタンスをアタッチするのにには、次のコードを使用します。

```csharp
// Set delegate
Window.Delegate = new EditorWindowDelegate(Window);
```

### <a name="saving-changes-before-closing-the-app"></a>アプリを閉じる前に変更を保存しています

最後に、Xamarin.Mac アプリは、Windows のいずれかの変更されたコンテンツが含まれ、終了する前に変更を保存するユーザーを許可するかどうかを確認する必要があります。 これを行うには、編集、`AppDelegate.cs`ファイルで上書き、`ApplicationShouldTerminate`メソッドと、次のようになります。

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

## <a name="working-with-multiple-windows"></a>複数の Windows の操作

ドキュメント ベースの Mac アプリケーションのほとんどは、同時に複数のドキュメントを編集できます。 などのテキスト エディターでは、複数のテキスト ファイルを同時に編集のため開くことができます。 既定で新しい Xamarin.Mac アプリケーションは、**ファイル**メニューで、**新規**項目に自動的にワイヤード (有線) にする、 `newDocument:` **アクション**します。

この新しい項目をアクティブにし、一度に複数のドキュメントを編集するには、メイン ウィンドウの複数のコピーを開くことを許可するでしょう。

編集、`AppDelegate.cs`ファイルを開き、次の計算されたプロパティを追加します。

```csharp
public int UntitledWindowCount { get; set;} =1;
```

これ (前述の Apple のガイドライン) あたりのユーザーへのフィードバックを提供できるように、保存されていないファイルの数を追跡するために使用します。

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

このコードで、ウィンドウ コント ローラーの新しいバージョンを作成、新しいウィンドウを読み込みます、メイン レポートとキー ウィンドウになります、およびタイトルを設定します。 ここで、アプリケーションの実行を選択して**新規**から、**ファイル**新しいエディター ウィンドウを開くし、表示されますが、メニュー。

[![](window-images/display04.png "新しい無題ウィンドウが追加されました")](window-images/display04.png#lightbox)

開く場合、 **Windows** ] メニューの [アプリケーションが自動的に追跡され、開いているウィンドウの処理を確認できます。

[![](window-images/display05.png "Windows メニュー")](window-images/display05.png#lightbox)

メニュー、Xamarin.Mac アプリケーションでの操作方法の詳細についてを参照してください、[メニューの作業](~/mac/user-interface/menu.md)ドキュメント。

<a name="Getting_the_Currently_Active_Window" />

### <a name="getting-the-currently-active-window"></a>現在アクティブなウィンドウを取得します。

複数のウィンドウ (ドキュメント) を開くことができます、Xamarin.Mac アプリケーションでは、現在、最上位のウィンドウ (キーのウィンドウ) を取得する必要がある生じるがあります。 次のコードでは、キーのウィンドウを返します。

```csharp
var window = NSApplication.SharedApplication.KeyWindow;
```

任意のクラスまたは現在、キーのウィンドウにアクセスする必要があるメソッドで呼び出すことができます。 返されますかどうかウィンドウが現在開いていない、`null`します。

<a name="Accessing-All-App-Windows" />

### <a name="accessing-all-app-windows"></a>すべてのアプリの Windows へのアクセス

あります、Xamarin.Mac アプリが現在オープンである windows のすべてにアクセスする必要。 たとえば場合、ユーザーを開く必要があるファイルを表示する、既存のウィンドウで開いているです。

`NSApplication.SharedApplication`保持、`Windows`アプリで開いているすべての windows の配列を格納するプロパティ。 アプリの現在の windows のすべてにアクセスするには、この配列を反復処理することができます。 例えば:

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

コード例では、カスタムの返された各ウィンドウをキャストする`ViewController`アプリおよびカスタムの値のテスト クラス`Path`プロパティに対してユーザーが開くファイルのパス。 ファイルが既に開いている場合は、そのウィンドウを前面に移動がいます。

<a name="Adjusting_the_Window_Size_in_Code" />

## <a name="adjusting-the-window-size-in-code"></a>コード ウィンドウのサイズを調整します。

アプリケーションがコード内のウィンドウのサイズを変更する必要がある場合もあります。 調整のサイズを変更し、ウィンドウを再配置の`Frame`プロパティ。 ウィンドウのサイズを調整する場合は、通常、macOS の座標システムのため、同じ場所にウィンドウを保持する、it の原点を調整することもする必要があります。

ここで左上隅を表します (0, 0)、iOS とは異なり、macOS では用いた数値座標系を使用して、画面の左下隅が (0, 0) を表します。 IOS では、座標は右に向かって下方向に移動するを増やします。 MacOS で、座標は、右上に値で増加します。 

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
> Windows のサイズとコード内の位置を調整するときは、Interface Builder で設定されている最小および最大サイズを考慮するかどうかを確認する必要があります。 これは自動的に無視されすると、ウィンドウを大きくまたはこれらの制限よりも小さいことができます。

<a name="Monitoring-Window-Size-Changes" />

## <a name="monitoring-window-size-changes"></a>ウィンドウのサイズ変更の監視

あります、Xamarin.Mac アプリの内部でウィンドウのサイズの変更を監視する必要。 たとえば、新しいサイズに合わせてコンテンツを再描画するには、です。

サイズ変更を監視するには、まず Xcode の Interface Builder でウィンドウ コント ローラーのカスタム クラスが割り当てられていることを確認します。 たとえば、`MasterWindowController`次で。

[![](window-images/resize01.png "Id インスペクター")](window-images/resize01.png#lightbox)

カスタム ウィンドウ コント ローラー クラスとモニターを次に、編集、`DidResize`ライブのサイズ変更の通知を受け取る、コント ローラーのウィンドウでイベント。 例えば:

```csharp
public override void WindowDidLoad ()
{
    base.WindowDidLoad ();

    Window.DidResize += (sender, e) => {
        // Do something as the window is being live resized
    };
}
```

必要に応じて、使用、`DidEndLiveResize`イベントをウィンドウのサイズを変更するユーザーが終了した後に、だけ通知されます。 たとえば、次のように入力します。

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

## <a name="setting-a-windows-title-and-represented-file"></a>タイトルのウィンドウを設定し、ファイルを表す

ドキュメントを表す windows を使用する場合`NSWindow`が、`DocumentEdited`プロパティに設定された場合`true`ファイルが変更されており、閉じる前に保存する必要がありますを示す値を付与するために閉じるボタンに小さなドットが表示されます。

編集、`ViewController.cs`ファイルを開き、次に変更します。

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

また監視、`WillClose`ウィンドウおよびチェックの状態のイベント、`DocumentEdited`プロパティ。 場合は`true`ファイルに変更を保存する権限をユーザーに付与する必要があります。 場合は、アプリを実行するいくつかのテキストを入力して、ドットが表示されます。

[![](window-images/file01.png "変更されたウィンドウ")](window-images/file01.png#lightbox)

ウィンドウを閉じるしようとすると、アラートが表示されます。

[![](window-images/file02.png "保存を表示するダイアログ")](window-images/file02.png#lightbox)

ファイルのウィンドウのタイトルを設定しますできる場合は、ドキュメントをファイルから読み込みです名前を使用して、`window.SetTitleWithRepresentedFilename (Path.GetFileName(path));`メソッド (いる`path`は開いているファイルを表す文字列です)。 さらに、私たちを使用して、ファイルの URL を設定、`window.RepresentedUrl = url;`メソッド。

URL は、OS によって認識されているファイルの種類に向いている場合、タイトル バーのアイコンが表示されます。 ユーザーが右のアイコンをクリックすると、ファイルへのパスが表示されます。

編集、`AppDelegate.cs`ファイルを開き、次のメソッドを追加します。

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

アプリを実行した場合を選択するようになりました**オープンしています.** から、**ファイル**メニューで、テキスト ファイルから、[、**開く**] ダイアログ ボックスを開きます。

[![](window-images/file03.png "開く ダイアログ ボックス")](window-images/file03.png#lightbox)

ファイルが表示され、ファイルのアイコン、タイトルに設定されます。

[![](window-images/file04.png "読み込まれたファイルの内容")](window-images/file04.png#lightbox)

<a name="Adding_a_New_Window_to_a_Project" />

## <a name="adding-a-new-window-to-a-project"></a>新しいウィンドウをプロジェクトに追加します。

メイン ドキュメント ウィンドウとは別に、Xamarin.Mac アプリケーションは、基本設定や検査パネルなど、ユーザーに他の種類のウィンドウを表示する必要があります。

新しいウィンドウを追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイルを開き、Xcode の Interface Builder で編集します。
2. 新しいドラッグ**ウィンドウ コント ローラー**から、**ライブラリ**上にドロップし、**デザイン サーフェイス**:

    [![](window-images/new01.png "ライブラリの新しいウィンドウ コント ローラーを選択します。")](window-images/new01.png#lightbox)
3. **Identity Inspector**、入力`PreferencesWindow`の**ストーリー ボード ID**: 

    [![](window-images/new02.png "ストーリー ボード ID の設定")](window-images/new02.png#lightbox)
5. インターフェイスを設計するには。 

    [![](window-images/new03.png "UI の設計")](window-images/new03.png#lightbox)
6. アプリのメニューを開きます (`MacWindows`) を選択します**設定しています.** コントロールを右クリックして新しいウィンドウにドラッグします。 

    [![](window-images/new05.png "セグエを作成します。")](window-images/new05.png#lightbox)
7. 選択**表示**ポップアップ メニュー。
6. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

コードを実行すると選択、**設定しています.** から、**アプリケーション メニュー**ウィンドウが表示されます。

[![](window-images/new04.png "サンプルの基本設定 メニュー")](window-images/new04.png#lightbox)

<a name="Working_with_Panels" />

## <a name="working-with-panels"></a>パネルの操作

この記事の冒頭で述べたように、パネルが他のウィンドウから浮いてし、ツールやドキュメントが開いている間にユーザーが使用できるコントロールを提供します。 

他の種類 ウィンドウを作成し、Xamarin.Mac アプリケーションで使用するのと同様、プロセス、基本的に同じです。

1. 新しいウィンドウの定義をプロジェクトに追加します。
2. ダブルクリックして、 `.xib` Xcode の Interface Builder で編集するためのウィンドウのデザインを開くファイル。
2. ウィンドウの必要なプロパティを設定、**属性インスペクター**と**サイズ インスペクター**します。
4. インターフェイスを構築および構成に必要なコントロールでドラッグして、**属性インスペクター**します。
5. 使用して、**サイズ インスペクター** UI 要素のサイズ変更を処理します。
6. C# を使用してコードに、ウィンドウの UI 要素を公開**Outlet**と**アクション**します。
7. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

**属性インスペクター**パネルに固有の次のオプションがあります。

[![](window-images/panel03.png "属性インスペクター")](window-images/panel03.png#lightbox)

- **スタイル**-からパネルのスタイルを調整することを許可する: 通常パネル (標準的なウィンドウのようになります)、ユーティリティのパネル (より小さいタイトル バーを持つ) HUD パネル (半透明タイトル バーの背景の一部であり)。
- **非ライセンス**-で決定、パネル、[キー] ウィンドウになります。
- **ドキュメントのモーダル**-場合ドキュメント モーダルで、アプリケーションのウィンドウの上、パネルがフローティングのみ、それ以外の場合よりも手前何よりです。


新しいパネルを追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**プロジェクトを右クリックし、選択、**追加** > **新しいファイル.**.
2. 新しいファイル] ダイアログ ボックスで、[ **Xamarin.Mac** > **Cocoa ウィンドウ コント ローラーと**:

    [![](window-images/panels00.png "新しいウィンドウ コント ローラーの追加")](window-images/panels00.png#lightbox)
3. **[名前]** に「`DocumentPanel`」と入力し、**[新規]** ボタンをクリックします。
4. ダブルクリックして、`DocumentPanel.xib`ファイルを開き、Interface Builder での編集します。 

    [![](window-images/new02.png "編集パネル")](window-images/new02.png#lightbox)
5. 既存のウィンドウを削除してからパネルをドラッグして、**ライブラリ インスペクター**で、**インターフェイス エディター**: 

    [![](window-images/panels01.png "既存のウィンドウを削除します。")](window-images/panels01.png#lightbox)
6. フック、パネル、**ファイルの所有者** - **ウィンドウ** - **アウトレット**: 

    [![](window-images/panels02.png "パネルの線をドラッグします。")](window-images/panels02.png#lightbox)
7. 切り替えて、 **Identity Inspector**パネルのクラスを設定および`DocumentPanel`: 

    [![](window-images/panels03.png "パネルのクラスの設定")](window-images/panels03.png#lightbox)
6. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。
7. 編集、`DocumentPanel.cs`ファイルを開き、次のクラス定義を変更します。 

    `public partial class DocumentPanel : NSPanel`
8. 変更内容をファイルに保存します。

編集、`AppDelegate.cs`ファイル、`DidFinishLaunching`メソッドの次のようになります。

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
        // Display panel
    var panel = new DocumentPanelController ();
    panel.Window.MakeKeyAndOrderFront (this);
}

```

アプリケーションを実行した場合は、パネルが表示されます。

[![](window-images/panels04.png "実行中のアプリ内のパネル")](window-images/panels04.png#lightbox)

> [!IMPORTANT]
> パネルの Windows が Apple では非推奨し、置き換える必要があります**インスペクター インターフェイス**します。 作成の完全な例については、**インスペクター** Xamarin.Mac アプリケーションでは、次を参照してください、 [MacInspector](https://developer.xamarin.com/samples/mac/MacInspector/)サンプル アプリです。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションで Windows とパネルの使用方法について詳しく説明をしました。 さまざまな種類を確認し、Windows とパネル、Windows とパネル Xcode の Interface Builder で作成および管理する方法、および c# コードで Windows とパネルを使用する方法を使用します。

## <a name="related-links"></a>関連リンク

- [MacWindows (サンプル)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [MacInspector (サンプル)](https://developer.xamarin.com/samples/mac/MacInspector/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [メニューの作業](~/mac/user-interface/menu.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
