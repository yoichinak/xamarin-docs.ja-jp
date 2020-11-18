---
title: '第 11 章の概要: バインド可能なインフラストラクチャ'
description: 'Xamarin.Forms でモバイル アプリを作成する: 第 11 章の概要: バインド可能なインフラストラクチャ'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e2858d0606cf9c5c97a3457b5b29f620e7da2bad
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93375136"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>第 11 章の概要: バインド可能なインフラストラクチャ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)

> [!NOTE]
> この本は 2016 年春に発行されて以降、改訂されていません。 多くの情報はまだ価値がありますが、一部の資料は古くなっており、トピックの中にはまったく正しくないものまたは不完全なものもあります。

すべての C# プログラマは、C# の "*プロパティ*" について理解しています。 プロパティには、*set* アクセサーと *get* アクセサーのどちらか一方または両方が含まれます。 それらは、共通言語ランタイムの "*CLR プロパティ*" と呼ばれることがよくあります。

Xamarin.Forms では、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) クラスによってカプセル化され、[`BindableObject`](xref:Xamarin.Forms.BindableObject) クラスによってサポートされる、"*バインド可能プロパティ*" と呼ばれる拡張プロパティ定義が定義されています。 これらのクラスは関連してはいますが、非常に異なります。`BindableProperty` は、プロパティ自体を定義するために使用されます。`BindableObject` は、バインド可能プロパティを定義するクラスの基底クラスであるという点で `object` に似ています。

## <a name="the-no-locxamarinforms-class-hierarchy"></a>Xamarin.Forms のクラス階層

[**ClassHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy) サンプルでは、リフレクションを使用して Xamarin.Forms のクラス階層が表示され、この階層において `BindableObject` によって果たされる重要な役割が示されています。 `BindableObject` は、`Object` から派生し、それを親クラスとする [`Element`](xref:Xamarin.Forms.Element) から、[`VisualElement`](xref:Xamarin.Forms.VisualElement) が派生します。 さらにこれを親として [`Page`](xref:Xamarin.Forms.Page) と [`View`](xref:Xamarin.Forms.View) が派生し、それは [`Layout`](xref:Xamarin.Forms.Layout) に対する親クラスです。

[![クラス階層の共有のトリプル スクリーンショット](images/ch11fg01-small.png "クラス階層の共有")](images/ch11fg01-large.png#lightbox "クラス階層の共有")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>BindableObject と BindableProperty の詳細

`BindableObject` から派生したクラスでは、多くの CLR プロパティがバインド可能プロパティ "によってサポートされている" と言われます。 たとえば、`Label` クラスの [`Text`](xref:Xamarin.Forms.Label.Text) プロパティは CLR プロパティですが、`Label` クラスでは、`BindableProperty` 型の [`TextProperty`](xref:Xamarin.Forms.Label.TextProperty) という名前のパブリックな静的読み取り専用フィールドも定義されています。

アプリケーションでは、普通に `Label` の `Text` プロパティを設定または取得できます。または、`Label.TextProperty` 引数を指定し、`BindableObject` によって定義されている [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object)) メソッドを呼び出すことで `Text` を設定できます。 同様に、アプリケーションでは、この場合も `Label.TextProperty` 引数を指定して [`GetValue`](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty)) メソッドを呼び出すことにより、`Text` プロパティの値を取得できます。 これについては、[**PropertySettings**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings) サンプルを参照してください。

実際には、`Text` CLR プロパティは、`BindableObject` と `Label.TextProperty` 静的プロパティの組み合わせによって定義された `SetValue` メソッドと `GetValue` メソッドを使用して、完全に実装されます。

`BindableObject` と `BindableProperty` では、以下に対するサポートが提供されます。

- プロパティの既定値の指定
- 現在の値の格納
- プロパティ値を検証するためのメカニズムの提供
- 1 つのクラスの関連するプロパティ間の整合性の維持
- プロパティの変更への応答
- プロパティが変更されようとしているとき、または変更されたときの通知のトリガー
- データ バインディングのサポート
- スタイルのサポート
- 動的リソースのサポート

バインド可能プロパティによってサポートされるプロパティが変更されるたびに、`BindableObject` によって、変更されたプロパティを示す [`PropertyChanged`](xref:Xamarin.Forms.BindableObject.PropertyChanged) イベントが生成されます。 プロパティが同じ値に設定されたときは、このイベントは生成されません。

一部のプロパティは、バインド可能プロパティによってサポートされていません。また、`Span` などの一部の Xamarin.Forms クラスは、`BindableObject` から派生していません。 `BindableObject` で `SetValue` メソッドと `GetValue` メソッドが定義されているため、`BindableObject` から派生したクラスのみがバインド可能プロパティをサポートできます。

`Span` は `BindableObject` から派生していないため、`Text` などのプロパティはどれも、バインド可能プロパティによってサポートされていません。 このため、`Span` の `Text` プロパティで `DynamicResource` を設定すると、前の章の [**DynamicVsStatic**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic) サンプルで例外が発生します。 [**DynamicVsStaticCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode) サンプルでは、`Element` によって定義された [`SetDynamicResource`](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String)) メソッドを使用して、コードで動的リソースを設定する方法が示されています。 最初の引数は `BindableProperty` 型のオブジェクトです。

同様に、`BindableObject` によって定義された [`SetBinding`](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase)) メソッドには、`BindableProperty` 型の最初の引数があります。

## <a name="defining-bindable-properties"></a>バインド可能プロパティの定義

静的な [`BindableProperty.Create`](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) メソッドを使用して独自のバインド可能プロパティを定義し、`BindableProperty` 型の静的な読み取り専用フィールドを作成できます。

これについては、[ **Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) ライブラリの [`AltLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs) クラスで示されています。 そのクラスは `Label` から派生し、フォント サイズをポイント単位で指定できます。 [**PointSizedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText) サンプルを参照してください。

`BindableProperty.Create` メソッドの 4 つの引数は必須です。

- `propertyName`: プロパティのテキスト名 (CLR プロパティ名と同じ)
- `returnType`: CLR プロパティの型
- `declaringType`: プロパティを宣言するクラスの型
- `defaultValue`: プロパティの既定値

`defaultValue` は `object` 型であるため、コンパイラは既定値の型を決定できる必要があります。 たとえば、`returnType` が `double` である場合、`defaultValue` は単に 0 ではなく 0.0 のように設定する必要があります。そうしないと、型の不一致によって実行時に例外が発生します。

また、バインド可能プロパティに次のものが含まれるのもよくあることです。

- `propertyChanged`: プロパティの値が変更されたときに呼び出される静的メソッド。 最初の引数は、プロパティが変更されたクラスのインスタンスです。

`BindableProperty.Create` に対する他の引数は、一般的ではありません。

- `defaultBindingMode`: データ バインディングとの関係で使用されます (「[**第 16 章「データ バインディング」**](chapter16.md)を参照)。
- `validateValue`: 有効な値を確認するためのコールバック
- `propertyChanging`: プロパティが変更されようとしていることを示すコールバック
- `coerceValue`: set 値を別の値に強制的に変換するためのコールバック
- `defaultValueCreate`: クラスのインスタンス間で共有できない既定値を作成するためのコールバック (たとえば、コレクション)

### <a name="the-read-only-bindable-property"></a>読み取り専用のバインド可能プロパティ

バインド可能プロパティは読み取り専用にできます。 読み取り専用のバインド可能プロパティを作成するには、静的メソッド [`BindableProperty.CreateReadOnly`](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate)) を呼び出して、[`BindablePropertyKey`](xref:Xamarin.Forms.BindablePropertyKey) 型のプライベートな静的読み取り専用フィールドを定義する必要があります。

次に、CLR プロパティの `set` アクセサーを `private` として定義し、`BindablePropertyKey` オブジェクトを指定して [`SetValue`](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object)) のオーバーロードを呼び出します。 これにより、プロパティがクラスの外部で設定されるのを防ぐことができます。

これについては、[**BaskervillesCount**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount) サンプルで使用されている [`CountedLabel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs) クラスで示されています。

## <a name="related-links"></a>関連リンク

- [第 11 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [第 11 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [連結可能プロパティ](~/xamarin-forms/xaml/bindable-properties.md)
