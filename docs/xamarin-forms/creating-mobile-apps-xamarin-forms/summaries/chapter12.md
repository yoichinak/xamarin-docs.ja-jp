---
title: 12 章の概要です。 スタイル
description: 'Xamarin.Forms を使用したモバイル アプリの作成: 12 章の概要です。 スタイル'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: d69c6c18a28c89742b186474656ee666b1e6a0ee
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241677"
---
# <a name="summary-of-chapter-12-styles"></a>12 章の概要です。 スタイル

Xamarin.Forms では、スタイルは、プロパティの設定のコレクションを共有する複数のビューを許可します。 これは、マークアップを削減でき、一貫性のあるビジュアル テーマを維持できます。

スタイルはほとんど常に定義され、マークアップで使用します。 型のオブジェクト[ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)リソース ディクショナリでインスタンス化し、設定は、 [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)のビジュアル要素を使用して、プロパティ、`StaticResource`または`DyanamicResource`マークアップ拡張機能です。

## <a name="the-basic-style"></a>基本的なスタイル

A`Style`いる必要があります、 [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)ビジュアルに適用されるオブジェクトの型に設定できます。 ときに、`Style`がインスタンス化されたリソース ディクショナリ (で一般的な) も必要になる、`x:Key`属性。

`Style`型のコンテンツ プロパティを持つ[ `Setters`](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.Setters/)の集合である[ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)オブジェクト。 各`Setter`関連付けます、 [ `Property` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Property/)で、 [ `Value`](https://developer.xamarin.com/api/property/Xamarin.Forms.Setter.Value/)です。

XAML で、`Property`設定は、CLR プロパティの名前 (など、`Text`のプロパティ`Button`) が、バインド可能なプロパティで、スタイル設定されたプロパティをバックアップする必要があります。 また、プロパティは、によって示されるクラスで定義される必要があります、`TargetType`設定、またはそのクラスに継承します。

指定することができます、`Value`プロパティ要素を使用して設定`<Setter.Value>`です。 設定できます`Value`を表すことのできないテキスト文字列またはオブジェクトに、`OnPlatform`オブジェクト、またはオブジェクトには、使用してインスタンス化`x:Arguments`または`x:FactoryMethod`です。 `Value`でプロパティを設定することも、`StaticResource`ディクショナリ内の別のアイテムの式。

[ **BasicStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle)プログラムが基本的な構文を示していてを参照する方法を示しています、`Style`で、`StaticResource`マークアップ拡張機能。

[![基本的なスタイルのトリプル スクリーン ショット](images/ch12fg01-small.png "の基本的なスタイル")](images/ch12fg01-large.png#lightbox "の基本的なスタイル")

`Style`オブジェクトとで作成した任意のオブジェクト、`Style`オブジェクトとして、`Value`設定を参照しているすべてのビューの間で共有される`Style`です。 `Style`すべての共有できないなどを含めることはできません、`View`から派生します。

イベント ハンドラーを設定することはできません、`Style`です。 `GestureRecognizers`プロパティを設定することはできません、`Style`バインド可能なプロパティで補助されていないためです。

## <a name="styles-in-code"></a>コード内のスタイル

一般的ではありませんがのインスタンスを作成して初期化`Style`コード内のオブジェクト。 示されるこれは、 [ **BasicStyleCode** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode)サンプルです。

## <a name="style-inheritance"></a>スタイルの継承

`Style` [ `BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/)に設定できるプロパティ、`StaticResource`別のスタイルを参照するマークアップ拡張機能です。 これにより、スタイルを以前のスタイルを継承し、追加またはプロパティの設定を上書きできます。 [ **StyleInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance)サンプルを示します。

場合`Style2`に基づく`Style1`、`TargetType`の`Style2`と同じである必要があります`Style1`から派生または`Style1`です。 リソース ディクショナリ`Style1`が格納されていると同じリソース ディクショナリをする必要があります`Style2`またはビジュアル ツリー内の上位のリソース ディクショナリ。

## <a name="implicit-styles"></a>暗黙的なスタイル

場合、`Style`リソース ディクショナリは必要ありません、`x:Key`属性の設定、辞書のキーを自動的に割り当てられると、`Style`オブジェクトになります、*暗黙的なスタイル*です。 ないビュー、`Style`設定と同じ型の`TargetType`としてそのスタイルを正確には検索、 [ **ImplicitStyle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle)サンプルを示します。

暗黙的なスタイルがから派生させることができます、`Style`で、`x:Key`設定は、逆はできなくします。 暗黙的なスタイルを明示的に参照することはできません。

3 種類のスタイルを使用して階層を実装することができ、 `BasedOn`:

- 定義されているスタイルから、`Application`と`Page`レイアウトがビジュアル ツリー内の下位に定義されているスタイルまでです。
- などの基本クラスに対して定義されたスタイルから`VisualElement`と`View`特定のクラスに対して定義されているスタイルにします。
- 明示的なディクショナリ キーは、暗黙的なスタイルを持つスタイル。

これらの階層が示されている、 [ **StyleHierarchy** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy)サンプルです。

## <a name="dynamic-styles"></a>動的なスタイル

リソース ディクショナリにスタイルを参照できます`DynamicResource`なく`StaticResource`です。 これにより、スタイル、*動的スタイル*です。 そのスタイルで置き換えた場合、リソース ディクショナリで別のスタイルで、同じキーとそのスタイルを参照しているビュー`DynamicResource`を自動的に変更します。 また、指定したキーを持つディクショナリ エントリが存在しないと、`StaticResource`例外を発生させるのには対象外`DynamicResource`です。

この手法を使用してスタイルまたはとしてテーマを動的に変更することができます、 [ **DynamicStyles** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles)サンプルを示します。

ただし、設定することはできません、`BasedOn`プロパティを`DynamicResource`拡張機能の構成のため`BasedOn`バインド可能なプロパティでバックアップされていません。 スタイルを動的に派生する設定しないで`BasedOn`です。 代わりに、設定、 [ `BaseResourceKey` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BaseResourceKey/)プロパティから派生するスタイルのディクショナリのキーをします。 [ **DynamicStylesInheritance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh)サンプルは、この手法を示します。

## <a name="device-styles"></a>デバイスのスタイル

[ `Device.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)入れ子になったクラスは、12 静的な読み取り専用フィールドを持つ 6 個のスタイルを定義、`TargetType`の`Label`一般的なテキストの使用法の種類を使用することができます。

型は、これらのフィールドの 6 つ`Style`に直接設定できること、`Style`コード内のプロパティ。

- [`BodyStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyle/)
- [`TitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyle/)
- [`SubtitleStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyle/)
- [`CaptionStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyle/)
- [`ListItemTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyle/)
- [`ListItemDetailTextStyle`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyle/)

型の他の 6 つのフィールドは、`string`と動的なスタイルのディクショナリのキーとして使用できます。

- [`BodyStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.BodyStyleKey/) "BodyStyle"に等しい
- [`TitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.TitleStyleKey/) "TitleStyle"に等しい
- [`SubtitleStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.SubtitleStyleKey/) "SubtitleStyle"に等しい
- [`CaptionStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.CaptionStyleKey/) "CaptionStyle"に等しい
- [`ListItemTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemTextStyleKey/) "ListItemTextStyle"に等しい
- [`ListItemDetailTextStyleKey`](https://developer.xamarin.com/api/field/Xamarin.Forms.Device+Styles.ListItemDetailTextStyleKey/) "ListItemDetailTextStyle"に等しい

これらのスタイルは説明、 [ **DeviceStylesList** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList)サンプルです。



## <a name="related-links"></a>関連リンク

- [12 章フル テキスト (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [章の 12 のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
