---
title: Xamarin.Forms のバインドのフォールバック
description: この記事では、バインドが失敗する場合に使用されるフォールバック値を定義することでより堅牢なバインドを作成する方法について説明します。
ms.prod: xamarin
ms.assetid: 637ACD9D-3E5D-4014-86DE-A77D1FEF238A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/16/2018
ms.openlocfilehash: 2a4b29df9148ce695f8f3ca5377e5848af1b775a
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171600"
---
# <a name="xamarinforms-binding-fallbacks"></a>Xamarin.Forms のバインドのフォールバック

場合がありますデータ バインドまたは失敗する場合、バインディング ソースが、解決できないため、バインディングの成功が返されますので、`null`値。 値コンバーター、またはその他の追加のコードでは、これらのシナリオを処理することができます、データ バインドできるより堅牢なバインド プロセスが失敗した場合に使用するフォールバック値を定義することで。 これは、定義することで実現できます、 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue)と[ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue)バインド式でのプロパティ。 これらのプロパティが存在しているので、 [ `BindingBase` ](xref:Xamarin.Forms.BindingBase)クラス、コンパイル済みのバインドのバインドと使用することができます、`Binding`マークアップ拡張機能。

> [!NOTE]
> 使用、 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue)と[ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue)バインド式でのプロパティは省略可能です。

## <a name="defining-a-fallback-value"></a>フォールバック値を定義します。

[ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue)プロパティで使用されるフォールバック値を定義するときに、バインド*ソース*を解決できません。 このプロパティを設定するための一般的なシナリオでは、異機種混在型のバインドされたコレクション内のすべてのオブジェクトが存在しないソースのプロパティにバインドする場合です。

**MonkeyDetail**ページ設定を示しています、 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue)プロパティ。

```xaml
<Label Text="{Binding Population, FallbackValue='Population size unknown'}"
       ... />   
```

バインディング、 [ `Label` ](xref:Xamarin.Forms.Label)定義、 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue)バインディング ソースを解決できない場合に、ターゲットの設定の値。 によって定義された値では、そのため、`FallbackValue`場合プロパティが表示されます、`Population`プロパティがバインドされたオブジェクトに対して存在しません。 ここで注意、`FallbackValue`プロパティの値は単一引用符 (アポストロフィ) 文字で区切られます。

定義するのではなく[ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue)プロパティの値を行内、ことをお勧めのリソースとして定義する、 [ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。 このアプローチの利点は、このような値は 1 つの場所で定義された後がより簡単にローカライズ可能です。 使用して、リソースを取得することができますし、`StaticResource`マークアップ拡張機能。

```xaml
<Label Text="{Binding Population, FallbackValue={StaticResource populationUnknown}}"
       ... />  
```

> [!NOTE]
> 設定することはできません、`FallbackValue`バインディング式を持つプロパティです。

実行中のプログラムを次に示します。

![FallbackValue バインド](binding-fallbacks-images/bindingunavailable-detail-cropped.png "FallbackValue バインド")

ときに、`FallbackValue`バインド式とバインド パスのプロパティが設定されていないか、パスの一部が解決されていない[ `BindableProperty.DefaultValue` ](xref:Xamarin.Forms.BindableProperty.DefaultValue)ターゲットに設定されます。 ただし、`FallbackValue`プロパティが設定され、バインディング パスまたはパスの一部が解決されていないの値、`FallbackValue`ターゲット プロパティの値が設定されています。 そのため、上、 **MonkeyDetail**ページ、 [ `Label` ](xref:Xamarin.Forms.Label) 「母集団のサイズ不明な」が表示されますが、バインドされたオブジェクトがないため、`Population`プロパティ。

> [!IMPORTANT]
> 定義済みの値コンバーターは、バインド式では実行されないときに、 [ `FallbackValue` ](xref:Xamarin.Forms.BindingBase.FallbackValue)プロパティを設定します。

## <a name="defining-a-null-replacement-value"></a>Null の置換値を定義します。

[ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue)プロパティで使用される置換値を定義するときに、バインド*ソース*が解決されるとが、値は`null`。 このプロパティを設定するための一般的なシナリオは、可能性のあるソースのプロパティにバインドするときに`null`でバインドのコレクション。

**猿**ページ設定を示しています、 [ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue)プロパティ。

```xaml
<ListView ItemsSource="{Binding Monkeys}"
          ...>
    <ListView.ItemTemplate>
        <DataTemplate>
            <ViewCell>
                <Grid>
                    ...
                    <Image Source="{Binding ImageUrl, TargetNullValue='https://upload.wikimedia.org/wikipedia/commons/2/20/Point_d_interrogation.jpg'}"
                           ... />
                    ...
                    <Label Text="{Binding Location, TargetNullValue='Location unknown'}"
                           ... />
                </Grid>
            </ViewCell>
        </DataTemplate>
    </ListView.ItemTemplate>
</ListView>
```

バインド、 [ `Image` ](xref:Xamarin.Forms.Image)と[ `Label` ](xref:Xamarin.Forms.Label)両方定義[ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue)バインディング パスがを返す場合に適用される値`null`. によって定義される値、そのため、`TargetNullValue`コレクション内の任意のオブジェクトのプロパティが表示されます場所、`ImageUrl`と`Location`プロパティが定義されていません。 ここで注意、`TargetNullValue`プロパティの値は単一引用符 (アポストロフィ) 文字で区切られます。

定義するのではなく[ `TargetNullValue` ](xref:Xamarin.Forms.BindingBase.TargetNullValue)プロパティの値を行内、ことをお勧めのリソースとして定義する、 [ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。 このアプローチの利点は、このような値は 1 つの場所で定義された後がより簡単にローカライズ可能です。 使用して、リソースを取得することができますし、`StaticResource`マークアップ拡張機能。

```xaml
<Image Source="{Binding ImageUrl, TargetNullValue={StaticResource fallbackImageUrl}}"
       ... />
<Label Text="{Binding Location, TargetNullValue={StaticResource locationUnknown}}"
       ... />
```

> [!NOTE]
> 設定することはできません、`TargetNullValue`バインディング式を持つプロパティです。

実行中のプログラムを次に示します。

[![TargetNullValue バインド](binding-fallbacks-images/bindingunavailable-small.png "TargetNullValue バインド")](binding-fallbacks-images/bindingunavailable-large.png#lightbox "TargetNullValue バインド")

ときに、`TargetNullValue`プロパティは、バインド式では、source の値に設定されていない`null`場合、値コンバーターが定義した場合、フォーマットに変換を`StringFormat`を定義し、結果は、ターゲットの設定。 ただし、`TargetNullValue`プロパティが設定のソース値`null`まだの場合、値コンバーターが定義されている場合に変換`null`の値を変換後、`TargetNullValue`プロパティが設定されて、ターゲット。

> [!IMPORTANT]
> バインディング式の文字列の書式設定が適用されないときに、`TargetNullValue`プロパティを設定します。

## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
