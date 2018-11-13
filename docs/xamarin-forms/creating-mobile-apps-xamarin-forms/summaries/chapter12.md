---
title: 第 12 章の概要です。 スタイル
description: 'Xamarin.Forms によるモバイル アプリの作成: 第 12 章の概要。 スタイル'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: b37a32df9944cd7b312decd9cd9312669b777bc1
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563375"
---
# <a name="summary-of-chapter-12-styles"></a>第 12 章の概要です。 スタイル

Xamarin.Forms では、スタイルは、プロパティの設定のコレクションを共有する複数のビューを許可します。 これは、マークアップが削減され、一貫性のあるビジュアル テーマを維持できるようにします。

スタイルはほぼ常に定義されているし、マークアップで使用します。 型のオブジェクト[ `Style` ](xref:Xamarin.Forms.Style)リソース ディクショナリでインスタンス化し、設定は、 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style)のビジュアル要素を使用して、プロパティ、`StaticResource`または`DynamicResource`マークアップ拡張機能。

## <a name="the-basic-style"></a>基本のスタイル

A`Style`いる必要があります、 [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType)に適用されるビジュアル オブジェクトの種類を設定します。 ときに、`Style`がインスタンス化される (一般的では)、リソース ディクショナリにも必要です、`x:Key`属性。

`Style`タイプの content プロパティを持つ[ `Setters`](xref:Xamarin.Forms.Style.Setters)のコレクションである[ `Setter` ](xref:Xamarin.Forms.Setter)オブジェクト。 各`Setter`関連付けます、 [ `Property` ](xref:Xamarin.Forms.Setter.Property)で、 [ `Value`](xref:Xamarin.Forms.Setter.Value)します。

XAML で、`Property`設定は、CLR プロパティの名前 (など、`Text`プロパティの`Button`) がバインド可能なプロパティでスタイルのプロパティをバックアップする必要があります。 示されたクラスのプロパティを定義する必要がありますも、`TargetType`設定、またはそのクラスに継承します。

指定することができます、`Value`プロパティ要素を使用して設定`<Setter.Value>`します。 設定できます`Value`、テキスト文字列で表現できないオブジェクトを`OnPlatform`オブジェクト、またはオブジェクトのインスタンスを使用して`x:Arguments`または`x:FactoryMethod`します。 `Value`でプロパティを設定することも、`StaticResource`ディクショナリ内の別のアイテムの式。

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle)プログラムの基本的な構文を示していますを参照する方法を示しています、`Style`で、`StaticResource`マークアップ拡張機能。

[![基本的なスタイルの 3 倍になるスクリーン ショット](images/ch12fg01-small.png "の基本的なスタイル")](images/ch12fg01-large.png#lightbox "の基本的なスタイル")

`Style`オブジェクトとで作成した任意のオブジェクト、`Style`オブジェクトとして、`Value`設定は、すべてのビューを参照することで共有`Style`します。 `Style`共有できないなどを含めることはできません、`View`から派生します。

イベント ハンドラーを設定することはできません、`Style`します。 `GestureRecognizers`プロパティを設定することはできません、`Style`バインド可能なプロパティに支えられていないためです。

## <a name="styles-in-code"></a>コード内のスタイル

一般的ではありませんが、インスタンス化および初期化`Style`コード内のオブジェクト。 これを示します、 [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode)サンプル。

## <a name="style-inheritance"></a>スタイル継承

`Style` [ `BasedOn` ](xref:Xamarin.Forms.Style.BasedOn)プロパティを設定できる、`StaticResource`別のスタイルを参照するマークアップ拡張機能。 これにより、以前のスタイル、継承の追加し、プロパティの設定を上書きするスタイル。 [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance)のサンプルで例示します。

場合`Style2`に基づいて`Style1`、`TargetType`の`Style2`と同じである必要があります`Style1`またはから派生した`Style1`します。 リソース ディクショナリ`Style1`が格納されていると同じリソース ディクショナリである必要があります`Style2`またはビジュアル ツリー内の上位のリソース ディクショナリ。

## <a name="implicit-styles"></a>暗黙的なスタイル

場合、`Style`リソース ディクショナリは必要ありません、`x:Key`属性の設定、辞書のキーを自動で割り当てることが、`Style`オブジェクトは、*暗黙的スタイル*。 ないビューを`Style`設定とその型と一致する、`TargetType`としてそのスタイルを正確には検索、 [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle)サンプルを示します。

派生できる暗黙的なスタイル、`Style`で、`x:Key`設定がない、その逆です。 暗黙的なスタイルを明示的に参照することはできません。

次の 3 つの種類のスタイルを使用して階層を実装して`BasedOn`:

- スタイルで定義されている、`Application`と`Page`までレイアウトがビジュアル ツリー内の下位で定義されたスタイル。
- スタイルなどの基本クラスの定義から`VisualElement`と`View`に特定のクラスに対して定義されたスタイル。
- 暗黙的なスタイルに明示的なディクショナリのキーを持つスタイル。

これらの階層が示されています、 [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy)サンプル。

## <a name="dynamic-styles"></a>動的なスタイル

リソース ディクショナリでスタイルを参照できます`DynamicResource`なく`StaticResource`します。 これにより、スタイル、*動的スタイル*します。 そのスタイルで置き換えた場合、リソース ディクショナリに別のスタイルで、同じキーを使用するスタイルを参照しているビュー`DynamicResource`を自動的に変更します。 また、指定したキーのディクショナリ エントリがないと、`StaticResource`例外を発生させるなく`DynamicResource`します。

この手法を使用するにはスタイルまたはとしてテーマを動的に変更する、 [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles)サンプルを示します。

ただし、設定することはできません、`BasedOn`プロパティを`DynamicResource`拡張機能の構成のため、`BasedOn`バインド可能なプロパティによってバックアップされていません。 スタイルを動的に派生する設定しないで`BasedOn`します。 代わりに、設定、 [ `BaseResourceKey` ](xref:Xamarin.Forms.Style.BaseResourceKey)プロパティから派生するスタイルのディクショナリのキーにします。 [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh)サンプルは、この手法を示します。

## <a name="device-styles"></a>デバイスのスタイル

[ `Device.Styles` ](xref:Xamarin.Forms.Device.Styles)の 6 つのスタイルと、12 静的読み取り専用のフィールドを定義する入れ子になったクラスを`TargetType`の`Label`一般的なテキストの使用法の種類に使用できます。

型は、これらのフィールドの 6 つの`Style`に直接設定できること、`Style`コード内のプロパティ。

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

その他の 6 つのフィールドには型の`string`動的なスタイルのディクショナリ キーとして使用できます。

- [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey) "BodyStyle"に等しい
- [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey) "TitleStyle"に等しい
- [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey) "SubtitleStyle"に等しい
- [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey) "CaptionStyle"に等しい
- [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey) "ListItemTextStyle"に等しい
- [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey) "ListItemDetailTextStyle"に等しい

これらのスタイルには、 [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList)サンプル。

## <a name="related-links"></a>関連リンク

- [第 12 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [第 12 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
