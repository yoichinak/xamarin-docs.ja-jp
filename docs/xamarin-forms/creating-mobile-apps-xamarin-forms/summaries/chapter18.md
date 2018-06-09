---
title: 第 18 章の概要です。 MVVM
description: 'Xamarin.Forms を使用したモバイル アプリの作成: 第 18 章の概要です。 MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 4a9e8221828ddd49c10dc4f14dc996acc167ae2f
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240432"
---
# <a name="summary-of-chapter-18-mvvm"></a>第 18 章の概要です。 MVVM

呼ぶことが、基になる、コードから、ユーザー インターフェイスを分離することにより、アプリケーションを設計する最善の方法の 1 つは、*ビジネス ロジック*です。 いくつかの手法が存在するが、XAML ベースの環境に合わせて作成されたものはモデル View-viewmodel または MVVM と呼ばれます。

## <a name="mvvm-interrelationships"></a>MVVM 相互の関係

MVVM アプリケーションには、3 つの層があります。

- モデル データを提供基になる場合がありますファイルを通じてまたは web にアクセスします。
- このビューは、通常 XAML で実装された、ユーザー インターフェイスまたはプレゼンテーション層です。
- ViewModel、接続、モデルとビュー

モデルでは、ViewModel を意識し、ViewModel では、ビューを意識です。 これら 3 つの層は、一般に、次のメカニズムを使用して相互に接続します。

![ビュー、ViewModel、およびビュー](images/ch18fg03.png "MVVM")

多くのプログラムのサイズが小さくより大きなもの) に多くの場合、モデルが存在しない場合またはその機能は、ViewModel に統合します。

## <a name="viewmodels-and-data-binding"></a>ViewModels とデータ バインディング

データ バインディングにして、ViewModel できる必要があります、ViewModel のプロパティが変更されたときに、ビューを通知します。 ViewModel を実装することで、 [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)インターフェイスで、`System.ComponentModel`名前空間。 これは、Xamarin.Forms ではなく、.NET の一部です。 (通常、ViewModels はプラットフォームに依存しないを維持しようとしています)。

`INotifyPropertyChanged`という名前の 1 つのイベントを宣言するインターフェイス[ `PropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)プロパティが変更されたことを示すです。

### <a name="a-viewmodel-clock"></a>ViewModel クロック

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit)ライブラリ型のプロパティを定義する`DateTime`変更がに基づいて、タイマーです。 クラスを実装`INotifyPropertyChanged`発生させると、`PropertyChanged`イベントされるたびに、`DateTime`プロパティが変更されました。

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock)サンプルは、この ViewModel をインスタンス化し、ViewModel へのデータ バインディングを使用して、更新された日付と時刻の情報を表示します。

### <a name="interactive-properties-in-a-viewmodel"></a>ViewModel で対話型のプロパティ

示すように、ViewModel でプロパティがより対話的なことがある、 [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs)含まれているクラスの[ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier)サンプルです。 データ バインドから 2 つの被乗数と乗数値を提供する`Slider`要素とする製品を表示、`Label`です。 ただし、その後は変更されません、ViewModel または分離コード ファイルと XAML でこのユーザー インターフェイスに大幅な変更をすることができます。

### <a name="a-color-viewmodel"></a>色 ViewModel

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit)ライブラリには、RGB および HSL の色のモデルが統合されています。 示されていることが、 [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders)サンプル。

[![TK のトリプル スクリーン ショット](images/ch18fg08-small.png "HSL カラー モデル")](images/ch18fg08-large.png#lightbox "HSL の色のモデル")

### <a name="streamlining-the-viewmodel"></a>ViewModel の合理化

ViewModels のコードを合理化を定義する、`OnPropertyChanged`メソッドを使用して、 [ `CallerMemberName` ](https://developer.xamarin.com/api/type/System.Runtime.CompilerServices.CallerMemberNameAttribute/)属性は、自動的に呼び出し元のプロパティ名を取得します。 [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit)ライブラリではこのと ViewModels の基本クラスを提供します。

## <a name="the-command-interface"></a>コマンド インターフェイス

MVVM はデータ バインディングで動作し、データ バインディングのプロパティと動作する MVVM が処理する際に、不十分なするようので、`Clicked`のイベント、`Button`または`Tapped`のイベント、`TapGestureRecognizer`です。 このようなイベントを処理する ViewModels を許可するには、Xamarin.Forms をサポートしています、*コマンド インターフェイス*です。

コマンド インターフェイスから明らかになって、`Button`を 2 つのパブリック プロパティ。

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) 型の[ `ICommand` ](https://developer.xamarin.com/api/type/System.Windows.Input.ICommand/) (で定義されている、`System.Windows.Input`名前空間)
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) 型の `Object`

ViewModel コマンド インターフェイスをサポートする型のプロパティを定義する必要があります`ICommand`にバインドし、データは、`Command`のプロパティ、`Button`です。 `ICommand`インターフェイスでは、2 つのメソッドと 1 つのイベントを宣言しています。

- [ `Execute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.Execute/p/System.Object/)型の引数を持つメソッド `object`
- A [ `CanExecute` ](https://developer.xamarin.com/api/member/System.Windows.Input.ICommand.CanExecute/p/System.Object/)型の引数を持つメソッド`object`を返す `bool`
- A [ `CanExecuteChanged` ](https://developer.xamarin.com/api/event/System.Windows.Input.ICommand.CanExecuteChanged/)イベント

内部的には、型の各プロパティの設定、ViewModel`ICommand`を実装するクラスのインスタンスに、`ICommand`インターフェイスです。 データ バインディングによって、`Button`が最初に呼び出す、`CanExecute`メソッド、メソッドを返す場合、それ自体または無効に`false`です。 また、ハンドラーを設定、`CanExecuteChanged`イベントと呼び出し`CanExecute`するたびにイベントが発生します。 場合、`Button`は呼び出しが有効な`Execute`メソッドされるたびに、`Button`をクリックします。

Xamarin.Forms、ためによりもいくつかの ViewModels する必要があり、コマンド インターフェイスを既にサポートこれら可能性があります。 Xamarin.Forms が提供する新しい ViewModels Xamarin.Forms でのみ使用されるものの[ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)クラスおよび[ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/)を実装するクラス、`ICommand`インターフェイスです。 ジェネリック型は、引数の型、`Execute`と`CanExecute`メソッドです。

### <a name="simple-method-executions"></a>単純なメソッドの実行

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree)サンプル、ViewModel 内にあるコマンド インターフェイスを使用する方法を示します。 [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs)クラス型の 2 つのプロパティを定義`ICommand`も、最も簡単なに渡される 2 つのプライベート プロパティを定義および[`Command`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/)です。 プログラムにするには、この ViewModel からのデータ バインドが含まれている、 `Command` 2 つのプロパティ`Button`要素。

`Button`要素を簡単に置き換えられます`TapGestureRecognizer`XAML でのオブジェクト コードは変更されません。

### <a name="a-calculator-almost"></a>電卓、ほぼ

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine)サンプルでは両方の使用、`Execute`と`CanExecute`のメソッド`ICommand`です。 使用して、 [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs)ライブラリです。 ViewModel、含む型のプロパティが 6 つ`ICommand`です。 これらがから初期化される、 [ `Command`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/)と[`Command`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/)の`Command`と[`Command<T>`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/)`Command<T>`します。 追加のコンピューターの数値キーはすべてで初期化されたプロパティにバインド`Command<T>`、および`string`への引数`Execute`と`CanExecute`特定のキーを識別します。

## <a name="viewmodels-and-the-application-lifecycle"></a>ViewModels とアプリケーション ライフ サイクル

`AdderViewModel`で使用される、 **AddingMachine**サンプルもという 2 つのメソッドを定義`SaveState`と`RestoreState`です。 これらのメソッドは、スリープ状態になったときに、もう一度起動時に、アプリケーションから呼び出されます。



## <a name="related-links"></a>関連リンク

- [第 18 章の完全なテキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [第 18 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
