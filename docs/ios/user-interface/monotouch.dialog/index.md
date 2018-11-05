---
title: MonoTouch.Dialog Xamarin.iOS 用の概要
description: このドキュメントには、MonoTouch.Dialog (山がについて説明しますD)、Xamarin.iOS での高速で宣言型の UI 開発用フレームワークです。 MonoTouch.Dialog Api を使用して、コードまたは JSON でインターフェイスを作成し、プルして更新、検索、背景イメージの読み込みなどの機能を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: c291a440a1937d2b0f1c229e3fa969caedba9ab9
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675459"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>MonoTouch.Dialog Xamarin.iOS 用の概要

MonoTouch.Dialog、山と呼ばれます略して、D は、開発者がアプリケーション画面とナビゲーション コント ローラーの表示、テーブルなどを作成する面倒な作業ではなく、情報を使用して構築できる、迅速な開発フォーム UI ツールキットです。そのため、UI の開発とコードの削減の大幅な簡略化したものを提供します。 たとえば、次のスクリーン ショットを考えてみます。

 [![](images/image1.png "たとえば、このスクリーン ショット")](images/image1.png#lightbox)

次のコードは、この画面全体を定義に使用しました。

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

IOS でのテーブルを使用する場合は大量の繰り返し実行するコードを多くの場合があります。
たとえば、テーブルが必要になるたび、そのテーブルを設定するデータ ソースが必要です。 ナビゲーション コント ローラーを介して接続されている 2 つのテーブル ベースの画面のあるアプリケーションでは、各画面は、多くの同じコードを共有します。

MT.D のテーブルの作成の一般的な API をカプセル化するすべてのコードでを簡略化します。 宣言型のオブジェクトがいっそう簡単に構文をバインドできる API の上に抽象化を提供します。 そのため、ある 2 つの Api で利用山D:

-   **低レベルの要素 API** –*要素 API*画面とそのコンポーネントを表す要素の階層ツリーの作成に基づきます。 要素 API に、開発者が最大限の柔軟性と Ui を作成することで制御できます。 さらに、要素の API に JSON で、サーバーから、UI の動的生成だけでなく、非常に高速の宣言を使用して宣言型の定義のサポートを拡張します。 
-   **高度なリフレクション API** – とも呼ばれる、*バインド*  *API* クラスは UI のヒントとし、山で注釈を付けるでD は自動的にオブジェクトに基づく画面を作成し、バックアップ、基になるオブジェクトとの間のバインドを表示 (および必要に応じて編集) 画面とを提供します。 上記の例では、リフレクション API の使用を示します。 この API は API の要素は、細かい制御を提供しませんが、クラスの属性に基づいて要素の階層を自動的に作成することにより複雑さ、さらに削減されます。 


MT.D は、UI 要素の画面の作成、ビルドの大規模なセットにパックされたが、カスタム要素および高度な画面レイアウトの必要性も認識。 そのため、機能拡張では、API をファーストクラスのおすすめです。 開発者は、既存の要素を拡張または新たに作成し、シームレスに統合し、ことができます。

さらに、mt.D では、多くの「プルして更新」読み込み中に、非同期のイメージをサポートします。 検索のサポートなど構築された、一般的な iOS UX の機能です。

この記事では山の使用方法の包括的な外観になりますD など。

-   **MT.D コンポーネント**– 山を構成するクラスについて重点的にこのD 短時間の短縮に取得を有効にします。 
-   **要素のリファレンス**– MT の組み込み要素の包括的な一覧D. 
-   **使用状況を高度な**– プルして更新、検索、背景イメージの読み込み、要素の階層を構築するための LINQ を使用して、カスタム要素、セルの作成などの高度な機能の情報し、山でのコント ローラーを使用して、D. 

## <a name="setting-up-mtd"></a>山の設定D

MT.D は、Xamarin.iOS で分散されます。 これを使用するを右クリックし、**参照**、Xamarin.iOS のノードは、Visual Studio 2017 または Visual Studio for Mac プロジェクトし、への参照を追加、 **MonoTouch.Dialog 1**アセンブリ。 次に、追加`using MonoTouch.Dialog`ソース内のステートメントは、必要に応じてコードします。

## <a name="understanding-the-pieces-of-mtd"></a>MT の部分を理解します。D

リフレクション API、山を使用する場合でもD は、直接要素 API を使用して作成された場合と同様に、内部的には、要素の階層を作成します。 また、前のセクションで説明されている JSON のサポートは要素をも作成されます。 このためは、MT の構成要素の基本的な知識がなくてもD.

MT.D は、次の 4 つの部分を使用して画面を作成します。

-  **DialogViewController**
-  **RootElement**
-  **セクション**
-  **要素**


### <a name="dialogviewcontroller"></a>DialogViewController

A *DialogViewController*、または*DVC* 、略してから継承`UITableViewController`そのための画面にテーブルを表します。 DVCs は、通常 UITableViewController と同じようにナビゲーション コント ローラーにプッシュされることができます。

### <a name="rootelement"></a>RootElement

A *RootElement*は DVC に移動するアイテムの最上位のコンテナーです。 要素を含めることができますし、セクションが含まれています。 RootElements はレンダリングされません。代わりにどのような実際にレンダリングを取得のコンテナーだけです。 RootElement が、DVC に割り当てられ、DVC、その子をレンダリングします。

### <a name="section"></a>セクション

セクションは、テーブル内のセルのグループです。 通常のテーブルのセクションで、必要に応じてことがあるできます、ヘッダーとフッターをいずれかをテキスト、または次のスクリーン ショットのように、カスタム ビューをします。

 [![](images/image2.png "ヘッダーとフッターをいずれかのことができますが、テキスト、またはカスタム ビューは、このスクリーン ショットのようにするように、必要に応じて、通常のテーブルのセクションで、")](images/image2.png#lightbox)

### <a name="element"></a>要素

要素は、テーブル内の実際のセルを表します。 MT.D は、さまざまなデータ型または異なる入力を表す要素のさまざまなパックされたものです。 たとえば、次のスクリーン ショットは、使用可能な要素のいくつかを示しています。

 [![](images/image3.png "たとえば、このスクリーン ショットは、利用可能な要素のいくつかを示しています。")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>セクションと RootElements

今すぐについて説明します RootElements とセクションではさらに詳しく説明します。

### <a name="rootelements"></a>RootElements

MonoTouch.Dialog プロセスを開始するには少なくとも 1 つ RootElement が必要です。

セクションまたは要素値を持つ、RootElement が初期化されている場合、この値は、画面の右側に表示されると、構成の概要を提供する要素の子を検索する使用されます。 たとえば、次のスクリーン ショットには、右側の「デザート」選択した砂漠の値と共に、詳細画面のタイトルを含むセルが左側のテーブルを示します。

 [![](images/image4.png "このスクリーン ショットは、左側のテーブルを右側のデザート、選択した砂漠の値と共に、詳細画面のタイトルを含むセルは")](images/image4.png#lightbox) [![](images/image5.png "これ次のスクリーン ショットには、右側のデザート、選択した砂漠の値と共に、詳細画面のタイトルを含むセルが左側のテーブルを示しています")](images/image5.png#lightbox)

ルート要素こともできます内のセクションでは、新しい入れ子になった構成ページの読み込みをトリガーする上記のようです。 このモードで使用すると、指定されたキャプションはセクション内にレンダリング中に使用され、サブページのタイトルとしても使用されます。 例えば:

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

上記の例では、「デザート」で、ユーザーがタップしたとき MonoTouch.Dialog は新しいページを作成し、「デザート」されているし、3 つの値のオプション グループのルートに移動します。

このサンプルで、RadioGroup に値「2」を渡しているので、オプション グループは「デザート」セクションでは「チョコレート ケーキ」を選択します。 つまり、リスト (0 インデックス) の 3 番目の項目を選択します。

Add メソッドを呼び出すか、c# 4 の初期化子構文を使用してセクションを追加します。
アニメーションがセクションを挿入する Insert メソッドが提供されます。

(、RadioGroup) ではなくグループのインスタンスを RootElement を作成する場合、RootElement セクションに表示される場合の集計値が BooleanElements と CheckboxElements Group.Key 値として同じキーを持つすべての累積数になります。

### <a name="sections"></a>セクション

セクションが画面に要素をグループ化に使用されています。 および、RootElement の唯一の有効な直接の子であります。 セクションでは、新しい RootElements をなど、標準的な要素のいずれかを含めることができます。

セクションに埋め込まれた RootElements は、新しいより深いレベルに移動に使用されます。

セクションには、文字列、または UIViews ヘッダーとフッターを持つことができます。
文字列をだけ使用する通常がカスタム Ui を作成するのには、ヘッダーまたはフッターとして任意の UIView を使用できます。 次のように作成するのに文字列を使用することができますか。

```csharp
var section = new Section ("Header", "Footer")
```

ビューを使用するには、コンス トラクターに、ビューを渡すだけ。

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header)
```

### <a name="getting-notified"></a>通知の取得

#### <a name="handling-nsaction"></a>NSAction の処理

MT.D の表面を`NSAction`コールバックを処理するためのデリゲート。
たとえば、山によって作成されたテーブルのセルのタッチ イベントを処理します。D. 山で要素を作成する場合D、単にコールバックを指定は、次に示すように関数します。

```csharp
new Section () {
        new StringElement ("Demo Callback", 
                delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>要素の値を取得します。

組み合わせて、`Element.Value`プロパティ、コールバックは、その他の要素で設定された値を取得できます。 次に例を示します。

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

このコードは、次に示すように、UI を作成します。 この例の完全なチュートリアルについてを参照してください。、[要素 API チュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)チュートリアル。

 [![](images/image6.png "Element.Value プロパティと組み合わせると、コールバックは他の要素で設定された値を取得できます。")](images/image6.png#lightbox)

ユーザーは、表の下のセルを押すと、匿名関数のコード実行から値を書き込み、`element`インスタンスを**アプリケーション出力**Visual Studio for mac のパッド

## <a name="built-in-elements"></a>組み込み要素

MT.D は、要素と呼ばれる組み込みのテーブル セルの項目数が付属します。
これらの要素を使用して、テーブルのセルなど、いくつかの名前を付ける文字列、浮動小数点数、日付およびイメージも、さまざまな種類を表示できます。 データ型を適切に表示する各要素が処理します。 たとえば、ブール型の要素をその値を切り替えるスイッチが表示されます。 同様に、浮動小数点数の要素を浮動小数点値を変更するスライダーが表示されます。

画像や html などの豊富なデータ型をサポートするためより複雑な要素があります。 たとえばが選択されている場合は、web ページを読み込むの UIWebView を開き、html 要素は、テーブルのセルにキャプションを表示します。

### <a name="working-with-element-values"></a>要素の値の操作

ユーザー入力の取得に使用される要素の公開をパブリック`Value`プロパティをいつでも、要素の現在の値を保持します。 ユーザーがアプリケーションを使用して自動的に更新されます。

これはすべての MonoTouch.Dialog の一部である要素の動作ですが、ユーザーが作成した要素の必須ではありません。

### <a name="string-element"></a>文字列の要素

A`StringElement`左側にある表のセルとセルの右側にある文字列値のキャプションを示しています。

 [![](images/image7.png "StringElement を左側にある表のセルとセルの右側にある文字列値のキャプションを示しています")](images/image7.png#lightbox)

使用する、`StringElement`ボタンと、デリゲートを指定します。

```csharp
new StringElement (
        "Click me",
        () => { new UIAlertView("Tapped", "String Element Tapped"
, null, "ok", null).Show(); })
```

 [![](images/image8.png "ボタンとして、StringElement を使用するには、デリゲートを指定します。")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>スタイルの文字列要素

A`StyledStringElement`文字列のいずれかの組み込みのテーブル セルのスタイルを使用して表示する、またはカスタム書式設定します。

 [![](images/image9.png "StyledStringElement により、いずれかの組み込みのテーブル セルのスタイルを使用して表示する文字列またはカスタムの書式設定")](images/image9.png#lightbox)

`StyledStringElement`クラスから派生`StringElement`、ことができますが、開発者は、いくつかのテキストの色、背景セル色、線の互換性に影響するモード、行数を表示するには、フォントなどのプロパティをカスタマイズおよびアクセサリを表示するかどうか。

### <a name="multiline-element"></a>複数行の要素

 [![](images/image10.png "複数行の要素")](images/image10.png#lightbox)

### <a name="entry-element"></a>エントリ要素

`EntryElement`名前と意味、ユーザー入力を取得するために使用します。 通常の文字列または文字が非表示、パスワードのいずれかをサポートします。

 [![](images/image11.png "ユーザー入力を取得する、EntryElement が使用されます。")](images/image11.png#lightbox)

3 つの値で初期化されます。

-  ユーザーに表示されるエントリのキャプション。
-  (これは、ユーザーにヒントを提供する淡色のテキスト) プレース ホルダー テキスト。 
-  テキストの値。


プレース ホルダーと値を null にすることができます。 ただし、キャプションが必要です。

任意の時点では、その値プロパティにアクセスすることができますの値を取得、`EntryElement`します。

さらに、`KeyboardType`データ入力用に必要なキーボードの種類のスタイルを作成時にプロパティを設定することができます。 これは、構成の値を使用して、キーボードを使用することができます`UIKeyboardType`次に示すよう。

-  数字
-  電話番号
-  URL
-  電子メール


### <a name="boolean-element"></a>ブール型要素

 [![](images/image12.png "ブール型要素")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>チェック ボックス要素

 [![](images/image13.png "チェック ボックス要素")](images/image13.png#lightbox)

### <a name="radio-element"></a>ラジオ要素

A`RadioElement`が必要です、`RadioGroup`で指定する、`RootElement`します。

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0))
```

 [![](images/image14.png "RadioElement が、RadioGroup、RootElement で指定する必要があります。")](images/image14.png#lightbox)

 `RootElements` オプションの要素を調整するためも使用されます。 `RadioElement`メンバーが複数のセクションでは (たとえばリング トーン セレクターとカスタムの個別の着信音に着信音のシステムからのようなものを導入するなど) にまたがることができます。 概要ビューは、現在選択されているオプションの要素に表示されます。 を使用するには、作成、`RootElement`グループ コンス トラクターのようにします。

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0))
```

内のグループの名前`RadioGroup`(ある場合) に格納されているページで選択した値と値のゼロをここでは、これが最初に選択した項目のインデックスを表示するために使用します。

### <a name="badge-element"></a>バッジの要素

 [![](images/image15.png "バッジの要素")](images/image15.png#lightbox)

### <a name="float-element"></a>浮動小数点数の要素

 [![](images/image16.png "浮動小数点数の要素")](images/image16.png#lightbox)

### <a name="activity-element"></a>アクティビティ要素

 [![](images/image17.png "アクティビティ要素")](images/image17.png#lightbox)

### <a name="date-element"></a>日付要素

 ![](images/image18.png "日付要素")

DateElement に対応するセルを選択すると、次に示すように日付選択カレンダーが表示されます。

 [![](images/image19.png "ように、DateElement に対応するセルを選択すると、日付の選択が表示されます。")](images/image19.png#lightbox)

### <a name="time-element"></a>Time 要素

 [![](images/image20.png "Time 要素")](images/image20.png#lightbox)

TimeElement に対応するセルが選択されている場合は、次に示す時間の選択が表示されます。

 [![](images/image21.png "ように、TimeElement に対応するセルを選択すると、時間の選択が表示されます。")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime 要素

 [![](images/image22.png "DateTime 要素")](images/image22.png#lightbox)

DateTimeElement に対応するセルを選択すると、次に示すように datetime ピッカーが表示されます。

 [![](images/image23.png "ように、DateTimeElement に対応するセルを選択すると、datetime ピッカーが表示されます。")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML 要素

 [![](images/image24.png "HTML 要素")](images/image24.png#lightbox)

`HTMLElement`の値を表示します。 その`Caption`プロパティ テーブルのセルにします。 選択すると、`Url`要素に割り当てられているに読み込まれます、`UIWebView`次に示すように制御します。

 [![](images/image25.png "次に示すように要素に割り当てられた Url が UIWebView コントロールに読み込まれるが選択した場合、")](images/image25.png#lightbox)

### <a name="message-element"></a>メッセージ要素

 [![](images/image26.png "メッセージ要素")](images/image26.png#lightbox)

### <a name="load-more-element"></a>複数の要素を読み込む

一覧で複数の項目を読み込むようにするのにには、この要素を使用します。 法線と読み込みのキャプション、ほかのフォントとテキストの色をカスタマイズすることができます。
`UIActivity`インジケーターの開始をアニメーション化して、セルをタップすると、読み込みのキャプションが表示され、`NSAction`に渡されるコンス トラクターを実行します。 コードで 1 回、`NSAction`が完了したら、`UIActivity`インジケーターは、アニメーション化を停止し、標準のキャプションが再び表示されます。

### <a name="uiview-element"></a>UIView 要素

さらに、任意のカスタム`UIView`を使用して表示できます、`UIViewElement`します。

### <a name="owner-drawn-element"></a>オーナー描画要素

この要素は、抽象クラスがサブクラス化する必要があります。 オーバーライドする必要があります、`Height(RectangleF bounds)`メソッドを要素の高さを返す必要がありますだけでなく`Draw(RectangleF bounds, CGContext context, UIView view)`でを行う必要があります、指定した境界内のすべてのカスタマイズされた描画コンテキストとビューのパラメーターを使用します。 この要素はサブクラス化の面倒を`UIView`、返されるセルに配置することのみを必要とする 2 つの単純な上書きを実装することです。 サンプル アプリでより優れたサンプルの実装を確認できます、`DemoOwnerDrawnElement.cs`ファイル。

クラスを実装するのに非常に単純な例を次に示します。

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

`JsonElement`のサブクラスは`RootElement`まで、`RootElement`ローカルまたはリモートの url から入れ子になった子の内容を読み込むことができます。

`JsonElement`は、 `RootElement` 2 つの形式でインスタンス化することができます。 1 つのバージョンを作成、`RootElement`をオンデマンドで内容が読み込まれます。 これらを使用して作成されて、`JsonElement`を最後に、url からコンテンツを読み込めません余分な引数を受け取るコンス トラクター。

```csharp
var je = new JsonElement ("Dynamic Data", "http://tirania.org/tmp/demo.json");
```

その他のフォームからローカル ファイルまたは既存のデータを作成する`System.Json.JsonObject`ですが、既に解析済みします。

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

山で JSON の使用の詳細についてはD を参照してください、 [JSON 要素のチュートリアル](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough)チュートリアル。

## <a name="other-features"></a>その他の機能

### <a name="pull-to-refresh-support"></a>プルして更新のサポート

 *プル-に-* *更新*視覚効果を最初にある、 *Tweetie2*アプリで、多くのアプリケーション間での一般的な効果のようになりました。

に、ダイアログ ボックスを自動的にプルして更新サポートを追加するだけ行う必要がある 2 つの処理: ユーザーは、データをプルするときに通知するイベント ハンドラーをフックし、通知、`DialogViewController`既定の状態に戻るデータが読み込まれたとき。

通知フックは、単純です。接続のみ、`RefreshRequested`上のイベント、 `DialogViewController`、次のように。

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

次のメソッドに`OnUserRequestedRefresh`、キューにいくつかのデータの読み込み、ネットから一部のデータを要求またはデータを計算するスレッドをスピンは。 データが読み込まれた後に通知する必要があります、`DialogViewController`に、新しいデータがあり、ビューを既定の状態に復元するには呼び出しを`ReloadComplete`:

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>検索のサポート

検索をサポートする設定、`EnableSearch`プロパティを`DialogViewController`します。 設定することも、`SearchPlaceholder`検索バーでのウォーターマーク テキストとして使用するプロパティ。

検索、ユーザーの種類として、ビューの内容が変更されます。 表示されるフィールドを検索し、それらをユーザーに示します。 `DialogViewController`プログラムから開始、終了、または結果に新しいフィルター操作をトリガーする 3 つのメソッドを公開します。 これらのメソッドは、以下に示します。

-  `StartSearch`
-  `FinishSearch`
-  `PerformFilter`


システムは、する場合は、この動作を変更できるように、拡張可能です。

### <a name="background-image-loading"></a>背景イメージの読み込み

MonoTouch.Dialog が組み込まれています、 [TweetStation](https://github.com/migueldeicaza/TweetStation)アプリケーションのイメージ ローダー。 このイメージ ローダーでは、バック グラウンドでキャッシュをサポートするイメージの読み込みに使用できるし、イメージが読み込まれたときに、コードを通知することができます。

発信ネットワーク接続の数も限定されます。

イメージ ローダーがで実装された、`ImageLoader`クラスで行う必要があるすべての呼び出しは、`DefaultRequestImage`メソッドのインスタンスと同様に、読み込むイメージの Uri を指定する必要が、`IImageUpdated`インターフェイスとなるされると呼び出されますイメージ ha%s が読み込まれています。

たとえば、次のコードがそのに Url からイメージを読み込みます、 `BadgeElement`:

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
        new Section(){
                new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
        }
};
```

ImageLoader クラスは、すべてのメモリに現在キャッシュされているイメージをリリースするときに呼び出すことのできる Purge メソッドを公開します。 現在のコードでは、50 個のイメージのキャッシュを持っています。 場合 (たとえば、大きい 50 個のイメージがあるが多すぎるためにイメージが必要な) 場合は、さまざまなキャッシュ サイズを使用すると、だけ ImageLoader のインスタンスを作成し、キャッシュに保持したいイメージの数を渡すことができます。

## <a name="using-linq-to-create-element-hierarchy"></a>LINQ を使用して、要素の階層を作成するには

LINQ と # の初期化の構文の巧妙な使用法を使用して LINQ を使用して、要素の階層を作成します。 たとえば、次のコードは、一部の文字列配列から画面を作成およびハンドル セルのそれぞれに渡される匿名関数を使用して選択`StringElement`:

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

XML データ ストアまたはデータからほぼまったく複雑なアプリケーションを作成するデータベースのデータを簡単に組み合わせる。

## <a name="extending-mtd"></a>山の拡張D

### <a name="creating-custom-elements"></a>カスタム要素を作成します。

既存の要素のいずれかから継承するか、要素のルート クラスから派生することによって、独自の要素を作成できます。

独自の要素を作成するには、次のメソッドをオーバーライドします。

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

実装する必要があります、要素には、変数のサイズを設定できる場合、`IElementSizing`インターフェイスで、1 つのメソッドが含まれています。

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
    float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

実装する方法を計画している場合、`GetCell`メソッドを呼び出して`base.GetCell(tv)`もオーバーライドする必要が返されるセルのカスタマイズ、および、`CellKey`次のように、要素に一意となるキーを返すプロパティ。

```csharp
static NSString MyKey = new NSString ("MyKey");
    protected override NSString CellKey {
        get {
            return MyKey;
        }
    }
```

これは、動作ではなく、ほとんどの要素、`StringElement`と`StyledStringElement`独自キーのセットを使用して、さまざまなレンダリングのシナリオのものとします。 これらのクラスのコードを複製する必要があります。

### <a name="dialogviewcontrollers-dvcs"></a>DialogViewControllers (DVCs)

リフレクションと要素 API の両方を使用して同じ`DialogViewController`します。 ビューの外観をカスタマイズすることがありますまたはの一部の機能を使用したい場合があります、 `UITableViewController` Ui の基本的な作成に勝ます。

`DialogViewController`のサブクラスだけでは、`UITableViewController`をカスタマイズするのと同じ方法でカスタマイズして、`UITableViewController`します。

たとえば、次のいずれかにリスト スタイルを変更したい`Grouped`または`Plain`、次のように、コント ローラーを作成するときに、プロパティを変更することで、この値を設定する可能性があります。

```csharp
var myController = new DialogViewController (root, true){
        Style = UITableViewStyle.Grouped;
    }
```

高度なカスタマイズの`DialogViewController`、その背景を設定するなどの場合サブクラスですし、上書き、適切なメソッドは、次の例で示すようにします。

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

別のカスタマイズ ポイントは、次の仮想メソッド、 `DialogViewController`:

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

このメソッドは、のサブクラスを返す必要があります`DialogViewController.Source`ケースで、セルのサイズに均等には、またはそのサブクラスの`DialogViewController.SizingSource`セルが偶数である場合。

この上書きを使用して、いずれかをキャプチャすることができます、`UITableViewSource`メソッド。 たとえば、 [TweetStation](https://github.com/migueldeicaza/TweetStation)これを使用して、ページのトップへ、ユーザーがスクロールされたときに追跡し、未読のツイートの数を適宜更新します。

## <a name="validation"></a>検証

要素に渡さないように検証自体として web ページに最適なモデルがおよびデスクトップ アプリケーションは、iPhone の対話モデルに直接マップされていません。

データの検証を実行する場合は、する必要がありますこれを行うユーザーが入力データにアクションをトリガーしたときにします。 たとえば、<span class="ui">完了</span>または<span class="ui">[次へ]</span>上部のツールバーで、またはいくつかのボタン`StringElement`ボタンとして次のステージに移動するために使用します。

これは、基本的な入力の検証を実行して複雑な検証サーバーでのユーザー/パスワードの組み合わせの有効性チェックのようなのでしょう。

エラーをユーザーに通知する方法は、特定のアプリケーションです。 ポップアップする可能性があります、`UIAlertView`またはヒントを表示します。

## <a name="summary"></a>まとめ

この記事では、多くの MonoTouch.Dialog についての情報について説明します。 方法の基礎を説明 mt.D は、動作、山を構成するさまざまなコンポーネントの説明D. さまざまな要素や山でサポートされているテーブルのカスタマイズも示しましたD と方法について説明 mt.D は、カスタム要素を拡張できます。 さらに山で JSON のサポートについて説明しましたJSON から要素を動的に作成できるようにする D.


## <a name="related-links"></a>関連リンク

- [MonoTouch.Dialog とスクリーン キャスト - Miguel de Icaza の作成、iOS のログイン画面](http://youtu.be/3butqB1EG0c)
- [スクリーン キャスト - MonoTouch.Dialog で iOS ユーザー インターフェイスを簡単に作成](http://youtu.be/j7OC5r8ZkYg)
- [チュートリアル: 要素 API を使用したアプリケーションの作成](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [チュートリアル: リフレクション API を使用したアプリケーションの作成](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [チュートリアル: JSON 要素を使用したユーザー インターフェイスの作成](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch.Dialog JSON マークアップ](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github の MonoTouch ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController クラスのリファレンス](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスのリファレンス](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
