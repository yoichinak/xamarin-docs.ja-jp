---
title: '第 8 章の概要: コードと XAML の調和'
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 8 章の概要: コードと XAML の調和'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 90db8b4f11095a2a56c82c3f563844efbcf7e2b1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136826"
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>第 8 章の概要: コードと XAML の調和

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)

この章では、XAML をより深く掘り下げ、特にコードと XAML の連携方法について説明します。

## <a name="passing-arguments"></a>引数の受け渡し

一般的なケースでは、XAML でインスタンス化されるクラスにはパラメーターなしのパブリック コンストラクターが必要です。生成されたオブジェクトは、プロパティの設定によって初期化されます。 ただし、オブジェクトをインスタンス化および初期化する方法は他にも 2 つあります。

これらは汎用的な手法ですが、ほとんどの場合、MVVM ビュー モデルと関連して使用されます。

### <a name="constructors-with-arguments"></a>引数を持つコンストラクター

[**ParameteredConstructorDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo) サンプルでは、`x:Arguments` タグを使用してコンストラクターの引数を指定する方法が示されています。 これらの引数は、引数の型を示す要素タグで区切る必要があります。 .NET の基本データ型の場合は、次のタグを使用できます。

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

### <a name="can-i-call-methods-from-xaml"></a>XAML からメソッドを呼び出すことはできますか?

[**FactoryMethodDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo) サンプルでは、`x:FactoryMethod` 要素を使用して、オブジェクトを作成するために呼び出されるファクトリ メソッドを指定する方法が示されています。 このようなファクトリ メソッドは、パブリックかつ静的である必要があり、それが定義されている型のオブジェクトが作成される必要があります。 (たとえば、[`Color.FromRgb`](xref:Xamarin.Forms.Color.FromRgb(System.Double,System.Double,System.Double)) メソッドは、パブリックかつ静的であり、`Color` 型の値を返すため、適切です。)ファクトリ メソッドの引数は `x:Arguments` タグ内で指定されます。

## <a name="the-xname-attribute"></a>x:Name 属性

`x:Name` 属性を使用すると、XAML でインスタンス化されたオブジェクトに名前を付けることができます。 この名前の規則は、C# の変数名の規則と同じです。 コンストラクターで `InitializeComponent` 呼び出しが返った後は、分離コード ファイルからこれらの名前を参照し、対応する XAML 要素にアクセスできます。 この名前は、実際には、XAML パーサーによって、生成された部分クラスのプライベート フィールドに変換されます。

[**XamlClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock) サンプルでは、`x:Name` を使用し、分離コード ファイルにおいて XAML で定義されている 2 つの `Label` 要素を現在の日付と時刻に保つ方法が示されています。

同じページ上の複数の要素に同じ名前を使用することはできません。 これは、`OnPlatform` を使用して、各プラットフォームに対して並列に名前付きオブジェクトを作成する場合に特に問題となります。 [**PlatformSpecificLabele**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels) サンプルは、そのような操作を行うためのより優れた方法を示しています。

## <a name="custom-xaml-based-views"></a>XAML ベースのカスタム ビュー

XAML でマークアップの繰り返しを回避するには、いくつかの方法があります。 一般的な手法の 1 つは、[`ContentView`](xref:Xamarin.Forms.ContentView) から派生する新しい XAML ベースのクラスを作成することです。 この手法は、[**ColorViewList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList) サンプルで示されています。 `ColorView` クラスは `ContentView` から派生し、特定の色とその名前を表示します。一方、`ColorViewListPage` クラスは通常どおり `ContentPage` から派生し、明示的に `ColorView` のインスタンスを 17 個作成します。

XAML で `ColorView` クラスにアクセスするには、別の XML 名前空間宣言が必要です。これは通常、同じアセンブリ内のクラスのために `local` と名付けれらます。

## <a name="events-and-handlers"></a>イベントとハンドラー

イベントは XAML のイベント ハンドラーに割り当てることができますが、そのイベント ハンドラー自体は分離コード ファイルで実装する必要があります。 [**XamlKeypad**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad) は、XAML でキーパッド ユーザー インターフェイスを構築する方法と、分離コード ファイルで `Clicked` ハンドラーを実装する方法を示しています。

## <a name="tap-gestures"></a>タップ ジェスチャ

`View` オブジェクトでは、タッチ入力を取得し、その入力からイベントを生成することができます。 `View` クラスでは [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) コレクション プロパティが定義されています。これには、[`GestureRecognizer`](xref:Xamarin.Forms.GestureRecognizer) から派生したクラスのインスタンスを 1 つ以上含めることができます。

[`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) によって [`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) イベントが生成されます。 [**MonkeyTap**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap) プログラムは、`TapGestureRecognizer` オブジェクトを 4 つの `BoxView` 要素にアタッチして、イミテーション ゲームを作成する方法を示しています。

[![モンキー タップのトリプル スクリーンショット](images/ch08fg07-small.png "イミテーション ゲーム")](images/ch08fg07-large.png#lightbox "イミテーション ゲーム")

ただし、実際には、**MonkeyTap** プログラムにはサウンドが必要です。 ([次の章](chapter09.md)をご覧ください。)

## <a name="related-links"></a>関連リンク

- [第 8 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [第 8 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [第 8 章の F# サンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
- [XAML での引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
