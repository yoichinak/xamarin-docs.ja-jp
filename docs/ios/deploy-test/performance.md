---
title: Xamarin.iOS のパフォーマンス
description: このドキュメントでは、Xamarin.iOS アプリケーションでパフォーマンスとメモリ使用量を改善する手法について説明します。
ms.prod: xamarin
ms.assetid: 02b1f628-52d9-49de-8479-f2696546ca3f
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 01/29/2016
ms.openlocfilehash: bfa8c2cdcdcd6305618c0cd8e9cb69bde59b4f0b
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030204"
---
# <a name="xamarinios-performance"></a>Xamarin.iOS のパフォーマンス

アプリケーションのパフォーマンスの低さは、さまざまな形でアプリケーションに現れます。 たとえば、アプリケーションが応答しなかったり、スクロールが遅くなったり、バッテリーの寿命が縮まったりすることがあります。 ただし、パフォーマンスを最適化するには、単に効率的なコードを実装するだけでは不十分です。 アプリケーション パフォーマンスに関わるユーザー エクスペリエンスも考慮する必要があります。 たとえば、操作の実行によって、ユーザーが他の操作を実行できない状況にならないようにすることで、ユーザー エクスペリエンスを改善できます。

このドキュメントでは、Xamarin.iOS アプリケーションでパフォーマンスとメモリ使用量を改善する手法について説明します。

> [!NOTE]
> この記事を読む前に、まず、「[Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)」(クロスプラットフォーム パフォーマンス) をお読みください。Xamarin プラットフォームを使用してビルドされたアプリケーションのメモリ使用量とパフォーマンスを改善するための、プラットフォーム固有ではない手法について説明されています。

## <a name="avoid-strong-circular-references"></a>強い循環参照を避ける

場合によっては、ガベージ コレクターからオブジェクトのメモリが再要求されないように、強い参照循環を作成することがあります。 たとえば、[`NSObject`](xref:Foundation.NSObject) から派生するサブクラス ([`UIView`](xref:UIKit.UIView) から継承されたクラスなど) は、次のコード例のように、`NSObject` から派生したコンテナーに追加され、Objective-C から強く参照されます。

```csharp
class Container : UIView
{
    public void Poke ()
    {
    // Call this method to poke this object
    }
}

class MyView : UIView
{
    Container parent;
    public MyView (Container parent)
    {
        this.parent = parent;
    }

    void PokeParent ()
    {
        parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

このコードで `Container` インスタンスを作成すると、C# オブジェクトは Objective-C オブジェクトに対して強い参照を持つことになります。 同様に、`MyView` インスタンスも、Objective-C オブジェクトへの強い参照を持つことになります。

さらに、`container.AddSubview` を呼び出すと、アンマネージ インスタンス `MyView` の参照カウントが増えます。 このとき、Xamarin.iOS ランタイムは `GCHandle` インスタンスを作成することで、マネージド コードの `MyView` オブジェクトを維持し続けます。これは、マネージド オブジェクトが参照を維持し続ける保証がないためです。 マネージド コードの観点からは、`GCHandle` に対する [`AddSubview`](xref:UIKit.UIView.AddSubview(UIKit.UIView)) の呼び出しではなくなった後に `MyView` オブジェクトは再要求されます。

アンマネージド `MyView` オブジェクトは、マネージド オブジェクトを示す `GCHandle` を持つことになります。これは*強いリンク*と呼ばれます。 マネージド オブジェクトには、`Container` インスタンスへの参照が含まれます。 そして `Container` インスタンスには `MyView` オブジェクトへのマネージド参照が含まれます。

含まれるオブジェクトがコンテナーに対するリンクを維持する場合、循環参照の処理に使用できる選択肢がいくつかあります。

- コンテナーへのリンクを `null` に設定することで、手動で循環を中断する。
- コンテナーに含まれているオブジェクトを手動で削除する。
- オブジェクトに対して `Dispose` を呼び出す。
- コンテナーに対して弱い参照を維持することで、循環参照を避ける。 詳細については、弱い参照のセクションを参照してください。

### <a name="using-weakreferences"></a>弱い参照の使用

循環を避ける方法の 1 つは、子から親への参照に弱い参照を使用することです。 たとえば、上記のコードは次のように記述できます。

```csharp
class Container : UIView
{
    public void Poke ()
    {
        // Call this method to poke this object
    }
}

class MyView : UIView
{
    WeakReference<Container> weakParent;
    public MyView (Container parent)
    {
        this.weakParent = new WeakReference<Container> (parent);
    }

    void PokeParent ()
    {
        if (weakParent.TryGetTarget (out var parent))
            parent.Poke ();
    }
}

var container = new Container ();
container.AddSubview (new MyView (container));
```

ここで、含まれるオブジェクトでは、親が維持されません。 ただし、親では、`container.AddSubView` を呼び出すことで子が維持されます。

デリゲートまたはデータ ソース パターンを使用する iOS API でも、同じことが起こります。たとえば、[`Delegate`](xref:UIKit.UITableView.Delegate*)
プロパティまたは [`DataSource`](xref:UIKit.UITableView.DataSource*) を
[`UITableView`](xref:UIKit.UITableView) クラスで設定するときなどに、ピア クラスに実装が含まれます。

プロトコルを実装するためだけに作成されたクラスの場合 ([`IUITableViewDataSource`](xref:UIKit.IUITableViewDataSource) など) にできることは、サブクラスの作成ではなく、クラスにインターフェイスを実装し、メソッドをオーバーライドし、`DataSource` プロパティを `this` に割り当てることです。

#### <a name="weak-attribute"></a>Weak 属性

[Xamarin.iOS 11.10](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/xamarin.ios_11/xamarin.ios_11.10.md#WeakAttribute) では、`[Weak]` 属性が導入されました。 `WeakReference <T>` と同様に、`[Weak]` を利用して[強い循環参照](https://docs.microsoft.com/xamarin/ios/deploy-test/performance#avoid-strong-circular-references)を中断できますが、コードがさらに短くなります。

次のコード例をご覧ください。`WeakReference <T>` を使用しています。

```csharp
public class MyFooDelegate : FooDelegate {
    WeakReference<MyViewController> controller;
    public MyFooDelegate (MyViewController ctrl) => controller = new WeakReference<MyViewController> (ctrl);
    public void CallDoSomething ()
    {
        MyViewController ctrl;
        if (controller.TryGetTarget (out ctrl)) {
            ctrl.DoSomething ();
        }
    }
}
```

同様のコードで `[Weak]` を使用すると、ずっと簡潔になります。

```csharp
public class MyFooDelegate : FooDelegate {
    [Weak] MyViewController controller;
    public MyFooDelegate (MyViewController ctrl) => controller = ctrl;
    public void CallDoSomething () => controller.DoSomething ();
}
```

次は、[委任](https://developer.apple.com/library/content/documentation/General/Conceptual/DevPedia-CocoaCore/Delegation.html)パターンで `[Weak]` を使用するもう 1 つの例です。

```csharp
public class MyViewController : UIViewController
{
    WKWebView webView;

    protected MyViewController (IntPtr handle) : base (handle) { }

    public override void ViewDidLoad ()
    {
        base.ViewDidLoad ();
        webView = new WKWebView (View.Bounds, new WKWebViewConfiguration ());
        webView.UIDelegate = new UIDelegate (this);
        View.AddSubview (webView);
    }
}

public class UIDelegate : WKUIDelegate
{
    [Weak] MyViewController controller;

    public UIDelegate (MyViewController ctrl) => controller = ctrl;

    public override void RunJavaScriptAlertPanel (WKWebView webView, string message, WKFrameInfo frame, Action completionHandler)
    {
        var msg = $"Hello from: {controller.Title}";
        var alertController = UIAlertController.Create (null, msg, UIAlertControllerStyle.Alert);
        alertController.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
        controller.PresentViewController (alertController, true, null);
        completionHandler ();
    }
}
```

### <a name="disposing-of-objects-with-strong-references"></a>強い参照を使用したオブジェクトの破棄

強い参照が存在し、依存関係を削除するのが困難な場合は、`Dispose` メソッドで親ポインターをクリアします。

コンテナーの場合は、`Dispose` メソッドをオーバーライドして含まれるオブジェクトを削除します。次にコード例を示します。

```csharp
class MyContainer : UIView
{
    public override void Dispose ()
    {
        // Brute force, remove everything
        foreach (var view in Subviews)
        {
              view.RemoveFromSuperview ();
        }
        base.Dispose ();
    }
}
```

親に対する強い参照を維持する子オブジェクトの場合は、`Dispose` 実装で親に対する参照をクリアします。

```csharp
class MyChild : UIView
{
    MyContainer container;
    public MyChild (MyContainer container)
    {
        this.container = container;
    }
    public override void Dispose ()
    {
        container = null;
    }
}
```

強い参照の解放の詳細については、「[Release IDisposable Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#idisposable)」(IDisposable リソースの解放) を参照してください。
また、ブログ記事「[Xamarin.iOS, the garbage collector and me (Xamarin.iOS とガベージ コレクターと私)](https://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me)」の説明もお勧めします。

### <a name="more-information"></a>詳細情報

詳細については、Cocoa With Love の「[Rules to Avoid Retain Cycles](https://www.cocoawithlove.com/2009/07/rules-to-avoid-retain-cycles.html)」(循環の保持を回避する規則)、StackOverflow の「[Is this a bug in MonoTouch GC](https://stackoverflow.com/questions/13058521/is-this-a-bug-in-monotouch-gc)」(これは MonoTouch GC のバグですか)、StackOverflow の「[Why can't MonoTouch GC kill managed objects with refcount &gt; 1?](https://stackoverflow.com/questions/13064669/why-cant-monotouch-gc-kill-managed-objects-with-refcount-1)」(参照カウントが 1 を超えるマネージド オブジェクトを MonoTouch GC でキルできないのはなぜですか?) を参照してください。

## <a name="optimize-table-views"></a>テーブル ビューを最適化する

ユーザーは、[`UITableView`](xref:UIKit.UITableView) インスタンスのスムーズなスクロールと読み込み時間の短縮を期待します。 ただし、セルに深い入れ子のビュー階層が含まれる場合、またはセルに複雑なレイアウトが含まれる場合、スクロールのパフォーマンスは低下する可能性があります。 ただし、`UITableView` のパフォーマンス低下を回避するために使用できる手法があります。

- セルを再利用する。 詳細については、「[Reuse Cells](#reuse-cells)」(セルの再利用) を参照してください。
- サブビューの数を減らす。
- Web サービスから取得されるセルのコンテンツをキャッシュする。
- 行の高さが同じでない場合はキャッシュする。
- セルと他のビューを非透過的にする。
- イメージのスケーリングとグラデーションを避ける。

これらの手法をすべて使用することで、[`UITableView`](xref:UIKit.UITableView) インスタンスのスクロールをスムーズに保つことができます。

### <a name="reuse-cells"></a>セルを再利用する

[`UITableView`](xref:UIKit.UITableView) で数百行を表示する場合、少数のみが 1 画面に表示されるなら、数百単位の [`UITableViewCell`](xref:UIKit.UITableViewCell) オブジェクトを作成するのはメモリの無駄です。 代わりに、画面に表示されるセルのみをメモリに読み込み、その再利用されるセルに**コンテンツ**を読み込むことができます。 こうすることで、数百単位の追加オブジェクトのインスタンス化を防ぎ、時間とメモリを節約することができます。

そのため、次のコード例のように、セルが画面から消えるときに、そのビューを再利用のためのキューに格納できます。

```csharp
class MyTableSource : UITableViewSource
{
    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        // iOS will create a cell automatically if one isn't available in the reuse pool
        var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

        // Perform required cell actions
        return cell;
    }
}
```

ユーザーがスクロールすると、[`UITableView`](xref:UIKit.UITableView) は `GetCell` のオーバーライドを呼び出し、新しいビューの表示を要求します。 このオーバーライドは [`DequeueReusableCell`](xref:UIKit.UITableView.DequeueReusableCell(Foundation.NSString)) メソッドを呼び出し、再利用できるセルがある場合はそのセルが返されます。

詳細については、「[Populating a Table with Data](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)」(データを使用したテーブルの設定) の「[Cell Reuse](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)」(セルの再利用) を参照してください。

## <a name="use-opaque-views"></a>非透過的なビューを使用する

透過性が定義されていないビューには、[`Opaque`](xref:UIKit.UIView.Opaque) プロパティを設定するようにします。 こうすることで、描画システムはビューを最適にレンダリングします。 この方法は、ビューが [`UIScrollView`](xref:UIKit.UIScrollView) に埋め込まれている場合、または複雑なアニメーションの一部の場合に特に重要です。 そうしないと、描画システムは他のコンテンツを含むビューを作成し、結果的にパフォーマンスが大きく低下する可能性があります。

## <a name="avoid-fat-xibs"></a>FAT XIB を避ける

大部分の XIB はストーリーボードに置き換えられましたが、まだ XIB が使用されている環境も一部にあります。 XIB がメモリに読み込まれると、すべての画像を含め、すべてのコンテンツがメモリに読み込まれます。 XIB にすぐに使用されないビューが含まれると、メモリは無駄になります。 そのため、XIB を使用する場合は、1 つのビュー コントローラーにつき XIB を 1 つのみにします。また、可能であれば、ビュー コントローラーのビュー階層を別の XIB に分けます。

## <a name="optimize-image-resources"></a>イメージ リソースを最適化する

アプリケーションが使用するリソースのうち最もコストが高いものとして画像があります。多くの場合、画像は高解像度でキャプチャされます。 そのため、[`UIImageView`](xref:UIKit.UIImageView) でアプリのバンドルから画像を表示する場合は、画像と `UIImageView` のサイズが同じになるようにします。 実行時に画像の拡大縮小を行うと、コストの高い操作になる可能性があります。`UIImageView` が [`UIScrollView`](xref:UIKit.UIScrollView) に埋め込まれている場合は特にそうです。

詳細については、「[Cross-Platform Performance](~/cross-platform/deploy-test/memory-perf-best-practices.md)」(クロスプラットフォーム パフォーマンス) ガイドの「[Optimize Image Resources](~/cross-platform/deploy-test/memory-perf-best-practices.md#optimizeimages)」(イメージ リソースの最適化) を参照してください。

## <a name="test-on-devices"></a>デバイスでテストする

物理デバイスでのアプリケーションの展開とテストは、できるだけ早く始めます。 シミュレーターは、デバイスの動作と制限とまったく同じではありません。そのため、できるだけはやく実際のデバイスのシナリオでテストすることが重要です。

特に、シミュレーターでは、物理デバイスのメモリまたは CPU の制限をシミュレートすることはできません。

## <a name="synchronize-animations-with-the-display-refresh"></a>アニメーションと表示の更新を同期する

ゲームには、ゲーム ロジックを実行し、画面を更新する厳格なループが含まれる傾向があります。 一般的なフレーム レート範囲は 1 秒あたり 30 から 60 フレームです。 開発者によっては、1 秒あたりの画面の更新回数を可能な限り多くした方がよいと考え、ゲームのシミュレーションを画面の更新を組み合わせて、1 秒あたりのフレーム数を 60 超にしようとするかもしれません。

一方、ディスプレイ サーバーは 1 秒あたり 60 下位の上限で画面の更新を実行します。 そのため、この上限よりも高速に画面を更新しようとすると、画面の停止や途切れが発生する可能性があります。 画面の更新がディスプレイの更新と同期するようにコードを作成することをお勧めします。 この同期には、[`CoreAnimation.CADisplayLink`](xref:CoreAnimation.CADisplayLink) クラスを使用できます。このクラスは、1 秒あたり 60 フレームで実行される視覚化やゲームに適したタイマーです。

## <a name="avoid-core-animation-transparency"></a>コア アニメーションの透過を避ける

コア アニメーションの透過を避けることで、ビットマップ作成のパフォーマンスが改善されます。 一般的に、可能な限り透過レイヤーと罫線のぼかしを避けることをお勧めします。

## <a name="avoid-code-generation"></a>コード生成を避ける

`System.Reflection.Emit` または*動的言語ランタイム*による動的なコード生成は避ける必要があります。これは、iOS カーネルが動的なコード実行を回避するからです。

## <a name="summary"></a>まとめ

この記事では、Xamarin.iOS でビルドされたアプリケーションのパフォーマンスを高めるための手法について説明しました。 これらの手法をすべて使用することで、CPU で実行される作業量や、アプリケーションで消費されるメモリ量を大幅に減らすことができます。

## <a name="related-links"></a>関連リンク

- [クロスプラットフォームのパフォーマンス](~/cross-platform/deploy-test/memory-perf-best-practices.md)
