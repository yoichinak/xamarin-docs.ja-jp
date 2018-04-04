---
title: イベント、プロトコルとデリゲート
description: 'この記事では、コールバックを受信して、ユーザー インターフェイス コントロールにデータを設定するために使用するキー iOS テクノロジを表示します。 これらのテクノロジは、イベント、プロトコル、およびデリゲートには。 この記事では、新機能について説明しますがこれらの各および c# から使用は、各方法です。 Xamarin.iOS が iOS コントロールを使用して、Xamarin.iOS Objective C の概念のプロトコルおよびデリゲートなどのサポートを提供する方法と、使い慣れた .NET イベントもを公開する方法を示しています (OBJECTIVE-C デリゲート混同しないように c# デリゲートを使用)。 この記事では、プロトコルの使用方法: Objective C のデリゲートは、およびデリゲート以外のシナリオでの基礎として両方を示す例も提供します。'
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 4c2888eb2d0b1ae79e10ca764e7bf14a1afb6c59
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="events-protocols-and-delegates"></a>イベント、プロトコルとデリゲート

_この記事では、コールバックを受信して、ユーザー インターフェイス コントロールにデータを設定するために使用するキー iOS テクノロジを表示します。これらのテクノロジは、イベント、プロトコル、およびデリゲートには。この記事では、新機能について説明しますがこれらの各および c# から使用は、各方法です。Xamarin.iOS が iOS コントロールを使用して、Xamarin.iOS Objective C の概念のプロトコルおよびデリゲートなどのサポートを提供する方法と、使い慣れた .NET イベントもを公開する方法を示しています (OBJECTIVE-C デリゲート混同しないように c# デリゲートを使用)。この記事では、プロトコルの使用方法: Objective C のデリゲートは、およびデリゲート以外のシナリオでの基礎として両方を示す例も提供します。_

Xamarin.iOS は、ほとんどのユーザーの操作のイベントを公開するのにコントロールを使用します。
Xamarin.iOS アプリケーションでは、従来の .NET アプリケーションのようにほぼ同じ方法でこれらのイベントを使用します。 たとえば、Xamarin.iOS UIButton クラスは、TouchUpInside と呼ばれるイベントを持っているし、.NET アプリでこのクラスとイベントが場合と同様に、このイベントを使用します。

この .NET 方法だけでなく、Xamarin.iOS はより複雑な相互作用およびデータ バインディングを使用できる別のモデルを公開します。 この方法は、Apple が代理人とプロトコルに呼び出しを使用します。 デリゲートは、C# の場合は、デリゲートを概念的に似ていますが、Objective C のデリゲートは、プロトコルに準拠しているクラス全体を定義し、1 つのメソッドを呼び出すの代わりにします。 プロトコルは、C# の場合は、インターフェイスに似ていますが、そのメソッドは省略可能にすることができます。 そのため、たとえばデータ UITableView を設定するには、デリゲートを実装するクラス自体への追加を呼び出して、UITableView UITableViewDataSource プロトコルで定義されているメソッドを作成します。

これらすべてのトピックについて学習します。 この記事で提供する強固な基盤 Xamarin.iOS、コールバックのシナリオを処理するためなど。

-  **イベント**– UIKit コントロールを使用して .NET イベント。
-  **プロトコル**– は、およびその使用方法、プロトコルの新機能を学習し、マップの注釈のデータを提供する例を作成します。
-  **デリゲート**– Objective C のデリゲートには、注釈を含むユーザーとの対話を処理するマップの例を拡張し、学習の強度のデリゲートとこれらの各を使用する場合の違いについて学習します。


プロトコルとバリアント汎用デリゲートを示すため、ここには、次に示すように、マップに注釈を追加する単純なマップ アプリケーションを構築します。

 [![](delegates-protocols-and-events-images/01-map.png "マップに注釈を追加する単純なマップ アプリケーションの一例")](delegates-protocols-and-events-images/01-map.png#lightbox) [ ![ ](delegates-protocols-and-events-images/04-annotation-with-callout.png "マップに追加された例注釈")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

このアプリへの取り組み、前に開始しましょう、UIKit の下にある .NET イベントを確認しています。

 <a name=".NET_Events_with_UIKit" />


## <a name="net-events-with-uikit"></a>UIKit と .NET イベント

Xamarin.iOS は、UIKit コントロールの .NET イベントを公開します。 たとえば、UIButton では、c# のラムダ式を使用する次のコードに示すように、.NET では、通常どおりに処理する TouchUpInside イベントがあります。

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

C# 2.0 スタイル匿名メソッドで次のようなこれを実装できます。

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

上記のコードは、UIViewContoller の ViewDidLoad メソッド内でのワイヤード (有線) します。 ボタンの変数は、iOS デザイナーまたはコードを追加する可能性があります ボタンを参照します。 次の図は、この記事に付属するサンプルから引用した iOS デザイナーに追加されるには、このボタンを示します。

 [![](delegates-protocols-and-events-images/02-interface-builder-outlet.png "IOS デザイナーで追加ボタン")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin.iOS には、ターゲット アクション スタイル コントロールで発生する相互作用に、コードを接続するのもサポートしています。 こんにちはボタンのターゲット アクションを作成するには、これをダブルクリックして、iOS デザイナーにします。 UIViewController の分離コード ファイルが表示され、開発者は、接続元のメソッドを挿入する場所を選択するように求められます。

 [![](delegates-protocols-and-events-images/03-interface-builder-action.png "UIViewControllers 分離コード ファイル")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

場所を選択すると、新しいメソッドが作成されコントロールにワイヤード (有線) アップします。 次の例では、メッセージが書き込まれますコンソールに、ボタンがクリックされたとき。

 [![](delegates-protocols-and-events-images/05-interface-builder-action.png "ボタンがクリックされたときにそのメッセージがコンソールに書き込まれます")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

IOS ターゲット アクションのパターンの詳細については、のターゲット アクションを参照してください" [iOS 用の核となるアプリケーション機能](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)"Apple の ios 開発者ライブラリです。

Xamarin.iOS で iOS デザイナーを使用する方法の詳細については、次を参照してください。、" [iOS デザイナー概要](~/ios/user-interface/designer/index.md)"ドキュメント。

 <a name="Events" />


## <a name="events"></a>イベント

さまざまなオプションがある場合は < からのイベントをインターセプトするには、: c# のラムダおよび低レベルの Objective C Api を使用するデリゲート関数を使用します。

次のセクションでは、どの程度の制御を必要に応じて、ボタンの接地イベントをキャプチャする方法を示しています。

 <a name="C#_Style" />


## <a name="c-style"></a>C# スタイル

デリゲートの構文を使用します。

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {    
    Console.WriteLine ("Touched");
};
```

場合はラムダ代わりにします。

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

する場合、複数のボタンは同じハンドラーを使用して、同じコードを共有します。

```csharp
void handler (object sender, EventArgs args)
{
   if (sender == button1)
      Console.WriteLine ("button1");
   else
      Console.WriteLine ("some other button");
}

button1.TouchDown += handler;
button2.TouchDown += handler;
```

<a name="Monitoring_more_than_one_kind_of_Event" />


## <a name="monitoring-more-than-one-kind-of-event"></a>1 つ以上の種類のイベントの監視

については、c# イベント UIControlEvent フラグでは、個々 のフラグに一対一のマッピングがあります。 2 つ以上のイベントを処理するコードの同じ部分がある場合、使用して、`UIControl.AddTarget`メソッド。

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

ラムダの構文を使用します。

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

場合は、特定のオブジェクトのインスタンスへのフックし、特定のセレクターを呼び出すことと同様に、OBJECTIVE-C の低レベルの機能を使用する必要があります。

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

注意してください、継承の基本クラスのインスタンス メソッドを実装する場合、パブリック メソッドである必要があります。

 <a name="Protocols" />


## <a name="protocols"></a>プロトコル

プロトコルは、メソッド宣言の一覧を提供する Objective C 言語機能です。 C# の場合、主な違いのインターフェイスに同様の目的は機能していること、プロトコルは省略可能なメソッドを持つことができますが向上します。 プロトコルを採用するクラスがそれらを実装しない場合、省略可能なメソッドは呼び出されません。 また、c# クラスが複数のインターフェイスを実装と同様、Objective C の単一のクラスは、複数のプロトコルを実装できます。

Apple は iOS 全体でのプロトコルを使用クラス c# インターフェイスと同じように動作してそのため、呼び出し元から実装するクラスを取り除く間を採用するのにためのコントラクトを定義します。 プロトコルが使用されます両方デリゲート以外のシナリオで (などで、`MKAnnotation`例を次に示す) とデリゲート (デリゲート セクションで、このドキュメントの後半で説明) とします。

 <a name="Protocols_with_Monotouch" />


### <a name="protocols-with-xamarinios"></a>Xamarin.ios とプロトコル

Xamarin.iOS から OBJECTIVE-C プロトコルを使用して例を見てをみましょう。 この例でを使用、`MKAnnotation`プロトコルで、一部の`MapKit`フレームワークです。 `MKAnnotation` マップに追加できる注釈についての情報を提供することを採用する任意のオブジェクトをできるようにするプロトコルです。 たとえば、実装するオブジェクト`MKAnnotation`注釈と関連付けられているタイトルの場所を提供します。

この方法で、`MKAnnotation`プロトコルを使用して注釈を伴う関連するデータを提供します。 注釈自体の実際のビューが採用するオブジェクトのデータから作成されて、`MKAnnotation`プロトコルです。 示すように次のスクリーン ショット) 注釈でユーザーをタップしたときに表示される引き出し線のテキストを取得するなど、`Title`プロトコルを実装するクラスのプロパティ。

 [![](delegates-protocols-and-events-images/04-annotation-with-callout.png "注釈で、ユーザーがタップしたときに使用する引き出しのテキストの例")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

次のセクションでは、プロトコル Deep Dive、」の説明に従って、Xamarin.iOS はプロトコルを抽象クラスにバインドします。 `MKAnnotation`プロトコル、バインドされている c# クラスの名前は`MKAnnotation`の名前、プロトコル、およびそれを模倣するためには、サブクラス`NSObject`CocoaTouch のルートの基本クラスです。 プロトコルは、getter と setter; 座標を実装する必要があります。ただし、タイトルとサブタイトルはオプションです。 したがって、`MKAnnotation`クラス、`Coordinate`プロパティは*抽象*、実装することを必要として`Title`と`Subtitle`プロパティがマークされている*仮想*、ように省略可能で、次のようにします。

```csharp
[Register ("MKAnnotation"), Model ]
public abstract class MKAnnotation : NSObject
{
    public abstract CLLocationCoordinate2D Coordinate
    {
        [Export ("coordinate")]
        get;
        [Export ("setCoordinate:")]
        set;
    }

    public virtual string Title
    {
        [Export ("title")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }

    public virtual string Subtitle
    {
        [Export ("subtitle")]
        get
        {
            throw new ModelNotImplementedException ();
        }
    }
...
}
```

任意のクラスだけから派生することによって注釈データを提供できます`MKAnnotation`少なくともいる限り、`Coordinate`プロパティを実装します。 たとえば、コンス トラクターで座標を取得しタイトルの文字列を返すサンプル クラスを次に示します。

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string _title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        _title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return _title;
        }
    }
}
```

バインドされているプロトコルでは、すべてのクラスをサブクラスとして持つ`MKAnnotation`注釈のビューを作成するときに、マップで使用される関連データを提供できます。 マップに注釈を追加するを呼び出すだけで、`AddAnnotation`のメソッド、`MKMapView`インスタンス、次のコードに示すように。

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
new CLLocationCoordinate2D (42.3467512, -71.0969456);

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

マップ変数をここではのインスタンスでは、 `MKMapView`、これは、マップ自体を表すクラス。 `MKMapView`を使用して、`Coordinate`から派生したデータ、`SampleMapAnnotation`マップにビューの注釈を配置するインスタンス。

`MKAnnotation`プロトコルは、既知のセットと、それを実装する任意のオブジェクトにまたがって機能せず、コンシューマー (ここではマップ) を必要とする実装の詳細について知ることです。 これは、さまざまな利用可能なコメントを追加するマップを簡略化します。

 <a name="Protocols_Deep_Dive" />


### <a name="protocols-deep-dive"></a>プロトコルの詳細

C# のインターフェイスが省略可能なメソッドをサポートしていないために、Xamarin.iOS はプロトコルを抽象クラスにマップします。 したがって、プロトコルにバインドされている抽象クラスから派生して、必要なメソッドを実装することで Xamarin.iOS で Objective C でプロトコルを採用することが実現されます。 これらのメソッドは、クラスの抽象メソッドとして公開されます。 プロトコルからのオプションのメソッドは、c# のクラスの仮想メソッドにバインドされます。

たとえばの一部をここでは、`UITableViewDataSource`にバインドされた Xamarin.iOS としてプロトコル。

```csharp
public abstract class UITableViewDataSource : NSObject
{
    [Export ("tableView:cellForRowAtIndexPath:")]
    public abstract UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath);
    [Export ("numberOfSectionsInTableView:")]
    public virtual int NumberOfSections (UITableView tableView){...}
...
}
```

抽象クラスに注意してください。 Xamarin.iOS は、プロトコルで/省略可能なために必要なメソッドをサポートする抽象クラスをします。
ただし、OBJECTIVE-C プロトコル (または c# のインターフェイス) とは異なり、c# クラスは多重継承をサポートしません。 これは、プロトコルを使用し、通常は入れ子になったクラスに c# コードの設計に影響します。 この問題に関するについては、デリゲートのセクションで、このドキュメントで後述します。

 `GetCell(…)` Objective C にバインドされている抽象メソッドは、*セレクター*、 `tableView:cellForRowAtIndexPath:`、メソッドは、必要なの`UITableViewDataSource`プロトコルです。 セレクターは、メソッド名、Objective C の用語です。 必要に応じてメソッドを適用するのに Xamarin.iOS は、それを抽象として宣言します。 その他のメソッドでは、`NumberOfSections(…)`にバインドされた`numberOfSectionsInTableview:`です。 このメソッドはプロトコルには、省略可能な Xamarin.iOS 宣言 virtual (C#) をオーバーライドする省略可能になります。

すべての iOS バインディング Xamarin.iOS 行われます。 ただし、Objective C からのプロトコルを手動でバインドする必要が生じた場合は、これを行うを持つクラスを修飾することによって、`ExportAttribute`です。 これは、Xamarin.iOS 自体によって使用される同じ方法です。

Xamarin.iOS で Objective C 型にバインドする方法の詳細については、記事を参照してください。 [Objective C 型のバインド](~/ios/platform/binding-objective-c/index.md)です。

私たちはありませんを通じてプロトコルでまだ、ただしです。 IOS の基礎として、使用 Objective C のデリゲートは、次のセクションのトピックです。

 <a name="Delegates" />


## <a name="delegates"></a>デリゲート

iOS では、OBJECTIVE-C デリゲートを使用して、1 つのオブジェクトの作業に渡します別、委任パターンを実装します。 作業を行っているオブジェクトは、最初のオブジェクトのデリゲートです。 オブジェクトは、特定の処理が行われます後にメッセージを送信して作業を実行するには、そのデリゲートを指示します。 Objective C のこのようなメッセージを送信するは C# の場合、メソッドの呼び出しに相当する機能です。 デリゲートは、これらの呼び出しに対する応答でメソッドを実装して、そのため、アプリケーションに機能を提供します。

デリゲートを使用すると、サブクラスを作成しなくてもクラスの動作を拡張できます。 1 つのクラスを呼び出して別、重要なアクションが発生した後と、iOS でのアプリケーションは多くの場合、デリゲートを使用します。 たとえば、`MKMapView`クラス、デリゲートにコールバックのユーザーがタップすると、マップ上注釈デリゲート クラスの作成者が、アプリケーション内で応答する機会を提供します。 この型のデリゲートの使用方法の例を使用して、この記事の後半の例では、Xamarin.iOS を持つデリゲートを操作できます。

この時点では、比較を行うクラスがそのデリゲートを呼び出すには、どのような方法を決定する方法です。 これは、プロトコルを使用する別の場所です。 通常は、デリゲートの利用方法は、採用するプロトコルから取得されます。

 <a name="How_Protocols_are_used_with_Delegates" />


### <a name="how-protocols-are-used-with-delegates"></a>デリゲートでプロトコルを使用する方法

プロトコルを使用して、マップに追加の注釈をサポートする以前方法を説明しました。
プロトコルは、既知の特定のイベントが発生すると、このようなユーザー タップ注釈でマップした後、またはテーブルのセルを選択した後に呼び出すクラスのメソッドのセットを提供するも使われます。 これらのメソッドを実装するクラスは、それらを呼び出すクラスのデリゲートと呼ばれます。

委任をサポートするクラスは、デリゲートを実装するクラスが割り当てられている、デリゲート プロパティを公開します。 デリゲートを実装するメソッドは、特定の委任を採用するプロトコルによって異なります。 `UITableView`メソッドを実装する、`UITableViewDelegate`プロトコルでの`UIAccelerometer`実装するメソッド、`UIAccelerometerDelegate`する対象の iOS 全体で他のクラス、デリゲートを公開するために、します。

`MKMapView`前述の例で説明しましたクラスを呼び出すデリゲートをという名前のプロパティがあります、さまざまなイベントが発生したときにします。 デリゲート`MKMapView`の種類は`MKMapViewDelegate`します。
これに使用が選択されている後に、注釈へ応答例が最初に間もなくみましょう強度のデリゲートの間の違いについて説明します。

 <a name="Strong_Delegates_vs._Weak_Delegates" />


### <a name="strong-delegates-vs-weak-delegates"></a>厳密なデリゲートとします。脆弱なデリゲート

これまで見てきたおデリゲートとは、強力なデリゲート、つまり、厳密に型指定です。 Xamarin.iOS バインディングは、iOS でのすべてのデリゲート プロトコルの厳密に型指定されたクラスに付属します。 ただし、iOS では、脆弱なデリゲートの概念もあります。 特定の委任について、OBJECTIVE-C プロトコルにバインドされているクラスをサブクラス化ではなく iOS も選択できますか、ExportAttribute でメソッドを装飾 NSObject から派生したクラスで自分でプロトコル メソッドをバインドし、適切なセレクターを指定します。
この方法を採用する場合は、プロパティのデリゲートの代わりに WeakDelegate プロパティに、クラスのインスタンスを割り当てます。 脆弱なデリゲートでは、別の継承階層の下位デリゲート クラスを実行する柔軟性を提供します。 強力なと弱いの両方のデリゲートを使用する Xamarin.iOS 例を見てみましょう。

 <a name="Example_using_a_Delegate_with_Xamarin.iOS" />


### <a name="example-using-a-delegate-with-xamarinios"></a>Xamarin.iOS でデリゲートの使用例

コードを実行するには、この例では、注釈をタップすると、ユーザーへの応答をサブクラスには`MKMapViewDelegate`にインスタンスを割り当てると、`MKMapView`の`Delegate`プロパティです。 `MKMapViewDelegate`プロトコルには、省略可能なメソッドのみが含まれています。
そのため、すべてのメソッドは仮想、Xamarin.iOS でこのプロトコルにバインドされる`MKMapViewDelegate`クラスです。 ユーザーは、注釈を選択したときに、`MKMapView`はインスタンスを送信、`mapView:didSelectAnnotationView:`その代理人にメッセージ。 Xamarin.iOS でこれを処理する必要がありますを上書きする、`DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)`メソッドに次のように MKMapViewDelegate サブクラス。

```csharp
public class SampleMapDelegate : MKMapViewDelegate
{
    public override void DidSelectAnnotationView (
        MKMapView mapView, MKAnnotationView annotationView)
    {
        var sampleAnnotation =
            annotationView.Annotation as SampleMapAnnotation;

        if (sampleAnnotation != null) {

            //demo accessing the coordinate of the selected annotation to
            //zoom in on it
            mapView.Region = MKCoordinateRegion.FromDistance(
                sampleAnnotation.Coordinate, 500, 500);

            //demo accessing the title of the selected annotation
            Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
        }
    }
}
```

前に示した SampleMapDelegate クラスが含むコント ローラー内の入れ子になったクラスとして実装されている、`MKMapView`インスタンス。 Objective c、多くの場合、クラス内で直接、複数のプロトコルを採用するコント ローラーが表示されます。 ただし、プロトコルは、Xamarin.iOS 内のクラスにバインドされているため、厳密に型指定されたデリゲートを実装するクラスが入れ子になったクラスとして通常含まれています。

インプレース デリゲート クラスの実装を使用するだけ済みますコント ローラー内のデリゲートのインスタンスをインスタンス化し、それを割り当てる、`MKMapView`の`Delegate`プロパティは、ここに示すようにします。

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    SampleMapDelegate _mapDelegate;
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();

        //set the map's delegate
        _mapDelegate = new SampleMapDelegate ();
        map.Delegate = _mapDelegate;
        ...
    }
    class SampleMapDelegate : MKMapViewDelegate
    {
        ...
    }
}
```

脆弱なデリゲートを使用すると、処理を行うから派生した任意のクラス、メソッドを自分でバインドする必要があります`NSObject`に割り当てると、`WeakDelegate`のプロパティ、`MKMapView`です。 以降、`UIViewController`クラスは、最終的にから派生`NSObject`Objective C のすべてのクラス CocoaTouch)、(にバインドされているメソッドを実装おできる単に`mapView:didSelectAnnotationView:`コント ローラーで直接にコント ローラーを割り当てると`MKMapView`の`WeakDelegate`、余分な入れ子になったクラスの必要性を回避します。 次のコードでは、この方法を示しています。

```csharp
public partial class Protocols_Delegates_EventsViewController : UIViewController
{
    ...
    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        //assign the controller directly to the weak delegate
        map.WeakDelegate = this;
    }
    //bind to the Objective-C selector mapView:didSelectAnnotationView:
    [Export("mapView:didSelectAnnotationView:")]
    public void DidSelectAnnotationView (MKMapView mapView,
        MKAnnotationView annotationView)
    {
        ...
    }
}
```

このコードを実行するときに、アプリケーションは、厳密に型指定されたデリゲートのバージョンを実行している場合とまったく同じ状態は動作します。 このコードの利点は、脆弱なデリゲートが厳密に型指定されたデリゲートを使用したときに作成された余分なクラスの作成を必要としないことです。 ただし、これは、タイプ セーフを犠牲にしてします。 間違いを犯すセレクターに渡されたにしたかどうか、 `ExportAttribute`、実行時まで調べるはありません。

 <a name="Events_and_Delegates" />


### <a name="events-and-delegates"></a>イベントとデリゲート

デリゲートは、.NET イベントの使用方法と同様に、iOS でのコールバックを使用します。 IOS に Api と OBJECTIVE-C デリゲートの使用方法のようにより .NET では、Xamarin.iOS が iOS でデリゲートが使用されているさまざまな場所での .NET イベントを公開します。

以前の実装など、場所、`MKMapViewDelegate`に応答した .NET イベントを使用して Xamarin.iOS で選択した注釈が実装も可能性があります。 イベントが定義する場合は、`MKMapView`と呼ばれると`DidSelectAnnotationView`です。 必要があります、`EventArgs`型のサブクラス`MKMapViewAnnotationEventsArgs`です。 `View`プロパティ`MKMapViewAnnotationEventsArgs`注釈ビューへの参照を提供するから同じ実装を行う可能性がありますが、ここに示したで示すように、前。

```csharp
map.DidSelectAnnotationView += (s,e) => {
    var sampleAnnotation = e.View.Annotation as SampleMapAnnotation;
    if (sampleAnnotation != null) {
        //demo accessing the coordinate of the selected annotation to
        //zoom in on it
        mapView.Region = MKCoordinateRegion.FromDistance (
            sampleAnnotation.Coordinate, 500, 500);

        //demo accessing the title of the selected annotation
        Console.WriteLine ("{0} was tapped", sampleAnnotation.Title);
    }
};
```

 <a name="Summary" />


## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS でイベント、プロトコル、およびデリゲートを使用する方法について説明します。 Xamarin.iOS がコントロールの通常の .NET スタイル イベントを公開する方法を説明しました。
次に Objective C などのプロトコルは c# のインターフェイスからさまざまな方法と Xamarin.iOS がそれらを使用する方法について説明しました。 最後に、Xamarin.iOS の観点から Objective C のデリゲートを調査します。 Xamarin.iOS 仕組み両方厳密と弱い型指定されたデリゲートと .NET イベント メソッドのデリゲートをバインドする方法を説明しました。


## <a name="related-links"></a>関連リンク

- [プロトコル、デリゲート、およびイベント (サンプル)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [Objective C 型のバインド](~/ios/platform/binding-objective-c/index.md)
- [Objective C プログラミング言語](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Xcode 4 でのユーザー インターフェイスの設計](http://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [IOS 用の核となるアプリケーションの機能](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
