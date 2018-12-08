---
title: 第 18 章の概要です。 MVVM
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 18 章の概要。 MVVM'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 6A774510-7709-4F60-8EF5-29D478176F8F
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
ms.openlocfilehash: db837ac8bfa1b7a946ee606e9481f9feb2a8a31f
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050126"
---
# <a name="summary-of-chapter-18-mvvm"></a>第 18 章の概要です。 MVVM

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)

呼ばれます、基になる、コードからのユーザー インターフェイスを分離することで、アプリケーションを構築する最善の方法の 1 つは、*ビジネス ロジック*します。 いくつかの手法が存在するが、XAML ベースの環境用に調整されているがモデル-ビュー-ビューモデルまたは MVVM と呼ばれます。

## <a name="mvvm-interrelationships"></a>MVVM の相互関係

MVVM アプリケーションでは、3 つの層があります。

- モデルを使用する、データを基になる場合がありますファイルまたは web にアクセスします。
- ビューは、通常 XAML で実装された、ユーザー インターフェイスまたはプレゼンテーション層
- モデルとビュー、ビューモデルが接続します。

モデルでは、ViewModel を意識し、ViewModel はビューの依存します。 これら 3 つの層は、一般に、次のメカニズムを使用して相互に接続します。

![ビュー、ビューモデル、およびビュー](images/ch18fg03.png "MVVM")

多くの小さいプログラム (とさらに大きなもの) に多くの場合、モデルが存在しないまたはその機能は、ViewModel に統合します。

## <a name="viewmodels-and-data-binding"></a>ビューモデル、およびデータ バインディング

データ バインドに参加して、ViewModel は、ViewModel のプロパティが変更されたときに、ビューに通知の対応にあります。 ビューモデルを実装することによって、 [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged)インターフェイスで、`System.ComponentModel`名前空間。 これは、Xamarin.Forms ではなく、.NET の一部です。 (通常、Viewmodel はプラットフォームの独立性を維持しようとしています)。

`INotifyPropertyChanged`インターフェイスという名前の 1 つのイベントを宣言します[ `PropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged)プロパティが変更されたことを示します。

### <a name="a-viewmodel-clock"></a>ViewModel クロック

[ `DateTimeViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/DateTimeViewModel.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit)ライブラリは、型のプロパティを定義します。`DateTime`変更が、タイマーに基づきます。 クラス実装`INotifyPropertyChanged`を発生させる、`PropertyChanged`イベントたびに、`DateTime`プロパティの変更。

[ **MvvmClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/MvvmClock)サンプルは、このビューモデルをインスタンス化し、ViewModel にデータ バインドを使用して、更新された日付と時刻の情報を表示します。

### <a name="interactive-properties-in-a-viewmodel"></a>ViewModel で対話型のプロパティ

対話形式より、ViewModel のプロパティに示すように、 [ `SimpleMultiplierViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/SimpleMultiplier/SimpleMultiplier/SimpleMultiplier/SimpleMultiplierViewModel.cs)クラスは、一部の[ **SimpleMultiplier** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/SimpleMultiplier)サンプル。 データ バインドから 2 つの被乗数と乗数値を提供する`Slider`要素で製品を表示し、`Label`します。 ただし、このユーザー インターフェイスを XAML でビューモデルまたは分離コード ファイルに見合った変更を加えるに大幅な変更を行うことができます。

### <a name="a-color-viewmodel"></a>色の ViewModel

[ `ColorViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ColorViewModel.cs)で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit)ライブラリには、RGB、HSL カラー モデルが統合されています。 これは、方法については、 [ **HslSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/HslSliders)サンプル。

[![TK の 3 倍になるスクリーン ショット](images/ch18fg08-small.png "HSL カラー モデル")](images/ch18fg08-large.png#lightbox "HSL カラー モデル")

### <a name="streamlining-the-viewmodel"></a>ビューモデルを効率化

定義することによって、ビューモデルのコードを効率化されることができます、`OnPropertyChanged`メソッドを使用して、 [ `CallerMemberName` ](xref:System.Runtime.CompilerServices.CallerMemberNameAttribute)属性には、自動的に呼び出し元のプロパティ名を取得します。 [ `ViewModelBase` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/ViewModelBase.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit)ライブラリは、このと Viewmodel の基本クラスを提供します。

## <a name="the-command-interface"></a>コマンド インターフェイス

MVVM はデータのバインドを操作し、MVVM を処理する際に不十分なことだと思いますので、プロパティを使用、データ バインドが動作を`Clicked`のイベントを`Button`または`Tapped`のイベントを`TapGestureRecognizer`します。 このようなイベントを処理するビューモデルを許可するには、Xamarin.Forms をサポートしています、*コマンド インターフェイス*します。

コマンド インターフェイスをマニフェストで自体、`Button`を 2 つのパブリック プロパティ。

- [`Command`](xref:Xamarin.Forms.Button.Command) 型の[ `ICommand` ](xref:System.Windows.Input.ICommand) (で定義されている、`System.Windows.Input`名前空間)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 型の `Object`

ビューモデル コマンド インターフェイスをサポートする型のプロパティを定義する必要があります`ICommand`しのデータにバインドされている、`Command`のプロパティ、`Button`します。 `ICommand`インターフェイスが 2 つのメソッドと 1 つのイベントを宣言します。

- [ `Execute` ](xref:System.Windows.Input.ICommand.Execute(System.Object))型の引数を持つメソッド `object`
- A [ `CanExecute` ](xref:System.Windows.Input.ICommand.CanExecute(System.Object))型の引数を持つメソッド`object`を返す `bool`
- A [ `CanExecuteChanged` ](xref:System.Windows.Input.ICommand.CanExecuteChanged)イベント

内部的には、型の各プロパティの設定を ViewModel`ICommand`を実装するクラスのインスタンスに、`ICommand`インターフェイス。 データ バインドを介して、`Button`が最初に呼び出す、`CanExecute`メソッド、メソッドを返す場合は無効になり、`false`します。 また、ハンドラーを設定、`CanExecuteChanged`イベントと呼び出し`CanExecute`そのイベントが発生するたびにします。 場合、`Button`が有効にすると、呼び出し、`Execute`メソッドたびに、`Button`がクリックされました。

以前、Xamarin.Forms をいくつかの Viewmodel がある可能性があり、これらコマンド インターフェイスを既にサポート可能性があります。 新しい Viewmodel は、Xamarin.Forms でのみ使用するのに、Xamarin.Forms が提供、 [ `Command` ](xref:Xamarin.Forms.Command)クラスと[ `Command<T>` ](xref:Xamarin.Forms.Command`1)実装するクラス、`ICommand`インターフェイス。 ジェネリック型は、引数の型、`Execute`と`CanExecute`メソッド。

### <a name="simple-method-executions"></a>単純なメソッドの実行

[ **PowersOfThree** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/PowersOfThree)サンプル ViewModel のコマンド インターフェイスを使用する方法を示します。 [ `PowersViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Chapter18/PowersOfThree/PowersOfThree/PowersOfThree/PowersViewModel.cs)クラス型の 2 つのプロパティを定義`ICommand`も、最も簡単なに渡される 2 つのプライベート プロパティを定義および[`Command`コンス トラクター](xref:Xamarin.Forms.Command.%23ctor(System.Action))します。 プログラムにはこの ViewModel からのデータ バインドが含まれています、 `Command` 2 つのプロパティ`Button`要素。

`Button`で、要素を簡単に置き換えることができます`TapGestureRecognizer`コードの変更を XAML 内のオブジェクト。

### <a name="a-calculator-almost"></a>電卓、ほぼ

[ **AddingMachine** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18/AddingMachine)によりサンプルの両方を使用して、`Execute`と`CanExecute`メソッドの`ICommand`します。 使用して、 [ `AdderViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AdderViewModel.cs)ライブラリ。 ViewModel には型の 6 つのプロパティが含まれています`ICommand`します。 これらがから初期化される、 [ `Command`コンス トラクター](xref:Xamarin.Forms.Command.%23ctor(System.Action))と[`Command`コンス トラクター](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean}))の`Command`と[`Command<T>`コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command%3CT%3E.Command%3CT%3E/p/System.Action%7BT%7D/System.Func%7BT,System.Boolean%7D/)`Command<T>`します。 計算機の数値キーはすべて使用して初期化されるプロパティにバインド`Command<T>`と`string`引数`Execute`と`CanExecute`特定のキーを識別します。

## <a name="viewmodels-and-the-application-lifecycle"></a>ビューモデル、およびアプリケーションのライフ サイクル

`AdderViewModel`で使用される、 **AddingMachine**サンプルでは、という 2 つのメソッドも定義します`SaveState`と`RestoreState`します。 これらのメソッドは、もう一度開始日時をスリープ状態になったときに、アプリケーションから呼び出されます。



## <a name="related-links"></a>関連リンク

- [第 18 章のフル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf)
- [第 18 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter18)
- [Xamarin.Forms 電子ブックを使用してエンタープライズ アプリケーション パターン](~/xamarin-forms/enterprise-application-patterns/index.md)
