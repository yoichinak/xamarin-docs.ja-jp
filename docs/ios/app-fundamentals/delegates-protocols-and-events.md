---
title: Xamarin. iOS のイベント、プロトコル、およびデリゲート
description: このドキュメントでは、Xamarin. iOS でイベント、プロトコル、およびデリゲートを操作する方法について説明します。 これらの基本的な概念は、Xamarin の iOS 開発で広く普及しています。
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/17/2017
ms.openlocfilehash: 1a7d7ec017bb226efb05014dc7ac80160aeaae48
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938361"
---
# <a name="events-protocols-and-delegates-in-xamarinios"></a>Xamarin. iOS のイベント、プロトコル、およびデリゲート

Xamarin iOS は、コントロールを使用して、ほとんどのユーザー操作のイベントを公開します。
Xamarin iOS アプリケーションは、従来の .NET アプリケーションとほぼ同じ方法でこれらのイベントを使用します。 たとえば、Xamarin. iOS UIButton クラスには TouchUpInside という名前のイベントがあり、このクラスとイベントが .NET アプリ内にある場合と同じように、このイベントを使用します。

この .NET アプローチに加えて、Xamarin では、より複雑な相互作用とデータバインディングに使用できる別のモデルが公開されています。 この方法では、Apple がデリゲートとプロトコルを呼び出します。 デリゲートは C# でのデリゲートと同じ概念に似ていますが、1つのメソッドを定義して呼び出すのではなく、目的の C のデリゲートは、プロトコルに準拠するクラス全体です。 プロトコルは C# のインターフェイスに似ていますが、メソッドは省略可能である点が異なります。 たとえば、UITableView にデータを設定するには、UITableViewDataSource プロトコルで定義されているメソッドを実装するデリゲートクラスを作成します。このメソッドは、UITableView がそれ自体を設定するために呼び出します。

この記事では、これらのすべてのトピックについて学習し、次のような Xamarin のコールバックシナリオを処理するための堅固な基盤を提供します。

- **イベント**– uikit コントロールでの .net イベントの使用。
- **プロトコル**–プロトコルの概要と使用方法、およびマップの注釈にデータを提供する例の作成について説明します。
- **デリゲート**–目的 C のデリゲートについて学習するには、注釈を含むユーザーの操作を処理するようにマップの例を拡張し、厳密なデリゲートと弱いデリゲートの違いを学習して、それぞれを使用するタイミングを学習します。

プロトコルとデリゲートについて説明するために、次に示すように、マップに注釈を追加する単純なマップアプリケーションを作成します。

マップに[ ![ 注釈を追加する単純なマップアプリケーションの例 (](delegates-protocols-and-events-images/01-map-sml.png)](delegates-protocols-and-events-images/01-map.png#lightbox) 
 [ ![ マップに追加された注釈の例](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png)](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox))

このアプリに取り組む前に、まず、UIKit の下にある .NET イベントを見てみましょう。

## <a name="net-events-with-uikit"></a>UIKit を使用した .NET イベント

Xamarin は、UIKit コントロールで .NET イベントを公開します。 たとえば、UIButton には TouchUpInside イベントがあります。これは、C# のラムダ式を使用する次のコードに示すように、通常の .NET の場合と同様に処理します。

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

これは、次のような C# 2.0 スタイルの匿名メソッドを使用して実装することもできます。

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

前のコードは、UIViewController のメソッドに接続されてい `ViewDidLoad` ます。 変数はボタンを参照します `aButton` 。このボタンは、IOS デザイナーまたはコードと共に追加できます。 次の図は、iOS デザイナーに追加されたボタンを示しています。

[![IOS Designer で追加されたボタン](delegates-protocols-and-events-images/02-interface-builder-outlet-sml.png)](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

また、Xamarin は、コントロールで発生する相互作用にコードを接続するためのターゲットアクションスタイルもサポートしています。 [ **Hello** ] ボタンのターゲットアクションを作成するには、iOS デザイナーでダブルクリックします。 UIViewController の分離コードファイルが表示され、開発者は接続方法を挿入する場所を選択するように求められます。

[![UIViewControllers 分離コードファイル](delegates-protocols-and-events-images/03-interface-builder-action-sml.png)](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

場所を選択すると、新しいメソッドが作成され、コントロールに接続されます。 次の例では、ボタンがクリックされると、メッセージがコンソールに書き込まれます。

[![ボタンがクリックされると、メッセージがコンソールに書き込まれます。](delegates-protocols-and-events-images/05-interface-builder-action-sml.png)](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

IOS ターゲットアクションパターンの詳細については、Apple の iOS Developer Library の「 [ios のコアアプリケーションコンピテンシー](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html) 」の「ターゲット-アクション」セクションを参照してください。

Xamarin で iOS Designer を使用する方法の詳細については、 [Ios designer の概要](~/ios/user-interface/designer/index.md)に関するドキュメントを参照してください。

## <a name="events"></a>events

UIControl からイベントをインターセプトする場合は、さまざまなオプションがあります。 C# ラムダとデリゲート関数を使用して、低レベルの目標 C Api を使用する方法です。

次のセクションでは、必要なコントロールの量に応じて、ボタンの着地イベントをキャプチャする方法を示します。

## <a name="c-style"></a>C# スタイル

デリゲート構文を使用する場合:

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {
    Console.WriteLine ("Touched");
};
```

代わりにラムダを使用する場合は、次のようにします。

```csharp
button.TouchDown += () => {
   Console.WriteLine ("Touched");
};
```

複数のボタンで同じハンドラーを使用して同じコードを共有する場合は、次のようにします。

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

## <a name="monitoring-more-than-one-kind-of-event"></a>複数の種類のイベントを監視する

UIControlEvent フラグの C# イベントには、個々のフラグへの一対一のマッピングがあります。 同じコードで2つ以上のイベントを処理する場合は、メソッドを使用し `UIControl.AddTarget` ます。

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

ラムダ構文を使用する場合:

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

特定のオブジェクトインスタンスへのフック、特定のセレクターの呼び出しなど、目標 C の下位レベルの機能を使用する必要がある場合は、次のようにします。

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

継承された基本クラスでインスタンスメソッドを実装する場合は、パブリックメソッドである必要があることに注意してください。

## <a name="protocols"></a>プロトコル

プロトコルは、メソッド宣言の一覧を提供する目的 C 言語の機能です。 これは、C# のインターフェイスと同様の目的を果たします。主な違いは、プロトコルがオプションのメソッドを持つことができることです。 プロトコルを採用するクラスが実装していない場合、省略可能なメソッドは呼び出されません。 また、C# のクラスで複数のインターフェイスを実装できるのと同じように、1つのクラスで複数のプロトコルを実装することもできます。

Apple では、iOS 全体でプロトコルを使用して、実装するクラスのコントラクトを定義し、呼び出し元から実装クラスを分離し、C# インターフェイスと同様に操作します。 非デリゲートのシナリオ ( `MKAnnotation` 次に示す例を参照) とデリゲート (このドキュメントで後述する「デリゲート」セクション) の両方で、プロトコルが使用されます。

### <a name="protocols-with-xamarinios"></a>Xamarin を使用したプロトコル

Xamarin. iOS の目的の C プロトコルを使用した例を見てみましょう。 この例で `MKAnnotation` は、フレームワークの一部であるプロトコルを使用し `MapKit` ます。 `MKAnnotation`は、これを採用する任意のオブジェクトが、マップに追加できる注釈に関する情報を提供できるようにするプロトコルです。 たとえば、を実装するオブジェクトは、 `MKAnnotation` 注釈の場所とそれに関連付けられているタイトルを提供します。

これにより、 `MKAnnotation` プロトコルが、注釈に付随する関連データを提供するために使用されます。 注釈自体の実際のビューは、プロトコルを採用するオブジェクトのデータから構築され `MKAnnotation` ます。 たとえば、次のスクリーンショットに示すように、ユーザーが注釈をタップしたときに表示される吹き出しのテキストは、 `Title` プロトコルを実装するクラスのプロパティから取得されます。

 [![ユーザーが注釈をタップしたときのコールアウトのテキストの例](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png)](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

次のセクション「[プロトコルの詳細](#protocols-deep-dive)」で説明されているように、Xamarin. iOS は、プロトコルを抽象クラスにバインドします。 プロトコルの場合 `MKAnnotation` 、バインドされた C# クラスには、 `MKAnnotation` プロトコルの名前を模倣する名前が付けられ `NSObject` ます。これは、CocoaTouch のルート基本クラスであるのサブクラスです。 このプロトコルでは、座標に対して getter と setter が実装されている必要があります。ただし、タイトルとサブタイトルは省略可能です。 したがって、 `MKAnnotation` クラスでは、 `Coordinate` プロパティは*abstract*であり、実装する必要があり `Title` `Subtitle` ます。次に示すように、プロパティとプロパティは*virtual*に設定され、省略可能になります。

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

クラスは、少なくとも `MKAnnotation` プロパティが実装されていれば、から派生するだけで注釈データを提供でき `Coordinate` ます。 たとえば、次のサンプルクラスは、コンストラクター内の座標を受け取り、タイトルの文字列を返します。

```csharp
/// <summary>
/// Annotation class that subclasses MKAnnotation abstract class
/// MKAnnotation is bound by Xamarin.iOS to the MKAnnotation protocol
/// </summary>
public class SampleMapAnnotation : MKAnnotation
{
    string title;

    public SampleMapAnnotation (CLLocationCoordinate2D coordinate)
    {
        Coordinate = coordinate;
        title = "Sample";
    }

    public override CLLocationCoordinate2D Coordinate { get; set; }

    public override string Title {
        get {
            return title;
        }
    }
}
```

関連付けられているプロトコルを使用して、サブクラスが `MKAnnotation` 注釈のビューを作成するときにマップによって使用される関連データを提供できます。 マップに注釈を追加するには、次の `AddAnnotation` コードに示すように、インスタンスのメソッドを呼び出し `MKMapView` ます。

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
    new CLLocationCoordinate2D (42.3467512, -71.0969456); // Boston

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

ここでのマップ変数は、 `MKMapView` マップ自体を表すクラスであるのインスタンスです。 は、 `MKMapView` インスタンスから派生したデータを使用して、 `Coordinate` `SampleMapAnnotation` 注釈ビューをマップに配置します。

プロトコルは、実装の `MKAnnotation` 詳細について知る必要があるコンシューマー (この場合はマップ) を使用せずに、それを実装するオブジェクト全体で既知の一連の機能を提供します。 これにより、さまざまな注釈をマップに追加することが効率化されます。

### <a name="protocols-deep-dive"></a>プロトコルの詳細

C# のインターフェイスは省略可能なメソッドをサポートしていないため、Xamarin は抽象クラスにプロトコルをマップします。 そのため、目的の C でプロトコルを採用するには、プロトコルにバインドされている抽象クラスから派生し、必要なメソッドを実装することによって、Xamarin. iOS で実現します。 これらのメソッドは、クラスの抽象メソッドとして公開されます。 プロトコルからの省略可能なメソッドは、C# クラスの仮想メソッドにバインドされます。

たとえば、Xamarin でバインドされているプロトコルの一部を次に示します `UITableViewDataSource` 。

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

クラスは abstract であることに注意してください。 Xamarin では、クラスを抽象クラスにして、プロトコルのオプション/必須メソッドをサポートします。
ただし、目標 C プロトコル (または C# インターフェイス) とは異なり、C# クラスは複数の継承をサポートしていません。 これは、プロトコルを使用する C# コードの設計に影響し、通常は入れ子になったクラスにつながります。 この問題の詳細については、このドキュメントで後述する「デリゲート」セクションを参照してください。

 `GetCell(…)`は、 *selector* `tableView:cellForRowAtIndexPath:` プロトコルの必須メソッドである、目的 C セレクターにバインドされた抽象メソッドです `UITableViewDataSource` 。 Selector は、メソッド名の目的の C 用語です。 必要に応じてメソッドを適用するために、Xamarin. iOS は抽象として宣言します。 もう1つのメソッドは、 `NumberOfSections(…)` にバインドされて `numberOfSectionsInTableview:` います。 このメソッドはプロトコルでは省略可能です。そのため、Xamarin ではこれを virtual として宣言し、C# では省略可能にします。

Xamarin では、すべての iOS バインドが自動的に処理します。 ただし、プロトコルを目的の C から手動でバインドする必要がある場合は、を使用してクラスを装飾することによって、それを行うことができ `ExportAttribute` ます。 これは、Xamarin によって使用される方法と同じです。

Xamarin の種類をバインドする方法の詳細については、「[バインディングの目的-c の型](~/ios/platform/binding-objective-c/index.md)」を参照してください。

ただし、まだプロトコルを使用しているわけではありません。 また、iOS では、次のセクションのトピックである目的 C のデリゲートの基礎としても使用されています。

## <a name="delegates"></a>代理人

iOS では、目的 C のデリゲートを使用して委任パターンを実装します。このパターンでは、1つのオブジェクトが別のオブジェクトに処理を渡します。 作業を行うオブジェクトは、最初のオブジェクトのデリゲートです。 オブジェクトは、特定の処理が行われた後にメッセージを送信することによって、処理を実行するようにデリゲートに指示します。 このようなメッセージを目的 C で送信することは、C# でメソッドを呼び出すことと機能的には同じです。 デリゲートは、これらの呼び出しに応答してメソッドを実装するため、アプリケーションに機能を提供します。

デリゲートを使用すると、クラスの動作を拡張することができます。サブクラスを作成する必要はありません。 IOS のアプリケーションは、重要なアクションが発生した後に、あるクラスが別のクラスにコールバックするときに、多くの場合、デリゲートを使用します。 たとえば、クラスは、 `MKMapView` ユーザーがマップ上で注釈をタップしたときにデリゲートにコールバックします。これにより、デリゲートクラスの作成者は、アプリケーション内で応答する機会を得ることができます。 この種類のデリゲートの使用例については、この記事の後半で説明します。たとえば、「Xamarin. iOS でのデリゲートの使用」をご覧ください。

この時点で、クラスがデリゲートで呼び出すメソッドをどのように決定するか疑問に思うかもしれません。 これは、プロトコルを使用するもう1つの場所です。 通常、デリゲートで使用できるメソッドは、使用するプロトコルによって取得されます。

### <a name="how-protocols-are-used-with-delegates"></a>デリゲートでのプロトコルの使用方法

ここでは、マップへの注釈の追加をサポートするためのプロトコルの使用方法について説明しました。
また、プロトコルを使用して、特定のイベントが発生した後にクラスが呼び出す既知のメソッドのセットを提供します。たとえば、ユーザーがマップ上で注釈をタップした後や、テーブル内のセルを選択した後などです。 これらのメソッドを実装するクラスは、それらを呼び出すクラスのデリゲートと呼ばれます。

デリゲートをサポートするクラスは、デリゲートを実装するクラスが割り当てられるデリゲートプロパティを公開することによってこれを行います。 デリゲートに実装するメソッドは、特定のデリゲートによって適用されるプロトコルによって異なります。 メソッドについては、 `UITableView` メソッドのプロトコルを実装し `UITableViewDelegate` `UIAccelerometer` `UIAccelerometerDelegate` ます。これは、デリゲートを公開する iOS の他のクラスに対しても同様に実装します。

`MKMapView`前の例で示したクラスには、デリゲートと呼ばれるプロパティもあります。このプロパティは、さまざまなイベントが発生した後に呼び出されます。 のデリゲートの `MKMapView` 型は `MKMapViewDelegate` です。
ここでは、選択した後に注釈に応答する例をすぐに使用しますが、まず、厳密なデリゲートと弱いデリゲートの違いについて説明します。

### <a name="strong-delegates-vs-weak-delegates"></a>強いデリゲートと弱いデリゲート

これまでに確認したデリゲートは厳密なデリゲートであり、厳密に型指定されています。 Xamarin の iOS バインディングには、iOS のすべてのデリゲートプロトコルに対して厳密に型指定されたクラスが付属しています。 ただし、iOS には弱いデリゲートという概念もあります。 IOS では、特定のデリゲートに対して目的の C プロトコルにバインドされたクラスをサブクラス化するのではなく、NSObject から派生した任意のクラスのプロトコルメソッドを自分でバインドし、メソッドを ExportAttribute で装飾して、適切なセレクターを指定することもできます。
この方法を使用する場合は、クラスのインスタンスを、Delegate プロパティではなく、"ユーザー設定の委任" プロパティに割り当てます。 弱いデリゲートを使用すると、デリゲートクラスを別の継承階層にする柔軟性が得られます。 厳密なデリゲートと弱いデリゲートの両方を使用する Xamarin の例を見てみましょう。

### <a name="example-using-a-delegate-with-xamarinios"></a>Xamarin でデリゲートを使用する例

この例でユーザーが注釈をタップしたときにコードを実行するには、をサブクラス化 `MKMapViewDelegate` して、のプロパティにインスタンスを割り当てることができ `MKMapView` `Delegate` ます。 プロトコルには、 `MKMapViewDelegate` 省略可能なメソッドのみが含まれています。
そのため、すべてのメソッドは、Xamarin. iOS クラスのこのプロトコルにバインドされている仮想です `MKMapViewDelegate` 。 ユーザーが注釈を選択すると、 `MKMapView` インスタンスは `mapView:didSelectAnnotationView:` そのデリゲートにメッセージを送信します。 これを Xamarin. iOS で処理するには、次の `DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)` ように MKMapViewDelegate サブクラスのメソッドをオーバーライドする必要があります。

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

上に示した SampleMapDelegate クラスは、インスタンスを含むコントローラーで入れ子になったクラスとして実装され `MKMapView` ます。 目的 C では、多くの場合、コントローラーがクラス内で直接複数のプロトコルを採用していることがわかります。 ただし、プロトコルは Xamarin のクラスにバインドされるため、厳密に型指定されたデリゲートを実装するクラスは、通常、入れ子になったクラスとして含まれます。

デリゲートクラスの実装を配置するには、次に示すように、コントローラーでデリゲートのインスタンスをインスタンス化し、そのインスタンスをのプロパティに割り当てる必要があり `MKMapView` `Delegate` ます。

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

弱いデリゲートを使用して同じことを実現するには、から派生した任意のクラスでメソッドを自分でバインド `NSObject` し、そのメソッドをのプロパティに割り当てる必要があり `WeakDelegate` `MKMapView` ます。 クラスは `UIViewController` 最終的にから派生したものであるため `NSObject` (CocoaTouch のすべての目的 C クラスと同様)、コントローラーに直接バインドされたメソッドを実装 `mapView:didSelectAnnotationView:` し、コントローラーをに割り当てるだけ `MKMapView` `WeakDelegate` で、追加の入れ子になったクラスの必要性を回避できます。 次のコードは、この方法を示しています。

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

このコードを実行すると、厳密に型指定されたデリゲートバージョンを実行したときとまったく同じように動作します。 このコードの利点は、弱いデリゲートでは、厳密に型指定されたデリゲートを使用したときに作成された追加のクラスを作成する必要がないことです。 ただし、これにはタイプセーフのコストが伴います。 に渡されたセレクターで間違いを犯した場合、実行可能に `ExportAttribute` なるまではわかりません。

### <a name="events-and-delegates"></a>イベントとデリゲート

デリゲートは、.NET でのイベントの使用方法と同様に、iOS のコールバックに使用されます。 IOS Api を作成し、目的の C のデリゲートを使用する方法が .NET と似ているようにするために、ios ではデリゲートが多くの場所で .NET に公開されています。

たとえば、以前の実装では、 `MKMapViewDelegate` 選択した注釈に応答したが .net イベントを使用して Xamarin に実装することもできます。 その場合、イベントはで定義され、 `MKMapView` と呼ばれ `DidSelectAnnotationView` ます。 これには、 `EventArgs` 型のサブクラスがあり `MKMapViewAnnotationEventsArgs` ます。 `View`のプロパティは、 `MKMapViewAnnotationEventsArgs` 次に示すように、注釈ビューへの参照を提供します。ここでは、前に示したのと同じ実装を続行できます。

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

## <a name="summary"></a>まとめ

この記事では、Xamarin. iOS でイベント、プロトコル、およびデリゲートを使用する方法について説明します。 Xamarin では、コントロールの通常の .NET スタイルイベントを公開する方法を説明しました。
次に、C 言語のプロトコルについて学習しました。これには、C# インターフェイスとの違いや、Xamarin で使用する方法などがあります。 最後に、Xamarin. iOS の観点から目的の C のデリゲートを調べます。 Xamarin では、厳密に型指定されたデリゲートと、.NET イベントをデリゲートメソッドにバインドする方法の両方をサポートしています。

## <a name="related-links"></a>関連リンク

- [プロトコル、デリゲート、およびイベント (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/protocols-delegates-events)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [バインディングの目的 C の型](~/ios/platform/binding-objective-c/index.md)
- [目的 C プログラミング言語](https://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Xcode 4 でのユーザーインターフェイスの設計](https://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [IOS のコアアプリケーションコンピテンシー](https://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
