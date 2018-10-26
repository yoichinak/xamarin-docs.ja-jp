---
title: Xamarin.Mac のメニュー
description: この記事では、Xamarin.Mac アプリケーションのメニューの操作について説明します。 作成およびメニューおよび Xcode と Interface Builder でのメニュー項目を維持し、それらをプログラムで操作がについて説明します。
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: d12815c92c1f608a5df4b3869fe9a17cb604b47a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50109680"
---
# <a name="menus-in-xamarinmac"></a>Xamarin.Mac のメニュー

_この記事では、Xamarin.Mac アプリケーションのメニューの操作について説明します。作成およびメニューおよび Xcode と Interface Builder でのメニュー項目を維持し、それらをプログラムで操作がについて説明します。_

Xamarin.Mac アプリケーションで c# と .NET を使用する場合は、OBJECTIVE-C と Xcode で作業する開発者は同じの Cocoa メニューへのアクセスがあります。 Xamarin.Mac は直接 Xcode と統合、ため、作成と、メニュー バー、メニューのおよびメニュー項目を維持 (または必要に応じて c# コードで直接作成する) Xcode の Interface Builder を使用できます。

メニューは、Mac アプリケーションのユーザー エクスペリエンスの不可欠な部分をおよびユーザー インターフェイスのさまざまな部分によく表示されます。

- **アプリケーションのメニュー バー** -これは、すべての Mac アプリケーションの画面の上部に表示されるメインのメニュー。
- **コンテキスト メニュー** -これらは、ユーザーを右クリックしたか、ウィンドウ内の項目をコントロール クリックすると表示されます。
- **ステータス バー** -これは項目が追加されると、左に拡大 (メニュー バーの時計の左側) に画面の上部に表示されるアプリケーションのメニュー バーの右端にある領域です。
- **ドッキング メニュー**のときにユーザーを右クリックしたコントロール クリックすると、アプリケーションのアイコン、または、ユーザーがアイコンを左クリックして、マウス ボタンを押したに表示される、ドッキング ステーション内の各アプリケーションのメニュー。
- **ポップアップのボタンをクリックし、プルダウン リスト**-ポップアップのボタンの選択項目を表示し、ユーザーがクリックされたときから選択するオプションの一覧が表示されます。 プルダウン リストには、ポップアップのボタンが通常は、現在のタスクのコンテキストに固有のコマンドを選択するための一種です。 ウィンドウをどこにでも両方表示できます。

[![例のメニューが](menu-images/intro01.png "例メニューは、")](menu-images/intro01-large.png#lightbox)

この記事では、Cocoa メニュー バー、メニューのおよび、Xamarin.Mac アプリケーションでメニュー項目の操作の基礎を取り上げます。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

見てしたい場合があります、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)、について説明します、ドキュメント、`Register`と`Export`属性Objective C のオブジェクトと UI 要素を c# クラスをワイヤ アップするために使用します。

## <a name="the-applications-menu-bar"></a>アプリケーションのメニュー バー 

アプリケーションのすべてのウィンドウが接続して、独自のメニュー バーがあることができます、Windows OS で実行されているとは異なりは、macOS で実行されているすべてのアプリケーションは、すべてのウィンドウでそのアプリケーションに使用される画面の上部を実行する 1 つのメニュー バーを持ちます。

[![メニュー バー](menu-images/appmenu01.png "メニュー バー")](menu-images/appmenu01-large.png#lightbox)

このメニュー バーの項目をアクティブ化または非アクティブ化された特定の時点で、現在のコンテキストや、アプリケーションとそのユーザー インターフェイスの状態に基づいて。 例: 上の項目、ユーザーは、テキスト フィールドを選択する場合、**編集**メニューなどが有効なものは**コピー**と**切り取り**します。

Apple に従ってされ、既定では、メニューとアプリケーションのメニュー バーに表示されるメニュー項目の標準セットがあるすべての macOS アプリケーション。

- **Apple メニュー** -このメニューはどのようなアプリケーションの実行に関係なく、常に、ユーザーに使用できる幅の項目のシステムへのアクセスを提供します。 これらの項目は、開発者によって変更ことはできません。
- **アプリのメニュー** -このメニューは、アプリケーションの名前を太字で表示され、ユーザーがどのようなアプリケーションが現在実行されているかを識別します。 全体しない、特定のドキュメントまたはアプリケーションの終了などのプロセスとしてアプリケーションに適用される項目が含まれています。
- **[ファイル] メニュー** - 項目を開くを作成するために使用するかドキュメントを保存するアプリケーションが処理します。 アプリケーションではないドキュメントに基づく、このメニューを名前変更し、削除します。
- **[編集] メニュー** -などのコマンドを保持**切り取り**、**コピー**、および**貼り付け**編集またはアプリケーションのユーザー インターフェイス内の要素の変更に使用されます。
- **[書式] メニュー** - そのテキストの書式設定を調整する保留コマンド テキスト、このメニューで、アプリケーションが動作している場合。
- **[表示] メニュー** -に影響するコマンド コンテンツの表示方法でアプリケーションのユーザー インターフェイス (表示) を保持します。
- **アプリケーション固有のメニュー** -これらは、任意の web ブラウザーのブックマーク メニュー) など、アプリケーションに固有のメニュー。 間で出現する必要があります、**ビュー**と**ウィンドウ**バーのメニュー。
- **ウィンドウ メニュー** -現在開いているウィンドウの一覧と同様に、アプリケーションで windows を操作するためのコマンドが含まれます。
- **[ヘルプ] メニュー** -アプリケーションでは、画面に表示されるヘルプを提供する場合、[ヘルプ] メニューがバーの右端のメニューをする必要があります。 

アプリケーションのメニュー バー、標準的なメニューとメニュー項目の詳細については、Apple を参照してください[ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)します。

### <a name="the-default-application-menu-bar"></a>既定のアプリケーションのメニュー バー

新しい Xamarin.Mac プロジェクトを作成するたびに、標準的な既定アプリケーション メニュー バーが macOS アプリケーション (前のセクションで説明したように) として通常必要のある一般的なアイテムが自動的に取得します。 アプリケーションの既定のメニュー バーが定義されている、 **Main.storyboard** (と共に、アプリケーションの UI の残りの部分) で、プロジェクトの下のファイル、 **Solution Pad**:  

![メインのストーリー ボードを選択します。](menu-images/appmenu02.png "メインのストーリー ボードを選択します。")

ダブルクリックして、 **Main.storyboard**メニュー エディターのインターフェイスが表示され、Xcode の Interface Builder での編集用に開くファイルを。

[![Xcode で UI を編集](menu-images/defaultbar01.png "Xcode では、UI の編集")](menu-images/defaultbar01-large.png#lightbox)

ここからボタンをクリックする項目など、**オープン**のメニュー項目、**ファイル**メニューおよび編集や、そのプロパティを調整、 **Attributes Inspector**:

[![メニューの属性の編集](menu-images/defaultbar02.png "メニューの属性の編集")](menu-images/defaultbar02-large.png#lightbox)

追加、編集、およびメニューとこの記事の後半で項目の削除に表示されます。 ようになりましただけにどのようなメニューとメニュー項目は既定で使用可能とする方法が公開されている自動的に一連の定義済み outlet と action を使用してコードを参照してください (詳細については、次を参照してくださいこの[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions) 。ドキュメント)。

たとえばをクリックした場合、**接続インスペクター**の**オープン**メニュー項目の最大ワイヤードが自動的に確認できます、`openDocument:`アクション。 

[![接続されているアクションを表示する](menu-images/defaultbar03.png "接続されているアクションを表示します。")](menu-images/defaultbar03-large.png#lightbox)

選択した場合、**最初レスポンダー**で、**インターフェイス階層**で下へスクロールし、**接続インスペクター**の定義が表示されます、 `openDocument:`アクションを**オープン**メニュー項目が (と共にいくつかその他の既定アクション アプリケーションは、自動的にコントロールまでケーブル接続されていません) に接続されています。

[![アタッチされたすべてのアクションを表示する](menu-images/defaultbar04.png "接続されているすべてのアクションを表示します。")](menu-images/defaultbar04-large.png#lightbox) 

なぜこれが重要なでしょうか。 次に、自動的に定義されているこれらのアクションが自動的に有効にしてメニュー項目を無効にするだけでなく、項目の組み込み機能を提供するには他の Cocoa ユーザー インターフェイス要素を扱う方法セクションが表示されます。

後で使用するこれらの組み込みのアクションを有効にし、コードから項目を無効にするには、選択した場合、独自の機能を提供します。

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>組み込みメニューの機能

いくつかの項目が自動的にワイヤード (有線) アップし有効にするが (完全に機能により自動的に組み込み) などがわかります任意の UI アイテムまたはコードを追加する前に、新しく作成された Xamarin.Mac アプリケーションの実行をした場合、 **Quit**内の項目、**アプリ**メニュー。

![有効になっているメニュー項目](menu-images/appmenu03.png "を有効になっているメニュー項目")

などの他のメニュー項目の while**切り取り**、**コピー**、および**貼り付け**ないです。

![メニュー項目を無効になっている](menu-images/appmenu04.png "メニュー項目を無効になっています")

アプリケーションを停止し、ダブルクリックしましょう、 **Main.storyboard**ファイル、 **Solution Pad**の Interface Builder を Xcode で開きます。 次に、ドラッグ、**テキスト ビュー**から、**ライブラリ**でウィンドウのビュー コント ローラー上に、**インターフェイス エディター**:

[![テキスト ビューを選択する、ライブラリから](menu-images/appmenu05.png "ライブラリからのテキスト ビューの選択")](menu-images/appmenu05-large.png#lightbox)

**制約エディター**みましょうウィンドウの端にテキスト ビューをピン留めし、設定、拡張して、すべて次の 4 つ赤い I ビーム、エディターの上部にあるをクリックして、ウィンドウの縮小、 **の4制約の追加**ボタンをクリックします。

[![2 つの制約を編集](menu-images/appmenu06.png "2 つの制約の編集")](menu-images/appmenu06-large.png#lightbox)

ユーザー インターフェイスのデザインに変更を保存し、Visual Studio for Mac で Xamarin.Mac プロジェクトに変更を同期するを戻します。 今すぐアプリケーションを起動、テキスト ビューにテキストを入力、選択し、および開く、**編集**メニュー。

![メニュー項目が自動的に有効/無効](menu-images/appmenu07.png "メニュー項目が自動的に有効/無効")

通知方法、**切り取り**、**コピー**と**貼り付け**項目がすべて 1 行のコードを記述することがなく自動的に有効になっており、完全に機能が。 

ここで何が起こっているんですか。 組み込み (前述)、既定のメニュー項目までワイヤード (有線) あるアクションを事前に定義、特定のアクションにフックの macOS の一部である Cocoa ユーザー インターフェイス要素のほとんどが組み込まれていますに注意してください (など`copy:`)。 したがって、アクティブなウィンドウに追加および選択すると、対応するメニュー項目または項目にアタッチされている時そのアクションは自動的に有効です。 ユーザーは、そのメニュー項目を選択する場合、UI 要素に組み込まれた機能と呼ばれ、すべて開発者が関与せずに実行します。

### <a name="enabling-and-disabling-menus-and-items"></a>有効にして、メニューおよび項目を無効化

ユーザー イベントが発生するたびに、既定で`NSMenu`自動的に有効にし、各表示されているメニューとメニューは、アプリケーションのコンテキストに基づいて項目を無効にします。 項目の有効/無効にする 3 つの方法はあります。

- **自動メニューを有効にする**の場合、メニュー項目が有効になっている`NSMenu`項目は、ワイヤード (有線) にするとなったアクションに応答する適切なオブジェクトを検索することができます。 たとえば、テキスト ビュー上組み込みフックに含まれていた、`copy:`アクション。
- **カスタム アクションと validateMenuItem:** - 任意のメニュー項目にバインドされている、[ウィンドウまたはビュー コント ローラーのカスタム アクション](#Working-with-Custom-Window-Actions)、追加することができます、`validateMenuItem:`アクションおよび手動で有効にするし、メニュー項目を無効化します。
- **手動でのメニューを有効にする**-手動で設定する、`Enabled`の各プロパティ`NSMenuItem`有効またはメニュー内の各項目を個別に無効にします。

システムを選択するには、設定、`AutoEnablesItems`のプロパティを`NSMenu`します。 `true` 自動 (既定の動作) と`false`は手動で行います。 

> [!IMPORTANT]
> 手動でのメニューの有効化を使用する場合は、メニューの [なし] 項目などの AppKit クラスによって制御されるものも含め`NSTextView`、自動的に更新されます。 有効にして、コードで手動ですべての項目を無効化は負担になります。

#### <a name="using-validatemenuitem"></a>ValidateMenuItem を使用します。

任意のメニュー項目にバインドされているため、前述のようを[ウィンドウまたはビュー コント ローラーのカスタム アクション](#Working-with-Custom-Window-Actions)、追加することができます、`validateMenuItem:`アクションと手動で有効またはメニュー項目を無効にします。

次の例では、`Tag`が有効/無効にしているメニュー項目の種類を決定するプロパティが使用されます、`validateMenuItem:`で選択したテキストの状態に基づいてアクションを`NSTextView`します。 `Tag` Interface Builder の各メニュー項目のプロパティが設定されています。

![Tag プロパティを設定](menu-images/validate01.png "Tag プロパティを設定")

次のコード ビュー コント ローラーに追加します。

```csharp
[Action("validateMenuItem:")]
public bool ValidateMenuItem (NSMenuItem item) {

    // Take action based on the menu item type
    // (As specified in its Tag)
    switch (item.Tag) {
    case 1:
        // Wrap menu items should only be available if
        // a range of text is selected
        return (TextEditor.SelectedRange.Length > 0);
    case 2:
        // Quote menu items should only be available if
        // a range is NOT selected.
        return (TextEditor.SelectedRange.Length == 0);
    }

    return true;
}
```

このコードを実行するとでテキストが選択されていない、 `NSTextView`、2 つのラップのメニュー項目が無効になります (ただしこれらは、ビュー コント ローラーのアクションにワイヤード (有線) は)。

![項目の表示を無効になっている](menu-images/validate02.png "が表示された項目を無効になっています")

テキストのセクションが選択されている、メニューが開いた場合、2 つのラップのメニュー項目は利用できます。

![項目の表示を有効になっている](menu-images/validate03.png "示すには、項目が有効になっています。")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>有効にして、コードでメニュー項目への応答

上 (テキスト フィールドの場合) など、UI デザインに特定の Cocoa ユーザー インターフェイス要素を追加するだけで述べたようにいくつかの既定のメニュー項目が有効になり、コードを記述することがなく自動的に機能します。 [次へ] をメニュー項目を有効にして、ユーザーが選択するときに、機能を提供する、Xamarin.Mac プロジェクトに独自の c# コードを追加することを見てみましょう。

使用できるユーザーが必要だなど、**オープン**内の項目、**ファイル** メニューのフォルダーを選択します。 アプリケーション全体の関数を抽出し、特定のウィンドウまたは UI 要素に限定されませんいるので、アプリケーション デリゲートにこれを処理するコードを追加できるようになります。

**Solution Pad**、ダブルクリックして、`AppDelegate.CS`ファイルを開き、編集します。

![選択、app delegate](menu-images/appmenu08.png "アプリ デリゲートの選択")

`DidFinishLaunching` メソッドの下に次のコードを追加します。

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

アプリケーションを実行して開く、、**ファイル**メニュー。 

![[ファイル] メニュー](menu-images/appmenu09.png "[ファイル] メニュー")

注意、**オープン**メニュー項目が有効になっているようになりました。 選択して、[開く] ダイアログが表示されます。

![開く ダイアログ](menu-images/appmenu10.png "を開く ダイアログ")

クリックした場合、**オープン**ボタン、警告メッセージが表示されます。

![ダイアログ メッセージの例](menu-images/appmenu11.png "ダイアログ メッセージの例")

ここで重要な行が`[Export ("openDocument:")]`に通知`NSMenu`を**AppDelegate**メソッドを持つ`void OpenDialog (NSObject sender)`に応答する、`openDocument:`アクション。 上記のことを思い出して場合、**オープン**メニュー項目が自動的にワイヤード (有線) をこのアクションに既定では Interface Builder で。

[![接続されているアクションを表示する](menu-images/defaultbar03.png "接続されているアクションを表示します。")](menu-images/defaultbar03-large.png#lightbox)

次のコードで独自のメニューのメニュー項目、およびアクションを作成して、応答を見てみましょう。

### <a name="working-with-the-open-recent-menu"></a>最近使用したメニューを開く の操作

既定で、**ファイル**メニューが含まれています、**開いて最近**ユーザーがアプリで開かれている最後のいくつかのファイルを追跡する項目。 作成する場合、`NSDocument`ベースの Xamarin.Mac アプリでは、このメニューが自動的に処理されます。 Xamarin.Mac アプリの他の種類の管理と、このメニュー項目を手動で応答できます。

手動で処理するために、**開いて最近**] メニューの [必要があります新しいファイルを開いてされているか、次を使用して保存することを通知します。

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

アプリを使用していない場合でも`NSDocuments`、使用することも、`NSDocumentController`維持するために、**開いて最近**] メニューの [送信することによって、`NSUrl`するファイルの場所で、`NoteNewRecentDocumentURL`のメソッド、`SharedDocumentController`します。

次に、オーバーライドする必要があります、`OpenFile`からユーザーが選択したすべてのファイルを開きますアプリ デリゲートのメソッド、**最近開いて**メニュー。 例えば:

```csharp
public override bool OpenFile (NSApplication sender, string filename)
{
    // Trap all errors
    try {
        filename = filename.Replace (" ", "%20");
        var url = new NSUrl ("file://"+filename);
        return OpenFile(url);
    } catch {
        return false;
    }
}
```

返す`true`返すそれ以外の場合、ファイルを開くことができる場合`false`と組み込みの警告が、ファイルが開けませんユーザーに表示されます。

パスとファイル名が返されるので、**開いて最近**メニューで、スペースを含めることができます、正しく作成する前にこの文字をエスケープする必要があります、`NSUrl`またはエラーが発生しました。 次のコードを実行します。

```csharp
filename = filename.Replace (" ", "%20");
```

最後に、作成、`NSUrl`を新しいウィンドウを開き、ファイルを読み込む、アプリ内のヘルパー メソッドがデリゲートを使用して、ファイルを指します。

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

すべてをまとめてプルするには実装の例を見てを見てみましょう、 **AppDelegate.cs**ファイル。

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace MacHyperlink
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }

        public override bool OpenFile (NSApplication sender, string filename)
        {
            // Trap all errors
            try {
                filename = filename.Replace (" ", "%20");
                var url = new NSUrl ("file://"+filename);
                return OpenFile(url);
            } catch {
                return false;
            }
        }
        #endregion

        #region Private Methods
        private bool OpenFile(NSUrl url) {
            var good = false;

            // Trap all errors
            try {
                var path = url.Path;

                // Is the file already open?
                for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
                    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
                    if (content != null && path == content.FilePath) {
                        // Bring window to front
                        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
                        return true;
                    }
                }

                // Get new window
                var storyboard = NSStoryboard.FromName ("Main", null);
                var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

                // Display
                controller.ShowWindow(this);

                // Load the text into the window
                var viewController = controller.Window.ContentViewController as ViewController;
                viewController.Text = File.ReadAllText(path);
                viewController.SetLanguageFromPath(path);
                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                viewController.View.Window.RepresentedUrl = url;

                // Add document to the Open Recent menu
                NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);

                // Make as successful
                good = true;
            } catch {
                // Mark as bad file on error
                good = false;
            }

            // Return results
            return good;
        }
        #endregion

        #region actions
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
                    // Open the document in a new window
                    OpenFile (url);
                }
            }
        }
        #endregion
    }
}
```

アプリの要件に基づき、たくないユーザーに同時に複数のウィンドウで同じファイルを開きます。 当社の例のアプリ、ユーザーが既に開いているファイルを選択した場合に (いずれかから、**最近開いて**または**を開く..** メニュー項目の場合)、前に、ファイルを含むウィンドウになります。

これを行うには、ヘルパー メソッドで、次のコードを使いました。

```csharp
var path = url.Path;

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

設計、`ViewController`内のファイルへのパスを保持するクラスをその`Path`プロパティ。 次に、アプリで現在開いているすべての windows をループします。 ファイルが、windows のいずれかでまだ開いて場合、を使用して他のすべての windows の前になります。

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

一致が検出されない場合は、読み込まれるファイルを使用して、新しいウィンドウが開かれたファイルが記載、**開いて最近**メニュー。

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);

// Load the text into the window
var viewController = controller.Window.ContentViewController as ViewController;
viewController.Text = File.ReadAllText(path);
viewController.SetLanguageFromPath(path);
viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
viewController.View.Window.RepresentedUrl = url;

// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

<a name="Working-with-Custom-Window-Actions" />

### <a name="working-with-custom-window-actions"></a>カスタムのウィンドウの動作の使用

同様に、組み込み**最初の応答側**標準メニュー項目を事前にワイヤード (有線) の操作、新しいカスタム アクションを作成して Interface Builder でのメニュー項目を接続することです。

最初に、アプリのウィンドウのコント ローラーのいずれかのカスタム アクションを定義します。 例えば:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

次に、アプリのストーリー ボード ファイルをダブルクリックして、 **Solution Pad**の Interface Builder を Xcode で開きます。 選択、**最初レスポンダー**下、**アプリケーション シーン**に切り替えて、 **Attributes Inspector**:

![Attributes Inspector](menu-images/action01.png "属性インスペクター")

をクリックして、 **+** の下部にあるボタン、 **Attributes Inspector**新しいカスタム アクションを追加します。

![新しいアクションを追加する](menu-images/action02.png "新しいアクションを追加します。")

ウィンドウ コント ローラー上に作成したカスタムの動作と同じ名前を付けます。

![アクション名を編集](menu-images/action03.png "アクション名を編集")

コントロールをクリックし、メニュー項目からドラッグ、**最初レスポンダー**下、**アプリケーション シーン**。 ポップアップ リストから作成した新しいアクションを選択します (`defineKeyword:`この例では)。

![アクションをアタッチ](menu-images/action04.png "アクションをアタッチします。")

ストーリー ボードに変更を保存し、Visual Studio for Mac の変更を同期するに戻ります。 アプリを実行する場合は、カスタム アクションが接続されているメニュー項目が自動的に有効/無効にする (オープンされているアクションでは、ウィンドウに基づく) と、アクションが起動メニュー項目を選択します。

[![新しいアクションをテスト](menu-images/action05.png "新しいアクションのテスト")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>追加、編集、およびメニューを削除します。

前のセクションで述べたように、Xamarin.Mac アプリケーションを事前設定された数の既定のメニューおよびメニュー項目に特定の UI コントロールに自動的にアクティブ化し、応答が付属します。 また有効にし、これらの既定の項目に対応するアプリケーションにコードを追加する方法もきました。

このセクションでは、不要なメニュー項目を削除する、メニューを再編成と新しいメニューのメニュー項目とアクションを追加することに注目します。

ダブルクリック、 **Main.storyboard**ファイル、 **Solution Pad**編集用に開きます。

[![Xcode で UI を編集](menu-images/maint01.png "Xcode では、UI の編集")](menu-images/maint01-large.png#lightbox)

特定 Xamarin.Mac アプリケーションのことはできません、既定値を使用する**ビュー**ですから、それを削除するメニュー。 **インターフェイス階層**選択、**ビュー**メイン メニュー バーの一部であるメニュー項目。

![ビューのメニュー項目を選択](menu-images/maint02.png "ビュー メニュー項目を選択します。")

Del キーを押すか、バック スペース、メニューを削除します。 次に、すべての項目で使用するつもり、**形式**メニューとサブメニューの下を使用するアイテムを移動します。 **インターフェイス階層**メニュー項目を選択します。

![複数の項目を強調表示](menu-images/maint03.png "複数の項目を強調表示")

親の下の項目をドラッグ**メニュー**サブメニュー現在されている場所から。

[![親メニューにメニュー項目をドラッグ](menu-images/maint04.png "親メニューにメニュー項目をドラッグします。")](menu-images/maint04-large.png#lightbox)

メニューは、以下のようになりますようになりました。

[![新しい場所に項目](menu-images/maint05.png "新しい場所に項目")](menu-images/maint05-large.png#lightbox)

[次へ] ドラッグ、**テキスト**の下にあるサブメニューを**形式**メニュー間にあるメイン メニュー バーに配置し、**形式**と**ウィンドウ**メニュー:

[![テキスト メニュー](menu-images/maint06.png "Text メニュー")](menu-images/maint06-large.png#lightbox)

下に移動、**形式**メニューと削除、**フォント**サブメニュー項目。 次に、選択、**形式**メニュー「フォント」名前を変更します。

[![フォント メニュー](menu-images/maint07.png "フォント メニュー")](menu-images/maint07-large.png#lightbox)

次が自動的に取得付加テキスト ビュー内のテキストには、選択したときに事前に定義されるフレーズのカスタム メニューを作成してみましょう。 検索ボックスの下部にある、**ライブラリ インスペクター** 「メニュー」の種類。 これは容易にできるようにすべてのメニューの UI 要素を探して操作します。

![ライブラリ インスペクター](menu-images/maint08.png "ライブラリ インスペクター")

これで、メニューを作成するには、次を実行してみましょう。

1. ドラッグ、**メニュー項目**から、**ライブラリ インスペクター**間にあるメニュー バーの上に、**テキスト**と**ウィンドウ**メニュー。 

    ![ライブラリに新しいメニュー項目を選択する](menu-images/maint10.png "ライブラリに新しいメニュー項目を選択します。")
2. 項目「フレーズ」の名前を変更します。 

    [![メニュー名を設定する](menu-images/maint09.png "メニュー名を設定します。")](menu-images/maint09-large.png#lightbox)
3. 次にドラッグして、**メニュー**から、**ライブラリ インスペクター**: 

    ![ライブラリから、メニューを選択する](menu-images/maint11.png "ライブラリから、メニューを選択します。")
4. 削除し、**メニュー**新しい**メニュー項目**先ほど作成し、「フレーズ」に名称変更します。 

    [![編集メニュー名](menu-images/maint12.png "メニュー名の編集")](menu-images/maint12-large.png#lightbox)
5. これで 3 つの既定の名前を変更してみましょう**メニュー項目**"Address"、「日付」および"Greeting"。 

    [![フレーズ メニュー](menu-images/maint13.png "語句のメニュー")](menu-images/maint13-large.png#lightbox)
6. 4 番目を追加してみましょう**メニュー項目**をドラッグして、**メニュー項目**から、**ライブラリ インスペクター**および「署名」呼び出し。 

    [![メニュー項目の名前を編集](menu-images/maint14.png "メニュー項目の名前を編集")](menu-images/maint14-large.png#lightbox)
7. メニュー バーには、変更を保存します。

これで、新しいメニュー項目が c# コードに公開されているように、カスタム アクションのセットを作成しましょう。 Xcode でに切り替えてみましょう、**アシスタント**ビュー。

[![必要なアクションを作成する](menu-images/maint15.png "必要なアクションを作成します。")](menu-images/maint15-large.png#lightbox)

それでは、次の操作を行います。

1. コントロールをドラッグ、**アドレス**メニュー項目を**AppDelegate.h**ファイル。
2. スイッチ、**接続**型**アクション**: 

    [![アクションの種類を選択する](menu-images/maint17.png "アクションの種類を選択します。")](menu-images/maint17-large.png#lightbox)
3. 入力、**名前**"phraseAddress"のキーを押して、 **Connect**新しいアクションを作成するボタン。 

    [![アクションの構成](menu-images/maint18.png "アクションの構成")](menu-images/maint18-large.png#lightbox)
4. 上記の手順を繰り返して、**日付**、 **Greeting**と**署名**メニュー項目。 

    [![完了したアクション](menu-images/maint19.png "完了済みのアクション")](menu-images/maint19-large.png#lightbox)
5. メニュー バーには、変更を保存します。

次に、コードからそのコンテンツを調整できるように、テキスト ビューのアウトレットを作成する必要があります。 選択、 **ViewController.h**ファイル、**アシスタント エディター**と呼ばれる新しいアウトレットを作成および`documentText`:

[![アウトレットを作成する](menu-images/maint20.png "アウトレットを作成します。")](menu-images/maint20-large.png#lightbox)

Visual Studio for Mac は Xcode から変更を同期に戻ります。 次の編集、 **ViewController.cs**ファイルを開き、次のようになります。

```csharp
using System;

using AppKit;
using Foundation;

namespace MacMenus
{
    public partial class ViewController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }

        public string Text {
            get { return documentText.Value; }
            set { documentText.Value = value; }
        } 
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            App.textEditor = this;
        }

        public override void ViewWillDisappear ()
        {
            base.ViewDidDisappear ();

            App.textEditor = null;
        }
        #endregion
    }
}
```

これにより、以外の場合は、このテキスト ビューのテキスト、`ViewController`クラスし、ウィンドウを取得したりがフォーカスを失ったときに、app delegate に通知します。 これで編集、 **AppDelegate.cs**ファイルを開き、次のようになります。

```csharp
using AppKit;
using Foundation;
using System;

namespace MacMenus
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public ViewController textEditor { get; set;} = null;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = false;
            dlg.CanChooseDirectories = true;

            if (dlg.RunModal () == 1) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
                    MessageText = "Folder Selected"
                };
                alert.RunModal ();
            }
        }

        partial void phrasesAddress (Foundation.NSObject sender) {

            textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
        }

        partial void phrasesDate (Foundation.NSObject sender) {

            textEditor.Text += DateTime.Now.ToString("D");
        }

        partial void phrasesGreeting (Foundation.NSObject sender) {

            textEditor.Text += "Dear Sirs,\n\n";
        }

        partial void phrasesSignature (Foundation.NSObject sender) {

            textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
        }
        #endregion
    }
}
```

ここでなりました、`AppDelegate`部分クラスのアクションとアウトレット Interface Builder で定義した使用できるようにします。 公開も、`textEditor`どのウィンドウは現在フォーカスを追跡するためにします。

次のメソッドは、カスタム メニューとメニュー項目を処理するために使用されます。

```csharp
partial void phrasesAddress (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
}

partial void phrasesDate (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += DateTime.Now.ToString("D");
}

partial void phrasesGreeting (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Dear Sirs,\n\n";
}

partial void phrasesSignature (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
}
```

ここで、アプリケーション内の項目のすべての実行、**語句**メニューがアクティブになるし、選択されている場合は、テキスト ビューに与えるという語句を追加します。

![実行されているアプリの例](menu-images/maint21.png "実行されているアプリの例")

下のアプリケーションのメニュー バーの操作の基礎をしたら、次のカスタム コンテキスト メニューの作成を見てみましょう。

### <a name="creating-menus-from-code"></a>コードからメニューを作成します。

Xcode の Interface Builder でメニューおよびメニュー項目を作成するだけでなく回 Xamarin.Mac アプリを作成、変更、またはコードから、メニューのサブメニュー、またはメニュー項目を削除する必要がある場合があります。

次の例では、メニュー項目とサブメニューに動的に作成される、稼働中の情報を保持するクラスが作成されます。

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace AppKit.TextKit.Formatter
{
    public class LanguageFormatCommand : NSObject
    {
        #region Computed Properties
        public string Title { get; set; } = "";
        public string Prefix { get; set; } = "";
        public string Postfix { get; set; } = "";
        public List<LanguageFormatCommand> SubCommands { get; set; } = new List<LanguageFormatCommand>();
        #endregion

        #region Constructors
        public LanguageFormatCommand () {

        }

        public LanguageFormatCommand (string title)
        {
            // Initialize
            this.Title = title;
        }

        public LanguageFormatCommand (string title, string prefix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
        }

        public LanguageFormatCommand (string title, string prefix, string postfix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
            this.Postfix = postfix;
        }
        #endregion
    }
}
```

#### <a name="adding-menus-and-items"></a>メニューと項目の追加

このクラス定義を次のルーチンは、のコレクションを解析`LanguageFormatCommand`オブジェクトと再帰的に渡された (Interface Builder で作成された) 既存のメニューの下部に追加することで、新しいメニューおよびメニュー項目をビルドします。

```csharp
private void AssembleMenu(NSMenu menu, List<LanguageFormatCommand> commands) {
    NSMenuItem menuItem;

    // Add any formatting commands to the Formatting menu
    foreach (LanguageFormatCommand command in commands) {
        // Add separator or item?
        if (command.Title == "") {
            menuItem = NSMenuItem.SeparatorItem;
        } else {
            menuItem = new NSMenuItem (command.Title);

            // Submenu?
            if (command.SubCommands.Count > 0) {
                // Yes, populate submenu
                menuItem.Submenu = new NSMenu (command.Title);
                AssembleMenu (menuItem.Submenu, command.SubCommands);
            } else {
                // No, add normal menu item
                menuItem.Activated += (sender, e) => {
                    // Apply the command on the selected text
                    TextEditor.PerformFormattingCommand (command);
                };
            }
        }
        menu.AddItem (menuItem);
    }
}
``` 

いずれかの`LanguageFormatCommand`は空白を含むオブジェクト`Title`プロパティの場合、このルーチンでは、**区切り記号のメニュー項目**(薄い灰色の線) メニュー セクションの間。

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

タイトルが指定されている場合は、そのタイトルの新しいメニュー項目が作成されます。

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

場合、`LanguageFormatCommand`オブジェクトには、子が含まれています。`LanguageFormatCommand`オブジェクト、サブメニューが作成されると、`AssembleMenu`メソッドは、そのメニューを構築するという再帰的に。

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

サブメニューがない新しいメニュー項目には、ユーザーが選択されているメニュー項目を処理するコードが追加されます。

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>メニューの作成のテスト

すべての場所で上記のコードの場合は、次にまとめて`LanguageFormatCommand`オブジェクトが作成されました。

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Stong","**","**"));
FormattingCommands.Add(new LanguageFormatCommand("Emphasize","_","_"));
FormattingCommands.Add(new LanguageFormatCommand("Inline Code","`","`"));
FormattingCommands.Add(new LanguageFormatCommand("Code Block","```\n","\n```"));
FormattingCommands.Add(new LanguageFormatCommand("Comment","<!--","-->"));
FormattingCommands.Add (new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Unordered List","* "));
FormattingCommands.Add(new LanguageFormatCommand("Ordered List","1. "));
FormattingCommands.Add(new LanguageFormatCommand("Block Quote","> "));
FormattingCommands.Add (new LanguageFormatCommand ());

var Headings = new LanguageFormatCommand ("Headings");
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 1","# "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 2","## "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 3","### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 4","#### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 5","##### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 6","###### "));
FormattingCommands.Add (Headings);

FormattingCommands.Add(new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Link","[","]()"));
FormattingCommands.Add(new LanguageFormatCommand("Image","![]("-")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[ ![]("-")](linkimagehere/index.md)"));
```

渡されたコレクションと、`AssembleMenu`関数 (で、**形式**メニューは、ベースとして設定)、次の動的メニューとメニュー項目が作成されます。

![実行中のアプリで新しいメニュー項目](menu-images/dynamic01.png "実行中のアプリで新しいメニュー項目")

#### <a name="removing-menus-and-items"></a>メニューと項目を削除します。

使用することができます、アプリのユーザー インターフェイスからいずれかのメニューまたはメニュー項目を削除する必要がある場合、`RemoveItemAt`のメソッド、`NSMenu`だけ、0 を付けることによりベース クラスを削除する項目のインデックス。

たとえば、メニューと上記のルーチンによって作成されたメニュー項目を削除するには、次のコードを使用するでした。

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

上記のコードの場合は動的に削除されませんので Xcode の Interface Builder と、アプリで使用可能な無償で最初の 4 つのメニュー項目が作成されます。

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>コンテキスト メニュー

ユーザーを右クリックしたか、ウィンドウ内の項目をコントロール クリックすると、コンテキスト メニューが表示されます。 既定では、macOS に既に組み込まれている UI 要素のいくつかのコンテキスト メニューのテキスト ビュー) などの添付することがあります。 ただし、独自のカスタム コンテキスト メニューのウィンドウに追加した UI 要素を作成したい場合もあります。

編集しましょう、 **Main.storyboard** Xcode でファイルを追加、**ウィンドウ**ウィンドウで、設計をその**クラス**で"NSPanel"に、 **Identity Inspector**、新しい追加**アシスタント**項目を**ウィンドウ**] メニューの [を使用して新しいウィンドウにアタッチし、**セグエを表示する**:

[![セグエの種類を設定](menu-images/context01.png "セグエの種類の設定")](menu-images/context01-large.png#lightbox)

それでは、次の操作を行います。

1. ドラッグ、**ラベル**から、**ライブラリ インスペクター**上に、**パネル**ウィンドウと、そのテキストを"Property"に設定。 

    [![ラベルの値を編集](menu-images/context03.png "ラベルの値を編集")](menu-images/context03-large.png#lightbox)
2. 次にドラッグして、**メニュー**から、**ライブラリ インスペクター** 、階層の表示と名前の変更、3 つの既定のメニュー項目のビュー コント ローラー上に**ドキュメント**、**テキスト**と**フォント**:

    [![必要なメニュー項目](menu-images/context02.png "必要なメニュー項目")](menu-images/context02-large.png#lightbox)
3. 今すぐコントロールからドラッグ、**プロパティ ラベル**上に、**メニュー**:

    [![セグエを作成するドラッグ](menu-images/context04.png "ドラッグ セグエを作成するには")](menu-images/context04-large.png#lightbox)
4. ポップアップ ダイアログ ボックスで、次のように選択します**メニュー**:。 

    ![セグエの種類を設定](menu-images/context05.png "セグエの種類の設定")
5. **Identity Inspector**、ビュー コント ローラーのクラスの"PanelViewController"に設定します。 

    [![設定すると、セグエ クラス](menu-images/context10.png "セグエ クラスの設定")](menu-images/context10-large.png#lightbox)
6. Visual Studio for Mac は、同期に戻るから Interface Builder を。
7. 切り替えて、**アシスタント エディター**を選択し、 **PanelViewController.h**ファイル。
8. アクションを作成、**ドキュメント**メニュー項目と呼ばれる`propertyDocument`: 

    [![アクションの構成](menu-images/context06.png "アクションの構成")](menu-images/context06-large.png#lightbox)
9. 残りのメニュー項目の作成アクションを繰り返します。 

    [![必要な操作](menu-images/context07.png "必要な操作")](menu-images/context07-large.png#lightbox)
10. 最後のアウトレットを作成、**プロパティ ラベル**と呼ばれる`propertyLabel`: 

    [![アウトレットを構成する](menu-images/context08.png "アウトレットを構成します。")](menu-images/context08-large.png#lightbox)
11. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

編集、 **PanelViewController.cs**ファイルを開き、次のコードを追加します。

```csharp
partial void propertyDocument (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Document";
}

partial void propertyFont (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Font";
}

partial void propertyText (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Text";
}
```

これでアプリケーションを実行すると、パネルの プロパティのラベルを右クリックし、このカスタム コンテキスト メニューが表示されます。 メニューから項目を選択、ラベルの値が変更されます。

![実行しているコンテキスト メニュー](menu-images/context09.png "を実行しているコンテキスト メニュー")

[次へ] のステータス バーのメニューの作成を見てみましょう。

## <a name="status-bar-menus"></a>ステータス バーのメニュー

ステータス バーのメニューでは、メニューや、アプリケーションの状態を反映して、イメージなど、ユーザーに一連の状態のメニュー項目との対話を提供するとフィードバックを表示します。 アプリケーションのステータス バーにあるメニューは、アプリケーションがバック グラウンドで実行されている場合でもが有効でアクティブです。 システム全体のステータス バーは、アプリケーションのメニュー バーの右側に配置されている、macOS で現在使用できる唯一のステータス バーです。

編集、 **AppDelegate.cs**ファイル、`DidFinishLaunching`メソッドの次のようになります。

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Create a status bar menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Text";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Text");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        PhraseAddress(address);
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        PhraseDate(date);
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        PhraseGreeting(greeting);
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        PhraseSignature(signature);
    };
    item.Menu.AddItem (signature);
}
```

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` により、システム全体のステータス バーにアクセスできます。 `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` 新しいステータス バーの項目を作成します。 そこから、メニューとメニュー項目の数を作成し、メニューを先ほど作成したステータス バーの項目にアタッチします。 

アプリケーションを実行した場合、新しいステータス バーの項目が表示されます。 メニューから項目を選択すると、テキスト ビュー内のテキストが変更されます。 

![ステータス バーのメニューが実行されている](menu-images/statusbar01.png "実行されているステータス バーのメニュー")

次に、カスタム ドック メニュー項目の作成を見てみましょう。

## <a name="custom-dock-menus"></a>カスタムのドック メニュー

ドック メニューは、ユーザーを右クリックしたかコントロールのドッキング ステーションでアプリケーションのアイコンをクリックする Mac アプリケーションが表示されます。

![カスタムのドッキング メニュー](menu-images/dock01.png "カスタム ドッキング メニュー")

次の手順に従って、アプリケーションのカスタムのドック メニューを作成してみましょう。

1. Visual Studio for Mac で、アプリケーションのプロジェクトと選択を右クリックして**追加** > **新しいファイル.** 新しいファイル ダイアログ ボックスで、次のように選択します**Xamarin.Mac** > **空のインターフェイス定義**、の"DockMenu"を使用して、**名前** をクリックし、**新規。** 新しいを作成するボタン**DockMenu.xib**ファイル。

    ![空のインターフェイス定義を追加する](menu-images/dock02.png "空のインターフェイス定義を追加します。")
2. **Solution Pad**をダブルクリック、 **DockMenu.xib**ファイルを開き、Xcode で編集します。 新規作成**メニュー**次の項目を含む:**アドレス**、**日付**、 **Greeting**、および**署名** 

    [![UI レイアウト](menu-images/dock03.png "UI のレイアウト")](menu-images/dock03-large.png#lightbox)
3. 次に、当社で、カスタム メニュー用に作成した既存のアクションを新しいメニュー項目を接続しましょう、[追加、編集および削除するメニュー](#Adding,_Editing_and_Deleting_Menus)前のセクション。 切り替えて、**接続インスペクター**を選択し、**最初レスポンダー**で、**インターフェイス階層**。 下にスクロールし、検索、`phraseAddress:`アクション。 そのアクションの円から線をドラッグ、**アドレス**メニュー項目。

    [![アクションの線をドラッグ](menu-images/dock04.png "アクションの線をドラッグします。")](menu-images/dock04-large.png#lightbox)
4. すべての他のメニュー項目にアタッチする、対応するアクションについて繰り返します。 

    [![必要な操作](menu-images/dock05.png "必要な操作")](menu-images/dock05-large.png#lightbox)
5. 次に、選択、**アプリケーション**で、**インターフェイス階層**します。 **接続インスペクター**、上の円から行をドラッグ、`dockMenu`アウトレットを作成したばかりのメニューに。

    [![アウトレットをネットワーク上をドラッグ](menu-images/dock06.png "アウトレットをネットワーク上をドラッグします。")](menu-images/dock06-large.png#lightbox)
6. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。
7. ダブルクリックして、 **Info.plist**ファイルを開き、編集します。 

    [![Info.plist ファイルの編集](menu-images/dock07.png "Info.plist ファイルの編集")](menu-images/dock07-large.png#lightbox)
8. をクリックして、**ソース**画面の下部にあるタブ。 

    [![ソース ビューを選択する](menu-images/dock08.png "ソース ビューを選択します。")](menu-images/dock08-large.png#lightbox)
9. をクリックして**新しいエントリを追加する**緑色のプラス ボタンをクリックして、プロパティ名を"AppleDockMenu"と"DockMenu"(拡張子のない新しい .xib ファイルの名前) に値を設定します。 

    [![DockMenu 項目の追加](menu-images/dock09.png "DockMenu 項目の追加")](menu-images/dock09-large.png#lightbox)

これで、アプリケーションを実行すると、Dock に対応するアイコンを右クリックし、当社の新しいメニュー項目が表示されます。

![例を実行しているドック メニューの](menu-images/dock10.png "を実行しているドック メニューの例")

メニューからカスタムの項目のいずれかを選択しました、テキスト ビュー内のテキストが変更されます。

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>ポップアップのボタンをクリックし、プルダウン リスト

ポップアップのボタンでは、選択項目を表示し、ユーザーがクリックされたときから選択するオプションの一覧が表示されます。 プルダウン リストには、ポップアップのボタンが通常は、現在のタスクのコンテキストに固有のコマンドを選択するための一種です。 ウィンドウをどこにでも両方表示できます。

次の手順に従って、アプリケーションのポップアップのカスタム ボタンを作成してみましょう。

1. 編集、 **Main.storyboard** Xcode とドラッグでファイルを**ショートカット ボタン**から、**ライブラリ インスペクター**上に、**パネル**で作成したウィンドウ[コンテキスト メニュー](#Contextual_Menus)セクション。 

    [![ポップアップのボタンの追加](menu-images/popup01.png "ポップアップ ボタンの追加")](menu-images/popup01-large.png#lightbox)
2. 新しいメニュー項目を追加し、項目のタイトルを設定するためにポップアップで:**アドレス**、**日付**、 **Greeting**、および**署名** 

    [![メニュー項目を構成する](menu-images/popup02.png "メニュー項目を構成します。")](menu-images/popup02-large.png#lightbox)
3. 次で、カスタム メニュー用に作成した既存のアクションを新しいメニュー項目を接続しましょう、[追加、編集および削除するメニュー](#Adding,_Editing_and_Deleting_Menus)前のセクション。 切り替えて、**接続インスペクター**を選択し、**最初レスポンダー**で、**インターフェイス階層**。 下にスクロールし、検索、`phraseAddress:`アクション。 そのアクションの円から線をドラッグ、**アドレス**メニュー項目。 

    [![アクションの線をドラッグ](menu-images/popup03.png "アクションの線をドラッグします。")](menu-images/popup03-large.png#lightbox)
4. すべての他のメニュー項目にアタッチする、対応するアクションについて繰り返します。 

    [![必要なすべてのアクション](menu-images/popup04.png "必要なすべてのアクション")](menu-images/popup04-large.png#lightbox)
5. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

今すぐ場合は、アプリケーションを実行し、ポップアップから項目を選択、テキスト ビュー内のテキストが変更されます。

![例を実行しているポップアップの](menu-images/popup05.png "を実行しているポップアップの例")

作成し、ポップアップのボタンとまったく同じ方法でプルダウン リストを操作できます。 同じように、コンテキスト メニューの既存のアクションへのアタッチ、代わりに独自のカスタム アクションを作成することが、[コンテキスト メニュー](#Contextual_Menus)セクション。

## <a name="summary"></a>まとめ

この記事では、メニューとメニュー項目、Xamarin.Mac アプリケーションでの使用方法について詳しく説明をしました。 最初のコンテキスト メニューを作成しました次に、ステータス バーのメニューを検査およびカスタム メニューをドッキングし、アプリケーションのメニュー バー調査します。 最後に、ポップアップ メニューとプルの一覧を説明します。


## <a name="related-links"></a>関連リンク

- [MacMenus (サンプル)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ヒューマン インターフェイス ガイドライン - メニュー](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [アプリケーションのメニューおよびポップアップ リストの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
