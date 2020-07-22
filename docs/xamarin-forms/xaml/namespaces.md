---
title: での XAML 名前空間Xamarin.Forms
description: XAML では、名前空間宣言に xmlns XML 属性を使用します。 この記事では、XAML 名前空間の構文について説明し、型にアクセスするために XAML 名前空間を宣言する方法を示します。
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/21/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7f35342134767ccdadfab086bfa14f6b610b325d
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84130378"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>での XAML 名前空間Xamarin.Forms

_XAML では、名前空間宣言に xmlns XML 属性を使用します。この記事では、XAML 名前空間の構文について説明し、型にアクセスするために XAML 名前空間を宣言する方法を示します。_

## <a name="overview"></a>概要

Xaml ファイルのルート要素内に常に存在する2つの XAML 名前空間宣言があります。 最初のは、次の XAML コード例に示すように、既定の名前空間を定義します。

```xaml
xmlns="http://xamarin.com/schemas/2014/forms"
```

既定の名前空間は、XAML ファイル内で定義されたプレフィックスなしの要素が、などのクラスを参照することを指定し Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) ます。

2番目の名前空間宣言では、 `x` 次の XAML コード例に示すように、プレフィックスを使用します。

```xaml
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML では、プレフィックスを使用して既定以外の名前空間を宣言し、名前空間内の型を参照するときにプレフィックスを使用します。 名前空間宣言は、xaml で定義された要素 `x` `x` と属性 (具体的には 2009 xaml 仕様) で、のプレフィックスを持つ xaml 内で定義された要素を使用することを指定します。

次の表は、 `x` でサポートされる名前空間属性の概要を示してい Xamarin.Forms ます。

|構成体|説明|
|--- |--- |
|`x:Arguments`|既定以外のコンストラクターまたはファクトリメソッドオブジェクトの宣言のコンストラクター引数を指定します。|
|`x:Class`|XAML で定義されているクラスの名前空間とクラス名を指定します。 クラス名は、分離コードファイルのクラス名と一致している必要があります。 このコンストラクトは、XAML ファイルのルート要素にのみ表示されることに注意してください。|
|`x:DataType`|XAML 要素とその子のバインド先となるオブジェクトの型を指定します。|
|`x:FactoryMethod`|オブジェクトの初期化に使用できるファクトリメソッドを指定します。|
|`x:FieldModifier`|名前付き XAML 要素に対して生成されるフィールドのアクセスレベルを指定します。|
|`x:Key`|内の各リソースに対して一意のユーザー定義キーを指定し `ResourceDictionary` ます。 キーの値は XAML リソースを取得するために使用され、通常はマークアップ拡張機能の引数として使用され `StaticResource` ます。|
|`x:Name`|XAML 要素のランタイムオブジェクト名を指定します。 の設定 `x:Name` は、コードでの変数の宣言に似ています。|
|`x:TypeArguments`|ジェネリック型のコンストラクターへのジェネリック型引数を指定します。|

属性の詳細については `x:DataType` 、「[コンパイル済みバインディング](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)」を参照してください。 属性の詳細については `x:FieldModifier` 、「[フィールド修飾子](~/xamarin-forms/xaml/field-modifiers.md)」を参照してください。 属性と属性の詳細につい `x:Arguments` `x:FactoryMethod` ては、「 [XAML で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)」を参照してください。 属性の詳細については `x:TypeArguments` 、「」を参照[して Xamarin.Forms ](generics.md)ください。

> [!NOTE]
> 上に示した名前空間属性に加えて、には、 Xamarin.Forms 名前空間プレフィックスを通じて使用できるマークアップ拡張機能も含まれてい `x` ます。 詳細については、「 [XAML マークアップ拡張機能](~/xamarin-forms/xaml/markup-extensions/consuming.md)の使用」を参照してください。

XAML では、名前空間宣言は親要素から子要素に継承されます。 そのため、XAML ファイルのルート要素で名前空間を定義すると、そのファイル内のすべての要素が名前空間宣言を継承します。

## <a name="declaring-namespaces-for-types"></a>型の名前空間の宣言

Xaml で型を参照するには、xaml 名前空間をプレフィックスで宣言し、共通言語ランタイム (CLR) 名前空間の名前を指定する名前空間宣言と、必要に応じてアセンブリ名を指定します。 これを実現するには、名前空間の宣言内に次のキーワードの値を定義します。

- **clr-namespace:** または**USING:** – XAML 要素として公開する型を含むアセンブリ内で宣言された clr 名前空間。 このキーワードは必須です。
- **assembly =** –参照されている CLR 名前空間を含むアセンブリ。 この値は、ファイル拡張子のないアセンブリの名前です。 アセンブリへのパスは、アセンブリを参照する XAML ファイルを含むプロジェクトファイル内の参照として確立される必要があります。 **Clr 名前空間**の値が、型を参照しているアプリケーションコードと同じアセンブリ内にある場合は、このキーワードを省略できます。

`clr-namespace`またはトークンを値から区切る文字はコロンであるのに `using` 対し、トークンを値から区切る文字は等号であることに注意して `assembly` ください。 2つのトークンの間で使用する文字はセミコロンです。

次のコード例は、XAML 名前空間の宣言を示しています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

または、次のように記述できます。

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local`プレフィックスは、名前空間内の型がアプリケーションに対してローカルであることを示すために使用される規則です。 また、型が別のアセンブリにある場合は、次の XAML コード例に示すように、名前空間の宣言でアセンブリ名を定義する必要もあります。

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

次の XAML コード例に示すように、インポートされた名前空間から型のインスタンスを宣言するときに、名前空間プレフィックスが指定されます。

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

カスタム名前空間スキーマの定義については、「 [XAML カスタム名前空間](custom-namespace-schemas.md)スキーマ」を参照してください。

## <a name="summary"></a>まとめ

この記事では、XAML 名前空間構文を紹介し、型にアクセスするために XAML 名前空間を宣言する方法を示しました。 XAML では、 `xmlns` 名前空間宣言に XML 属性を使用します。 xaml では、xaml 名前空間をプレフィックスで宣言することによって型を参照できます。

## <a name="related-links"></a>関連リンク

- [XAML での引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
- [での XAML のジェネリックXamarin.Forms](generics.md)
