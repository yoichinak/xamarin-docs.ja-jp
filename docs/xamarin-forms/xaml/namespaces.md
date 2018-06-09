---
title: Xamarin.Forms で XAML 名前空間
description: XAML では、名前空間宣言 xmlns XML 属性を使用します。 この記事では、XAML 名前空間の構文を紹介し、型にアクセスする XAML 名前空間を宣言する方法を示します。
ms.prod: xamarin
ms.assetid: C03B5553-B199-4A19-9F0F-E5BCE1DB268F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/10/2017
ms.openlocfilehash: faa4998869b918caaf5bc4252dc81a5745199c93
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245834"
---
# <a name="xaml-namespaces-in-xamarinforms"></a>Xamarin.Forms で XAML 名前空間

_XAML では、名前空間宣言 xmlns XML 属性を使用します。この記事では、XAML 名前空間の構文を紹介し、型にアクセスする XAML 名前空間を宣言する方法を示します。_

## <a name="overview"></a>概要

常に、XAML ファイルのルート要素内にある 2 つの XAML 名前空間宣言があります。 次の XAML コードの例で示すように、最初は既定の名前空間を定義します。

```csharp
xmlns="http://xamarin.com/schemas/2014/forms"
```

既定の名前空間プレフィックスのない XAML ファイル内で定義されている要素を参照している Xamarin.Forms クラスなどを指定する[ `ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)です。

2 番目の名前空間宣言を使用して、`x`プレフィックス、XAML コードの例を次に示すようにします。

```csharp
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
```

XAML では、プレフィックスを使用して、名前空間内の型を参照するときに使用されているプレフィックスで、既定以外の名前空間を宣言します。 `x`名前空間の宣言を指定のプレフィックスを持つ XAML 内の要素が定義されている`x`XAML (具体的には、2009 XAML 仕様) に内在する要素と属性を使用します。

次の表にアウトライン、 `x` Xamarin.Forms によってサポートされる名前空間属性。

|構成体|説明|
|--- |--- |
|`x:Arguments`|既定ではない、コンス トラクターまたはファクトリ メソッド オブジェクト宣言のコンス トラクター引数を指定します。|
|`x:Class`|XAML で定義されたクラスの名前空間とクラス名を指定します。 クラス名は、分離コード ファイルのクラス名と一致する必要があります。 このコンストラクトは、XAML ファイルのルート要素にのみ表示できますに注意してください。|
|`x:FactoryMethod`|オブジェクトを初期化するために使用できるファクトリ メソッドを指定します。|
|`x:Key`|内の各リソースの一意のユーザー定義キーを指定します、`ResourceDictionary`です。 キーの値が XAML リソースの取得に使用され、は、通常の引数として使用、`StaticResource`マークアップ拡張機能です。|
|`x:Name`|XAML 要素のランタイム オブジェクトの名前を指定します。 設定`x:Name`コード内の変数を宣言に似ています。|
|`x:TypeArguments`|ジェネリック型のコンス トラクターは、ジェネリック型引数を指定します。|

詳細については、 `x:Arguments`、 `x:FactoryMethod`、および`x:TypeArguments`属性を参照してください[XAML で引数を渡す](~/xamarin-forms/xaml/passing-arguments.md)です。

XAML では、名前空間の宣言は子要素を親要素から継承します。 そのため、名前空間を定義する XAML ファイルのルート要素で、そのファイル内のすべての要素は、名前空間宣言を継承します。

## <a name="declaring-namespaces-for-types"></a>型の名前空間を宣言すること

型は、共通言語ランタイム (CLR) の名前空間名、および必要に応じてアセンブリ名を指定する名前空間の宣言で、プレフィックスを持つ XAML 名前空間を宣言することによって、XAML で参照できます。 名前空間宣言内で次のキーワードの値を定義することでこれを実現します。

- **clr 名前空間:** または**を使用して:** – CLR 名前空間は、XAML 要素として公開する型を含むアセンブリ内で宣言します。 このキーワードが必要です。
- **アセンブリ =** – 参照先の CLR 名前空間を含むアセンブリです。 この値は、ファイル拡張子を除いた、アセンブリの名前です。 アセンブリを参照する XAML ファイルを含むプロジェクト ファイル内の参照として、アセンブリへのパスを確立する必要があります。 場合、このキーワードを省略できます、 **clr 名前空間**値が型を参照するアプリケーション コードと同じアセンブリ内にします。

文字を分離することに注意してください、`clr-namespace`または`using`値からトークンがありますが、コロン、文字の分離、`assembly`値からトークンが等号 (=)。 2 つのトークンの間で使用する文字は、セミコロンです。

次のコード例は、XAML 名前空間の宣言を示しています。

```xaml
<ContentPage ... xmlns:local="clr-namespace:HelloWorld" ...>
  ...
</ContentPage>
```

代わりに、このとしてを記述できます。

```xaml
<ContentPage ... xmlns:local="using:HelloWorld" ...>
  ...
</ContentPage>
```

`local`プレフィックスは名前空間内の型がアプリケーションに対してローカルであることを示すために使用される規則です。 また場合、型は、別のアセンブリでは、アセンブリ名必要がありますも定義する名前空間の宣言での XAML コードの例を次に示すように。

```xaml
<ContentPage ... xmlns:behaviors="clr-namespace:Behaviors;assembly=BehaviorsLibrary" ...>
  ...
</ContentPage>
```

次の XAML コードの例に示されていると、インポートされた名前空間の型のインスタンスを宣言するときに、名前空間プレフィックスを指定しています。

```xaml
<ListView ...>
  <ListView.Behaviors>
    <behaviors:EventToCommandBehavior EventName="ItemSelected" ... />
  </ListView.Behaviors>
</ListView>
```

## <a name="summary"></a>まとめ

この記事では、XAML 名前空間の構文を導入し、型にアクセスする XAML 名前空間を宣言する方法を示しました。 XAML を使用して、`xmlns`プレフィックスを持つ XAML 名前空間を宣言することで、名前空間の宣言と型の XML 属性を XAML で参照されることができます。


## <a name="related-links"></a>関連リンク

- [XAML で引数の受け渡し](~/xamarin-forms/xaml/passing-arguments.md)
