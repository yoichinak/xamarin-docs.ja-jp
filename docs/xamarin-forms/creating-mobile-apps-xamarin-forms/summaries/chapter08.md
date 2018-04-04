---
title: 第 8 章の概要です。 コードと調和で XAML
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 5970DEEB-1FC9-4F78-B4F6-D403E16D22ED
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: ffd2d508e99508309ec07c6bc65c8d716427bdff
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-8-code-and-xaml-in-harmony"></a>第 8 章の概要です。 コードと調和で XAML

この章では XAML をより深くについて説明し、特にコードと XAML の相互作用します。

## <a name="passing-arguments"></a>引数渡し

一般的には、XAML でインスタンス化されるクラスがあります、パブリック パラメーターなしコンス トラクターです。結果オブジェクトはプロパティの設定を初期化します。 ただし、他の 2 つの方法はオブジェクトのインスタンス化し、初期化されることがあります。

汎用的な手法は、ほとんどの場合 MVVM ビュー モデルに関連して使用されます。

### <a name="constructors-with-arguments"></a>引数を持つコンス トラクター

[ **ParameteredConstructorDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ParameteredConstructorDemo)サンプルを使用する方法を示します、`x:Arguments`タグ コンス トラクター引数を指定します。 これらの引数は、引数の型を示す要素タグで区切る必要があります。 基本的な .NET データ型を使用して、次のタグを使用できます。

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

[ **FactoryMethodDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FactoryMethodDemo)サンプルを使用する方法を示します、`x:FactoryMethod`が呼び出され、オブジェクトを作成するファクトリ メソッドを指定する要素。 このようなファクトリ メソッドがパブリックで静的なをする必要があり、定義されている型のオブジェクトを作成する必要があります。 (たとえば、 [ `Color.FromRgb` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromRgb/p/System.Double/System.Double/System.Double/)) メソッドを修飾ためパブリックで静的な型の値を返します`Color`)。ファクトリ メソッドの引数が内で指定された`x:Arguments`タグ。

## <a name="the-xname-attribute"></a>X:name 属性

`x:Name`属性では、名前にする、XAML でインスタンス化されるオブジェクト。 これらの名前のルールは、c# 変数名と同じです。 戻り値の後、`InitializeComponent`コンス トラクターで呼び出して、分離コード ファイルは、XAML の対応する要素にアクセスするこれらの名前を参照できます。 名前は、生成された部分クラスのプライベート フィールドに、XAML パーサーで実際に変換されます。

[ **XamlClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlClock)サンプルでの使用`x:Name`して、2 つの分離コード ファイルを許可する`Label`現在の日付と時間で更新された XAML で定義された要素。

同じ名前は、同じページ上の複数の要素を使用できません。 これを使用する場合は、特定の問題が、`OnPlatform`プラットフォームごとにオブジェクトの名前を付けたを並列で作成します。 [ **PlatformSpecificLabele** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/PlatformSpecificLabels)ようなことを向上させる方法のサンプルに示します。

## <a name="custom-xaml-based-views"></a>XAML ベースのカスタム ビュー

XAML のマークアップの繰り返しを避けるためにいくつかの方法はあります。 1 つの一般的な方法は XAML ベースの新しいクラスから派生した[ `ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)です。 この手法はではデモンストレーション、 [ **ColorViewList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/ColorViewList)サンプルです。 `ColorView`クラスから派生`ContentView`特定の色とその名前を表示するときに、`ColorViewListPage`クラスから派生`ContentPage`17 インスタンスを作成し、通常どおりに明示的に`ColorView`です。

アクセス、 `ColorView` XAML でのクラスには、よくという別の XML 名前空間宣言が必要があります`local`の同じアセンブリ内のクラス。

## <a name="events-and-handlers"></a>イベントとハンドラー

イベントは、XAML では、イベント ハンドラーに割り当てることができますが、分離コード ファイル内自体、イベント ハンドラーを実装する必要があります。 [ **XamlKeypad** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/XamlKeypad)を実装する方法と、XAML でのキーボード ユーザー インターフェイスを構築する方法を示しています、`Clicked`分離コード ファイル内のハンドラー。

## <a name="tap-gestures"></a>タップ ジェスチャ

どの`View`オブジェクトがタッチ入力を取得し、その入力からのイベントを生成します。 `View`クラスを定義、 [ `GestureRecognizers` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.GestureRecognizers/)から派生したクラスの 1 つまたは複数のインスタンスを含めることができるコレクション プロパティ[ `GestureRecognizer`](https://developer.xamarin.com/api/type/Xamarin.Forms.GestureRecognizer/)です。

[ `TapGestureRecognizer` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)生成[ `Tapped` ](https://developer.xamarin.com/api/event/Xamarin.Forms.TapGestureRecognizer.Tapped/)イベント。 [ **MonkeyTap** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/MonkeyTap)プログラムは、アタッチする方法を示します`TapGestureRecognizer`4 オブジェクト`BoxView`装ってゲームを作成する要素。

[![サル タップのトリプル スクリーン ショット](images/ch08fg07-small.png "模倣ゲーム")](images/ch08fg07-large.png#lightbox "模倣ゲーム")

**MonkeyTap**プログラムは、サウンドを本当に必要です。 (を参照してください[次のチャプター](chapter09.md))。



## <a name="related-links"></a>関連リンク

- [第 8 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf)
- [第 8 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08)
- [第 8 章 f# のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter08/FS/XamlKeypad)
