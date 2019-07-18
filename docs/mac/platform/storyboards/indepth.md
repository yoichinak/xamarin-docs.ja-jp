---
title: Xamarin.Mac でストーリー ボードの使用
description: このドキュメントでは、コード、ビュー コント ローラーのライフ サイクル、応答側のチェーンからそれらを読み込む方法を調べて、Xamarin.Mac でストーリー ボードを使用する方法について説明します、セグエ、ウィンドウのコント ローラーやジェスチャ認識機能です。
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: e24b448bedc60a537bfcd4a5bfbdbe9562163818
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865930"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Xamarin.Mac でストーリー ボードの使用

ストーリー ボードでは、すべてのビュー コント ローラーの機能の概要に分割する特定のアプリの UI を定義します。 Xcode の Interface Builder では、これらのコント ローラーの各は、独自のシーンでは存在します。

[![](indepth-images/intro01.png "Xcode の Interface Builder のストーリー ボード")](indepth-images/intro01.png#lightbox)

ストーリー ボードはリソース ファイル (の拡張子を持つ`.storyboard`) は、コンパイルおよび出荷されたときに、Xamarin.Mac アプリのバンドルに含まれるを取得します。 アプリの開始のストーリー ボードを定義する編集の`Info.plist`ファイルし、選択、**メイン インターフェイス**ドロップダウン ボックスから。 

[![](indepth-images/sb01.png "Info.plist エディター")](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>コードからの読み込み

コードから特定のストーリー ボードを読み込むし、ビュー コント ローラーを手動で作成する必要がある場合があります。 次のコードを使用すると、この操作を実行します。

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

`FromName`アプリのバンドルに含まれている指定した名前のストーリー ボード ファイルを読み込みます。 `InstantiateControllerWithIdentifier`特定の Id を持つビュー コント ローラーのインスタンスを作成します。 UI を設計するときに、Xcode の Interface Builder で Id を設定します。

[![](indepth-images/sb02.png "ストーリー ボード ID の設定")](indepth-images/sb02.png#lightbox)

必要に応じて、使用、`InstantiateInitialController`インターフェイス ビルダーでの最初のコント ローラーが割り当てられているビュー コント ローラーを読み込みます。

[![](indepth-images/sb03.png "最初のコント ローラーを設定")](indepth-images/sb03.png#lightbox)

マークされている、**ストーリー ボードのエントリ ポイント**し、上記のオープン終了方向。

<a name="View-Controllers" />

## <a name="view-controllers"></a>ビュー コント ローラー

ビュー コント ローラーは、Mac アプリ内の情報の指定されたビューとその情報を提供するデータ モデルの間のリレーションシップを定義します。 ストーリー ボードの各最上位レベルのシーンは、Xamarin.Mac アプリのコード内の 1 つのビュー コント ローラーを表します。

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>ビュー コント ローラーのライフ サイクル

いくつかの新しいメソッドに追加された、 `NSViewController` macOS でストーリー ボードをサポートするクラス。 最も重要なは、次の方法を使用して、特定のビュー コント ローラーによって管理されているビューのライフ サイクルに応答します。

- `ViewDidLoad` -このメソッドは、ストーリー ボード ファイルからビューが読み込まれるときに呼び出されます。
- `ViewWillAppear` -このメソッドは、ビューが画面に表示される直前に呼び出されます。
- `ViewDidAppear` -ビューが画面に表示された後、直接このメソッドが呼び出されます。
- `ViewWillDisappear` -このメソッドは、画面から、ビューが削除される直前に呼び出されます。
- `ViewDidDisappear` -ビューが画面から削除された後、直接このメソッドが呼び出されます。
- `UpdateViewConstraints` -ビューを定義する制約の自動レイアウト位置とサイズを更新する必要性このメソッドが呼び出されます。
- `ViewWillLayout` -このメソッドは、このビューのサブビューが画面のレイアウトの直前に呼び出されます。
- `ViewDidLayout` -ビューのサブビューが画面に配置した後、直接このメソッドが呼び出されます。

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>応答側のチェーン

さらに、`NSViewControllers`のウィンドウの一部になった_レスポンダー チェーン_:

[![](indepth-images/vc01.png "応答側のチェーン")](indepth-images/vc01.png#lightbox)

そのため、取得、切り取り、コピーおよび貼り付けメニュー項目の選択などのイベントに応答するワイヤード (有線) アップします。 この自動ビュー コント ローラーに接続は、macOS Sierra (10.12) で実行されているアプリでのみ行われます以降。

<a name="Containment" />

### <a name="containment"></a>含有

(分割ビュー コント ローラー タブのビュー コント ローラーなど) のビュー コント ローラーを今すぐ実装するストーリー ボード、_コンテインメント_よう他のサブ ビュー コント ローラーを「含める」ことな。

[![](indepth-images/vc02.png "ビュー コント ローラーのコンテインメントの例")](indepth-images/vc02.png#lightbox)

子ビュー コント ローラーはメソッドを含み、プロパティに結びつけること操作を表示して、画面からビューを削除して、親ビュー コント ローラーへのバックアップします。

MacOS に組み込まれているすべてのコンテナーのビュー コント ローラーでは、Apple は、独自のコンテナーのカスタムのビュー コント ローラーを作成する場合に従うことをお勧めしますが、特定のレイアウトがあります。

[![](indepth-images/vc03.png "ビュー コント ローラーのレイアウト")](indepth-images/vc03.png#lightbox)

コレクション ビュー コント ローラーには、それぞれのビューが含まれている 1 つまたは複数のビュー コント ローラーを含む各コレクション ビュー項目の配列が含まれています。

<a name="Segues" />

## <a name="segues"></a>セグエ

セグエ、アプリの UI を定義するシーンの間の関係を提供します。 Ios ストーリー ボードでの作業について熟知する場合は、あるとわかっているセグエの iOS は、通常、全画面表示ビュー間の遷移を定義します。 Segues が通常定義と、macOS からこれに対し"[コンテインメント](#Containment)"、1 つのシーンはシーンの親の子です。

MacOS、ほとんどのアプリを分割ビュー タブなどの UI 要素を使用して、同じウィンドウ内のビューをグループ化する傾向があります。 ビューが画面外に移行する必要がある、iOS とは異なりの限られた物理により領域を表示します。

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>プレゼンテーションをセグエします。

包含への macOS の傾向を指定するには、状況がある場所_プレゼンテーション セグエ_モーダル Windows、シート ビュー Popovers などを使用します。 macOS 提供しますが、次の組み込みのセグエの種類します。

- **表示**-非モーダル ウィンドウとして、セグエのターゲットを表示します。 たとえば、アプリでのドキュメント ウィンドウの別のインスタンスを提示するのにこの種類のセグエを使用します。
- **モーダル**-モーダル ウィンドウとしてセグエの対象を表示します。 たとえば、アプリの環境設定ウィンドウを表示するのにこのタイプのセグエを使用します。
- **シート**-親ウィンドウに接続されている場合、シート、セグエの対象を表示します。 たとえば、この種類のセグエ検索と置換のシートを使用します。
- **ポップ オーバー** -ポップ オーバー ウィンドウのように、セグエの対象を表示します。 たとえば、このセグエの種類を使用して、UI 要素が、ユーザーがクリックされたときの選択肢が表示されます。
- **カスタム**-開発者によって定義されたカスタムのセグエ型を使用してセグエの対象を表示します。 参照してください、[を作成するカスタム セグエ](#Creating-Custom-Segues)詳細については後述します。

プレゼンテーションのセグエを使用する場合をオーバーライドできます、`PrepareForSegue`親ビュー コント ローラーのメソッドをおよび変数の初期化に提示する、表示されているビュー コント ローラーへのデータを提供します。

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>トリガー セグエ

トリガーされた Segues を使用すると、名前付きの Segues を指定できます (を使用して、**識別子**インターフェイス ビルダーでのプロパティ) または呼び出すことによって、ユーザーがボタンのクリックしてなどのイベントでトリガーし、`PerformSegue`コード内のメソッド。

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

セグエ ID は、アプリの UI をレイアウトするときに、Xcode の Interface Builder の内部で定義されます。

[![](indepth-images/sg02.png "入力、セグエ名")](indepth-images/sg02.png#lightbox)

オーバーライドする必要があります、セグエのソースとして動作しているビュー コント ローラーで、`PrepareForSegue`メソッドと、セグエを実行する前に、すべての初期化が必要な操作とビューの指定されたコント ローラーが表示されます。

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

必要に応じて、オーバーライドすることができます、`ShouldPerformSegue`メソッドとコントロールを使用して、セグエを実際に実行するかどうかC#コード。 手動で表示されるビュー コント ローラーを呼び出して、`DismissController`不要になったときに、それらを表示から削除するメソッド。

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>セグエ カスタム作成

アプリが macOS で定義されているビルドの Segues によって提供されないセグエの種類を必要とする場合があります。 大文字と小文字の場合は、作成割り当てることができるカスタム セグエ Xcode の Interface Builder でアプリの UI をレイアウトするときにします。

たとえば、(新しいウィンドウで、ターゲットのシーンを開く) ではなく、ウィンドウ内の現在のビュー コント ローラーを置き換える新しいセグエの種類を作成する次のコードを使用できます。

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

- 使用している、`Register`目的-C/macOS には、このクラスを公開する属性。
- オーバーライドして、`Perform`実際には、カスタムのセグエのアクションを実行するメソッド。
- ウィンドウの置き換えられるの`ContentViewController`コント ローラー、セグエのターゲット (宛先) で定義されているものとします。
- 使用したメモリを解放するための元のビュー コント ローラーを削除する予定です、`RemoveFromParentViewController`メソッド。

この新しいセグエの種類では、Xcode の Interface Builder を使用するには、アプリを最初に、コンパイルし、Xcode に切り替え、新しい 2 つのシーンの間にセグエを追加する必要があります。 設定、**スタイル**に**カスタム**と**セグエ クラス**に`ReplaceViewSegue`(、カスタムのセグエ クラスの名前)。

[![](indepth-images/sg01.png "セグエ クラスの設定")](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>ウィンドウのコント ローラー

ウィンドウ コント ローラーが含まれてし、macOS アプリを作成できるさまざまなウィンドウの種類を制御します。 ストーリー ボードには、次の機能があります。

1. コンテンツ ビュー コント ローラーが提供する必要があります。 子ウィンドウが同じコンテンツ View Controller になります。
2. `Storyboard`プロパティが読み込まれているウィンドウ コント ローラーから、それ以外の場合、ストーリー ボードを含む`null`ストーリー ボードから読み込まれていない場合。
3. 呼び出すことができます、`DismissController`メソッドを指定されたウィンドウを閉じるし、ビューから削除します。

ビュー コント ローラーのようなウィンドウのコント ローラーの実装、 `PerformSegue`、`PrepareForSegue`と`ShouldPerformSegue`メソッド セグエ操作のソースとして使用できます。

ウィンドウ コント ローラーは macOS アプリの次の機能を担当します。

- 特定のウィンドウを管理します。
- 管理は、ウィンドウのタイトル バーとツールバー (該当する場合)。
- ウィンドウの内容を表示するコンテンツのビュー コント ローラーを管理します。

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>ジェスチャ レコグナイザー

MacOS 用のジェスチャ レコグナイザーは iOS の対応するとほぼ同じであり、アプリの UI 要素に簡単に (マウス ボタンをクリックする) などのジェスチャを追加する開発者が許可されます。

ただしで iOS のジェスチャによって決定されます (2 本の指で画面をタップ) するなど、アプリのデザイン最も macOS のジェスチャはハードウェアによって決定されます。

ジェスチャ レコグナイザーを使用すると、UI 内の項目にカスタムの相互作用を追加するために必要なコードの量を大幅に削減できます。 ように自動的に二重と 1 つのクリック間隔を決定できます をクリックし、イベントなどをドラッグします。

オーバーライドする代わりに、`MouseDown`ビュー コント ローラーにイベントを使うべきジェスチャ レコグナイザー ストーリー ボードを使用する場合のユーザー入力イベントを処理します。

次のジェスチャ レコグナイザーは macOS で使用できます。

- `NSClickGestureRecognizer` -マウス ダウンし、イベントを登録します。
- `NSPanGestureRecognizer` 登録は、マウス ボタンの下にドラッグ アンド リリース イベント。
- `NSPressGestureRecognizer` 一定時間イベントのマウス ボタンを押した登録します。
- `NSMagnificationGestureRecognizer` -トラック パッド ハードウェアから拡大イベントを登録します。
- `NSRotationGestureRecognizer` -トラック パッド ハードウェアから回転イベントを登録します。

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>ストーリー ボードの参照の使用

ストーリー ボードの参照を使用すると、大規模で複雑なストーリー ボードのデザインを受け取り、小さいストーリー ボード、元の参照を取得するため複雑さを削除して、結果として得られる個別のストーリー ボードを簡単に設計および管理に分割することができます。

また、ストーリー ボードの参照を提供できます、_アンカー_同じストーリー ボードまたは別の特定のシーン内の別のシーンにします。

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>外部のストーリー ボードを参照します。

外部のストーリー ボードへの参照を追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**プロジェクト名を右クリックし、選択、**追加** > **新しいファイル.**  >  **Mac** > **ストーリー ボード**します。 入力、**名前**新しいストーリー ボードをクリックして、**新規**ボタン。 

    [![](indepth-images/ref01.png "新しいストーリー ボードを追加します。")](indepth-images/ref01.png#lightbox)
2. **ソリューション エクスプ ローラー**Xcode の Interface Builder での編集用に開きます新しいストーリー ボードの名前をダブルクリックします。
3. 通常は、変更を保存、新しいストーリー ボードのシーンのレイアウトをデザインします。 

    [![](indepth-images/ref02.png "インターフェイスの設計")](indepth-images/ref02.png#lightbox)
4. インターフェイス ビルダーでへの参照を追加すること、ストーリー ボードに切り替えます。
5. ドラッグ、**の参照をストーリー ボード**から、**オブジェクト ライブラリ**デザイン サーフェイスに。 

    [![](indepth-images/ref03.png "ライブラリ内の参照をストーリー ボードを選択します。")](indepth-images/ref03.png#lightbox)
6. **属性インスペクター**の名前を選択、**ストーリー ボード**上記で作成します。 

    [![](indepth-images/ref04.png "参照を構成します。")](indepth-images/ref04.png#lightbox)
7. (ボタン) のように既存のシーンでの UI ウィジェットをコントロール クリックしを新しいセグエを作成、**ストーリー ボードにリファレンス**作成しました。  ポップアップ メニューから選択**表示**セグエを完了します。 

    [![](indepth-images/ref06.png "セグエの種類の設定")](indepth-images/ref06.png#lightbox) 
8. ストーリー ボードに変更を保存します。
9. Visual Studio for Mac は、変更を同期に戻ります。

アプリが実行され、セグエをストーリー ボードの参照で指定された外部のストーリー ボードから初期ウィンドウ コント ローラーから作成した UI 要素に、ユーザーがクリックしたときに表示されます。

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>外部のストーリー ボードで特定のシーンを参照します。

特定のシーンへの参照を追加するには、外部のストーリー ボード (および最初のウィンドウ コント ローラー以外) が、次の実行します。

1. **ソリューション エクスプ ローラー**、外部のストーリー ボードを開き、Xcode の Interface Builder での編集 をダブルクリックします。
2. 新しいシーンを追加し、通常どおりにそのレイアウトをデザインします。 

    [![](indepth-images/ref07.png "Xcode でレイアウトのデザイン")](indepth-images/ref07.png#lightbox)
3. **Identity Inspector**、入力、**ストーリー ボード ID**新しいシーンのウィンドウのコント ローラー。 

    [![](indepth-images/ref08.png "ストーリー ボード ID の設定")](indepth-images/ref08.png#lightbox)
4. インターフェイス ビルダーでへの参照を追加すること、ストーリー ボードを開きます。
5. ドラッグ、**の参照をストーリー ボード**から、**オブジェクト ライブラリ**デザイン サーフェイスに。 

    [![](indepth-images/ref03.png "ライブラリからの参照をストーリー ボードを選択します。")](indepth-images/ref03.png#lightbox)
6. **Identity Inspector**の名前を選択、**ストーリー ボード**と**参照 ID** (ストーリー ボード ID) 上で作成したシーンの。 

    [![](indepth-images/ref09.png "参照 ID の設定")](indepth-images/ref09.png#lightbox)
7. (ボタン) のように既存のシーンでの UI ウィジェットをコントロール クリックしを新しいセグエを作成、**ストーリー ボードにリファレンス**作成しました。 ポップアップ メニューから選択**表示**セグエを完了します。 

    [![](indepth-images/ref06.png "セグエの種類の設定")](indepth-images/ref06.png#lightbox) 
8. ストーリー ボードに変更を保存します。
9. Visual Studio for Mac は、変更を同期に戻ります。

セグエをシーンにから作成した UI 要素をクリックする、アプリが実行され、ユーザー、特定**ストーリー ボード ID**からストーリー ボードの参照で指定された外部のストーリー ボードが表示されます。

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>同じストーリー ボードの特定のシーンを参照します。

特定のシーンと同じストーリー ボードへの参照を追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、ストーリー ボードを開き、編集をダブルクリックします。
2. 新しいシーンを追加し、通常どおりにそのレイアウトをデザインします。 

    [![](indepth-images/ref11.png "Xcode でストーリー ボードの編集")](indepth-images/ref11.png#lightbox)
3. **Identity Inspector**、入力、**ストーリー ボード ID**新しいシーンのウィンドウのコント ローラー。 

    [![](indepth-images/ref12.png "ストーリー ボード ID の設定")](indepth-images/ref12.png#lightbox)
4. ドラッグ、**の参照をストーリー ボード**から、**ツールボックス**デザイン サーフェイスに。 

    [![](indepth-images/ref03.png "ライブラリからの参照をストーリー ボードを選択します。")](indepth-images/ref03.png#lightbox)
5. **属性インスペクター**、**参照 ID** (ストーリー ボード ID) 上で作成したシーンの。 

    [![](indepth-images/ref13.png "参照 ID の設定")](indepth-images/ref13.png#lightbox)
6. (ボタン) のように既存のシーンでの UI ウィジェットをコントロール クリックしを新しいセグエを作成、**ストーリー ボードにリファレンス**作成しました。 ポップアップ メニューから選択**表示**セグエを完了します。 

    [![](indepth-images/ref06.png "セグエの種類の選択")](indepth-images/ref06.png#lightbox) 
7. ストーリー ボードに変更を保存します。
8. Visual Studio for Mac は、変更を同期に戻ります。

セグエをシーンにから作成した UI 要素をクリックする、アプリが実行され、ユーザー、特定**ストーリー ボード ID**でストーリー ボードの参照で指定された同じストーリー ボードが表示されます。

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>複雑なストーリー ボードの例

Xamarin.Mac アプリでストーリー ボードの操作の複雑な例を参照してください、 [SourceWriter サンプル アプリ](https://developer.xamarin.com/samples/mac/SourceWriter/)します。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter コード全体に詳細なコメントが付いていて、可能な場合は、重要な技術やメソッド、Xamarin.Mac ガイド ドキュメントの関連情報へのリンクが示されます。

## <a name="related-links"></a>関連リンク

- [MacStoryboard (サンプル)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [Windows の操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
