---
title: Xamarin. Mac のメニュー
description: この記事では、Xamarin. Mac アプリケーションでのメニューの使用について説明します。 Xcode と Interface Builder でのメニューおよびメニュー項目の作成と管理、およびプログラムによる操作について説明します。
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 7a19b2e70ff18ae43cb65804c6c125890fa1851b
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/25/2019
ms.locfileid: "70770985"
---
# <a name="menus-in-xamarinmac"></a>Xamarin. Mac のメニュー

_この記事では、Xamarin. Mac アプリケーションでのメニューの使用について説明します。Xcode と Interface Builder でのメニューおよびメニュー項目の作成と管理、およびプログラムによる操作について説明します。_

Xamarin. Mac C#アプリケーションでおよび .net を使用する場合、開発者が目的の C と Xcode で作業するのと同じ cocoa メニューにアクセスできます。 Xcode は直接統合されているため、Xcode の Interface Builder を使用して、メニューバー、メニュー、およびメニュー項目を作成および管理できます (また、 C#必要に応じて、コード内で直接作成することもできます)。

メニューは、Mac アプリケーションのユーザーエクスペリエンスの不可欠な部分であり、一般的にユーザーインターフェイスのさまざまな部分に表示されます。

- **アプリケーションのメニューバー** -これは、すべての Mac アプリケーションの画面の上部に表示されるメインメニューです。
- **コンテキストメニュー** -ユーザーがウィンドウ内の項目を右クリックまたはコントロールクリックすると表示されます。
- **ステータスバー** -これは、アプリケーションのメニューバーの右端にある、(メニューバーの時計の左側に) 画面の上部に表示される領域です。項目が追加されると、左に拡大します。
- **Dock menu** -ユーザーがアプリケーションのアイコンを右クリックまたはコントロールクリックしたとき、またはユーザーがアイコンを左クリックしてマウスボタンを押したときに表示される、ドッキング内の各アプリケーションのメニュー。
- **ポップアップボタンとプルダウンリスト**-ポップアップボタンは選択された項目を表示し、ユーザーがクリックしたときに選択するオプションの一覧を表示します。 プルダウンリストは、現在のタスクのコンテキストに固有のコマンドを選択するために通常使用されるポップアップボタンの一種です。 どちらもウィンドウ内の任意の場所に表示できます。

[![メニューの例](menu-images/intro01.png "メニューの例")](menu-images/intro01-large.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでの Cocoa のメニューバー、メニュー、およびメニュー項目の操作の基本について説明します。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。この記事をご覧ください。

[Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの「[クラス/ C#メソッドを目的](~/mac/internals/how-it-works.md)として公開する」セクションを参照してください。 C#クラスをに接続する`Register`ため`Export`に使用される属性と属性についても説明します。目的 C オブジェクトと UI 要素。

## <a name="the-applications-menu-bar"></a>アプリケーションのメニューバー 

Windows OS で実行されるアプリケーションとは異なり、すべてのウィンドウに独自のメニューバーをアタッチできますが、macOS で実行されるすべてのアプリケーションには、そのアプリケーションのすべてのウィンドウで使用される画面の上部に表示されるメニューバーが1つあります。

[![メニューバー](menu-images/appmenu01.png "メニューバー")](menu-images/appmenu01-large.png#lightbox)

このメニューバーの項目は、現在のコンテキストまたはアプリケーションの状態とそのユーザーインターフェイスに基づいて、特定の時点でアクティブ化または非アクティブ化されます。 たとえば、ユーザーがテキストフィールドを選択した場合は、 **[編集]** メニューの **[コピー]** 、 **[切り取り]** などの項目が有効になります。

Apple によると、既定では、すべての macOS アプリケーションには、アプリケーションのメニューバーに表示されるメニューとメニュー項目の標準セットがあります。

- **Apple menu** -このメニューを使用すると、どのアプリケーションが実行されているかに関係なく、ユーザーが常に利用できるシステム全体の項目にアクセスできます。 これらの項目は、開発者が変更することはできません。
- **アプリメニュー** -このメニューには、アプリケーションの名前が太字で表示され、ユーザーは現在実行中のアプリケーションを特定できます。 これには、アプリケーションの終了など、特定のドキュメントやプロセスではなく、アプリケーション全体に適用される項目が含まれます。
- **[ファイル] メニュー** -アプリケーションが使用するドキュメントを作成、開く、または保存するために使用される項目。 アプリケーションがドキュメントベースでない場合は、このメニューの名前を変更したり、削除したりできます。
- **[編集] メニュー** -アプリケーションのユーザーインターフェイスの要素を編集または変更するために使用される**切り取り**、**コピー**、**貼り付け**などのコマンドを保持します。
- **[書式] メニュー** -アプリケーションがテキストで動作する場合、このメニューには、そのテキストの書式を調整するコマンドが表示されます。
- **[表示] メニュー** -アプリケーションのユーザーインターフェイスでコンテンツを表示 (表示) する方法に影響を与えるコマンドを保持します。
- **アプリケーション固有のメニュー** -これらは、アプリケーションに固有のすべてのメニューです (web ブラウザーの [ブックマーク] メニューなど)。 これらは、バーの **[表示]** メニューと [**ウィンドウ]** メニューの間に表示されます。
- [**ウィンドウ] メニュー** -アプリケーションでウィンドウを操作するためのコマンドと、現在開いているウィンドウの一覧が表示されます。
- **[ヘルプ] メニュー** -アプリケーションが画面に表示されるヘルプを表示する場合は、バーの一番右のメニューに [ヘルプ] メニューが表示されます。 

アプリケーションのメニューバーと標準メニューおよびメニュー項目の詳細については、「Apple の[ヒューマンインターフェイスのガイドライン](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)」を参照してください。

### <a name="the-default-application-menu-bar"></a>既定のアプリケーションのメニューバー

新しい Xamarin. Mac プロジェクトを作成するたびに、標準の既定のアプリケーションメニューバーが自動的に取得されます。これには、前のセクションで説明したように、macOS アプリケーションで通常使用される一般的な項目があります。 アプリケーションの既定のメニューバーは、(アプリの UI の残りの部分と共に)**メインのストーリーボード**ファイルで、 **Solution Pad**のプロジェクトの下に定義されます。  

![メインのストーリーボードを選択します](menu-images/appmenu02.png "メインのストーリーボードを選択します")

Xcode の Interface Builder で編集するために**メインのストーリーボード**ファイルをダブルクリックすると、メニューエディターのインターフェイスが表示されます。

[![Xcode で UI を編集する](menu-images/defaultbar01.png "Xcode で UI を編集する")](menu-images/defaultbar01-large.png#lightbox)

ここでは、 **[ファイル]** メニューの メニューを **[開く]** 項目などの項目をクリックし、**属性インスペクター**でそのプロパティを編集または調整できます。

[![メニューの属性の編集](menu-images/defaultbar02.png "メニューの属性の編集")](menu-images/defaultbar02-large.png#lightbox)

メニューと項目の追加、編集、および削除については、この記事の後半で説明します。 ここでは、既定で使用できるメニューとメニュー項目、および定義済みの一連のアウトレットとアクションを使用してコードに自動的に公開される方法を確認するだけです (詳細については[、アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するドキュメントを参照してください)。

たとえば、 **[開く]** メニュー項目`openDocument:`の**接続インスペクター**をクリックすると、自動的にアクションに接続されていることがわかります。 

[![添付されたアクションの表示](menu-images/defaultbar03.png "添付されたアクションの表示")](menu-images/defaultbar03-large.png#lightbox)

**インターフェイス階層**で**最初のレスポンダー**を選択し、**接続インスペクター**を下にスクロールすると、[開く] メニュー項目がアタッチ`openDocument:`されているアクションの定義 (と共に表示されます) が表示されます。アプリケーションのその他の既定のアクションは、自動的にコントロールに接続されません。

[![すべてのアタッチされたアクションを表示]する(menu-images/defaultbar04.png "すべてのアタッチされたアクションを表示")する](menu-images/defaultbar04-large.png#lightbox) 

なぜこれが重要なのでしょうか。 次のセクションでは、これらの自動的に定義されたアクションが他の Cocoa ユーザーインターフェイス要素と連携して、メニュー項目を自動的に有効または無効にしたり、項目の組み込み機能を提供したりする方法について説明します。

後で、これらの組み込みアクションを使用して、コードの項目を有効または無効にし、選択されたときに独自の機能を提供します。

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>組み込みメニュー機能

UI 項目またはコードを追加する前に新しく作成した Xamarin. Mac アプリケーションを実行すると、いくつかの項目が自動的に接続され、自動的に有効になります (たとえば、の**Quit** 項目)。**アプリ**メニュー:

![有効なメニュー項目](menu-images/appmenu03.png "有効なメニュー項目")

**切り取り**、**コピー**、**貼り付け**などの他のメニュー項目は次のようにはなりません。

![無効なメニュー項目](menu-images/appmenu04.png "無効なメニュー項目")

ここでは、アプリケーションを停止し、 **Solution Pad**の**メインのストーリーボード**ファイルをダブルクリックして、Xcode の Interface Builder で編集するために開きます。 次に、**インターフェイスエディター**で、**ライブラリ**の**テキストビュー**をウィンドウのビューコントローラーにドラッグします。

[![ライブラリからのテキストビューの選択](menu-images/appmenu05.png "ライブラリからのテキストビューの選択")](menu-images/appmenu05-large.png#lightbox)

**[制約エディター]** で、テキストビューをウィンドウの端にピン留めし、エディターの上部にある4つのすべての赤の I ビームをクリックし、4 つの **[制約の追加]** ボタンをクリックして、ウィンドウを拡大または縮小するように設定します。

[![制約の編集](menu-images/appmenu06.png "制約の編集")](menu-images/appmenu06-large.png#lightbox)

変更内容をユーザーインターフェイスのデザインに保存し、Visual Studio for Mac を切り替えて、変更を Xamarin プロジェクトと同期します。 ここで、アプリケーションを起動し、テキストビューにテキストを入力して選択し、 **[編集]** メニューを開きます。

![メニュー項目は自動的に有効または無効]になります。(menu-images/appmenu07.png "メニュー項目は自動的に有効または無効")になります。

**切り取り**、**コピー**、**貼り付け**の各項目が自動的に有効になり、完全に機能することに注意してください。1行のコードを記述する必要はありません。 

ここで何が起こっているんですか。 組み込みの事前定義されたアクションは、既定のメニュー項目 (前述のとおり) に接続されるため、macOS に含まれる Cocoa のユーザーインターフェイス要素のほとんどは、特定のアクション ( `copy:`など) へのフックを構築しています。 そのため、ウィンドウに追加され、アクティブで、選択されると、対応するメニュー項目またはそのアクションに関連付けられている項目が自動的に有効になります。 ユーザーがそのメニュー項目を選択すると、UI 要素に組み込まれている機能が呼び出され、実行されます。開発者が介入する必要はありません。

### <a name="enabling-and-disabling-menus-and-items"></a>メニューと項目の有効化と無効化

既定では、ユーザーイベントが発生するたび`NSMenu`に、は、アプリケーションのコンテキストに基づいて、表示される各メニューおよびメニュー項目を自動的に有効または無効にします。 項目を有効または無効にするには、次の3つの方法があります。

- **自動メニューの有効化**-項目が接続され`NSMenu`ているアクションに応答する適切なオブジェクトがによって検出された場合に、メニュー項目が有効になります。 たとえば、上のテキストビューには、 `copy:`アクションへのフックが組み込まれています。
- **カスタムアクションと validateMenuItem:** -[ウィンドウまたはビューコントローラーのカスタムアクション](#Working-with-Custom-Window-Actions)にバインドされているすべてのメニュー項目につい`validateMenuItem:`て、アクションを追加し、メニュー項目を手動で有効または無効にすることができます。
- **手動メニューの有効化**-各メニューの`Enabled`各項目を`NSMenuItem`個別に有効または無効にするには、各プロパティを手動で設定します。

システムを選択するには`AutoEnablesItems` `NSMenu`、のプロパティを設定します。 `true`は自動 (既定の動作) で`false`あり、手動です。 

> [!IMPORTANT]
> 手動メニューの有効化を選択した場合は、のような`NSTextView`appkit クラスによって制御されていても、どのメニュー項目も自動的に更新されません。 コード内のすべての項目を手動で有効または無効にする責任があります。

#### <a name="using-validatemenuitem"></a>ValidateMenuItem の使用

前述のように、[ウィンドウまたはビューコントローラーのカスタムアクション](#Working-with-Custom-Window-Actions)にバインドされているすべてのメニュー項目に`validateMenuItem:`対して、アクションを追加し、メニュー項目を手動で有効または無効にすることができます。

次の例`Tag`では、プロパティを使用して、内の選択したテキスト`NSTextView`の状態に基づいて、 `validateMenuItem:`アクションによって有効または無効にされるメニュー項目の種類を決定します。 プロパティ`Tag`は、各メニュー項目の Interface Builder で設定されています。

![Tag プロパティの設定](menu-images/validate01.png "Tag プロパティの設定")

次のコードがビューコントローラーに追加されます。

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

このコードが実行され、で`NSTextView`テキストが選択されていない場合、2つの wrap メニュー項目は無効になります (ビューコントローラー上のアクションに接続されている場合でも)。

![無効化]された項目の表示(menu-images/validate02.png "無効化")された項目の表示

テキストのセクションが選択され、メニューが再び開くと、2つの wrap メニュー項目が使用できるようになります。

![表示 (有効な項目]を)(menu-images/validate03.png "表示 (有効な項目")を)

## <a name="enabling-and-responding-to-menu-items-in-code"></a>コード内のメニュー項目の有効化と応答

前述のように、特定の Cocoa ユーザーインターフェイス要素を UI デザイン (テキストフィールドなど) に追加するだけで、いくつかの既定のメニュー項目が自動的に有効になり、コードを記述しなくても自動的に機能します。 次に、独自C#のコードを Xamarin. Mac プロジェクトに追加して、メニュー項目を有効にし、ユーザーが選択したときに機能を提供できるようにします。

たとえば、ユーザーが **[ファイル]** メニューの **[開く]** 項目を使用してフォルダーを選択できるようにするとします。 これはアプリケーション全体の機能であるため、ウィンドウまたは UI 要素を指定できるようにするため、アプリケーションデリゲートに対して処理するコードを追加します。

**Solution Pad**で、 `AppDelegate.CS`ファイルをダブルクリックして開き、編集します。

![アプリのデリゲートを選択する](menu-images/appmenu08.png "アプリのデリゲートを選択する")

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

ここでアプリケーションを実行し、 **[ファイル]** メニューを開きます。 

![[ファイル] メニュー](menu-images/appmenu09.png "[ファイル] メニュー")

[メニューを**開く**] 項目が有効になっていることを確認します。 これを選択すると、[開く] ダイアログボックスが表示されます。

![開い]ているダイアログ(menu-images/appmenu10.png "開い")ているダイアログ

**[開く]** ボタンをクリックすると、アラートメッセージが表示されます。

![ダイアログメッセージの例](menu-images/appmenu11.png "ダイアログメッセージの例")

ここでのキー行`[Export ("openDocument:")]`は、 **appdelegate**に`openDocument:`アクションに応答するメソッド`void OpenDialog (NSObject sender)`があることを示し`NSMenu`ています。 上記の手順を忘れた場合、 **[開く]** メニュー項目は、既定では Interface Builder で自動的にこのアクションに接続されます。

[![アタッチされたアクションの表示](menu-images/defaultbar03.png "アタッチされたアクションの表示")](menu-images/defaultbar03-large.png#lightbox)

次に、独自のメニュー、メニュー項目、およびアクションを作成し、コードでそれらに応答する方法を見てみましょう。

### <a name="working-with-the-open-recent-menu"></a>開いている [最近使ったもの] メニューを操作する

既定では、 **[ファイル]** メニューには、ユーザーがアプリを使用して開いた最後のいくつかのファイルを追跡する、**最近開い**た項目が表示されます。 ベースの Xamarin. Mac `NSDocument`アプリを作成する場合、このメニューは自動的に処理されます。 その他の種類の Xamarin. Mac アプリの場合は、このメニュー項目の管理と応答を手動で行う必要があります。

[**最近使っ**たもの] メニューを手動で処理するには、まず次のものを使用して新しいファイルが開かれたか、保存されたことを通知する必要があります。

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

アプリがを`NSDocuments`使用していない場合でも、を`SharedDocumentController` `NSDocumentController`使用して、ファイル`NoteNewRecentDocumentURL`の場所が`NSUrl`のをに送信することにより、[**最近**使ったもの] メニューを維持します。

次に、[**最近使っ**た`OpenFile`もの] メニューからユーザーが選択したファイルを開くために、アプリのデリゲートのメソッドをオーバーライドする必要があります。 次に例を示します。

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

ファイル`true`を開くことができる場合はを返し`false`ます。それ以外の場合はを返し、ファイルを開くことができなかったことを示す警告がユーザーに表示されます。

**[最近使っ**たもの] メニューから返されるファイル名とパスにはスペースが含まれる場合があるため、を`NSUrl`作成する前にこの文字を適切にエスケープする必要があります。そうしないと、エラーが発生します。 これを行うには、次のコードを使用します。

```csharp
filename = filename.Replace (" ", "%20");
```

最後に、ファイルを`NSUrl`指すを作成し、app delegate のヘルパーメソッドを使用して新しいウィンドウを開き、そこにファイルを読み込みます。

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

すべてをまとめて取得するには、 **AppDelegate.cs**ファイルで実装の例を見てみましょう。

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

アプリの要件に基づいて、ユーザーが複数のウィンドウで同じファイルを同時に開くことがないようにすることができます。 このサンプルアプリでは、ユーザーが既に開いているファイル ([**最近開い**たファイル] または **[開く**]) を選択した場合。 メニュー項目)。ファイルを含むウィンドウが前面に表示されます。

これを実現するために、次のコードをヘルパーメソッドで使用しています。

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

`ViewController`クラスは、 `Path`プロパティ内のファイルへのパスを保持するように設計されています。 次に、アプリで現在開いているすべてのウィンドウをループします。 ファイルが既にいずれかのウィンドウで開かれている場合は、次を使用して、他のすべてのウィンドウの前面に移動します。

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

一致するものが見つからない場合は、ファイルが読み込まれた状態で新しいウィンドウが開き、[**最近使っ**たもの] メニューにファイルが表示されます。

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

### <a name="working-with-custom-window-actions"></a>カスタムウィンドウアクションの操作

標準メニュー項目に事前に接続されている、組み込みの**最初の応答側**アクションと同じように、新しいカスタムアクションを作成し、それらを Interface Builder のメニュー項目に接続することができます。

まず、アプリのウィンドウコントローラーの1つにカスタムアクションを定義します。 次に例を示します。

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

次に、 **Solution Pad**でアプリのストーリーボードファイルをダブルクリックして、Xcode の Interface Builder で編集するために開きます。 **アプリケーションシーン**の**最初のレスポンダー**を選択し、[属性]**インスペクター**に切り替えます。

![属性インスペクター](menu-images/action01.png "属性インスペクター")

[属性 **+** ]**インスペクター**の下部にあるボタンをクリックして、新しいカスタムアクションを追加します。

![新しいアクションの追加](menu-images/action02.png "新しいアクションの追加")

ウィンドウコントローラーに作成したカスタムアクションと同じ名前を付けます。

![アクション名の編集](menu-images/action03.png "アクション名の編集")

コントロールをクリックし、メニュー項目から**アプリケーションシーン**の**最初の応答側**にドラッグします。 ポップアップリストから、先ほど作成した新しいアクション (`defineKeyword:`この例では) を選択します。

![アクションのアタッチ](menu-images/action04.png "アクションのアタッチ")

変更をストーリーボードに保存し Visual Studio for Mac に戻り、変更を同期します。 アプリを実行すると、カスタムアクションを接続したメニュー項目が自動的に有効または無効になり (操作が開かれているウィンドウに基づいて)、メニュー項目を選択すると、その操作が実行されます。

[![新しいアクションのテスト](menu-images/action05.png "新しいアクションのテスト")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>メニューの追加、編集、および削除

前のセクションで説明したように、Xamarin. Mac アプリケーションには、特定の UI コントロールが自動的にアクティブ化して応答するメニューとメニュー項目があらかじめ設定されています。 また、アプリケーションにコードを追加して、これらの既定の項目を有効にし、応答する方法についても説明しました。

このセクションでは、必要のないメニュー項目を削除し、メニューを再構成し、新しいメニュー、メニュー項目、およびアクションを追加する方法を説明します。

**Solution Pad**のメインの**ストーリーボード**ファイルをダブルクリックして、編集用に開きます。

[![Xcode で UI を編集する](menu-images/maint01.png "Xcode で UI を編集する")](menu-images/maint01-large.png#lightbox)

特定の Xamarin. Mac アプリケーションでは、既定の **[表示]** メニューを使用しないので、削除します。 **インターフェイス階層**で、メインメニューバーの一部である **[表示]** メニュー項目を選択します。

[![表示] メニュー項目の選択][(menu-images/maint02.png "表示] メニュー項目の選択")

Del キーまたは backspace キーを押して、メニューを削除します。 次に、 **[書式]** メニューのすべての項目を使用するのではなく、使用しようとしている項目をサブメニューから移動します。 **インターフェイス階層**で、次のメニュー項目を選択します。

![複数の項目の強調表示](menu-images/maint03.png "複数の項目の強調表示")

現在のサブメニューから親**メニュー**の下に項目をドラッグします。

[![親メニューへのメニュー項目のドラッグ](menu-images/maint04.png "親メニューへのメニュー項目のドラッグ")](menu-images/maint04-large.png#lightbox)

メニューは次のようになります。

[![新しい場所の項目](menu-images/maint05.png "新しい場所の項目")](menu-images/maint05-large.png#lightbox)

次に、 **[書式]** メニューの **[テキスト]** サブメニューをドラッグして、 **[書式]** メニューと [**ウィンドウ]** メニューの間のメインメニューバーに配置します。

[![テキストメニュー](menu-images/maint06.png "テキストメニュー")](menu-images/maint06-large.png#lightbox)

**[書式]** メニューに戻り、 **[フォント]** サブメニュー項目を削除してみましょう。 次に、 **[書式]** メニューを選択し、"Font" という名前に変更します。

[![フォント メニュー](menu-images/maint07.png "フォント メニュー")](menu-images/maint07-large.png#lightbox)

次に、選択されたときにテキストビュー内のテキストに自動的に追加される、事前定義の語句のカスタムメニューを作成してみましょう。 **ライブラリインスペクター**の下部にある [検索] ボックスに、「menu」と入力します。 これにより、すべてのメニュー UI 要素を簡単に見つけて使用できるようになります。

![ライブラリインスペクター](menu-images/maint08.png "ライブラリインスペクター")

次に、メニューを作成してみましょう。

1. **メニュー項目**を**ライブラリインスペクター**から、 **[テキスト]** メニューと **[ウィンドウ]** メニューの間のメニューバーにドラッグします。 

    ![ライブラリ内の新しいメニュー項目を選択]する(menu-images/maint10.png "ライブラリ内の新しいメニュー項目を選択")する
2. 項目 "語句" の名前を変更します。 

    [![メニュー名の設定](menu-images/maint09.png "メニュー名の設定")](menu-images/maint09-large.png#lightbox)
3. 次に、**ライブラリインスペクター**から**メニュー**をドラッグします。 

    ![ライブラリからメニューを選択する](menu-images/maint11.png "ライブラリからメニューを選択する")
4. 作成した新しい**メニュー項目**の**メニュー**をドロップし、名前を "語句" に変更します。 

    [![メニュー名の編集](menu-images/maint12.png "メニュー名の編集")](menu-images/maint12-large.png#lightbox)
5. 次に、3つの既定の**メニュー項目**"Address"、"Date"、"挨拶" の名前を変更してみましょう。 

    [![[語句] メニュー](menu-images/maint13.png "[語句] メニュー")](menu-images/maint13-large.png#lightbox)
6. 次に、**ライブラリインスペクター**から**メニュー項目**をドラッグして "Signature" を呼び出すことによって、4番目の**メニュー項目**を追加してみましょう。 

    [![メニュー項目名の編集](menu-images/maint14.png "メニュー項目名の編集")](menu-images/maint14-large.png#lightbox)
7. 変更内容をメニューバーに保存します。

次に、新しいメニュー項目がコードにC#公開されるように、一連のカスタムアクションを作成してみましょう。 Xcode let で**アシスタント**ビューに切り替えます。

[![必要なアクションの作成](menu-images/maint15.png "必要なアクションの作成")](menu-images/maint15-large.png#lightbox)

次の手順を実行してみましょう。

1. **[アドレス]** メニュー項目から**appdelegate .h**ファイルにコントロールをドラッグします。
2. **接続**の種類を**操作**に切り替えます。 

    [![アクションの種類の選択](menu-images/maint17.png "アクションの種類の選択")](menu-images/maint17-large.png#lightbox)
3. "PhraseAddress" という**名前**を入力し、 **[接続]** ボタンを押して新しいアクションを作成します。 

    [![アクションの構成](menu-images/maint18.png "アクションの構成")](menu-images/maint18-large.png#lightbox)
4. **日付**、**あいさつ**、**署名**の各メニュー項目に対して上記の手順を繰り返します。 

    [![完了]したアクション(menu-images/maint19.png "完了")したアクション](menu-images/maint19-large.png#lightbox)
5. 変更内容をメニューバーに保存します。

次に、テキストビューのアウトレットを作成して、コードからコンテンツを調整できるようにする必要があります。 **アシスタントエディター**で`documentText` **viewcontroller .h**ファイルを選択し、という名前の新しいアウトレットを作成します。

[![アウトレットの作成](menu-images/maint20.png "アウトレットの作成")](menu-images/maint20-large.png#lightbox)

Visual Studio for Mac に戻り、Xcode から変更を同期します。 次に、 **ViewController.cs**ファイルを編集し、次のように表示します。

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

これにより、テキストビューのテキストが`ViewController`クラスの外部に公開され、ウィンドウがフォーカスを取得したとき、またはフォーカスを失ったときに、アプリのデリゲートが通知されます。 ここで、 **AppDelegate.cs**ファイルを編集し、次のように表示します。

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

ここでは、Interface Builder `AppDelegate`で定義したアクションとアウトレットを使用できるように、部分クラスを作成しました。 また、を公開`textEditor`して、どのウィンドウが現在フォーカスされているかを追跡します。

次のメソッドは、カスタムメニューおよびメニュー項目を処理するために使用されます。

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

ここでアプリケーションを実行すると、 **[語句]** メニュー内のすべての項目がアクティブになり、選択したときにテキストビューに次の語句が追加されます。

![実行中のアプリの例](menu-images/maint21.png "実行中のアプリの例")

アプリケーションのメニューバーの操作の基本を説明したので、次はカスタムコンテキストメニューを作成してみましょう。

### <a name="creating-menus-from-code"></a>作成 (コードからメニューを)

Xcode の Interface Builder でメニューとメニュー項目を作成するだけでなく、コードからメニュー、サブメニュー、またはメニュー項目を作成、変更、または削除する必要がある場合もあります。

次の例では、クラスを作成し、その場で動的に作成されるメニュー項目とサブメニューに関する情報を保持します。

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

このクラスが定義されている場合、次のルーチン`LanguageFormatCommand`は、オブジェクトのコレクションを解析し、新しいメニューとメニュー項目を (Interface Builder で作成された) 既存のメニューの下部に追加することによって再帰的に作成します。

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

`LanguageFormatCommand` 空`Title`のプロパティを持つオブジェクトの場合、このルーチンはメニューセクションの間に**区切りのメニュー項目**(細い灰色の線) を作成します。

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

タイトルが指定されている場合は、そのタイトルの新しいメニュー項目が作成されます。

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

オブジェクトに`LanguageFormatCommand`子`LanguageFormatCommand`オブジェクトが含まれている場合は、サブ`AssembleMenu`メニューが作成され、そのメニューを構築するためにメソッドが再帰的に呼び出されます。

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

サブメニューのない新しいメニュー項目については、ユーザーが選択したメニュー項目を処理するコードが追加されます。

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>メニュー作成のテスト

上記のすべてのコードを配置し、次のオブジェクトの`LanguageFormatCommand`コレクションが作成された場合。

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Strong","**","**"));
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
FormattingCommands.Add(new LanguageFormatCommand("Image","![](",")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[![](",")](LinkImageHere)"));
```

このコレクションは、([ `AssembleMenu` **書式**] メニューがベースとして設定された) 関数に渡され、次の動的メニューとメニュー項目が作成されます。

![実行中のアプリの新しいメニュー項目](menu-images/dynamic01.png "実行中のアプリの新しいメニュー項目")

#### <a name="removing-menus-and-items"></a>メニューと項目の削除

アプリのユーザーインターフェイスからメニューまたはメニュー項目を削除する必要がある場合は、削除する`RemoveItemAt`項目の 0 `NSMenu`から始まるインデックスを指定するだけで、クラスのメソッドを使用できます。

たとえば、上記のルーチンによって作成されたメニューとメニュー項目を削除するには、次のコードを使用します。

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

上記のコードの場合は、最初の4つのメニュー項目が Xcode の Interface Builder と aways に作成され、アプリで使用できるようになるため、動的に削除されません。

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>コンテキストメニュー

コンテキストメニューは、ユーザーがウィンドウ内の項目を右クリックまたはコントロールクリックしたときに表示されます。 既定では、macOS に組み込まれているいくつかの UI 要素には、コンテキストメニュー (テキストビューなど) が既にアタッチされています。 ただし、ウィンドウに追加した UI 要素に対して独自のカスタムコンテキストメニューを作成することが必要になる場合があります。

Xcode でメインの**ストーリーボード**ファイルを編集し、**ウィンドウ**ウィンドウを**id インスペクター**で "NSPanel" に設定し、新しい**アシスタント** **項目を [** **ウィンドウ**] メニューに追加して、それを新しいにアタッチしてみましょう。**Show セグエ**を使用しているウィンドウ:

[![セグエ型の設定](menu-images/context01.png "セグエ型の設定")](menu-images/context01-large.png#lightbox)

次の手順を実行してみましょう。

1. **ライブラリインスペクター**から**パネル**ウィンドウに**ラベル**をドラッグし、そのテキストを "Property" に設定します。 

    [![ラベルの値の編集](menu-images/context03.png "ラベルの値の編集")](menu-images/context03-large.png#lightbox)
2. 次に、**ライブラリインスペクター**からビュー階層のビューコントローラーに**メニュー**をドラッグし、3つの既定のメニュー項目**ドキュメント**、**テキスト**、**フォント**の名前を変更します。

    [![必須のメニュー項目](menu-images/context02.png "必須のメニュー項目")](menu-images/context02-large.png#lightbox)
3. 次に、**プロパティラベル**から**メニュー**にコントロールをドラッグします。

    [![ドラッグしてセグエを作成する](menu-images/context04.png "ドラッグしてセグエを作成する")](menu-images/context04-large.png#lightbox)
4. ポップアップダイアログから、[**メニュー]** を選択します。 

    ![セグエ型の設定](menu-images/context05.png "セグエ型の設定")
5. **Id インスペクター**から、ビューコントローラーのクラスを "パネル viewcontroller" に設定します。 

    [![セグエクラスの設定](menu-images/context10.png "セグエクラスの設定")](menu-images/context10-large.png#lightbox)
6. 同期する Visual Studio for Mac に戻り、Interface Builder に戻ります。
7. **アシスタントエディター**に切り替えて、[**パネル**ファイル] を選択します。
8. という名前`propertyDocument`の**ドキュメント**メニュー項目のアクションを作成します。 

    [![アクションの構成](menu-images/context06.png "アクションの構成")](menu-images/context06-large.png#lightbox)
9. 残りのメニュー項目に対して、操作の作成を繰り返します。 

    [![必要な操作](menu-images/context07.png "必要な操作")](menu-images/context07-large.png#lightbox)
10. 最後に、という`propertyLabel`**プロパティラベル**のアウトレットを作成します。 

    [![アウトレットの構成](menu-images/context08.png "アウトレットの構成")](menu-images/context08-large.png#lightbox)
11. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

**PanelViewController.cs**ファイルを編集し、次のコードを追加します。

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

ここで、アプリケーションを実行し、パネルでプロパティラベルを右クリックすると、カスタムコンテキストメニューが表示されます。 メニューから項目を選択すると、ラベルの値が変更されます。

![実行中のコンテキストメニュー](menu-images/context09.png "実行中のコンテキストメニュー")

次に、ステータスバーのメニューを作成する方法を見てみましょう。

## <a name="status-bar-menus"></a>ステータスバーのメニュー

ステータスバーメニューには、メニューやアプリケーションの状態を反映した画像など、ユーザーに対してまたはフィードバックとの対話を行うステータスメニュー項目のコレクションが表示されます。 アプリケーションがバックグラウンドで実行されている場合でも、アプリケーションのステータスバーメニューが有効になり、アクティブになります。 システム全体のステータスバーは、アプリケーションのメニューバーの右側にあり、macOS で現在利用可能なステータスバーのみです。

**AppDelegate.cs**ファイルを編集して、メソッドを`DidFinishLaunching`次のようにしてみましょう。

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

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;`システム全体のステータスバーへのアクセスを提供します。 `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);`新しいステータスバー項目を作成します。 ここでは、メニューと多数のメニュー項目を作成し、作成したステータスバー項目にメニューを添付します。 

アプリケーションを実行すると、新しいステータスバー項目が表示されます。 メニューから項目を選択すると、テキストビューのテキストが変更されます。 

![実行中のステータスバーメニュー](menu-images/statusbar01.png "実行中のステータスバーメニュー")

次に、カスタムドックメニュー項目を作成する方法を見てみましょう。

## <a name="custom-dock-menus"></a>カスタムドックメニュー

ユーザーがドッキング中のアプリケーションのアイコンを右クリックまたはコントロールクリックすると、Mac アプリケーションの dock メニューが表示されます。

![カスタムドックメニュー](menu-images/dock01.png "カスタムドックメニュー")

次の手順に従って、アプリケーションのカスタムドックメニューを作成してみましょう。

1. Visual Studio for Mac で、アプリケーションのプロジェクトを右クリックし、[新しいファイルの**追加** >  **...** ] を選択します。[新しいファイル] ダイアログで、[ **Xamarin. Mac** > **空のインターフェイス定義**] を選択し、**名前**として "DockMenu" を使用します。次に、 **[新規]** ボタンをクリックして、新しい**DockMenu xib**ファイルを作成します。

    ![空のインターフェイス定義の追加](menu-images/dock02.png "空のインターフェイス定義の追加")
2. **Solution Pad**で、 **DockMenu xib**ファイルをダブルクリックして、Xcode で編集するために開きます。 次の項目を使用して、新しい**メニュー**を作成します。**住所**、**日付**、**あいさつ**、**署名** 

    [![UI のレイアウト](menu-images/dock03.png "UI のレイアウト")](menu-images/dock03-large.png#lightbox)
3. 次に、前の「[メニューの追加、編集、および削除](#Adding,_Editing_and_Deleting_Menus)」セクションでカスタムメニュー用に作成した既存のアクションに、新しいメニュー項目を接続してみましょう。 **接続インスペクター**に切り替えて、**インターフェイス階層**内の**最初のレスポンダー**を選択します。 下にスクロールして`phraseAddress:` 、アクションを見つけます。 そのアクションの円の線を **[アドレス]** メニュー項目にドラッグします。

    [![ドラッグしてアクション]を接続する(menu-images/dock04.png "ドラッグしてアクション")を接続する](menu-images/dock04-large.png#lightbox)
4. 他のすべてのメニュー項目についても繰り返して、対応するアクションにアタッチします。 

    [![必要な操作](menu-images/dock05.png "必要な操作")](menu-images/dock05-large.png#lightbox)
5. 次に、**インターフェイス階層**で**アプリケーション**を選択します。 **接続インスペクター**で、 `dockMenu`コンセントの円から、先ほど作成したメニューに線をドラッグします。

    [![ケーブルをコンセントにドラッグする](menu-images/dock06.png "ケーブルをコンセントにドラッグする")](menu-images/dock06-large.png#lightbox)
6. 変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。
7. ファイルをダブルクリックして開き、編集**し**ます。 

    [![Info.plist ファイルの編集](menu-images/dock07.png "Info.plist ファイルの編集")](menu-images/dock07-large.png#lightbox)
8. 画面の下部にある **[ソース]** タブをクリックします。 

    [![ソースビューの選択](menu-images/dock08.png "ソースビューの選択")](menu-images/dock08-large.png#lightbox)
9. **[新しいエントリの追加]** をクリックし、緑色のプラスボタンをクリックします。次に、プロパティ名を "AppleDockMenu" に設定し、値を "DockMenu" (拡張子のない xib ファイルの名前) に設定します。 

    [![DockMenu 項目の追加](menu-images/dock09.png "DockMenu 項目の追加")](menu-images/dock09-large.png#lightbox)

ここで、アプリケーションを実行し、Dock のアイコンを右クリックすると、新しいメニュー項目が表示されます。

![実行中のドッキングメニューの例](menu-images/dock10.png "実行中のドッキングメニューの例")

メニューからカスタム項目のいずれかを選択すると、テキストビューのテキストが変更されます。

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>ポップアップボタンとプルダウンリスト

ポップアップボタンは選択された項目を表示し、ユーザーがクリックしたときに選択するオプションの一覧を表示します。 プルダウンリストは、現在のタスクのコンテキストに固有のコマンドを選択するために通常使用されるポップアップボタンの一種です。 どちらもウィンドウ内の任意の場所に表示できます。

次の手順に従って、アプリケーションのカスタムポップアップボタンを作成します。

1. Xcode の**メインのストーリーボード**ファイルを編集し、[**ライブラリ] インスペクター**から[コンテキストメニュー](#Contextual_Menus)セクションで作成した**パネル**ウィンドウに**ポップアップボタン**をドラッグします。 

    [![ポップアップボタンの追加](menu-images/popup01.png "ポップアップボタンの追加")](menu-images/popup01-large.png#lightbox)
2. 新しいメニュー項目を追加し、ポップアップの項目のタイトルを次のように設定します。**住所**、**日付**、**あいさつ**、**署名** 

    [![メニュー項目の構成](menu-images/popup02.png "メニュー項目の構成")](menu-images/popup02-large.png#lightbox)
3. 次に、前の「[メニューの追加、編集、および削除](#Adding,_Editing_and_Deleting_Menus)」セクションでカスタムメニュー用に作成した既存のアクションに、新しいメニュー項目を接続してみましょう。 **接続インスペクター**に切り替えて、**インターフェイス階層**内の**最初のレスポンダー**を選択します。 下にスクロールして`phraseAddress:` 、アクションを見つけます。 そのアクションの円の線を **[アドレス]** メニュー項目にドラッグします。 

    [![ドラッグしてアクション]を接続する(menu-images/popup03.png "ドラッグしてアクション")を接続する](menu-images/popup03-large.png#lightbox)
4. 他のすべてのメニュー項目についても繰り返して、対応するアクションにアタッチします。 

    [![すべての必要なアクション](menu-images/popup04.png "すべての必要なアクション")](menu-images/popup04-large.png#lightbox)
5. 変更を保存し、Visual Studio for Mac に戻って Xcode と同期します。

ここで、アプリケーションを実行してポップアップから項目を選択すると、テキストビューのテキストが次のように変更されます。

![実行中のポップアップの例](menu-images/popup05.png "実行中のポップアップの例")

プルダウンリストの作成と操作は、ポップアップボタンとまったく同じ方法で行うことができます。 既存のアクションにアタッチする代わりに、[コンテキスト](#Contextual_Menus)メニューセクションのコンテキストメニューの場合と同様に、独自のカスタムアクションを作成することもできます。

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでメニューとメニュー項目を操作する方法について詳しく説明しました。 まず、アプリケーションのメニューバーを調べてから、コンテキストメニューを作成しました。次に、ステータスバーのメニューとカスタムドックメニューを調べました。 最後に、ポップアップメニューとプルダウンリストについて説明します。

## <a name="related-links"></a>関連リンク

- [MacMenus (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macmenus)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ヒューマンインターフェイスのガイドライン-メニュー](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [アプリケーションメニューとポップアップリストの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
