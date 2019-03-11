---
title: イベント、プロトコル、Xamarin.iOS でのデリゲート
description: このドキュメントでは、イベント、プロトコルを使用する方法について説明し、Xamarin.iOS にデリゲートします。 これらの基本的な概念は、Xamarin.iOS の開発ではユビキタスです。
ms.prod: xamarin
ms.assetid: 7C07F0B7-9000-C540-0FC3-631C29610447
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 09/17/2017
ms.openlocfilehash: 83f9651fa7fd20709c620258833ae4a152ffd0eb
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563668"
---
# <a name="events-protocols-and-delegates-in-xamarinios"></a>イベント、プロトコル、Xamarin.iOS でのデリゲート

Xamarin.iOS では、コントロールを使用して、ほとんどのユーザー操作のイベントを公開します。
Xamarin.iOS アプリケーションでは、従来の .NET アプリケーションと同様に、ほぼ同じ方法でこれらのイベントを使用します。 たとえば、Xamarin.iOS UIButton クラスは TouchUpInside というイベントが、.NET アプリでこのクラスとイベントがいた場合と同様に、このイベントを使用します。

Xamarin.iOS は、この .NET のアプローチだけでなく、より複雑な相互作用とデータ バインドに使用できる別のモデルを公開します。 この方法論では、Apple がデリゲートとプロトコルと呼ぶものを使用します。 デリゲートは、C#のデリゲートと概念が似ていますが、単一のメソッドを定義して呼び出すのではなく、Objective C のデリゲートはプロトコルに準拠しているクラス全体を定義します。メソッドが選択可能な場合を除き、プロトコルはC＃のインターフェースに似ています。たとえば、データの UITableView を設定すると、設定自体を呼び出す UITableView UITableViewDataSource プロトコルで定義されているメソッドを実装するデリゲート クラスを作成します。

この記事では、これらすべてのトピックについて説明しますで提供する強固な基盤、Xamarin.iOS でのコールバックのシナリオを処理するためなど。

- **イベント**– UIKit コントロールを使用して .NET イベント。
- **プロトコル**– プロトコルとその使用方法の学習内容と、マップの注釈のデータを提供する例を作成します。
- **デリゲート**– Objective C のデリゲートには、注釈を含むユーザーの操作を処理するためにマップの例を拡張し、強力と脆弱のデリゲートとこれらの各を使用する場合の違いを学習について学習します。

プロトコル、デリゲートを示すためには、次に示すように、マップに注釈を追加するシンプルな地図アプリケーションを作成します。

[![](delegates-protocols-and-events-images/01-map-sml.png "マップに注釈を追加するシンプルな地図アプリケーションの例")](delegates-protocols-and-events-images/01-map.png#lightbox)
[![](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png "マップに追加された例注釈")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

このアプリを使って取り組むには、前に開始しましょう .NET のイベントは、UIKit で調べることで。

## <a name="net-events-with-uikit"></a>UIKit で .NET イベント

Xamarin.iOS では、UIKit コントロールの .NET イベントを公開します。 たとえば、TouchUpInside イベントを使用する次のコードに示すように、.NET では、通常どおりに処理するには UIButton、C#ラムダ式。

```csharp
aButton.TouchUpInside += (o,s) => {
    Console.WriteLine("button touched");
};
```

これを実装することも、C#次のような匿名メソッドを 2.0 スタイル。

```csharp
aButton.TouchUpInside += delegate {
    Console.WriteLine ("button touched");
};
```

上記のコードのワイヤード (有線)、`ViewDidLoad`の UIViewController のメソッド。 `aButton`変数を参照して、ボタンは、iOS デザイナーまたはコードを追加できます。 次の図は、iOS Designer に追加されているボタンを示しています。

[![](delegates-protocols-and-events-images/02-interface-builder-outlet-sml.png "IOS Designer で追加ボタン")](delegates-protocols-and-events-images/02-interface-builder-outlet.png#lightbox)

Xamarin.iOS には、コントロールで発生する相互作用にコードを接続するターゲット アクション スタイルもサポートしています。 ターゲットのアクションを作成する、**こんにちは**ボタンをダブルクリック iOS Designer でします。 UIViewController の分離コード ファイルが表示され、開発者は接続のメソッドを挿入する場所の選択を求められます。

[![](delegates-protocols-and-events-images/03-interface-builder-action-sml.png "UIViewControllers の分離コード ファイル")](delegates-protocols-and-events-images/03-interface-builder-action.png#lightbox)

場所を選択すると、新しいメソッドが作成されコントロールにワイヤード (有線) アップします。 次の例では、メッセージが書き込まれますコンソールに、ボタンがクリックされたとき。

[![](delegates-protocols-and-events-images/05-interface-builder-action-sml.png "ボタンがクリックされたときにそのメッセージがコンソールに書き込まれます")](delegates-protocols-and-events-images/05-interface-builder-action.png#lightbox)

IOS ターゲット アクション パターンに関する詳細については、ターゲット操作 セクションを参照してください。 [ios アプリケーションのコア コンピテンシー](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html) Apple の ios 開発者ライブラリ。

Xamarin.iOS で iOS Designer を使用する方法の詳細については、次を参照してください。、 [iOS デザイナーの概要](~/ios/user-interface/designer/index.md)ドキュメント。

## <a name="events"></a>イベント

さまざまなオプションを < からのイベントをインターセプトしたい場合がある: を使用してから、C#ラムダおよび低レベルの Objective C Api を使用するデリゲート関数。

次のセクションでは、必要な量の制御によって、ボタンの接地イベントをキャプチャする方法を示します。

## <a name="c-style"></a>C#スタイル

デリゲートの構文を使用します。

```csharp
UIButton button = MakeTheButton ();
button.TouchDown += delegate {
    Console.WriteLine ("Touched");
};
```

代わりにラムダを使用するなどの: 場合

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

## <a name="monitoring-more-than-one-kind-of-event"></a>1 つ以上の種類のイベントの監視

C#イベント UIControlEvent フラグを個々 のフラグを 1 対 1 マッピングがあります。 2 つ以上のイベントを処理を使用すると、コードの同じ部分がある場合、`UIControl.AddTarget`メソッド。

```csharp
button.AddTarget (handler, UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

ラムダ構文を使用します。

```csharp
button.AddTarget ((sender, event)=> Console.WriteLine ("An event happened"), UIControlEvent.TouchDown | UIControlEvent.TouchCancel);
```

場合は、特定のオブジェクトのインスタンスへのフックや特定のセレクターの起動など、OBJECTIVE-C の低レベルの機能を使用する必要があります。

```csharp
[Export ("MySelector")]
void MyObjectiveCHandler ()
{
    Console.WriteLine ("Hello!");
}

// In some other place:

button.AddTarget (this, new Selector ("MySelector"), UIControlEvent.TouchDown);
```

ただし、継承の基本クラスのインスタンス メソッドを実装する場合、パブリック メソッドがあります。

## <a name="protocols"></a>プロトコル

プロトコルは、メソッド宣言の一覧を提供する Objective C 言語機能です。 機能のインターフェイスに似た目的C#、プロトコルが省略可能なメソッドを持つことができるされている主な違いです。 プロトコルを採用するクラスがそれらを実装しない場合、省略可能なメソッドは呼び出されません。 同様、Objective C での 1 つのクラスが、複数のプロトコルを実装することも、C#クラスは、複数のインターフェイスを実装できます。

Apple では、iOS でプロトコルを使用して、したがって動作と同じように、呼び出し元から実装するクラスを抽象しながら、採用するクラスのコントラクトを定義、C#インターフェイス。 プロトコルが使用されるデリゲート以外のシナリオでは、両方 (などで、`MKAnnotation`例を次のように)、デリゲート (デリゲートのセクションでは、このドキュメントの後半で説明) とを使用して。

### <a name="protocols-with-xamarinios"></a>Xamarin.ios を使用するプロトコル

Xamarin.iOS から OBJECTIVE-C プロトコルを使用して例を見ていきましょう。 この例で使用します、`MKAnnotation`プロトコルで、一部の`MapKit`フレームワーク。 `MKAnnotation` プロトコルにより、マップに追加できる注釈についての情報を提供することを採用する任意のオブジェクトです。 たとえば、実装するオブジェクト`MKAnnotation`注釈とそれに関連付けられているタイトルの位置を提供します。

この方法で、`MKAnnotation`注釈に付属している関連データを提供するプロトコルを使用します。 注釈自体の実際のビューが採用するオブジェクトのデータから構築された、`MKAnnotation`プロトコル。 たとえば、(次のスクリーン ショットに示す) と、注釈で、ユーザーがタップしたときに表示される引き出し線のテキストに由来します`Title`プロトコルを実装するクラスのプロパティ。

 [![](delegates-protocols-and-events-images/04-annotation-with-callout-sml.png "吹き出し注釈で、ユーザーがタップしたときのテキストの例")](delegates-protocols-and-events-images/04-annotation-with-callout.png#lightbox)

次のセクションで説明した[プロトコル Deep Dive](#protocols-deep-dive)Xamarin.iOS のプロトコルを抽象クラスにバインドします。 `MKAnnotation`プロトコル、バインドされているC#クラスの名前が`MKAnnotation`のサブクラスは、プロトコルの名前を模倣するために、 `NSObject`、CocoaTouch のルートの基本クラス。 プロトコルは、getter および setter 座標; のために実装が必要です。ただし、タイトルとサブタイトルは省略可能です。 そのため、`MKAnnotation`クラス、`Coordinate`プロパティは*抽象*を実装することを必要として、`Title`と`Subtitle`プロパティがマークされている*仮想*、次に示す、省略可能なことにします。

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

任意のクラスだけから派生することによって注釈データを提供できます`MKAnnotation`、少なくともいる限り、`Coordinate`プロパティを実装します。 たとえば、コンス トラクターで、座標を受け取り、タイトルの文字列を返しますサンプル クラスです。

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

バインドされているプロトコルを介してすべてのクラスをサブクラスとして持つ`MKAnnotation`注釈の表示を作成するときに、マップで使用される関連データを提供することができます。 マップに注釈を追加するに呼び出すだけです、`AddAnnotation`のメソッド、`MKMapView`の次のコードに示すように、インスタンスします。

```csharp
//an arbitrary coordinate used for demonstration here
var sampleCoordinate =
    new CLLocationCoordinate2D (42.3467512, -71.0969456); // Boston

//create an annotation and add it to the map
map.AddAnnotation (new SampleMapAnnotation (sampleCoordinate));
```

ここで、マップ変数のインスタンスである、 `MKMapView`、マップ自体を表すクラスです。 `MKMapView`を使用して、`Coordinate`から派生したデータ、`SampleMapAnnotation`インスタンス マップ上の注釈のビューを配置します。

`MKAnnotation`プロトコルは、既知のセットを提供します。 それを実装する任意のオブジェクト間で、機能のコンシューマー (この場合のマップ) が必要はありません実装の詳細について知る。 これは、さまざまな利用可能なコメントを追加するマップに効率化します。

### <a name="protocols-deep-dive"></a>プロトコルの詳細情報

C#インターフェイスは、省略可能なメソッドをサポートしない、Xamarin.iOS のプロトコルを抽象クラスにマップされます。 そのため、プロトコルにバインドされている抽象クラスから派生して、必要なメソッドの実装によって Xamarin.iOS で Objective C でのプロトコルを採用することが実現されます。 これらのメソッドは、クラスの抽象メソッドとして公開されます。 仮想メソッドにバインドされるプロトコルからの省略可能なメソッド、C#クラス。

たとえばの一部をここでは、`UITableViewDataSource`プロトコルにバインドされている Xamarin.iOS として。

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

抽象クラスに注意してください。 Xamarin.iOS では、クラスをプロトコル/省略可能なために必要なメソッドをサポートするために抽象になります。
Objective C のプロトコルとは異なり、(またはC#インターフェイス)、C#クラスは、多重継承をサポートしません。 デザインに影響C#プロトコルを使用しすることにより、通常のコードには、クラスが入れ子になった。 この問題についてについては、デリゲートのセクションで、このドキュメントで後述します。

 `GetCell(…)` Objective C にバインドされている、抽象メソッドである*セレクター*、`tableView:cellForRowAtIndexPath:`の必要なメソッドは、`UITableViewDataSource`プロトコル。 セレクターは、メソッド名の Objective C 用語です。 必要に応じてメソッドを適用するには、Xamarin.iOS は、それを抽象として宣言します。 その他のメソッドでは、`NumberOfSections(…)`にバインドされた`numberOfSectionsInTableview:`します。 このメソッドは、Xamarin.iOS では宣言 virtual で上書きする省略可能なので、プロトコルでは、省略可能なC#します。

Xamarin.iOS は、すべての iOS バインドで処理されます。 ただし、OBJECTIVE-C からのプロトコルを手動でバインドする必要がある場合は、これを行うクラスを修飾することによって、`ExportAttribute`します。 これは、Xamarin.iOS 自体で使用される同じメソッドです。

Xamarin.iOS で Objective C 型をバインドする方法の詳細については、この記事を参照してください。 [Objective-c のバインド型](~/ios/platform/binding-objective-c/index.md)します。

私たちありませんを通じてプロトコルでまだ、ただしです。 IOS の基礎として、使用 Objective C のデリゲートは、次のセクションのトピックでは、します。

## <a name="delegates"></a>デリゲート

iOS では、OBJECTIVE-C でデリゲートを使用して、1 つのオブジェクト パスのしくみ別に、委任パターンを実装します。 作業オブジェクトは、最初のオブジェクトのデリゲートです。 オブジェクトは、特定の処理が行われます後にメッセージを送信することによって作業を実行するには、そのデリゲートを指示します。 機能的には、メソッドを呼び出すことは、objective-c でこのようなメッセージを送信するC#します。 デリゲートは、これらの呼び出しに応答メソッドを実装し、そのため、アプリケーションに機能を提供します。

デリゲートでは、サブクラスを作成しなくてもクラスの動作を拡張できます。 IOS でのアプリケーションは、1 つのクラスを呼び出して別、重要なアクションが発生した後と多くの場合、デリゲートを使用します。 たとえば、`MKMapView`クラスそのデリゲートへのユーザーがタップすると、マップに注釈がデリゲート クラスの作成者、アプリケーション内で応答する機会を提供します。 Xamarin.iOS でデリゲートをこの種類のデリゲートの使用状況の例を使用して、この記事の後半の例を操作できます。

この時点と思うかもしれませんクラスがそのデリゲートを呼び出すには、どのような方法を決定する方法。 これは、プロトコルを使用する別の場所です。 通常は、デリゲートの使用可能なメソッドは、採用するプロトコル。

### <a name="how-protocols-are-used-with-delegates"></a>デリゲートでプロトコルを使用する方法

プロトコルを使用してマップを注釈の追加をサポートする以前方法を説明しました。
プロトコルは、既知のクラスが特定のイベントが発生した、たとえば、ユーザーをマップに注釈をタップした後またはテーブルのセルを選択した後に呼び出すためのメソッドのセットを提供するも使用されます。 これらのメソッドを実装するクラスは、それらを呼び出すクラスのデリゲートと呼ばれます。

委任をサポートするクラスは、デリゲートを実装するクラスが割り当てられているデリゲート プロパティを公開することで行います。 デリゲートを実装するメソッドは、特定のデリゲートを採用するプロトコルによって異なります。 `UITableView`メソッドを実装する、`UITableViewDelegate`プロトコルでの`UIAccelerometer`実装するメソッド、`UIAccelerometerDelegate`する対象の iOS 全体でその他のすべてのクラス、デリゲートを公開するのに、します。

`MKMapView`前の例で使用したクラスは、呼び出すことは、デリゲートと呼ばれるプロパティもあります。 さまざまなイベントが発生した後。 デリゲート`MKMapView`の種類は`MKMapViewDelegate`します。
選択すると後、に、注釈に応答する例が最初で間もなくみましょうこれ使用します強力と脆弱のデリゲートの違いについて説明します。

### <a name="strong-delegates-vs-weak-delegates"></a>Vs の厳密なデリゲート。弱い委任

これまでに紹介しましたデリゲートとは、強力なデリゲート、厳密に型指定された意味です。 Xamarin.iOS バインディングは、iOS では、すべてデリゲート プロトコルの厳密に型指定されたクラスに付属します。 ただし、iOS では、弱い委任の概念もあります。 OBJECTIVE-C プロトコルにバインドして、特定のデリゲート クラスをサブクラス化ではなく iOS することもできますたい、ExportAttribute でメソッドを装飾 NSObject から派生したクラスでプロトコル メソッドを自分でバインドを選択し、適切なセレクターを指定します。
この方法で実行する場合は、デリゲート プロパティの代わりに WeakDelegate プロパティに、クラスのインスタンスを割り当てます。 弱い委任では、別の継承階層の下位デリゲート クラスを取得する柔軟性を提供します。 強力と脆弱の両方のデリゲートを使用する Xamarin.iOS の例を見てみましょう。

### <a name="example-using-a-delegate-with-xamarinios"></a>Xamarin.iOS でデリゲートを使用した例

コードを実行するには、この例では注釈をタップして、ユーザーへの応答にできるサブクラス`MKMapViewDelegate`にインスタンスを割り当てると、`MKMapView`の`Delegate`プロパティ。 `MKMapViewDelegate`プロトコルには、省略可能なメソッドのみが含まれています。
そのため、すべてのメソッドが仮想、Xamarin.iOS では、このプロトコルにバインドされる`MKMapViewDelegate`クラス。 ユーザーが、注釈を選択すると、`MKMapView`インスタンスは送信、`mapView:didSelectAnnotationView:`そのデリゲートへのメッセージ。 オーバーライドする Xamarin.iOS では、これを処理するため、`DidSelectAnnotationView (MKMapView mapView, MKAnnotationView annotationView)`メソッドでは、このような MKMapViewDelegate サブクラスです。

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

前に示した SampleMapDelegate クラスを含むコント ローラー内の入れ子になったクラスとして実装、`MKMapView`インスタンス。 Objective-c では、多くの場合、クラス内で直接、複数のプロトコルを採用するコント ローラーが表示されます。 ただし、Xamarin.iOS のクラスには、プロトコルがバインドされている、ために、厳密に型指定されたデリゲートを実装するクラスは、通常は入れ子になったクラスとして含まれています。

インプレース デリゲート クラスの実装を使用するだけで済みますコント ローラー内のデリゲートのインスタンスをインスタンス化し、それを割り当てる、`MKMapView`の`Delegate`プロパティは、ここに示すようにします。

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

派生したクラス、メソッドを自分でバインドする必要が、弱い委任を使用して、同じことを実現する、`NSObject`に割り当てると、`WeakDelegate`のプロパティ、`MKMapView`します。 以降、`UIViewController`から最終的に派生`NSObject`(Objective C のすべてのクラス CocoaTouch) などをメソッドにバインドされている単純に実装できる`mapView:didSelectAnnotationView:`コント ローラーで直接にコント ローラーに割り当てると`MKMapView`の`WeakDelegate`、余分な入れ子になったクラスの必要性を回避します。 次のコードでは、この方法を示しています。

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

このコードを実行するときに、アプリケーションは、デリゲートが厳密に型指定されたバージョンを実行している場合とまったく動作します。 このコードから特典は、弱いデリゲートが厳密に型指定されたデリゲートを使用したときに作成された余分なクラスの作成を必要としないことです。 ただし、これは、タイプ セーフと引き換え。 渡されたセレクターで間違いをしたかどうか、 `ExportAttribute`、実行時まで確認はありません。

### <a name="events-and-delegates"></a>イベントとデリゲート

デリゲートは、.NET のイベントの使用方法と同じように iOS でのコールバックに使用されます。 IOS を使用するには、Api と OBJECTIVE-C のデリゲートの使用方法よりのように思える .NET では、Xamarin.iOS は、デリゲートが iOS で使用されているさまざまな場所での .NET イベントを公開します。

以前の実装ではたとえば、場所、`MKMapViewDelegate`に応答した .NET イベントを使用して選択したコメントが Xamarin.iOS で実装することもできます。 その場合は、イベントで定義される`MKMapView`と呼ばれる、`DidSelectAnnotationView`します。 必要があります、`EventArgs`型のサブクラス`MKMapViewAnnotationEventsArgs`します。 `View`プロパティの`MKMapViewAnnotationEventsArgs`注釈ビューへの参照を提供する、同じ実装を行うことがこれからが前に示すように、ここで。

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

この記事では、Xamarin.iOS でのイベント、プロトコル、およびデリゲートを使用する方法について説明します。 Xamarin.iOS がコントロールの通常の .NET スタイルのイベントを公開する方法を説明しました。
異なる方法を含めた、OBJECTIVE-C プロトコルの学習次C#インターフェイスと Xamarin.iOS での使用方法。 最後に、Xamarin.iOS の観点から OBJECTIVE-C でデリゲートを調査します。 Xamarin.iOS の仕組み両方厳密と弱い型指定されたデリゲート、および .NET イベント メソッドをデリゲートにバインドする方法を説明しました。

## <a name="related-links"></a>関連リンク

- [プロトコル、デリゲート、およびイベント (サンプル)](https://developer.xamarin.com/samples/Protocols_Delegates_Events/)
- [Hello, iOS](~/ios/get-started/hello-ios/index.md)
- [OBJECTIVE-C のバインドの種類](~/ios/platform/binding-objective-c/index.md)
- [Objective C のプログラミング言語](http://developer.apple.com/library/ios/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
- [Xcode 4 でのユーザー インターフェイスの設計](http://developer.apple.com/library/ios/#documentation/IDEs/Conceptual/Xcode4TransitionGuide/InterfaceBuilder/InterfaceBuilder.html)
- [IOS 用の核となるアプリケーションの機能](http://developer.apple.com/library/ios/#DOCUMENTATION/General/Conceptual/Devpedia-CocoaApp/TargetAction.html)
