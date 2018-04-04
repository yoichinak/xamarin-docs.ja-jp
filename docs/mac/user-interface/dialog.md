---
title: ダイアログ ボックス
description: この記事では、ダイアログ ボックスとモーダル ウィンドウ Xamarin.Mac アプリケーションでの操作について説明します。 これは、標準のダイアログの操作と c# コードでこれらのコントロールと対話する、Xcode とインターフェイスのビルダーでモーダル ウィンドウを作成するについて説明します。
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: cd0b57cc40d3f7cffd4d5711ec04a7225be870e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="dialogs"></a>ダイアログ ボックス

同じダイアログおよびモーダル ウィンドウへのアクセスがある Xamarin.Mac アプリケーションでは、c# と .NET で作業するときをで作業する開発者*OBJECTIVE-C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成し、モーダル ウィンドウの管理 (または必要に応じて c# コードで直接作成すること)。

ダイアログでは、ユーザー アクションへの応答に表示され、通常の方法のユーザーが操作を完了できますを提供します。 ダイアログ ボックスを終了する前に、ユーザーからの応答が必要です。

Windows は、(アプリケーションを続行する前に閉じる必要があります、エクスポート ダイアログ) などのモーダルまたはモードレスの状態 (などを同時に開いて複数のドキュメントを持つことができるテキスト エディター) で使用することができます。

[![](dialog-images/dialog03.png "[開く] ダイアログ ボックス")](dialog-images/dialog03.png#lightbox)

この記事で Xamarin.Mac アプリケーションでダイアログおよびモーダル ウィンドウの操作の基礎について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>ダイアログ ボックスの概要

ダイアログ ボックスでは、(ファイルを保存する) などのユーザー アクションへの応答が表示され、そのアクションを完了するための手段を提供します。 ダイアログ ボックスを終了する前に、ユーザーからの応答が必要です。

Apple にに従っては、ダイアログを表示する次の 3 つの方法があります。

- **ドキュメントのモーダル**-A ドキュメント モーダル ダイアログを防止、ユーザーは非表示にするまで、特定のドキュメント内の他の処理を実行します。
- **アプリのモーダル**- アプリケーションのモーダル ダイアログを防止、ユーザーは非表示にするまで、アプリケーションと対話します。
- **モードレス**A モードレス ダイアログ設定を変更するダイアログ ボックスで、ドキュメント ウィンドウとも対話中にユーザーを有効にします。

### <a name="modal-window"></a>モーダル ウィンドウ

任意の標準`NSWindow`モーダルとして表示することによってカスタマイズされたダイアログとして使用できます。

[![](dialog-images/modal01.png "例のモーダル ウィンドウ")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>ドキュメントのモーダル ダイアログ シート

A_シート_ダイアログ ボックスを閉じる、まで、ウィンドウとの対話をしないように、特定のドキュメント ウィンドウにアタッチされているモーダル ダイアログ ボックスします。 シートは、元の現れたでき、1 つだけのシート ウィンドウを開いている任意の時点でウィンドウにアタッチされています。

[![](dialog-images/sheet08.png "例のモーダル シート")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Windows の設定

環境設定ウィンドウは、ユーザーの変更頻度が低いアプリケーションの設定を含むモードレス ダイアログです。 環境設定ウィンドウは、多くの場合、設定の異なるグループ間で切り替えるには、ユーザーを許可するツールバーを含めます。

[![](dialog-images/dialog02.png "例の基本設定 ウィンドウ")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>[開く] ダイアログ

開く ダイアログ ボックスは、ユーザーを見つけて、アプリケーションでアイテムを開く一貫した方法を示します。

[![](dialog-images/dialog03.png "[開く] ダイアログ ボックス")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>印刷とページ セットアップ ダイアログ

標準の印刷 macOS およびを使用するすべてのアプリケーションで発生するページ セットアップ ダイアログ ボックスをユーザーが一貫した印刷できるように、アプリケーションを表示できます。

印刷 ダイアログ ボックスは、両方無料浮動 ダイアログ ボックスとして表示できます。

[![](dialog-images/print01.png "印刷ダイアログ ボックス")](dialog-images/print01.png#lightbox)

または、シートとして表示することができます。

[![](dialog-images/print02.png "印刷のシート")](dialog-images/print02.png#lightbox)

ページの 設定 ダイアログ ボックスは、両方無料浮動 ダイアログ ボックスとして表示できます。

[![](dialog-images/print03.png "ページの設定 ダイアログ ボックス")](dialog-images/print03.png#lightbox)

または、シートとして表示することができます。

[![](dialog-images/print04.png "ページ セットアップ シート")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>ファイルの保存

[保存] ダイアログ ボックスでは、ユーザー アプリケーションの項目を保存する方法を統一できます。 [保存] ダイアログ ボックスが 2 つの状態:**最小限**(折りたたまれたとも呼ばれます)。

[![](dialog-images/save01.png "保存 ダイアログ")](dialog-images/save01.png#lightbox)

および**展開**状態。

[![](dialog-images/save02.png "展開保存ダイアログ")](dialog-images/save02.png#lightbox)

**最小限**シートとして保存 ダイアログ ボックスを表示することも。

[![](dialog-images/save03.png "最小限のシート保存")](dialog-images/save03.png#lightbox)

同様、**展開**保存 ダイアログ ボックス。

[![](dialog-images/save04.png "展開のシートを保存します。")](dialog-images/save04.png#lightbox)

詳細については、次を参照してください、[ダイアログ](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1)Apple のセクション[OS X のヒューマン インターフェイス ガイドライン。](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>モーダル ウィンドウをプロジェクトに追加します。

メイン ドキュメント ウィンドウとは別に Xamarin.Mac アプリケーションは、設定や検査パネルなどのユーザーにその他の種類のウィンドウを表示する必要があります。

新しいウィンドウを追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**を開き、 `Main.storyboard` Xcode のインターフェイスのビルダーで編集するためのファイルです。
2. 新しいドラッグ**ビュー コント ローラー**デザイン サーフェイスに。

    [![](dialog-images/new01.png "ライブラリからビュー コント ローラーを選択します。")](dialog-images/new01.png#lightbox)
3. **Identity インスペクター**、入力`CustomDialogController`の**クラス名**: 

    [![](dialog-images/new02.png "クラス名を設定します。")](dialog-images/new02.png#lightbox)
4. Mac 用 Visual Studio に切り替えます、Xcode と同期し、作成できるように、`CustomDialogController.h`ファイル。
5. Xcode に戻るし、インターフェイスをデザインします。 

    [![](dialog-images/new03.png "Xcode で UI のデザイン")](dialog-images/new03.png#lightbox)
6. 作成、**モーダル話題**ダイアログのウィンドウ ダイアログを開いて、UI 要素から、新しいビュー コント ローラーにコントロールをドラッグして、アプリのメイン ウィンドウからです。 割り当てる、**識別子** `ModalSegue`: 

    [![](dialog-images/new06.png "モーダル segue")](dialog-images/new06.png#lightbox)
6. いずれかのワイヤ アップ**アクション**と**コンセント**: 

    [![](dialog-images/new04.png "アクションを設定します。")](dialog-images/new04.png#lightbox)
6. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

ように、`CustomDialogController.cs`次のようなファイルの内容。

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

このコードは、タイトルと、ダイアログ ボックスの説明を設定するいくつかのプロパティとキャンセルまたは許容されるダイアログ ボックスに反応するためのいくつかのイベントを公開します。

次に、編集、`ViewController.cs`ファイルを上書き、`PrepareForSegue`メソッドされ次のようになります。

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

このコードは、ダイアログ ボックスに定義した Xcode のインターフェイスのビルダー segue を初期化し、タイトルと説明を設定します。 また、ダイアログ ボックスで、ユーザーは、選択を処理します。

アプリケーションを実行したり、カスタム ダイアログを表示できます。

[![](dialog-images/new05.png "ダイアログ ボックスの例")](dialog-images/new05.png#lightbox)

詳細については、Xamarin.Mac アプリケーションで windows を使用して、参照してください、[ウィンドウの操作](~/mac/user-interface/window.md)ドキュメント。

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>カスタムのシートの作成

A_シート_ダイアログ ボックスを閉じる、まで、ウィンドウとの対話をしないように、特定のドキュメント ウィンドウにアタッチされているモーダル ダイアログ ボックスします。 シートは、元の現れたでき、1 つだけのシート ウィンドウを開いている任意の時点でウィンドウにアタッチされています。 

Xamarin.Mac でカスタムのシートを作成するには次の操作してみましょう。

1. **ソリューション エクスプ ローラー**を開き、 `Main.storyboard` Xcode のインターフェイスのビルダーで編集するためのファイルです。
2. 新しいドラッグ**ビュー コント ローラー**デザイン サーフェイスに。

    [![](dialog-images/new01.png "ライブラリからビュー コント ローラーを選択します。")](dialog-images/new01.png#lightbox)
2. ユーザー インターフェイスを設計します。

    [![](dialog-images/sheet01.png "UI の設計")](dialog-images/sheet01.png#lightbox)
3. 作成、**シート話題**メイン ウィンドウに新しいビュー コント ローラーから。 

    [![](dialog-images/sheet02.png "シート segue 種類を選択します。")](dialog-images/sheet02.png#lightbox)
4. **Identity インスペクター**、ビューのコント ローラーの名前を付けます**クラス** `SheetViewController`: 

    [![](dialog-images/sheet03.png "クラス名を設定します。")](dialog-images/sheet03.png#lightbox)
5. 必要に応じていずれかを定義**コンセント**と**アクション**: 

    [![](dialog-images/sheet04.png "必須のコンセントとアクションを定義します。")](dialog-images/sheet04.png#lightbox)
6. 変更内容を保存し、同期する Mac 用の Visual Studio に戻ります。

次に、編集、`SheetViewController.cs`ファイルし、次のようになります。

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

次に、編集、`ViewController.cs`ファイルで、編集、`PrepareForSegue`メソッドされ次のようになります。

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

アプリケーションを実行すると、シートを開きは、ウィンドウにアタッチされます。

[![](dialog-images/sheet08.png "例シート")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>環境設定 ダイアログを作成します。

インターフェイスのビルダーで基本設定のビューをレイアウトお前に、設定、切り替えを処理するカスタム segue 型を追加する必要になります。 新しいクラスをプロジェクトに追加し、それを呼び出す`ReplaceViewSeque `です。 クラスを編集し、次のようになります。

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

作成したカスタム segue で新しいウィンドウ、環境設定を処理する Xcode のインターフェイスのビルダーに追加できます。

新しいウィンドウを追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**を開き、 `Main.storyboard` Xcode のインターフェイスのビルダーで編集するためのファイルです。
2. 新しいドラッグ**ウィンドウ コント ローラー**デザイン サーフェイスに。

    [![](dialog-images/pref01.png "ライブラリからウィンドウ コント ローラーを選択します。")](dialog-images/pref01.png#lightbox)
3. 近くのウィンドウを整列、**メニュー バー**デザイナー。

    [![](dialog-images/pref02.png "新しいウィンドウを追加します。")](dialog-images/pref02.png#lightbox)
4. あるタブで、基本設定のビューには、接続されているビュー コント ローラーのコピーを作成します。

    [![](dialog-images/pref03.png "必須のコント ローラーの表示を追加します。")](dialog-images/pref03.png#lightbox)
5. 新しいドラッグ**ツールバー コント ローラー**から、**ライブラリ**:

    [![](dialog-images/pref04.png "ライブラリからツールバーのコント ローラーを選択します。")](dialog-images/pref04.png#lightbox)
6. デザイン画面で、ウィンドウの上にドロップします。

    [![](dialog-images/pref05.png "新しいツールバー コント ローラーの追加")](dialog-images/pref05.png#lightbox)
7. レイアウトのデザイン、ツールバーの:

    [![](dialog-images/pref06.png "ツールバーのレイアウト")](dialog-images/pref06.png#lightbox)
8. コントロールをクリックし、それぞれからドラッグ**ツール バー ボタン**を上記で作成したビューにします。 選択、**カスタム**話題型。

    [![](dialog-images/pref07.png "Segue の種類を設定")](dialog-images/pref07.png#lightbox)
9. 新しい Segue を選択し、設定、**クラス**に`ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Setting segue クラス")](dialog-images/pref08.png#lightbox)
10. **Menubar デザイナー**デザイン画面で、[アプリケーション] メニューから選択**設定しています.**コントロールをクリックし、ドラッグ、環境設定ウィンドウを作成するには**表示**話題。

    [![](dialog-images/pref09.png "Segue の種類を設定")](dialog-images/pref09.png#lightbox)
11. 変更内容を保存し、同期する Mac 用の Visual Studio に戻ります。

コードを実行すると、選択、**設定しています.**から、**アプリケーション メニュー**ウィンドウが表示されます。

[![](dialog-images/pref10.png "例の設定 ウィンドウ")](dialog-images/pref10.png#lightbox)

Windows とツールバーの操作の詳細についてを参照してください、 [Windows](~/mac/user-interface/window.md)と[ツールバー](~/mac/user-interface/toolbar.md)ドキュメント。

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>保存と読み込みの基本設定

一般的な macOS アプリでユーザーが、アプリのユーザー設定のいずれかに変更を行うときにそれらの変更自動的に保存されます。 Xamarin.Mac アプリで、これを処理する最も簡単な方法では、システム全体のすべてのユーザーの設定を管理し、共有する 1 つのクラスを作成します。

最初に、追加、新しい`AppPreferences`をプロジェクトにクラスし、継承`NSObject`です。 環境設定が使用するように設計する[データ バインディングと、キーと値のコーディング](~/mac/app-fundamentals/databinding.md)の作成プロセスがようにし、はるかに簡単にフォーム、基本設定を維持します。 少量の単純なデータ型の環境設定ためを使用して組み込み`NSUserDefaults`を格納および値を取得します。

編集、`AppPreferences.cs`ファイルし、次のようになります。

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLangauge")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLangauge", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLangauge");
                SaveInt ("DefaultLangauge", value, true);
                DidChangeValue ("DefaultLangauge");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

など、このクラスにいくつかのヘルパー ルーチンが含まれています`SaveInt`、 `LoadInt`、 `SaveColor`、 `LoadColor`、操作をするためなど`NSUserDefaults`容易にします。 また、`NSUserDefaults`を処理する機能を組み込むことはありません`NSColors`、`NSColorToHexString`と`NSColorFromHexString`メソッドを使用して色を web ベースの 16 進文字列に変換 (`#RRGGBBAA`場所`AA`アルファの透過性は、) することができます簡単に保存され取得します。

`AppDelegate.cs`ファイルのインスタンスを作成、 **AppPreferences**アプリ全体を使用するオブジェクトです。

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>基本設定のビューに配線設定

次に、上記で作成したビューと基本設定ウィンドウの UI 要素に基本設定クラスを接続します。 インターフェイス ビルダーでは、基本設定のビューのコント ローラーを選択に切り替えると、 **Identity インスペクター**、コント ローラーのカスタム クラスを作成します。 

[![](dialog-images/prefs12.png "Id 検査")](dialog-images/prefs12.png#lightbox)

編集するため、新しく作成されたクラスを開き、変更を同期する Mac 用の Visual Studio に切り替えます。 次のようにクラスを行います。

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

このクラスがここでは、次の 2 つのことを行うことに注意してください。 最初に、ヘルパーがある`App`プロパティへのアクセスを、 **AppDelegate**簡単にします。 2 番目、`Preferences`プロパティは、グローバルな公開**AppPreferences**クラスの UI コントロールとデータ バインディングは、このビューに配置します。

次に、ストーリー ボード ファイルをダブルクリックして再びインターフェイス ビルダーで開く (および上だけに行われた変更を参照してください)。 インターフェイスを作成の基本設定のビューに必要な UI コントロールをドラッグします。 各コントロールに切り替え、**バインド インスペクター**の個々 のプロパティにバインドし、 **AppPreference**クラス。

[![](dialog-images/prefs13.png "バインディングの検査")](dialog-images/prefs13.png#lightbox)

パネル (コント ローラーの表示) のすべての上記の手順を繰り返して、基本設定のプロパティが必要なします。

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>開いているすべての Windows に基本設定を適用する変更します。

前述のように、ユーザーがアプリのユーザー設定、それらの変更のいずれかに変更を加えます時に、アプリケーションが自動的に保存され、任意の windows に適用される一般的な macOS で、ユーザーが持っている、アプリケーションで開いてです。

最小限のコーディング作業の量で、エンドユーザーに円滑かつ透過的を実行するには、このプロセスは、慎重に計画し、アプリの設定と windows のデザインが許可されます。

利用するアプリの環境設定ウィンドウで、コンテンツ ビュー コント ローラーにアクセスする次のヘルパー プロパティの追加、 **AppDelegate**容易にします。

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

次に、目次またはユーザーの設定に基づく動作を構成するクラスを追加します。

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

ユーザーの設定に準拠しているかどうかを確認するウィンドウを最初に開いたときに、構成メソッドを呼び出す必要があります。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

次に、編集、`AppDelegate.cs`ファイルおよび任意の基本設定を適用する次のメソッドは、すべての開いているウィンドウに変更を追加します。

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

次に、追加、`PreferenceWindowDelegate`をプロジェクトにクラスし、次のようになります。

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWidowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

これを基本設定ウィンドウを閉じるときに、すべての開いているウィンドウに送信される基本設定変更となります。

最後に、基本設定ウィンドウ コント ローラーを編集し、上記で作成したデリゲートを追加します。

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWidowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

これらすべての変更にでは、場合は、ユーザーがアプリの設定を編集し、基本設定ウィンドウを閉じます変更は、すべての開いているウィンドウに適用されます。

[![](dialog-images/prefs14.png "例の設定 ウィンドウ")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>[開く] ダイアログ

開く ダイアログ ボックスでは、一貫した方法を見つけて、アプリケーションでアイテムを開いたユーザーを表示します。 Xamarin.Mac アプリケーションで開く ダイアログを表示するには、次のコードを使用します。

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

上記のコードで、ファイルの内容を表示する、新しいドキュメント ウィンドウを開いています。 これを交換する必要があります機能を持つコードは、アプリケーションで必要です。

使用する場合は、次のプロパティは使用可能な`NSOpenPanel`:

- **CanChooseFiles**場合 -`true`ユーザーがファイルを選択できます。
- **CanChooseDirectories**場合 -`true`ユーザーがディレクトリを選択できます。
- **AllowsMultipleSelection**場合 -`true`ユーザーは一度に 1 つ以上のファイルを選択することができます。
- **ResolveAliases**場合 -`true`を選択して、エイリアスは、元のファイルのパスに解決します。
- **AllowedFileTypes** -ユーザーがいずれかの拡張として選択できるファイルの種類の文字列配列または_ユーティリ ティー_です。 既定値は`null`、これにより、任意のファイルを開くことができません。

`RunModal ()`メソッドは、[開く] ダイアログを表示およびユーザーがファイルまたはディレクトリで指定されているプロパティ) を選択できるようにしを返します`1`、ユーザーがクリックした場合、**開く**ボタンをクリックします。

[開く] ダイアログをユーザーの選択したファイルまたはディレクトリ内の Url の配列として返します、`URL`プロパティです。

プログラムを実行すると、選択、**オープンしています.**項目を**ファイル**メニューで、次が表示されます。 

[![](dialog-images/dialog03.png "[開く] ダイアログ ボックス")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>印刷とページ セットアップ ダイアログ

標準の印刷 macOS およびを使用するすべてのアプリケーションで発生するページ セットアップ ダイアログ ボックスをユーザーが一貫した印刷できるように、アプリケーションを表示できます。

次のコードは、標準の印刷 ダイアログ ボックスに表示されます。

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

設定している場合、`ShowPrintAsSheet`プロパティを`false`アプリケーションを実行し、印刷ダイアログ ボックスを表示、次が表示されます。

[![](dialog-images/print01.png "印刷ダイアログ ボックス")](dialog-images/print01.png#lightbox)

場合設定、`ShowPrintAsSheet`プロパティを`true`アプリケーションを実行し、印刷ダイアログ ボックスを表示、次が表示されます。

[![](dialog-images/print02.png "印刷のシート")](dialog-images/print02.png#lightbox)

次のコード ページ レイアウト ダイアログが表示されます。

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

設定している場合、`ShowPrintAsSheet`プロパティを`false`アプリケーションを実行し、印刷レイアウト ダイアログを表示、次が表示されます。

[![](dialog-images/print03.png "ページの設定 ダイアログ ボックス")](dialog-images/print03.png#lightbox)

場合設定、`ShowPrintAsSheet`プロパティを`true`アプリケーションを実行し、印刷レイアウト ダイアログを表示、次が表示されます。

[![](dialog-images/print04.png "ページ セットアップ シート")](dialog-images/print04.png#lightbox)

印刷とセットアップのダイアログ ボックスのページの操作の詳細については、Apple を参照してください[NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092)、 [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080)と[印刷の概要](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1)ドキュメントです。

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>保存ダイアログ

[保存] ダイアログ ボックスでは、ユーザー アプリケーションの項目を保存する方法を統一できます。

次のコードは、標準的な保存ダイアログに表示されます。

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

`AllowedFileTypes`プロパティは、ファイルに保存する、ユーザーが選択できるファイルの種類の文字列配列。 拡張機能としてか、ファイルの種類を指定することができますか_ユーティリ ティー_です。 既定値は`null`、これにより、ファイルの種類を使用します。

設定している場合、`ShowSaveAsSheet`プロパティを`false`アプリケーションを実行し、選択、**名前を付けて保存しています.**から、**ファイル**メニューで、次が表示されます。

[![](dialog-images/save01.png "A 保存 ダイアログ ボックス")](dialog-images/save01.png#lightbox)

ユーザーは、ダイアログ ボックスを拡大できます。

[![](dialog-images/save02.png "展開の保存 ダイアログ ボックス")](dialog-images/save02.png#lightbox)

設定している場合、`ShowSaveAsSheet`プロパティを`true`アプリケーションを実行し、選択、**名前を付けて保存しています.**から、**ファイル**メニューで、次が表示されます。

[![](dialog-images/save03.png "シート保存")](dialog-images/save03.png#lightbox)

ユーザーは、ダイアログ ボックスを拡大できます。

[![](dialog-images/save04.png "展開のシートを保存します。")](dialog-images/save04.png#lightbox)

詳細については、[保存] ダイアログ ボックスの操作で、Apple を参照してください[NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098)ドキュメント。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、モーダル ウィンドウ、シートおよび Xamarin.Mac アプリケーション内の標準のシステム ダイアログ ボックスの作業についての詳細を取得しました。 型が異なり、モーダル ウィンドウ、シートとダイアログの使用方法を説明しましたモーダル ウィンドウおよび Xcode でシートを作成および管理のインターフェイスのビルダーとモーダル ウィンドウを操作する方法、シートし、ダイアログ ボックスの c# コードです。

## <a name="related-links"></a>関連リンク

- [MacWindows (サンプル)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [メニュー](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [ツールバー](~/mac/user-interface/toolbar.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [シートの概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [印刷の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
