---
title: リフレクション API を使用して Xamarin iOS アプリケーションを作成する
description: このドキュメントでは、属性で装飾されたクラスに基づいて UI を作成する Monotouch.dialog 属性ベースのリフレクション API について説明します。
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 6482d0626874b3f2ca5e90efb0e376be60551fd7
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528450"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>リフレクション API を使用して Xamarin iOS アプリケーションを作成する

MT。D リフレクション API を使用すると、MT の属性でクラスを修飾できます。D は、を使用して画面を自動的に作成します。 リフレクション API は、これらのクラスと画面に表示されるものとの間のバインディングを提供します。 この API は、elements API が行う細かい制御を提供していませんが、クラスの装飾に基づいて要素階層を自動的に構築することにより、複雑さが軽減されます。

## <a name="setting-up-mtd"></a>MT を設定しています。A

MT.D は、Xamarin. iOS と共に配布されます。 これを使用するには、Visual Studio 2017 または Visual Studio for Mac で Xamarin. iOS プロジェクトの **[参照]** ノードを右クリックし、 **monotouch.dialog**アセンブリへの参照を追加します。 次に、 `using MonoTouch.Dialog`必要に応じて、ソースコードにステートメントを追加します。

## <a name="getting-started-with-the-reflection-api"></a>リフレクション API の概要

リフレクション API の使用方法は次のように単純です。

1. MT で修飾されたクラスを作成します。D 属性。
1. `BindingContext`インスタンスを作成し、上記のクラスのインスタンスを渡します。 
1. を作成し、を渡し`BindingContext’s` `RootElement`ます。 `DialogViewController` 


リフレクション API の使用方法を示す例を見てみましょう。 この例では、次のように単純なデータ入力画面を作成します。

 [![](reflection-api-walkthrough-images/01-expense-entry.png "この例では、次に示すように単純なデータ入力画面を作成します。")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>MT を使用したクラスの作成。D 属性

リフレクション API を使用するには、まず、属性で修飾されたクラスを使用する必要があります。 これらの属性は MT によって使用されます。D Elements API からオブジェクトを作成するために内部的に。 たとえば、次のクラス定義を考えてみます。

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

では、セクションのヘッダーを`UITableView`設定するために文字列引数を使用して、のセクションが作成されます。 `SectionAttribute` セクションが宣言されると、その後に続くすべてのフィールドが、別のセクションが宣言されるまで、そのセクションに含まれます。
フィールドに対して作成されるユーザーインターフェイス要素の型は、フィールドの型と MT によって異なります。D 属性を装飾します。

たとえば、 `Name`フィールド`string`はで`EntryAttribute`あり、では修飾されています。 この結果、テキスト入力フィールドと指定されたキャプションを含む行がテーブルに追加されます。 同様に`IsApproved` 、フィールド`bool`はであるの`CheckboxAttribute`で、テーブルセルの右側にチェックボックスが表示されます。 MT.このケースでは、属性に指定されていないフィールド名 (スペースを自動的に追加する) を使用してキャプションを作成します。

## <a name="adding-the-bindingcontext"></a>BindingContext の追加

`Expense`クラスを使用するには、を`BindingContext`作成する必要があります。 `BindingContext`は、属性クラスのデータをバインドして要素の階層を作成するクラスです。 作成するには、単にインスタンス化し、属性付きクラスのインスタンスをコンストラクターに渡します。

たとえば、 `Expense`クラスで属性を使用して宣言した UI を追加するには`AppDelegate`、の`FinishedLaunching`メソッドに次のコードを含めます。

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

次に示すように、UI を作成するために必要`BindingContext`な操作`DialogViewController`は、にを追加`RootViewController`し、ウィンドウのとして設定することだけです。

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
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

アプリケーションを実行すると、上に示す画面が表示されます。

### <a name="adding-a-uinavigationcontroller"></a>UINavigationController の追加

ただし、 `BindingContext`に渡された "タスクの作成" というタイトルは表示されないことに注意してください。 これは、 `DialogViewController`が`UINavigatonController`の一部ではないためです。 次に示す`UINavigationController` `UINavigationController`ように、を`RootViewController,`ウィンドウとして追加し、の`DialogViewController`ルートとしてを追加するコードを変更してみましょう。

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

アプリケーションを実行すると、次のスクリーンショットに示す`UINavigationController’s`ように、タイトルがナビゲーションバーに表示されます。

 [![](reflection-api-walkthrough-images/02-create-task.png "アプリケーションを実行すると、UINavigationControllers ナビゲーションバーにタイトルが表示されるようになりました。")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

を含める`UINavigationController`ことにより、MT の他の機能を利用できるようになりました。ナビゲーションが必要な D。 たとえば、 `Expense`クラスに列挙を追加して、経費のカテゴリと MT を定義できます。D は選択画面を自動的に作成します。 例を示すために`Expense` 、次のよう`ExpenseCategory`にフィールドを含めるようにクラスを変更します。

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

アプリケーションを実行すると、次のように、カテゴリのテーブルに新しい行が生成されます。

 [![](reflection-api-walkthrough-images/03-set-details.png "アプリケーションを実行すると、次のように、カテゴリのテーブルに新しい行が表示されます。")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

行を選択すると、次に示すように、アプリケーションは列挙に対応する行を含む新しい画面に移動します。

 [![](reflection-api-walkthrough-images/04-set-category.png "行を選択すると、アプリケーションは、列挙に対応する行を含む新しい画面に移動します。")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>まとめ

この記事では、リフレクション API のチュートリアルについて説明します。 クラスに属性を追加して、表示される内容を制御する方法を説明しました。 また、を`BindingContext`使用して、クラスから作成された要素階層にデータをバインドする方法や、MT の使用方法についても説明しました。を`UINavigationController`持つ D。


## <a name="related-links"></a>関連リンク

- [MTDReflectionWalkthrough (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdreflectionwalkthrough)
- [Monotouch.dialog ダイアログの概要](~/ios/user-interface/monotouch.dialog/index.md)
- [Elements API のチュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [JSON 要素のチュートリアル](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github の Monotouch.dialog ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスの参照](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
