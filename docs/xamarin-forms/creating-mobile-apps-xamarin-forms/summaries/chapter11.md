---
title: 第 11 章の概要です。 バインド可能なインフラストラクチャ
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 9a35388f9ddd3adbefc3401352874a774376655b
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>第 11 章の概要です。 バインド可能なインフラストラクチャ

すべての c# プログラマが c# に慣れて*プロパティ*です。 プロパティが含まれる、*設定*アクセサー、および*取得*アクセサー。 多くの場合と呼ばれる*CLR プロパティ*共通言語ランタイム用です。

Xamarin.Forms と呼ばれる拡張されたプロパティ定義を定義する、*バインド可能なプロパティ*によってカプセル化、 [ `BindableProperty` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableProperty/)クラスし、でサポートされている、 [ `BindableObject` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindableObject/)クラスです。 これらのクラスは関連がかなり異なります。`BindableProperty`自体; プロパティを定義するために使用`BindableObject`に似ています`object`であることがバインド可能なプロパティを定義するクラスの基本クラスです。

## <a name="the-xamarinforms-class-hierarchy"></a>Xamarin.Forms クラスの階層構造

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy)サンプルは、Xamarin.Forms のクラス階層を表示およびによって実行される重要な役割をデモンストレーションするリフレクションを使用して`BindableObject`この階層にします。 `BindableObject` 派生した`Object`に親クラスでは[ `Element` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Element/)元となる[ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/)派生します。 これは、親クラスに[ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)と[ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)、これは、親クラスを[ `Layout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Layout/):

[![クラスの階層の共有のトリプル スクリーン ショット](images/ch11fg01-small.png "クラス階層の共有")](images/ch11fg01-large.png#lightbox "クラス階層の共有")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>BindableObject と BindableProperty にピーク

派生したクラスで`BindableObject`多くの CLR プロパティを「支えられている」バインド可能なプロパティと呼ばれます。 たとえば、 [ `Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/)のプロパティ、`Label`クラスは、CLR プロパティが、`Label`クラスも、パブリックな静的読み取り専用フィールドという名前を定義[ `TextProperty` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextProperty/)の型`BindableProperty`です。

アプリケーションの設定または取得できます、`Text`プロパティの`Label`通常、アプリケーションを設定できますか、`Text`を呼び出して、 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindableProperty/System.Object/)によって定義されたメソッド`BindableObject`で、 `Label.TextProperty`引数。 同様に、アプリケーションがの値を取得する、`Text`プロパティを呼び出して、 [ `GetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.GetValue/p/Xamarin.Forms.BindableProperty/)メソッドを使用して、`Label.TextProperty`引数。 示されるこれは、 [**設定**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings)サンプルです。

実際には、 `Text` CLR プロパティを実装する完全、`SetValue`と`GetValue`によって定義されたメソッド`BindableObject`と組み合わせて、`Label.TextProperty`静的プロパティです。

`BindableObject` および`BindableProperty`サポートを提供します。

- プロパティの既定値を提供
- 現在の値を格納します。
- プロパティの値を検証するためのメカニズムを提供します。
- 1 つのクラスに関連するプロパティの間で一貫性を維持します。
- プロパティが変更された場合の対処
- プロパティが変更しようかが変更されたときに通知をトリガーします。
- データ バインディングをサポートします。
- スタイルをサポートします。
- 動的なリソースをサポートします。

ときに、バインド可能なプロパティの変更によってバックアップされているプロパティ`BindableObject`発生、 [ `PropertyChanged` ](https://developer.xamarin.com/api/event/Xamarin.Forms.BindableObject.PropertyChanged/)変更されたプロパティを識別するイベントです。 プロパティが同じ値に設定されている場合、このイベントは起動しません。

一部のプロパティは、バインド可能なプロパティ、および Xamarin.Forms の一部のクラスではバックアップされません&mdash;など`Span`&mdash;からは派生しません`BindableObject`です。 派生したクラスのみ`BindableObject`ために、バインド可能なプロパティをサポートできる`BindableObject`定義、`SetValue`と`GetValue`メソッドです。

`Span`から派生していない`BindableObject`、そのプロパティのいずれも&mdash;など`Text`&mdash;バインド可能なプロパティでサポートされます。 その理由は、`DynamicResource`の設定、`Text`プロパティの`Span`で例外が発生、 [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic)サンプルは、前のチャプターです。 [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode)サンプルを使用してコードで動的なリソースを設定する方法を示します、 [ `SetDynamicResource` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Element.SetDynamicResource/p/Xamarin.Forms.BindableProperty/System.String/)によって定義されたメソッド`Element`です。 最初の引数は型のオブジェクト`BindableProperty`です。

同様に、 [ `SetBinding` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetBinding/p/Xamarin.Forms.BindableProperty/Xamarin.Forms.BindingBase/)メソッドによって定義された`BindableObject`型の最初の引数を持つ`BindableProperty`します。

## <a name="defining-bindable-properties"></a>バインド可能なプロパティを定義します。

静的なを使用して、独自のバインド可能なプロパティを定義する[ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/)メソッドまたは若干短く[ `BindableProperty.Create` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.Create/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/)型の静的な読み取り専用フィールドを作成するオーバー ロード`BindableProperty`です。

これに示されている、 [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs)クラス内で、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリです。 クラスの派生元`Label`フォント サイズをポイント単位で指定することができます。 示されていることが、 [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText)サンプルです。

4 つの引数、`BindableProperty.Create`メソッドが必要。

- `propertyName`: (CLR プロパティの名前と同じ) プロパティの名前は、
- `returnType`: CLR プロパティの型
- `declaringType`: プロパティを宣言するクラスの種類
- `defaultValue`: プロパティの既定値

`defaultValue`の種類は`object`コンパイラは、既定値の型を決定できる必要があります。 たとえば場合、`returnType`は`double`、`defaultValue`だけではなく 0.0 0 の場合のように設定する必要がありますまたは型の不一致は、実行時に例外をトリガーします。

バインド可能なプロパティを含めるは非常に一般的です。

- `propertyChanged`: プロパティに値が変更されたときに、静的メソッドが呼び出されます。 最初の引数は、そのプロパティが変更されたクラスのインスタンスです。

他の引数`BindableProperty.Create`と一般的ではありません。

- `defaultBindingMode`: データ バインディングに関連して使用 (で説明したよう[**章 16。データ バインディング**](chapter16.md))
- `validateValue`: 有効な値をチェックするコールバック
- `propertyChanging`: プロパティを変更することを示すコールバック
- `coerceValue`: 他の値の設定値を強制的にコールバック
- `defaultValueCreate`: コールバック クラス (たとえば、コレクション) のインスタンス間で共有することはできませんを既定値を作成するには

### <a name="the-read-only-bindable-property"></a>読み取り専用のバインド可能なプロパティ

バインド可能なプロパティは読み取り専用にできます。 読み取り専用のバインド可能なプロパティを作成するには、静的メソッドを呼び出す必要がある[ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/Xamarin.Forms.BindableProperty+CreateDefaultValueDelegate/)または、短い[ `BindableProperty.CreateReadOnly` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableProperty.CreateReadOnly/p/System.String/System.Type/System.Type/System.Object/Xamarin.Forms.BindingMode/Xamarin.Forms.BindableProperty+ValidateValueDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangedDelegate/Xamarin.Forms.BindableProperty+BindingPropertyChangingDelegate/Xamarin.Forms.BindableProperty+CoerceValueDelegate/)型のプライベート静的な読み取り専用フィールドを定義する[ `BindablePropertyKey`](https://developer.xamarin.com/api/type/Xamarin.Forms.BindablePropertyKey/).

次に、CLR プロパティを定義する`set`としてアクセサー`private`を呼び出す、 [ `SetValue` ](https://developer.xamarin.com/api/member/Xamarin.Forms.BindableObject.SetValue/p/Xamarin.Forms.BindablePropertyKey/System.Object/)を持つオーバー ロード、`BindablePropertyKey`オブジェクト。 これは、プロパティが、クラスの外に設定されていることを防ぎます。

これに示されている、 [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs)で使用されるクラス、 [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount)サンプルです。



## <a name="related-links"></a>関連リンク

- [第 11 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [第 11 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
