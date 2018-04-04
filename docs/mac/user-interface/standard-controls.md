---
title: 標準コントロール
description: ここでは、チェック ボックス、ボタン、ラベル、テキスト フィールドなどの標準の AppKit コントロールの操作について説明し、Xamarin.Mac アプリケーションでコントロールをセグメント化します。 これは、インターフェイスのビルダーを持つインターフェイスに追加して、コード内とやり取りするについて説明します。
ms.prod: xamarin
ms.assetid: d2593883-d255-431f-9781-75f04d8cecea
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3fe155508b60cbe502c3beca58426528d6f49c9d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="standard-controls"></a>標準コントロール

_ここでは、チェック ボックス、ボタン、ラベル、テキスト フィールドなどの標準の AppKit コントロールの操作について説明し、Xamarin.Mac アプリケーションでコントロールをセグメント化します。これは、インターフェイスのビルダーを持つインターフェイスに追加して、コード内とやり取りするについて説明します。_

同じアクセス権がある Xamarin.Mac アプリケーションでは、c# と .NET で作業するとき AppKit コントロールで作業する開発者*OBJECTIVE-C*と*Xcode*はします。 Xamarin.Mac は、Xcode と直接統合を使用すると Xcode の_インターフェイス ビルダー_を作成および Appkit コントロールを保持 (または必要に応じて c# コードで直接作成すること)。

AppKit コントロールは、Xamarin.Mac アプリケーションのユーザー インターフェイスの作成に使用する UI 要素を示します。 これらのボタン、ラベル、テキスト フィールド、チェック ボックスおよびセグメント化されたコントロールなどの要素で構成されているし、が発生するインスタント アクションまたは結果が表示、ユーザーがそれらを操作します。

[![](standard-controls-images/intro01.png "サンプル アプリのメイン画面")](standard-controls-images/intro01.png#lightbox)

この記事でしれませんにはアプリケーションで Xamarin.Mac AppKit コントロールの操作の基本について説明します。 作業することを強くお勧め、[こんにちは, Mac](~/mac/get-started/hello-mac.md)具体的には、最初の記事、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)と[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)セクションでは、これとは、主な概念と、この記事で使用する方法について説明します。

確認することも、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクション、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)が説明されても、ドキュメント、`Register`と`Export`コマンドObjective C のオブジェクトと UI 要素に、c# クラスをワイヤ アップするために使用します。

<a name="Introduction_to_Controls_and_Views" />

## <a name="introduction-to-controls-and-views"></a>コントロールとビューの概要

(旧称 Mac OS X) macOS AppKit フレームワークを使用してユーザー インターフェイス コントロールの標準セットを提供します。 これらのボタン、ラベル、テキスト フィールド、チェック ボックスおよびセグメント化されたコントロールなどの要素で構成されているし、が発生するインスタント アクションまたは結果が表示、ユーザーがそれらを操作します。

AppKit コントロールのすべてである、標準的な組み込みの外観で、ほとんどの用途に適した、使用するためのウィンドウ フレームの領域内で代替の外観を指定するいくつか、_自然な彩度効果_サイドバー領域と同様にやなどのコンテキスト、通知センター ウィジェット。

Apple は、次のガイドラインを示している AppKit コントロールを使用する場合。

- 同じビューでのコントロールのサイズを混合しないでください。
- 一般に、コントロールの垂直方向にサイズ変更しないでください。
- システムのフォントとコントロール内で適切なテキストのサイズを使用します。
- コントロールの適切な間隔を使用します。

詳細については、pleas を参照してください、[に関するコントロールとビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。

<a name="Using_Controls_in_a_Window_Frame" />

### <a name="using-controls-in-a-window-frame"></a>ウィンドウ フレームでコントロールの使用

ウィンドウのフレームの領域に含めるを許可する表示スタイルが含まれる AppKit コントロールのサブセットがあります。 例については、メール アプリのツールバーを参照してください。

[![](standard-controls-images/mailapp.png "Mac のウィンドウ フレーム")](standard-controls-images/mailapp.png#lightbox)

- **ボタンのテクスチャを丸める**- a`NSButton`のスタイルと`NSTexturedRoundedBezelStyle`です。
- **セグメント化されたコントロールの丸めをテクスチャ**- a`NSSegmentedControl`のスタイルと`NSSegmentStyleTexturedRounded`です。
- **セグメント化されたコントロールの丸めをテクスチャ**- a`NSSegmentedControl`のスタイルと`NSSegmentStyleSeparated`です。
- **ポップアップ メニューのテクスチャを丸める**- a`NSPopUpButton`のスタイルと`NSTexturedRoundedBezelStyle`です。
- **ドロップダウン メニューのテクスチャを丸める**- a`NSPopUpButton`のスタイルと`NSTexturedRoundedBezelStyle`です。
- **検索バー** - a`NSSearchField`です。

ウィンドウ フレームで AppKit コントロールを操作するときに、Apple は、次のガイドラインを勧めします。

- ウィンドウの本文には、ウィンドウ フレームの特定のコントロールのスタイルを使用しないでください。
- ウィンドウ フレームのウィンドウの本文のコントロールやスタイルを使用しないでください。

詳細については、pleas を参照してください、[に関するコントロールとビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1) Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。

<a name="Creating_a_User_Interface_in_Interface_Builder" />

## <a name="creating-a-user-interface-in-interface-builder"></a>インターフェイスのビルダーでのユーザー インターフェイスの作成

新しい Xamarin.Mac Cocoa アプリケーションを作成するときに、既定で標準の空白、ウィンドウを取得します。 この windows がで定義されている、`.storyboard`プロジェクトに自動的に含まれるファイル。 Windows のデザインを編集する、**ソリューション エクスプ ローラー**、ダブルクリックして、`Main.storyboard`ファイル。

[![](standard-controls-images/edit01.png "ソリューション エクスプ ローラーでメインのストーリー ボードを選択します。")](standard-controls-images/edit01.png#lightbox)

Xcode のインターフェイスのビルダーで、ウィンドウのデザインが開きます。

[![](standard-controls-images/edit02.png "Xcode でストーリー ボードの編集")](standard-controls-images/edit02.png#lightbox)

ユーザー インターフェイスを作成するから UI 要素 (AppKit コントロール) をドラッグします、**ライブラリ インスペクター**を**インターフェイス エディター**インターフェイスのビルダー。 次の例で、**垂直分割ビュー**コントロールなったから薬、**ライブラリ インスペクター**のウィンドウに配置し、**インターフェイス エディター**:

[![](standard-controls-images/edit03.png "ライブラリから分割ビューを選択します。")](standard-controls-images/edit03.png#lightbox)

インターフェイスのビルダーで、ユーザー インターフェイスを作成する方法の詳細についてを参照してください、 [Xcode とインターフェイスのビルダーの概要を](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)ドキュメント。

<a name="Sizing_and_Positioning" />

### <a name="sizing-and-positioning"></a>サイズおよび位置

コントロールは、ユーザー インターフェイスに含まれている、使用して、**制約エディター**値を手動で入力してその場所とサイズを設定し、および方法を制御、コントロールが自動的に配置されているときにサイズを親ウィンドウまたはビューサイズを変更します。

[![](standard-controls-images/edit04.png "制約を設定")](standard-controls-images/edit04.png#lightbox)

使用して、**赤い I ビーム**の外側を囲む、 **Autoresizing**ボックスを_スティック_(x, y) 特定の場所を制御します。 例えば: 

[![](standard-controls-images/edit05.png "制約の編集")](standard-controls-images/edit05.png#lightbox)

指定、選択したコントロール (で、**階層ビュー** & **インターフェイス エディター**) サイズ変更または移動したように、ウィンドウまたはビューの上および右の場所をスタックします。 

その他の要素の高さと幅などのエディター コントロールのプロパティ:

[![](standard-controls-images/edit06.png "高さの設定")](standard-controls-images/edit06.png#lightbox)

使用して制約を含む、要素の配置を制御することも、**配置エディター**:

[![](standard-controls-images/edit07.png "配置エディター")](standard-controls-images/edit07.png#lightbox)

> [!IMPORTANT]
> IOS とは異なり、(0, 0) は、上 macOS (0, 0) で、画面の左下隅は、左下隅です。 これは、macOS 数学座標系を使用して、右上に値が増加し番号値であるためです。 ユーザー インターフェイス上で AppKit コントロールを配置するときに、これを考慮に入れておく必要があります。

<a name="Setting_a_Custom_Class" />

### <a name="setting-a-custom-class"></a>カスタム クラスを設定

AppKit コントロールを使用したはサブクラスと既存のコントロールを必要し、そのクラスの独自のカスタム バージョンを作成します。 たとえば、ソース リストのカスタム バージョンを定義します。

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

ここで、`[Register("SourceListView")]`命令の公開、 `SourceListView` OBJECTIVE-C が置かれるようにするクラスはインターフェイスのビルダーで使用できます。 詳細についてを参照してください、 [c# を公開するクラス/OBJECTIVE-C メソッド](~/mac/internals/how-it-works.md)のセクションで、 [Xamarin.Mac 内部](~/mac/internals/how-it-works.md)を文書化について説明します、`Register`と`Export`コマンドの使用ワイヤをする c# クラスを OBJECTIVE-C オブジェクトと UI 要素。

場所で上記のコードでは、デザイン サーフェイスを拡張する基本の型の AppKit コントロールをドラッグできます (次の例で、**ソース リスト**) に切り替え、 **Identity インスペクター**設定と**カスタム クラス**Objective C に公開した名前 (例`SourceListView`)。

[![](standard-controls-images/edit10.png "Xcode でのカスタム クラスの設定")](standard-controls-images/edit10.png#lightbox)

<a name="Exposing_Outlets_and_Actions" />

### <a name="exposing-outlets-and-actions"></a>コンセントとアクションを公開します。

いずれかとして公開する必要がある AppKit コントロールは、c# コードでアクセスすることができます、前に、**コンセント**またはと**アクション**です。 いずれかで指定されたコントロールで選択これを行うには、**インターフェイス階層**または**インターフェイス エディター**に切り替えると、**アシスタント ビュー** (、があることを確認`.h`編集用に選択ウィンドウの)。

[![](standard-controls-images/edit11.png "編集する正しいファイルを選択します。")](standard-controls-images/edit11.png#lightbox)

コントロールのドラッグを付けてくださいに AppKit コントロールから`.h`作成を開始するファイル、**コンセント**または**アクション**:

[![](standard-controls-images/edit12.png "ドラッグしてコンセントまたはアクションを作成するには")](standard-controls-images/edit12.png#lightbox)

作成しへの露出の種類の選択、**コンセント**または**アクション**、**名前**: 

[![](standard-controls-images/edit13.png "コンセントまたはアクションを構成します。")](standard-controls-images/edit13.png#lightbox)


操作の詳細については**コンセント**と**アクション**を参照してください、[コンセントとアクション](~/mac/get-started/hello-mac.md#Outlets_and_Actions)のセクションで、 [Xcode とインターフェイスの概要ビルダー](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder)ドキュメント。

<a name="Synchronizing_Changes_with_Xcode" />

### <a name="synchronizing-changes-with-xcode"></a>Xcode との変更の同期

切り替えたときに Visual Studio に戻り for Mac Xcode から、Xcode に対して行った変更は、Xamarin.Mac プロジェクトで自動的に同期されます。

選択した場合、`SplitViewController.designer.cs`で、**ソリューション エクスプ ローラー**を表示することができる方法、**コンセント**と**アクション**c# コードで接続されています。

[![](standard-controls-images/sync01.png "Xcode での変更を同期します。")](standard-controls-images/sync01.png#lightbox)

通知方法の定義、`SplitViewController.designer.cs`ファイル。

```csharp
[Outlet]
AppKit.NSSplitViewItem LeftController { get; set; }

[Outlet]
AppKit.NSSplitViewItem RightController { get; set; }

[Outlet]
AppKit.NSSplitView SplitView { get; set; }
```

内の定義に合わせて、 `MainWindow.h` Xcode でファイル。

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

Visual Studio for Mac をリッスンへの変更が表示されるよう、`.h`ファイル、およびそれぞれにそれらの変更を自動的に同期`.designer.cs`ファイルをアプリケーションに公開することです。 場合もあります`SplitViewController.designer.cs`部分クラスでは、Visual Studio for Mac は、変更する必要があるないように`SplitViewController.cs `クラスに加え変更を上書きします。

通常必要はありませんを開くには、 `SplitViewController.designer.cs` 、自分でこれがここに示す教育する目的でのみです。

> [!IMPORTANT]
> ほとんどの場合、Visual Studio for Mac は自動的に Xcode で行った変更を表示し、Xamarin.Mac プロジェクトに同期させるだけです。 同期が自動的に行われないときは Xcode に戻り、再び Visual Studio for Mac に戻ります。 こうすると通常、同期サイクルが開始します。

<a name="Working_with_Buttons" />

## <a name="working-with-buttons"></a>ボタンの操作

AppKit は、ユーザー インターフェイスの設計で使用できるボタンのいくつかの型を提供します。 詳細についてを参照してください、[ボタン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1)Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。 

[![](standard-controls-images/buttons01.png "ボタンのさまざまな種類の例")](standard-controls-images/buttons01.png#lightbox)

ボタンを使用して公開されている場合、**コンセント**、次のコードは押されていることに応答します。

```csharp
ButtonOutlet.Activated += (sender, e) => {
        FeedbackLabel.StringValue = "Button Outlet Pressed";
};
```

経由で公開されているボタンの**アクション**、`public partial`メソッドは自動的に作成されますを Xcode で選択した名前を持つ。 応答する、**アクション**、クラスにある部分メソッドを完了する、**アクション**で定義されていた。 例えば:

```csharp
partial void ButtonAction (Foundation.NSObject sender) {
    // Do something in response to the Action
    FeedbackLabel.StringValue = "Button Action Pressed";
}
```

した状態にあるボタンの (と同様に**で**と**オフ**)、状態をチェックまたはで設定された、`State`プロパティに対して、`NSCellStateValue`列挙型。 例えば:

```csharp
DisclosureButton.Activated += (sender, e) => {
    LorumIpsum.Hidden = (DisclosureButton.State == NSCellStateValue.On);
};
```

ここで`NSCellStateValue`を指定できます。

- **オン** - ボタンをクリックするか (チェック ボックスにチェック マーク) など、コントロールが選択されています。
- **オフ**-、ボタンが押されていないか、コントロールが選択されていません。
- **混合**-混在**で**と**オフ**状態です。

<a name="Mark-a-Button-as-Default-and-Set-Key-Equivalent" />

### <a name="mark-a-button-as-default-and-set-key-equivalent"></a>マーク ボタンをクリックすると既定の該当するショートカット キーを設定

どのボタンに対してもユーザー インターフェイスのデザインに追加したが、そのボタンとしてのマークを付ける、_既定_ ボタンを押したときにアクティブになる、**戻り/Enter**キーボードのキー。 MacOS などで、このボタンには既定では青色の背景色が表示されます。

既定としてボタンを設定するには、Xcode のインターフェイスのビルダーで選択します。 次に、**属性インスペクター**を選択、**キーと同じ**フィールドとキーを押して、**戻り/Enter**キー。

[![](standard-controls-images/buttons03.png "該当するショートカットは、キーの編集")](standard-controls-images/buttons03.png#lightbox)

同様に、マウスの代わりに、キーボードを使用して、ボタンをアクティブ化に使用できる任意のキー シーケンスを割り当てることができます。 たとえばでキーを押すことコマンド C 上の図でします。

アプリを実行し、ウィンドウのボタンでは、キーとコマンドを押しながら C キーを押した場合は、中心、ボタンのアクションをアクティブにする (として- ボタンをクリックした場合)。

<a name="Working_with_Checkboxes_and_Radio_Buttons" />

## <a name="working-with-checkboxes-and-radio-buttons"></a>チェック ボックスとオプション ボタンを使用します。

AppKit は、いくつかのチェック ボックスと、ユーザー インターフェイスの設計で使用できるオプション ボタン グループの種類を提供します。 詳細についてを参照してください、[ボタン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsButtons.html#//apple_ref/doc/uid/20000957-CH48-SW1)Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。 

[![](standard-controls-images/buttons02.png "使用可能なチェック ボックスの種類の例")](standard-controls-images/buttons02.png#lightbox)


チェック ボックスやオプション ボタン (を介して公開される**コンセント**) した状態 (と同様に**で**と**オフ**)、状態をチェックまたはで設定された、`State`プロパティに対して、`NSCellStateValue`列挙型。 例えば:

```csharp
AdjustTime.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Adjust Time: {0}",AdjustTime.State == NSCellStateValue.On);
};
```

ここで`NSCellStateValue`を指定できます。

- **オン** - ボタンをクリックするか (チェック ボックスにチェック マーク) など、コントロールが選択されています。
- **オフ**-、ボタンが押されていないか、コントロールが選択されていません。
- **混合**-混在**で**と**オフ**状態です。

ボタンを選択するには、ラジオ ボタン グループで、公開として選択するラジオ ボタン、**コンセント**設定とその`State`プロパティです。 たとえば、次のように入力します。

```csharp
partial void SelectCar (Foundation.NSObject sender) {
    TransportationCar.State = NSCellStateValue.On;
    FeedbackLabel.StringValue = "Car Selected";
}
```

ラジオ ボタンをグループとして動作させ、選択された状態を自動的に処理のコレクションを取得するには、新規に作成**アクション**してグループ内のすべてのボタンをアタッチします。

![](standard-controls-images/buttons04.png "新しいアクションの作成")

次に、一意な割り当てる`Tag`ラジオ ボタンごとに、**属性インスペクター**:

![](standard-controls-images/buttons05.png "ラジオ ボタンのタグの編集")

変更内容を保存し、Mac を Visual Studio に戻りを処理するコードを追加、**アクション**に関連付けられているすべてのラジオ ボタン。

```csharp
partial void NumberChanged(Foundation.NSObject sender)
{
    var check = sender as NSButton;
    Console.WriteLine("Changed to {0}", check.Tag);
}
```

使用することができます、`Tag`プロパティを表示するラジオ ボタンが選択されました。

<a name="Working_with_Menu_Controls" />

## <a name="working-with-menu-controls"></a>メニュー コントロールの操作

AppKit は、ユーザー インターフェイスの設計で使用できるメニュー コントロールのいくつかの型を提供します。 詳細についてを参照してください、[メニュー コントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlswithMenus.html#//apple_ref/doc/uid/20000957-CH100-SW1)Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。 

[![](standard-controls-images/menu01.png "メニュー コントロールの例")](standard-controls-images/menu01.png#lightbox)

<a name="Providing-Menu-Control-Data" />

### <a name="providing-menu-control-data"></a>メニュー コントロールのデータを提供します。

(インターフェイスのビルダーで事前に定義したりできるコードを使用して設定されます)、内部の一覧からいずれか、ドロップダウン リストを設定する macOS に利用可能なメニュー コントロールを設定することができますか、独自のカスタムの外部データ ソースを提供することによりします。

<a name="Working-with-Internal-Data" />

#### <a name="working-with-internal-data"></a>内部データの操作

インターフェイスのビルダー、メニュー コントロールで項目を定義するだけでなく (など`NSComboBox`) を追加、編集、または維持することも内部の一覧から項目を削除できるようにするメソッドの完全なセットを提供します。

- `Add` -新しい項目をリストの末尾に追加します。
- `GetItem` -指定したインデックス位置にある項目を返します。
- `Insert` -指定した位置にある一覧で新しい項目を挿入します。
- `IndexOf` -指定した項目のインデックスを返します。
- `Remove` -指定したアイテムを一覧から削除します。
- `RemoveAll` -すべての項目を一覧から削除します。
- `RemoveAt` -指定したインデックス位置にある項目を削除します。
- `Count` -リスト内の項目数を返します。

> [!IMPORTANT]
> 外部データ ソースを使用している場合 (`UsesDataSource = true`)、上記のメソッドを呼び出すと、例外がスローされます。

<a name="Working-with-an-External-Data-Source" />

#### <a name="working-with-an-external-data-source"></a>外部データ ソースの操作

メニュー コントロールに行を提供する組み込みの内部データではなく、必要に応じて外部データ ソースを使用し、項目 (SQLite データベースなど) の独自のバッキング ストアを提供できます。

外部データ ソースを操作するには、メニュー コントロールのデータ ソースのインスタンスを作成します (`NSComboBoxDataSource`など) に必要なデータを提供するいくつかのメソッドをオーバーライドします。

- `ItemCount` -リスト内の項目数を返します。
- `ObjectValueForItem` -指定されたインデックスの項目の値を返します。
- `IndexOfItem` -付与項目の値のインデックスを返します。
- `CompletedString` -部分的に型指定された項目の値の一致する最初の項目の値を返します。 オートコンプリートを有効になっている場合にのみ、このメソッドが呼び出す (`Completes = true`)。

参照してください、[データベースとコンボ ボックス](~/mac/app-fundamentals/databases.md#Databases-and-ComboBoxes)のセクションで、[データベースの操作](~/mac/app-fundamentals/databases.md)詳細についてはドキュメントです。

<a name="Adjusting-the-Lists-Appearance" />

### <a name="adjusting-the-lists-appearance"></a>リストの外観を調整します。

メニュー コントロールの外観を調整する、次の方法があります。

- `HasVerticalScroller` If`true`コントロールが垂直スクロール バーを表示します。 
- `VisibleItems` -コントロールを開いたときに表示される項目の数を調整します。 既定値は 5 (5)。
- `IntercellSpacing` -提供することによって、特定のアイテムの周囲のスペースの量を調整する、`NSSize`場所、`Width`左と右の余白を指定し、`Height`項目の前後にスペースを指定します。
- `ItemHeight` -リスト内の各項目の高さを指定します。

ドロップダウンの種類の`NSPopupButtons`、最初のメニュー項目がコントロールのタイトルを提供します。 たとえば、次のように入力します。 

[![](standard-controls-images/menu02.png "例メニュー コントロール")](standard-controls-images/menu02.png#lightbox)

タイトルを変更するには、としては、この項目を公開、**コンセント**し、次のようなコードを使用します。

```csharp
DropDownSelected.Title = "Item 1";
```

<a name="Manipulating-the-Selected-Items" />

### <a name="manipulating-the-selected-items"></a>選択した項目を操作します。

次のメソッドとプロパティは、メニュー コントロールのリストで選択された項目を操作することを許可します。

- `SelectItem` -指定したインデックス位置にある項目を選択します。
- `Select` -指定した項目の値を選択します。
- `DeselectItem` -指定したインデックス位置にある項目を選択解除します。
- `SelectedIndex` -現在選択されている項目のインデックスを返します。
- `SelectedValue` -現在選択されている項目の値を返します。

使用して、`ScrollItemAtIndexToTop`一覧の上部にある指定されたインデックス位置の項目を表示して、`ScrollItemAtIndexToVisible`に指定したインデックス位置にある項目が表示されるまで一覧をスクロールします。

<a name="Responding to Events" />

### <a name="responding-to-events"></a>イベントへの応答

メニュー コントロールは、ユーザーとの対話に応答する、次のイベントを提供します。

- `SelectionChanged` -は、ユーザーが、一覧から値を選択したときに呼び出されます。
- `SelectionIsChanging` -新しいユーザーが選択した項目がアクティブな選択前に、は呼び出されます。
- `WillPopup` -項目のドロップダウン リストが表示される前に、は呼び出されます。
- `WillDismiss` -項目のドロップダウン リストを閉じる前に、は呼び出されます。

`NSComboBox` 、コントロールがすべて含まれているとして同一のイベント、 `NSTextField`、ように、`Changed`ユーザーがコンボ ボックス内のテキストの値を編集するたびに呼び出されるイベント。

必要に応じて、対応することができます、内部データ メニュー項目が、項目をアタッチすることによって選択されているインターフェイスのビルダーで定義されている、**アクション**応答に、次のようにコードを使用して**アクション**中ユーザーによってトリガーされます。

```csharp
partial void ItemOne (Foundation.NSObject sender) {
    DropDownSelected.Title = "Item 1";
    FeedbackLabel.StringValue = "Item One Selected";
}
```

メニューやメニュー コントロールを操作の詳細についてを参照してください、[メニュー](~/mac/user-interface/menu.md)と[ポップアップ ボタンをクリックし、プルダウンで表示される一覧](~/mac/user-interface/menu.md)ドキュメント。

<a name="Working_with_Selection_Controls" />

## <a name="working-with-selection-controls"></a>選択コントロールの操作

AppKit は、ユーザー インターフェイスの設計で使用できる選択コントロールのいくつかの型を提供します。 詳細についてを参照してください、[選択コントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsSelection.html#//apple_ref/doc/uid/20000957-CH49-SW1)Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。 

[![](standard-controls-images/select01.png "選択コントロールの例")](standard-controls-images/select01.png#lightbox)

2 つの方法の選択コントロールとして公開することにより、ユーザーとの対話にあるときに追跡するために、**アクション**です。 例えば:

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

設定または選択コントロールの値を読み、使用、`IntValue`プロパティです。 例えば:

```csharp
FeedbackLabel.StringValue = string.Format("Stepper Value: {0:###}",TickedSlider.IntValue);
```

(も色および画像も) などの特別なコントロールでは、それらの値の種類に固有のプロパティがあります。 たとえば、次のように入力します。

```csharp
CollorWell.Color = NSColor.Red;
ImageWell.Image = NSImage.ImageNamed ("tag.png");

```

`NSDatePicker`は日付と時刻を直接操作するための次のプロパティがあります。

- **DateValue** -として現在の日付と時刻の値、`NSDate`です。
- **ローカル**-ユーザーの場所として、`NSLocal`です。
- **TimeInterval** -時間の値として、`Double`です。
- **タイム ゾーン**-とユーザーのタイム ゾーン、`NSTimeZone`です。

<a name="Working_with_Indicator_Controls" />

## <a name="working-with-indicator-controls"></a>インジケーターのコントロールの操作

AppKit は、ユーザー インターフェイスの設計で使用できるインジケーター コントロールのいくつかの型を提供します。 詳細についてを参照してください、[インジケーター コントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsIndicators.html#//apple_ref/doc/uid/20000957-CH50-SW1)Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。 

[![](standard-controls-images/level01.png "インジケーターのコントロールの例")](standard-controls-images/level01.png#lightbox)

インジケーターのコントロールがあるユーザーの操作として公開することでいずれかを追跡するために 2 つの方法、**アクション**または**コンセント**アタッチして、**デリゲート**に`Activated`イベント。 例えば:

```csharp
LevelIndicator.Activated += (sender, e) => {
    FeedbackLabel.StringValue = string.Format("Level: {0:###}",LevelIndicator.DoubleValue);
};
```

読み取るか、インジケーターのコントロールの値を設定、使用、`DoubleValue`プロパティです。 例えば:

```csharp
FeedbackLabel.StringValue = string.Format("Rating: {0:###}",Rating.DoubleValue);
```

表示されるときに、不定と非同期の進行状況インジケーターをアニメーション化する必要があります。 使用して、`StartAnimation`表示されるときに、アニメーションを開始するメソッド。 例えば:

```csharp
Indeterminate.StartAnimation (this);
AsyncProgress.StartAnimation (this);
```

呼び出す、`StopAnimation`メソッドは、アニメーションを停止します。

<a name="Working_with_Text_Controls" />

## <a name="working-with-text-controls"></a>テキスト コントロールの操作

AppKit は、ユーザー インターフェイスの設計のために使用するテキスト コントロールのいくつかの型を提供します。 詳細についてを参照してください、[テキスト コントロール](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsText.html#//apple_ref/doc/uid/20000957-CH51-SW1)Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。 

[![](standard-controls-images/text01.png "テキスト コントロールの例")](standard-controls-images/text01.png#lightbox)

テキスト フィールド (`NSTextField`)、次のイベントを使用してユーザーの操作を追跡できます。

- **変更**-ユーザーがフィールドの値を変更、いつでも発生します。 たとえば、すべての文字で次のように入力します。
- **EditingBegan** -ユーザーがフィールドの編集を選択したときに発生します。
- **EditingEnded** - ユーザーがフィールドに、Enter キーを押すか、フィールドのままにする場合。

使用して、`StringValue`プロパティを取得したり、フィールドの値を設定します。 例えば:

```csharp
FeedbackLabel.StringValue = string.Format("User ID: {0}",UserField.StringValue);
```

使用することができますを表示または編集数値フィールドで、`IntValue`プロパティです。 例えば:

```csharp
FeedbackLabel.StringValue = string.Format("Number: {0}",NumberField.IntValue);
```

`NSTextView`おすすめフルテキストは、編集し、組み込みの書式を設定して領域を表示します。 同様に、`NSTextField`を使用して、`StringValue`プロパティを読み取るまたは領域の値を設定します。

テキスト ビューを使った Xamarin.Mac アプリでの動作の複雑な例の例を参照してください、 [SourceWriter サンプル アプリ](https://developer.xamarin.com/samples/mac/SourceWriter/)です。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter コード全体に詳細なコメントが付いていて、可能な場合は、重要な技術やメソッド、Xamarin.Mac ガイド ドキュメントの関連情報へのリンクが示されます。

<a name="Working_with_Content_Views" />

## <a name="working-with-content-views"></a>コンテンツ ビューを使用します。

AppKit は、いくつかの種類のユーザー インターフェイスの設計で使用できるコンテンツのビューを提供します。 詳細についてを参照してください、[コンテンツ ビュー](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Apple のセクション[OS X のヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)です。

[![](standard-controls-images/content01.png "例のコンテンツ ビュー")](standard-controls-images/content01.png#lightbox)

<a name="Popovers" />

### <a name="popovers"></a>Popover

重なっては、特定のコントロールまたは画面上の領域に直接関連する機能を提供する一時的な UI 要素です。 コントロールまたはそれに関連する、領域を含むウィンドウの上、重なってフローティング状態になったし、の境界線に登場しました元となるポイントを指定するには矢印が含まれています。

重なってを作成するには、次の操作を行います。

1. 開く、`.storyboard`でダブルクリックしてに重なってを追加するウィンドウのファイル、**ソリューション エクスプ ローラー**
2. ドラッグ、**コント ローラーを表示**から、**ライブラリ インスペクター**上に、**インターフェイス エディター**: 

    [![](standard-controls-images/content02.png "ライブラリからビュー コント ローラーを選択します。")](standard-controls-images/content02.png#lightbox)
4. サイズとレイアウトの定義、**カスタム ビュー**: 

    [![](standard-controls-images/content04.png "レイアウトを編集")](standard-controls-images/content04.png#lightbox)
5. コントロールをクリックし、上にあるポップアップのソースからドラッグして、**ビュー コント ローラー**: 

    [![](standard-controls-images/content05.png "ドラッグして、segue を作成するには")](standard-controls-images/content05.png#lightbox)
6. 選択**重なって**ポップアップ メニューから。 

    [![](standard-controls-images/content06.png "Segue の種類を設定")](standard-controls-images/content06.png#lightbox)
7. 変更内容を保存し、Xcode と同期する Mac 用の Visual Studio に戻ります。

<a name="Tab_Views" />

### <a name="tab-views"></a>タブのビュー

タブのビュー (セグメント化されたコントロールのような表示されている) が、一連のビューと呼ばれると組み合わせるタブ リストで構成_ペイン_です。 ユーザーが新しいタブを選択するとそれに関連付けられているウィンドウが表示されます。 各ウィンドウには、独自のコントロールのセットが含まれています。

Xcode のインターフェイスのビルダーでのタブのビューを使用するときに使用して、**属性インスペクター**タブの数を設定します。

[![](standard-controls-images/content08.png "タブの数の編集")](standard-controls-images/content08.png#lightbox)

各タブを選択して、**インターフェイス階層**を設定する、**タイトル**UI 要素を追加し、その**ウィンドウ**:

[![](standard-controls-images/content09.png "Xcode でのタブを編集")](standard-controls-images/content09.png#lightbox)

<a name="Data_Binding_AppKit_Controls" />

## <a name="data-binding-appkit-controls"></a>データ バインディング AppKit コントロール

Xamarin.Mac アプリケーションのコーディングのキー値と、データ バインドの手法を使用するの記述し、保守を作成し、UI 要素を使用する必要のあるコードの量を大幅に削減できます。 また、バックアップ データをさらに分離することの利点がある場合 (_データ モデル_)、正面からのユーザー インターフェイスを終了 (_モデル-ビュー-コント ローラー_) より柔軟なアプリケーションの保守が容易に先行します。デザインします。

キーと値のコーディング (KVC) は、インスタンス変数からアクセスするのではなくプロパティを識別するキー (特殊な形式の文字列) またはアクセサー メソッドを使用して、オブジェクトのプロパティを直接アクセスするための機構 (`get/set`)。 Xamarin.Mac アプリの準拠のアクセサーをキーと値のコーディングを実装すると、キーと値を観察し (KVO)、データ バインディング、基本データ、Cocoa バインディング、および scriptability などその他の macOS 機能へのアクセスを取得します。

詳細についてを参照してください、[の単純データ バインディング](~/mac/app-fundamentals/databinding.md#Simple_Data_Binding)のセクションで、[データ バインディングと、キーと値のコーディング](~/mac/app-fundamentals/databinding.md)ドキュメント。


<a name="Summary" />

## <a name="summary"></a>まとめ

この記事では、Xamarin.Mac アプリケーションでのボタン、ラベル、テキスト フィールド、チェック ボックスをセグメント化されたコントロールなど、標準的な AppKit コントロールの操作についての詳細を取得しました。 これは、Xcode のインターフェイスのビルダーでのユーザー インターフェイスのデザインに追加すること、コンセントとアクションを使用してコードには公開および c# コードで AppKit コントロールの操作について説明します。

## <a name="related-links"></a>関連リンク

- [MacControls (サンプル)](https://developer.xamarin.com/samples/mac/MacControls/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows](~/mac/user-interface/window.md)
- [データ バインディングとキー値コーディング](~/mac/app-fundamentals/databinding.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [コントロールおよびビューについて](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsAll.html#//apple_ref/doc/uid/20000957-CH46-SW1)
