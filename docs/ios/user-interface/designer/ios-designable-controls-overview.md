---
title: "IOS 用の Xamarin デザイナーでカスタム コントロール"
description: "IOS 用の Xamarin デザイナーは、プロジェクトで作成、または Xamarin コンポーネント ストアなどの外部ソースから参照されているカスタム コントロールのレンダリングをサポートします。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 83ec11ab6a17717dd9556122745afc8d87959186
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>IOS 用の Xamarin デザイナーでカスタム コントロール

_IOS 用の Xamarin デザイナーは、プロジェクトで作成、または Xamarin コンポーネント ストアなどの外部ソースから参照されているカスタム コントロールのレンダリングをサポートします。_

IOS 用の Xamarin デザイナーは、アプリケーションのユーザー インターフェイスを視覚化するための強力なツールであり、WYSIWYG 編集のほとんどの iOS のビューとコント ローラーの表示のサポートを提供します。 アプリでは、iOS に組み込まれているものを拡張するカスタム コントロールもあります。 これらのカスタム コントロールがいくつかのガイドラインを念頭に書き込まれる場合、iOS デザイナーより豊富な編集エクスペリエンスを提供することも表示できることができます。 このドキュメントではこれらのガイドラインを確認します。

## <a name="requirements"></a>必要条件

次のすべての要件を満たしているコントロールをデザイン サーフェイスにレンダリングします。

1.  直接または間接のサブクラスである[UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)または[UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller)です。 その他の[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)サブクラスは、デザイン画面上にアイコンとして表示されます。
2.  [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/)目標 C. に公開するには
3.  [IntPtr の必要なコンス トラクター](~/ios/internals/api-design/index.md)です。
4.  実装するか、 [IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/)インターフェイスかが、 [DesignTimeVisibleAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DesignTimeVisibleAttribute/)を True に設定します。

上記の要件を満たすコードで定義されているコントロールは、そのを含むプロジェクトは、シミュレーターの場合はコンパイル時に、デザイナーに表示されます。 既定では、すべてのカスタム コントロールが表示されます、**カスタム コンポーネント**のセクションで、**ツールボックス**です。 ただし、 [CategoryAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.CategoryAttribute/)別のセクションを指定するカスタム コントロールのクラスに適用することができます。

デザイナーは、サード パーティ製 Objective C ライブラリの読み込みをサポートしていません。

## <a name="custom-properties"></a>カスタム プロパティ

カスタム コントロールによって宣言されたプロパティは、次の条件が満たされた場合、[プロパティ] パネルに表示されます。

1.  プロパティには、パブリックの get アクセス操作子および set アクセス操作子があります。
1.  プロパティには、 [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/)と同様に、 [BrowsableAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.BrowsableAttribute/)を True に設定します。
1.  プロパティの型が数値型、列挙型、string、bool、 [SizeF](https://developer.xamarin.com/api/type/System.Drawing.SizeF/)、 [UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/)、または[UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/)です。 サポートされている型のリストは、将来拡張できます。


プロパティで装飾できるはまた、 [DisplayNameAttribute](https://developer.xamarin.com/api/type/System.ComponentModel.DisplayNameAttribute/)プロパティ パネルに表示されるラベルを指定します。

## <a name="initialization"></a>初期化

`UIViewController`サブクラスを使用してください、 [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/)デザイナーで作成されたビューに依存するコードのメソッドです。

`UIView`およびその他の`NSObject`サブクラスを[AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/)メソッドはレイアウト ファイルから読み込み後に、カスタム コントロールの初期化を実行する推奨される場所です。 これは、コントロールのコンス トラクターを実行する前に設定される場合、プロパティのパネルで設定されたカスタム プロパティは設定されていないため`AwakeFromNib`と呼びます。


```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        // Initialize the view here.
    }
}
```

コントロールもをする場合はコードから直接作成する、次のように、共通の初期化コードを持つメソッドを作成することがあります。

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
    }
}
```

## <a name="property-initialization-and-awakefromnib"></a>プロパティの初期化と AwakeFromNib

IOS デザイナー内に設定されている値を上書きしないか、カスタム コンポーネントでのデザインのプロパティを初期化するタイミングと場所に注意してください。 たとえば、次のコードを実行します。

```csharp
[Register ("CustomView"), DesignTimeVisible (true)]
public class CustomView : UIView {
    
    [Export ("Counter"), Browsable (true)]
    public int Counter {get; set;}

    public CustomView (IntPtr handle) : base (handle) { }

    public CustomView ()
    {
        // Called when created from code.
        Initialize ();
    }

    public override void AwakeFromNib ()
    {
        // Called when loaded from xib or storyboard.
        Initialize ();
    }

    void Initialize ()
    {
        // Common initialization code here.
        Counter = 0;
    }
}
```

`CustomView`コンポーネントが公開する`Counter`iOS デザイナー内の開発者によって設定できるプロパティです。 どのような値は、デザイナーでの値の内部設定に関係なく、ただし、`Counter`プロパティは常にゼロ (0) になります。 その理由を説明します。

-  インスタンス、`CustomControl`ストーリー ボード ファイルを大ききます。
-  IOS デザイナーで変更されたすべてのプロパティが設定されます (の値を設定するなど、`Counter`に 2 つ (2)、たとえば)。
-  `AwakeFromNib`メソッドが実行され、コンポーネントの呼び出しが行われた`Initialize`メソッドです。
-  内部`Initialize`の値、`Counter`プロパティがゼロ (0) にリセットされます。


上記のような状況を修正するか、初期化、`Counter`プロパティ別の場所 (コンポーネントのコンス トラクターなど) は無効か、`AwakeFromNib`メソッドと呼び出し`Initialize`新機能の外部でさらに初期化が必要なコンポーネントがない場合そのコンス トラクターで現在処理中はします。

## <a name="design-mode"></a>デザイン モード

デザイン サーフェイスにカスタム コントロールは、いくつかの制限に従わなければなりません。

-  アプリ バンドルのリソースでは、デザイン モードで使用できません。 イメージはを介して読み込まれるときに使用可能な[UIImage メソッド](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM)です。
-  Web 要求などの非同期操作は、デザイン モードでない実行してください。 デザイン画面は、アニメーションやコントロールの UI に、他の非同期更新をサポートしていません。


カスタム コントロールを実装できます[IComponent](https://developer.xamarin.com/api/type/System.ComponentModel.IComponent/)を使用して、[切り替え](https://developer.xamarin.com/api/property/System.ComponentModel.ISite.DesignMode/)プロパティを確認して、デザイン画面上にあるかどうか。 この例では、ラベルが表示されます「デザイン モード」で、デザイン画面と"Runtime"実行時に。

```csharp
[Register ("DesignerAwareLabel")]
public class DesignerAwareLabel : UILabel, IComponent {

    #region IComponent implementation

    public ISite Site { get; set; }
    public event EventHandler Disposed;

    #endregion

    public DesignerAwareLabel (IntPtr handle) : base (handle) { }

    public override void AwakeFromNib ()
    {
        if (Site != null &amp;&amp; Site.DesignMode)
            Text = "Design Mode";
        else
            Text = "Runtime";
    }
}
```

常に確認する必要があります、`Site`プロパティ`null`任意のたメンバーにアクセスする前にします。 場合`Site`は`null`コントロールがデザイナーで実行されていないと想定しても安全です。
デザイン モードで`Site`コントロールのコンス トラクターの実行後と前に設定されます`AwakeFromNib`と呼びます。

## <a name="debugging"></a>デバッグ

上記の要件を満たしているコントロールはツールボックスに表示され、サーフェイス上に描画します。
コントロールは表示されず、コントロールまたはその依存関係のいずれかのバグを確認します。

デザイン画面は、その他のコントロールを表示するために継続中に個々 のコントロールによってスローされる例外をキャッチ多くの場合、できます。 コントロールの障害は赤のプレース ホルダーに置き換えられ、感嘆符のアイコンをクリックすると例外のトレースを表示することができます。

 ![](ios-designable-controls-overview-images/exception-box.png "赤のプレース ホルダーと例外の詳細コントロールの障害")

コントロールのデバッグ シンボルがある場合、トレースは、ファイル名および行番号にがあります。 スタック トレース内の行をクリックすると、ソース コードでは、その行にジャンプします。

場合は、デザイナーには、コントロールの障害を特定できない、デザイン サーフェイスの上部にある警告メッセージが表示されます。

 ![](ios-designable-controls-overview-images/info-bar.png "デザイン画面の上部にある警告メッセージ")

完全なレンダリングは、欠陥のあるコントロールを固定またはデザイン サーフェイスから削除されたときに再開されます。

## <a name="summary"></a>まとめ

この記事には、作成し、iOS デザイナーでカスタム コントロールのアプリケーションが導入されました。 まず、デザイン画面に表示して、[プロパティ] パネルにカスタム プロパティを公開するコントロールが満たす必要がある要件を説明します。 分離のコントロールと、デザイン モードのプロパティの初期化コードが見てします。 動作の説明が最後に例外がスローされたときに、この問題を解決する方法です。