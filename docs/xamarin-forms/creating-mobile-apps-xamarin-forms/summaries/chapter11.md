---
title: 第 11 章の概要です。 バインド可能なインフラストラクチャ
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 11 章の概要。 バインド可能なインフラストラクチャ'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 34671C48-0ED4-4B76-A33D-D6505390DC5B
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: f9e3326c0f55469cfa84a019a674679d82dfc007
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53054237"
---
# <a name="summary-of-chapter-11-the-bindable-infrastructure"></a>第 11 章の概要です。 バインド可能なインフラストラクチャ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)

すべての c# のプログラマが c# に精通して*プロパティ*します。 プロパティが含まれる、*設定*アクセサーおよび*取得*アクセサー。 多くの場合と呼ばれる*CLR プロパティ*共通言語ランタイム。

Xamarin.Forms という拡張プロパティ定義を定義する、*バインド可能なプロパティ*によってカプセル化、 [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)クラスをサポートし、 [ `BindableObject` ](xref:Xamarin.Forms.BindableObject)クラス。 これらのクラスは関連がかなり異なります。`BindableProperty`プロパティ自体を定義するために使用`BindableObject`などは`object`バインド可能なプロパティを定義するクラスの基本クラスです。

## <a name="the-xamarinforms-class-hierarchy"></a>Xamarin.Forms のクラス階層

[ **ClassHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/ClassHierarchy)サンプルでは、リフレクションを使用して、Xamarin.Forms のクラス階層を表示してが果たす重要な役割を実演`BindableObject`この階層にします。 `BindableObject` 派生した`Object`に親クラスでは[ `Element` ](xref:Xamarin.Forms.Element)元[ `VisualElement` ](xref:Xamarin.Forms.VisualElement)派生します。 これは、親クラス[ `Page` ](xref:Xamarin.Forms.Page)と[ `View` ](xref:Xamarin.Forms.View)、親クラスである[ `Layout` ](xref:Xamarin.Forms.Layout):

[![クラス階層の共有の 3 倍になるスクリーン ショット](images/ch11fg01-small.png "クラス階層の共有")](images/ch11fg01-large.png#lightbox "クラス階層の共有")

## <a name="a-peek-into-bindableobject-and-bindableproperty"></a>BindableObject に BindableProperty のピーク

派生するクラスで`BindableObject`CLR プロパティの多くは、"サポート"されるによって連結可能プロパティと言われています。 たとえば、 [ `Text` ](xref:Xamarin.Forms.Label.Text)のプロパティ、`Label`クラスは、CLR プロパティが、`Label`クラスでは、パブリック静的読み取り専用という名前のフィールドも定義します[ `TextProperty` ](xref:Xamarin.Forms.Label.TextProperty)の型`BindableProperty`します。

アプリケーションの設定または取得できます、`Text`プロパティの`Label`通常、アプリケーションで設定できますか、`Text`呼び出すことによって、 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindableProperty,System.Object))によって定義されたメソッド`BindableObject`で、 `Label.TextProperty`引数。 同様に、アプリケーションがの値を取得できます、`Text`プロパティを呼び出すことによって、 [ `GetValue` ](xref:Xamarin.Forms.BindableObject.GetValue(Xamarin.Forms.BindableProperty))メソッドを使用してもう一度、`Label.TextProperty`引数。 これを示します、 [**設定**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PropertySettings)サンプル。

実際、 `Text` CLR プロパティを実装する完全、`SetValue`と`GetValue`によって定義されたメソッド`BindableObject`と組み合わせて、`Label.TextProperty`静的プロパティ。

`BindableObject` `BindableProperty`サポートを提供します。

- プロパティの既定値を提供
- 現在の値を格納します。
- プロパティの値を検証するためのメカニズムを提供します。
- 1 つのクラスに関連するプロパティの間で一貫性を維持します。
- プロパティの変更に応答します。
- プロパティが変更しようかが変更されたときに通知をトリガーします。
- データ バインドのサポート
- スタイルをサポートしています。
- 動的リソースのサポート

たびに、バインド可能なプロパティの変更によってサポートされるプロパティ`BindableObject`発生、 [ `PropertyChanged` ](xref:Xamarin.Forms.BindableObject.PropertyChanged)変更されたプロパティを識別するイベントです。 プロパティが同じ値に設定されている場合、このイベントは発生しません。

一部のプロパティは、バインド可能なプロパティは、および Xamarin.Forms の一部のクラスによってバックアップされません&mdash;など`Span`&mdash;から派生していない`BindableObject`します。 派生したクラスのみ`BindableObject`ために、バインド可能なプロパティをサポートできる`BindableObject`定義、`SetValue`と`GetValue`メソッド。

`Span`から派生していない`BindableObject`、そのプロパティのいずれも&mdash;など`Text`&mdash;バインド可能なプロパティによってサポートされます。 これは、ため、`DynamicResource`の設定、`Text`のプロパティ`Span`で例外を発生させます、 [ **DynamicVsStatic** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter10/DynamicVsStatic)前の章のサンプル。 [ **DynamicVsStaticCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/DynamicVsStaticCode)サンプル コードを使用して動的なリソースを設定する方法を示して、 [ `SetDynamicResource` ](xref:Xamarin.Forms.Element.SetDynamicResource(Xamarin.Forms.BindableProperty,System.String))メソッドによって定義された`Element`します。 最初の引数は型のオブジェクト`BindableProperty`します。

同様に、 [ `SetBinding` ](xref:Xamarin.Forms.BindableObject.SetBinding(Xamarin.Forms.BindableProperty,Xamarin.Forms.BindingBase))メソッドによって定義された`BindableObject`型の最初の引数を持つ`BindableProperty`します。

## <a name="defining-bindable-properties"></a>バインド可能なプロパティを定義します。

静的なを使用して、独自のバインド可能なプロパティを定義する[ `BindableProperty.Create` ](xref:Xamarin.Forms.BindableProperty.Create(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))型の静的な読み取り専用フィールドを作成するメソッドを`BindableProperty`します。

これは、方法については、 [ `AltLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AltLabel.cs)クラス、 [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)ライブラリ。 クラスの派生元`Label`ポイントのフォント サイズを指定できます。 これは、方法については、 [ **PointSizedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/PointSizedText)サンプル。

4 つの引数、`BindableProperty.Create`メソッドが必要です。

- `propertyName`: (CLR プロパティの名前と同じ) プロパティのテキスト名
- `returnType`: CLR プロパティの型
- `declaringType`: プロパティを宣言するクラスの型
- `defaultValue`: プロパティの既定値

`defaultValue`の種類は`object`コンパイラは、既定値の型を決定できる必要があります。 たとえば場合、`returnType`は`double`、`defaultValue`だけではなく 0.0 の 0 のように設定する必要がありますまたは型の不一致実行時に例外がトリガーされます。

バインド可能なプロパティを含めるは非常に一般的です。

- `propertyChanged`: プロパティ値を変更するときに、静的メソッドが呼び出されます。 最初の引数は、プロパティが変更されたクラスのインスタンスです。

他の引数`BindableProperty.Create`は一般的でないです。

- `defaultBindingMode`: データ バインディングに関連して使用 (で説明したよう[ **16 章です。データ バインディング**](chapter16.md))
- `validateValue`: 有効な値をチェックするコールバック
- `propertyChanging`: プロパティが変更を通知するコールバック
- `coerceValue`: 設定の値を別の値を強制的にコールバック
- `defaultValueCreate`: 既定値 (たとえば、コレクション) クラスのインスタンス間で共有することはできませんを作成するコールバック

### <a name="the-read-only-bindable-property"></a>読み取り専用のバインド可能なプロパティ

バインド可能なプロパティは読み取り専用にできます。 読み取り専用のバインド可能なプロパティを作成するには、静的メソッドを呼び出す必要があります[ `BindableProperty.CreateReadOnly` ](xref:Xamarin.Forms.BindableProperty.CreateReadOnly(System.String,System.Type,System.Type,System.Object,Xamarin.Forms.BindingMode,Xamarin.Forms.BindableProperty.ValidateValueDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangedDelegate,Xamarin.Forms.BindableProperty.BindingPropertyChangingDelegate,Xamarin.Forms.BindableProperty.CoerceValueDelegate,Xamarin.Forms.BindableProperty.CreateDefaultValueDelegate))型のプライベート静的読み取り専用フィールドを定義する[ `BindablePropertyKey`](xref:Xamarin.Forms.BindablePropertyKey)します。

CLR プロパティを定義し、`set`アクセサーとして`private`を呼び出す、 [ `SetValue` ](xref:Xamarin.Forms.BindableObject.SetValue(Xamarin.Forms.BindablePropertyKey,System.Object))オーバー ロード、`BindablePropertyKey`オブジェクト。 これは、プロパティがクラス外で設定されていることを防ぎます。

これは、方法については、 [ `CountedLabel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/CountedLabel.cs)で使用されるクラス、 [ **BaskervillesCount** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11/BaskervillesCount)サンプル。

## <a name="related-links"></a>関連リンク

- [第 11 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch11-Apr2016.pdf)
- [第 11 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter11)
- [連結可能プロパティ](~/xamarin-forms/xaml/bindable-properties.md)
