---
title: メニュー
description: この記事と Xamarin.Mac アプリケーションでメニューの作業について説明します。 これは、作成およびメニューとメニュー項目には、Xcode とインターフェイスのビルダーを維持し、それらのプログラムの操作について説明します。
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 50c9cf333ff7965bbdfbb964a2301e677eb6aa59
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="menus"></a>メニュー

_この記事と Xamarin.Mac アプリケーションでメニューの作業について説明します。これは、作成およびメニューとメニュー項目には、Xcode とインターフェイスのビルダーを維持し、それらのプログラムの操作について説明します。_

Xamarin.Mac アプリケーションでは、c# と .NET で作業するとき、OBJECTIVE-C、Xcode で作業する開発者は同じの Cocoa メニューへのアクセスがあります。 Xamarin.Mac は、Xcode と直接統合、ためには、Xcode のインターフェイス ビルダーを作成し、メニュー バー、メニューのおよびメニュー項目の管理 (または必要に応じて c# コードで直接作成すること) を使用することができます。

メニューは、Mac アプリケーションのユーザー エクスペリエンスの不可欠な部分し、通常、ユーザー インターフェイスのさまざまな部分に表示されます。

- **アプリケーションのメニュー バー** -これは、すべての Mac アプリケーションの画面の上部に表示されるメインのメニュー。
- **コンテキスト メニュー** -これらときに表示されるユーザーを右クリックしたコントロールのウィンドウで項目をクリックします。
- **ステータス バー** -これは、(メニュー バーのクロックの左側) に、画面の上部に表示され、左側が増えるにつれて拡張項目を追加するアプリケーション メニュー バーの右端にある領域にします。
- **メニューをドッキング**-ときにユーザーを右クリックしてまたはアプリケーションのアイコンをコントロール クリックすると、または、ユーザーがアイコンを左クリックして、マウス ボタンを押したままに表示される、ドッキング ステーション内の各アプリケーションのメニュー。
- **ポップアップのボタンをクリックし、プルダウンで表示されるリスト**-選択した項目を表示し、ポップアップ ボタンとは、ユーザーがクリックされたときからを選択するオプションの一覧が表示されます。 プルダウンで表示されるリストは、通常、現在のタスクのコンテキストに固有のコマンドを選択するために使用ポップアップのボタンの一種です。 ウィンドウで任意の場所両方とも表示できます。

[![例メニュー](menu-images/intro01.png "例メニュー")](menu-images/intro01-large.png#lightbox)

この記事で Cocoa メニュー バー、メニューのおよび Xamarin.Mac アプリケーションでのメニュー項目の操作の基礎について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`属性Objective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

## <a name="the-applications-menu-bar"></a>アプリケーションのメニュー バー 

すべてのウィンドウがそれに付随する独自のメニュー バーを持つことができます、Windows OS で実行されているアプリケーションとは異なり macOS で実行されているすべてのアプリケーションには、アプリケーションのウィンドウごとに使用される画面の上部に配置を実行する 1 つのメニュー バーがあります。

[![メニュー バー](menu-images/appmenu01.png "メニュー バー")](menu-images/appmenu01-large.png#lightbox)

このメニュー バー上の項目がアクティブまたは非アクティブ化された特定の時点で、現在のコンテキストやアプリケーションとそのユーザー インターフェイスの状態に基づきます。 例: 上の項目、ユーザーは、テキスト フィールドを選択する場合、**編集**メニューは、ように有効になっているものは**コピー**と**切り取り**です。

Apple に従ってし、既定では、macOS のすべてのアプリケーションは、メニューとアプリケーションのメニュー バーに表示されるメニュー項目の標準セットを持ちます。

- **Apple メニュー** -このメニューはどのようなアプリケーションが実行されているに関係なく、すべての時間を使用する場合は、ユーザーに提供されるワイドの項目システムへのアクセスを提供します。 これらの項目は、開発者によって変更できません。
- **アプリのメニュー** -このメニューは、アプリケーションの名前が太字で表示されますでき、ユーザー アプリケーションが現在実行中を識別します。 全体されません、特定のドキュメントまたはアプリケーションを終了などのプロセスとしてアプリケーションに適用される項目が含まれています。
- **[ファイル] メニュー** - 項目を使用して作成する、開くには、またはドキュメントを保存しますアプリケーションに処理します。 場合は、アプリケーションがドキュメントに基づく、このメニューを名前を変更または削除できます。
- **[編集] メニュー** -などのコマンドを保持**切り取り**、**コピー**、および**貼り付け**使用されるアプリケーションのユーザー インターフェイスで要素を編集または変更します。
- **[書式] メニュー** - 場合は、アプリケーションでは、そのテキストの書式設定を調整する保留コマンドをテキストでこのメニューに動作します。
- **[表示] メニュー** -影響を与えるコマンド コンテンツの表示方法、アプリケーションのユーザー インターフェイス (表示) を保持します。
- **アプリケーション固有のメニュー** -これらは、任意の web ブラウザーのブックマーク メニュー) など、アプリケーションに固有のメニュー。 間に表示する必要がありますが、**ビュー**と**ウィンドウ**バーのメニュー。
- **[ウィンドウ] メニュー** -現在開いているウィンドウの一覧と同様に、アプリケーションで windows を操作するコマンドが含まれています。
- **ヘルプ メニュー** -ヘルプ メニュー バーの右端のメニューをする必要があります、アプリケーションでは、画面に表示されるヘルプを提供する場合。 

アプリケーションのメニュー バー、標準のメニューおよびメニュー項目の詳細については、Apple を参照してください[ヒューマン インターフェイス ガイドライン](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)です。

### <a name="the-default-application-menu-bar"></a>既定のアプリケーションのメニュー バー

新しい Xamarin.Mac プロジェクトを作成するたびに、標準的な既定アプリケーション メニュー バーに macOS アプリケーション (前のセクションで説明したように) として通常必要のある一般的な項目があるが自動的に取得します。 アプリケーションの既定のメニュー バーがで定義されている、 **Main.storyboard** (と共に、アプリの UI の残りの部分) のプロジェクトの下のファイル、**ソリューション パッド**:  

![メインのストーリー ボードを選択して](menu-images/appmenu02.png "メインのストーリー ボードを選択")

ダブルクリックして、 **Main.storyboard**ファイル用に開けませんが、メニュー エディターのインターフェイスと Xcode のインターフェイスのビルダーでの編集が表示されます。

[![Xcode で UI を編集](menu-images/defaultbar01.png "Xcode で UI を編集")](menu-images/defaultbar01-large.png#lightbox)

ここからおできますで項目をクリックしてなど、**開く**のメニュー項目、**ファイル**メニュー編集やでそのプロパティを調整する、**属性インスペクター**:

[![メニューの属性の編集](menu-images/defaultbar02.png "メニューの属性の編集")](menu-images/defaultbar02-large.png#lightbox)

追加、編集、およびメニューおよびこの記事の後半の項目を削除するのには表示されます。 ようになりましたたいだけどのようなメニューおよびメニュー項目は既定で使用できるとどのように、公開されている自動的に定義済みのコンセントとアクションのセットを使用してコードを参照してください (詳細については、次を参照してくださいこの[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)。ドキュメント)。

たとえばをクリックした場合、**接続インスペクター**の**開く**メニュー項目の最大ワイヤードは自動的に確認できます、`openDocument:`アクション。 

[![接続されているアクションを表示する](menu-images/defaultbar03.png "接続されているアクションを表示します。")](menu-images/defaultbar03-large.png#lightbox)

選択した場合、**最初レスポンダー**で、**インターフェイス階層**で下へスクロールし、**接続インスペクター**の定義が表示されます、 `openDocument:`アクションを**開く**メニュー項目が (と共にいくつか他の既定の操作をアプリケーションは、コントロールまで自動的にケーブル接続されていません) に接続されています。

[![アタッチ先のすべてのアクションを表示する](menu-images/defaultbar04.png "接続されているすべてのアクションを表示します。")](menu-images/defaultbar04-large.png#lightbox) 

なぜこれが重要ですか。 次に、自動的に定義されているこれらのアクションが自動的に有効にしてメニュー項目を無効にするし、同様に、アイテムの組み込み機能を提供するには他の Cocoa ユーザー インターフェイス要素を操作する方法セクションが表示されます。

後で使用するこれらの組み込みアクションを有効にし、コードからの項目を無効にするには、選択した場合、独自の機能を提供します。

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>組み込みメニューの機能

場合は、実行、UI 項目やコードを追加する前に新しく作成された Xamarin.Mac アプリケーションかがわかりますをいくつかの項目は自動的にワイヤード (有線) アップおよびに対して有効にする (完全に機能と自動的に組み込み) など、 **Quit**内の項目、**アプリ**メニュー。

![有効なメニュー項目](menu-images/appmenu03.png "有効なメニュー項目")

など、他のメニュー項目を while**切り取り**、**コピー**、および**貼り付け**は。

![メニュー項目を無効になっている](menu-images/appmenu04.png "メニュー項目を無効になっています")

アプリケーションを停止し、ダブルクリックしてみましょう、 **Main.storyboard**ファイルで、**ソリューション パッド**を開くには、Xcode で編集するためのインターフェイスのビルダー。 次に、ドラッグ、**テキスト ビュー**から、**ライブラリ**でウィンドウのビューのコント ローラー上に、**インターフェイス エディター**:

[![ライブラリからテキスト ビューを選択すると](menu-images/appmenu05.png "ライブラリからテキスト ビューを選択します。")](menu-images/appmenu05-large.png#lightbox)

**制約エディター**みましょうウィンドウの端にテキスト ビューをピン留めし、設定、拡張して、すべて次の 4 つ赤いすれば-ビーム、エディターの上部にあるをクリックして、ウィンドウの縮小、 **4制約を追加**ボタンをクリックします。

[![制約を編集](menu-images/appmenu06.png "制約の編集")](menu-images/appmenu06-large.png#lightbox)

ユーザー インターフェイスのデザインに変更を保存し、Xamarin.Mac プロジェクトで、変更を同期するために Mac を Visual Studio に切り替えします。 今すぐアプリケーションを起動、テキスト ビューにテキストを入力、選択し、開く、**編集**メニュー。

![メニュー項目が自動的に有効/無効](menu-images/appmenu07.png "メニュー項目が自動的に有効/無効")

注意してください方法、**切り取り**、**コピー**と**貼り付け**項目は、1 行のコードを記述せずに自動的に有効になっているおよび完全に機能します。 

ここで何が起こっているんですか。 組み込みの (上記) と既定のメニュー項目までワイヤード (有線) あるアクションを事前に定義、特定のアクションにフック macOS の一部である Cocoa ユーザー インターフェイス要素のほとんどが組み込まれてに注意してください (など`copy:`)。 したがって、アクティブなウィンドウに追加および選択すると、対応するメニュー項目または項目にアタッチされている時そのアクションが自動的に有効です。 ユーザーは、そのメニュー項目を選択する場合は、UI 要素に組み込まれた機能と呼ばれ、開発者が関与させることも、実行します。

### <a name="enabling-and-disabling-menus-and-items"></a>有効にして、メニュー項目を無効にします。

既定では、ユーザー イベントが発生するたびに`NSMenu`自動的に有効にし、各表示されているメニューと項目に基づいて、アプリケーションのコンテキスト メニューを無効にします。 これには項目の有効/無効にする 3 つの方法があります。

- **自動メニューを有効にする**の場合は、メニュー項目が有効になっている`NSMenu`する項目は、ワイヤード (有線) アップをアクションに応答する適切なオブジェクトを検索することができます。 たとえば、テキスト上のビューにフックを組み込みのある、`copy:`アクション。
- **カスタム アクションと validateMenuItem:**にバインドされているすべてのメニュー項目の[ウィンドウまたはビューのコント ローラーのカスタム アクション](#Working-with-Custom-Window-Actions)、追加することができます、`validateMenuItem:`アクションと手動で有効にするにまたはメニュー項目を無効にします。
- **手動でのメニューを有効にする**-手動で設定する、`Enabled`の各プロパティ`NSMenuItem`を有効にするにまたはメニュー内の各項目を個別に無効にします。

選択するには、システム設定、`AutoEnablesItems`のプロパティ、`NSMenu`です。 `true` 自動 (既定の動作) と`false`は手動です。 

> [!IMPORTANT]
> 手動でのメニューが有効化を使用する場合は、メニューの [なし] 項目などの AppKit クラスによって制御されるものであっても`NSTextView`、自動的に更新されます。 有効にして、コードで手動ですべての項目を無効化の負担になります。

#### <a name="using-validatemenuitem"></a>ValidateMenuItem を使用します。

バインドされているすべてのメニュー項目について、前述のよう、[ウィンドウまたはビュー コント ローラーのカスタム アクション](#Working-with-Custom-Window-Actions)、追加することができます、`validateMenuItem:`アクションと手動で有効にするにまたはメニュー項目を無効にします。

次の例で、`Tag`が有効/無効にするするメニュー項目の種類を決定するプロパティが使用されます、`validateMenuItem:`で選択したテキストの状態に基づいて、アクション、`NSTextView`です。 `Tag`プロパティが設定されてインターフェイス ビルダーの各メニュー項目。

![タグ プロパティを設定](menu-images/validate01.png "タグ プロパティの設定")

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

このコードを実行するとでテキストが選択されていない場合、 `NSTextView`、(これらは、ビュー コント ローラーのアクションにワイヤード (有線) は) 場合でも、2 つのラップ メニュー項目が無効になります。

![無効になって表示項目](menu-images/validate02.png "を示すには、項目が無効になっています")

テキストの一部が選択され、メニューを再度開く、2 つの折り返しのメニュー項目が提供されます。

![表示を有効になっている項目](menu-images/validate03.png "を示すには、項目が有効になっています。")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>有効にして、コード内のメニュー項目への応答

特定 Cocoa ユーザー インターフェイス要素 (テキスト フィールドなど)、UI 設計に追加するだけで、上まで見てきたよう既定のメニュー項目のいくつかが有効になりますされ、すべてのコードを記述しなくても自動的に機能します。 [次へ]、Xamarin.Mac プロジェクトをメニュー項目を有効にし、ユーザーが選択するときに、機能を提供する独自の c# コードを追加することを確認してみましょう。

使用できるユーザーを選びました let say など、**開く**内の項目、**ファイル** メニューのフォルダーを選択します。 これが、アプリケーション全体の関数があることを目的し、付与ウィンドウまたは UI 要素に限定されない、のでしようとしてこれに対処する、アプリケーションのデリゲートにコードを追加します。

**ソリューション パッド**をダブルクリックして、`AppDelegate.CS`ファイルを開いて編集するファイル。

![アプリのデリゲートを選択すると](menu-images/appmenu08.png "アプリ デリゲートを選択します。")

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

今すぐアプリケーションを実行し、開いてみましょう、**ファイル**メニュー。 

![[ファイル] メニュー](menu-images/appmenu09.png "[ファイル] メニュー")

注意して、**開く**メニュー項目が有効になりました。 選択して、[開く] ダイアログが表示されます。

![開いているダイアログ](menu-images/appmenu10.png "[開く] ダイアログ")

クリックした場合、**開く** ボタン、警告メッセージが表示されます。

![ダイアログ メッセージの例を](menu-images/appmenu11.png "ダイアログ メッセージの例")

ここでは、キーの行を行が`[Export ("openDocument:")]`、か`NSMenu`を**AppDelegate**メソッドがあります`void OpenDialog (NSObject sender)`に応答する、`openDocument:`アクション。 上記のわかりやすい場合、**開く**メニュー項目が自動的にワイヤード (有線) をこのアクションにインターフェイスのビルダーで既定では。

[![接続されているアクションを表示する](menu-images/defaultbar03.png "接続されているアクションを表示します。")](menu-images/defaultbar03-large.png#lightbox)

次のコードでそれらに独自のメニューのメニュー項目とアクションの作成と応答を見てみましょう。

### <a name="working-with-the-open-recent-menu"></a>最近使用したメニューを開く の操作

既定では、**ファイル** メニューには、**開く最近**ユーザーがアプリを開いた最後のいくつかのファイルを追跡する項目。 作成する場合は、`NSDocument`ベース Xamarin.Mac アプリでは、このメニューをするために自動的に処理されます。 その他の種類の Xamarin.Mac アプリに、管理と、このメニュー項目を手動で応答を担当になります。

手動で処理するために、**開く最近** メニューは、まず、新しいファイルを開いてされているか、次を使用して保存されたこと、ことを通知します。

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

アプリが使用されていない場合でも`NSDocuments`、使用することも、`NSDocumentController`維持するために、**開く最近**送信することでメニュー、`NSUrl`するファイルの場所に、`NoteNewRecentDocumentURL`のメソッド、`SharedDocumentController`です。

次に、オーバーライドする必要があります、`OpenFile`からユーザーが選択したファイルを開くアプリケーションのデリゲートのメソッド、**最近開いて**メニュー。 例えば:

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

返す`true`それ以外の場合を返す場合は、ファイルを開くには、`false`とファイルが開いていないこと、ユーザーに、組み込みの警告が表示されます。

パスとファイル名が返されるので、**開く最近**] メニューの [スペースを含めることが、作成する前にこの文字を正しくエスケープする必要があります、`NSUrl`またはエラーが発生しました。 次のコードを実行します。

```csharp
filename = filename.Replace (" ", "%20");
```

最後に、作成、`NSUrl`を参照する、ファイルと、アプリ内のヘルパー メソッドを委任する新しいウィンドウを開き、ファイルを読み込むに使用します。

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

すべてまとめてみましょうの実装例を見て、 **<code>appdelegate.cs</code>**ファイル。

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

アプリの要件に基づき、たくないユーザーが同時に複数のウィンドウで、同じファイルを開きます。 アプリでは、例をユーザーが既に開いているファイルを選択した場合 (いずれかから、**最近開いて**または**を開きます..** メニュー項目の場合)、ファイルを含むウィンドウには、前面になります。

これを実現するには、このヘルパー メソッドでは、次のコードを使用します。

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

設計しました、`ViewController`クラス内のファイルにパスを保持するためにその`Path`プロパティです。 次に、アプリで現在開いているすべての windows をループ処理します。 場合は、ファイルが既に開いているウィンドウのいずれかを使用して他のすべての windows の前面になります。

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

一致が見つからない、読み込まれたファイルを使用して新しいウィンドウを開くし、ファイルに記載されて、**開く最近**メニュー。

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

同様に、組み込み**最初レスポンダー**標準メニュー項目を事前にワイヤード (有線) に付属しているアクション、新しいカスタム アクションを作成してそれらのインターフェイスのビルダーでのメニュー項目をネットワーク上でします。

まず、アプリのウィンドウのコント ローラーの 1 つで、カスタム アクションを定義します。 例えば:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

次に、アプリのストーリー ボードでファイルをダブルクリック、**ソリューション パッド**を開くには、Xcode で編集するためのインターフェイスのビルダー。 選択、**最初レスポンダー**下にある、**アプリケーション シーン**に切り替えて、**属性インスペクター**:

![属性のインスペクター](menu-images/action01.png "属性インスペクター")

クリックして、 **+**の下部にあるボタン、**属性インスペクター**新しいカスタム アクションを追加します。

![新しいアクションを追加する](menu-images/action02.png "新しいアクションを追加します。")

ウィンドウのコント ローラー上に作成したカスタムの動作と同じ名前を付けます。

![アクション名を編集](menu-images/action03.png "アクション名を編集")

コントロールをクリックし、メニュー項目をからドラッグ、**最初レスポンダー**下にある、**アプリケーション シーン**です。 ポップアップ リストから作成した新しいアクションを選択します (`defineKeyword:`この例では)。

![アクションをアタッチする](menu-images/action04.png "アクションをアタッチします。")

ストーリー ボードに変更を保存し、Mac の変更を同期用の Visual Studio に戻ります。 アプリを実行する場合は、カスタム アクションが接続されているメニュー項目が自動的に有効/無効にする (開く操作では、ウィンドウに基づく) と、アクションを起動メニュー項目を選択します。

[![新しいアクションをテスト](menu-images/action05.png "新しいアクションのテスト")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>追加、編集、およびメニューを削除します。

前のセクションで示したように、Xamarin.Mac アプリケーションの既定のメニューおよびメニュー項目を特定の UI コントロールに自動的にアクティブ化し、応答のプリセットの数が付属します。 有効にして、これらの既定の項目に応答するアプリケーションにコードを追加する方法も得られます。

このセクションでは、必要があるメニュー項目の削除、メニューを再編成と新しいメニューのメニュー項目とアクションを追加するのになります。

ダブルクリックして、 **Main.storyboard**ファイルで、**ソリューション パッド**ファイルを開いて編集します。

[![Xcode で UI を編集](menu-images/maint01.png "Xcode で UI を編集")](menu-images/maint01-large.png#lightbox)

特定の Xamarin.Mac アプリケーション守られていない、既定値を使用して**ビュー**メニューを削除するようにします。 **インターフェイス階層**選択、**ビュー**メニュー項目は、メイン メニュー バーの一部であります。

![ビューのメニュー項目を選択](menu-images/maint02.png "ビュー メニュー項目を選択")

Delete キーを押すか、バック スペース、メニューを削除します。 次に、すべての項目を使用して今回は、**形式**メニューと、サブメニューから を使用しようとして、アイテムを移動します。 **インターフェイス階層**メニュー項目を選択します。

![複数の項目を強調表示](menu-images/maint03.png "複数の項目を強調表示")

親の下の項目をドラッグして**メニュー**サブメニュー現在されている場所から。

[![親メニューにメニュー項目をドラッグする](menu-images/maint04.png "親メニューにメニュー項目をドラッグします。")](menu-images/maint04-large.png#lightbox)

メニューのようになります。

[![新しい場所に項目](menu-images/maint05.png "が新しい場所に項目")](menu-images/maint05-large.png#lightbox)

[次へ] ドラッグして、**テキスト**から下にあるサブメニューを**形式**メニュー間にあるメイン メニュー バー上に配置し、**形式**と**ウィンドウ**メニュー:

[![テキスト メニュー](menu-images/maint06.png "[テキスト] メニュー")](menu-images/maint06-large.png#lightbox)

下に見ていきましょう、**形式**メニューと削除、**フォント**サブメニュー項目。 次に、選択、**形式**メニュー「フォント」名前を変更します。

[![フォント メニュー](menu-images/maint07.png "フォント メニュー")](menu-images/maint07-large.png#lightbox)

次が自動的に取得付加テキスト ビュー内のテキストには、選択したときに事前に定義されるフレーズのカスタム メニューを作成してみましょう。 下部の [検索] ボックスに、**ライブラリ インスペクター** 「メニュー」の種類 これは容易にできるようにすべてのメニューの UI 要素の検索し、操作します。

![ライブラリ インスペクター](menu-images/maint08.png "ライブラリ インスペクター")

これで、次のように、メニューを作成しましょう。

1. ドラッグ、**メニュー項目**から、**ライブラリ インスペクター**間にあるメニュー バーの上に、**テキスト**と**ウィンドウ**メニュー。 

    ![ライブラリに新しいメニュー項目を選択すると](menu-images/maint10.png "ライブラリに新しいメニュー項目を選択します。")
2. 項目「語句」の名前を変更します。 

    [![メニュー名を設定する](menu-images/maint09.png "メニュー名の設定")](menu-images/maint09-large.png#lightbox)
3. 次にドラッグ、**メニュー**から、**ライブラリ インスペクター**: 

    ![ライブラリのメニューを選択すると](menu-images/maint11.png "ライブラリから、メニューを選択します。")
4. ドロップし、**メニュー**新しい**メニュー項目**先ほど作成し、名前を「語句」に変更します。 

    [![メニュー名の編集](menu-images/maint12.png "メニュー名の編集")](menu-images/maint12-large.png#lightbox)
5. これで 3 つの既定の名前を変更してみましょう**メニュー項目**"Address"、「日付」および"Greeting"。 

    [![語句メニュー](menu-images/maint13.png "フレーズ メニュー")](menu-images/maint13-large.png#lightbox)
6. 4 番目を追加してみましょう。**メニュー項目**をドラッグして、**メニュー項目**から、**ライブラリ インスペクター**および"署名"呼び出し。 

    [![メニュー項目の名前を編集](menu-images/maint14.png "メニュー項目の名前を編集")](menu-images/maint14-large.png#lightbox)
7. メニュー バーに、変更を保存します。

これで、新しいメニュー項目が c# コードに公開されているように、カスタム アクションのセットを作成しましょう。 Xcode でみましょうに切り替え、**アシスタント**ビュー。

[![必要なアクションを作成する](menu-images/maint15.png "必要なアクションを作成します。")](menu-images/maint15-large.png#lightbox)

それでは、次の操作を行います。

1. コントロールをドラッグ、**アドレス**メニュー項目を**AppDelegate.h**ファイル。
2. スイッチ、**接続**型**アクション**: 

    [![アクションの種類を選択すると](menu-images/maint17.png "アクションの種類を選択します。")](menu-images/maint17-large.png#lightbox)
3. 入力、**名前**"phraseAddress"とキーを押して、**接続**新しいアクションを作成するにはボタン。 

    [![アクションの構成](menu-images/maint18.png "アクションの構成")](menu-images/maint18-large.png#lightbox)
4. 上記の手順を繰り返します、**日付**、 **Greeting**、および**署名**メニュー項目。 

    [![完了済みのアクション](menu-images/maint19.png "完了済みの操作")](menu-images/maint19-large.png#lightbox)
5. メニュー バーに、変更を保存します。

次にコードからそのコンテンツを調整できるように、テキスト ビューのコンセントを作成する必要があります。 選択、 **ViewController.h**ファイルで、**アシスタント エディター**と呼ばれる新しいコンセントを作成および`documentText`:

[![コンセントを作成する](menu-images/maint20.png "コンセントを作成します。")](menu-images/maint20-large.png#lightbox)

Xcode からの変更を同期 Mac 用の Visual Studio に戻ります。 次の編集、 **ViewController.cs**ファイルし、次のようになります。

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

これにより、テキスト ビューの外部のテキスト、`ViewController`クラスし、ウィンドウを取得したりがフォーカスを失ったときに、アプリのデリゲートを通知します。 今すぐ編集、 **<code>appdelegate.cs</code>**ファイルし、次のようになります。

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

ここで加えられた、`AppDelegate`部分的なクラスのアクションとインターフェイスのビルダーでは、定義した出力に使用できるようにします。 公開しても、`textEditor`をどのウィンドウが、現在フォーカスを追跡します。

このカスタム メニューとメニュー項目を処理する次の方法が使用されます。

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

ここでアプリケーションでは、すべての項目の実行、**語句**メニューがアクティブになるし、付与語句を選択すると、テキスト ビューに追加されます。

![実行されているアプリの使用例](menu-images/maint21.png "実行されているアプリの例")

アプリケーションのメニュー バーの操作の基礎ができましたが、カスタムのコンテキスト メニューの作成を見てみましょう。

### <a name="creating-menus-from-code"></a>コードからメニューの作成

Xcode のインターフェイスのビルダーでは、メニューおよびメニュー項目を作成する、に加えて回 Xamarin.Mac アプリを作成、変更、またはコードからメニューのサブメニュー、またはメニュー項目を削除する必要がある場合があります。

次の例では、クラスは、メニュー項目と、稼働中に動的に作成するサブメニューに関する情報を保持するために作成されます。

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

このクラスを定義するで次のルーチンは、のコレクションを解析`LanguageFormatCommand`オブジェクトまで再帰的に渡された (インターフェイスのビルダーで作成された) 既存のメニューの下部に追加して、新しいメニューおよびメニュー項目をビルドします。

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

いずれかの`LanguageFormatCommand`を空白の文字列を持つオブジェクト`Title`プロパティでは、このルーチンを作成、**区切り記号のメニュー項目**(薄い灰色の線) メニュー セクションの間。

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

タイトルが提供される場合は、そのタイトルの新しいメニュー項目が作成されます。

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

場合、`LanguageFormatCommand`オブジェクトには、子が含まれています。`LanguageFormatCommand`オブジェクト、サブメニューが作成されると、`AssembleMenu`メソッドは、再帰的にそのメニューを構築するために呼び出されます。

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

代わりに、上記のコードを添えて場合の次のコレクション`LanguageFormatCommand`オブジェクトが作成されました。

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

渡されたコレクションと、`AssembleMenu`関数 (で、**形式**メニューは、ベースとして設定)、次の動的メニューおよびメニュー項目が作成されます。

![新しいメニュー項目は実行中のアプリで](menu-images/dynamic01.png "実行中のアプリの新しいメニュー項目")

#### <a name="removing-menus-and-items"></a>メニューと項目を削除します。

使用することができます、アプリのユーザー インターフェイスからすべてのメニューまたはメニュー項目を削除する必要がある場合、`RemoveItemAt`のメソッド、`NSMenu`クラスだけで、ゼロを付けることによりベースを削除する項目のインデックス。

たとえば、メニューおよび上記のルーチンによって作成されたメニュー項目を削除するには、次のコードを使用でした。

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

上記のコードの場合に動的に削除されません、最初の 4 つのメニュー項目は Xcode のインターフェイスのビルダーと、アプリで利用可能なポイントで作成されます。

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>コンテキスト メニュー

コンテキスト メニューは、ユーザーを右クリックしてまたはウィンドウ内の項目をコントロール クリックしたときに表示されます。 既定では、既に macOS に組み込まれている UI 要素のいくつかには、(、テキスト ビューなど) に接続されているコンテキスト メニューがあります。 ただし、ウィンドウに追加した UI 要素のカスタム独自コンテキスト メニューを作成する場合もあります。

編集しましょう、 **Main.storyboard** Xcode でファイルを追加、**ウィンドウ**、デザイン ウィンドウの設定、**クラス**で"NSPanel"に、 **Identity インスペクター**、追加、新しい**アシスタント**項目を**ウィンドウ**] メニューの [を使用して新しいウィンドウにアタッチし、**話題表示**:

[![設定の segue 種類](menu-images/context01.png "segue の種類を設定")](menu-images/context01-large.png#lightbox)

それでは、次の操作を行います。

1. ドラッグ、**ラベル**から、**ライブラリ インスペクター**上に、**パネル**ウィンドウし、そのテキストを"Property"に設定します。 

    [![ラベルの値を編集](menu-images/context03.png "ラベルの値の編集")](menu-images/context03-large.png#lightbox)
2. 次にドラッグ、**メニュー**から、**ライブラリ インスペクター** 、階層の表示と名前を変更する、3 つの既定のメニュー項目のビューのコント ローラー上に**ドキュメント**、**テキスト**と**フォント**:

    [![必要なメニュー項目](menu-images/context02.png "必要なメニュー項目")](menu-images/context02-large.png#lightbox)
3. 今すぐコントロールからドラッグ、**プロパティ ラベル**上に、**メニュー**:

    [![ドラッグすると、segue](menu-images/context04.png "ドラッグ、segue を作成するには")](menu-images/context04-large.png#lightbox)
4. ポップアップ ダイアログ ボックスから選択**メニュー**: 

    ![設定の segue 種類](menu-images/context05.png "segue の種類を設定")
5. **Identity インスペクター**ビューのコント ローラーのクラスの"PanelViewController"に設定します。 

    [![Setting segue クラス](menu-images/context10.png "設定 segue クラス")](menu-images/context10-large.png#lightbox)
6. Mac に同期するには Visual Studio に戻るし、インターフェイスのビルダーに戻ります。
7. 切り替えて、**アシスタント エディター**を選択し、 **PanelViewController.h**ファイル。
8. に対するアクションの作成、**ドキュメント**メニュー項目と呼ばれる`propertyDocument`: 

    [![アクションの構成](menu-images/context06.png "アクションの構成")](menu-images/context06-large.png#lightbox)
9. 残りのメニュー項目の作成操作を繰り返します。 

    [![必要なアクション](menu-images/context07.png "必要な操作")](menu-images/context07-large.png#lightbox)
10. コンセントを最後に作成、**プロパティ ラベル**と呼ばれる`propertyLabel`: 

    [![コンセントを構成する](menu-images/context08.png "コンセントを構成します。")](menu-images/context08-large.png#lightbox)
11. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

編集、 **PanelViewController.cs**ファイルし、次のコードを追加します。

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

今すぐアプリケーションを実行すると、パネルのプロパティのラベルを右クリックしは、カスタムのコンテキスト メニューが表示されます。 選択すると、項目のメニューから、ラベルの値が変更されます。

![実行しているコンテキスト メニュー](menu-images/context09.png "を実行しているコンテキスト メニュー")

[次へ] のステータス バーのメニューの作成を見てみましょう。

## <a name="status-bar-menus"></a>ステータス バーのメニュー

ステータス バーのメニューは、メニューやアプリケーションの状態を反映してイメージなどのユーザーと対話を提供するメニュー項目の状態またはフィードバックのコレクションを表示します。 アプリケーションのステータス バーのメニューは、アプリケーションがバック グラウンドで実行されている場合でもは有効でアクティブなです。 システム全体のステータス バーは、アプリケーションのメニュー バーの右側に配置されている、macOS で現在使用できる唯一のステータス バーです。

編集しましょう、 **<code>appdelegate.cs</code>**ファイルし、`DidFinishLaunching`次のようなメソッドの検索。

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

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` アクセスを提供する、システム全体のステータス バーです。 `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` 新しい項目のステータス バーを作成します。 そこから、メニューとメニュー項目の数を作成し、メニューを作成したステータス バーの項目にアタッチします。 

アプリケーションを実行して、新しいステータス バーの項目が表示されます。 メニュー項目を選択すると、テキスト ビュー内のテキストが変更されます。 

![実行しているステータス バーのメニュー](menu-images/statusbar01.png "を実行しているステータス バーのメニュー")

次に、カスタム ドック メニュー項目の作成を見てみましょう。

## <a name="custom-dock-menus"></a>カスタム ドック メニュー

ユーザーを右クリックしてまたはドッキング ステーションでアプリケーションのアイコンをコントロール クリックしたときに、Mac アプリケーションのドック メニューが表示されます。

![カスタム メニューのドッキング](menu-images/dock01.png "カスタム メニューをドッキングします。")

次の手順を実行して、アプリケーション用のカスタム ドック メニューを作成しましょう。

1. Mac 用 Visual Studio で、アプリケーションのプロジェクトと選択 を右クリックして**追加** > **新しいファイル.**新しいファイル ダイアログ ボックスで、次のように選択します**Xamarin.Mac** > **空のインターフェイス定義**、の"DockMenu"を使用して、**名前** をクリックし、**新規。**クリックすると、新しい作成**DockMenu.xib**ファイル。

    ![空のインターフェイス定義を追加する](menu-images/dock02.png "空のインターフェイス定義を追加します。")
2. **ソリューション パッド**をダブルクリックして、 **DockMenu.xib**ファイルを開き、Xcode で編集します。 新規作成**メニュー**次の項目を含む:**アドレス**、**日付**、 **Greeting**、および**署名** 

    [![UI のレイアウト](menu-images/dock03.png "UI のレイアウト")](menu-images/dock03-large.png#lightbox)
3. 次に、接続してで、カスタムのメニューを作成した、既存のアクションを新しいメニュー項目、[追加、編集および削除するメニュー](#Adding,_Editing_and_Deleting_Menus)前のセクションです。 切り替えて、**接続インスペクター**を選択し、**最初レスポンダー**で、**インターフェイス階層**です。 下へスクロールし、検索、`phraseAddress:`アクション。 そのアクションの円から線をドラッグして、**アドレス**メニュー項目。

    [![アクションの線をドラッグする](menu-images/dock04.png "アクションの線をドラッグします。")](menu-images/dock04-large.png#lightbox)
4. 他のメニュー項目に対応するアクションを添付するすべてのごとに繰り返します。 

    [![必要なアクション](menu-images/dock05.png "必要な操作")](menu-images/dock05-large.png#lightbox)
5. 次に、選択、**アプリケーション**で、**インターフェイス階層**です。 **接続インスペクター**、円の線をドラッグして、`dockMenu`売店先ほど作成したメニューに。

    [![コンセントをネットワーク上をドラッグする](menu-images/dock06.png "コンセントをネットワーク上をドラッグします。")](menu-images/dock06-large.png#lightbox)
6. 変更を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。
7. ダブルクリックして、 **Info.plist**ファイルを開いて編集するファイル。 

    [![Info.plist ファイルの編集](menu-images/dock07.png "Info.plist ファイルの編集")](menu-images/dock07-large.png#lightbox)
8. クリックして、**ソース**画面の下部にあるタブ。 

    [![ソース ビューを選択する](menu-images/dock08.png "ソース ビューを選択します。")](menu-images/dock08-large.png#lightbox)
9. をクリックして**新しいエントリを追加**緑色のプラス ボタンをクリックして、プロパティ名を"AppleDockMenu"と"DockMenu"(拡張子を除いた新しい .xib ファイルの名前) に値を設定します。 

    [![DockMenu 項目の追加](menu-images/dock09.png "DockMenu 項目の追加")](menu-images/dock09-large.png#lightbox)

これで、アプリケーションを実行すると、ドッキング ステーションに対応するアイコンを右クリックし、マイクロソフトの新しいメニュー項目が表示されます。

![ドック メニューを実行している例](menu-images/dock10.png "を実行しているドック メニューの例")

カスタムの項目のいずれかを選ぶ メニューから、このテキスト ビュー内のテキストは変更されます。

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>ポップアップのボタンをクリックし、プルダウンで表示されるリスト

ポップアップ ボタンは、選択した項目が表示され、、ユーザーがクリックされたときからを選択するオプションの一覧が表示されます。 プルダウンで表示されるリストは、通常、現在のタスクのコンテキストに固有のコマンドを選択するために使用ポップアップのボタンの一種です。 ウィンドウで任意の場所両方とも表示できます。

次の手順を実行して、アプリケーションのポップアップのカスタム ボタンを作成しましょう。

1. 編集、 **Main.storyboard** Xcode とドラッグでのファイル、**ショートカット ボタン**から、**ライブラリ インスペクター**上に、**パネル**で作成したウィンドウ[コンテキスト メニュー](#Contextual_Menus)セクション。 

    [![ショートカット ボタンの追加](menu-images/popup01.png "ポップアップ ボタンの追加")](menu-images/popup01-large.png#lightbox)
2. 新しいメニュー項目を追加しに、ポップアップ画面で、項目のタイトルを設定します**アドレス**、**日付**、 **Greeting**、および**署名。** 

    [![メニュー項目を構成する](menu-images/popup02.png "メニュー項目を構成します。")](menu-images/popup02-large.png#lightbox)
3. 次に、接続してで、カスタムのメニューを作成した既存のアクションを新しいメニュー項目、[追加、編集および削除するメニュー](#Adding,_Editing_and_Deleting_Menus)前のセクションです。 切り替えて、**接続インスペクター**を選択し、**最初レスポンダー**で、**インターフェイス階層**です。 下へスクロールし、検索、`phraseAddress:`アクション。 そのアクションの円から線をドラッグして、**アドレス**メニュー項目。 

    [![アクションの線をドラッグする](menu-images/popup03.png "アクションの線をドラッグします。")](menu-images/popup03-large.png#lightbox)
4. 他のメニュー項目に対応するアクションを添付するすべてのごとに繰り返します。 

    [![必要なすべてのアクション](menu-images/popup04.png "必要なすべての操作")](menu-images/popup04-large.png#lightbox)
5. 変更を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

今すぐ、アプリケーションを実行、ポップアップから項目を選択すると、テキスト ビュー内のテキストは変更されます。

![実行しているポップアップの例](menu-images/popup05.png "を実行しているポップアップの例")

作成でき、ポップアップのボタンとまったく同じ方法で、プルダウンで表示されるリストを使用することができます。 まったく同じように、コンテキスト メニューの既存の操作へのアタッチ、代わりに、独自のカスタム アクションを作成する可能性があります、[コンテキスト メニュー](#Contextual_Menus)セクションです。

## <a name="summary"></a>まとめ

この記事では、メニューおよび Xamarin.Mac アプリケーションでメニュー項目の操作についての詳細を取得しました。 まず、アプリケーションのメニュー バーでは、コンテキスト メニューの作成について説明しました次に、ステータス バーのメニューを検査およびカスタム ドック メニュー、お検査されます。 最後に、ポップアップ メニューおよびプルダウンで表示されるリストを説明します。


## <a name="related-links"></a>関連リンク

- [MacMenus (サンプル)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ヒューマン インターフェイス ガイドライン メニュー](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [アプリケーションのメニューとポップアップ リストの概要](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
