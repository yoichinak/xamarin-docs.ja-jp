---
title: "C# 6 の新機能の概要"
description: "C# 言語 – version 6 – の最新バージョンは、小さい定型、強化されたわかりやすくするため、および一貫性に言語を発展させるが続行されます。 クリーナーの初期化の構文を使用する機能は catch または finally ブロック、および null 条件で待機しますか。 演算子は、特に便利です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4B4E41A8-68BA-4E2B-9539-881AC19971B
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/22/2017
ms.openlocfilehash: 22b03c43509f3e3a55cd36ead5adef79c9086ba4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="c-6-new-features-overview"></a>C# 6 の新機能の概要

_C# 言語 – version 6 – の最新バージョンは、小さい定型、強化されたわかりやすくするため、および一貫性に言語を発展させるが続行されます。クリーナーの初期化の構文を使用する機能は catch または finally ブロック、および null 条件で待機しますか。演算子は、特に便利です。_

このドキュメントでは、C# 6 の新機能について説明します。 モノラル コンパイラによって完全にサポートされて、開発者は、Xamarin のすべてのターゲット プラットフォームでの新機能を使用して開始できます。

この記事には、C# 6 コードの基本的な使用方法を説明する簡単なスニペットが含まれています。
サンプル アプリケーションは、Xamarin のすべてのターゲット プラットフォームで実行し、さまざまな機能を実行するコマンド ライン プログラムです。

# <a name="requirements"></a>必要条件

## <a name="development-environment"></a>開発環境

### <a name="mac"></a>Mac

* **Visual Studio for Mac** C# 6 のサポート: ビルドして c# 6 の機能を使用した Xamarin アプリをコンパイルします。
  詳細について[Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/)です。

### <a name="windows"></a>Windows

* **Visual Studio 2015 および 2017**上 C# 6 の完全なサポートがあるとします。 以前のバージョンの Visual Studio (リフレッシュ レート。 2013、2012) C# 6 ではサポートされません。

* **Xamarin Studio for Windows**エディターで C# 6 の機能をサポートしていません。



## <a name="compiler"></a>コンパイラ

モノラル C# 6 コンパイラがこれはモノラル 4.0 以降に含まれる[無償でダウンロードできる](http://www.mono-project.com/download/)です。
Mac 用の visual Studio では、Mono でシステムをインストール、自動的に更新します。

Windows ユーザー必要があります[Visual Studio 2015 または 2017 ^](https://www.visualstudio.com/) (、IDE とは、Xamarin Studio for Windows を選択) 場合でも C# 6 コードのコンパイルにインストールします。

^ または *[Microsoft ビルド ツール 2015](http://www.microsoft.com/en-us/download/details.aspx?id=48159)* コマンド ラインのコンパイルまたはビルド サーバー、例を示します。

# <a name="using-c-6"></a>C# 6 を使用します。

For mac C# 6 コンパイラがすべての最新バージョンの Visual Studio で使用します。
コマンド ライン コンパイラを使用していることを確認する必要があります`mcs --version`4.0 以降を返します。
Mac のユーザー用の visual Studio が参照することでインストールされている Mono 4 (またはそれ以降) がある場合を確認することができます**に関する Visual Studio for Mac > Visual Studio for Mac > 詳細を表示する**です。

# <a name="less-boilerplate"></a>以下の定型句
## <a name="using-static"></a>using static
列挙型、および特定のクラスなど、 `System.Math`、静的な値と関数の所有者では、主には、します。 C# 6 で、1 つを含む型のすべての静的メンバーをインポートできます`using static`ステートメントです。 C# 5 および 6 c# での典型的な三角関数を比較します。

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

`using static` パブリック加えません`const`などのフィールド`Math.PI`と`Math.E`、直接アクセスできます。

    for (var angle = 0.0; angle <= Math.PI * 2.0; angle += Math.PI / 8) ... //PI is const, not static, so requires Math.PI

## <a name="using-static-with-extension-methods"></a>拡張メソッドで静的の使用

`using static`機能の動作は少し拡張メソッドを使用します。 拡張メソッドを使用して記述されていますが`static`、操作対象のインスタンスなし理にかなわない、します。 ため`using static`のターゲットの種類を使用可能になる拡張メソッドの拡張メソッドを定義する型を使用する (メソッドの`this`型)。 たとえば、`using static System.Linq.Enumerable`の API を拡張するために使用できる`IEnumerable<T>`なしですべての LINQ の種類のオブジェクト。

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

前の例を動作の違いを示しています: 拡張メソッド`Enumerable.Where`静的メソッドの中に、配列に関連付けられて`String.Join`への参照なしで呼び出される、`String`型です。

## <a name="nameof-expressions"></a>nameof 式
参照すると場合によっては、名前には、変数またはフィールドを与えられました。 C# 6 で、 `nameof(someVariableOrFieldOrType)` 、文字列が返されます`"someVariableOrFieldOrType"`です。 たとえば、スローするときに、`ArgumentException`しているか、非常にどの引数が無効の名前を指定します。

    throw new ArgumentException ("Problem with " + nameof(myInvalidArgument))

主な利点`nameof`式は、型チェックは、リファクタリング ツール電源と互換性があります。 型チェック`nameof`式は状況で特にへようこそ で、`string`型を動的に関連付けるために使用します。 たとえば、iOS で、`string`プロトタイプを使用する型の指定に使用される`UITableViewCell`内のオブジェクト、`UITableView`です。 `nameof` この関連付けはスペル ミスまたは粗雑なリファクタリングのため失敗しませんを保証できます。

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (nameof(CellTypeA), indexPath);
        cell.TextLabel.Text = objects [indexPath.Row].ToString ();
        return cell;
    }

修飾名を渡すことができますが`nameof`、最後の要素だけ (最後後`.`) が返されます。 たとえば、Xamarin.Forms でデータ バインディングを追加できます。

    var myReactiveInstance = new ReactiveType ();
    var myLabelOld.BindingContext = myReactiveInstance;
    var myLabelNew.BindingContext = myReactiveInstance;
    var myLabelOld.SetBinding (Label.TextProperty, "StringField");
    var myLabelNew.SetBinding (Label.TextProperty, nameof(ReactiveType.StringField));

2 回の呼び出し`SetBinding`同一の値を渡している:`nameof(ReactiveType.StringField)`は`"StringField"`ではなく、`"ReactiveType.StringField"`最初に想定される場合があります。

# <a name="null-conditional-operator"></a>Null 条件演算子
C# の以前の更新プログラムには null 許容型と null 合体演算子の概念が導入された`??`null 許容値を処理するときに定型コードの量を削減します。 C# 6 は、「null 条件演算子」を含むこのテーマを引き続き`?.`します。 Null 条件演算子が、オブジェクトがない場合に、メンバー値を返します式の右側にあるオブジェクトで使用すると`null`と`null`それ以外の場合。

    var ss = new string[] { "Foo", null };
    var length0 = ss [0]?.Length; // 3
    var length1 = ss [1]?.Length; // null
    var lengths = ss.Select (s => s?.Length ?? 0); //[3, 0]

(両方`length0`と`length1`型であると推論される`int?`)

最後の行に、前の例に示す、 `?` null 条件演算子と組み合わせて、 `??` null 合体演算子。 新しい C# 6 null 条件演算子を返します`null`時点 null 合体演算子が介入して提供する場合は 0、配列内の 2 番目の要素で、 `lengths` (するかどうかが該当するか、ありませんが、もちろん、特定の問題) の配列。

Null 条件演算子は、多くのアプリケーションでの定型的な null チェック必要量を減らす大幅必要があります。

Null 条件演算子のあいまいさのためにいくつかの制限があります。 直後にすることはできません、`?`かっこで囲まれた引数リストを使用したのと期待デリゲートを行うには。

    SomeDelegate?("Some Argument") // Not allowed

ただし、`Invoke`分離に使用できる、`?`引数リストからおよびがであるマーク付きの向上を`null`-定型のブロックを確認しています。

    public event EventHandler HandoffOccurred;
    public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
    {
        HandoffOccurred?.Invoke (this, userActivity.UserInfo);
        return true;
    }


# <a name="string-interpolation"></a>文字列補間
`String.Format`関数は従来のインデックスとして使用書式指定文字列内のプレース ホルダーなど`String.Format("Expected: {0} Received: {1}.", expected, received`)。 もちろん、新しい値を追加することが常に関係、面倒な場合ほとんどの引数をカウント、プレース ホルダー、アドレスの再設定し、タスクの引数リストの右側のシーケンスで、新しい引数を挿入します。

C# 6 の新しい文字列の補間機能が大幅に改良`String.Format`です。 付いて文字列内の変数を直接名前をここで、`$`です。 たとえば、次のようになります。

    $"Expected: {expected} Received: {received}."

変数がチェックされます、もちろん、およびスペル ミスや可用性ではない変数には、コンパイラ エラーが発生します。

プレース ホルダーは、単純な変数である必要はありません、任意の式があることができます。 これらのプレース ホルダー内では、引用符を使用することができます*せず*これらの引用符をエスケープします。 たとえば、注意してください、`"s"`次に。

    var s = $"Timestamp: {DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}"

配置と書式指定構文の文字列の補間をサポートしている`String.Format`です。 同様以前記述`{index, alignment:format}`、記述する c# 6 で`{placeholder, alignment:format}`:

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

結果:

    The value is       1.00.
    The value is       2.00.
    The value is       3.00.
    The value is       4.00.
    The value is      12.00.
    The value is 123,456.00.
    Minimum is 1.00.

文字列の補間は糖衣構文の`String.Format`: では使用できません`@""`文字列リテラルと互換性がない`const`プレース ホルダーを使用しない場合でも。

    const string s = $"Foo"; //Error : const requires value

文字列の補間と関数の引数の構築の一般的なユース ケース、まだエスケープ、エンコード、およびカルチャの問題について注意する必要があります。 SQL と URL のクエリ、もちろんに不可欠な不要部分を削除します。 同様に`String.Format`、文字列の補間使用して、`CultureInfo.CurrentCulture`です。 使用して`CultureInfo.InvariantCulture`少し好きは。

    Thread.CurrentThread.CurrentCulture  = new CultureInfo ("de");
    Console.WriteLine ($"Today is: {DateTime.Now}"); //"21.05.2015 13:52:51"
    Console.WriteLine ($"Today is: {DateTime.Now.ToString(CultureInfo.InvariantCulture)}"); //"05/21/2015 13:52:51"

# <a name="initialization"></a>初期化
C# 6 は、さまざまなプロパティ、フィールド、およびメンバーを指定する簡潔な方法を提供します。

## <a name="auto-property-initialization"></a>自動プロパティ初期化
フィールドと同じ簡潔な方法で、自動プロパティを初期化ようになりましたことができます。 自動の変更できないプロパティは、getter のみで記述できます。

    class ToDo
    {
        public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
        public DateTime Created { get; } = DateTime.Now;

コンス トラクターでは、get アクセス操作子専用の自動-プロパティの値を設定できます。

    class ToDo
    {
        public DateTime Due { get; set; } = DateTime.Now.AddDays(1);
        public DateTime Created { get; } = DateTime.Now;
        public string Description { get; }

        public ToDo (string description)
        {
           this.Description = description; //Can assign (only in constructor!)
        }

この自動プロパティ初期化は、一般的な領域を節約して機能とそれらのオブジェクトの不変性を強調する開発者にとって boon の両方がします。

## <a name="index-initializers"></a>インデックス初期化子
C# 6 には、インデクサーを持つ型のキーと値の両方を設定することはインデックスの初期化子が導入されています。 通常、これは`Dictionary`-データ構造体のスタイルを設定します。

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

## <a name="expression-bodied-function-members"></a>関数の式の本文メンバー
ラムダ関数では、領域の節約うちの 1 つは単に、いくつかの利点があります。 同様に、式の本文のクラスのメンバーは、小規模関数を指定することは、以前のバージョンの C# 6 よりも少し簡潔を許可します。

関数の式の本文メンバーは、従来のブロックの構文ではなく、ラムダの矢印構文を使用します。

      public override string ToString () => $"{FirstName} {LastName}";

ラムダの下向きの矢印構文が使用されないことが明示的に注意してください`return`です。 関数が返す`void`式は、ステートメントにもあります。

    public void Log(string message) => System.Console.WriteLine($"{DateTime.Now.ToString ("s", System.Globalization.CultureInfo.InvariantCulture )}: {message}");

メンバーの式の本文は、ルールされる可能性がありますを`async`メソッドがないプロパティのサポートします。

    //A method, so async is valid
    public async Task DelayInSeconds(int seconds) => await Task.Delay(seconds * 1000);
    //The following property will not compile
    public async Task<int> LeisureHours => await Task.FromResult<char> (DateTime.Now.DayOfWeek.ToString().First()) == 'S' ? 16 : 5;


# <a name="exceptions"></a>例外
ない 2 つの方法について: 例外処理は困難です。 C# 6 の新機能より柔軟で一貫性のある例外処理を作成します。

## <a name="exception-filters"></a>例外フィルター
異常な状況は、定義上、例外が発生して、理由やコードに関する非常に困難であることができます*すべて*方法は、特定の種類の例外が発生する可能性があります。 C# 6 には、ランタイムで評価されるフィルターを使用して、例外ハンドラーを保護する機能が導入されています。 これは、追加することで、`when (bool)`後、通常のパターン`catch(ExceptionType)`宣言します。 フィルターに関連する解析エラーを区別する、次に、`date`他の解析エラーではなくパラメーター。

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

## <a name="await-in-catchfinally"></a>... catch await 最後にしています.
`async` C# 5 で導入された機能は言語のゲーム チェンジャーをされています。 C# 5 で`await`では許可されませんでした`catch`と`finally`ブロック、面倒な作業の値を指定した、`async/await`機能します。 C# 6 は、次のスニペットに示すようにプログラムを一貫して待機する非同期の結果を許可するこのような制限を削除します。

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


# <a name="summary"></a>まとめ

C# 言語も優れた手法の昇格とツールをサポートしているときに開発者の生産性を向上させるのに進化し続けます。 このドキュメントでは、c# 6 で新しい言語機能の概要を与えの使用方法を簡単に説明しました。

## <a name="related-links"></a>関連リンク

- [C# 6 の新しい言語機能](https://github.com/dotnet/roslyn/wiki/New-Language-Features-in-C%23-6)
- [C# 6 を使用してブック](https://developer.xamarin.com/workbooks/csharp/csharp6/csharp6.workbook)
