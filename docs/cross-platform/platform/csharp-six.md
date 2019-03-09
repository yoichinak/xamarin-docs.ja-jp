---
title: C#6 の新機能の概要
description: 最新バージョンのC#言語 – バージョン 6 – は少ない定型、明確さの向上、および一貫性のある言語の進化を続けています。 クリーナーの初期化構文を使用する機能は、catch、finally ブロック、および null 条件で待機しますか。 演算子は、特に便利です。
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.custom: xamu-video
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: d5478a09c461ec8f1bf51efaa7b4dc2f862d69b4
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668941"
---
# <a name="c-6-new-features-overview"></a>C#6 の新機能の概要

_最新バージョンのC#言語 – バージョン 6 – は少ない定型、明確さの向上、および一貫性のある言語の進化を続けています。クリーナーの初期化構文を使用する機能は、catch、finally ブロック、および null 条件で待機しますか。演算子は、特に便利です。_

このドキュメントの新しい機能を紹介するC#6。 Mono コンパイラで完全にサポートされているし、開発者が Xamarin のすべてのターゲット プラットフォームでの新機能の使用を開始することができます。

この記事には簡単なスニペットが含まれています、C#基本的な使用方法を説明する 6 コード。
サンプル アプリケーションは、すべての Xamarin のターゲット プラットフォームで実行され、さまざまな機能を実行するコマンド ライン プログラムです。


> [!VIDEO https://youtube.com/embed/7UdV7zGPfMU]

**新C#6 により、 [Xamarin University](https://university.xamarin.com/)**


## <a name="development-environment"></a>開発環境

### <a name="mac"></a>Mac

* **Visual Studio for Mac**はサポートしていますC#6: ビルドしてコンパイルを使用して Xamarin アプリC#6 の機能です。
  詳細をご覧ください[Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/)します。

### <a name="windows"></a>Windows

* **Visual Studio 2015 と 2017**とは、完全なサポートが上記のC#6。 Visual Studio の以前のバージョンはサポートされませんC#6。

* **Xamarin Studio for Windows**はサポートしていませんC#6 エディター機能。



## <a name="compiler"></a>コンパイラ

Mono C# 6 のコンパイラは、これは Mono 4.0 以降に含まれている[無償でダウンロードできる](https://www.mono-project.com/download/)します。
Visual Studio for Mac は、システム上の Mono のインストールを自動的に更新します。

Windows ユーザー必要があります[Visual Studio 2015 または 2017 ^](https://visualstudio.microsoft.com/)コンパイルにインストールされているC#6 コード (お使いの IDE として Xamarin Studio for Windows を選択する) 場合でもです。

^ または*[Microsoft Build Tools 2015](https://www.microsoft.com/download/details.aspx?id=48159)* コマンド ライン コンパイルやビルド サーバー、たとえばします。

## <a name="using-c-6"></a>使用してC#6

C# 6 のコンパイラを使用ですべての最新バージョンの Visual Studio for mac。
コマンド ライン コンパイラを使用することを確認する必要があります`mcs --version`4.0 以上を返します。
Visual Studio for Mac ユーザーが参照することでインストールされている Mono 4 (またはそれ以降) がある場合を確認することができます**について Visual Studio for Mac > Visual Studio for Mac > 詳細の表示**します。

## <a name="less-boilerplate"></a>以下の定型コード
### <a name="using-static"></a>using static
列挙型、および特定のクラスなど`System.Math`は静的な値と関数の所有者では主にします。 C# 6、1 つの型のすべての静的メンバーをインポートする`using static`ステートメント。 典型的な三角関数の比較C#5 とC#6。

```csharp
// Classic C#
class MyClass
{
    public static Tuple<double,double> SolarAngleOld(double latitude, double declination, double hourAngle)
    {
        var tmp = Math.Sin (latitude) * Math.Sin (declination) + Math.Cos (latitude) * Math.Cos (declination) * Math.Cos (hourAngle);
        return Tuple.Create (Math.Asin (tmp), Math.Acos (tmp));
    }
}

// C# 6
using static System.Math;

class MyClass
{
    public static Tuple<double, double> SolarAngleNew(double latitude, double declination, double hourAngle)
    {
        var tmp = Asin (latitude) * Sin (declination) + Cos (latitude) * Cos (declination) * Cos (hourAngle);
        return Tuple.Create (Asin (tmp), Acos (tmp));
    }
}
```

`using static` 公開は行いません`const`などのフィールド`Math.PI`と`Math.E`、直接アクセス。

```csharp
for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... 
//PI is const, not static, so requires Math.PI
```

### <a name="using-static-with-extension-methods"></a>拡張メソッドと静的な using

`using static`機能が拡張メソッドで少し異なる方法で動作します。 拡張メソッドを使用して記述されています`static`、操作対象のインスタンスがない意味はありません。 したがって`using static`拡張メソッドでは、ターゲットの種類を使用可能になる拡張メソッドを定義する型を使用 (メソッドの`this`型)。 たとえば、`using static System.Linq.Enumerable`の API を拡張するために使用できる`IEnumerable<T>`せずにすべての LINQ の種類のオブジェクト。

```csharp
using static System.Linq.Enumerable;
using static System.String;

class Program
{
    static void Main()
    {
        var values = new int[] { 1, 2, 3, 4 };
        var evenValues = values.Where (i => i % 2 == 0);
        System.Console.WriteLine (Join(",", evenValues));
    }
}
```

前の例では、動作の違い: 拡張メソッド`Enumerable.Where`静的メソッドの中に、配列に関連付けられた`String.Join`への参照なしで呼び出される、`String`型。

### <a name="nameof-expressions"></a>nameof 式
参照すると場合によっては、名前には、変数またはフィールドを指定しました。 C# 6、`nameof(someVariableOrFieldOrType)`は、文字列を返します`"someVariableOrFieldOrType"`します。 たとえば、スローするときに、`ArgumentException`するどの引数が無効の名前を指定することが非常には。

```csharp
throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))
```

主な利点`nameof`式は型がチェックされるは、リファクタリング ツールを利用したと互換性があること。 型チェック`nameof`式は状況で特にへようこそ で、`string`型を動的に関連付けるために使用します。 たとえば、iOS で、`string`プロトタイプを使用する型を指定するために使用`UITableViewCell`内のオブジェクト、`UITableView`します。 `nameof` この関連付けは、スペルミスまたはずさんなリファクタリングのため失敗しないを保証できます。

```csharp
public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
{
    var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
    cell.TextLabel.Text = objects [indexPath.Row].ToString ();
    return cell;
}
```

修飾名を渡すことができますが`nameof`、最後の要素のみ (最後後`.`) が返されます。 たとえば、Xamarin.Forms でのデータ バインディングを追加できます。

```csharp
var myReactiveInstance = new ReactiveType ();
var myLabelOld.BindingContext = myReactiveInstance;
var myLabelNew.BindingContext = myReactiveInstance;
var myLabelOld.SetBinding (Label.TextProperty, "StringField");
var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));
```

2 回の呼び出し`SetBinding`同一の値を渡している:`nameof(ReactiveType.StringField)`は`"StringField"`ではなく、`"ReactiveType.StringField"`最初に予想どおりです。

## <a name="null-conditional-operator"></a>Null 条件演算子
前に更新C#null 許容型と null 合体演算子の概念を導入`??`null 許容の値を処理するときに、定型コードの量を削減します。 C#6 は、「null 条件演算子」では、このテーマを引き続き`?.`します。 オブジェクトがない場合、null 条件演算子がメンバー値を返します、式の右側にあるオブジェクトで使用すると`null`と`null`それ以外の場合。

```csharp
var ss = new string[] { "Foo", null };
var length0 = ss [0]?.Length; // 3
var length1 = ss [1]?.Length; // null
var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]
```

(どちらも`length0`と`length1`型であると推論される`int?`)

前の例の最後の行、 `?` null 条件演算子と組み合わせて、 `??` null 合体演算子。 新しいC#6 の null 条件演算子を返します`null`この時点で、null 合体演算子が開始され、提供する場合は 0、配列内の 2 番目の要素で、 `lengths` (適切なまたはできませんが、もちろん、かどうかの配列。問題に固有)。

Null 条件演算子は、定型"null"検査必要に応じて、多くのアプリケーションでの量を減らす大幅する必要があります。

Null 条件演算子のあいまいさのためにいくつかの制限があります。 直後にすることはできません、`?`かっこで囲まれた引数のリストを使用する場合がありますしたいと考えてデリゲートでの操作を行います。

```csharp
SomeDelegate?("Some Argument") // Not allowed
```

ただし、`Invoke`を分離するために使用できる、`?`引数リストから経由ではまだ改善し、 `null`-定型コードのブロックをチェックします。

```csharp
public event EventHandler HandoffOccurred;
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{
    HandoffOccurred?.Invoke (this, userActivity.UserInfo);
    return true;
}
```

## <a name="string-interpolation"></a>文字列補間
`String.Format`関数が従来のインデックスとして使用、書式指定文字列内のプレース ホルダーなど`String.Format("Expected: {0} Received: {1}.", expected, received`)。 もちろん、新しい値を追加することが常に行われて引数までカウントする、プレース ホルダーを再割り当て、および引数リストで適切な順序で、新しい引数を挿入することの面倒な場合の小さなタスク。

C#6 の新しい文字列補間機能が大幅に改良`String.Format`します。 付いて文字列内の変数を直接名前を次に、`$`します。 たとえば、次のようになります。

```csharp
$"Expected: {expected} Received: {received}."
```

変数は、チェック、およびスペル ミスや可用性ではない変数には、コンパイラ エラーが発生します。

プレース ホルダーは、単純な変数にする必要はありません、任意の式になることができます。 これらのプレース ホルダー内には、引用符を使用することができます*せず*これらの引用符をエスケープします。 たとえばに注意してください、`"s"`次で。

```csharp
var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"
```

文字列補間の構文を書式設定と配置をサポートする`String.Format`します。 同じですが書いた以前`{index, alignment:format}`のC#を記述する 6 `{placeholder, alignment:format}`:

```csharp
using static System.Linq.Enumerable;
using System;

class Program
{
    static void Main ()
    {
        var values = new int[] { 1, 2, 3, 4, 12, 123456 };
        foreach (var s in values.Select (i => $"The value is { i,10:N2}.")) {
            Console.WriteLine (s);
        }
    Console.WriteLine ($"Minimum is { values.Min(i => i):N2}.");
    }
}
```
結果:

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

文字列補間は糖衣構文の`String.Format`: では使用できません`@""`文字列リテラルと互換性がない`const`プレース ホルダーを使用しない場合でも。

```csharp
const string s = $"Foo"; //Error : const requires value
```

関数の引数文字列補間を構築するための一般的なユース ケースで引き続きエスケープ、エンコード、およびカルチャの問題について注意する必要があります。 SQL と URL のクエリは、もちろん、重要の不要部分です。 同様`String.Format`、文字列補間は、`CultureInfo.CurrentCulture`します。 使用して`CultureInfo.InvariantCulture`が少し冗長な。

```csharp
Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"
```

## <a name="initialization"></a>初期化

C#6 では、さまざまなプロパティ、フィールド、およびメンバーを指定する簡潔な方法を提供します。

### <a name="auto-property-initialization"></a>自動プロパティ初期化

フィールドと同じ簡潔な方法で、自動プロパティを初期化ようになりましたことができます。 自動の変更できないプロパティに getter のみを記述できます。

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
```

コンス トラクターでは、get アクセス操作子のみの自動-プロパティの値を設定できます。

```csharp
class ToDo
{
    public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
    public DateTime Created { get; } = DateTime.Now;
    public string Description { get; }

    public ToDo (string description)
    {
        this.Description = description; //Can assign (only in constructor!)
    }
```

自動プロパティのこの初期化は、一般的な領域を節約して機能し、それらのオブジェクトの不変性を強調したい開発者にとってメリットがあります。

### <a name="index-initializers"></a>インデックス初期化子

C#6 には、インデクサーの種類のキーと値の両方を設定することはインデックスの初期化子が導入されています。 通常、これには`Dictionary`-データ構造のスタイルを設定します。

```csharp
partial void ActivateHandoffClicked (WatchKit.WKInterfaceButton sender)
{
    var userInfo = new NSMutableDictionary {
        ["Created"] = NSDate.Now,
        ["Due"] = NSDate.Now.AddSeconds(60 * 60 * 24),
        ["Task"] = Description
    };
    UpdateUserActivity ("com.xamarin.ToDo.edit", userInfo, null);
    statusLabel.SetText ("Check phone");
}
```

### <a name="expression-bodied-function-members"></a>式形式の関数メンバー

ラムダ関数では、単純に領域の節約うちの 1 つは、いくつかの利点があります。 同様に、式形式のクラスのメンバーがいた以前のバージョンのよりも、もう少し簡潔に表現する小規模関数を許可C#6。

式形式の関数メンバーは、従来のブロックの構文ではなく、ラムダ矢印構文を使用します。

```csharp
public override string ToString () => $"{FirstName} {LastName}";
```

ラムダ矢印構文が使用しないことを明示的に注意してください`return`します。 関数が返す`void`式は、ステートメントにもあります。

```csharp
public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");
```

式形式メンバーをルールも影響を受けますを`async`メソッドがプロパティではなくすることは。

```csharp
//A method, so async is valid
public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
//The following property will not compile
public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;
```

## <a name="exceptions"></a>例外

ない 2 つの方法について: 例外処理はすぐに困難です。 新機能C#6 が柔軟かつ一貫性のある、例外処理を作成します。

### <a name="exception-filters"></a>例外フィルター

異常な状況は、定義上、例外が発生して、理由やコードに関する非常に困難な場合もできる*すべて*方法の特定の種類の例外が発生する可能性があります。 C#6 には、ランタイムで評価されるフィルターを使用して、実行ハンドラーを保護する機能が導入されています。 これは、追加することで、`when (bool)`後、通常のパターン`catch(ExceptionType)`宣言します。 次に、フィルターを区別に関連する解析エラー、`date`他の解析エラーではなくパラメーター。

```csharp
public void ExceptionFilters(string aFloat, string date, string anInt)
{
    try
    {
        var f = Double.Parse(aFloat);
        var d = DateTime.Parse(date);
        var n = Int32.Parse(anInt);
    } catch (FormatException e) when (e.Message.IndexOf("DateTime") > -1) {
        Console.WriteLine ($"Problem parsing \"{nameof(date)}\" argument");
    } catch (FormatException x) {
        Console.WriteLine ("Problem parsing some other argument");
    }
}
```

### <a name="await-in-catchfinally"></a>catch... await 最後にしています.

`async`に導入された機能C#5 は、言語のゲーム チェンジャーをされています。 C# 5、`await`で許可されませんでした`catch`と`finally`ブロック、面倒な作業の値を指定した、`async/await`機能します。 C#6 は、非同期の結果を次のスニペットに示すように、このプログラムを通じて一貫して待機することができます、この制限を削除します。

```csharp
async void SomeMethod()
{
    try {
        //...etc...
    } catch (Exception x) {
        var diagnosticData = await GenerateDiagnosticsAsync (x);
        Logger.log (diagnosticData);
    } finally {
        await someObject.FinalizeAsync ();
    }
}
```

## <a name="summary"></a>まとめ

C#言語では、中にも役立つベスト プラクティスを昇格して、ツールをサポートしている開発者の生産性を向上させるのに進化し続けています。 このドキュメントの新しい言語機能の概要を与えC#6 とその使用方法を簡単に説明しました。

## <a name="related-links"></a>関連リンク

- [新しい言語機能C#6](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)

