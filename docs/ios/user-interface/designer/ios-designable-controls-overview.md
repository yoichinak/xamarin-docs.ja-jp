---
title: IOS 用の Xamarin のデザイナーでカスタム コントロール
description: IOS 用の Xamarin デザイナーでは、プロジェクトの作成または Xamarin コンポーネント ストアなどの外部ソースから参照されているカスタム コントロールのレンダリングをサポートします。
ms.prod: xamarin
ms.assetid: D8F07D63-B006-4050-9D1B-AC6FCDA71B99
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 00bf7290d5f7165feb5b67cd91c15a96b7d3eaf8
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118371"
---
# <a name="custom-controls-in-the-xamarin-designer-for-ios"></a>IOS 用の Xamarin のデザイナーでカスタム コントロール

_IOS 用の Xamarin デザイナーでは、プロジェクトの作成または Xamarin コンポーネント ストアなどの外部ソースから参照されているカスタム コントロールのレンダリングをサポートします。_

IOS 用の Xamarin のデザイナーは、アプリケーションのユーザー インターフェイスを視覚化するための強力なツールであり、WYSIWYG 編集のほとんどの iOS ビューとビュー コント ローラーのサポートを提供します。 アプリは、iOS に組み込まれているものを拡張するカスタム コントロールを含めることもできます。 いくつかのガイドラインを考慮してでは、これらのカスタム コントロールが書き込まれた場合、iOS Designer より豊富な編集機能を提供することによってもレンダリングされることができます。 このドキュメントではこれらのガイドラインを参照してください。

## <a name="requirements"></a>必要条件

次のすべての要件を満たすコントロールをデザイン サーフェイスにレンダリングします。

1.  直接または間接的なサブクラスが[UIView](https://developer.xamarin.com/api/type/UIKit.UIView/)または[UIViewController](https://developer.xamarin.com/api/type/UIKit.UIView/Controller)します。 その他の[NSObject](https://developer.xamarin.com/api/type/Foundation.NSObject/)サブクラスは、デザイン サーフェイス上のアイコンとして表示されます。
2.  [RegisterAttribute](https://developer.xamarin.com/api/type/Foundation.RegisterAttribute/) OBJECTIVE-C に公開するには
3.  [必要な IntPtr コンス トラクター](~/ios/internals/api-design/index.md)します。
4.  実装するか、 [IComponent](xref:System.ComponentModel.IComponent)インターフェイスまたはが、 [DesignTimeVisibleAttribute](xref:System.ComponentModel.DesignTimeVisibleAttribute)を True に設定します。

上記の要件を満たすコードで定義されているコントロールは、シミュレーターに含まれるプロジェクトがコンパイルされるときに、デザイナーに表示されます。 既定ではすべてのカスタム コントロールに表示されます、**カスタム コンポーネント**のセクション、**ツールボックス**します。 ただし、 [CategoryAttribute](xref:System.ComponentModel.CategoryAttribute)別のセクションを指定するカスタム コントロールのクラスに適用することができます。

デザイナーは、サード パーティ製の OBJECTIVE-C ライブラリの読み込みをサポートしていません。

## <a name="custom-properties"></a>カスタム プロパティ

カスタム コントロールで宣言されたプロパティは、次の条件が満たされた場合、[プロパティ] パネルに表示されます。

1.  プロパティには、パブリック ゲッターとセッターがあります。
1.  プロパティは、 [ExportAttribute](https://developer.xamarin.com/api/type/Foundation.ExportAttribute/)と同様に、 [BrowsableAttribute](xref:System.ComponentModel.BrowsableAttribute)を True に設定します。
1.  プロパティの型が数値型、列挙型、string、bool、 [SizeF](xref:System.Drawing.SizeF)、[示す UIColor](https://developer.xamarin.com/api/type/UIKit.UIColor/)、または[UIImage](https://developer.xamarin.com/api/type/UIKit.UIImage/)します。 サポートされている型のこの一覧は、将来拡張可能性があります。


プロパティで装飾できるはまた、 [DisplayNameAttribute](xref:System.ComponentModel.DisplayNameAttribute)プロパティ パネルに表示されるラベルを指定します。

## <a name="initialization"></a>初期化

`UIViewController`サブクラスを使用する必要がある、 [ViewDidLoad](https://developer.xamarin.com/api/member/UIKit.UIViewController.ViewDidLoad/)デザイナーで作成したビューに依存するコードのメソッド。

`UIView`およびその他の`NSObject`サブクラスを[AwakeFromNib](https://developer.xamarin.com/api/member/Foundation.NSObject.AwakeFromNib/)メソッドは、レイアウト ファイルから読み込まれるカスタム コントロールの初期化の実行をお勧めします。 これは、コントロールのコンス トラクターを実行するが、前に設定する、ときに、[プロパティ] パネルで設定されたカスタム プロパティは設定されていないため`AwakeFromNib`が呼び出されます。


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

場合は、コントロールのコードから直接作成する設計もメソッドがこのような一般的な初期化コードを作成したい場合があります。

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

IOS Designer 内で設定されている値を上書きしないか、カスタム コンポーネントでデザイン可能なプロパティを初期化するタイミングと場所に注意する必要があります。 たとえば、次のコードを実行します。

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

`CustomView`コンポーネントが公開、 `Counter` iOS デザイナー内の開発者によって設定できるプロパティです。 ただし、どのような値は、デザイナーの値の内部設定に関係なく、`Counter`プロパティがゼロ (0) になります。 その理由を説明します。

-  インスタンス、`CustomControl`ストーリー ボード ファイルから膨らみます。
-  IOS デザイナーで変更されたプロパティが設定されます (の値を設定するなど、`Counter`に 2 つ (2)、たとえば)。
-  `AwakeFromNib`メソッドが実行され、コンポーネントの呼び出しが行われた`Initialize`メソッド。
-  内部`Initialize`の値、`Counter`プロパティがゼロ (0) にリセットされます。


上記のような状況を修正するか、初期化、`Counter`プロパティ別の場所 (コンポーネントのコンス トラクターなど) をオーバーライドしないか、`AwakeFromNib`メソッドと呼び出し`Initialize`何の外部でさらに初期化が必要なコンポーネントがない場合そのコンス トラクターによって現在処理中です。

## <a name="design-mode"></a>デザイン モード

デザイン サーフェイスでは、カスタム コントロールは、いくつかの制限に従う必要があります。

-  デザイン モードでアプリ バンドルのリソースが利用できません。 イメージを経由で読み込まれたときに使用できます。 [UIImage メソッド](https://developer.xamarin.com/api/type/UIKit.UIImage/%2fM)します。
-  デザイン モードで、web 要求などの非同期操作を実行しない必要があります。 デザイン画面は、アニメーションまたはコントロールの UI に、他の非同期更新をサポートしていません。


カスタム コントロールを実装できます[IComponent](xref:System.ComponentModel.IComponent)を使用して、[ください](xref:System.ComponentModel.ISite.DesignMode)プロパティをデザイン画面であるかを確認します。 この例でラベルに表示されます「デザイン モード」で、デザイン画面と「ランタイム」時。

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

常に確認する必要があります、`Site`プロパティ`null`前に、そのメンバーのいずれかにアクセスしようとしています。 場合`Site`は`null`コントロールがデザイナーで実行されていないと考えても安全です。
デザイン モードで`Site`コントロールのコンス トラクターが実行された後と前に設定されます`AwakeFromNib`が呼び出されます。

## <a name="debugging"></a>デバッグ

上記の要件を満たすコントロールはツールボックスに表示され、表面に表示されます。
コントロールを表示しない場合は、コントロールまたはその依存関係のいずれかのバグを確認します。

デザイン画面では、他のコントロールを表示するためにも引き続き個別のコントロールによってスローされた例外をキャッチできます多くの場合。 コントロールの障害は、赤のプレース ホルダーに置き換えられ、感嘆符のアイコンをクリックして、例外トレースを表示することができます。

 ![](ios-designable-controls-overview-images/exception-box.png "赤のプレース ホルダーと例外の詳細コントロールの障害")

デバッグ シンボルがコントロールで使用できる場合は、トレース ファイル名や行番号にがあります。 スタック トレース内の行をクリックすると、ソース コードでは、その行にジャンプします。

デザイナーをコントロールの障害ときない、デザイン画面の上部に警告メッセージが表示されます。

 ![](ios-designable-controls-overview-images/info-bar.png "デザイン画面の上部に警告メッセージ")

完全なレンダリングは、欠陥のあるコントロールを固定またはデザイン画面から削除されたときに再開されます。

## <a name="summary"></a>まとめ

この記事には、作成し、iOS デザイナーでカスタム コントロールのアプリケーションが導入されました。 まず、デザイン画面に表示して、[プロパティ] パネルでのカスタム プロパティを公開するコントロールを満たす必要のある要件を説明します。 -コントロールと切り替えプロパティの初期化の分離コードで照合されます。 動作の説明が最後に例外がスローされたときに、この問題を解決する方法。