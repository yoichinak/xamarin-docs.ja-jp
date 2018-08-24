---
title: Xamarin.Mac で標準のコントロール
description: この記事では、チェック ボックス、ボタン、ラベル、テキスト フィールドなどの標準の AppKit コントロールの操作について説明し、Xamarin.Mac アプリケーションでコントロールをセグメント化されました。 これは、Interface Builder を使用して、インターフェイスに追加することで、コードで操作するについて説明します。
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 1760e764c4918f597c95a4c811be9166e33bb1f3
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/13/2018
ms.locfileid: "40250947"
---
# <a name="standard-controls-in-xamarinmac"></a>Xamarin.Mac で標準のコントロール

_この記事では、チェック ボックス、ボタン、ラベル、テキスト フィールドなどの標準の AppKit コントロールの操作について説明し、Xamarin.Mac アプリケーションでコントロールをセグメント化されました。これは、Interface Builder を使用して、インターフェイスに追加することで、コードで操作するについて説明します。_

同じへのアクセス、Xamarin.Mac アプリケーションで c# と .NET を使用する場合がある AppKit コントロールで作業する開発者*Objective C*と*Xcode*は。 Xamarin.Mac は直接 Xcode と統合、ためには、Xcode を使用して_Interface Builder_を作成し、Appkit コントロールを維持 (または必要に応じて c# コードで直接作成) します。

AppKit コントロールは、Xamarin.Mac アプリケーションのユーザー インターフェイスの作成に使用する UI 要素を示します。 ボタン、ラベル、テキスト フィールド、チェック ボックスおよびセグメント付きコントロールなどの要素で構成され、ユーザーがそれらを操作するときに、インスタント アクションまたは結果が表示を発生させます。

[![](standard-controls-images/intro01.png "サンプル アプリのメイン画面")](standard-controls-images/intro01.png#lightbox)

この記事では、Xamarin.Mac アプリケーションで AppKit コントロールの操作の基礎を取り上げます。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)と[Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)ほどのセクションでは、主要な概念と、この記事で使用する方法について説明します。

見てしたい場合があります、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)、について説明します、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素を c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>コントロールとビューの概要

macOS (旧称 Mac OS X) は、AppKit フレームワークを使用してユーザー インターフェイス コントロールの標準セットを提供します。 ボタン、ラベル、テキスト フィールド、チェック ボックスおよびセグメント付きコントロールなどの要素で構成され、ユーザーがそれらを操作するときに、インスタント アクションまたは結果が表示を発生させます。

ほとんどの用途に適してされる標準的な組み込みの外観を与えるすべての AppKit コントロール、ウィンドウ フレームの領域または使用するための代替の外観をいくつか指定、_自然な彩度効果_サイドバー領域のようにやなどのコンテキストを通知センター ウィジェット。

AppKit コントロールを使用する場合に、Apple は、次のガイドラインをお勧めします。

- 同じビューでのコントロールのサイズの混在を回避します。
- 一般に、コントロールの垂直方向にサイズ変更しないでください。
- システム フォントと、コントロール内で適切なテキストのサイズを使用します。
- コントロールの適切な間隔を使用します。

詳細については、pleas を参照してください、[についてコントロールとビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>ウィンドウ フレームでコントロールの使用

ウィンドウのフレームの領域にすることが可能な表示スタイルを含む AppKit コントロールのサブセットがあります。 たとえば、メール アプリのツールバーを参照してください。

[![](standard-controls-images/mailapp.png "Mac のウィンドウ フレーム")](standard-controls-images/mailapp.png#lightbox)

- **ボタンのテクスチャを丸める**-`NSButton`のスタイルと`NSTexturedRoundedBezelStyle`します。
- **セグメント化されたコントロールの丸めをテクスチャ**-`NSSegmentedControl`のスタイルと`NSSegmentStyleTexturedRounded`します。
- **セグメント化されたコントロールの丸めをテクスチャ**-`NSSegmentedControl`のスタイルと`NSSegmentStyleSeparated`します。
- **ポップアップ メニューのテクスチャを丸める**-`NSPopUpButton`のスタイルと`NSTexturedRoundedBezelStyle`します。
- **ドロップダウン メニューのテクスチャを丸める**-`NSPopUpButton`のスタイルと`NSTexturedRoundedBezelStyle`します。
- **検索バー** -`NSSearchField`します。

Apple では、ウィンドウ フレームの AppKit コントロールを使用する場合、次のガイドラインをお勧めします。

- ウィンドウの本文には、ウィンドウ フレームの特定のコントロールのスタイルを使用しないでください。
- ウィンドウ フレームには、本文のウィンドウのコントロールやスタイルを使用しないでください。

詳細については、pleas を参照してください、[についてコントロールとビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>インターフェイス ビルダーでのユーザー インターフェイスを作成します。

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、`.storyboard`プロジェクトに自動的に含まれるファイル。 Windows のデザインを編集する、**ソリューション エクスプ ローラー**、ダブルクリック、`Main.storyboard`ファイル。

[![](standard-controls-images/edit01.png "ソリューション エクスプ ローラーで、メインのストーリー ボードを選択します。")](standard-controls-images/edit01.png#lightbox)

これが、Xcode の Interface Builder でウィンドウのデザインを開きます。

[![](standard-controls-images/edit02.png "Xcode でストーリー ボードの編集")](standard-controls-images/edit02.png#lightbox)

ユーザー インターフェイスを作成するから UI 要素 (AppKit コントロール) をドラッグします、**ライブラリ インスペクター**を**インターフェイス エディター**インターフェイス ビルダーでします。 次の例で、**垂直分割ビュー**コントロールになったから薬、**ライブラリ インスペクター**でウィンドウに配置し、**インターフェイス エディター**:

[![](standard-controls-images/edit03.png "ライブラリから分割ビューを選択します。")](standard-controls-images/edit03.png#lightbox)

インターフェイス ビルダーでのユーザー インターフェイスを作成する方法の詳細についてを参照してください、 [Xcode と Interface Builder の概要](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)ドキュメント。

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>配置してサイズ変更

コントロールは、ユーザー インターフェイスに含まれている、使用して、**制約エディター**値を手動で入力してその場所とサイズを設定し、コントロールが自動的に配置されているし、ときにサイズを制御する親ウィンドウまたはビューサイズを変更します。

[![](standard-controls-images/edit04.png "制約の設定")](standard-controls-images/edit04.png#lightbox)

使用して、**赤い I ビーム**の外側に、 **Autoresizing**ボックス_スティック_(x, y) 特定の場所にコントロール。 例えば: 

[![](standard-controls-images/edit05.png "制約の編集")](standard-controls-images/edit05.png#lightbox)

指定します、選択したコントロール (で、**階層ビュー** & **インターフェイス エディター**) がサイズ変更または移動するように、ウィンドウまたはビューの上および右の場所を停止します。 

エディターの他の要素は、高さや幅などのプロパティを制御します。

[![](standard-controls-images/edit06.png "高さの設定")](standard-controls-images/edit06.png#lightbox)

使用して制約を持つ要素の配置を制御することも、**配置エディター**:

[![](standard-controls-images/edit07.png "配置エディター")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> IOS とは異なり、(0, 0) 上にあるの macOS (0, 0) で、画面の左上隅が左下隅。 MacOS 上および右に値が増加し、番号の値を数学的座標系を使用しているためにです。 このユーザー インターフェイスの AppKit コントロールを配置するときに考慮に考慮する必要があります。

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>カスタム クラスの設定

AppKit コントロールを使用したはサブクラス化し、既存のコントロールする必要があり、そのクラスの独自のカスタム バージョンを作成するもあります。 たとえば、ソース リストのカスタム バージョンを定義します。

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

場所、`[Register("SourceListView")]`命令が公開、`SourceListView`インターフェイス ビルダーでは、OBJECTIVE-C になるようにクラスを使用できます。 詳細についてを参照してください、 [c# を公開するクラス/メソッドを OBJECTIVE-C](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac の内部](~/mac/internals/how-it-works.md)を文書化、について説明します、`Register`と`Export`コマンドの使用ワイヤを c# クラス OBJECTIVE-C オブジェクトを UI 要素。

場所で上記のコードでは、デザイン サーフェイスに AppKit コントロールを拡張する基本の型をドラッグできます (次の例で、**ソース リスト**) に切り替える、 **Id インスペクター**設定**カスタム クラス**を OBJECTIVE-C に公開する名前 (例`SourceListView`)。

[![](standard-controls-images/edit10.png "Xcode でのカスタム クラスの設定")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>公開する Outlet と Action

いずれかとして公開する必要がある、AppKit コントロールは、c# コードでアクセスできる、前に、**アウトレット**またはと**アクション**します。 いずれかで指定されたコントロールで選択これを行うには**インターフェイス階層**または**インターフェイス エディター**に切り替えると、**アシスタント ビュー** (、があることを確認`.h`編集用に選択されたウィンドウの)。

[![](standard-controls-images/edit11.png "編集する正しいファイルを選択します。")](standard-controls-images/edit11.png#lightbox)

コントロールをドラッグ、付与に AppKit コントロールから`.h`作成を開始するファイル、**アウトレット**または**アクション**:

[![](standard-controls-images/edit12.png "ドラッグして、コンセントまたはアクションを作成するには")](standard-controls-images/edit12.png#lightbox)

作成し、脅威の種類の選択、**アウトレット**または**アクション**、**名前**: 

[![](standard-controls-images/edit13.png "アウトレットまたはアクションを構成します。")](standard-controls-images/edit13.png#lightbox)


操作の詳細については**Outlet**と**アクション**を参照してください、 [Outlet と Action](~/mac/get-started/hello-mac.md#outlets-and-actions)のセクション、 [Xcode とインターフェイスの概要ビルダー](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder)ドキュメント。

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Xcode との変更の同期

切り替えたときに Visual Studio for Mac Xcode、Xcode で行った変更に自動的に Xamarin.Mac プロジェクトと同期されています。

選択した場合、`SplitViewController.designer.cs`で、**ソリューション エクスプ ローラー**表示することができる方法、**アウトレット**と**アクション**c# コードで接続されています。

[![](standard-controls-images/sync01.png "Xcode と変更の同期")](standard-controls-images/sync01.png#lightbox)

通知方法の定義、`SplitViewController.designer.cs`ファイル。

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

内に定義行、 `MainWindow.h` Xcode でファイル。

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

Visual Studio for Mac がの変更をリッスンするよう、`.h`ファイルを開き、それぞれでこれらの変更を自動的に同期`.designer.cs`ファイル、アプリケーションに公開します。 場合もあります`SplitViewController.designer.cs`部分クラスは、Visual Studio for Mac は、変更する必要があるないように`SplitViewController.cs `クラスに加え変更を上書きします。

通常必要はありませんを開く、 `SplitViewController.designer.cs` 、自分でこれがここで紹介教育目的のみ。

> [!IMPORTANT]
> ほとんどの場合、Visual Studio for Mac が自動的に Xcode で加えられた変更を参照してくださいし、し、Xamarin.Mac プロジェクトに同期されます。 同期が自動的に行われないときは Xcode に戻り、再び Visual Studio for Mac に戻ります。 こうすると通常、同期サイクルが開始します。

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>ボタンの操作

AppKit では、複数の種類のユーザー インターフェイスの設計で使用できるボタンを提供します。 詳細についてを参照してください、[ボタン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。 

[![](standard-controls-images/buttons01.png "ボタンのさまざまな種類の例")](standard-controls-images/buttons01.png#lightbox)

ボタンが経由で公開されている場合、**アウトレット**、次のコードが押されていることに応答します。

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

経由で公開されているボタンの**アクション**、`public partial`メソッドが自動的に作成されますが Xcode で選択した名前を持つ。 応答する、**アクション**、クラスの部分メソッドを完了する、**アクション**で定義されていた。 例えば:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

状態のボタンの (など**で**と**オフ**)、状態をチェックまたはに設定できる、`State`プロパティに対して、`NSCellStateValue`列挙型。 例えば:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

場所`NSCellStateValue`を指定できます。

- **** - ボタンをクリックするか (チェック ボックスのチェック) など、コントロールが選択されています。
- **オフ**-、ボタンが押されていないか、コントロールが選択されていません。
- **混合**-混在**で**と**オフ**状態。

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>既定値し、キーと同等の設定ボタンをマークします。

ユーザー インターフェイスのデザインに追加した任意のボタンとしてそのボタンを指定することができます、_既定_ボタンを押したときにアクティブになる、**戻り値/Enter**キーボードのキー。 MacOS、このボタンは既定では青色の背景色を受信します。

ボタンを既定として設定するには、Xcode の Interface Builder で選択します。 次に、**属性インスペクター**を選択、**キーと等価**フィールドとキーを押して、**戻り値/Enter**キー。

[![](standard-controls-images/buttons03.png "キーと同等の編集")](standard-controls-images/buttons03.png#lightbox)

同様に、マウスではなくキーボードを使用して、ボタンをアクティブに使用できる任意のキー シーケンスを割り当てることができます。 たとえば、上の図でコマンドを押しながら C キーを押します。

ときに、アプリの実行しボタンで、ウィンドウは、キーと重点を置いた、ユーザーがコマンドを押しながら C キーを押した場合、ボタンのアクションが有効に (として- ボタンをクリックした場合)。

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>チェック ボックスおよびラジオ ボタンの使用

AppKit では、複数の種類のチェック ボックスやオプション ボタン グループにユーザー インターフェイスの設計で使用できるを提供します。 詳細についてを参照してください、[ボタン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。 

[![](standard-controls-images/buttons02.png "チェック ボックスを使用可能な型の例")](standard-controls-images/buttons02.png#lightbox)


チェック ボックスおよびラジオ ボタン (を介して公開される**Outlet**) 状態になります (など**で**と**オフ**)、状態をチェックまたはに設定できる、`State`プロパティに対して、`NSCellStateValue`列挙型。 例えば:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

場所`NSCellStateValue`を指定できます。

- **** - ボタンをクリックするか (チェック ボックスのチェック) など、コントロールが選択されています。
- **オフ**-、ボタンが押されていないか、コントロールが選択されていません。
- **混合**-混在**で**と**オフ**状態。

ラジオ ボタン グループのボタンを選択する公開として選択するラジオ ボタン、**アウトレット**設定とその`State`プロパティ。 たとえば、次のように入力します。

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

グループとして機能し、選択された状態を自動的に処理するラジオ ボタンのコレクションを取得するには、新規作成**アクション**して、グループ内のすべてのボタンをアタッチします。

![](standard-controls-images/buttons04.png "新しいアクションを作成します。")

次に、割り当てる一意`Tag`ラジオ ボタンごとに、**属性インスペクター**:

![](standard-controls-images/buttons05.png "ラジオ ボタンのタグの編集")

変更を保存し、for Mac に Visual Studio に戻り、処理するコードを追加、**アクション**に関連付けられているすべてのラジオ ボタン。

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

使用することができます、`Tag`プロパティを選択したラジオ ボタンを参照してください。

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>メニュー コントロールの操作

AppKit では、いくつかの種類のユーザー インターフェイスの設計で使用できるメニュー コントロールを提供します。 詳細についてを参照してください、[メニュー コントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。 

[![](standard-controls-images/menu01.png "メニュー コントロールの例")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>メニュー コントロールのデータを提供します。

(またはできる、Interface Builder で事前定義されたコードを使用して設定されます) 内部の一覧からいずれか、ドロップダウン リストの設定を macOS に使用可能なメニュー コントロールを設定できますか、独自のカスタムの外部データ ソースを提供することで。

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>内部データの使用

インターフェイス ビルダーでは、メニュー コントロールの項目を定義するだけでなく (など`NSComboBox`)、追加、編集、または保持することが内部の一覧から項目を削除するためのメソッドの完全なセットを提供します。

- `Add` -新しい項目をリストの末尾に追加します。
- `GetItem` -指定したインデックス位置にある項目を返します。
- `Insert` -指定した場所にある一覧で新しい項目を挿入します。
- `IndexOf` -指定された項目のインデックスを返します。
- `Remove` -指定したアイテムの一覧からを削除します。
- `RemoveAll` -すべての項目を一覧から削除します。
- `RemoveAt` -指定したインデックス位置にある項目を削除します。
- `Count` -一覧で項目の数を返します。

> [!IMPORTANT]
> Extern のデータ ソースを使用している場合 (`UsesDataSource = true`)、上記のメソッドを呼び出すと、例外がスローされます。

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>外部データ ソースの操作

メニュー コントロールの行を提供する組み込みの内部データを使用する代わりに必要に応じて外部データ ソースを使用し、項目 (SQLite データベースの場合) などの独自のバッキング ストアを提供できます。

外部データ ソースを操作するには、メニュー コントロールのデータ ソースのインスタンスを作成します (`NSComboBoxDataSource`など) し、必要なデータを提供するいくつかのメソッドをオーバーライドします。

- `ItemCount` -一覧で項目の数を返します。
- `ObjectValueForItem` -指定されたインデックスの項目の値を返します。
- `IndexOfItem` -特定の項目の値のインデックスを返します。
- `CompletedString` -部分的に型指定された項目の値の一致する最初の項目の値を返します。 オートコンプリートが有効になっている場合にのみ、このメソッドが呼び出す (`Completes = true`)。

参照してください、[データベースとコンボ ボックス](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes)のセクション、[データベースでの作業](~/mac/app-fundamentals/databases.md)詳細についてはドキュメントです。

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>リストの外観を調整します。

次のメソッドは、メニュー コントロールの外観を調整できます。

- `HasVerticalScroller` If`true`コントロールが垂直スクロール バーを表示します。 
- `VisibleItems` -コントロールを開いたときに表示される項目の数を調整します。 既定値は 5 (5)。
- `IntercellSpacing` 提供することで、特定の項目の周囲の空白の量を調整する、`NSSize`場所、`Width`左と右の余白を指定します、`Height`アイテムの前後にスペースを指定します。
- `ItemHeight` -一覧で、各項目の高さを指定します。

ドロップダウン リストの種類の`NSPopupButtons`、最初のメニュー項目コントロールのタイトルを提供します。 たとえば、次のように入力します。 

[![](standard-controls-images/menu02.png "メニュー コントロールの例")](standard-controls-images/menu02.png#lightbox)

タイトルを変更するには、としては、この項目を公開、**アウトレット**し、次のようなコードを使用します。

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>選択した項目を操作します。

次のメソッドとプロパティ メニュー コントロールのリストで選択したアイテムの操作を使用します。

- `SelectItem` -指定したインデックス位置にある項目を選択します。
- `Select` -指定した項目の値を選択します。
- `DeselectItem` -指定したインデックス位置にある項目を選択解除します。
- `SelectedIndex` -現在選択されている項目のインデックスを返します。
- `SelectedValue` -現在選択されている項目の値を返します。

使用、`ScrollItemAtIndexToTop`一覧の上部にある指定したインデックス位置にある項目を表示して、`ScrollItemAtIndexToVisible`を指定したインデックス位置にある項目が表示されるまで一覧をスクロールします。

<a name="Responding to Events" />

### <a name="responding-to-events"></a>イベントへの応答

メニュー コントロールは、次のユーザーとの対話に応答するイベントを提供します。

- `SelectionChanged` -は、ユーザーがリストから値を選択したときに呼び出されます。
- `SelectionIsChanging` は、新しいユーザーが選択した項目がアクティブな選択範囲が呼び出されます。
- `WillPopup` -は、項目のドロップダウン リストが表示される前に呼び出されます。
- `WillDismiss` -は、項目のドロップダウン リストを閉じる前に呼び出されます。

`NSComboBox`コントロールすべて含まれているのと同じイベント、`NSTextField`など、`Changed`ユーザー コンボ ボックス内のテキストの値を編集するたびに呼び出されるイベント。

必要に応じて、対応できる、内部データ メニュー項目が項目をアタッチすることで選択されているインターフェイス ビルダーで定義されている、**アクション**に応答する、次のようなコードを使用して**アクション**されています。ユーザーによってトリガーされます。

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

メニューとメニュー コントロールの操作方法の詳細についてを参照してください、[メニュー](~/mac/user-interface/menu.md)と[ポップアップのボタンとドロップダウン リストを一覧表示](~/mac/user-interface/menu.md)ドキュメント。

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>選択範囲のコントロールの操作

AppKit では、いくつかの種類のユーザー インターフェイスの設計で使用できる選択コントロールを提供します。 詳細についてを参照してください、[選択コントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。 

[![](standard-controls-images/select01.png "選択コントロールの例")](standard-controls-images/select01.png#lightbox)

2 つの方法の選択コントロールとして公開することで、ユーザーの操作がある場合に追跡する、**アクション**します。 例えば:

```csharp
partial void SegmentButtonPressed (Foundation.NSObject sender) {
    FeedbackLabel.StringValue = string.Format("Button {0} Pressed",SegmentButtons.SelectedSegment);
}
```

または、アタッチすることにより、**デリゲート**を`Activated`イベント。 例えば:

```csharp
TickedSlider.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
};
```

を設定または選択コントロールの値を読み取るには、使用、`IntValue`プロパティ。 例えば:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

色、およびイメージも) などの専門的なコントロールは、その値の型の特定のプロパティを持ちます。 たとえば、次のように入力します。

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

`NSDatePicker`に日付と時刻を直接操作するための次のプロパティがあります。

- **DateValue** -として現在の日付と時刻の値を`NSDate`します。
- **ローカル**-ユーザーの場所として、`NSLocal`します。
- **TimeInterval** -時刻の値として、`Double`します。
- **タイム ゾーン**-として、ユーザーのタイム ゾーン、`NSTimeZone`します。

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>インジケーターのコントロールの操作

AppKit では、いくつかの種類のユーザー インターフェイスの設計で使用できるインジケーター コントロールを提供します。 詳細についてを参照してください、[インジケーター コントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。 

[![](standard-controls-images/level01.png "インジケーターのコントロールの例")](standard-controls-images/level01.png#lightbox)

インジケーター コントロールにいずれかとして公開することで、ユーザーの操作を追跡するために 2 つの方法がある、**アクション**または**アウトレット**とアタッチを**デリゲート**に`Activated`イベント。 例えば:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

インジケーターのコントロールの値の設定を読み取ったり、使用、`DoubleValue`プロパティ。 例えば:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

表示されるときに、不定と非同期の進行状況インジケーターをアニメーション化する必要があります。 使用して、`StartAnimation`表示されるときにアニメーションを開始するメソッド。 例えば:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

呼び出す、`StopAnimation`メソッドは、アニメーションを停止します。

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>テキスト コントロールの操作

AppKit では、いくつかの種類のユーザー インターフェイスの設計で使用できるテキスト コントロールを提供します。 詳細についてを参照してください、[テキスト コントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。 

[![](standard-controls-images/text01.png "テキスト コントロールの例")](standard-controls-images/text01.png#lightbox)

テキスト フィールド (`NSTextField`)、次のイベントは、ユーザーの操作を追跡するために使用できます。

- **変更された**-いつでも、ユーザーがフィールドの値に変更が発生します。 たとえば、次のように入力すると、すべての文字にします。
- **EditingBegan** -は、ユーザーを編集するためのフィールドを選択するときに発生します。
- **EditingEnded** - フィールドで Enter キーを押すか、フィールドのままとします。

使用して、`StringValue`プロパティを読み取り、またはフィールドの値を設定します。 例えば:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

数値の値を編集または表示するフィールドでは、使用することができます、`IntValue`プロパティ。 例えば:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

`NSTextView`おすすめフルテキストは、編集および組み込みの書式設定を使用して領域を表示します。 ように、`NSTextField`を使用して、`StringValue`プロパティを読み取ったり、領域の値を設定します。

Xamarin.Mac アプリでのテキスト ビューの操作の複雑な例の例を参照してください、 [SourceWriter サンプル アプリ](https://developer.xamarin.com/samples/mac/SourceWriter/)します。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter コード全体に詳細なコメントが付いていて、可能な場合は、重要な技術やメソッド、Xamarin.Mac ガイド ドキュメントの関連情報へのリンクが示されます。

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>コンテンツ ビューを使用します。

AppKit では、いくつかの種類のユーザー インターフェイスの設計で使用できるコンテンツ ビューを提供します。 詳細についてを参照してください、[コンテンツ ビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1)の Apple の「 [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)します。

[![](standard-controls-images/content01.png "コンテンツ ビューの例")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popovers

ポップ オーバーは、特定のコントロールまたは画面上の領域に直接関連する機能を提供する一時的な UI 要素です。 コントロールまたは関連しています。 その領域を含むウィンドウをポップ オーバーから浮いてし、境界線が登場しました元となるポイントを指定するには矢印が含まれています。

ポップ オーバーを作成するには、次の操作を行います。

1. 開く、`.storyboard`にポップ オーバーをダブルクリックして追加するウィンドウのファイル、**ソリューション エクスプ ローラー**
2. ドラッグ、**コント ローラー表示**から、**ライブラリ インスペクター**上に、**インターフェイス エディター**: 

    [![](standard-controls-images/content02.png "ビュー コント ローラーをライブラリから選択します。")](standard-controls-images/content02.png#lightbox)
4. サイズとレイアウトの定義、**カスタム ビュー**: 

    [![](standard-controls-images/content04.png "レイアウトの編集")](standard-controls-images/content04.png#lightbox)
5. コントロールをクリックし、上にポップアップのソースからドラッグして、**ビュー コント ローラー**: 

    [![](standard-controls-images/content05.png "ドラッグしてセグエを作成するには")](standard-controls-images/content05.png#lightbox)
6. 選択**ポップ オーバー**ポップアップ メニューから。 

    [![](standard-controls-images/content06.png "セグエの種類の設定")](standard-controls-images/content06.png#lightbox)
7. 変更を保存し、Visual Studio for Mac は Xcode と同期に戻ります。

<a name="Tab_Views" />

### <a name="tab-views"></a>タブ ビュー

タブ ビューはタブのリスト (セグメント化されたコントロールと同様に) は、一連のビューと呼ばれると組み合わせる_ペイン_します。 ユーザーは、新しいタブを選択するときにそれに関連付けられているウィンドウが表示されます。 各ウィンドウには、独自のコントロールのセットが含まれています。

Xcode の Interface Builder でのタブ ビューを使用する場合は、使用、**属性インスペクター**タブの数を設定します。

[![](standard-controls-images/content08.png "タブの数の編集")](standard-controls-images/content08.png#lightbox)

各タブを選択して、**インターフェイス階層**を設定するその**タイトル**UI 要素を追加し、その**ウィンドウ**:

[![](standard-controls-images/content09.png "Xcode でタブの編集")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>データ バインディングの AppKit コントロール

キー値コーディングとデータ バインディングの手法を使用すると、Xamarin.Mac アプリケーションで、コードの記述し、保守を設定し、UI 要素を使用する必要があるの量を大幅に短縮できます。 さらに、バックアップ データを分離の利点がある場合も (_データ モデル_)、正面からのユーザー インターフェイスを終了します (_モデル-ビュー-コント ローラー_)、柔軟性の高いアプリケーションを管理しやすくします。デザインします。

キー値コーディング (KVC) は、オブジェクトのプロパティを直接アクセスする、インスタンス変数からアクセスするのではなくプロパティを識別するキー (特殊な形式の文字列) またはアクセサー メソッドを使用するためのメカニズム (`get/set`)。 キー値コーディング準拠アクセサーを実装すると、Xamarin.Mac アプリケーションで、キーと値を観察し (KVO)、データ バインディング、Core Data、Cocoa バインディング、およびあることを示すなどの他の macOS 機能へのアクセスを行えます。

詳細についてを参照してください、[の単純データ バインディング](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding)のセクション、[データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)ドキュメント。


<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでのボタン、ラベル、テキスト フィールド、チェック ボックスおよびセグメント付きコントロールなどの標準の AppKit コントロールの使用方法について詳しく説明をしました。 これは、Xcode の Interface Builder でのユーザー インターフェイスのデザインに追加すること、Outlet と Action を使用してコードに公開すること、および c# コードでの AppKit コントロールの操作について説明します。

## <a name="related-links"></a>関連リンク

- [MacControls (サンプル)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [コントロールとビューについて](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
