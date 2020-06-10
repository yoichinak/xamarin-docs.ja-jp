---
title: リフレクション API を使用して Xamarin iOS アプリケーションを作成する
description: このドキュメントでは、属性で装飾されたクラスに基づいて UI を作成する Monotouch.dialog 属性ベースのリフレクション API について説明します。
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 1a6391c0e626c60fe35acee61f55f2f202f077b8
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573444"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>リフレクション API を使用して Xamarin iOS アプリケーションを作成する

MT。D リフレクション API を使用すると、MT の属性でクラスを修飾できます。D は、を使用して画面を自動的に作成します。 リフレクション API は、これらのクラスと画面に表示されるものとの間のバインディングを提供します。 この API は、elements API が行う細かい制御を提供していませんが、クラスの装飾に基づいて要素階層を自動的に構築することにより、複雑さが軽減されます。

## <a name="setting-up-mtd"></a>MT を設定しています。A

MT.D は、Xamarin. iOS と共に配布されます。 これを使用するには、Visual Studio 2017 または Visual Studio for Mac で Xamarin. iOS プロジェクトの [**参照**] ノードを右クリックし、 **monotouch.dialog**アセンブリへの参照を追加します。 次に、 `using MonoTouch.Dialog` 必要に応じて、ソースコードにステートメントを追加します。

## <a name="getting-started-with-the-reflection-api"></a>リフレクション API の概要

リフレクション API の使用方法は次のように単純です。

1. MT で修飾されたクラスを作成します。D 属性。
1. インスタンスを作成し `BindingContext` 、上記のクラスのインスタンスを渡します。 
1. を作成し、を `DialogViewController` 渡し `BindingContext’s` `RootElement` ます。 

リフレクション API の使用方法を示す例を見てみましょう。 この例では、次のように単純なデータ入力画面を作成します。

 [![](reflection-api-walkthrough-images/01-expense-entry.png "In this example, we'll build a simple data entry screen as shown here")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

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

では、 `SectionAttribute` セクションのヘッダーを `UITableView` 設定するために文字列引数を使用して、のセクションが作成されます。 セクションが宣言されると、その後に続くすべてのフィールドが、別のセクションが宣言されるまで、そのセクションに含まれます。
フィールドに対して作成されるユーザーインターフェイス要素の型は、フィールドの型と MT によって異なります。D 属性を装飾します。

たとえば、 `Name` フィールドはであり、 `string` では修飾されてい `EntryAttribute` ます。 この結果、テキスト入力フィールドと指定されたキャプションを含む行がテーブルに追加されます。 同様に、フィールドはであるので、テーブル `IsApproved` `bool` `CheckboxAttribute` セルの右側にチェックボックスが表示されます。 MT.このケースでは、属性に指定されていないフィールド名 (スペースを自動的に追加する) を使用してキャプションを作成します。

## <a name="adding-the-bindingcontext"></a>BindingContext の追加

クラスを使用するに `Expense` は、を作成する必要があり `BindingContext` ます。 は、 `BindingContext` 属性クラスのデータをバインドして要素の階層を作成するクラスです。 作成するには、単にインスタンス化し、属性付きクラスのインスタンスをコンストラクターに渡します。

たとえば、クラスで属性を使用して宣言した UI を追加するに `Expense` は、のメソッドに次のコードを含め `FinishedLaunching` `AppDelegate` ます。

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

次に示すように、UI を作成するために必要な操作は、 `BindingContext` にを追加 `DialogViewController` し、ウィンドウのとして設定することだけです `RootViewController` 。

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

ただし、に渡された "タスクの作成" というタイトルは表示されないことに注意して `BindingContext` ください。 これは、 `DialogViewController` がの一部ではないためです `UINavigatonController` 。 `UINavigationController` `RootViewController,` `DialogViewController` 次に示すように、をウィンドウとして追加し、のルートとしてを追加するコードを変更してみましょう `UINavigationController` 。

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

アプリケーションを実行すると、次のスクリーンショットに示すように、タイトルがナビゲーションバーに表示され `UINavigationController’s` ます。

 [![](reflection-api-walkthrough-images/02-create-task.png "Now when we run the application, the title appears in the UINavigationControllers navigation bar")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

を含めることにより `UINavigationController` 、MT の他の機能を利用できるようになりました。ナビゲーションが必要な D。 たとえば、クラスに列挙を追加して、 `Expense` 経費のカテゴリと MT を定義できます。D は選択画面を自動的に作成します。 例を示すために、次のよう `Expense` にフィールドを含めるようにクラスを変更し `ExpenseCategory` ます。

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

 [![](reflection-api-walkthrough-images/03-set-details.png "Running the application now results in a new row in the table for the category as shown")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

行を選択すると、次に示すように、アプリケーションは列挙に対応する行を含む新しい画面に移動します。

 [![](reflection-api-walkthrough-images/04-set-category.png "Selecting the row results in the application navigating to a new screen with rows corresponding to the enumeration")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary"></a>

## <a name="summary"></a>まとめ

この記事では、リフレクション API のチュートリアルについて説明します。 クラスに属性を追加して、表示される内容を制御する方法を説明しました。 また、を使用して、 `BindingContext` クラスから作成された要素階層にデータをバインドする方法や、MT の使用方法についても説明しました。を持つ D `UINavigationController` 。

## <a name="related-links"></a>関連リンク

- [MTDReflectionWalkthrough (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdreflectionwalkthrough)
- [Monotouch.dialog ダイアログの概要](~/ios/user-interface/monotouch.dialog/index.md)
- [Elements API のチュートリアル](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [JSON 要素のチュートリアル](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Github の Monotouch.dialog ダイアログ](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation アプリケーション](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController クラスのリファレンス](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController クラスの参照](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
