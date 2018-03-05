---
title: "ストーリー ボードの概要"
description: "ストーリー ボードは、外観を視覚的に表現し、アプリケーションのフローのです。 Xamarin には、利用するために、ストーリー ボードの視覚的に、アプリケーションの画面をデザインして、ビューにアクセスできるように Xamarin.iOS アプリケーション デザイナーが導入コント ローラーの詳細に制御を c# segues とします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: A3339BD2-9F56-7965-25F5-4B7C991EB775
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 8bc262ff739cc65da80d887a6dea11ecc708e866
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="introduction-to-storyboards"></a>ストーリー ボードの概要

_ストーリー ボードは、外観を視覚的に表現し、アプリケーションのフローのです。Xamarin には、利用するために、ストーリー ボードの視覚的に、アプリケーションの画面をデザインして、ビューにアクセスできるように Xamarin.iOS アプリケーション デザイナーが導入コント ローラーの詳細に制御を c# segues とします。_

このガイドでどのようなストーリー ボードを用いては、主要なコンポーネント – Segues などのいくつかを確認します。 ストーリー ボードを作成および使用方法に紹介開発者がどのような利点があるとします。

ストーリー ボード ファイル形式は、apple iOS アプリケーションの UI のビジュアル表現として導入された、前に、開発者は各ビューのコント ローラー用 XIB ファイルを作成し、各ビューの間のナビゲーションを手動でプログラムします。  ストーリー ボードを使用して、開発者はでき、コント ローラーの表示とデザイン画面上でそれらの間のナビゲーションの両方を定義し、アプリケーションのユーザー インターフェイスの WYSIWYG 編集を提供します。

ストーリー ボードの作成、開かれたおよび Xamarin iOS デザイナーを使用して編集します。 このガイドもチュートリアル、デザイナーを使用して、ナビゲーションをプログラミングする c# を使用しているときに、ストーリー ボードを作成する方法です。


## <a name="requirements"></a>必要条件

ストーリー ボードは、インストールされている Xamarin ワークロードで iOS Mac 用の Visual Studio のデザイナーや、Visual Studio 2015 や 2017 使用できます。

## <a name="what-is-a-storyboard"></a>ストーリー ボードとは何ですか。

ストーリー ボードは、アプリケーション内のすべての画面のビジュアル表現です。 各シーンを表す、シーンのシーケンスが含まれている、*ビュー コント ローラー*とその*ビュー*です。 これらのビューは、オブジェクトを含めることが、[コントロール](~/ios/user-interface/controls/index.md)により、ユーザー、アプリケーションと対話します。 このビューとコントロールのコレクション (または*サブビュー*) と呼ばれる、*コンテンツの階層を表示*です。 シーンが接続されているによって、コント ローラーの表示の間の遷移を表すオブジェクトを提案します。 これは通常、最初のビューでは、上のオブジェクトと接続しているビュー間 segue を作成することで実現されます。 デザイン サーフェイス上の関係は、次の図に示します。

 [ ![](images/storyboardsview.png "このイメージの図解は、デザイン画面上の関係")](images/storyboardsview.png)

ように、ストーリー ボードは、既に表示されてコンテンツを持つ、シーンの各をレイアウトし、両者間の接続を示しています。  IPhone でについてシーンは安全であるということを想定することをこの時点では、注目に値します*シーン*ストーリー ボードには 1 に等しい*画面*デバイス上のコンテンツのです。 ただし、一度に – 表示する複数のシーンを設定することは iPad でなど、重なってビュー コント ローラーを使用します。

これにはストーリー ボードを使用して、Xamarin を使用する場合は特に、アプリケーションの UI を作成する多くの利点があります。 含む – すべてのオブジェクトとして、UI のビジュアル表現はまず、[カスタム コントロール](~/ios/user-interface/designer/ios-designable-controls-overview.md)– はデザイン時に表示されます。 つまり、構築またはの外観とフローを視覚化するアプリケーションを配置する前にします。 たとえば、上の画像を考えてみましょう。 簡単に見て、デザイン サーフェス数では、各ビューのレイアウトおよびすべてのものがどのように関係から通知ことができます。 これは、新機能により、ストーリー ボードは、非常に強力なです。

イベントは、特に iOS デザイナーを使用する場合、ストーリー ボードでより管理しやすくします。 ほとんどの UI コントロールは、プロパティ パッドで使用できるイベントの一覧があります。 イベント ハンドラーをここに追加し、コント ローラーの表示のクラスの部分メソッドで完了しました.

ストーリー ボードのコンテンツは、XML ファイルとして格納されます。 At ビルド時、すべて`.storyboard`ファイル nibs と呼ばれるバイナリ ファイルにコンパイルします。 実行時に、これら nibs が初期化され、新しいビューを作成するインスタンス化します。

## <a name="segues"></a>Segues

A *Segue*、または*話題オブジェクト*シーンの間の遷移を表すために iOS 開発に使用します。 押しながら、segue を作成する、 **Ctrl**キーと別に 1 つのシーンからをクリックしてドラッグします。 ように、マウスをドラッグしたは、次の図に示すように、segue が潜在顧客場所を示す青色コネクタが表示されます。

 [ ![](images/createsegue.png "青色コネクタが表示されたら、この画像に示すように、segue が潜在顧客場所を示す")](images/createsegue.png)

マウス時に、メニューが表示されますので、segue のアクションを選択します。 これは、次の図のようになります可能性があります。 

**前の iOS 8 とサイズ クラス**:

[ ![](images/segue1.png "アクションの話題ドロップダウン サイズ クラスなし")](images/segue1.png)

**クラスのサイズとアダプティブ Segues を使用して**:

[ ![](images/16new.png "サイズのクラスを含むアクションの話題ドロップダウン")](images/16new.png)

> [!IMPORTANT]
> **注:** Ctrl キーを押し、としてマップを使用している VMWare の Windows 仮想マシンの場合、_を右クリックして_既定でマウス ボタンをクリックします。 Segue を作成するには、を通じてキーボード設定を編集**設定** > **キーボードおよびマウス** > **マウス ショートカット**再マップと、**副ボタン**下図のようにします。
> 
> [ ![](images/image22.png "キーボードとマウスの設定")](images/image22.png)
> 
> 通常どおり、ビューのコント ローラー間 segue を追加できるようになりましたにします。

さまざまな種類の切り替え効果、各中断の制御をユーザーに新しいビュー コント ローラーを表示する方法や、ストーリー ボード内の他のビュー コント ローラーとの対話方法があります。 これらについて以下に説明します。 カスタム遷移を実装する segue オブジェクトをサブクラス化することも。

-  **表示/プッシュ**– プッシュ話題ビュー コント ローラーをナビゲーション スタックに追加します。 元のプッシュ ビュー コント ローラーのナビゲーションと同じコント ローラー、スタックに追加されているビュー コント ローラーの一部であると見なします。 同じ動作が`pushViewController`、し、通常、画面上のデータ間にいくつかの関係があるときに使用します。 話題、プッシュを使用して、ナビゲーション バー、[戻る] ボタンとタイトルのスタックで、ドリル ダウン ナビゲーション階層の表示を許可するには、各ビューに追加する余裕を選べます。
-  **モーダル**– モーダル segue が表示されている動画の遷移のオプションを使用して、プロジェクト内の任意の 2 つのビュー コント ローラー間のリレーションシップを作成します。 子ビュー コント ローラーが完全がわかりにくくなる親ビューのコント ローラーに表示されるときにします。 プッシュとは異なり話題を使用します。 [戻る] ボタンを追加します。話題モーダルを使用するときに`DismissViewController`前のビュー コント ローラーを返すために使用する必要があります。
-  **カスタム**– カスタムのサブクラスとして作成できる segue` UIStoryboardSegue`です。
-  **アンワインド**– アンワインド話題プッシュまたはモーダルを後方に移動するために使用できる話題 – たとえば、ビューのモーダルとして表示されるコント ローラーを閉じる。 さらに、1 つだけでなくを介してアンワインドできますが、一連のプッシュとモーダル segues して戻る を 1 つのナビゲーション階層内の複数のステップのアンワインド操作。 使用する方法を理解するアンワインドの話題、ios、読み取り、 [Segues アンワインド作成](https://developer.xamarin.com/recipes/ios/general/storyboard/unwind_segue/)レシピします。
-  **Sourceless** – sourceless segue 初期ビュー コント ローラーを含むシーンを示し、そのためユーザーがどのビューは最初に表示します。 次に示す segue によって表されます。  

    [ ![](images/sourcelesssegue.png "Sourceless segue")](images/sourcelesssegue.png)

### <a name="adaptive-segue-types"></a>アダプティブ話題型

 iOS 8 が導入された[サイズ クラス](~/ios/user-interface/storyboards/unified-storyboards.md#size-classes)iOS ストーリー ボード ファイルをすべての iOS デバイス用の 1 つの UI を作成する開発者を有効にすると、すべての利用可能な画面サイズを使用するようにします。 既定では、すべての新しい Xamarin.iOS アプリケーションはサイズのクラスを使用します。 古いプロジェクトからサイズ クラスを使用するのを参照してください、 [Unified ストーリー ボードの概要](~/ios/user-interface/storyboards/unified-storyboards.md)ガイドです。 
 
サイズのクラスを使用しているアプリケーションは、新しい使用も[*アダプティブ Segues*](~/ios/user-interface/storyboards/unified-storyboards.md)です。 サイズのクラスを使用する場合は、ことはないを直接指定するかどうかを注意してください iPhone または iPad を使用します。 つまりおを常に使用する必要がある不動産量に関係なく、同じを検索する 1 つの UI を作成します。 アダプティブ Segues 作業時間、環境を判断し、コンテンツを表示する最善の方法を決定します。 アダプティブ Segues 次に示します。 

[ ![](images/adaptivesegue.png "アダプティブ Segues ドロップダウン")](images/adaptivesegue.png)

<table>
    <thead>
        <tr>
            <th>話題</th>
            <th>説明</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>表示</td>
            <td>これとよく似ています、プッシュが話題が、アカウントに、画面のコンテンツがかかりません。 </td>
        </tr>
        <tr>
            <td>詳細の表示</td>
            <td>アプリケーションには、(たとえば、iPAd で分割ビュー コント ローラー) のマスター/詳細ビューが表示されて、コンテンツの詳細ビューに置き換わります。 アプリケーションには、マスターのみが表示されている場合<strong>または</strong>詳細、コンテンツ ビューのコント ローラー スタックの一番上に置き換えられます。</td>
        </tr>
        <tr>
            <td>Presentation</td>
            <td>これは、モーダル segue に似ていますしのプレゼンテーションと遷移のスタイルの選択できます。</td>
        </tr>
        <tr>
            <td>重なってプレゼンテーション</td>
            <td>これは、重なってとしてコンテンツが表示します。</td>
        </tr>
    </tbody>
</table>

### <a name="transferring-data-with-segues"></a>Segues を使用してデータを転送します。

Segue の利点は、遷移で終了しません。 また、ビューのコント ローラー間でデータの転送を管理するも使用できます。 これは、オーバーライドすることで実現、`PrepareForSegue`初期ビュー コント ローラー コンピューターと社内でデータを処理するメソッド。 Segue がトリガーされる – たとえば、ボタンを押す – で、アプリケーションが呼び出すこのメソッドは、新しいビュー コント ローラーを準備する機会を提供する*する前に*任意ナビゲーションが発生します。 下のコードから、 [Phoneword](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)サンプルを示します。 


```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, 
NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    var callHistoryContoller = segue.DestinationViewController 
                                  as CallHistoryController;

    if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
}
```

この例では、`PrepareForSegue`メソッドは、ユーザーが、segue がトリガーされたときに呼び出されます。 まず、'受信' ビュー コント ローラーのインスタンスを作成し、この segue の宛先ビュー コント ローラーとして設定する必要があります。 これは、次のコードの行で。

```csharp
var callHistoryContoller = segue.DestinationViewController as CallHistoryController;
```

メソッドがプロパティを設定する機能、`DestinationViewController`です。 この例では移動しましたこれの利点と呼ばれる一覧を渡すことによって`PhoneNumbers`を`CallHistoryController`し、同じ名前のオブジェクトに代入します。

```csharp
if (callHistoryContoller != null) {
        callHistoryContoller.PhoneNumbers = PhoneNumbers;
    }
```

移行が完了すると、ユーザーに表示される、`CallHistoryController`設定済みのリストを使用します。

## <a name="adding-a-storyboard-to-a-non-storyboard-project"></a>ストーリー ボードを非ストーリー ボード プロジェクトに追加します。

場合によっては、以前ストーリー ボード ファイルに、ストーリー ボードを追加する必要があります。 1 回でこれを行う Visual Studio for Mac は、次の手順で合理化できます。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. 参照して新しいストーリー ボード ファイルを作成する**ファイル > 新しいファイル > iOS > ストーリー ボード**下図のように、します。 
    
    [ ![](images/new-storyboard-xs.png "新しいファイル ダイアログ")](images/new-storyboard-xs.png)

2. ストーリー ボード名を追加、 **Main インターフェイス**のセクションで、 **Info.plist**次のように、します。
    
    [ ![](images/infoplist.png "Info.plist エディター")](images/infoplist.png)
    
    これにより、初期ビュー コント ローラーでインスタンス化するのと同じ、`FinishedLaunching`アプリ デリゲート内のメソッドです。 このオプションを設定して、アプリケーション ウィンドウ (下記参照) をインスタンス化、メインのストーリー ボードの読み込みおよびとしてストーリー ボードの初期ビュー Controller (sourceless Segue の横にある 1 つ) のインスタンスを割り当てます、`RootViewController`ウィンドウはのプロパティ画面に表示するウィンドウです。

3. `AppDelegate`、既定の上書き`Window`ウィンドウのプロパティを実装する次のコードに、メソッド。
        
        public override UIWindow Window {
            get;
            set;
            }
            
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. プロジェクトを右クリックして新しいストーリー ボード ファイルを作成**追加 > 新しいファイル > iOS > 空のストーリー ボード**下図のように、します。 
    
    [ ![](images/new-storyboard-vs.png "新しい項目 ダイアログ")](images/new-storyboard-vs.png)

2. ストーリー ボード名を追加、 **Main インターフェイス**iOS アプリケーションを次に示すようのセクション。
    
    [ ![](images/ios-app.png "Info.plist エディター")](images/ios-app.png)
    
    これにより、初期ビュー コント ローラーでインスタンス化するのと同じ、`FinishedLaunching`アプリ デリゲート内のメソッドです。 このオプションを設定して、アプリケーション ウィンドウ (下記参照) をインスタンス化、メインのストーリー ボードの読み込みおよびとしてストーリー ボードの初期ビュー Controller (sourceless Segue の横にある 1 つ) のインスタンスを割り当てます、`RootViewController`ウィンドウはのプロパティ画面に表示するウィンドウです。

3. `AppDelegate`、既定の上書き`Window`ウィンドウのプロパティを実装する次のコードに、メソッド。

        public override UIWindow Window {
            get;
            set;
            }
            
-----

## <a name="creating-a-storyboard-with-the-ios-designer"></a>IOS デザイナーをストーリー ボードを作成します。

ストーリー ボードは、iOS、統合されたシームレスに Visual Studio での Mac と Visual Studio の Xamarin デザイナーを使用して作成できます。

次の作業を開始する iOS デザイナーを使用して、ストーリー ボードを作成する、[こんにちは, iOS マルチ スクリーン](~/ios/get-started/hello-ios-multiscreen/index.md)ガイドです。 このチュートリアルでは、Segues、および、コントロールのイベントを処理する方法を使用してビュー コント ローラー間のナビゲーションを調査します。

## <a name="instantiate-storyboards-manually"></a>ストーリー ボードを手動でインスタンス化します。

ストーリー ボードが、ストーリー ボード内の個々 のビューのコント ローラーがインスタンス化することを使用して完全に、プロジェクト内の個々 の XIB ファイルを置き換える`Storyboard.InstantiateViewController`です。

場合もありますアプリケーションでは、デザイナーによって提供される組み込みのストーリー ボードの遷移を処理することはできません特別な要件があります。 たとえば、でした。 アプリケーションの現在の状態に応じて、同じボタンからさまざまな画面を起動するアプリケーションを作成する場合おコント ローラーの表示を手動でインスタンス化し、社内での移行をプログラムに可能性があります。

次のスクリーン ショットでは、それらの間の 2 つコント ローラーの表示、デザイン サーフェイス上のない話題は示しています。 次のセクションがコードでその遷移設定する方法について説明します。

 [ ![](images/viewcontrollerspink.png "このスクリーン ショットは、それらの間の 2 つコント ローラーの表示、デザイン サーフェイスのない話題を示しています。")](images/viewcontrollerspink.png)

1. 追加、 _iPhone ストーリー ボードを空にする_既存プロジェクトのプロジェクトに。
    
    [ ![](images/add-storyboard1.png "ストーリー ボードを追加します。")](images/add-storyboard1.png)

2. 新しく作成されたストーリー ボードを開くをダブルクリックし、新しい**ナビゲーション コント ローラー**デザイン画面にします。 ナビゲーション コント ローラーは、次のように、ルート ビュー、コント ローラーとは既定では、UI のないです。

    [ ![](images/uinavigationcontroller.png "コント ローラー Segues ビュー")](images/uinavigationcontroller.png)

3. 選択、_ビュー コント ローラー_下部の黒のバーをクリックするとします。 デザイナーの **プロパティ パッド** **Identity**おビュー コント ローラーのカスタム クラスだけでなく、一意の ID を指定できます。 設定、**クラス名**と**ストーリー ボード ID**に`MainViewController`です。

    [ ![](images/identitypanelnew.png "カスタム クラスを指定します。")](images/identitypanelnew.png)

4. ストーリー ボードからコント ローラーの表示のインスタンスを作成する必要がありますが、後で、コード内で参照するストーリー ボード ID が使用されます。 ストーリー ボード ID と一致する復元の ID を設定、状態を復元する必要があるビュー コント ローラーが正しく再作成を取得することを確認します。

5. 現在だけある 1 つのビュー コント ローラー。 別のビュー コント ローラーをデザイン画面にドラッグします。 **プロパティ パッド**、識別情報、クラスとストーリー ボード ID を設定`PinkViewController`下図のように、します。

    [ ![](images/pinkvcnew.png "プロパティのパッド")](images/pinkvcnew.png)
    
    IDE では、コント ローラーの表示をこれらのカスタム クラスを作成します。 表示できるこれら、**ソリューション パッド**次のスクリーン ショットに示すように、します。
    
    [ ![](images/solution-pad.png "ソリューションの埋め込み")](images/solution-pad.png)

6. `PinkViewController`、コント ローラーのフレームの中央に向けてをクリックして、ビューを選択します。 パッドでは、プロパティ、[表示] を変更、**背景**マゼンタに。
    
    [ ![](images/pinkcontroller.png "背景色を設定")](images/pinkcontroller.png)

7. 最後に、ボタンをドラッグして、**ツールボックス**上に、`MainViewController`です。 パッドでは、プロパティ、名前を付けます`PinkButton`とタイトル GoToPink、下図のようにします。

    [ ![](images/pinkbutton.png "ボタンの名前を設定します。")](images/pinkbutton.png)

ストーリー ボードが完了するには、プロジェクトを今すぐ配置して、空の画面が得されます。 Ide、ストーリー ボードを使用して、最初のビューとして使用するルート ビュー コント ローラーを設定する必要があるためにです。 通常この使用できる、プロジェクトのオプションでは、上記のようにします。 次を追加することでコードでは、同じ結果を得ることがこの例ではただし、 **AppDelegate**:

```csharp
public partial class AppDelegate : UIApplicationDelegate
    {
        UIWindow window;
        public static UIStoryboard Storyboard = UIStoryboard.FromName ("MainStoryboard", null);
        public static UIViewController initialViewController;

        public override bool FinishedLaunching (UIApplication app, NSDictionary options)
        {
            window = new UIWindow (UIScreen.MainScreen.Bounds);

            initialViewController = Storyboard.InstantiateInitialViewController () as UIViewController;

            window.RootViewController = initialViewController;
            window.MakeKeyAndVisible ();
            return true;
        }

    }
```

多くのコードであるが、いくつかの行のみに習熟していません。 最初に、登録して、ストーリー ボード、 **AppDelegate**ストーリー ボードの名前を渡すことによって**MainStoryboard**です。 呼び出して、ストーリー ボードから、最初のビューのコント ローラーのインスタンスを作成するアプリケーションを次に、お知らせ`InstantiateInitialViewController`ストーリー ボードし、そのビュー コント ローラー アプリケーションのルート ビュー コント ローラーとして設定します。 このメソッドは、ユーザーに表示される、最初の画面を決定し、そのビュー コント ローラーの新しいインスタンスを作成します。

IDE が作成されたソリューション ウィンドウで通知、`MainViewcontroller.cs`クラス、およびその`corresponding designer.cs`手順 4. で、プロパティ パッドにクラス名を追加しました。 基本クラスを含む特殊なコンス トラクターを作成したこのクラスが分かります。

```csharp
public MainViewController (IntPtr handle) : base (handle) 
{
}
```


IDE が自動的に追加されているデザイナーを使用してストーリー ボードを作成するときに、 [[登録]](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/)の上部にある属性、`designer.cs`クラスとで指定されたストーリー ボード ID と同じである文字列識別子に渡す、前の手順です。 これは、c# にリンク、ストーリー ボードに関連するシーンです。

あった既存のクラスを追加することができます、ある時点で**いない**デザイナーで作成します。 この場合、通常の方法でこのクラスを登録するは。

```csharp
[Register ("MainViewController")]
public partial class MainViewController : UIViewController
{
public MainViewController (IntPtr handle) : base (handle) 
{
}

...
}
```

クラスとメソッドを登録する方法の詳細についてを参照してください、[型レジストラー](http://docs.xamarin.com/guides/ios/advanced_topics/registrar/)ドキュメント。

このクラスで最後の手順で、ネットワーク上で、ボタンとピンク ビュー コント ローラーへの移行をします。 インスタンス化、`PinkViewController`ストーリー ボードです。 次に、おはプログラムのプッシュの話題`PushViewController`次のコード例で示すように。

```csharp
public partial class MainViewController : UIViewController
{
    UIViewController pinkViewController;

    public MainViewController (IntPtr handle) : base (handle)
    {

    }

    public override void AwakeFromNib ()
    {
    // Called when loaded from xib or storyboard.

    this.Initialize ();
    }

    public void Initialize(){

    //Instantiating View Controller with Storyboard ID 'PinkViewController'
    pinkViewController = Storyboard.InstantiateViewController ("PinkViewController") as PinkViewController;
    }

    public override void ViewDidLoad ()
    {
    base.ViewDidLoad ();

    //When we push the button, we will push the pinkViewController onto our current Navigation Stack
    PinkButton.TouchUpInside += (o, e) =&gt; {
        this.NavigationController.PushViewController (pinkViewController, true);
    };
    }

}
```

アプリケーションを実行するには、2 画面アプリケーションが生成されます。

![](images/finishedstoryboard.png "画面の実行のサンプル アプリ")

## <a name="conditional-segues"></a>条件付き Segues します。

多くの場合、1 つのビュー コント ローラーから、[次へ] への移動は、特定の条件によって異なります。 たとえば、簡単なログイン画面を作成おされた場合はのみが必要に次の画面に移動する*場合*ユーザー名とパスワードが検証されていました。

次の例ではパスワード フィールド上のサンプルを追加します。 ユーザーはのみにアクセスできる、 *PinkViewController*場合、正しいパスワードを入力すると、それ以外の場合、エラーが表示されます。

始める前に、1 ~ 8 の上記の手順に従います。 次の手順での ストーリー ボードを作成、開始、UI を作成し、アプリのデリゲートを使用するどのビュー コント ローラーを伝えてください。 お RootViewController であるとします。

1. ここで、UI を構築しに記載されているその他のビューを追加してみましょう、`MainViewController`次のスクリーン ショットでこのような外観にすること。

    - UITextField
        - Name: PasswordTextField
        - シークレットのパスワードを入力してください ' をプレース ホルダー。
    - UILabel
        - テキスト: ' エラー: パスワードが間違っています。 いてはいけない渡さないでください '。
        - 色: 赤
        - 配置: センター
        - 行: 2
        - 'Hidden' のチェック ボックスがオン 
        
    [ ![](images/passwordvc.png "Center 行")](images/passwordvc.png)
    
2. ピンクへ移動 ボタンと Ctrl - からドラッグしてビュー コント ローラー間 Segue を作成、 *PinkButton*を*PinkViewController*を選択して**プッシュ**マウス時に. 

3. Segue をクリックし、*識別子* `SegueToPink`:

    [ ![](images/namesegue.png "Segue をクリックし、識別子 SegueToPink を付けます")](images/namesegue.png)  
    

4. 最後には、次の ShouldPerformSegue メソッドを追加、`MainViewController`クラス。

    ```csharp
    public override bool ShouldPerformSegue (string segueIdentifier, NSObject sender)
    {
        
        if(segueIdentifier == "SegueToPink"){
            if (PasswordTextField.Text == "password") {
                PasswordTextField.ResignFirstResponder ();
                return true;
            }
            else{
                ErrorLabel.Hidden = false;
                return false;
            }
        }
        return base.ShouldPerformSegue (segueIdentifier, sender);
    }
    ```

このコードに segueIdentifier を指定した、`SegueToPink`話題、; 条件をテストし、ここでは有効なパスワード。 場合は、条件の戻り値`true`、Segue が実行され、が提示されます、`PinkViewController`です。 場合`false`、新しいビュー コント ローラーは表示されません。

ShouldPerformSegue メソッドへの segueIdentifier 引数をチェックして、このアプローチをこのビューのコント ローラー上の任意の Segue お適用できます。 ここでのみがある 1 つの Segue 識別子 –`SegueToPink`です。

Storyboards.Conditional ソリューションを参照してください、[手動ストーリー ボード サンプル](https://developer.xamarin.com/samples/monotouch/ManualStoryboard/)作業例についてはします。

<a name="Using-Storyboard-References" />

## <a name="using-storyboard-references"></a>ストーリー ボードの参照の使用

ストーリー ボードの参照では、大規模で複雑なストーリー ボードのデザインおよび、元の参照を取得するより小さなストーリー ボードに分割することができます、したがって削除複雑さを削除して、その結果を個別に行う ストーリー ボード簡単にデザインして維持します。

また、ストーリー ボードの参照を提供できます、_アンカー_同じストーリー ボードまたは別の特定のシーン内の別のシーンにします。

<a name="Referencing-an-External-Storyboard" />

### <a name="referencing-an-external-storyboard"></a>外部のストーリー ボードを参照します。

外部のストーリー ボードへの参照を追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、プロジェクト名を右クリックし **追加** > **新しいファイル.**  >  **iOS** > **ストーリー ボード**です。 入力、**名前**新しいストーリー ボードとをクリックして、**新規**ボタン。
    
    [ ![](images/ref01.png "新しいファイル ダイアログ")](images/ref01.png)
    
2. 操作と同じよう通常、変更を保存は、新しいストーリー ボードのシーンのレイアウトをデザインします。 
    
    [ ![](images/ref02.png "新しいシーンのレイアウト")](images/ref02.png)
    
3. Ios デザイナーへの参照を追加しようとするストーリー ボードを開きます。

4. ドラッグ、**参照をストーリー ボード**から、**ツールボックス**デザイン サーフェイスに。 
    
    [ ![](images/ref03.png "ストーリー ボードの参照")](images/ref03.png)
    
5. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**の名前を選択、**ストーリー ボード**上に作成します。 

    [ ![](images/ref04.png "ウィジェット タブ")](images/ref04.png)
    
6. 既存のシーンで (ボタン) のような UI ウィジェットのコントロールをクリックしを新しい Segue を作成、**ストーリー ボード参照**先ほど作成しました。 

    [ ![](images/ref05.png "Segue を作成します。")](images/ref05.png) 
    
7. ポップアップ メニューから選択**表示**Segue を完了します。 

    [ ![](images/ref06.png "Segue を完了するの表示 を選択")](images/ref06.png) 
    
8. ストーリー ボードに変更を保存します。

アプリを実行し、ストーリー ボードの参照で指定された外部のストーリー ボードから初期ビュー コント ローラーから、Segue を作成した UI 要素にユーザーがクリックしたときに表示されます。

<a name="Referencing-a-Specific-Scene-in-an-External-Storyboard" />

### <a name="referencing-a-specific-scene-in-an-external-storyboard"></a>外部のストーリー ボード内の特定のシーンを参照します。

特定のシーンへの参照を追加するには、外部のストーリー ボード (および初期のビュー コント ローラー以外)、次を操作します。

1. **ソリューション エクスプ ローラー**、外部のストーリー ボード ファイルを開いて編集する をダブルクリックします。

2. 新しいシーンを追加し、通常どおりにそのレイアウトをデザインします。 

    [ ![](images/ref07.png "新しいシーン レイアウト")](images/ref07.png)
    
3. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**、入力、**ストーリー ボード ID**新しいシーンのビューのコント ローラーの。 

    [ ![](images/ref08.png "新しいシーン ビュー コント ローラーのストーリー ボード ID を入力します。")](images/ref08.png)
    
3. Ios デザイナーへの参照を追加しようとするストーリー ボードを開きます。

4. ドラッグ、**参照をストーリー ボード**から、**ツールボックス**デザイン サーフェイスに。 

    [ ![](images/ref03.png "ストーリー ボードの参照")](images/ref03.png)
    
5. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**の名前を選択、**ストーリー ボード**と**参照 ID** (ストーリー ボード ID) の上記で作成したシーン: 

    [ ![](images/ref09.png "ウィジェット タブ ")](images/ref09.png)
    
6. 既存のシーンで (ボタン) のような UI ウィジェットのコントロールをクリックしを新しい Segue を作成、**ストーリー ボード参照**先ほど作成しました。 

    [ ![](images/ref10.png "Segue を作成します。")](images/ref10.png) 
    
7. ポップアップ メニューから選択**表示**Segue を完了します。 

    [ ![](images/ref06.png "Segue を完了するの表示 を選択")](images/ref06.png) 
    
8. ストーリー ボードに変更を保存します。

UI 要素を作成された、Segue のシーンをアプリが実行し、ユーザーがクリックした、指定された**ストーリー ボード ID**からストーリー ボードの参照で指定された外部のストーリー ボードが表示されます。

<a name="Referencing-a-Specific-Scene-in-the-Same-Storyboard" />

### <a name="referencing-a-specific-scene-in-the-same-storyboard"></a>同じストーリー ボード内の特定のシーンを参照します。

特定のシーン同じストーリー ボードへの参照を追加するには、次の操作を行います。

1. **ソリューション エクスプ ローラー**、ストーリー ボード ファイルを開いて編集する をダブルクリックします。

2. 新しいシーンを追加し、通常どおりにそのレイアウトをデザインします。 

    [ ![](images/ref11.png "新しいシーン レイアウト")](images/ref11.png)

3. **ウィジェット**のタブ、**プロパティ エクスプ ローラー**、入力、**ストーリー ボード ID**新しいシーンのビューのコント ローラーの。 

    [ ![](images/ref12.png "ウィジェット タブ")](images/ref12.png)
    
3. ドラッグ、**参照をストーリー ボード**から、**ツールボックス**デザイン サーフェイスに。 

    [ ![](images/ref03.png "ストーリー ボードの参照")](images/ref03.png)
    
5. **ウィジェット**のタブ、**プロパティ エクスプ ローラー****参照 ID** (ストーリー ボード ID) 上で作成した、シーンの。 

    [ ![](images/ref13.png "ウィジェット タブ")](images/ref13.png)
    
6. 既存のシーンで (ボタン) のような UI ウィジェットのコントロールをクリックしを新しい Segue を作成、**ストーリー ボード参照**先ほど作成しました。 

    [ ![](images/ref14.png "Segue を作成します。")](images/ref14.png) 
    
7. ポップアップ メニューから選択**表示**Segue を完了します。 

    [ ![](images/ref06.png "Segue を完了するの表示 を選択")](images/ref06.png) 
    
8. ストーリー ボードに変更を保存します。

UI 要素を作成された、Segue のシーンをアプリが実行し、ユーザーがクリックした、指定された**ストーリー ボード ID**ストーリー ボードの参照で指定された同じストーリー ボードが表示されます。

## <a name="summary"></a>まとめ

この記事では、ストーリー ボードとどのように可能性がある iOS アプリケーションの開発では有用の概念を紹介します。 これは、シーン、コント ローラーの表示、ビューとビュー階層、およびさまざまな種類の Segues と共にシーンをリンクする方法について説明します。  についても検討コント ローラーの表示がインスタンス化する手動でストーリー ボード、および作成の条件付き Segues からです。



## <a name="related-links"></a>関連リンク

- [手動のストーリー ボード (サンプル)](https://developer.xamarin.com/samples/ManualStoryboard/)
- [IOS デザイナーの概要](~/ios/user-interface/designer/introduction.md)
- [ストーリー ボードへの変換](http://developer.apple.com/library/ios/#releasenotes/Miscellaneous/RN-AdoptingStoryboards/)
- [UIStoryboard クラスのリファレンス](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIStoryboard_Class/Reference/Reference.html)
