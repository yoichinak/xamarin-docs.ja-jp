---
title: '第 12 章の概要: スタイル'
description: 'Xamarin.Forms で Mobile Apps を作成する: 第 12 章の概要: スタイル'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3EAE6BDC-8EFB-464B-A87B-1C35B8387BB3
author: davidbritch
ms.author: dabritch
ms.date: 07/19/2018
ms.openlocfilehash: 408f171a3c7c690b700f7be21a3dcaff503467d9
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/10/2020
ms.locfileid: "65926912"
---
# <a name="summary-of-chapter-12-styles"></a>第 12 章の概要: スタイル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)

Xamarin.Forms では、スタイルを使用して、複数のビューでプロパティ設定のコレクションを共有できます。 これにより、マークアップが減少し、一貫したビジュアル テーマを維持できます。

スタイルは、ほとんどの場合、マークアップで定義および使用されます。 [`Style`](xref:Xamarin.Forms.Style) 型のオブジェクトは、リソース ディクショナリでインスタンス化された後、`StaticResource` または `DynamicResource` マークアップ拡張機能を使用して、ビジュアル要素の [`Style`](xref:Xamarin.Forms.NavigableElement.Style) プロパティに設定されます。

## <a name="the-basic-style"></a>基本スタイル

`Style` を使用するには、その [`TargetType`](xref:Xamarin.Forms.Style.TargetType) を、適用対象のビジュアル オブジェクトの型に設定する必要があります。 リソース ディクショナリで `Style` がインスタンス化されるときは (一般的)、`x:Key` 属性も必要です。

`Style` には [`Setters`](xref:Xamarin.Forms.Style.Setters) 型のコンテンツプロパティがあり、これは [`Setter`](xref:Xamarin.Forms.Setter) オブジェクトのコレクションです。 各 `Setter` では、[`Property`](xref:Xamarin.Forms.Setter.Property) と [`Value`](xref:Xamarin.Forms.Setter.Value) が関連付けられます。

XAML では、`Property` の設定は CLR プロパティの名前 (`Button` の `Text` プロパティなど) ですが、スタイル設定されたプロパティはバインド可能プロパティによってサポートされる必要があります。 また、プロパティは、`TargetType` の設定によって示されるクラスで定義されているか、またはそのクラスによって継承されている必要があります。

`Value` の設定は、プロパティ要素 `<Setter.Value>` を使用して指定できます。 これにより、テキスト文字列では表現できないオブジェクト、`OnPlatform` オブジェクト、`x:Arguments` または `x:FactoryMethod` を使用してインスタンス化されたオブジェクトに対し、`Value` を設定できます。 `Value` プロパティは、`StaticResource` 式を使用して、ディクショナリ内の別の項目に設定することもできます。

[**BasicStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyle) プログラムでは、基本的な構文と、`StaticResource` マークアップ拡張機能で `Style` を参照する方法が示されています。

[![基本スタイルのトリプル スクリーンショット](images/ch12fg01-small.png "基本スタイル")](images/ch12fg01-large.png#lightbox "基本スタイル")

`Style` オブジェクトと、`Value` 設定として `Style` オブジェクト内に作成されたすべてのオブジェクトは、その `Style` を参照しているすべてのビュー間で共有されます。 `Style` には、`View` 派生など、共有できないものを含めることはできません。

イベント ハンドラーを `Style` で設定することはできません。 `GestureRecognizers` プロパティは、バインド可能プロパティでサポートされていないため、`Style` では設定できません。

## <a name="styles-in-code"></a>コード内のスタイル

一般的ではありませんが、`Style` オブジェクトをコード内でインスタンス化して初期化することができます。 これは、[**BasicStyleCode**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/BasicStyleCode) サンプルで示されています。

## <a name="style-inheritance"></a>スタイルの継承

`Style` には [`BasedOn`](xref:Xamarin.Forms.Style.BasedOn) プロパティがあり、別のスタイルを参照する `StaticResource` マークアップ拡張機能に設定できます。 これにより、スタイルを以前のスタイルから継承し、プロパティの設定を追加または置換できます。 これについては、[**StyleInheritance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleInheritance) サンプルを参照してください。

`Style2` が `Style1` に基づいている場合、`Style2` の `TargetType` は、`Style1` と同じであるか、`Style1` から派生している必要があります。 `Style1` が格納されているリソース ディクショナリは、`Style2` と同じリソース ディクショナリであるか、またはビジュアル ツリー内で上位にあるリソース ディクショナリである必要があります。

## <a name="implicit-styles"></a>暗黙的なスタイル

リソース ディクショナリ内の `Style` に `x:Key` 属性設定がない場合は、ディクショナリ キーが自動的に割り当てられ、`Style` オブジェクトが "*暗黙的なスタイル*" になります。 `Style` 設定を持たず、その型が `TargetType` と完全に一致しているビューでは、[**ImplicitStyle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/ImplicitStyle) サンプルで示されているように、そのスタイルが検索されます。

暗黙的なスタイルは、`x:Key` 設定のある `Style` から派生できますが、それ以外の方法ではできません。 暗黙的なスタイルを明示的に参照することはできません。

スタイルと `BasedOn` では、次の 3 種類の階層を実装できます。

- `Application` や `Page` で定義されているスタイルから、ビジュアル ツリー内の下位のレイアウトで定義されているスタイルまで。
- `VisualElement` や `View` などの基底クラスに対して定義されているスタイルから、特定のクラスに対して定義されているスタイルまで。
- 明示的なディクショナリ キーを持つスタイルから、暗黙的なスタイルまで。

これらの階層については、[**StyleHierarchy**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/StyleHierarchy) サンプルで示されています。

## <a name="dynamic-styles"></a>動的なスタイル

リソース ディクショナリ内のスタイルは、`StaticResource`ではなく `DynamicResource` で参照できます。 これにより、スタイルは "*動的スタイル*" になります。 そのスタイルが、リソース ディクショナリ内で同じキーを持つ別のスタイルによって置き換えられた場合、`DynamicResource` でそのスタイルを参照するビューは自動的に変更されます。 また、指定されたキーを持つディクショナリ エントリが存在しない場合、`StaticResource` では例外が発生しますが、`DynamicResource` では発生しません。

[**DynamicStyles**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynamicStyles) サンプルで示されているように、この手法を使用して、スタイルやテーマを動的に変更できます。

ただし、`BasedOn` はバインド可能プロパティによってサポートされていないため、`BasedOn` プロパティを `DynamicResource` マークアップ拡張に設定することはできません。 スタイルを動的に派生させるには、`BasedOn` を設定しないでください。 代わりに、[`BaseResourceKey`](xref:Xamarin.Forms.Style.BaseResourceKey) プロパティを、派生元にしたいスタイルのディクショナリ キーに設定します。 [**DynamicStylesInheritance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DynaStylesInh) サンプルでは、この手法が示されています。

## <a name="device-styles"></a>デバイスのスタイル

入れ子になった [`Device.Styles`](xref:Xamarin.Forms.Device.Styles) クラスでは、`TargetType` が `Label` である 6 つのスタイルに対して 12 の静的読み取り専用フィールドが定義されており、一般的なテキスト使用方法の種類で使用できます。

これらのフィールドの 6 つは `Style` 型であり、コードで `Style` プロパティに直接設定できます。

- [`BodyStyle`](xref:Xamarin.Forms.Device.Styles.BodyStyle)
- [`TitleStyle`](xref:Xamarin.Forms.Device.Styles.TitleStyle)
- [`SubtitleStyle`](xref:Xamarin.Forms.Device.Styles.SubtitleStyle)
- [`CaptionStyle`](xref:Xamarin.Forms.Device.Styles.CaptionStyle)
- [`ListItemTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyle)
- [`ListItemDetailTextStyle`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyle)

他の 6 つのフィールドは `string` 型で、動的スタイルのディクショナリ キーとして使用できます。

- "BodyStyle" に等しい [`BodyStyleKey`](xref:Xamarin.Forms.Device.Styles.BodyStyleKey)
- "TitleStyle" に等しい [`TitleStyleKey`](xref:Xamarin.Forms.Device.Styles.TitleStyleKey)
- "SubtitleStyle" に等しい [`SubtitleStyleKey`](xref:Xamarin.Forms.Device.Styles.SubtitleStyleKey)
- "CaptionStyle" に等しい [`CaptionStyleKey`](xref:Xamarin.Forms.Device.Styles.CaptionStyleKey)
- "ListItemTextStyle" に等しい [`ListItemTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemTextStyleKey)
- "ListItemDetailTextStyle" に等しい [`ListItemDetailTextStyleKey`](xref:Xamarin.Forms.Device.Styles.ListItemDetailTextStyleKey)

これらのスタイルは、[**DeviceStylesList**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12/DeviceStylesList) サンプルで示されています。

## <a name="related-links"></a>関連リンク

- [第 12 章の全文 (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch12-Apr2016.pdf)
- [第 12 章のサンプル](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter12)
- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
