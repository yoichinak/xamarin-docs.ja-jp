---
title: Xamarin の Monotouch.dialog の概要
description: このドキュメントでは、Monotouch.dialog (MT) について説明します。D) は、Xamarin を使用して、宣言型 UI の迅速な開発を行うためのフレームワークです。 Monotouch.dialog Api を使用して、コードまたは JSON でインターフェイスを作成し、プルから更新、検索、背景画像の読み込みなどの機能を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 52A35B24-C23B-8461-A8FF-5928A2128FB0
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: cc7d966e7dd0371e967775d112789af9f0ad7c59
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528518"
---
# <a name="introduction-to-monotouchdialog-for-xamarinios"></a>Xamarin の Monotouch.dialog の概要

Monotouch.dialog。 MT と呼ばれます。D は、簡単な UI 開発ツールキットです。これを使用すると、開発者はビューコントローラーやテーブルなどの面倒な作業を作成するのではなく、情報を使用してアプリケーション画面とナビゲーションを構築できます。そのため、UI の開発とコードの削減が大幅に簡素化されます。 たとえば、次のスクリーンショットを考えてみます。

 [![](images/image1.png "たとえば、次のスクリーンショットを考えてみます。")](images/image1.png#lightbox)

次のコードは、この画面全体を定義するために使用されています。

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

IOS でテーブルを使用する場合、多くの場合、大量の繰り返し実行するコードがあります。
たとえば、テーブルが必要になるたびに、そのテーブルを設定するためにデータソースが必要になります。 ナビゲーションコントローラーを介して接続されている2つのテーブルベースの画面があるアプリケーションでは、各画面は同じコードを多数共有します。

MT.D では、すべてのコードをテーブル作成用の汎用 API にカプセル化することで、これを簡略化しています。 次に、その API の上に抽象化を提供します。これにより、宣言型オブジェクトのバインディング構文を簡単に使用できるようになります。 そのため、MT で使用できる Api は2つあります。A

- **低レベル要素 api** – *elements api*は、画面とそのコンポーネントを表す要素の階層ツリーを作成することに基づいています。 Elements API を使用すると、開発者は Ui を作成する際の柔軟性と制御を最大限に高めることができます。 さらに、Elements API は JSON による宣言型定義の高度なサポートを備えています。これにより、非常に高速な宣言と、サーバーからの動的 UI 生成が可能になります。 
- **高度なリフレクション API** – とも呼ばれる、*バインド*  *API* クラスは UI のヒントとし、山で注釈を付けるでD は自動的にオブジェクトに基づく画面を作成し、バックアップ、基になるオブジェクトとの間のバインドを表示 (および必要に応じて編集) 画面とを提供します。 上記の例では、リフレクション API の使用方法を示しています。 この API は、elements API が行う細かい制御を提供していませんが、クラス属性に基づいて要素階層を自動的に構築することで、複雑さをさらに軽減します。 


MT.D には、画面作成用の多数の組み込み UI 要素が用意されていますが、カスタマイズされた要素や高度な画面レイアウトが必要であることも認識しています。 そのため、拡張機能は API に組み込まれているファーストクラスの機能です。 開発者は、既存の要素を拡張したり、新しい要素を作成してシームレスに統合したりできます。

さらに、MT.D には、多くの一般的な iOS UX 機能が組み込まれています。これには、"更新の取得"、"非同期のイメージ読み込み"、"検索のサポート" などがあります。

この記事では、MT の使用方法について詳しく説明します。D (次を含む):

- **MT。D コンポーネント**– MT を構成するクラスを理解することに重点を置いています。D を使用すると、短時間で迅速に作業を開始できます。 
- **Elements リファレンス**– MT の組み込み要素の包括的な一覧です。A. 
- **高度な使用法**–これは、プルから更新、検索、背景画像の読み込み、LINQ を使用した要素階層の構築、MT で使用するカスタム要素、セル、およびコントローラーの作成などの高度な機能に対応しています。A. 

## <a name="setting-up-mtd"></a>MT を設定しています。A

MT.D は、Xamarin. iOS と共に配布されます。 これを使用するには、Visual Studio 2017 または Visual Studio for Mac で Xamarin. iOS プロジェクトの **[参照]** ノードを右クリックし、 **monotouch.dialog**アセンブリへの参照を追加します。 次に、 `using MonoTouch.Dialog`必要に応じて、ソースコードにステートメントを追加します。

## <a name="understanding-the-pieces-of-mtd"></a>MT の部分を理解する。A

リフレクション API を使用している場合でも、MT です。D は、Elements API を直接使用して作成された場合と同様に、要素階層を内部で作成します。 また、前のセクションで説明した JSON サポートでも要素が作成されます。 このため、MT の構成要素について基本的な知識を持っていることが重要です。A.

MT.D は、次の4つの部分を使用して画面を構築します。

- **DialogViewController**
- **RootElement**
- **セクション**
- **要素**


### <a name="dialogviewcontroller"></a>DialogViewController

DVCの場合は、を継承`UITableViewController`します。この場合、テーブルを含む画面を表します。 DVCs は、通常の UITableViewController と同様にナビゲーションコントローラーにプッシュできます。

### <a name="rootelement"></a>RootElement

*Rootelement*は、DVC に入る項目の最上位レベルのコンテナーです。 これには、要素を含めることができるセクションが含まれています。 RootElements はレンダリングされません。実際に表示されるのは単なるコンテナーです。 RootElement は DVC に割り当てられ、その子が DVC によってレンダリングされます。

### <a name="section"></a>セクション

セクションは、テーブル内のセルのグループです。 通常のテーブルセクションと同様に、次のスクリーンショットに示すように、必要に応じてヘッダーとフッターを指定することもできます。これはテキストでもカスタムビューでもかまいません。

 [![](images/image2.png "通常のテーブルセクションと同様に、必要に応じて、このスクリーンショットのように、テキストまたはカスタムビューのいずれかを指定できるヘッダーとフッターを含めることができます。")](images/image2.png#lightbox)

### <a name="element"></a>要素

要素は、テーブル内の実際のセルを表します。 MT.D には、さまざまなデータ型または異なる入力を表すさまざまな要素が用意されています。 たとえば、次のスクリーンショットは、使用可能ないくつかの要素を示しています。

 [![](images/image3.png "たとえば、次のスクリーンショットは、使用可能ないくつかの要素を示しています。")](images/image3.png#lightbox)

## <a name="more-on-sections-and-rootelements"></a>セクションと RootElements の詳細

ここで、RootElements とセクションについてさらに詳しく説明します。

### <a name="rootelements"></a>RootElements

Monotouch.dialog プロセスを開始するには、少なくとも1つの RootElement が必要です。

RootElement がセクション/要素の値で初期化されている場合、この値を使用して、構成の概要を提供する子要素を検索します。これは、ディスプレイの右側に表示されます。 たとえば、次のスクリーンショットでは、左側にテーブルが表示され、右側に詳細画面のタイトル、"デザート"、および選択した砂漠の値が含まれています。

 [![](images/image4.png "このスクリーン ショットは、左側のテーブルを右側のデザート、選択した砂漠の値と共に、詳細画面のタイトルを含むセルは")](images/image4.png#lightbox) [![](images/image5.png "これ次のスクリーン ショットには、右側のデザート、選択した砂漠の値と共に、詳細画面のタイトルを含むセルが左側のテーブルを示しています")](images/image5.png#lightbox)

上の例のように、ルート要素をセクション内で使用して、入れ子になった新しい構成ページの読み込みをトリガーすることもできます。 このモードで使用すると、指定されたキャプションがセクション内に表示されるときに使用され、サブページのタイトルとしても使用されます。 例:

```csharp
var root = new RootElement ("Meals") {
    new Section ("Dinner") {
        new RootElement ("Dessert", new RadioGroup ("dessert", 2)) {
            new Section () {
                new RadioElement ("Ice Cream", "dessert"),
                new RadioElement ("Milkshake", "dessert"),
                new RadioElement ("Chocolate Cake", "dessert")
            }
        }
    }
};
```

上の例では、ユーザーが "デザート" をタップすると、Monotouch.dialog によって新しいページが作成され、ルートが "デザート" で、3つの値を持つラジオグループがあります。

このサンプルでは、"デザート" セクションで "2" という値を RadioGroup に渡したため、ラジオグループによって "チョコレートケーキ" が選択されています。 これは、リストの3番目の項目 (ゼロインデックス) を選択することを意味します。

Add メソッドを呼び出すか、 C# 4 つの初期化子構文を使用すると、セクションが追加されます。
挿入メソッドは、アニメーションを使用してセクションを挿入するために用意されています。

RadioGroup の代わりにグループインスタンスを使用して RootElement を作成する場合、セクションに表示される RootElement の summary 値は、BooleanElements と同じキーを持つすべてのと CheckboxElements の累積数になります。キー値。

### <a name="sections"></a>セクション

セクションは、画面内の要素をグループ化するために使用され、RootElement の唯一の有効な直接の子です。 セクションには、新しい RootElements を含む標準の要素を含めることができます。

セクションに埋め込まれている RootElements は、新しいより深いレベルに移動するために使用されます。

セクションには、ヘッダーとフッターを文字列として、または UIViews として含めることができます。
通常は、文字列のみを使用しますが、カスタム Ui を作成するには、任意の UIView をヘッダーまたはフッターとして使用できます。 文字列を使用して、次のように作成できます。

```csharp
var section = new Section ("Header", "Footer");
```

ビューを使用するには、次のようにビューをコンストラクターに渡します。

```csharp
var header = new UIImageView (Image.FromFile ("sample.png"));
var section = new Section (header);
```

### <a name="getting-notified"></a>通知を受け取る

#### <a name="handling-nsaction"></a>NSAction の処理

MT.D は、 `NSAction`コールバックを処理するためのデリゲートとしてを使用します。
たとえば、MT によって作成されたテーブルセルのタッチイベントを処理するとします。A. MT を使用して要素を作成する場合。次に示すように、単にコールバック関数を指定します。

```csharp
new Section () {
    new StringElement ("Demo Callback", delegate { Console.WriteLine ("Handled"); })
}
```

#### <a name="retrieving-element-value"></a>要素の値を取得しています

コールバックは`Element.Value` 、プロパティと組み合わせて、他の要素で設定された値を取得できます。 次に例を示します。

```csharp
var element = new EntryElement (task.Name, "Enter task description", task.Description);
                
var taskElement = new RootElement (task.Name) {
    new Section () { element },
    new Section () { new DateElement ("Due Date", task.DueDate) },
    new Section ("Demo Retrieving Element Value") {
        new StringElement ("Output Task Description", delegate { Console.WriteLine (element.Value); })
    }
};
```

次に示すように、このコードでは UI を作成します。 この例の完全なチュートリアルについては、 [ELEMENTS API チュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)のチュートリアルを参照してください。

 [![](images/image6.png "また、コールバックは他の要素で設定された値を取得できます。値プロパティと組み合わせて使用します。")](images/image6.png#lightbox)

ユーザーが下部のテーブルセルを押すと、匿名関数のコードが実行され、 `element`インスタンスから Visual Studio for Mac の**アプリケーション出力**パッドに値が書き込まれます。

## <a name="built-in-elements"></a>組み込み要素

MT.D には、要素と呼ばれる多数の組み込みテーブルセルアイテムが付属しています。
これらの要素を使用して、テーブルのセル (文字列、浮動小数点数、日付、画像など) にさまざまな種類の異なる型を表示することができます。 各要素は、データ型を適切に表示します。 たとえば、ブール型の要素には、その値を切り替えるスイッチが表示されます。 同様に、float 要素には、浮動小数点値を変更するためのスライダーが表示されます。

画像や html などの豊富なデータ型をサポートするために、さらに複雑な要素もあります。 たとえば、選択したときに web ページを読み込むために UIWebView を開く html 要素は、テーブルセルにキャプションを表示します。

### <a name="working-with-element-values"></a>要素の値の操作

ユーザー入力のキャプチャに使用される要素は、 `Value`いつでも要素の現在の値を保持するパブリックプロパティを公開します。 ユーザーがアプリケーションを使用すると、自動的に更新されます。

これは、Monotouch.dialog の一部であるすべての要素の動作ですが、ユーザーが作成した要素には必要ありません。

### <a name="string-element"></a>String 要素

は`StringElement` 、テーブルセルの左側にキャプションを表示し、セルの右側に文字列値を表示します。

 [![](images/image7.png "StringElement は、テーブルセルの左側にキャプションを、セルの右側に文字列値を表示します。")](images/image7.png#lightbox)

をボタン`StringElement`として使用するには、デリゲートを指定します。

```csharp
new StringElement ("Click me", () => { 
    new UIAlertView("Tapped", "String Element Tapped", null, "ok", null).Show();
});
```

 [![](images/image8.png "StringElement をボタンとして使用するには、デリゲートを指定します。")](images/image8.png#lightbox)

### <a name="styled-string-element"></a>スタイルが指定した文字列要素

で`StyledStringElement`は、組み込みのテーブルセルスタイルまたはカスタム書式設定を使用して文字列を表示できます。

 [![](images/image9.png "StyledStringElement を使用すると、組み込みのテーブルセルスタイルまたはカスタム書式設定を使用して文字列を表示できます。")](images/image9.png#lightbox)

クラス`StyledStringElement`はから`StringElement`派生しますが、開発者は、フォント、テキストの色、背景のセルの色、改行モード、表示する行数、アクセサリを表示するかどうかなど、いくつかのプロパティをカスタマイズできます。

### <a name="multiline-element"></a>複数行要素

 [![](images/image10.png "複数行要素")](images/image10.png#lightbox)

### <a name="entry-element"></a>Entry 要素

`EntryElement`名前が示すように、は、ユーザー入力を取得するために使用されます。 これは、文字が非表示になっている通常の文字列またはパスワードのいずれかをサポートしています。

 [![](images/image11.png "EntryElement は、ユーザー入力を取得するために使用されます。")](images/image11.png#lightbox)

次の3つの値で初期化されます。

- ユーザーに表示されるエントリのキャプション。
- プレースホルダーテキスト (これは、ユーザーにヒントを提供するグレー表示のテキストです)。 
- テキストの値。


プレースホルダーと値には null を指定できます。 ただし、キャプションは必須です。

任意の時点で、その値プロパティにアクセスすると、 `EntryElement`の値を取得できます。

さらに`KeyboardType` 、プロパティは、作成時にデータ入力に必要なキーボードの種類のスタイルに設定できます。 これは、次に示すの`UIKeyboardType`値を使用してキーボードを構成するために使用できます。

- 数字
- 電話番号
- URL
- 電子メール


### <a name="boolean-element"></a>ブール型の要素

 [![](images/image12.png "ブール型の要素")](images/image12.png#lightbox)

### <a name="checkbox-element"></a>Checkbox 要素

 [![](images/image13.png "Checkbox 要素")](images/image13.png#lightbox)

### <a name="radio-element"></a>Radio 要素

では、 `RadioGroup`でを指定`RootElement`する必要があります。`RadioElement`

```csharp
mtRoot = new RootElement ("Demos", new RadioGroup("MyGroup", 0));
```

 [![](images/image14.png "RadioElement を使用するには、RootElement に RadioGroup を指定する必要があります。")](images/image14.png#lightbox)

 `RootElements`は、無線要素を調整するためにも使用されます。 メンバー `RadioElement`は複数のセクションにまたがることができます (たとえば、リングトーンセレクターに似たようなものを実装し、システムの着信音からカスタムリングトーンを分離します)。 [概要] ビューには、現在選択されているラジオ要素が表示されます。 これを使用するには`RootElement` 、次のように、グループコンストラクターを使用してを作成します。

```csharp
var root = new RootElement ("Meals", new RadioGroup ("myGroup", 0));
```

内`RadioGroup`のグループの名前は、含まれるページ (存在する場合) で選択されている値を表示するために使用されます。この場合の値は0で、この場合は最初に選択された項目のインデックスになります。

### <a name="badge-element"></a>バッジ要素

 [![](images/image15.png "バッジ要素")](images/image15.png#lightbox)

### <a name="float-element"></a>Float 要素

 [![](images/image16.png "Float 要素")](images/image16.png#lightbox)

### <a name="activity-element"></a>Activity 要素

 [![](images/image17.png "Activity 要素")](images/image17.png#lightbox)

### <a name="date-element"></a>Date 要素

 ![](images/image18.png "Date 要素")

DateElement に対応するセルが選択されると、次のような日付の選択が表示されます。

 [![](images/image19.png "DateElement に対応するセルが選択されると、日付の選択が表示されます。")](images/image19.png#lightbox)

### <a name="time-element"></a>Time 要素

 [![](images/image20.png "Time 要素")](images/image20.png#lightbox)

TimeElement に対応するセルが選択されると、次のようにタイムピッカーが表示されます。

 [![](images/image21.png "TimeElement に対応するセルが選択されると、タイムピッカーが表示されます。")](images/image21.png#lightbox)

### <a name="datetime-element"></a>DateTime 要素

 [![](images/image22.png "DateTime 要素")](images/image22.png#lightbox)

DateTimeElement に対応するセルが選択されると、次のように datetime ピッカーが表示されます。

 [![](images/image23.png "DateTimeElement に対応するセルが選択されると、datetime ピッカーが表示されます。")](images/image23.png#lightbox)

### <a name="html-element"></a>HTML 要素

 [![](images/image24.png "HTML 要素")](images/image24.png#lightbox)

は`HTMLElement` 、テーブルセルにその`Caption`プロパティの値を表示します。 選択されている`Url`場合、要素に割り当てられた`UIWebView`は次のようにコントロールに読み込まれます。

 [![](images/image25.png "選択されている場合、要素に割り当てられた Url は、次に示すように UIWebView コントロールに読み込まれます。")](images/image25.png#lightbox)

### <a name="message-element"></a>Message 要素

 [![](images/image26.png "Message 要素")](images/image26.png#lightbox)

### <a name="load-more-element"></a>さらに要素を読み込む

この要素を使用して、ユーザーがリスト内の項目をさらに読み込むことができるようにします。 フォントとテキストの色だけでなく、通常のキャプションや読み込みキャプションをカスタマイズすることもできます。
インジケーター `UIActivity`はアニメーション化を開始し、ユーザーがセルをタップすると読み込みキャプションが表示され`NSAction` 、コンストラクターに渡されたが実行されます。 の`NSAction`コードが完了すると、インジケーターは`UIActivity`アニメーションを停止し、通常のキャプションが再び表示されます。

### <a name="uiview-element"></a>UIView 要素

また、カスタム`UIView`はを`UIViewElement`使用して表示できます。

### <a name="owner-drawn-element"></a>オーナー描画要素

この要素は抽象クラスであるため、サブクラス化する必要があります。 `Height(RectangleF bounds)` 要素`Draw(RectangleF bounds, CGContext context, UIView view)`の高さを返すメソッドをオーバーライドする必要があります。また、コンテキストとビューのパラメーターを使用して、特定の境界内でカスタマイズされたすべての描画を行う必要があります。 この要素は、を`UIView`サブクラス化し、返されるセルに配置して、2つの単純なオーバーライドを実装するだけで済みます。 `DemoOwnerDrawnElement.cs`ファイルのサンプルアプリで、より適切なサンプル実装を確認できます。

クラスを実装する非常に簡単な例を次に示します。

```csharp
public class SampleOwnerDrawnElement : OwnerDrawnElement
{
    public SampleOwnerDrawnElement (string text) : base(UITableViewCellStyle.Default, "sampleOwnerDrawnElement")
    {
        this.Text = text;
    }

    public string Text { get; set; }

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

は、を`RootElement` `JsonElement` 拡張して、入れ子になった子の内容をローカルまたはリモートのurlから読み込むことができるようにするの`RootElement`サブクラスです。

`JsonElement` は、2つの形式で`RootElement`インスタンス化できるです。 1つのバージョン`RootElement`では、必要に応じてコンテンツを読み込むが作成されます。 これらは、末尾に追加`JsonElement`の引数を受け取るコンストラクターを使用して作成されます。これは、コンテンツの読み込み元の url です。

```csharp
var je = new JsonElement ("Dynamic Data", "https://tirania.org/tmp/demo.json");
```

もう1つの形式では、ローカルファイルまたは既`System.Json.JsonObject`に解析済みの既存のデータからデータが作成されます。

```csharp
var je = JsonElement.FromFile ("json.sample");
using (var reader = File.OpenRead ("json.sample"))
    return JsonElement.FromJson (JsonObject.Load (reader) as JsonObject, arg);
```

MT で JSON を使用する方法の詳細については、「」を参照してください。D 「 [JSON 要素のチュートリアル](http://docs.xamarin.com/guides/ios/user_interface/monotouch.dialog/json_element_walkthrough)」を参照してください。

## <a name="other-features"></a>その他の機能

### <a name="pull-to-refresh-support"></a>プルから更新への対応

 *プル*から*更新*は、 *Tweetie2*アプリで最初に見られた視覚効果であり、多くのアプリケーションで一般的な効果がありました。

自動的な自動更新サポートをダイアログに追加するには、次の2つの操作を実行するだけです。ユーザーがデータを取得したときに通知される`DialogViewController`ようにイベントハンドラーをフックし、データが読み込まれたときにに通知して、既定の状態に戻ります。

通知をフックするのは簡単です。次のように`RefreshRequested` 、 `DialogViewController`でイベントに接続するだけです。

```csharp
dvc.RefreshRequested += OnUserRequestedRefresh;
```

次に、メソッド`OnUserRequestedRefresh`で、いくつかのデータの読み込みをキューに入れたり、データをネットワークから要求したり、スレッドをスピンしてデータを計算したりします。 データが読み込まれたら、新しいデータが存在`DialogViewController`することを通知し、ビューを既定の状態に復元するには、次のようにを`ReloadComplete`呼び出します。

```csharp
dvc.ReloadComplete ();
```

### <a name="search-support"></a>サポートの検索

検索をサポートするには`EnableSearch` 、 `DialogViewController`でプロパティを設定します。 検索バーで透かしテキスト`SearchPlaceholder`として使用するプロパティを設定することもできます。

検索では、ユーザーの入力に応じてビューの内容が変更されます。 表示されているフィールドを検索し、ユーザーに表示します。 は`DialogViewController` 、結果に対して新しいフィルター操作をプログラムで開始、終了、またはトリガーする3つのメソッドを公開します。 これらのメソッドを次に示します。

- `StartSearch`
- `FinishSearch`
- `PerformFilter`


システムは拡張可能なので、必要に応じてこの動作を変更できます。

### <a name="background-image-loading"></a>背景画像の読み込み

Monotouch.dialog には、 [TweetStation](https://github.com/migueldeicaza/TweetStation)アプリケーションのイメージローダーが組み込まれています。 このイメージローダーを使用すると、イメージをバックグラウンドで読み込み、キャッシュをサポートし、イメージが読み込まれたときにコードに通知することができます。

また、発信ネットワーク接続の数も制限されます。

イメージローダーは`ImageLoader`クラスに実装されています。 `DefaultRequestImage`メソッドを呼び出す必要があるのは、読み込むイメージの Uri と、イメージの ha 時に呼び出される`IImageUpdated`インターフェイスのインスタンスを指定することだけです。が読み込まれました。

たとえば、次のコードでは、Url `BadgeElement`からにイメージを読み込みます。

```csharp
string uriString = "http://some-server.com/some image url";

var rootElement = new RootElement("Image Loader") {
    new Section() {
        new BadgeElement( ImageLoader.DefaultRequestImage( new Uri(uriString), this), "Xamarin")
    }
};
```

ImageLoader クラスは、メモリに現在キャッシュされているすべてのイメージを解放するときに呼び出すことができる Purge メソッドを公開します。 現在のコードには、50イメージのキャッシュがあります。 別のキャッシュサイズを使用する場合 (たとえば、イメージが大きすぎて50イメージが大きすぎると予想される場合) は、ImageLoader のインスタンスを作成し、キャッシュに保持するイメージの数を渡すだけで済みます。

## <a name="using-linq-to-create-element-hierarchy"></a>LINQ を使用して要素階層を作成する

LINQ とC#の初期化構文の使い方により、linq を使用して要素階層を作成できます。 たとえば、次のコードでは、いくつかの文字列配列から画面を作成し、それぞれ`StringElement`に渡される匿名関数によってセルの選択を処理します。

```csharp
var rootElement = new RootElement ("LINQ root element") {
    from x in new string [] { "one", "two", "three" }
    select new Section (x) {
        from y in "Hello:World".Split (':')
        select (Element) new StringElement (y, delegate { Debug.WriteLine("cell tapped"); })
    }
};
```

これにより、XML データストアやデータベースのデータと簡単に組み合わせて、複雑なアプリケーションをデータからほぼ完全に作成することができます。

## <a name="extending-mtd"></a>MT を拡張しています。A

### <a name="creating-custom-elements"></a>カスタム要素の作成

既存の要素を継承するか、ルートクラス要素から派生することによって、独自の要素を作成できます。

独自の要素を作成するには、次のメソッドをオーバーライドします。

```csharp
// To release any heavy resources that you might have
void Dispose (bool disposing);

// To retrieve the UITableViewCell for your element
// you would need to prepare the cell to be reused, in the
// same way that UITableView expects reusable cells to work
UITableViewCell GetCell (UITableView tv);

// To retrieve a "summary" that can be used with
// a root element to render a summary one level up.  
string Summary ();

// To detect when the user has tapped on the cell
void Selected (DialogViewController dvc, UITableView tableView, NSIndexPath path);

// If you support search, to probe if the cell matches the user input
bool Matches (string text);
```

要素のサイズが可変の場合は、1つのメソッドを`IElementSizing`含むインターフェイスを実装する必要があります。

```csharp
// Returns the height for the cell at indexPath.Section, indexPath.Row
float GetHeight (UITableView tableView, NSIndexPath indexPath);
```

を呼び出し`GetCell` `base.GetCell(tv)` 、返されたセルをカスタマイズすることによってメソッドを実装する予定がある場合`CellKey`は、次のように、プロパティをオーバーライドして、要素に対して一意のキーを返す必要があります。

```csharp
static NSString MyKey = new NSString ("MyKey");
protected override NSString CellKey {
    get {
        return MyKey;
    }
}
```

これはほとんどの要素に対して機能し`StringElement`ます`StyledStringElement`が、とでは使用できません。また、さまざまなレンダリングシナリオで独自のキーセットを使用します。 これらのクラスでコードをレプリケートする必要があります。

### <a name="dialogviewcontrollers-dvcs"></a>コントロールビューコントローラー (DVCs)

リフレクションと Elements API は、どちらも同じ`DialogViewController`を使用します。 場合によっては、ビューの外観をカスタマイズしたり、ui の基本的な作成を超える`UITableViewController`の機能を使用したりすることが必要になる場合があります。

はのサブクラス`UITableViewController`であるだけで、を`UITableViewController`カスタマイズするのと同じ方法でカスタマイズできます。 `DialogViewController`

たとえば、リストスタイルをまたは`Grouped` `Plain`のいずれかに変更する場合は、コントローラーを作成するときにプロパティを変更することによって、次のようにこの値を設定できます。

```csharp
var myController = new DialogViewController (root, true) {
    Style = UITableViewStyle.Grouped;
}
```

の背景を設定するなど`DialogViewController`、の高度なカスタマイズについては、次の例に示すように、それをサブクラス化し、適切なメソッドをオーバーライドします。

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

別のカスタマイズポイントは、の`DialogViewController`次の仮想メソッドです。

```csharp
public override Source CreateSizingSource (bool unevenRows)
```

このメソッドは、セルが均等`DialogViewController.Source`にサイズ設定されている場合はのサブクラスを`DialogViewController.SizingSource`返し、セルが不均一である場合はのサブクラスを返します。

このオーバーライドを使用すると、 `UITableViewSource`任意のメソッドをキャプチャできます。 たとえば、 [TweetStation](https://github.com/migueldeicaza/TweetStation)はこれを使用して、ユーザーが一番上にスクロールした時間を追跡し、それに応じて未読のツイートの数を更新します。

## <a name="validation"></a>検証

Web ページおよびデスクトップアプリケーションに適したモデルは、iPhone の相互作用モデルに直接マップされないため、要素は検証自体を提供しません。

データの検証を行う場合は、ユーザーが入力されたデータを使用してアクションをトリガーするときに、この操作を行う必要があります。 たとえば、上部のツールバーの **[完了]** または **[次へ**] ボタン、または次のステージに進むボタンとして使用されているものがあり`StringElement`ます。

ここでは、基本的な入力検証を実行し、サーバーとのユーザー/パスワードの組み合わせの有効性を確認するなど、より複雑な検証を実行します。

ユーザーにエラーを通知する方法は、アプリケーション固有です。 ポップアップを`UIAlertView`表示したり、ヒントを表示したりできます。

## <a name="summary"></a>まとめ

この記事では、Monotouch.dialog に関するさまざまな情報について説明します。 ここでは、MT の基礎について説明しました。D は、MT を構成するさまざまなコンポーネントを対象としています。A. また、MT によってサポートされるさまざまな要素とテーブルのカスタマイズについても説明しました。D では、MT について説明しました。D はカスタム要素で拡張できます。 さらに、MT での JSON サポートについても説明しました。JSON から動的に要素を作成できる D。


## <a name="related-links"></a>関連リンク

- [チュートリアル: 要素 API を使用したアプリケーションの作成](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [チュートリアル: リフレクション API を使用したアプリケーションの作成](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [チュートリアル: JSON 要素を使用してユーザーインターフェイスを作成する](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Monotouch.dialog JSON マークアップ](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github の Monotouch.dialog ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [UITableViewController クラスのリファレンス](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスの参照](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
