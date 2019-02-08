---
title: Xamarin.Forms の XAML 名前空間
description: XAML では、名前空間宣言 xmlns XML 属性を使用します。 この記事では、XAML 名前空間の構文を紹介し、型にアクセスする XAML 名前空間を宣言する方法を示します。
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
ms.openlocfilehash: be6154631b8b51ec61feb4c713d925ff30505b7d
ms.sourcegitcommit: 93c9fe61eb2cdfa530960b4253eb85161894c882
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/07/2019
ms.locfileid: "55831756"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>Xamarin.Forms の XAML 名前空間

_XAML では、名前空間宣言 xmlns XML 属性を使用します。この記事では、XAML 名前空間の構文を紹介し、型にアクセスする XAML 名前空間を宣言する方法を示します。_

## <a name="overview"></a>概要

常に、XAML ファイルのルート要素内にある 2 つの XAML 名前空間宣言があります。 最初は、次の XAML コード例に示すように、既定の名前空間を定義します。

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

既定の名前空間のプレフィックスのない XAML ファイル内で定義されている要素を参照している Xamarin.Forms のクラスなどを指定します[ `ContentPage`](xref:Xamarin.Forms.ContentPage)します。

2 番目の名前空間宣言を使用して、`x`プレフィックス、次の XAML コード例に示すようにします。

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML では、プレフィックスを使用して、名前空間内の型を参照するときに使用されているプレフィックスで、既定以外の名前空間を宣言します。 `x`名前空間の宣言では、プレフィックスを持つ XAML 内で要素が定義されていることを指定します`x`要素とは、XAML (具体的には、2009 XAML 仕様) に固有の属性に使用します。

次の表にアウトライン、 `x` Xamarin.Forms でサポートされている名前空間の属性。

|構成体|説明|
|--- |--- |
|`x:Arguments`|コンス トラクター引数、既定ではないコンス トラクターまたはファクトリ メソッドのオブジェクトの宣言を指定します。|
|`x:Class`|XAML で定義されているクラスの名前空間とクラス名を指定します。 クラス名は、分離コード ファイルのクラス名と一致する必要があります。 このコンス トラクターが XAML ファイルのルート要素でのみ表示できることに注意してください。|
|`x:DataType`|XAML の要素とその子にバインドするオブジェクトの種類を指定します。|
|`x:FactoryMethod`|オブジェクトを初期化するために使用できるファクトリ メソッドを指定します。|
|`x:FieldModifier`|生成されたフィールドの名前付き XAML 要素のアクセス レベルを指定します。|
|`x:Key`|内の各リソースにある一意のユーザー定義のキーを指定します、`ResourceDictionary`します。 キーの値を使用して、XAML リソースを取得し、通常は引数として使用、`StaticResource`マークアップ拡張機能。|
|`x:Name`|XAML 要素のランタイム オブジェクトの名前を指定します。 設定`x:Name`コード内の変数の宣言に似ています。|
|`x:TypeArguments`|ジェネリック型のコンス トラクターにジェネリック型引数を指定します。|

詳細については、`x:DataType`属性は、「[コンパイル バインド](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)します。 詳細については、`x:FieldModifier`属性は、「[フィールド修飾子](~/xamarin-forms/xaml/field-modifiers.md)します。 詳細については、 `x:Arguments`、 `x:FactoryMethod`、および`x:TypeArguments`属性を参照してください[XAML で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)します。

XAML では、名前空間の宣言は、子要素を親要素から継承します。 そのため、XAML ファイルのルート要素で名前空間を定義するときにそのファイル内のすべての要素は、名前空間宣言を継承します。

## <a name="declaring-namespaces-for-types"></a>型の名前空間を宣言します。

種類は、共通言語ランタイム (CLR) 名前空間の名前と必要に応じて、アセンブリ名を指定する名前空間宣言で、プレフィックスを持つ XAML 名前空間を宣言することで、XAML で参照できます。 これは、名前空間宣言内で次のキーワードの値を定義することで実現されます。

- **clr 名前空間:** または**を使用して:** – CLR 名前空間は、XAML 要素として公開する型を含むアセンブリ内で宣言します。 このキーワードが必要です。
- **アセンブリ =** -参照されている CLR 名前空間を含むアセンブリ。 この値は、ファイル拡張子を除いた、アセンブリの名前です。 アセンブリを参照する XAML ファイルを含むプロジェクト ファイル内の参照として、アセンブリへのパスを確立する必要があります。 場合、このキーワードを省略できます、 **clr 名前空間**値が型を参照するアプリケーション コードと同じアセンブリ内にします。

区切る文字に注意してください、`clr-namespace`または`using`値からトークンがありますが、コロン文字の分離、`assembly`等号 (=) は、その値からトークン。 2 つのトークンの間で使用する文字は、セミコロンです。

次のコード例では、XAML 名前空間の宣言を示します。

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

また、これとしてを記述できます。

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local`プレフィックスは、名前空間内の型がアプリケーションに対してローカルを示すために使用される規則です。 または、型が別のアセンブリである場合は、アセンブリ名が定義することも名前空間の宣言での XAML コードの例を次に示す。

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

次の XAML コード例に示すとしてインポートされた名前空間からの型のインスタンスを宣言するときに、名前空間プレフィックスを指定しています。

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

カスタムの名前空間のスキーマを定義する方法の詳細については、次を参照してください。 [XAML カスタム Namespace スキーマ](custom-namespace-schemas.md)します。

## <a name="summary"></a>まとめ

この記事では、XAML 名前空間の構文を導入し、型にアクセスする XAML 名前空間を宣言する方法を示しました。 XAML を使用して、`xmlns`名前空間の宣言と型の XML 属性は、プレフィックスを持つ XAML 名前空間を宣言することにより XAML で参照できます。

## <a name="related-links"></a>関連リンク

- [XAML で引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
