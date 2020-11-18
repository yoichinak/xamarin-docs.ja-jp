---
title: 第 18 章の概要。 MVVM
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 18 章の概要。 MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 15342144a080f3056c1aaa3441f6d1ec850f4dc2
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374057"
---
# <a name="summary-of-chapter-18-mvvm"></a>第 18 章の概要。 MVVM

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)

> [!NOTE]
> この本は 2016 年春に発行されて以降、改訂されていません。 多くの情報はまだ価値がありますが、一部の資料は古くなっており、トピックの中にはまったく正しくないものまたは不完全なものもあります。

アプリケーションを設計する最良の方法の 1 つは、基になるコード ("*ビジネス ロジック*" とも呼ばれます) からユーザー インターフェイスを分離することです。 いくつかの手法がありますが、XAML ベースの環境に合った手法は Model-View-ViewModel または MVVM と呼ばれます。

## <a name="mvvm-interrelationships"></a>MVVM の相関関係

MVVM アプリケーションには、3 つのレイヤーがあります。

- Model によって、基になるデータが提供されます。ファイルや Web アクセスが使用される場合があります
- View はユーザー インターフェイス レイヤーまたはプレゼンテーション層です。通常は XAML で実装されます
- ViewModel によって、Model と View が接続されます

Model は ViewModel について何も知らず、ViewModel は View について何も知りません。 これら 3 つのレイヤーは、通常、次のメカニズムを使用して相互に接続されます。

![View、ViewModel、View](images/ch18fg03.png "MVVM")

多くの小規模なプログラムでは (より大規模なプログラムでも)、Model が存在しないか、その機能が ViewModel に統合されていることがよくあります。

## <a name="viewmodels-and-data-binding"></a>ViewModel とデータ バインディング

データ バインディングを使用するために、ViewModel では、ViewModel のプロパティが変更されたときに View に通知できる必要があります。 ViewModel では、`System.ComponentModel` 名前空間の [`INotifyPropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) インターフェイスを実装することによってこれを行います。 これは、Xamarin.Forms ではなく .NET の一部です。 (通常、ViewModel はプラットフォームに依存しないようにします。)

`INotifyPropertyChanged` インターフェイスでは、変更されたプロパティを示す [`PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged) という名前の単一のイベントが宣言されます。

### <a name="a-viewmodel-clock"></a>ViewModel の時計

[ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) ライブラリの [`DateTimeViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs) によって、タイマーに基づいて変化する `DateTime` 型のプロパティが定義されます。 このクラスでは `INotifyPropertyChanged` が実装され、`DateTime` プロパティが変更されるたびに `PropertyChanged` イベントが発生します。

[**MvvmClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock) サンプルでは、この ViewModel をインスタンス化し、その ViewModel に対してデータ バインディングを使用して、更新された日付と時刻の情報を表示します。

### <a name="interactive-properties-in-a-viewmodel"></a>ViewModel の対話型プロパティ

[**SimpleMultiplier**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier) サンプルに含まれている [`SimpleMultiplierViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs) クラスが示しているように、ViewModel のプロパティはより対話的にすることができます。 データ バインディングによって、2 つの `Slider` 要素から被乗数と乗数の値が提供され、`Label` でその積が表示されます。 ただし、XAML でこのユーザー インターフェイスに広範な変更を加えつつ、ViewModel や分離コード ファイルにそれに伴う変更を生じさせないようにすることができます。

### <a name="a-color-viewmodel"></a>カラー ViewModel

[ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) ライブラリの [`ColorViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs) では、RGB と HSL のカラー モデルが統合されています。 これは [**HslSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders) サンプルで示されています。

[![TK のトリプル スクリーンショット](images/ch18fg08-small.png "HSL カラー モデル")](images/ch18fg08-large.png#lightbox "HSL カラー モデル")

### <a name="streamlining-the-viewmodel"></a>ViewModel の合理化

ViewModel のコードは、呼び出し元のプロパティ名を自動的に取得する [`CallerMemberName`](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute) 属性を使用して `OnPropertyChanged` メソッドを定義することで、合理化できます。 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit) ライブラリの [`ViewModelBase`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs) クラスでは、これが行われ、ViewModel の基底クラスが提供されます。

## <a name="the-command-interface"></a>コマンド インターフェイス

MVVM ではデータ バインディングが使用され、データ バインディングではプロパティが使用されます。そのため、MVVM は、`Button` の `Clicked` イベントや `TapGestureRecognizer` の `Tapped` イベントを処理する場合には不完全であるように見えます。 ViewModel でこのようなイベントを処理できるようにするために、Xamarin.Forms では "*コマンド インターフェイス*" がサポートされています。

コマンド インターフェイスは、`Button` 内で次の 2 つのパブリック プロパティを使用して明示されます。

- [`ICommand`](xref:System.Windows.Input.ICommand) 型 (`System.Windows.Input` 名前空間で定義されています) の [`Command`](xref:Xamarin.Forms.Button.Command)
- `Object` 型の [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter)

コマンド インターフェイスをサポートするために、ViewModel では、後で `Button`の `Command` プロパティにデータ バインディングされる `ICommand` 型のプロパティを定義する必要があります。 `ICommand` インターフェイスでは、2 つのメソッドと 1 つのイベントが宣言されています。

- `object` 型の引数を持つ [`Execute`](xref:System.Windows.Input.ICommand.Execute(System.Object)) メソッド
- `object` 型の引数を持ち、`bool` を返す [`CanExecute`](xref:System.Windows.Input.ICommand.CanExecute(System.Object)) メソッド
- [`CanExecuteChanged`](xref:System.Windows.Input.ICommand.CanExecuteChanged) イベント

ViewModel では、内部的に、`ICommand` 型の各プロパティが、`ICommand` インターフェイスを実装するクラスのインスタンスに設定されます。 データ バインディングを通じて、`Button` では最初に `CanExecute` メソッドが呼び出され、そのメソッドが `false` を返す場合はそれ自体が無効になります。 また、`CanExecuteChanged` イベントのハンドラーも設定され、そのイベントが発生するたびに `CanExecute` が呼び出されます。 `Button` が有効になっている場合は、`Button` がクリックされるたびに `Execute` メソッドが呼び出されます。

Xamarin.Forms よりも前から存在する ViewModel を利用している場合があり、それらで既に、コマンド インターフェイスがサポートされていることがあります。 Xamarin.Forms とだけ使用することを想定された新しい ViewModel のために、Xamarin.Forms には、`ICommand` インターフェイスを実装する [`Command`](xref:Xamarin.Forms.Command) クラスと [`Command<T>`](xref:Xamarin.Forms.Command`1) クラスが用意されています。 ジェネリック型は、`Execute` および `CanExecute` メソッドに対する引数の型です。

### <a name="simple-method-executions"></a>シンプルなメソッドの実行

[**PowersOfThree**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree) サンプルでは、ViewModel でコマンド インターフェイスを使用する方法が示されています。 [`PowersViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs) クラスでは、`ICommand` 型の 2 つのプロパティと、最もシンプルな [`Command` コンストラクター](xref:Xamarin.Forms.Command.%23ctor(System.Action))に渡される 2 つのプライベート プロパティが定義されています。 このプログラムには、この ViewModel から 2 つの `Button` 要素の `Command` プロパティへのデータ バインディングが含まれています。

`Button` 要素は、コードを変更せずに、XAML で `TapGestureRecognizer` オブジェクトに簡単に置き換えることができます。

### <a name="a-calculator-almost"></a>(ほぼ) 電卓

[**AddingMachine**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine) サンプルでは、`ICommand` の `Execute` メソッドと `CanExecute` メソッドの両方が使用されています。 [ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) ライブラリの [`AdderViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs) クラスが使用されています。 ViewModel には、`ICommand` 型のプロパティが 6 つ含まれています。 これらは、`Command` の [`Command` コンストラクター](xref:Xamarin.Forms.Command.%23ctor(System.Action))および [`Command` コンストラクター](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean}))と、`Command<T>` の [`Command<T>` コンストラクター](/dotnet/api/xamarin.forms.command.-ctor?view=xamarin-forms#Xamarin_Forms_Command__ctor_System_Action_System_Object__System_Func_System_Object_System_Boolean__)から初期化されます。 足し算プログラムの数値キーは、すべて `Command<T>` で初期化されるプロパティにバインドされます。また、`Execute` と `CanExecute` に対する `string` 引数によって、特定のキーが識別されます。

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModel とアプリケーションのライフサイクル

**AddingMachine** サンプルで使用されている `AdderViewModel` では、`SaveState` と `RestoreState` という名前の 2 つのメソッドも定義されています。 これらのメソッドは、アプリケーションがスリープ状態になるときと再び起動するときに呼び出されます。

## <a name="related-links"></a>関連リンク

- [第 18 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [第 18 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
- [Xamarin.Forms 電子ブックを使用するエンタープライズ アプリケーション パターン](~/xamarin-forms/enterprise-application-patterns/index.md)
