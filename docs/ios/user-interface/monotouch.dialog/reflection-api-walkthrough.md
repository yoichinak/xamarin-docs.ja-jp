---
title: リフレクション API を使用して Xamarin.iOS アプリケーションを作成します。
description: このドキュメントでは、MonoTouch.Dialog 属性に基づくリフレクション API、属性で修飾されたクラスに基づく UI を作成します。 これについて説明します。
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 9ea31d977352c5cc9609136c74099c99c08bdc30
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675184"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>リフレクション API を使用して Xamarin.iOS アプリケーションを作成します。

MTD リフレクション API では、あるクラスを使用できますその山属性で修飾された。D では、自動的に画面を作成します。 リフレクション API は、これらのクラスと画面に表示される内容の間のバインドを提供します。 この API が API の要素をきめ細かく制御を提供していないが、クラス装飾に基づいて要素の階層を自動的に作成することによりの複雑さを軽減します。

## <a name="setting-up-mtd"></a>山の設定D

MT.D は、Xamarin.iOS で分散されます。 これを使用するを右クリックし、**参照**、Xamarin.iOS のノードは、Visual Studio 2017 または Visual Studio for Mac プロジェクトし、への参照を追加、 **MonoTouch.Dialog 1**アセンブリ。 次に、追加`using MonoTouch.Dialog`ソース内のステートメントは、必要に応じてコードします。

## <a name="getting-started-with-the-reflection-api"></a>リフレクション API の概要

リフレクション API を使用すると、同じくらい簡単です。

1.  山で修飾されたクラスを作成します。D 属性。
1.  作成、`BindingContext`インスタンス、上記のクラスのインスタンスを渡します。 
1.  作成、`DialogViewController`を渡して、 `BindingContext’s` `RootElement` 。 


リフレクション API を使用する方法を説明する例を見てみましょう。 この例では、次に示すように単純なデータ入力画面を作成します。

 [![](reflection-api-walkthrough-images/01-expense-entry.png "次に示すよう、この例で単純なデータ入力画面を構築しますします")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>山によるクラスの作成D 属性

リフレクション API を使用する必要があります。 まずは、属性で修飾されたクラスです。 これらの属性が山によって使用されます。D を作成するには、内部的には、要素 API からオブジェクトです。 たとえば、次のクラス定義を考えてみます。

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

`SectionAttribute`のセクションになります、`UITableView`セクションのヘッダーを設定するために使用する文字列引数を持つ、作成されます。 セクションが宣言した後、別のセクションが宣言されるまでそれに続くすべてのフィールドはここでは、含まれます。
フィールドの型と MT によってフィールド用に作成されたユーザー インターフェイス要素の型が異なりますD 属性を修飾します。

たとえば、`Name`フィールドは、`string`で装飾されていると、`EntryAttribute`します。 これにより、テキスト入力フィールドとキャプションを指定のテーブルに追加する行。 同様に、`IsApproved`フィールドは、`bool`で、`CheckboxAttribute`表のセルの右側には、チェック ボックスのテーブル行内の結果として得られる。 MT.D を使用して、フィールド名のスペースを自動的に追加する属性で指定されていないために、この場合、キャプションを作成します。

## <a name="adding-the-bindingcontext"></a>BindingContext の追加

使用する、`Expense`クラスを作成する必要があります、`BindingContext`します。 A`BindingContext`は要素の階層を作成する属性付きクラスからデータをバインドするクラスです。 1 つを作成するには、単にインスタンス化お属性付きクラスのインスタンスをコンス トラクターに渡します。

たとえば、UI を使用して宣言を追加する属性、`Expense`クラスを次のコードを含める、`FinishedLaunching`のメソッド、 `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

必要な UI を作成するためには、追加し、`BindingContext`を`DialogViewController`として設定し、 `RootViewController`  ウィンドウで、次に示すの。

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

これでアプリケーションを実行して表示される前に示した画面になります。

### <a name="adding-a-uinavigationcontroller"></a>UINavigationController を追加します。

ただしに注意してください、タイトル「タスクを作成」に渡されることを`BindingContext`は表示されません。 これは、ため、`DialogViewController`がの一部ではなく、`UINavigatonController`します。 追加するコードを変更してみましょう、`UINavigationController`としてウィンドウの`RootViewController,`を追加し、`DialogViewController`のルートとして、`UINavigationController`次に示すよう。

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

アプリケーションを実行にタイトルが表示されます。 これで、`UINavigationController’s`として以下のスクリーン ショットのナビゲーション バー。

 [![](reflection-api-walkthrough-images/02-create-task.png "今すぐアプリケーションを実行すると、タイトルは UINavigationControllers のナビゲーション バーに表示されます。")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

含めることによって、 `UINavigationController`MT の他の機能のメリットを得ることができますナビゲーションのために必要な D. たとえば、列挙を追加できます、`Expense`経費、および山のカテゴリを定義するクラスD は、選択画面を自動的に作成されます。 例として、変更、`Expense`クラスを`ExpenseCategory`次のようにフィールドします。

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

ように、カテゴリのテーブルの新しい行に結果今すぐアプリケーションを実行しています。

 [![](reflection-api-walkthrough-images/03-set-details.png "カテゴリのテーブルの新しい行に示すよう結果今すぐアプリケーションを実行しています。")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

アプリケーション内を移動する新しい画面に、列挙型に対応する行を次に示す結果の行を選択します。

 [![](reflection-api-walkthrough-images/04-set-category.png "アプリケーションに移動する新しい画面を列挙型に対応する行を含む結果の行を選択")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>まとめ

この記事では、リフレクション API のチュートリアルが表示されます。 表示される内容を制御するためのクラスに属性を追加する方法を紹介しました。 使用する方法についても説明します、`BindingContext`クラスから山を使用する方法についても、作成される要素の階層にデータをバインドするにはD、`UINavigationController`します。


## <a name="related-links"></a>関連リンク

- [MTDReflectionWalkthrough (サンプル)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [MonoTouch.Dialog とスクリーン キャスト - Miguel de Icaza の作成、iOS のログイン画面](http://youtu.be/3butqB1EG0c)
- [スクリーン キャスト - MonoTouch.Dialog で iOS ユーザー インターフェイスを簡単に作成](http://youtu.be/j7OC5r8ZkYg)
- [MonoTouch ダイアログの概要](~/ios/user-interface/monotouch.dialog/index.md)
- [要素の API のチュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [JSON 要素のチュートリアル](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github の MonoTouch ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスのリファレンス](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
