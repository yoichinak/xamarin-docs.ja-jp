---
title: Xamarin. Mac の標準コントロール
description: この記事では、Xamarin. Mac アプリケーションでのボタン、ラベル、テキストフィールド、チェックボックス、セグメント化されたコントロールなど、標準的な AppKit コントロールの操作について説明します。 Interface Builder を使用してインターフェイスに追加し、コードでそれらを操作する方法について説明します。
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: d9ea7a822b8b841df682a20a70d9231996a17d3d
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436696"
---
# <a name="standard-controls-in-xamarinmac"></a>Xamarin. Mac の標準コントロール

_この記事では、Xamarin. Mac アプリケーションでのボタン、ラベル、テキストフィールド、チェックボックス、セグメント化されたコントロールなど、標準的な AppKit コントロールの操作について説明します。Interface Builder を使用してインターフェイスに追加し、コードでそれらを操作する方法について説明します。_

Xamarin. Mac アプリケーションで C# と .NET を使用する場合、 *Xcode および*で作業している開発者が行う*のと同じ*appkit コントロールにアクセスできます。 Xcode は直接統合されているため、Xcode の _Interface Builder_ を使用して Appkit コントロールを作成して管理することができます (または、必要に応じて C# コードで直接作成することもできます)。

AppKit コントロールは、Xamarin. Mac アプリケーションのユーザーインターフェイスを作成するために使用される UI 要素です。 これらは、ボタン、ラベル、テキストフィールド、チェックボックス、セグメント化されたコントロールなどの要素で構成され、ユーザーが操作すると、インスタントアクションや表示される結果が発生します。

[![サンプルアプリのメイン画面](standard-controls-images/intro01.png)](standard-controls-images/intro01.png#lightbox)

この記事では、Xamarin. Mac アプリケーションで AppKit コントロールを操作するための基本について説明します。 この記事で使用する主要な概念と手法について説明しているように、最初に [Hello, Mac](~/mac/get-started/hello-mac.md) の記事「 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) 」と「 [アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions) 」セクションをご覧になることを強くお勧めします。

「 [C# のクラス/メソッドを](~/mac/internals/how-it-works.md) [Xamarin. Mac の内部](~/mac/internals/how-it-works.md) ドキュメントの前に公開する」セクションを参照して `Register` `Export` ください。 c# クラスを目的の c オブジェクトと UI 要素に接続するために使用されるコマンドとコマンドについても説明します。

<a name="Introduction_to_Controls_and_Views"></a>

## <a name="introduction-to-controls-and-views"></a>コントロールとビューの概要

macOS (旧称 Mac OS X) は、AppKit フレームワークを使用して、ユーザーインターフェイスコントロールの標準セットを提供します。 これらは、ボタン、ラベル、テキストフィールド、チェックボックス、セグメント化されたコントロールなどの要素で構成され、ユーザーが操作すると、インスタントアクションや表示される結果が発生します。

すべての AppKit コントロールは、標準的な組み込みの外観を備えており、ほとんどの用途に適しています。また、ウィンドウの枠領域や、通知センターのウィジェットなどの _自然効果_ のコンテキストで使用するために、別の外観を指定することもできます。

Apple では、AppKit コントロールを使用するときに、次のガイドラインを提案しています。

- 同じビューにコントロールのサイズを混在させないようにします。
- 一般に、コントロールのサイズを垂直方向に変更することは避けてください。
- コントロール内でシステムフォントと適切なテキストサイズを使用します。
- コントロール間の適切な間隔を使用します。

詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[コントロールとビューについ](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)て」を参照してください。

<a name="Using_Controls_in_a_Window_Frame"></a>

### <a name="using-controls-in-a-window-frame"></a>ウィンドウフレームでのコントロールの使用

AppKit コントロールのサブセットには、ウィンドウのフレーム領域に含めることができる表示スタイルが含まれています。 例については、メールアプリのツールバーをご覧ください。

[![Mac ウィンドウフレーム](standard-controls-images/mailapp.png)](standard-controls-images/mailapp.png#lightbox)

- **Round Textured Button** `NSButton` のスタイルを使用して、テクスチャボタンを丸め `NSTexturedRoundedBezelStyle` ます。
- というスタイルを持つ、**丸めら**れたセグメント化されたコントロール `NSSegmentedControl` `NSSegmentStyleTexturedRounded` 。
- というスタイルを持つ、**丸めら**れたセグメント化されたコントロール `NSSegmentedControl` `NSSegmentStyleSeparated` 。
- **テクスチャのポップアップメニュー** - `NSPopUpButton` のスタイルを持つ `NSTexturedRoundedBezelStyle` 。
- のスタイルを使用して、テクスチャを丸めた**ドロップダウンメニュー** `NSPopUpButton` `NSTexturedRoundedBezelStyle` 。
- **検索バー** -A `NSSearchField` .

Apple では、AppKit コントロールをウィンドウフレームで操作するときに、次のガイドラインを提案しています。

- ウィンドウ本体では、ウィンドウフレーム固有のコントロールスタイルを使用しないでください。
- ウィンドウ枠でウィンドウ本体のコントロールまたはスタイルを使用しないでください。

詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[コントロールとビューについ](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)て」を参照してください。

<a name="Creating_a_User_Interface_in_Interface_Builder"></a>

## <a name="creating-a-user-interface-in-interface-builder"></a>Interface Builder でのユーザーインターフェイスの作成

新しい Xamarin. Mac Cocoa アプリケーションを作成すると、既定で標準の空白のウィンドウが表示されます。 このウィンドウは `.storyboard` 、プロジェクトに自動的に含まれるファイルに定義されています。 Windows のデザインを編集するには、 **ソリューションエクスプローラー**で、ファイルをダブルクリックし `Main.storyboard` ます。

[![ソリューションエクスプローラーでメインのストーリーボードを選択する](standard-controls-images/edit01.png)](standard-controls-images/edit01.png#lightbox)

これにより、Xcode の Interface Builder でウィンドウのデザインが開きます。

[![Xcode でストーリーボードを編集する](standard-controls-images/edit02.png)](standard-controls-images/edit02.png#lightbox)

ユーザーインターフェイスを作成するには、 **ライブラリインスペクター** から Interface Builder の **インターフェイスエディター** に UI 要素 (appkit コントロール) をドラッグします。 次の例では、 **垂直分割ビュー** コントロールが **ライブラリインスペクター** から薬品を作成し、 **インターフェイスエディター**でウィンドウに配置されています。

[![ライブラリから分割ビューを選択する](standard-controls-images/edit03.png)](standard-controls-images/edit03.png#lightbox)

Interface Builder でのユーザーインターフェイスの作成の詳細については、 [Xcode の概要と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) のドキュメントを参照してください。

<a name="Sizing_and_Positioning"></a>

### <a name="sizing-and-positioning"></a>サイズと位置設定

コントロールがユーザーインターフェイスに含まれている場合は、 **制約エディター** を使用して、手動で値を入力して位置とサイズを設定し、親ウィンドウまたはビューのサイズが変更されたときに、コントロールの位置とサイズを自動的に設定する方法を制御します。

[![制約の設定](standard-controls-images/edit04.png)](standard-controls-images/edit04.png#lightbox)

**Autoresizing**ボックスの外側の**赤い I ビーム**を使用して、特定の (x, y) 位置にコントロールを_貼り_付けます。 次に例を示します。 

[![制約の編集](standard-controls-images/edit05.png)](standard-controls-images/edit05.png#lightbox)

サイズを変更または移動したときに、選択したコントロール (**階層ビュー**  &  **インターフェイスエディター**内) がウィンドウまたはビューの上部および右側の位置にスタックするように指定します。 

エディターのその他の要素は、Height や Width などのプロパティを制御します。

[![高さの設定](standard-controls-images/edit06.png)](standard-controls-images/edit06.png#lightbox)

**配置エディター**を使用して、制約を持つ要素の配置を制御することもできます。

[![配置エディター](standard-controls-images/edit07.png)](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> IOS の場合 (0, 0) は画面の左上隅、macOS (0, 0) は左下隅にあります。 これは、macOS では数値の値が上と右に増加する数学的な座標系が使用されるためです。 ユーザーインターフェイスに AppKit コントロールを配置するときは、この点に注意する必要があります。

<a name="Setting_a_Custom_Class"></a>

### <a name="setting-a-custom-class"></a>カスタムクラスの設定

AppKit コントロールを操作するときは、サブクラスと既存のコントロールを使用して、そのクラスのカスタムバージョンを独自に作成する必要があります。 たとえば、ソースリストのカスタムバージョンを定義すると、次のようになります。

```csharp
using System;
using AppKit;
using Foundation;

namespace AppKit
{
    [Register("SourceListView")]
    public class SourceListView : NSOutlineView
    {
        #region Computed Properties
        public SourceListDataSource Data {
            get {return (SourceListDataSource)this.DataSource; }
        }
        #endregion

        #region Constructors
        public SourceListView ()
        {

        }

        public SourceListView (IntPtr handle) : base(handle)
        {

        }

        public SourceListView (NSCoder coder) : base(coder)
        {

        }

        public SourceListView (NSObjectFlag t) : base(t)
        {

        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

        }
        #endregion

        #region Public Methods
        public void Initialize() {

            // Initialize this instance
            this.DataSource = new SourceListDataSource (this);
            this.Delegate = new SourceListDelegate (this);

        }

        public void AddItem(SourceListItem item) {
            if (Data != null) {
                Data.Items.Add (item);
            }
        }
        #endregion

        #region Events
        public delegate void ItemSelectedDelegate(SourceListItem item);
        public event ItemSelectedDelegate ItemSelected;

        internal void RaiseItemSelected(SourceListItem item) {
            // Inform caller
            if (this.ItemSelected != null) {
                this.ItemSelected (item);
            }
        }
        #endregion
    }
}
```

ここ `[Register("SourceListView")]` で、命令はクラスを目的の C に公開し、を `SourceListView` Interface Builder で使用できるようにします。 詳細については、「 [Xamarin. Mac の内部](~/mac/internals/how-it-works.md)ドキュメント」の「 [c# クラス/メソッドを目的に公開](~/mac/internals/how-it-works.md)する」セクションを参照してください `Register` `Export` 。 c# クラスを目的の c オブジェクトと UI 要素に接続するために使用されるコマンドとコマンドについて説明しています。

上記のコードを使用して、拡張する基本型の AppKit コントロールをデザインサーフェイス (下の例では **ソースリスト**) にドラッグし、 **id インスペクター** に切り替えて、 **カスタムクラス** を、目的の C (例) に公開した名前に設定し `SourceListView` ます。次のようになります。

[![Xcode でのカスタムクラスの設定](standard-controls-images/edit10.png)](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions"></a>

### <a name="exposing-outlets-and-actions"></a>アウトレットとアクションの公開

AppKit コントロールに C# コードでアクセスできるようにするには、そのコントロールを **アウトレット** または **アクション**として公開する必要があります。 これを行うには、 **インターフェイス階層** または **インターフェイスエディター** で指定されたコントロールを選択し、 **アシスタントビュー** に切り替えます ( `.h` ウィンドウのが編集用に選択されていることを確認してください)。

[![正しいファイルを選択して編集する](standard-controls-images/edit11.png)](standard-controls-images/edit11.png#lightbox)

制御-AppKit コントロールから、 `.h` 次のように、 **アウトレット** または **アクション**の作成を開始するためのファイルを指定ファイルにドラッグします。

[![ドラッグしてアウトレットまたはアクションを作成する](standard-controls-images/edit12.png)](standard-controls-images/edit12.png#lightbox)

作成する露出の種類を選択し、 **アウトレット** または **アクション** に **名前**を付けます。 

[![アウトレットまたはアクションを構成する](standard-controls-images/edit13.png)](standard-controls-images/edit13.png#lightbox)

**アウトレット**と**アクション**の操作の詳細については、 [Xcode と Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)のドキュメントの概要に関する記事の「[アウトレットとアクション](~/mac/get-started/hello-mac.md#outlets-and-actions)」セクションを参照してください。

<a name="Synchronizing_Changes_with_Xcode"></a>

### <a name="synchronizing-changes-with-xcode"></a>Xcode との変更の同期

Xcode から Visual Studio for Mac に戻ると、Xcode で行ったすべての変更が自動的に Xamarin. Mac プロジェクトと同期されます。

ソリューションエクスプローラーでを選択すると、 `SplitViewController.designer.cs` 次の C# コードで**アウトレット** **Solution Explorer**と**アクション**がどのようにつながっているかを確認できます。

[![Xcode との変更の同期](standard-controls-images/sync01.png)](standard-controls-images/sync01.png#lightbox)

ファイル内の定義は次のように `SplitViewController.designer.cs` なります。

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

Xcode のファイル内の定義で行アップし `MainWindow.h` ます。

```csharp
@interface SplitViewController : NSSplitViewController {
    NSSplitViewItem *_LeftController;
    NSSplitViewItem *_RightController;
    NSSplitView *_SplitView;
}

@property (nonatomic, retain) IBOutlet NSSplitViewItem *LeftController;

@property (nonatomic, retain) IBOutlet NSSplitViewItem *RightController;

@property (nonatomic, retain) IBOutlet NSSplitView *SplitView;
```

ご覧のように、Visual Studio for Mac はファイルへの変更をリッスン `.h` し、それぞれのファイルでこれらの変更を自動的に同期して、 `.designer.cs` アプリケーションに公開します。 また、 `SplitViewController.designer.cs` は部分クラスであるため、クラスに加えられた変更を上書きするように Visual Studio for Mac を変更する必要がないことにも注意して `SplitViewController.cs` ください。

通常、を開く必要はありません `SplitViewController.designer.cs` 。ここでは、教育目的でのみ提供されていました。

> [!IMPORTANT]
> ほとんどの場合、Visual Studio for Mac は Xcode で行われた変更を自動的に確認し、それらを Xamarin. Mac プロジェクトに同期します。 同期が自動的に行われないときは Xcode に戻り、再び Visual Studio for Mac に戻ります。 こうすると通常、同期サイクルが開始します。

<a name="Working_with_Buttons"></a>

## <a name="working-with-buttons"></a>ボタンの操作

AppKit には、ユーザーインターフェイスの設計で使用できるいくつかの種類のボタンが用意されています。 詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[ボタン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1)」セクションを参照してください。 

[![さまざまなボタンの種類の例](standard-controls-images/buttons01.png)](standard-controls-images/buttons01.png#lightbox)

ボタンが **コンセント**経由で公開されている場合、次のコードは押されていることに応答します。

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

**アクション**によって公開されているボタンの場合、Xcode で選択した名前を使用して、 `public partial` メソッドが自動的に作成されます。 **アクション**に応答するには、**アクション**が定義されているクラスの部分メソッドを完了します。 次に例を示します。

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

状態 ( **On** や **Off**など) が設定されているボタンの場合は、その状態をチェックするか、 `State` 列挙型に対してプロパティを使用して設定できます `NSCellStateValue` 。 次に例を示します。

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

次の場所 `NSCellStateValue` を指定できます。

- **[オン** ]-ボタンがプッシュされるか、またはコントロールが選択されます (チェックボックスをオンにするなど)。
- **オフ** -ボタンが押されていないか、コントロールが選択されていません。
- **Mixed** - **オン** と **オフ** の状態の組み合わせ。

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent"></a>

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>ボタンを既定としてマークし、同等のキーを設定する

ユーザーインターフェイスのデザインに追加したボタンについては、ユーザーがキーボードの enter キー**または enter**キーを押したときにアクティブになる_既定_のボタンとしてそのボタンをマークできます。 MacOS では、このボタンは既定で青い背景色で表示されます。

ボタンを既定として設定するには、Xcode の Interface Builder でそのボタンを選択します。 次に、 **属性インスペクター**で、同等の **キー** フィールドを選択し、Enter キー **または enter** キーを押します。

[![同等のキーの編集](standard-controls-images/buttons03.png)](standard-controls-images/buttons03.png#lightbox)

同様に、マウスの代わりにキーボードを使用してボタンをアクティブ化するために使用できる任意のキーシーケンスを割り当てることができます。 たとえば、上の図のコマンド-C キーを押します。

アプリが実行され、ボタンのあるウィンドウがキーでフォーカスが設定されている場合、ユーザーがコマンド C を押すと、ボタンの操作がアクティブになります (ユーザーがボタンをクリックした場合と同様)。

<a name="Working_with_Checkboxes_and_Radio_Buttons"></a>

## <a name="working-with-checkboxes-and-radio-buttons"></a>チェックボックスとオプションボタンの操作

AppKit には、ユーザーインターフェイスの設計で使用できる複数の種類のチェックボックスとオプションボタングループが用意されています。 詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[ボタン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1)」セクションを参照してください。 

[![使用可能な checkbox 型の例](standard-controls-images/buttons02.png)](standard-controls-images/buttons02.png#lightbox)

チェックボックスおよびラジオボタン ( **コンセント**経由で公開されます) には状態 ( **オン** と **オフ**など) があり、 `State` 列挙型に対してプロパティを使用して状態を確認または設定できます `NSCellStateValue` 。 次に例を示します。

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

次の場所 `NSCellStateValue` を指定できます。

- **[オン** ]-ボタンがプッシュされるか、またはコントロールが選択されます (チェックボックスをオンにするなど)。
- **オフ** -ボタンが押されていないか、コントロールが選択されていません。
- **Mixed** - **オン** と **オフ** の状態の組み合わせ。

ラジオボタングループのボタンを選択するには、ラジオボタンを **コンセント** として選択し、そのプロパティを設定し `State` ます。 たとえば次のようになります。

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

グループとして機能するラジオボタンのコレクションを取得し、選択した状態を自動的に処理するには、新しい **アクション** を作成し、グループ内のすべてのボタンをそれにアタッチします。

![新しいアクションの作成](standard-controls-images/buttons04.png)

次に、 `Tag` **属性インスペクター**の各ラジオボタンに一意のを割り当てます。

![ラジオボタンのタグの編集](standard-controls-images/buttons05.png)

変更を保存し Visual Studio for Mac に戻り、すべてのオプションボタンがアタッチされている **アクション** を処理するコードを追加します。

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

プロパティを使用し `Tag` て、どのオプションボタンが選択されたかを確認できます。

<a name="Working_with_Menu_Controls"></a>

## <a name="working-with-menu-controls"></a>メニューコントロールの操作

AppKit には、ユーザーインターフェイスの設計で使用できるさまざまな種類のメニューコントロールが用意されています。 詳細については、Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)の[メニューコントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1)に関するセクションを参照してください。 

[![メニューコントロールの例](standard-controls-images/menu01.png)](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data"></a>

### <a name="providing-menu-control-data"></a>メニューコントロールデータの提供

MacOS で使用できるメニューコントロールは、内部リスト (Interface Builder で事前に定義されているか、コードで設定可能)、または独自のカスタムの外部データソースを提供することによってドロップダウンリストに入力するように設定できます。

<a name="Working-with-Internal-Data"></a>

#### <a name="working-with-internal-data"></a>内部データの操作

Interface Builder での項目の定義に加えて、メニューコントロール (など) には、 `NSComboBox` 保持する内部リストの項目を追加、編集、または削除できるメソッドの完全なセットが用意されています。

- `Add` -新しい項目をリストの末尾に追加します。
- `GetItem` -指定されたインデックス位置にある項目を返します。
- `Insert` -指定された場所のリストに新しい項目を挿入します。
- `IndexOf` -指定された項目のインデックスを返します。
- `Remove` -指定された項目を一覧から削除します。
- `RemoveAll` -リストからすべての項目を削除します。
- `RemoveAt` -指定されたインデックス位置にある項目を削除します。
- `Count` -リスト内の項目の数を返します。

> [!IMPORTANT]
> Extern データソース () を使用している場合は、 `UsesDataSource = true` 上記のメソッドのいずれかを呼び出すと例外がスローされます。

<a name="Working-with-an-External-Data-Source"></a>

#### <a name="working-with-an-external-data-source"></a>外部データソースの操作

組み込みの内部データを使用してメニューコントロールの行を提供するのではなく、必要に応じて外部データソースを使用して、項目のバッキングストア (SQLite データベースなど) を提供できます。

外部データソースを操作するには、メニューコントロールのデータソースのインスタンスを作成 `NSComboBoxDataSource` し (たとえば)、必要なデータを提供するいくつかのメソッドをオーバーライドします。

- `ItemCount` -リスト内の項目の数を返します。
- `ObjectValueForItem` -指定されたインデックスの項目の値を返します。
- `IndexOfItem` -指定された項目の値のインデックスを返します。
- `CompletedString` -部分的に型指定された項目の値について、最初に一致した項目の値を返します。 このメソッドは、オートコンプリートが有効になっている場合にのみ呼び出され `Completes = true` ます ()。

詳細については、[データベースの操作](~/mac/app-fundamentals/databases.md)に関するドキュメントの「[データベースとコンボ](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes)セクション」を参照してください。

<a name="Adjusting-the-Lists-Appearance"></a>

### <a name="adjusting-the-lists-appearance"></a>リストの外観を調整する

メニューコントロールの外観を調整するには、次のメソッドを使用できます。

- `HasVerticalScroller` -の場合 `true` 、コントロールに垂直スクロールバーが表示されます。 
- `VisibleItems` -コントロールを開いたときに表示される項目の数を調整します。 既定値は5です。
- `IntercellSpacing` -が左余白と右余白を指定し、が項目の前後のスペースを指定するを指定して、指定した項目の周囲の余白を調整 `NSSize` `Width` し `Height` ます。
- `ItemHeight` -リスト内の各項目の高さを指定します。

のドロップダウン型の場合 `NSPopupButtons` 、最初のメニュー項目にコントロールのタイトルが表示されます。 たとえば次のようになります。 

[![メニューコントロールの例](standard-controls-images/menu02.png)](standard-controls-images/menu02.png#lightbox)

タイトルを変更するには、この項目を **アウトレット** として公開し、次のようなコードを使用します。

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items"></a>

### <a name="manipulating-the-selected-items"></a>選択した項目の操作

次のメソッドとプロパティを使用すると、メニューコントロールの一覧で選択した項目を操作できます。

- `SelectItem` -指定されたインデックス位置にある項目を選択します。
- `Select` -指定された項目の値を選択します。
- `DeselectItem` -指定されたインデックス位置にある項目を選択解除します。
- `SelectedIndex` -現在選択されている項目のインデックスを返します。
- `SelectedValue` -現在選択されている項目の値を返します。

を使用して、 `ScrollItemAtIndexToTop` リストの先頭にある指定したインデックス位置に項目を表示し、をスクロールして、指定した `ScrollItemAtIndexToVisible` インデックスの項目が表示されるまで一覧をスクロールします。

<a name="Responding to Events"></a>

### <a name="responding-to-events"></a>イベントへの応答

メニューコントロールは、ユーザーの操作に応答するために次のイベントを提供します。

- `SelectionChanged` -ユーザーがリストから値を選択したときに呼び出されます。
- `SelectionIsChanging` -新しいユーザーが選択した項目がアクティブな選択項目になる前に、が呼び出されます。
- `WillPopup` -項目のドロップダウンリストが表示される前に、が呼び出されます。
- `WillDismiss` -項目のドロップダウンリストが閉じられる前に、が呼び出されます。

コントロールの場合は、 `NSComboBox` `NSTextField` `Changed` ユーザーがコンボボックス内のテキストの値を編集するたびに呼び出されるイベントなど、と同じイベントのすべてが含まれます。

必要に応じて、項目を **アクション** にアタッチし、次のようなコードを使用して、ユーザーによってトリガーされる **アクション** に応答することによって、選択 Interface Builder で定義されている内部データメニュー項目に応答できます。

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

メニューとメニューコントロールを操作する方法の詳細については、 [メニュー](~/mac/user-interface/menu.md) と [ポップアップボタン](~/mac/user-interface/menu.md) のドキュメントを参照してください。

<a name="Working_with_Selection_Controls"></a>

## <a name="working-with-selection-controls"></a>選択コントロールの操作

AppKit には、ユーザーインターフェイスの設計で使用できるいくつかの種類の選択コントロールが用意されています。 詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[選択コントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1)」セクションを参照してください。 

[![選択範囲コントロールの例](standard-controls-images/select01.png)](standard-controls-images/select01.png#lightbox)

選択コントロールにユーザー操作があるタイミングを追跡するには、2つの方法があります。これは、選択コントロールを **アクション**として公開することによって行います。 次に例を示します。

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

または、イベントに **デリゲート** をアタッチし `Activated` ます。 次に例を示します。

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

選択コントロールの値を設定または読み取るには、プロパティを使用し `IntValue` ます。 次に例を示します。

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

特殊なコントロール (色ウェルやイメージウェルなど) には、その値の型に固有のプロパティがあります。 たとえば次のようになります。

```csharp
ColorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

には、 `NSDatePicker` 日付と時刻を直接操作するための次のプロパティがあります。

- **DateValue** -現在の日付と時刻の値をとして指定し `NSDate` ます。
- **Local** -としてのユーザーの場所 `NSLocal` 。
- **Timeinterval** -としての時間の値 `Double` 。
- **TimeZone** -としてのユーザーのタイムゾーン `NSTimeZone` 。

<a name="Working_with_Indicator_Controls"></a>

## <a name="working-with-indicator-controls"></a>インジケーターコントロールの操作

AppKit には、ユーザーインターフェイスの設計で使用できるいくつかの種類のインジケーターコントロールが用意されています。 詳細については、Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)の[インジケーターコントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1)に関するセクションを参照してください。 

[![インジケーターコントロールの例](standard-controls-images/level01.png)](standard-controls-images/level01.png#lightbox)

インジケーターコントロールにユーザーの操作があるタイミングを追跡するには、次の2つの方法があります。これは、インジケーターコントロールを **アクション** または **アウトレット** として公開し、 **デリゲート** をイベントにアタッチすることによって行い `Activated` ます。 次に例を示します。

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

インジケーターコントロールの値を読み取りまたは設定するには、プロパティを使用し `DoubleValue` ます。 次に例を示します。

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

表示すると、不確定および非同期の進行状況インジケーターをアニメーション化する必要があります。 アニメーションが `StartAnimation` 表示されたら、メソッドを使用してアニメーションを開始します。 次に例を示します。

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

メソッドを呼び出すと `StopAnimation` 、アニメーションが停止します。

<a name="Working_with_Text_Controls"></a>

## <a name="working-with-text-controls"></a>テキストコントロールの操作

AppKit には、ユーザーインターフェイスの設計で使用できるさまざまな種類のテキストコントロールが用意されています。 詳細については、Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)の[テキストコントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1)に関するセクションを参照してください。 

[![テキストコントロールの例](standard-controls-images/text01.png)](standard-controls-images/text01.png#lightbox)

テキストフィールド () の場合 `NSTextField` 、次のイベントを使用してユーザーの操作を追跡できます。

- **Changed** -ユーザーがフィールドの値を変更するたびに発生します。 たとえば、すべての文字が入力された場合などです。
- 編集が**開始**されました-ユーザーが編集のためにフィールドを選択すると、が発生します。
- **編集が終了** -ユーザーがフィールドで enter キーを押すか、フィールドの外に移動したとき。

プロパティを使用して、 `StringValue` フィールドの値を読み取りまたは設定します。 次に例を示します。

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

数値を表示または編集するフィールドの場合は、プロパティを使用でき `IntValue` ます。 次に例を示します。

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

には、 `NSTextView` 組み込みの書式設定を備えた、すべての機能を備えたテキストの編集と表示領域が用意されています。 と同様に、プロパティを使用して、 `NSTextField` `StringValue` 領域の値を読み取ったり設定したりします。

Xamarin. Mac アプリでテキストビューを操作する複雑な例の例については、 [Sourcewriter サンプルアプリ](/samples/xamarin/mac-samples/sourcewriter)を参照してください。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter コード全体に詳細なコメントが付いていて、可能な場合は、重要な技術やメソッド、Xamarin.Mac ガイド ドキュメントの関連情報へのリンクが示されます。

<a name="Working_with_Content_Views"></a>

## <a name="working-with-content-views"></a>コンテンツビューの操作

AppKit には、ユーザーインターフェイスの設計で使用できるさまざまな種類のコンテンツビューが用意されています。 詳細については、「Apple の[OS X ヒューマンインターフェイスガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)」の「[コンテンツビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) 」セクションを参照してください。

[![コンテンツビューの例](standard-controls-images/content01.png)](standard-controls-images/content01.png#lightbox)

<a name="Popovers"></a>

### <a name="popovers"></a>マイネーム

Segue は、特定のコントロールまたは画面領域に直接関連する機能を提供する一時的な UI 要素です。 Segue は、それが関連付けられているコントロールまたは領域を含むウィンドウの上にフローティングし、その境界線には、それが出現するポイントを示す矢印が含まれます。

Segue を作成するには、次の手順を実行します。

1. Segue を `.storyboard` 追加するウィンドウのファイルを開きます。これを行うには、**ソリューションエクスプローラー**でダブルクリックします。
2. **ビューコントローラー**を**ライブラリインスペクター**から**インターフェイスエディター**にドラッグします。 

    [![ライブラリからビューコントローラーを選択する](standard-controls-images/content02.png)](standard-controls-images/content02.png#lightbox)
3. **カスタムビュー**のサイズとレイアウトを定義します。 

    [![レイアウトの編集](standard-controls-images/content04.png)](standard-controls-images/content04.png#lightbox)
4. コントロールをクリックし、ポップアップのソースから **ビューコントローラー**にドラッグします。 

    [![ドラッグしてセグエを作成する](standard-controls-images/content05.png)](standard-controls-images/content05.png#lightbox)
5. ポップアップメニューから [ **segue** ] を選択します。 

    [![セグエ型の設定](standard-controls-images/content06.png)](standard-controls-images/content06.png#lightbox)
6. 変更を保存し Visual Studio for Mac に戻り、Xcode と同期します。

<a name="Tab_Views"></a>

### <a name="tab-views"></a>タブビュー

タブビューは、タブリスト (セグメント化されたコントロールに似ています) と、 _ペイン_と呼ばれるビューのセットで構成されます。 ユーザーが新しいタブを選択すると、そのタブに接続されているウィンドウが表示されます。 各ペインには、独自のコントロールのセットが含まれています。

Xcode の Interface Builder でタブビューを操作するときは、 **属性インスペクター** を使用してタブの数を設定します。

[![タブの数の編集](standard-controls-images/content08.png)](standard-controls-images/content08.png#lightbox)

**インターフェイス階層**内の各タブを選択して、その**タイトル**を設定し、UI 要素を**ペイン**に追加します。

[![Xcode のタブの編集](standard-controls-images/content09.png)](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls"></a>

## <a name="data-binding-appkit-controls"></a>データバインディング AppKit コントロール

UI 要素を設定して操作するために、Xamarin. Mac アプリケーションでキー値のコードとデータバインディングの手法を使用することにより、記述して維持する必要があるコードの量を大幅に減らすことができます。 また、フロントエンドのユーザーインターフェイス (_モデルビューコントローラー_) からバッキングデータ (_データモデル_) をさらに分離することもできます。これにより、管理が容易になり、アプリケーションの設計をより柔軟に行うことができます。

キー値のコーディング (KVC) は、オブジェクトのプロパティに間接的にアクセスするためのメカニズムです。キー (特殊な書式設定文字列) を使用して、インスタンス変数またはアクセサーメソッド () を使用してアクセスするのではなく、プロパティを識別し `get/set` ます。 Xamarin. Mac アプリケーションでキー値のコーディングに準拠したアクセサーを実装することによって、キー値の観察 (KVO)、データバインディング、コアデータ、Cocoa バインド、および scriptability などの他の macOS 機能にアクセスできます。

詳細については、[データバインディングとキー値のコーディング](~/mac/app-fundamentals/databinding.md)に関するドキュメントの「[単純なデータバインディング](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding)」を参照してください。

<a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、Xamarin. Mac アプリケーションで、ボタン、ラベル、テキストフィールド、チェックボックス、セグメント化されたコントロールなど、標準的な AppKit コントロールを操作する方法について詳しく説明しました。 ここでは、Xcode の Interface Builder のユーザーインターフェイス設計に追加し、アウトレットとアクションを使用してコードに公開し、C# コードで AppKit コントロールを使用する方法について説明します。

## <a name="related-links"></a>関連リンク

- [MacControls (サンプル)](/samples/xamarin/mac-samples/maccontrols)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [コントロールとビューについて](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)