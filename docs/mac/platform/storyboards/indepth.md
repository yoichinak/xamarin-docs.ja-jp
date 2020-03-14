---
title: Xamarin. Mac でストーリーボードを操作する
description: このドキュメントでは、Xamarin. Mac でストーリーボードを使用して、コード、ビューコントローラーのライフサイクル、応答側チェーン、セグエ、ウィンドウコントローラー、ジェスチャレコグナイザーなどからそれらを読み込む方法を確認する方法について説明します。
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 06774d13cf053b661dd0b2d24b7df0c0b767b4db
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306207"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Xamarin. Mac でストーリーボードを操作する

ストーリーボードは、特定のアプリのすべての UI を定義し、そのビューコントローラーの機能の概要に分類します。 Xcode の Interface Builder では、各コントローラーが独自のシーンに存在します。

[Xcode の Interface Builder でストーリーボードを ![する](indepth-images/intro01.png)](indepth-images/intro01.png#lightbox)

ストーリーボードは、コンパイル時および配布時に Xamarin. Mac アプリのバンドルに含まれるリソースファイル (`.storyboard`の拡張機能) です。 アプリの開始ストーリーボードを定義するには、`Info.plist` ファイルを編集し、ドロップダウンボックスから**メインインターフェイス**を選択します。 

[情報エディターの ![](indepth-images/sb01.png)](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>読み込み (コードから)

コードから特定のストーリーボードを読み込み、ビューコントローラーを手動で作成することが必要になる場合があります。 このアクションを実行するには、次のコードを使用します。

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

`FromName` は、アプリのバンドルに含まれている指定された名前のストーリーボードファイルを読み込みます。 `InstantiateControllerWithIdentifier` は、指定された Id を持つビューコントローラーのインスタンスを作成します。 UI をデザインするときに、Xcode の Interface Builder で Id を設定します。

[ストーリーボード ID の設定 ![](indepth-images/sb02.png)](indepth-images/sb02.png#lightbox)

必要に応じて、`InstantiateInitialController` メソッドを使用して、Interface Builder で初期コントローラーが割り当てられているビューコントローラーを読み込むことができます。

[初期コントローラーの設定 ![](indepth-images/sb03.png)](indepth-images/sb03.png#lightbox)

これは、**ストーリーボードのエントリポイント**と、上の [終了] 矢印によってマークされます。

<a name="View-Controllers" />

## <a name="view-controllers"></a>コントローラーの表示

ビューコントローラーは、Mac アプリ内の情報の特定のビューと、その情報を提供するデータモデルとの間の関係を定義します。 ストーリーボードの最上位レベルの各シーンは、Xamarin. Mac アプリのコード内の1つのビューコントローラーを表します。

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>ビューコントローラーのライフサイクル

MacOS でストーリーボードをサポートするために、いくつかの新しいメソッドが `NSViewController` クラスに追加されました。 最も重要なのは、指定されたビューコントローラーによって制御されるビューのライフサイクルに応答するために、次のメソッドを使用することです。

- `ViewDidLoad`-このメソッドは、ストーリーボードファイルからビューが読み込まれるときに呼び出されます。
- `ViewWillAppear`-このメソッドは、画面にビューが表示される直前に呼び出されます。
- `ViewDidAppear`-このメソッドは、ビューが画面に表示された直後に呼び出されます。
- `ViewWillDisappear`-このメソッドは、画面からビューが削除される直前に呼び出されます。
- `ViewDidDisappear`-このメソッドは、画面からビューが削除された直後に呼び出されます。
- `UpdateViewConstraints`-このメソッドは、ビューの自動レイアウト位置とサイズを定義する制約を更新する必要があるときに呼び出されます。
- `ViewWillLayout`-このビューのサブビューが画面上にレイアウトされる直前にこのメソッドが呼び出されます。
- `ViewDidLayout`-このメソッドは、ビューのサブビューが画面上にレイアウトされた直後に呼び出されます。

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>応答側チェーン

さらに、`NSViewControllers` がウィンドウの_応答側チェーン_の一部になりました。

[応答側チェーンの ![](indepth-images/vc01.png)](indepth-images/vc01.png#lightbox)

また、切り取り、コピー、貼り付けの各メニュー項目の選択などのイベントを受信して応答するように設定されています。 この自動ビューコントローラーのワイヤアップは、macOS Sierra (10.12) 以上で実行されているアプリでのみ発生します。

<a name="Containment" />

### <a name="containment"></a>含有

ストーリーボードでは、ビューコントローラー (分割ビューコントローラーやタブビューコントローラーなど) が_コンテインメント_を実装できるようになりました。これにより、他のサブビューコントローラーを "含める" ことができます。

[ビューコントローラーのコンテインメントの例 ![](indepth-images/vc02.png)](indepth-images/vc02.png#lightbox)

子ビューコントローラーには、親ビューコントローラーに関連付けたり、画面からビューの表示や削除を行ったりするためのメソッドとプロパティが含まれています。

MacOS に組み込まれているすべてのコンテナービューコントローラーには、独自のカスタムコンテナービューコントローラーを作成する場合に従うことをお勧めする特定のレイアウトがあります。

[ビューコントローラーのレイアウトの ![](indepth-images/vc03.png)](indepth-images/vc03.png#lightbox)

コレクションビューコントローラーには、コレクションビューアイテムの配列が含まれており、それぞれに独自のビューを含む1つまたは複数のビューコントローラーが含まれています。

<a name="Segues" />

## <a name="segues"></a>セグエ

セグエは、アプリの UI を定義するすべてのシーン間の関係を提供します。 IOS のストーリーボードでの作業に慣れている場合は、通常、セグエ for iOS が全画面表示の切り替え効果を定義していることがわかります。 これは macOS とは異なり、セグエでは通常、1つのシーンが親シーンの子である "[含有](#Containment)" が定義されます。

MacOS では、ほとんどのアプリは、分割ビューやタブなどの UI 要素を使用して、同じウィンドウ内にビューをグループ化する傾向があります。 IOS とは異なり、物理的な表示領域が限られているために、ビューをオンまたはオフに切り替える必要がある場合。

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>プレゼンテーションセグエ

MacOS の傾向に対しては、モーダルウィンドウ、シートビュー、Popovers などの_プレゼンテーションの_が使用される場合があります。 macOS には、次の組み込みのセグエ型が用意されています。

- **Show** -セグエのターゲットを非モーダルウィンドウとして表示します。 たとえば、この種類のセグエを使用して、アプリにドキュメントウィンドウの別のインスタンスを表示します。
- **モーダル**-セグエのターゲットをモーダルウィンドウとして表示します。 たとえば、この種類のセグエを使用して、アプリの [基本設定] ウィンドウを表示します。
- **シート**-セグエのターゲットを、親ウィンドウにアタッチされたシートとして表示します。 たとえば、この種類のセグエを使用して、[検索と置換] シートを表示します。
- **Segue** -セグエのターゲットを segue ウィンドウのように表示します。 たとえば、ユーザーが UI 要素をクリックしたときにオプションを表示するには、このセグエ type を使用します。
- **Custom** -開発者によって定義されたカスタムセグエ型を使用して、セグエのターゲットを表示します。 詳細については、後述の「[カスタムセグエの作成](#Creating-Custom-Segues)」を参照してください。

Presentation セグエを使用する場合は、表示する親ビューコントローラーの `PrepareForSegue` メソッドをオーバーライドして、変数を初期化し、表示されるビューコントローラーにデータを提供することができます。

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>トリガーされたセグエ

トリガーされたセグエを使用すると、名前付きのセグエを (Interface Builder の**識別子**プロパティを使用して) 指定し、ユーザーがボタンをクリックしたり、コードで `PerformSegue` メソッドを呼び出したりするなどのイベントによってトリガーされるようにすることができます。

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

セグエ ID は、アプリの UI をレイアウトするときに、Xcode の Interface Builder 内で定義されます。

[セグエ名の入力 ![](indepth-images/sg02.png)](indepth-images/sg02.png#lightbox)

セグエのソースとして動作しているビューコントローラーでは、`PrepareForSegue` メソッドをオーバーライドし、セグエを実行して指定されたビューコントローラーが表示される前に、必要な初期化を行う必要があります。

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

必要に応じて、`ShouldPerformSegue` メソッドをオーバーライドし、セグエが実際にコードによっC#て実行されるかどうかを制御できます。 手動で表示されたビューコントローラーの場合は、`DismissController` メソッドを呼び出して、不要になったときに表示から削除します。

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>カスタムセグエの作成

MacOS で定義されているビルドインセグエによって提供されていないセグエの種類がアプリに必要な場合があります。 この場合は、アプリの UI をレイアウトするときに、Xcode の Interface Builder で割り当てることができるカスタムのセグエを作成できます。

たとえば、ウィンドウ内の現在のビューコントローラーを置き換える新しいセグエ型を作成するには (ターゲットシーンを新しいウィンドウで開くのではなく)、次のコードを使用します。

```csharp
using System;
using AppKit;
using Foundation;

namespace OnCardMac
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

            // Swap the controllers
            source.View.Window.ContentViewController = destination;

            // Release memory
            source.RemoveFromParentViewController ();
        }
        #endregion

    }
        
}
```

ここで注意すべき点がいくつかあります。

- このクラスを目的の C/macOS に公開するために、`Register` 属性を使用しています。
- `Perform` メソッドをオーバーライドして、実際にカスタムセグエのアクションを実行しようとしています。
- ウィンドウの `ContentViewController` コントローラーを、セグエのターゲット (送信先) で定義されているコントローラーに置き換えます。
- `RemoveFromParentViewController` メソッドを使用して、元のビューコントローラーを削除してメモリを解放します。

この新しいセグエ type を Xcode の Interface Builder で使用するには、まずアプリをコンパイルし、次に Xcode に切り替えて、2つのシーン間に新しいセグエを追加する必要があります。 **スタイル**を**custom**に設定し、**セグエクラス**を `ReplaceViewSegue` (カスタムセグエクラスの名前) に設定します。

[セグエクラスを設定する ![](indepth-images/sg01.png)](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>ウィンドウコントローラー

ウィンドウコントローラーは、macOS アプリで作成できるさまざまなウィンドウの種類を格納および制御します。 ストーリーボードには、次の機能があります。

1. コンテンツビューコントローラーを提供する必要があります。 これは、子ウィンドウと同じコンテンツビューコントローラーになります。
2. `Storyboard` プロパティには、ウィンドウコントローラーが読み込まれたストーリーボードが含まれます。それ以外の場合は、ストーリーボードから読み込まれていない場合は `null` ます。
3. `DismissController` メソッドを呼び出して、指定されたウィンドウを閉じ、ビューから削除することができます。

ビューコントローラーと同様に、ウィンドウコントローラーは `PerformSegue`、`PrepareForSegue`、および `ShouldPerformSegue` メソッドを実装し、セグエ操作のソースとして使用できます。

Window Controller は、macOS アプリの次の機能を担当します。

- 特定のウィンドウを管理します。
- これらは、ウィンドウのタイトルバーとツールバー (使用可能な場合) を管理します。
- これらは、コンテンツビューコントローラーを管理して、ウィンドウの内容を表示します。

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>ジェスチャレコグナイザー

MacOS のジェスチャレコグナイザーは、iOS の対応するものとほぼ同じであり、開発者はアプリの UI の要素にジェスチャ (マウスボタンのクリックなど) を簡単に追加できます。

ただし、iOS のジェスチャはアプリの設計によって決定されます (2 本の指で画面をタップするなど)。 macOS のほとんどのジェスチャはハードウェアによって決定されます。

ジェスチャレコグナイザーを使用すると、UI の項目にカスタム対話を追加するために必要なコードの量を大幅に減らすことができます。 ダブルクリックとシングルクリックの間で自動的に決定されるため、イベントをクリックしてドラッグします。

ビューコントローラーの `MouseDown` イベントをオーバーライドする代わりに、ストーリーボードを操作するときにユーザー入力イベントを処理するジェスチャ認識エンジンを使用する必要があります。

MacOS では、次のジェスチャレコグナイザーを利用できます。

- `NSClickGestureRecognizer`-マウスの下と上にイベントを登録します。
- `NSPanGestureRecognizer`-マウスボタンを押したまま、ドラッグアンドリリースイベント。
- `NSPressGestureRecognizer`-特定の時間イベントに対してマウスボタンを押したままにした登録。
- `NSMagnificationGestureRecognizer`-トラックパッドハードウェアから拡大イベントを登録します。
- `NSRotationGestureRecognizer`-トラックパッドハードウェアからローテーションイベントを登録します。

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>ストーリーボード参照の使用

ストーリーボード参照を使用すると、大規模で複雑なストーリーボード設計を作成し、元から参照される小さなストーリーボードに分割することができます。これにより、複雑さが解消され、結果として得られる個々のストーリーボードの設計と保守が容易になります。

さらに、ストーリーボード参照は、同じストーリーボード内の別のシーンまたは別のシーンの特定のシーンに_アンカー_を提供できます。

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>外部ストーリーボードの参照

外部ストーリーボードへの参照を追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、プロジェクト名を右クリックし、 **[追加]** [ > **新しいファイル**] [ > **Mac** > **ストーリーボード**] の順に選択します。 新しいストーリーボードの**名前**を入力し、 **[新規]** ボタンをクリックします。 

    [新しいストーリーボードを追加 ![](indepth-images/ref01.png)](indepth-images/ref01.png#lightbox)
2. **ソリューションエクスプローラー**で、新しいストーリーボードの名前をダブルクリックして、Xcode の Interface Builder で編集するために開きます。
3. 通常どおりに新しいストーリーボードのシーンのレイアウトをデザインし、変更を保存します。 

    [インターフェイスをデザイン ![には](indepth-images/ref02.png)](indepth-images/ref02.png#lightbox)
4. Interface Builder に、参照を追加するストーリーボードに切り替えます。
5. **オブジェクトライブラリ**からデザインサーフェイスに**ストーリーボード参照**をドラッグします。 

    [ライブラリでストーリーボード参照を選択 ![には](indepth-images/ref03.png)](indepth-images/ref03.png#lightbox)
6. **属性インスペクター**で、上で作成した**ストーリーボード**の名前を選択します。 

    [参照の構成 ![](indepth-images/ref04.png)](indepth-images/ref04.png#lightbox)
7. 既存のシーンで UI ウィジェット (ボタンなど) をコントロールクリックし、作成した**ストーリーボード参照**に新しいセグエを作成します。  ポップアップメニューから **[表示]** を選択して、セグエを完了します。 

    [セグエの種類の設定 ![](indepth-images/ref06.png)](indepth-images/ref06.png#lightbox) 
8. ストーリーボードへの変更を保存します。
9. Visual Studio for Mac に戻り、変更を同期します。

アプリを実行し、ユーザーがセグエを作成した UI 要素をクリックすると、ストーリーボードの参照で指定された外部ストーリーボードの最初のウィンドウコントローラーが表示されます。

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>外部ストーリーボードでの特定のシーンの参照

特定のシーンへの参照を (最初のウィンドウコントローラーではなく) 外部ストーリーボードに追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、外部ストーリーボードをダブルクリックして、Xcode の Interface Builder で編集するために開きます。
2. 次のように、新しいシーンを追加し、そのレイアウトをデザインします。 

    [Xcode でレイアウトをデザインする ![](indepth-images/ref07.png)](indepth-images/ref07.png#lightbox)
3. **Id インスペクター**で、新しいシーンのウィンドウコントローラーの**ストーリーボード ID**を入力します。 

    [ストーリーボード ID の設定 ![](indepth-images/ref08.png)](indepth-images/ref08.png#lightbox)
4. Interface Builder に参照を追加するストーリーボードを開きます。
5. **オブジェクトライブラリ**からデザインサーフェイスに**ストーリーボード参照**をドラッグします。 

    [ライブラリからストーリーボード参照を選択 ![には](indepth-images/ref03.png)](indepth-images/ref03.png#lightbox)
6. **Id インスペクター**で、前の手順で作成したシーンの**ストーリーボード**の名前と**参照 ID** (ストーリーボード id) を選択します。 

    [参照 ID の設定 ![](indepth-images/ref09.png)](indepth-images/ref09.png#lightbox)
7. 既存のシーンで UI ウィジェット (ボタンなど) をコントロールクリックし、作成した**ストーリーボード参照**に新しいセグエを作成します。 ポップアップメニューから **[表示]** を選択して、セグエを完了します。 

    [セグエの種類の設定 ![](indepth-images/ref06.png)](indepth-images/ref06.png#lightbox) 
8. ストーリーボードへの変更を保存します。
9. Visual Studio for Mac に戻り、変更を同期します。

アプリが実行され、ユーザーがセグエを作成した UI 要素をクリックすると、ストーリーボードの参照で指定された外部ストーリーボードから、指定された**ストーリーボード ID**を持つシーンが表示されます。

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>同じストーリーボード内の特定のシーンの参照

同じストーリーボードに特定のシーンへの参照を追加するには、次の手順を実行します。

1. **ソリューションエクスプローラー**で、ストーリーボードをダブルクリックして開き、編集します。
2. 次のように、新しいシーンを追加し、そのレイアウトをデザインします。 

    [Xcode でストーリーボードを編集 ![には](indepth-images/ref11.png)](indepth-images/ref11.png#lightbox)
3. **Id インスペクター**で、新しいシーンのウィンドウコントローラーの**ストーリーボード ID**を入力します。 

    [ストーリーボード ID の設定 ![](indepth-images/ref12.png)](indepth-images/ref12.png#lightbox)
4. **ツールボックス**からデザインサーフェイスに**ストーリーボード参照**をドラッグします。 

    [ライブラリからストーリーボード参照を選択 ![には](indepth-images/ref03.png)](indepth-images/ref03.png#lightbox)
5. **属性インスペクター**で、前の手順で作成したシーンの [**参照 ID** (ストーリーボード id)] を選択します。 

    [参照 ID の設定 ![](indepth-images/ref13.png)](indepth-images/ref13.png#lightbox)
6. 既存のシーンで UI ウィジェット (ボタンなど) をコントロールクリックし、作成した**ストーリーボード参照**に新しいセグエを作成します。 ポップアップメニューから **[表示]** を選択して、セグエを完了します。 

    [セグエの種類を選択する ![](indepth-images/ref06.png)](indepth-images/ref06.png#lightbox) 
7. ストーリーボードへの変更を保存します。
8. Visual Studio for Mac に戻り、変更を同期します。

アプリが実行され、ユーザーがセグエを作成した UI 要素をクリックすると、ストーリーボードの参照で指定された同じストーリーボードに、指定された**ストーリーボード ID**を持つシーンが表示されます。

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>複雑なストーリーボードの例

Xamarin. Mac アプリでストーリーボードを操作する複雑な例については、 [Sourcewriter サンプルアプリ](https://docs.microsoft.com/samples/xamarin/mac-samples/sourcewriter)を参照してください。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter コード全体に詳細なコメントが付いていて、可能な場合は、重要な技術やメソッド、Xamarin.Mac ガイド ドキュメントの関連情報へのリンクが示されます。

## <a name="related-links"></a>関連リンク

- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
