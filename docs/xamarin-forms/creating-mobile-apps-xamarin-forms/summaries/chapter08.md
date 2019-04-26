---
title: 第 8 章の概要です。 コードと XAML の調和
description: Xamarin.Forms によるモバイル アプリの作成。第 8 章の概要です。 コードと XAML の調和
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 75f153499edb6d979f9a0269a1439eaf8ca53878
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61334245"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>第 8 章の概要です。 コードと XAML の調和

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)

この章では XAML をより深くについて説明し、特にコードと XAML の相互作用します。

## <a name="passing-arguments"></a>引数の受け渡し

一般的なケースでは、XAML でインスタンス化されるクラスのパブリック コンス トラクター; が必要結果のオブジェクトは、プロパティの設定を使用して初期化されます。 ただし、オブジェクトのインスタンスが作成され初期化されているその他の 2 つの方法はあります。

これらは、汎用的な手法が、ほとんどの場合 MVVM ビュー モデルに関連して使用されます。

### <a name="constructors-with-arguments"></a>引数を持つコンス トラクター

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo)サンプルを使用する方法を示して、`x:Arguments`コンス トラクター引数を指定するタグ。 これらの引数は、引数の型を示す要素タグで区切る必要があります。 .NET の基本のデータ型で、次のタグを使用できます。

- `x:Object`
- `x:Boolean`
- `x:Byte`
- `x:Int16`
- `x:Int32`
- `x:Int64`
- `x:Single`
- `x:Double`
- `x:Decimal`
- `x:Char`
- `x:String`
- `x:TimeSpan`
- `x:Array`
- `x:DateTime`

### <a name="can-i-call-methods-from-xaml"></a>XAML からメソッドを呼び出すことはできますか。

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo)サンプルを使用する方法を示して、`x:FactoryMethod`オブジェクトを作成するために呼び出されるファクトリ メソッドを指定する要素。 このようなファクトリ メソッドは public と static をする必要があり、定義されている型のオブジェクトを作成する必要があります。 (たとえば、 [ `Color.FromRgb` ](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double))メソッドを修飾がパブリックであり、静的な型の値を返しますので`Color`)。内でファクトリ メソッドに引数が指定されて`x:Arguments`タグ。

## <a name="the-xname-attribute"></a>X:name 属性

`x:Name`属性は、名前を指定するのには XAML でインスタンス化されるオブジェクトを使用できます。 これらの名前の規則は、c# 変数名の場合と同様です。 戻り値の後、`InitializeComponent`コンス トラクターで呼び出して、分離コード ファイルは、これらの名前に対応する XAML 要素にアクセスするを参照できます。 名前は、生成された部分クラスのプライベート フィールドに、XAML パーサーによって実際に変換されます。

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock)サンプルの使用を示します`x:Name`して、2 つの分離コード ファイルを許可する`Label`現在の日付と時刻で更新する XAML で定義された要素。

同じ名前は、同じページに複数の要素を使用できません。 これを使用する場合、特定の問題は、`OnPlatform`プラットフォームごとに名前付きオブジェクトを並列で作成します。 [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels)ようなことにより良い方法を示します。

## <a name="custom-xaml-based-views"></a>XAML ベースのカスタム ビュー

XAML のマークアップの繰り返しを回避するためにいくつかの方法はあります。 1 つの一般的な方法は XAML ベースの新しいクラスから派生した[ `ContentView`](xref:Xamarin.Forms.ContentView)します。 この手法の説明については、 [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList)サンプル。 `ColorView`クラスから派生`ContentView`特定の色とその名前を表示するときに、`ColorViewListPage`クラスから派生`ContentPage`通常どおり、明示的に作成のインスタンス数は 17`ColorView`します。

アクセス、 `ColorView` XAML でクラスには、よくという名前の別の XML 名前空間宣言が必要があります`local`同じアセンブリ内のクラス。

## <a name="events-and-handlers"></a>イベントとハンドラー

イベントは、XAML 内のイベント ハンドラーに割り当てることができますが、分離コード ファイルにイベント ハンドラー自体を実装する必要があります。 [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) XAML でのキーボード ユーザー インターフェイスを構築する方法および実装する方法を示しています、`Clicked`分離コード ファイル内のハンドラー。

## <a name="tap-gestures"></a>ジェスチャをタップします。

すべて`View`オブジェクトは、タッチ入力を取得し、その入力からイベントを生成します。 `View`クラスを定義、 [ `GestureRecognizers` ](xref:Xamarin.Forms.View.GestureRecognizers)コレクション プロパティから派生するクラスの 1 つまたは複数のインスタンスを含むことのできる[ `GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer)します。

[ `TapGestureRecognizer` ](xref:Xamarin.Forms.TapGestureRecognizer)生成[ `Tapped` ](xref:Xamarin.Forms.TapGestureRecognizer.Tapped)イベント。 [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap)プログラムは、アタッチする方法を示します`TapGestureRecognizer`オブジェクトを 4 つ`BoxView`偽造品のゲームを作成する要素。

[![Monkey タップの 3 倍になるスクリーン ショット](images/ch08fg07-small.png "まがい物ゲーム")](images/ch08fg07-large.png#lightbox "まがい物ゲーム")

**MonkeyTap**プログラムは、サウンドを本当に必要です。 (を参照してください[[次へ] の章](chapter09.md))。

## <a name="related-links"></a>関連リンク

- [第 8 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [第 8 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [第 8 章F#サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [XAML で引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
