---
title: MonoTouch.Dialog の概要
description: MonoTouch.Dialog (mt.D) toolkit は、アプリケーションの迅速な Xamarin.iOS で UI の開発の不可欠なフレームワークです。 MT.D は、高速で簡単に複雑なアプリケーションのコント ローラーのナビゲーションやテーブルなどの面倒ではなく、宣言型の方法を使用して UI を定義します。さらに、山D では、完全な制御または自動化されたアプローチでは、だけでなく更新するプル、背景画像の読み込みなどの追加機能を開発者に提供、サポート、および JSON データを使用して動的な UI の生成を検索する Api の柔軟なセットがあります。 このガイドには山を使用するさまざまな方法が導入されていますD、高度な使用を深く電子です。
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: be979b35ffdd597dae74f1f661a381ae44433b10
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-monotouchdialog"></a>MonoTouch.Dialog の概要

_MonoTouch.Dialog (mt.D) toolkit は、アプリケーションの迅速な Xamarin.iOS で UI の開発の不可欠なフレームワークです。MT.D は、高速で簡単に複雑なアプリケーションのコント ローラーのナビゲーションやテーブルなどの面倒ではなく、宣言型の方法を使用して UI を定義します。さらに、山D では、完全な制御または自動化されたアプローチでは、だけでなく更新するプル、背景画像の読み込みなどの追加機能を開発者に提供、サポート、および JSON データを使用して動的な UI の生成を検索する Api の柔軟なセットがあります。このガイドには山を使用するさまざまな方法が導入されていますD、高度な使用を深く電子です。_


山と呼ばれる MonoTouch.Dialog短い、D はアプリケーション画面およびコント ローラーの表示、テーブルなどを作成する面倒ではなく、情報を使用してナビゲーションを構築する開発者ができる、迅速な開発の UI ツールキットです。そのため、UI の開発およびコードの削減の大幅な簡略化バージョンを提供します。 たとえば、次のスクリーン ショットがあるとします。

 [![](images/image1.png "たとえば、このスクリーン ショット")](images/image1.png#lightbox)

次のコードは、この画面全体の定義に使用されました。

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
    [Caption("Category")]
    public Category ExpenseCategory;
}
```

IOS でのテーブルを使用する場合がある多くの場合、さまざまな繰り返し実行するコードです。
たとえば、テーブルは、必要なたびにそのテーブルを設定するデータ ソースが必要です。 ナビゲーションのコント ローラーを使用して接続されている 2 つのテーブル ベースの画面が含まれるアプリケーションでは、各画面は、同じコードの多くを共有します。

MT.D によってテーブルの作成用の汎用的な API にカプセル化するすべてのコードを簡略化されます。 宣言型のオブジェクトでも見やすくするための構文をバインディングでは、その API の上に抽象化を用意します。 そのため、2 つの Api では使用 mt. です。D:

-   **低レベルの要素 API** –*要素 API*画面とそのコンポーネントを表す要素の階層ツリーの作成に基づきます。 最大限の柔軟性と制御の Ui を作成する開発者は要素の API を使用します。 さらに、要素 API では、サーバーから動的 UI 生成だけでなく、非常に高速の宣言は、JSON を使用して宣言型の定義のサポートに進んでいます。 
-   **高度なリフレクション API** – とも呼ばれる、*バインド**API*クラスに注釈を付けると、UI のヒント、山でD は自動的にオブジェクトに基づく画面を作成して、基になるオブジェクトのバックアップと新機能の間のバインドを表示 (および必要に応じて編集) 画面で、提供します。 上記の例では、リフレクション API の使用を示します。 この API は、詳細に API 要素は、制御を提供しませんが、クラスの属性に基づいて要素の階層を自動的に作成して、さらに複雑さが軽減こと。 


MT.D は、画面を作成するための UI 要素に組み込まれている大きなセットでパックされたが、必要なカスタマイズされた要素および高度な画面レイアウトとも認識します。 そのため、機能拡張では、最上位機能を備えた API に手作りです。 開発者は、既存の要素を拡張または新たに作成してシームレスに統合し、ことができます。

さらに、山D では、「プル更新」サポート、非同期のイメージを読み込み、およびサポートの検索構築された iOS UX 機能は共通の数があります。

この記事は包括的なを見て山の操作D など。

-   **MT.D コンポーネント**– この山を構成するクラスを理解することに焦点を当てますD 速度をすばやく取得を有効にします。 
-   **要素のリファレンス**– 機械翻訳の組み込み要素の包括的な一覧D. 
-   **使用状況を高度な**– この高度な機能と更新するプル、検索、背景イメージの読み込み、要素の階層を構築する LINQ を使用して作成など、セルのカスタム要素して山でのコント ローラーを使用してD. 

## <a name="understanding-the-pieces-of-mtd"></a>機械翻訳の部分を理解します。D

リフレクション API、山を使用する場合でもD は、直接要素 API を使用して作成された場合と同様に、内部で要素の階層を作成します。 また、前のセクションに記載されている JSON サポートは要素をも作成されます。 このためは機械翻訳の構成要素の基本的な知識を理解しておくことD.

MT.D は、次の 4 つの部分を使用して画面を作成します。

-  **DialogViewController**
-  **RootElement**
-  **セクション**
-  **要素**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*、または*DVC*から継承を短い、`UITableViewController`そのため、テーブルと画面を表します。 DVCs は、正規 UITableViewController と同じようにナビゲーション コント ローラーにプッシュされることができます。

### <a name="rootelement"></a>RootElement

A *RootElement* DVC に移動するアイテムの最上位のコンテナーです。 要素を含めることができますしのセクションが含まれています。 RootElements はレンダリングされません。代わりにどのような実際に表示される取得用のコンテナーだけであります。 RootElement が、DVC に割り当てられ、DVC が後、その子を描画します。

### <a name="section"></a>セクション

セクションは、テーブル内のセルのグループです。 通常のテーブル セクションで、オプションで持つことができます、ヘッダーとフッターするかをテキスト、または次のスクリーン ショットのように、カスタム ビューをします。

 [![](images/image2.png "ヘッダーとフッターするかをするテキスト、またはこのスクリーン ショットのように、カスタム ビューで通常のテーブル セクションで、オプションで持つことができます、")](images/image2.png#lightbox)

### <a name="element"></a>要素

要素は、テーブル内の実際のセルを表します。 MT.D は、さまざまな異なるデータ型または異なる入力を表す要素にパックされたものです。 たとえば、次のスクリーン ショットでは、利用可能な要素の一部を示しています。

 [![](images/image3.png "たとえば、このスクリーン ショットが利用可能な要素の一部を示しています")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>セクションと RootElements on

今すぐ説明 RootElements およびセクションで詳しくします。

### <a name="rootelements"></a>RootElements

MonoTouch.Dialog プロセスを開始するには、少なくとも 1 つ RootElement が必要です。

セクションまたは要素値を持つ、RootElement が初期化された場合、この値は、子は、画面の右側に表示されると、構成の概要を提供する要素を検索する使用されます。 たとえば、次のスクリーン ショットには、右側の「デザート」選択した desert の値と一緒に詳細画面のタイトルを含むセルが左側のテーブルを示します。

 [![](images/image4.png "このスクリーン ショットには、右側のデザート、選択した desert の値と一緒に詳細画面のタイトルを含むセルが左側のテーブルを示しています")](images/image4.png#lightbox) [ ![ ](images/image5.png "これは、次のスクリーン ショットには、右側のデザート、選択した desert の値と一緒に詳細画面のタイトルを含むセルが左側のテーブルを示しています")](images/image5.png#lightbox)

ルート要素こともできます内のセクションでは、新しい入れ子になった構成ページの読み込みをトリガーする上記のようにします。 このモードで使用されている場合、指定されたキャプションはセクション内にレンダリングされるときに使用し、サブページのタイトルとしても使用します。 例えば:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner"){
            new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
                new Section () {
                    new RadioElement ("Ice Cream", "dessert"),
                    new RadioElement ("Milkshake", "dessert"),
                    new RadioElement ("Chocolate Cake", "dessert")
                }
            }
        }
    }
```

上記の例では、「デザート」で、ユーザーがタップしたときに MonoTouch.Dialog は新しいページを作成して「デザート」されていると、3 つの値のオプション グループのルートに移動します。

このサンプルで値「2」、RadioGroup に渡されたために、オプション グループは「デザート」セクションでは「チョコレート ケーキ」を選択します。 つまり、リスト (0 インデックス) の 3 番目の項目を選択します。

Add メソッドの呼び出しまたは c# 4 初期化子構文を使用してセクションを追加します。
アニメーションがセクションを挿入する Insert メソッドが提供されます。

(、RadioGroup) ではなく、グループ インスタンスの RootElement を作成する場合、RootElement セクションに表示される場合の集計値が BooleanElements と CheckboxElements Group.Key 値として同じキーを持つすべての累積数になります。

### <a name="sections"></a>セクション

RootElement の唯一の有効な直接の子と、画面に要素をグループ化のセクションでが使用されます。 セクションでは、新しい RootElements を含め、標準的な要素のいずれかを含めることができます。

セクションに埋め込まれている RootElements は、新しい深いレベルに移動に使用されます。

セクションには、文字列、または UIViews としてヘッダーとページ フッターを持つことができます。
通常は同じ文字列を使用するが、カスタム Ui 作成は、ヘッダーまたはフッターとして任意 UIView を使用できます。 文字列は、次のように作成するのにか、使用できます。

```csharp
var section = new Section ("Header", "Footer")
```

ビューを使用するのにだけ、ビューをコンス トラクターに渡します。

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>通知の取得

#### <a name="handling-nsaction"></a>NSAction の処理

MT.D サーフェス、`NSAction`コールバックを処理するためのデリゲートとします。
たとえば、テーブル セルの山によって作成された、タッチ イベントを処理します。D. 山で要素を作成する場合D、コールバックを指定するだけに、次のように機能します。

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>要素の値を取得します。

組み合わせて、`Element.Value`プロパティ、コールバックはその他の要素に設定された値を取得できます。 次に例を示します。

```csharp
var element = new EntryElement (task.Name, "Enter task description",
        task.Description);
                
var taskElement = new RootElement (task.Name){
        new Section () { element },
        new Section () { 
                new DateElement ("Due Date", task.DueDate)
        },
        new Section ("Demo Retrieving Element Value") {
                new StringElement ("Output Task Description", 
                        delegate { Console.WriteLine (element.Value); })
        }
};
```

このコードは、次に示すように、UI を作成します。 この例の完全なチュートリアルについては、次を参照してください。、[要素 API チュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)チュートリアルです。

 [![](images/image6.png "Element.Value プロパティと組み合わせると、コールバックはその他の要素に設定された値を取得できます。")](images/image6.png#lightbox)

ユーザーは、表の下のセルを押すと、匿名関数のコードが実行されるから値を書き込み、`element`インスタンスを**アプリケーション出力**Visual Studio for mac のパッド

## <a name="built-in-elements"></a>組み込み要素

MT.D、要素と呼ばれる、組み込みのテーブル セル アイテムの数が付属します。
これらの要素は、文字列、浮動小数点数、日付などもイメージは、いくつかの名前を付けるテーブル セルにさまざまな種類を表示に使用されます。 データ型を適切に表示するの各要素が行われます。 たとえば、ブール型の要素をその値を切り替えるスイッチが表示されます。 同様に、浮動小数点数の要素を float 型の値を変更するスライダーが表示されます。

画像や html などの豊富なデータ型をサポートするために、さらに複雑な要素があります。 たとえばを選択すると、web ページを読み込む UIWebView が開き、html 要素は、表のセルにキャプションを表示します。

### <a name="working-with-element-values"></a>要素の値の操作

ユーザー入力をキャプチャするために使用する要素を公開パブリック`Value`いつでも、要素の現在の値を保持するプロパティです。 ユーザーはアプリケーションを使用するように自動的に更新されます。

これは MonoTouch.Dialog の一部である要素のすべての動作ですが、ユーザーが作成した要素の必須ではありません。

### <a name="string-element"></a>文字列の要素

A`StringElement`左側にある表のセルとセルの右側にある文字列値のキャプションを表示します。

 [![](images/image7.png "StringElement 左側にある表のセルとセルの右側にある文字列値のキャプションを表示します。")](images/image7.png#lightbox)

使用する、`StringElement`ボタンとデリゲートを指定します。

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "使用するには、StringElement ボタンとしてデリゲートを指定します")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>スタイル設定された文字列の要素

A`StyledStringElement`により、いずれかの組み込みのテーブル セルのスタイルを使用して表示する文字列またはカスタムの書式設定します。

 [![](images/image9.png "StyledStringElement により、いずれかの組み込みのテーブル セルのスタイルを使用して表示する文字列またはカスタムの書式設定")](images/image9.png#lightbox)

`StyledStringElement`クラスから派生`StringElement`、ことができますが、開発者は、いくつかのテキストの色、背景セル色、行区切りモード、行数を表示するには、フォントなどのプロパティをカスタマイズおよびアクセサリを表示するかどうか。

### <a name="multiline-element"></a>複数行の要素

 [![](images/image10.png "複数行の要素")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry 要素

`EntryElement`名前と意味、ユーザー入力を取得するために使用します。 通常の文字列または文字を非表示、パスワードのいずれかをサポートします。

 [![](images/image11.png "ユーザー入力を取得する、EntryElement が使用されます。")](images/image11.png#lightbox)

3 つの値で初期化されます。

-  ユーザーに表示されるエントリのキャプションです。
-  (これは、ユーザーにヒントを提供する淡色テキスト) プレース ホルダー テキスト。 
-  テキストの値です。


プレース ホルダーと値を null にすることができます。 ただし、キャプションが必要です。

任意の時点では、その値プロパティにアクセスすることができますの値を取得、`EntryElement`です。

さらに、`KeyboardType`データ エントリの必要なキーボード型スタイルの作成時にプロパティを設定することができます。 値を使用してキーボードを構成できます`UIKeyboardType`以下の一覧にします。

-  数字
-  電話番号
-  URL
-  電子メール


### <a name="boolean-element"></a>ブール型要素

 [![](images/image12.png "ブール型要素")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>チェック ボックス要素

 [![](images/image13.png "チェック ボックス要素")](images/image13.png#lightbox)

### <a name="radio-element"></a>ラジオ要素

A`RadioElement`が必要です、`RadioGroup`で指定する、`RootElement`です。

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "RadioElement、RootElement で指定する RadioGroup が必要です。")](images/image14.png#lightbox)

 `RootElements` ラジオ要素の調整に使用されます。 `RadioElement`メンバーが複数のセクションでは (たとえばリング トーン セレクターとカスタムの個別の着信音に着信音にシステムからのようなものを導入するなど) にまたがることができます。 概要ビューは、現在選択されているオプションの要素に表示されます。 これを使用する、作成、`RootElement`グループのコンス トラクターで、次のようにします。

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

内のグループの名前`RadioGroup`(存在する場合) を含むページで選択した値と値のゼロをここでは、これが最初に選択した項目のインデックスを表示するために使用します。

### <a name="badge-element"></a>バッジ要素

 [![](images/image15.png "バッジ要素")](images/image15.png#lightbox)

### <a name="float-element"></a>浮動小数点数の要素

 [![](images/image16.png "浮動小数点数の要素")](images/image16.png#lightbox)

### <a name="activity-element"></a>アクティビティ要素

 [![](images/image17.png "アクティビティ要素")](images/image17.png#lightbox)

### <a name="date-element"></a>日付要素

 ![](images/image18.png "日付要素")

DateElement に対応するセルを選択すると、次に示すように日付選択カレンダーが表示されます。

 [![](images/image19.png "ように、DateElement に対応するセルを選択すると、日付選択カレンダーが表示されます。")](images/image19.png#lightbox)

### <a name="time-element"></a>Time 要素

 [![](images/image20.png "Time 要素")](images/image20.png#lightbox)

TimeElement に対応するセルを選択すると、次に示すように時刻の選択が表示されます。

 [![](images/image21.png "ように、TimeElement に対応するセルを選択すると、時刻の選択が表示されます。")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime 要素

 [![](images/image22.png "DateTime 要素")](images/image22.png#lightbox)

DateTimeElement に対応するセルを選択すると、次に示すように datetime ピッカーが表示されます。

 [![](images/image23.png "ように datetime ピッカーを表示、DateTimeElement に対応するセルを選択すると、")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML 要素

 [![](images/image24.png "HTML 要素")](images/image24.png#lightbox)

`HTMLElement`の値を表示、`Caption`テーブル セル内のプロパティです。 選択すると、Whe、`Url`要素に割り当てられているに読み込まれる、`UIWebView`次に示すように制御します。

 [![](images/image25.png "Whe が選択されている場合、次のように UIWebView コントロール内の要素に割り当てられている Url が読み込まれる")](images/image25.png#lightbox)

### <a name="message-element"></a>メッセージ要素

 [![](images/image26.png "メッセージ要素")](images/image26.png#lightbox)

### <a name="load-more-element"></a>複数の要素を読み込む

一覧で、複数の項目を読み込むようにするのにには、この要素を使用します。 通常の読み込みのキャプションとフォントとテキストの色をカスタマイズすることができます。
`UIActivity`インジケーターの開始をアニメーション化して、セルをタップすると、読み込みキャプションが表示され、`NSAction`に渡されるコンス トラクターを実行します。 1 回のコードに、`NSAction`が終了し、`UIActivity`インジケーターは、アニメーションを停止し、標準のキャプションが再び表示されます。

### <a name="uiview-element"></a>UIView 要素

さらに、任意のカスタム`UIView`を使用して表示できます、`UIViewElement`です。

### <a name="owner-drawn-element"></a>オーナー描画要素

抽象クラスは、この要素をサブクラス化する必要があります。 オーバーライドする必要があります、`Height(RectangleF bounds)`メソッドでは、要素の高さを返す必要がありますだけでなく`Draw(RectangleF bounds, CGContext context, UIView view)`でを行う必要があります、指定された境界内のすべてのカスタマイズされた描画されたコンテキストとビューのパラメーターを使用します。 この要素はたいへんなサブクラス化、 `UIView`、返されるセルに配置することのみを 2 つの単純なオーバーライドを実装する必要があること。 内のサンプル アプリに優れた実装のサンプルを表示できます、`DemoOwnerDrawnElement.cs`ファイル。

クラスの実装の非常に単純な例を次に示します。

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
 {
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text
    {
        get;set;    
    }

    public override void Draw (RectangleF bounds, CGContext context, UIView view)
    {
        UIColor.White.SetFill();
        context.FillRect(bounds);

        UIColor.Black.SetColor();   
        view.DrawString(this.Text, new RectangleF(10, 15, bounds.Width - 20, bounds.Height - 30), UIFont.BoldSystemFontOfSize(14.0f), UILineBreakMode.TailTruncation);
    }

    public override float Height (RectangleF bounds)
    {
        return 44.0f;
    }
 }
```

### <a name="json-element"></a>JSON 要素

`JsonElement`のサブクラスは、`RootElement`自身を拡張する、`RootElement`ローカルまたはリモートの url から入れ子になった子のコンテンツを読み込むことができます。

`JsonElement`は、 `RootElement` 2 つの形式でインスタンス化することができます。 1 つのバージョンを作成、`RootElement`要求時にコンテンツを読み込むことができます。 これらを使用して作成されて、`JsonElement`からコンテンツを読み込む url を末尾に余分な引数を受け取るコンス トラクター。

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

他のフォームからローカル ファイルまたは既存のデータを作成する`System.Json.JsonObject`を解析します。

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

山で JSON の使用に関する詳細についてD を参照してください、 [JSON 要素チュートリアル](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough)チュートリアルです。

## <a name="other-features"></a>その他の機能

### <a name="pull-to-refresh-support"></a>更新するプル サポート

 *プル-を-* *更新*視覚効果を最初にある、 *Tweetie2*アプリで、多数のアプリケーションで一般的な効果になりました。

に、ダイアログ ボックスを自動的に更新するプル サポートを追加するだけで済みますを 2 つの処理を行うには: ユーザーがデータを取り出すときに通知するイベント ハンドラーをフックし、通知、`DialogViewController`既定の状態に戻るデータが読み込まれるときにします。

通知フックが簡単です。接続のみ、`RefreshRequested`でイベントを`DialogViewController`、次のようにします。

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

その後、メソッド  `OnUserRequestedRefresh`、キューにいくつかのデータの読み込み、ネットから一部のデータを要求またはデータを計算するスレッドをスピンはします。 データが読み込まれるに通知する必要があります、`DialogViewController`に、新しいデータがあり、ビューを既定の状態に復元するにこれを行うを呼び出している`ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>検索のサポート

検索をサポートする設定、`EnableSearch`プロパティを`DialogViewController`です。 設定することも、`SearchPlaceholder`検索バーでのウォーターマーク テキストとして使用するプロパティです。

検索、ユーザーの種類と、ビューの内容も変更されます。 表示フィールドを検索し、ユーザーに示します。 `DialogViewController`プログラムを開始、終了、または結果に新しいフィルター操作をトリガーする 3 つのメソッドを公開します。 これらのメソッドは、次に示します。

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


システムは、拡張可能な場合は、この動作を変更することができます。

### <a name="background-image-loading"></a>背景イメージの読み込み

MonoTouch.Dialog が組み込まれており、 [TweetStation](https://github.com/migueldeicaza/TweetStation)アプリケーションのイメージ ローダー。 このイメージ ローダーでは、バック グラウンドでキャッシュをサポートしているイメージを読み込むために使用して、イメージが読み込まれたときに、コードを通知することができます。

出力方向のネットワーク接続の数が制限されます。

イメージ ローダーでの実装、`ImageLoader`クラスを行うだけで済みますが、呼び出し、`DefaultRequestImage`メソッドのインスタンスだけでなく、負荷を対象とするイメージの Uri を提供する必要が、`IImageUpdated`インターフェイスとなるときに呼び出されましたイメージ ha読み込まれています。

たとえば、次のコードに Url からイメージを読み込みます、 `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

ImageLoader クラスでは、すべてのメモリに現在キャッシュされているイメージを解放するときに呼び出すことのできる Purge メソッドを公開します。 現在のコードでは、50 個のイメージのキャッシュを持っています。 場合 (たとえば、イメージが大きい 50 個のイメージであるが多すぎるために必要とする) 場合は、別のキャッシュ サイズを使用する場合、だけ ImageLoader のインスタンスを作成し、キャッシュに保持するイメージの数を渡すことができます。

## <a name="using-linq-to-create-element-hierarchy"></a>LINQ を使用して要素の階層を作成するには

LINQ と # の初期化の構文の巧妙な使用法を使用して LINQ を使用して、要素の階層を作成します。 たとえば、次のコードは、一部の文字列配列から画面を作成し、ハンドルはそれぞれに渡される匿名関数を使用して選択範囲のセルの`StringElement`:

```csharp
var rootElement = new RootElement ("LINQ root element") {
from x in new string [] { "one", "two", "three" }
select new Section (x) {
from y in "Hello:World".Split (':')
select (Element) new StringElement (y,
delegate { Debug.WriteLine("cell tapped"); })
}
};
```

これは、XML データ ストアまたはほぼ完全データから複雑なアプリケーションを作成するデータベースからデータと簡単に組み合わせる可能性があります。

## <a name="extending-mtd"></a>山を拡張します。D

### <a name="creating-custom-elements"></a>カスタム要素を作成します。

既存の要素から継承することで、または要素のルート クラスから派生することで、独自の要素を作成できます。

独自の要素を作成するには、次のメソッドをオーバーライドするします。

```csharp
// To release any heavy resources that you might have
    void Dispose (bool disposing);

    // To retrieve the UITableViewCell for your element
    // you would need to prepare the cell to be reused, in the
    // same way that UITableView expects reusable cells to work
    UITableViewCell GetCell (UITableView tv)

    // To retrieve a "summary" that can be used with
    // a root element to render a summary one level up.  
    string Summary ()
    // To detect when the user has tapped on the cell
    void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path)
    // If you support search, to probe if the cell matches the user input
    bool Matches (string text)
```

実装する必要がある場合は、この要素は、可変サイズであることができます、`IElementSizing`インターフェイスで、1 つのメソッドが含まれています。

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

実装を計画している場合、`GetCell`メソッドを呼び出して`base.GetCell(tv)`も上書きする必要が返されるセルのカスタマイズ、および、`CellKey`次のように、要素に一意となるキーを返すプロパティ。

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

これは、ほとんどの要素がないでは、`StringElement`と`StyledStringElement`レンダリングのさまざまなシナリオの独自のキーのセットで使用されるとします。 これらのクラス内のコードをレプリケートする必要があります。

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

リフレクションと要素の API の両方を使用して同じ`DialogViewController`です。 ビューの外観をカスタマイズすることがありますするかの一部の機能を使用する場合があります、 `UITableViewController` Ui の基本的な作成に勝るです。

`DialogViewController`のサブクラスでは単なるは、`UITableViewController`をカスタマイズすると同じ方法でカスタマイズして、`UITableViewController`です。

たとえば、次のいずれかの一覧のスタイルを変更する場合は`Grouped`または`Plain`、次のように、コント ローラーを作成するときに、プロパティを変更することで、この値を設定する可能性があります。

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

高度なカスタマイズの`DialogViewController`などの背景を設定するとサブクラス化し、上書き、適切なメソッドは、次の例で示すようにします。

```csharp
class SpiffyDialogViewController : DialogViewController {
    UIImage image;

    public SpiffyDialogViewController (RootElement root, bool pushing, UIImage image) 
        : base (root, pushing) 
    {
        this.image = image;
    }

    public override LoadView ()
    {
        base.LoadView ();
        var color = UIColor.FromPatternImage(image);
        TableView.BackgroundColor = UIColor.Clear;
        ParentViewController.View.BackgroundColor = color;
    }
}
```

別のカスタマイズ ポイントは、次の仮想メソッドで、 `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

このメソッドは、のサブクラスを返す必要があります`DialogViewController.Source`の場合はここで、セルは均等にサイズ設定、またはそのサブクラスの`DialogViewController.SizingSource`セルが偶数である場合。

この上書きを使用して、いずれかをキャプチャすることができます、`UITableViewSource`メソッドです。 たとえば、 [TweetStation](https://github.com/migueldeicaza/TweetStation)これを使用して、ユーザーが、ページのトップへスクロールするときに追跡し、それに応じて未読ツイートの数を更新します。

## <a name="validation"></a>検証

要素を指定しない検証自体は、web ページの方が適しているモデルとしてデスクトップ アプリケーションは、iPhone の相互作用モデルに直接マップされません。

データ検証を実行する場合は、する必要がありますこれを行うとき、ユーザーが入力データにアクションをトリガーします。 たとえば、<span class="ui">完了</span>または<span class="ui">次へ</span>  ボタンをツールバーの上部またはその一部`StringElement`ボタンとして次のステージに移動するために使用します。

これは、ここで基本的な入力検証を実行して、複雑なサーバーとユーザー/パスワードの組み合わせの有効性の確認などの検証。

エラーをユーザーに通知する方法は、特定のアプリケーションです。 ポップアップでした、`UIAlertView`またはヒントを表示します。

## <a name="summary"></a>まとめ

この記事では、多くの MonoTouch.Dialog に関する情報について説明します。 方法の基礎を説明 mt.動作し、山を構成するさまざまなコンポーネントを対象と DD. さまざまな要素と山でサポートされているテーブルのカスタマイズも示しましたD 方法について説明し、mt.D は、カスタム要素を拡張できます。 山での JSON サポートをさらにその説明JSON から要素を動的に作成できるようにする D.


## <a name="related-links"></a>関連リンク

- [-スクリーン キャストである Miguel de Icaza により iOS ログイン画面 MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [スクリーン キャスト - MonoTouch.Dialog で iOS ユーザー インターフェイスを簡単に作成](http://youtu.be/j7OC5r8ZkYg)
- [チュートリアル: 要素 API を使用したアプリケーションの作成](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [チュートリアル: リフレクション API を使用したアプリケーションの作成](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [チュートリアル: JSON 要素を使用したユーザー インターフェイスの作成](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON マークアップ](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github の MonoTouch ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController クラスのリファレンス](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスのリファレンス](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
