---
title: "Xamarin.Forms のバインドのフォールバック" の説明: "この記事では、バインドが失敗した場合に使用されるフォールバック値を定義することでバインドをより堅牢にする方法について説明します。"
ms.prod: xamarin ms.assetid: 637ACD9D-3E5D-4014-86DE-A77D1FEF238A ms.technology: xamarin-forms author: davidbritch ms.author: dabritch ms.date: 08/16/2018 no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# <a name="xamarinforms-binding-fallbacks"></a>Xamarin.Forms でのバインドのフォールバック

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

バインディング ソースを解決できないため、またはバインドは成功しても `null` 値が返されるために、データ バインディングが失敗する場合があります。 これらのシナリオについては、値コンバーターまたは他の追加コードで対処できますが、バインド プロセスが失敗した場合に使用されるフォールバック値を定義することでデータ バインディングをより堅牢にすることができます。 このためには、バインド式で [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) プロパティと [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) プロパティを定義します。 これらのプロパティは、[`BindingBase`](xref:Xamarin.Forms.BindingBase) クラス内にあるため、バインド、コンパイル済みのバインド、`Binding` マークアップ拡張で使用することができます。

> [!NOTE]
> バインド式での [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) プロパティと [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) プロパティの使用は任意です。

## <a name="defining-a-fallback-value"></a>フォールバック値の定義

[`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) プロパティを使用すると、バインディング *ソース*を解決できない場合に使用されるフォールバック値を定義できます。 このプロパティを設定する一般的なシナリオは、バインドされた異種型コレクション内のすべてのオブジェクトに存在しない可能性のあるソース プロパティにバインドする場合です。

次の **MonkeyDetail** ページは、[`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) プロパティの設定例を示します。

```xaml
<Label Text="{Binding Population, FallbackValue='Population size unknown'}"
       ... />   
```

[`Label`](xref:Xamarin.Forms.Label) でのバインドは、バインディング ソースを解決できない場合にターゲットで設定される [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) 値を定義します。 このため、`FallbackValue` プロパティで定義される値は、バインドされたオブジェクトに `Population` プロパティが存在しない場合に表示されます。 ここでは、`FallbackValue` プロパティ値が一重引用符 (アポストロフィ) 文字で区切られていることに注意してください。

[`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) プロパティ値をインラインで定義しないで、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) でリソースとして定義することをお勧めします。 この方法を使用すると、このような値を 1 か所で 1 回定義するだけで済み、ローカライズがより簡単になるという利点があります。 リソースは、`StaticResource` マークアップ拡張を使用して取得することができます。

```xaml
<Label Text="{Binding Population, FallbackValue={StaticResource populationUnknown}}"
       ... />  
```

> [!NOTE]
> バインド式を使用して `FallbackValue` を設定することはできません。

実行中のプログラムを次に示します。

![FallbackValue のバインド](binding-fallbacks-images/bindingunavailable-detail-cropped.png "FallbackValue のバインド")

バインド式に `FallbackValue` プロパティが設定されておらず、バインド パスまたはその一部が解決されない場合、ターゲットでは、[`BindableProperty.DefaultValue`](xref:Xamarin.Forms.BindableProperty.DefaultValue) が設定されます。 一方、バインド式に `FallbackValue` プロパティが設定されており、バインド パスまたはその一部が解決されない場合、ターゲットでは、`FallbackValue` 値プロパティの値が設定されます。 したがって、**MonkeyDetail** ページでは、バインドされたオブジェクトに `Population` プロパティがないため、[`Label`](xref:Xamarin.Forms.Label) により、"Population size unknown" (作成サイズが不明) が表示されます。

> [!IMPORTANT]
> [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) プロパティが設定されている場合、バインド式で、定義済みの値コンバーターは実行されません。

## <a name="defining-a-null-replacement-value"></a>null の置換値の定義

[`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) プロパティを使用すると、バインド *ソース*は解決できても値が `null` である場合に使用される置換値を定義できます。 このプロパティを設定する一般的なシナリオは、値が `null` である可能性のある、バインド コレクション内のソース プロパティにバインドする場合です。

次の **Monkeys** ページは、[`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) プロパティの設定例を示します。

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

[`Image`](xref:Xamarin.Forms.Image) および [`Label`](xref:Xamarin.Forms.Label) でのバインドは両方とも、バインド パスが `null` を返す場合に適用される [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) 値を定義します。 このため、`ImageUrl` および `Location` が定義されていない場合、コレクション内のオブジェクトに対して `TargetNullValue` で定義された値が表示されます。 ここでは、`TargetNullValue` プロパティ値が一重引用符 (アポストロフィ) 文字で区切られていることに注意してください。

[`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) プロパティ値をインラインで定義しないで、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) でリソースとして定義することをお勧めします。 この方法を使用すると、このような値を 1 か所で 1 回定義するだけで済み、ローカライズがより簡単になるという利点があります。 リソースは、`StaticResource` マークアップ拡張を使用して取得することができます。

```xaml
<Image Source="{Binding ImageUrl, TargetNullValue={StaticResource fallbackImageUrl}}"
       ... />
<Label Text="{Binding Location, TargetNullValue={StaticResource locationUnknown}}"
       ... />
```

> [!NOTE]
> バインド式を使用して `TargetNullValue` を設定することはできません。

実行中のプログラムを次に示します。

[![TargetNullValue のバインド](binding-fallbacks-images/bindingunavailable-small.png "TargetNullValue のバインド")](binding-fallbacks-images/bindingunavailable-large.png#lightbox "TargetNullValue のバインド")

バインド式に `TargetNullValue` プロパティが設定されていない場合、ソース値の `null` は、変換され (値コンバーターが定義されている場合)、書式設定された (`StringFormat` が定義されている場合) 後、その結果がターゲットで設定されます。 一方、`TargetNullValue` プロパティが設定されている場合、ソース値 `null` は、値コンバーターが定義されていれば変換されます。変換後も `null` のままであれば、ターゲットでは `TargetNullValue` プロパティの値が設定されます。

> [!IMPORTANT]
> `TargetNullValue` プロパティが設定されている場合、バインド式で文字列の書式設定は適用されません。

## <a name="related-links"></a>関連リンク

- [データ バインディングのデモ (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)
