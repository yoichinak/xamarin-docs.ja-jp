---
title: Xamarin. Mac のダイアログ
description: この記事では、Xamarin. Mac アプリケーションでのダイアログとモーダルウィンドウの使用について説明します。 Xcode と Interface builder でのモーダルウィンドウの作成、標準ダイアログの使用、およびコード内でC#のこれらのコントロールとの対話について説明します。
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 03fad33d49f1454700c118ad44c8582453a75eee
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645653"
---
# <a name="dialogs-in-xamarinmac"></a>Xamarin. Mac のダイアログ

Xamarin. Mac C#アプリケーションでと .net を使用する場合、 *Xcode と* *で作業*している開発者が同じダイアログとモーダルウィンドウにアクセスできます。 Xcode は直接統合されているため、Xcode の_Interface Builder_を使用してモーダルウィンドウを作成および維持できます (または、 C#必要に応じて、コード内で直接作成することもできます)。

ユーザーの操作に応答してダイアログが表示され、通常、ユーザーがアクションを完了する方法が示されます。 ダイアログを閉じるには、ユーザーからの応答が必要です。

ウィンドウは、モードレス状態 (複数のドキュメントを一度に開くことができるテキストエディターなど) またはモーダル (アプリケーションを続行する前に閉じる必要があるエクスポートダイアログなど) で使用できます。

[![](dialog-images/dialog03.png "[開く] ダイアログボックス")](dialog-images/dialog03.png#lightbox)

この記事では、Xamarin. Mac アプリケーションでのダイアログとモーダルウィンドウの操作の基本について説明します。 最初に、 [Hello, Mac](~/mac/get-started/hello-mac.md)の記事を使用して作業することを強くお勧めします。具体的には、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)および[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)に関するセクションで説明します。これは、で使用する主要な概念と手法に関するものです。この記事をご覧ください。

[Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメントの「[クラス/ C#メソッドを目的に公開](~/mac/internals/how-it-works.md)する」セクションを参照して、 C#クラスをに接続するために使用`Register`さ`Export`れるコマンドとコマンドについて説明します。目的 C オブジェクトと UI 要素。

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>ダイアログの概要

ユーザー操作 (ファイルの保存など) への応答としてダイアログが表示され、ユーザーはその操作を完了することができます。 ダイアログを閉じるには、ユーザーからの応答が必要です。

Apple によれば、ダイアログを表示するには次の3つの方法があります。

- **ドキュメントモーダル**-ドキュメントモーダルダイアログでは、ユーザーは、ドキュメントが破棄されるまで、特定のドキュメント内で他の操作を実行できません。
- **アプリモーダル**-アプリのモーダルダイアログでは、ユーザーがアプリケーションと対話できないようにします。
- **モードレス**モードレスダイアログボックスを使用すると、ユーザーはドキュメントウィンドウと対話しながらダイアログの設定を変更できます。

### <a name="modal-window"></a>モーダル ウィンドウ

任意の`NSWindow`標準をモーダルとして表示することで、カスタマイズされたダイアログとして使用できます。

[![](dialog-images/modal01.png "モーダルウィンドウの例")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>ドキュメントモーダルダイアログシート

_シート_は、特定のドキュメントウィンドウに関連付けられたモーダルダイアログで、ユーザーがダイアログを閉じるまでウィンドウと対話できないようにします。 ウィンドウが表示されているウィンドウにシートが添付されており、一度に1つのウィンドウで開くことができるシートは1つだけです。

[![](dialog-images/sheet08.png "モーダルシートの例")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>[基本設定] ウィンドウ

[基本設定] ウィンドウは、ユーザーが頻繁に変更するアプリケーションの設定を含むモードレスダイアログです。 [基本設定] ウィンドウには、ユーザーがさまざまな設定グループを切り替えることができるツールバーが含まれていることがよくあります。

[![](dialog-images/dialog02.png "基本設定ウィンドウの例")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>ダイアログを開く

[開く] ダイアログボックスでは、アプリケーション内の項目を検索して開くための一貫した方法がユーザーに提供されます。

[![](dialog-images/dialog03.png "[開く] ダイアログボックス")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>印刷とページ設定のダイアログ

macOS には、標準の印刷とページ設定のダイアログボックスが用意されており、ユーザーは使用するすべてのアプリケーションで一貫した印刷機能を使用できます。

[印刷] ダイアログボックスは、フリーフローティングダイアログボックスの両方として表示できます。

[![](dialog-images/print01.png "[印刷] ダイアログボックス")](dialog-images/print01.png#lightbox)

または、このファイルをシートとして表示することもできます。

[![](dialog-images/print02.png "印刷シート")](dialog-images/print02.png#lightbox)

[ページ設定] ダイアログは、無料のフローティングダイアログボックスの両方として表示できます。

[![](dialog-images/print03.png "ページ設定ダイアログ")](dialog-images/print03.png#lightbox)

または、このファイルをシートとして表示することもできます。

[![](dialog-images/print04.png "ページ設定シート")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>ダイアログの保存

[保存] ダイアログを使用すると、アプリケーションにアイテムを保存するための一貫した方法をユーザーが選択できます。 [保存] ダイアログには次の2つの状態があります。**最小**(折りたたまれているとも呼ばれます):

[![](dialog-images/save01.png "保存ダイアログ")](dialog-images/save01.png#lightbox)

**展開**された状態は次のようになります。

[![](dialog-images/save02.png "拡張保存ダイアログ")](dialog-images/save02.png#lightbox)

[**最小**保存] ダイアログボックスは、次のようにシートとして表示することもできます。

[![](dialog-images/save03.png "最小保存シート")](dialog-images/save03.png#lightbox)

**展開**された [保存] ダイアログボックスは次のようになります。

[![](dialog-images/save04.png "展開された保存シート")](dialog-images/save04.png#lightbox)

詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[ダイアログ](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1)」セクションを参照してください。

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>プロジェクトへのモーダルウィンドウの追加

メインのドキュメントウィンドウとは別に、Xamarin. Mac アプリケーションでは、他の種類のウィンドウをユーザーに表示する必要がある場合があります (ユーザー設定やインスペクターパネルなど)。

新しいウィンドウを追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、Xcode の Interface Builder `Main.storyboard`で編集するファイルを開きます。
2. 新しい**ビューコントローラー**をデザインサーフェイスにドラッグします。

    [![](dialog-images/new01.png "ライブラリからビューコントローラーを選択する")](dialog-images/new01.png#lightbox)
3. **Id インスペクター**で、 `CustomDialogController` **クラス名**として「」と入力します。 

    [![](dialog-images/new02.png "クラス名の設定")](dialog-images/new02.png#lightbox)
4. Visual Studio for Mac に戻り、Xcode との同期を許可して、 `CustomDialogController.h`ファイルを作成します。
5. Xcode に戻り、インターフェイスを設計します。 

    [![](dialog-images/new03.png "Xcode で UI を設計する")](dialog-images/new03.png#lightbox)
6. ダイアログボックスを開く UI 要素からコントロールをドラッグして、アプリのメインウィンドウから新しいビューコントローラーに**モーダルセグエ**を作成します。 識別子`ModalSegue`を割り当てます。 

    [![](dialog-images/new06.png "モーダルセグエ")](dialog-images/new06.png#lightbox)
7. すべての**アクション**と**アウトレット**を接続します。 

    [![](dialog-images/new04.png "アクションの構成")](dialog-images/new04.png#lightbox)
8. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

ファイルの`CustomDialogController.cs`外観を次のようにします。

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

このコードでは、いくつかのプロパティを公開して、ダイアログのタイトルと説明を設定し、いくつかのイベントを表示して、ダイアログがキャンセルまたは受け入れられるようにします。

次に、 `ViewController.cs`ファイルを編集し、 `PrepareForSegue`メソッドをオーバーライドして、次のようにします。

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

このコードは、Xcode の Interface Builder で定義したセグエをダイアログに初期化し、タイトルと説明を設定します。 また、ユーザーがダイアログボックスで行った選択を処理します。

アプリケーションを実行し、カスタムダイアログを表示することができます。

[![](dialog-images/new05.png "ダイアログの例")](dialog-images/new05.png#lightbox)

Xamarin. Mac アプリケーションで windows を使用する方法の詳細については、 [windows](~/mac/user-interface/window.md)のドキュメントを参照してください。

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>カスタムシートの作成

_シート_は、特定のドキュメントウィンドウに関連付けられたモーダルダイアログで、ユーザーがダイアログを閉じるまでウィンドウと対話できないようにします。 ウィンドウが表示されているウィンドウにシートが添付されており、一度に1つのウィンドウで開くことができるシートは1つだけです。 

Xamarin. Mac でカスタムシートを作成するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、Xcode の Interface Builder `Main.storyboard`で編集するファイルを開きます。
2. 新しい**ビューコントローラー**をデザインサーフェイスにドラッグします。

    [![](dialog-images/new01.png "ライブラリからビューコントローラーを選択する")](dialog-images/new01.png#lightbox)
3. ユーザーインターフェイスを設計します。

    [![](dialog-images/sheet01.png "UI デザイン")](dialog-images/sheet01.png#lightbox)
4. メインウィンドウから新しいビューコントローラーに**シートセグエ**を作成します。 

    [![](dialog-images/sheet02.png "シートのセグエの種類の選択")](dialog-images/sheet02.png#lightbox)
5. **Id インスペクター**で、ビューコントローラーの**クラス** `SheetViewController`にという名前を指定します。 

    [![](dialog-images/sheet03.png "クラス名の設定")](dialog-images/sheet03.png#lightbox)
6. 必要な**アウトレット**と**アクション**を定義します。 

    [![](dialog-images/sheet04.png "必要なコンセントとアクションを定義する")](dialog-images/sheet04.png#lightbox)
7. 変更を保存し、Visual Studio for Mac に戻って同期します。

次に、 `SheetViewController.cs`ファイルを編集し、次のように表示します。

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

次に、 `ViewController.cs`ファイルを編集し、 `PrepareForSegue`メソッドを編集して、次のようにします。

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

アプリケーションを実行してシートを開くと、ウィンドウに接続されます。

[![](dialog-images/sheet08.png "例のシート")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>基本設定ダイアログの作成

Interface Builder で基本設定ビューをレイアウトする前に、カスタムのセグエ型を追加して、ユーザー設定の切り替えを処理する必要があります。 新しいクラスをプロジェクトに追加し、それ`ReplaceViewSeque`を呼び出します。 クラスを編集し、次のようにします。

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

カスタムセグエを作成したので、Xcode の Interface Builder に新しいウィンドウを追加して、設定を処理することができます。

新しいウィンドウを追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、Xcode の Interface Builder `Main.storyboard`で編集するファイルを開きます。
2. 新しい**ウィンドウコントローラー**をデザインサーフェイスにドラッグします。

    [![](dialog-images/pref01.png "ライブラリからウィンドウコントローラーを選択します")](dialog-images/pref01.png#lightbox)
3. ウィンドウを**メニューバー**デザイナーの近くに配置します。

    [![](dialog-images/pref02.png "新しいウィンドウの追加")](dialog-images/pref02.png#lightbox)
4. 基本設定ビューにタブが表示されるように、アタッチされたビューコントローラーのコピーを作成します。

    [![](dialog-images/pref03.png "必要なビューコントローラーの追加")](dialog-images/pref03.png#lightbox)
5. 新しい**ツールバーコントローラー**を**ライブラリ**からドラッグします。

    [![](dialog-images/pref04.png "ライブラリからツールバーコントローラーを選択します")](dialog-images/pref04.png#lightbox)
6. 次のように、デザインサーフェイスのウィンドウにドロップします。

    [![](dialog-images/pref05.png "新しいツールバーコントローラーの追加")](dialog-images/pref05.png#lightbox)
7. ツールバーのデザインをレイアウトします。

    [![](dialog-images/pref06.png "ツールバーのレイアウト")](dialog-images/pref06.png#lightbox)
8. コントロールをクリックし、**ツールバー**の各ボタンから、前に作成したビューにドラッグします。 **カスタム**セグエの種類を選択してください:

    [![](dialog-images/pref07.png "セグエ型の設定")](dialog-images/pref07.png#lightbox)
9. 新しいセグエを選択し、**クラス**を次`ReplaceViewSegue`のように設定します。

    [![](dialog-images/pref08.png "セグエクラスの設定")](dialog-images/pref08.png#lightbox)
10. デザインサーフェイスのメニュー**バーデザイナー**で、アプリケーション] メニューの **[基本設定...]** を選択し、コントロールをクリックして [基本設定 ウィンドウまでドラッグし、 **Show**セグエを作成します。

    [![](dialog-images/pref09.png "セグエ型の設定")](dialog-images/pref09.png#lightbox)
11. 変更を保存し、Visual Studio for Mac に戻って同期します。

コードを実行し、[**アプリケーション] メニュー**の **[基本設定...]** を選択すると、ウィンドウが表示されます。

[![](dialog-images/pref10.png "[基本設定] ウィンドウの例")](dialog-images/pref10.png#lightbox)

Windows とツールバーの使用方法の詳細については、 [windows](~/mac/user-interface/window.md)と[ツールバー](~/mac/user-interface/toolbar.md)のドキュメントを参照してください。

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>設定の保存と読み込み

一般的な macOS アプリでは、ユーザーがアプリのユーザー設定を変更すると、それらの変更は自動的に保存されます。 Xamarin. Mac アプリでこれを処理する最も簡単な方法は、1つのクラスを作成して、すべてのユーザーの基本設定を管理し、システム全体を共有することです。

まず、新しい`AppPreferences`クラスをプロジェクトに追加し、を`NSObject`継承します。 基本設定は、[データバインディングとキー値のコード](~/mac/app-fundamentals/databinding.md)を使用するように設計されています。これにより、基本設定フォームを作成して維持するプロセスがより簡単になります。 基本設定は少量の単純なデータ型で構成されるため、組み込み`NSUserDefaults`のを使用して値を格納および取得します。

`AppPreferences.cs`ファイルを編集し、次のように表示します。

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
        [Export("DefaultLanguage")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLanguage", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLanguage");
                SaveInt ("DefaultLanguage", value, true);
                DidChangeValue ("DefaultLanguage");
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

このクラス`SaveInt`には`LoadInt` `SaveColor`、簡単`LoadColor`に作業できるように、、、、などのいくつかのヘルパールーチンが含まれ`NSUserDefaults`ています。 また、に`NSUserDefaults`は、を処理`NSColorFromHexString` `NSColorToHexString` `NSColors`するための組み込みの方法がないため、およびメソッドを使用して、色を web ベースの`#RRGGBBAA` 16 `AA`進文字列 (ここではアルファの透明度) に変換します。簡単に保存したり取得したりできます。

ファイルで、アプリ全体で使用される**apppreferences**オブジェクトのインスタンスを作成します。 `AppDelegate.cs`

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

### <a name="wiring-preferences-to-preference-views"></a>優先順位ビューへの設定の配線

次に、[基本設定] ウィンドウと上で作成したビューの UI 要素に優先クラスを接続します。 Interface Builder で、基本設定ビューコントローラーを選択し、 **Id インスペクター**に切り替えて、コントローラーのカスタムクラスを作成します。 

[![](dialog-images/prefs12.png "Id インスペクター")](dialog-images/prefs12.png#lightbox)

Visual Studio for Mac に戻り、変更を同期し、新しく作成したクラスを編集用に開きます。 クラスは次のようになります。

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

このクラスでは、次の2つの処理が行われていることに注意してください。まず、 **appdelegate**に簡単`App`にアクセスできるようにするヘルパープロパティがあります。 次に、 `Preferences`プロパティは、このビューに配置されているすべての UI コントロールを使用して、データバインディングのグローバル**apppreferences**クラスを公開します。

次に、ストーリーボードファイルをダブルクリックして Interface Builder で再び開きます (上記の変更を確認してください)。 好みのインターフェイスを構築するために必要な UI コントロールをビューにドラッグします。 各コントロールに対して、**バインドインスペクター**に切り替え、 **apppreference**クラスの個々のプロパティにバインドします。

[![](dialog-images/prefs13.png "バインディングインスペクター")](dialog-images/prefs13.png#lightbox)

必要なすべてのパネル (ビューコントローラー) と基本設定プロパティについて、上記の手順を繰り返します。

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>すべての開いているウィンドウに基本設定の変更を適用する

前述のように、一般的な macOS アプリでは、ユーザーがアプリのユーザー設定を変更すると、それらの変更は自動的に保存され、ユーザーがアプリケーションで開いている可能性のあるすべてのウィンドウに適用されます。

アプリの基本設定とウィンドウを慎重に計画し、設計することで、このプロセスをエンドユーザーに対してスムーズかつ透過的に実行でき、最小限のコーディング作業が可能になります。

アプリの基本設定を使用するウィンドウについては、次のヘルパープロパティをコンテンツビューコントローラーに追加して、 **Appdelegate**に簡単にアクセスできるようにします。

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

次に、クラスを追加して、ユーザーの設定に基づいてコンテンツまたは動作を構成します。

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

ユーザーの設定に準拠していることを確認するために、最初にウィンドウを開いたときに、構成メソッドを呼び出す必要があります。

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

次に、 `AppDelegate.cs`ファイルを編集し、次のメソッドを追加して、すべての開いているウィンドウに基本設定の変更を適用します。

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

次に、クラス`PreferenceWindowDelegate`をプロジェクトに追加し、次のように表示します。

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWindowDelegate : NSWindowDelegate
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
        public PreferenceWindowDelegate (NSWindow window)
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

これにより、基本設定ウィンドウが閉じたときに、すべての開いているウィンドウに設定の変更が送信されます。

最後に、基本設定ウィンドウコントローラーを編集し、上で作成したデリゲートを追加します。

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
            Window.Delegate = new PreferenceWindowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

これらのすべての変更が適用され、ユーザーがアプリの設定を編集し、ユーザー設定ウィンドウを閉じると、すべての開いているウィンドウに変更が適用されます。

[![](dialog-images/prefs14.png "[基本設定] ウィンドウの例")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>[開く] ダイアログ

[開く] ダイアログボックスでは、アプリケーション内の項目を検索して開くための一貫した方法をユーザーに提供します。 開いているダイアログを Xamarin. Mac アプリケーションで表示するには、次のコードを使用します。

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

上のコードでは、新しいドキュメントウィンドウを開いて、ファイルの内容を表示しています。 このコードを、アプリケーションで必要な機能に置き換える必要があります。

を`NSOpenPanel`使用する場合は、次のプロパティを使用できます。

- **[ファイル]** -ユーザー `true`がファイルを選択できる場合はです。
- **[ディレクトリ]** -ユーザー `true`がディレクトリを選択できる場合は、
- **Allows複数選択**-ユーザー `true`が一度に複数のファイルを選択できる場合。
- **Resolvealiases** - `true`を選択してエイリアスを選択すると、元のファイルのパスに解決されます。
- **AllowedFileTypes** -拡張子または_UTI_のいずれかとしてユーザーが選択できるファイルの種類の文字列配列です。 既定値は`null`で、任意のファイルを開くことができます。

メソッド`RunModal ()`は、開く ダイアログボックスを表示し、ユーザーが **開く** ボタンをクリックした`1`ときに、ファイルまたはディレクトリを選択できるようにします。

[開く] ダイアログボックスで`URL`は、ユーザーが選択したファイルまたはディレクトリが、プロパティの url の配列として返されます。

プログラムを実行し、 **[ファイル]** メニューから **[開く]** 項目を選択すると、次のように表示されます。 

[![](dialog-images/dialog03.png "[開く] ダイアログボックス")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>[印刷とページの設定] ダイアログ

macOS には、標準の印刷とページ設定のダイアログボックスが用意されており、ユーザーは使用するすべてのアプリケーションで一貫した印刷機能を使用できます。

次のコードは、標準の [印刷] ダイアログボックスを示しています。

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

`ShowPrintAsSheet`プロパティをに`false`設定し、アプリケーションを実行して [印刷] ダイアログを表示すると、次のように表示されます。

[![](dialog-images/print01.png "[印刷] ダイアログボックス")](dialog-images/print01.png#lightbox)

`ShowPrintAsSheet`プロパティをに設定する`true`と、アプリケーションが実行されて [印刷] ダイアログボックスが表示され、次のように表示されます。

[![](dialog-images/print02.png "印刷シート")](dialog-images/print02.png#lightbox)

次のコードは、[ページレイアウト] ダイアログを表示します。

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

`ShowPrintAsSheet`プロパティをに`false`設定し、アプリケーションを実行して [印刷レイアウト] ダイアログを表示すると、次のように表示されます。

[![](dialog-images/print03.png "ページ設定ダイアログ")](dialog-images/print03.png#lightbox)

`ShowPrintAsSheet`プロパティをに`true`設定してアプリケーションを実行し、[印刷レイアウト] ダイアログを表示すると、次のように表示されます。

[![](dialog-images/print04.png "ページ設定シート")](dialog-images/print04.png#lightbox)

[印刷] および [ページ設定] ダイアログの操作の詳細については、Apple の[NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092)、 [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) 、および[印刷に](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1)関するドキュメントの概要を参照してください。

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>[保存] ダイアログ

[保存] ダイアログを使用すると、アプリケーションにアイテムを保存するための一貫した方法をユーザーが選択できます。

次のコードは、標準の [保存] ダイアログを示しています。

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

`AllowedFileTypes`プロパティは、ユーザーがファイルを保存するために選択できるファイルの種類の文字列配列です。 ファイルの種類は、拡張子または_UTI_として指定できます。 既定値は`null`で、任意のファイルの種類を使用できます。

`ShowSaveAsSheet`プロパティを`false`に設定した場合は、アプリケーションを実行し、 **[ファイル]** メニューから **[名前を付けて保存...]** を選択すると、次の内容が表示されます。

[![](dialog-images/save01.png "[保存] ダイアログボックス")](dialog-images/save01.png#lightbox)

ユーザーは、ダイアログを展開できます。

[![](dialog-images/save02.png "展開された [保存] ダイアログボックス")](dialog-images/save02.png#lightbox)

`ShowSaveAsSheet`プロパティを`true`に設定した場合は、アプリケーションを実行し、 **[ファイル]** メニューから **[名前を付けて保存...]** を選択すると、次の内容が表示されます。

[![](dialog-images/save03.png "保存シート")](dialog-images/save03.png#lightbox)

ユーザーは、ダイアログを展開できます。

[![](dialog-images/save04.png "展開された保存シート")](dialog-images/save04.png#lightbox)

[保存] ダイアログの操作の詳細については、Apple の[Nssavepanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098)のドキュメントを参照してください。

<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションでのモーダルウィンドウ、シート、および標準システムダイアログボックスの使用方法について詳しく説明しました。 モーダルウィンドウ、シート、およびダイアログのさまざまな種類と使用方法、Xcode の Interface Builder でモーダルウィンドウとシートを作成および管理する方法、およびコードでC#モーダルウィンドウ、シート、およびダイアログを操作する方法について説明しました。

## <a name="related-links"></a>関連リンク

- [MacWindows (サンプル)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [メニュー](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [ツールバー](~/mac/user-interface/toolbar.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [シートの概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [印刷の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
