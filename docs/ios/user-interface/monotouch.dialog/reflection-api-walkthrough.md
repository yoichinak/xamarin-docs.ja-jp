---
title: "チュートリアル: リフレクション API を使用してアプリケーションの作成"
description: "要素の API、MonoTouch.Dialog (山に加えて属性ベースのリフレクション API D) も含まれています。 リフレクション API は、山と画面を作成します。D クラスの属性で修飾することと同じくらい簡単です。 この記事では、リフレクション API を使用してアプリケーションを作成する方法を示す説明を提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ec5ca2883c6e109a67ee8a4ecb25fe938d0df4ec
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="walkthrough-creating-an-application-using-the-reflection-api"></a>チュートリアル: リフレクション API を使用してアプリケーションの作成

_要素の API、MonoTouch.Dialog (山に加えて属性ベースのリフレクション API D) も含まれています。リフレクション API は、山と画面を作成します。D クラスの属性で修飾することと同じくらい簡単です。この記事では、リフレクション API を使用してアプリケーションを作成する方法を示す説明を提供します。_


山D リフレクション API により、クラスをその山属性で装飾D は、自動的に画面の作成に使用します。 リフレクション API は、これらのクラスと画面に表示される内容の間のバインドを提供します。 この API は、詳細に API 要素は制御を提供するが、クラス装飾に基づいて要素の階層を自動的に作成して複雑さを軽減します。

 <a name="Getting_Started_with_the_Reflection_API" />


## <a name="getting-started-with-the-reflection-api"></a>リフレクション API の概要

リフレクション API を使用することなどの単純なです。

1.  山で修飾されたクラスを作成します。D 属性。
1.  作成する、`BindingContext`インスタンス、上記のクラスのインスタンスを渡します。 
1.  作成する、`DialogViewController`を渡して、 `BindingContext’s` `RootElement`です。 


リフレクション API を使用する方法を説明する例を見てみましょう。 この例では次のように単純なデータ入力画面を構築します。

 [![](reflection-api-walkthrough-images/01-expense-entry.png "この例でビルドします単純なデータ入力画面は、ここに示すように")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

 <a name="Creating_a_Class_with_MT.D_Attributes" />


## <a name="creating-a-class-with-mtd-attributes"></a>山によるクラスの作成D 属性

リフレクション API を使用する必要があります最初の手順は、属性で修飾されたクラスです。 これらの属性は、山によって使用されます。D を作成するには、内部的には、要素の API からオブジェクトです。 たとえば、次のクラス定義があるとします。

```csharp
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
}
```

`SectionAttribute`のセクションになります、`UITableView`セクションのヘッダーを設定するために使用する文字列引数を持つ、作成します。 セクションが宣言されると、それに続くすべてのフィールドが含まれますセクションでは、別のセクションが宣言されるまで。
フィールドの型と、山によってフィールド用に作成されたユーザー インターフェイス要素の型が異なります修飾して D 属性です。

たとえば、`Name`フィールドは、`string`で装飾されていると、`EntryAttribute`です。 これは、結果、テキスト入力フィールドとキャプションを指定のテーブルに追加された新しい行にします。 同様に、`IsApproved`フィールドは、`bool`で、 `CheckboxAttribute`、その結果、テーブル セルの右側にあるチェック ボックスとテーブルの行にします。 MT.D では、フィールド名にスペースを自動的に追加する属性で指定されていないために、ここでは、キャプションを作成できません。

 <a name="Adding_the_BindingContext" />


## <a name="adding-the-bindingcontext"></a>BindingContext の追加

使用する、`Expense`クラスを作成する必要があります、`BindingContext`です。 A`BindingContext`要素の階層を作成する属性クラスからデータをバインドするクラスです。 1 つを作成するには単にそれをインスタンス化し、属性付きクラスのインスタンスをコンス トラクターに渡します。

例については、UI を使用して宣言を追加する属性、`Expense`クラスでは次のコードを含め、`FinishedLaunching`のメソッド、 `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

UI を作成するには、追加し、`BindingContext`を`DialogViewController`として設定し、`RootViewController`次のように、ウィンドウの。

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
   
        window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        var expense = new Expense ();
        var bctx = new BindingContext (null, expense, "Create a task");
        var dvc = new DialogViewController (bctx.Root);
            
        window.RootViewController = dvc;
        window.MakeKeyAndVisible ();
            
        return true;
}
```

画面で表示される前に示した結果今すぐアプリケーションを実行します。

 <a name="Adding_a_UINavigationController" />


### <a name="adding-a-uinavigationcontroller"></a>UINavigationController を追加します。

ただしに注意してくださいをタイトル「タスクを作成する」に渡されることを`BindingContext`は表示されません。 これは、ため、`DialogViewController`がの一部ではなく、`UINavigatonController`です。 追加するコードを変更してみましょう、`UINavigationController`ウィンドウのとして`RootViewController,`を追加し、`DialogViewController`のルートとして、`UINavigationController`次のように。

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

アプリケーションを実行おにタイトルが表示されます。 これで、`UINavigationController’s`下の例のスクリーン ショットとしてナビゲーション バー。

 [![](reflection-api-walkthrough-images/02-create-task.png "今すぐアプリケーションを実行したときに、タイトルは UINavigationControllers ナビゲーション バーに表示されます。")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

含めることによって、 `UINavigationController`、して利用できるよう山の他の機能ナビゲーションのために必要な D. たとえば、列挙を追加できる、`Expense`経費、および山のカテゴリを定義するクラスD は、選択画面を自動的に作成されます。 示すために、変更、`Expense`クラスに含める、`ExpenseCategory`ようフィールドします。

```csharp
public enum Category
{
        Travel,
        Lodging,
        Books
}
        
public class Expense
{
        …

        [Caption("Category")]
        public Category ExpenseCategory;
}
```

今すぐアプリケーションを実行している結果、新しい行のカテゴリの表に示すように。

 [![](reflection-api-walkthrough-images/03-set-details.png "今すぐアプリケーションを実行している結果のカテゴリのテーブルの新しい行に示すよう")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

行を選択すると結果、アプリケーションを移動する新しい画面へ、列挙型に対応する行を含む次のようになります。

 [![](reflection-api-walkthrough-images/04-set-category.png "行を選択すると、アプリケーション内を移動する新しい画面に、列挙型に対応する行の結果します。")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>まとめ

この記事では、リフレクション API のチュートリアルが表示されます。 表示される内容を制御するクラスに属性を追加する方法を紹介します。 使用する方法を説明した、`BindingContext`山を使用する方法だけでなく、作成される要素の階層のクラスからデータをバインドするにはD、`UINavigationController`です。


## <a name="related-links"></a>関連リンク

- [MTDReflectionWalkthrough (サンプル)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [-スクリーン キャストである Miguel de Icaza により iOS ログイン画面 MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [スクリーン キャスト - MonoTouch.Dialog で iOS ユーザー インターフェイスを簡単に作成](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch ダイアログの概要](~/ios/user-interface/monotouch.dialog/index.md)
- [要素の API のチュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [JSON 要素のチュートリアル](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github の MonoTouch ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスのリファレンス](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
