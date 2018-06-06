---
title: Xamarin.Mac 内のストーリー ボードの使用
description: このドキュメントでは、Xamarin.Mac、コード、ビュー コント ローラーのライフ サイクル、レスポンダーのチェーンからそれらを読み込む方法を調べることでストーリー ボードを使用する方法について説明します、segues、ウィンドウのコント ローラー、ジェスチャ レコグナイザーなどです。
ms.prod: xamarin
ms.assetid: DF4DF7C2-DDD7-4A32-B375-5C5446301EC5
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 72986ed4247c3b6f66f6f1813d74bf0a95d0de53
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792841"
---
# <a name="working-with-storyboards-in-xamarinmac"></a>Xamarin.Mac 内のストーリー ボードの使用

ストーリー ボードでは、すべてのコント ローラーの表示の機能の概要に分割する特定のアプリの UI を定義します。 Xcode のインターフェイスのビルダーでは、独自のシーンにこれらのコント ローラーの各住んでいます。

[![](indepth-images/intro01.png "Xcode のインターフェイスのビルダー内のストーリー ボード")](indepth-images/intro01.png#lightbox)

ストーリー ボードは、リソース ファイル (拡張子の`.storyboard`) は、コンパイルおよび出荷されたときに、Xamarin.Mac アプリのバンドルに含まれるを取得します。 アプリの開始、ストーリー ボードを定義する編集の`Info.plist`ファイルおよび選択した、 **Main インターフェイス**ドロップダウン ボックスから。 

[![](indepth-images/sb01.png "Info.plist エディター")](indepth-images/sb01.png#lightbox)

<a name="Loading-from-Code" />

## <a name="loading-from-code"></a>コードからの読み込み

コードから特定のストーリー ボードを読み込んで、ビューのコント ローラーを手動で作成する必要がある生じる可能性があります。 次のコードを使用すると、この操作を実行します。

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);
```

`FromName`指定した名前のアプリのバンドルに含まれているストーリー ボード ファイルを読み込みます。 `InstantiateControllerWithIdentifier`指定された Id を持つビュー コント ローラーのインスタンスを作成します。 UI を設計するときは、Xcode のインターフェイスのビルダーで Id を設定します。

[![](indepth-images/sb02.png "ストーリー ボード ID を設定")](indepth-images/sb02.png#lightbox)

必要に応じて、使用することができます、`InstantiateInitialController`インターフェイス ビルダーで初期のコント ローラーが割り当てられているビュー コント ローラーを読み込みます。

[![](indepth-images/sb03.png "最初のコント ローラーを設定")](indepth-images/sb03.png#lightbox)

マークされている、**ストーリー ボード エントリ ポイント**と上記の終了開く矢印です。

<a name="View-Controllers" />

## <a name="view-controllers"></a>コント ローラーの表示

コント ローラーの表示は、Mac アプリ内の情報の特定のビューと、その情報を提供するデータ モデル間のリレーションシップを定義します。 ストーリー ボードの各トップ レベル シーンでは、Xamarin.Mac アプリのコード内の 1 つのビュー コント ローラーを表します。

<a name="The-View-Controller-Lifecycle" />

### <a name="the-view-controller-lifecycle"></a>ビューのコント ローラーのライフ サイクル

いくつかの新しいメソッドに追加された、 `NSViewController` macOS でストーリー ボードをサポートするクラス。 次の方法を使用して、指定されたビュー コント ローラーによって管理されているビューのライフ サイクルに応答が最も重要なは。

- `ViewDidLoad` -このメソッドは、ストーリー ボード ファイルからビューが読み込まれるときに呼び出されます。
- `ViewWillAppear` -このメソッドは、ビューは、画面に表示される直前に呼び出されます。
- `ViewDidAppear` -ビューが画面に表示された後、このメソッドが直接呼び出されます。
- `ViewWillDisappear` -このメソッドは、画面から、ビューが削除される直前に呼び出されます。
- `ViewDidDisappear` 、画面から、ビューが削除された後このメソッドが直接呼び出されます。
- `UpdateViewConstraints` -このメソッドは、ビューを定義する制約の自動レイアウト位置とサイズ更新が必要です。
- `ViewWillLayout` -このメソッドは、このビューのサブビューが画面のレイアウトの直前に呼び出されます。
- `ViewDidLayout` -ビューのサブビューが画面に配置した後、このメソッドが直接呼び出されます。

<a name="The-Responder-Chain" />

### <a name="the-responder-chain"></a>応答側チェーン

さらに、`NSViewControllers`のウィンドウの一部であるようになりました_レスポンダー チェーン_:

[![](indepth-images/vc01.png "応答側チェーン")](indepth-images/vc01.png#lightbox)

そのため、ワイヤード (有線) アップを受け取り、切り取り、コピー、貼り付けのメニュー項目の選択などのイベントに応答します。 この自動ビュー コント ローラー ワイヤは macOS Sierra (10.12) で実行されているアプリでのみ行われます値を超えています。

<a name="Containment" />

### <a name="containment"></a>含有

ストーリー ボードでコント ローラーの表示 (分割ビュー コント ローラー タブのビュー コント ローラーなど) を実装できるようになりました_コンテインメント_「がある」その他のサブ コント ローラーの表示になるよう、します。

[![](indepth-images/vc02.png "ビューのコント ローラーのコンテインメントの例")](indepth-images/vc02.png#lightbox)

子ビュー コント ローラーには、メソッドが含まれてし、プロパティに結びつけることが、親ビューのコント ローラーおよび表示すると、画面から削除、ビューを操作するをバックアップします。

MacOS に組み込まれているすべてのコンテナー ビュー コント ローラーでは、Apple 独自カスタム コンテナー コント ローラーの表示を作成する場合に従うことを示す特定のレイアウトがあります。

[![](indepth-images/vc03.png "ビュー コント ローラー レイアウト")](indepth-images/vc03.png#lightbox)

コレクション ビューのコント ローラーには、コレクション アイテムの表示、独自のビューを含む 1 つまたは複数のビュー コント ローラーを含むそれぞれの配列が含まれています。

<a name="Segues" />

## <a name="segues"></a>Segues

Segues アプリの UI を定義するシーンの間の関係を提供します。 Ios ストーリー ボードでの作業に慣れている場合がわかっている Segues 用、iOS は、通常、全画面ビュー間の遷移を定義します。 これとは異なります macOS、Segues は通常を定義するときに"[コンテインメント](#Containment)"で、1 つのシーンは、親シーンの子です。

MacOS、ほとんどのアプリを分割ビューやタブなどの UI 要素を使用して、同じウィンドウ内で同時に、ビューをグループ化する傾向があります。 IOS、上と画面をオフに遷移するビューが必要があるとは異なり制限物理により領域を表示できます。

<a name="Presentation-Segues" />

### <a name="presentation-segues"></a>プレゼンテーション Segues します。

場合によっては包含に向かって macOS の傾向を指定するには、ここで_プレゼンテーション Segues_モーダル ウィンドウ、シート ビューおよび Popover などが使用されます。 次の組み込みの話題提供 macOS 型します。

- **表示**-非モーダル ウィンドウとして、Segue のターゲットを表示します。 たとえば、アプリのドキュメント ウィンドウの別のインスタンスを提示するのに Segue のこの型を使用します。
- **モーダル**-モーダル ウィンドウとして Segue のターゲットを表示します。 たとえば、Segue のこの型を使用して、アプリの環境設定ウィンドウを表示します。
- **シート**-親ウィンドウにアタッチされて、シート、Segue のターゲットを表示します。 例については、この種類は、検索と置換のシートを表示する提案を使用します。
- **重なって**-重なってウィンドウと同様に、Segue のターゲットを表示します。 たとえば、UI 要素が、ユーザーがクリックされたときに、オプションを表示するのにこの Segue 型を使用します。
- **カスタム**-開発者によって定義されたカスタムの話題型を使用して Segue のターゲットを表示します。 参照してください、[を作成するカスタム Segues](#Creating-Custom-Segues)詳細については、後述の「します。

オーバーライドできますプレゼンテーション Segues を使用する場合、`PrepareForSegue`および変数の初期化にプレゼンテーション親ビュー コント ローラーのメソッド、表示されているビュー コント ローラーに任意のデータを提供します。

<a name="Triggered-Segues" />

### <a name="triggered-segues"></a>トリガーされる Segues

トリガーされた Segues を使用すると、名前付き Segues を指定できます (を使用して、**識別子**インターフェイス ビルダーでのプロパティ) または呼び出すことによって、ユーザーがボタンのクリックしてなどのイベントによってトリガーし、`PerformSegue`コード内のメソッド。

```csharp
// Display the Scene defined by the given Segue ID
PerformSegue("MyNamedSegue", this);
``` 

話題 ID は、アプリの UI をレイアウトするときに、Xcode のインターフェイスのビルダー内で定義されます。

[![](indepth-images/sg02.png "入力する、名前の話題")](indepth-images/sg02.png#lightbox)

オーバーライドする必要があります、Segue のソースとして動作しているビュー コント ローラー、`PrepareForSegue`メソッドと、Segue が実行される前に、初期化が必要な操作とビューの指定されたコント ローラーが表示されます。

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

必要に応じて、オーバーライドすることができます、`ShouldPerfromSegue`メソッドと c# コードを使用して、Segue が実際に実行されるかどうか制御します。 手動で示されたビュー コント ローラーを呼び出して、`DismissController`不要になったときに、それらを表示から削除するメソッド。

<a name="Creating-Custom-Segues" />

### <a name="creating-custom-segues"></a>Segues カスタムを作成します。

アプリが用意されていない macOS で定義されているビルドで Segues Segue 型を必要とする場合があります。 大文字と小文字の場合は、作成割り当てることができるカスタム話題 Xcode のインターフェイスのビルダーで、アプリの UI レイアウトに使用するときにします。

たとえば、(の代わりに、新しいウィンドウで、ターゲット シーンを開く) ウィンドウ内の現在のビュー コント ローラーを置き換える新しい Segue 型を作成するには、次のコードを使用おことができます。

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

いくつか次に注意してください:

- 使用して、`Register`目標-C/macOS に、このクラスを公開する属性。
- 私たちが上書きされて、`Perform`メソッドを実際には、カスタム Segue のアクションを実行します。
- ウィンドウの置き換えられるの`ContentViewController`コント ローラー、Segue のターゲット (ターゲット) によって定義されたものとします。
- 使用してメモリを解放するための元のビュー コント ローラーを削除する予定です、`RemoveFromParentViewController`メソッドです。

この新しい Segue タイプを Xcode のインターフェイスのビルダーを使用するのには、最初に、このアプリをコンパイルし、Xcode に切り替え、2 つのシーンの間で新しい Segue を追加する必要があります。 設定、**スタイル**に**カスタム**と**話題クラス**に`ReplaceViewSegue`(カスタム Segue クラスの名前)。

[![](indepth-images/sg01.png "Setting Segue クラス")](indepth-images/sg01.png#lightbox)

<a name="Triggered-Segues" />

## <a name="window-controllers"></a>ウィンドウのコント ローラー

ウィンドウのコント ローラーが含まれてし、macOS アプリを作成できるさまざまなウィンドウの種類を制御します。 ストーリー ボードの次の機能があります。

1. コンテンツ ビュー コント ローラーを提供する必要があります。 同じコンテンツ ビューを持つコント ローラー ウィンドウの子になります。
2. `Storyboard`プロパティには、ウィンドウのコント ローラーから読み込まれた、それ以外の場合、ストーリー ボード`null`ストーリー ボードから読み込まれていない場合。
3. 呼び出すことができます、`DismissController`メソッドを指定されたウィンドウを閉じるし、ビューから削除します。

ウィンドウのコント ローラーを実装するコント ローラーの表示と同様に、 `PerformSegue`、`PrepareForSegue`と`ShouldPerfromSegue`メソッド Segue 操作のソースとして使用できます。

ウィンドウのコント ローラーは、macOS アプリの次の機能を担当します。

- これらは、特定の改ページを管理します。
- 管理ウィンドウのタイトル バーとツールバー (使用可能な場合)。
- ウィンドウの内容を表示するコンテンツのビュー コント ローラーを管理します。

<a name="Gesture-Recognizers" />

## <a name="gesture-recognizers"></a>ジェスチャ レコグナイザー

MacOS のジェスチャ レコグナイザーは iOS の対応するとほぼ同じであり、アプリの UI 要素に簡単に (マウスのボタンをクリック) などのジェスチャを追加する開発者が許可されます。

ただし、ここで、iOS でジェスチャによって決定されます (2 本の指で画面をタップ) するなど、アプリのデザイン最も macOS でジェスチャはハードウェアによって決定されます。

ジェスチャ レコグナイザーを使用すると、UI 内の項目にカスタムの相互作用を追加するために必要なコードの量を大幅に減らすことができます。 できる自動的に決定するには double と 1 つのクリックの間で、をクリックし、イベントなどをドラッグします。

オーバーライドする代わりに、`MouseDown`ビュー コント ローラー内のイベント、する必要がありますを使用しているジェスチャ レコグナイザー ストーリー ボードを操作するときのユーザー入力イベントを処理します。

次のジェスチャ レコグナイザーは、macOS で使用できます。

- `NSClickGestureRecognizer` -マウス ダウンし、上のイベントを登録します。
- `NSPanGestureRecognizer` 登録は、マウス ダウン ボタン、ドラッグし、イベントを離します。
- `NSPressGestureRecognizer` 起動時のイベントが一定時間のマウス ボタンを押した登録します。
- `NSMagnificationGestureRecognizer` -トラック パッド ハードウェアからの拡大率イベントを登録します。
- `NSRotationGestureRecognizer` -トラック パッド ハードウェアから回転イベントを登録します。

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>ストーリー ボードの参照の使用

ストーリー ボードの参照では、大規模で複雑なストーリー ボードのデザインおよび、元の参照を取得するより小さなストーリー ボードに分割することができます、したがって削除複雑さを削除して、その結果を個別に行う ストーリー ボード簡単にデザインして維持します。

また、ストーリー ボードの参照を提供できます、_アンカー_同じストーリー ボードまたは別の特定のシーン内の別のシーンにします。

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>外部のストーリー ボードを参照します。

外部のストーリー ボードへの参照を追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、プロジェクト名を右クリックし **追加** > **新しいファイル.**  >  **Mac** > **ストーリー ボード**です。 入力、**名前**新しいストーリー ボードとをクリックして、**新規**ボタン。 

    [![](indepth-images/ref01.png "新しいストーリー ボードを追加します。")](indepth-images/ref01.png#lightbox)
2. **ソリューション エクスプ ローラー**名をダブルクリックして、新しいストーリー ボード ファイルを開いて Xcode のインターフェイスのビルダーで編集します。
2. 操作と同じよう通常、変更を保存は、新しいストーリー ボードのシーンのレイアウトをデザインします。 

    [![](indepth-images/ref02.png "インターフェイスの設計")](indepth-images/ref02.png#lightbox)
3. インターフェイス ビルダーへの参照を追加しようとしているストーリー ボードに切り替えます。
4. ドラッグ、**参照をストーリー ボード**から、**オブジェクト ライブラリ**デザイン サーフェイスに。 

    [![](indepth-images/ref03.png "ライブラリ内の参照をストーリー ボードを選択します。")](indepth-images/ref03.png#lightbox)
5. **属性インスペクター**の名前を選択、**ストーリー ボード**上に作成します。 

    [![](indepth-images/ref04.png "参照を構成します。")](indepth-images/ref04.png#lightbox)
6. 既存のシーンで (ボタン) のような UI ウィジェットのコントロールをクリックしを新しい Segue を作成、**ストーリー ボード参照**先ほど作成しました。  ポップアップ メニューから選択**表示**Segue を完了します。 

    [![](indepth-images/ref06.png "Segue の種類を設定")](indepth-images/ref06.png#lightbox) 
8. ストーリー ボードに変更を保存します。
9. 変更内容を同期する Mac 用の Visual Studio に戻ります。

アプリが実行され、ユーザーがクリックした UI 要素上、ストーリー ボードの参照で指定された外部のストーリー ボードから初期ウィンドウ コント ローラーから、Segue を作成したときに表示されます。

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>外部のストーリー ボード内の特定のシーンを参照します。

特定のシーンへの参照を追加するには、外部のストーリー ボード (および初期のウィンドウ コント ローラー以外)、次を操作します。

1. **ソリューション エクスプ ローラー**、外部のストーリー ボード ファイルを開いて Xcode のインターフェイスのビルダーで編集する をダブルクリックします。
2. 新しいシーンを追加し、通常どおりにそのレイアウトをデザインします。 

    [![](indepth-images/ref07.png "Xcode でレイアウトのデザイン")](indepth-images/ref07.png#lightbox)
3. **Identity インスペクター**、入力、**ストーリー ボード ID**新しいシーンのウィンドウのコント ローラー。 

    [![](indepth-images/ref08.png "ストーリー ボード ID を設定")](indepth-images/ref08.png#lightbox)
3. インターフェイスのビルダーへの参照を追加することになっているストーリー ボードを開きます。
4. ドラッグ、**参照をストーリー ボード**から、**オブジェクト ライブラリ**デザイン サーフェイスに。 

    [![](indepth-images/ref03.png "ライブラリからの参照をストーリー ボードを選択します。")](indepth-images/ref03.png#lightbox)
5. **Identity インスペクター**の名前を選択、**ストーリー ボード**と**参照 ID** (ストーリー ボード ID) 上で作成した、シーンの。 

    [![](indepth-images/ref09.png "参照 ID の設定")](indepth-images/ref09.png#lightbox)
6. 既存のシーンで (ボタン) のような UI ウィジェットのコントロールをクリックしを新しい Segue を作成、**ストーリー ボード参照**先ほど作成しました。 ポップアップ メニューから選択**表示**Segue を完了します。 

    [![](indepth-images/ref06.png "Segue の種類を設定")](indepth-images/ref06.png#lightbox) 
8. ストーリー ボードに変更を保存します。
9. 変更内容を同期する Mac 用の Visual Studio に戻ります。

UI 要素を作成された、Segue のシーンをアプリが実行し、ユーザーがクリックした、指定された**ストーリー ボード ID**からストーリー ボードの参照で指定された外部のストーリー ボードが表示されます。

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>同じストーリー ボード内の特定のシーンを参照します。

特定のシーン同じストーリー ボードへの参照を追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、ストーリー ボード ファイルを開いて編集する をダブルクリックします。
2. 新しいシーンを追加し、通常どおりにそのレイアウトをデザインします。 

    [![](indepth-images/ref11.png "Xcode でストーリー ボードの編集")](indepth-images/ref11.png#lightbox)
3. **Identity インスペクター**、入力、**ストーリー ボード ID**新しいシーンのウィンドウのコント ローラー。 

    [![](indepth-images/ref12.png "ストーリー ボード ID を設定")](indepth-images/ref12.png#lightbox)
3. ドラッグ、**参照をストーリー ボード**から、**ツールボックス**デザイン サーフェイスに。 

    [![](indepth-images/ref03.png "ライブラリからの参照をストーリー ボードを選択します。")](indepth-images/ref03.png#lightbox)
5. **属性インスペクター****参照 ID** (ストーリー ボード ID) 上で作成した、シーンの。 

    [![](indepth-images/ref13.png "参照 ID の設定")](indepth-images/ref13.png#lightbox)
6. 既存のシーンで (ボタン) のような UI ウィジェットのコントロールをクリックしを新しい Segue を作成、**ストーリー ボード参照**先ほど作成しました。 ポップアップ メニューから選択**表示**Segue を完了します。 

    [![](indepth-images/ref06.png "Segue の種類の選択")](indepth-images/ref06.png#lightbox) 
8. ストーリー ボードに変更を保存します。
9. 変更内容を同期する Mac 用の Visual Studio に戻ります。

UI 要素を作成された、Segue のシーンをアプリが実行し、ユーザーがクリックした、指定された**ストーリー ボード ID**ストーリー ボードの参照で指定された同じストーリー ボードが表示されます。

<a name="Complex-Storyboard-Example" />

## <a name="complex-storyboard-example"></a>複雑なストーリー ボードの使用例

ストーリー ボードで Xamarin.Mac アプリでの作業の複雑な例を参照してください、 [SourceWriter サンプル アプリ](https://developer.xamarin.com/samples/mac/SourceWriter/)です。 SourceWriter は、コードの完了とシンプルな構文の強調表示をサポートするシンプルなソース コード エディターです。

SourceWriter コード全体に詳細なコメントが付いていて、可能な場合は、重要な技術やメソッド、Xamarin.Mac ガイド ドキュメントの関連情報へのリンクが示されます。

## <a name="related-links"></a>関連リンク

- [MacStoryboard (サンプル)](https://developer.xamarin.com/samples/mac/MacStoryboard/)
- [Hello Mac](~/mac/get-started/hello-mac.md)
- [ウィンドウの操作](~/mac/user-interface/window.md)
- [OS X ヒューマン インターフェイス ガイドライン](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Windows の概要](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
