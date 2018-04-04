---
title: バインド パス
description: サブ プロパティにアクセスし、コレクションのメンバーへのデータ バインディングを使用します。
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: f75cfcf4bfd5ffa71699f62b30145b732421d964
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="binding-path"></a>バインド パス

例ではすべて、以前のデータ バインディング、 [ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Binding.Path/)のプロパティ、`Binding`クラス (または[ `Path` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Path/)のプロパティ、`Binding`マークアップ拡張機能) が設定されています。1 つのプロパティです。 設定を実際に可能であれば`Path`を*サブプロパティ*(プロパティのプロパティ)、またはコレクションのメンバーにします。

たとえば、ページが含まれている、 `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time`プロパティの`TimePicker`の種類は`TimeSpan`を参照するデータ バインドを作成したい場合は、`TotalSeconds`そのプロパティ`TimeSpan`値。 データ バインディングを次に示します。

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```
         
`Time`プロパティの型は`TimeSpan`を持つ、`TotalSeconds`プロパティです。 `Time`と`TotalSeconds`プロパティがピリオドで接続しています。 内の項目、`Path`プロパティには、これらのプロパティの型が文字列は常に参照します。

例およびその他のいくつかで表示されている、**パス バリエーション**ページ。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:globe="clr-namespace:System.Globalization;assembly=mscorlib"
             x:Class="DataBindingDemos.PathVariationsPage"
             Title="Path Variations"
             x:Name="page">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Label">
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="HorizontalTextAlignment" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    
    <StackLayout Margin="10, 0">
        <TimePicker x:Name="timePicker" />

        <Label Text="{Binding Source={x:Reference timePicker},
                              Path=Time.TotalSeconds,
                              StringFormat='{0} total seconds'}" />

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children.Count,
                              StringFormat='There are {0} children in this StackLayout'}" />
        
        <Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                              Path=DateTimeFormat.DayNames[3],
                              StringFormat='The middle day of the week is {0}'}" />

        <Label>
            <Label.Text>
                <Binding Path="DateTimeFormat.DayNames[3]"
                         StringFormat="The middle day of the week in France is {0}">
                    <Binding.Source>
                        <globe:CultureInfo>
                            <x:Arguments>
                                <x:String>fr-FR</x:String>
                            </x:Arguments>
                        </globe:CultureInfo>
                    </Binding.Source>
                </Binding>
            </Label.Text>
        </Label>

        <Label Text="{Binding Source={x:Reference page},
                              Path=Content.Children[1].Text.Length,
                              StringFormat='The first Label has {0} characters'}" />
    </StackLayout>
</ContentPage>
```

2 番目の`Label`、バインド ソースは、ページ自体です。 `Content`プロパティの型は`StackLayout`を持つ、`Children`型のプロパティ`IList<View>`を持つ、`Count`子供の数を示すプロパティです。

## <a name="paths-with-indexers"></a>インデクサーのパス

3 番目のバインディング`Label`で、**パス バリエーション**ページを参照、 [ `CultureInfo` ](https://developer.xamarin.com/api/type/System.Globalization.CultureInfo/)クラス内で、`System.Globalization`名前空間。

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

ソースが、静的に設定されている`CultureInfo.CurrentCulture`プロパティと型のオブジェクトは`CultureInfo`します。 クラスをという名前のプロパティを定義する`DateTimeFormat`型の[ `DateTimeFormatInfo` ](https://developer.xamarin.com/api/type/System.Globalization.DateTimeFormatInfo/)を格納している、`DayNames`コレクション。 インデックスでは、4 番目の項目を選択します。

4 番目`Label`何かフランスに関連付けられているカルチャが似ています。 `Source`バインドのプロパティに設定されて`CultureInfo`コンス トラクターを持つオブジェクト。

```xaml
<Label>
    <Label.Text>
        <Binding Path="DateTimeFormat.DayNames[3]"
                 StringFormat="The middle day of the week in France is {0}">
            <Binding.Source>
                <globe:CultureInfo>
                    <x:Arguments>
                        <x:String>fr-FR</x:String>
                    </x:Arguments>
                </globe:CultureInfo>
            </Binding.Source>
        </Binding>
    </Label.Text>
</Label>
```

参照してください[コンス トラクターの引数を渡す](~/xamarin-forms/xaml/passing-arguments.md#constructor_arguments)XAML でコンス トラクターの引数を指定する方法の詳細についてはします。

最後に、最後の例がに似ていますが、2 番目の子のいずれかを参照する点を除いて、 `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

その子は、`Label`を持つ、`Text`型のプロパティ`String`を持つ、`Length`プロパティです。 最初の`Label`レポート、`TimeSpan`で設定、`TimePicker`そのテキストが変更されたときに、最終的な`Label`も変わります。

3 つすべてのプラットフォームで実行されているプログラムを次に示します。

[![パスのバリエーション](binding-path-images/pathvariations-small.png "パス バリエーション")](binding-path-images/pathvariations-large.png#lightbox "パスのバリエーション")

## <a name="debugging-complex-paths"></a>複合パスのデバッグ

複合パスの定義を構築するが困難にすることができます。 各サブ プロパティの型、または正しくサブプロパティ [次へ] を追加するコレクション内の項目の種類を理解する必要がありますが、型自体は、パスには表示されません。 適切な手法の 1 つでは、インクリメンタル ビルド パスを構成し、中間結果を確認します。 その最後の例のなしで起動することが`Path`すべてで定義します。

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

バインディング ソースの種類を表示するか、`DataBindingDemos.PathVariationsPage`です。 わかって`PathVariationsPage`から派生した`ContentPage`があるので、`Content`プロパティ。

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

型、`Content`プロパティに明らかにするようになりました`Xamarin.Forms.StackLayout`です。 追加、`Children`プロパティを`Path`され、型は`Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`、これは、Xamarin.Forms が明らかに、コレクション型の内部クラス。 インデックスを追加して、型が`Xamarin.Forms.Label`です。 この方法で続行します。

Xamarin.Forms では、バインド パスを処理すると、インストール、`PropertyChanged`ハンドラーを実装するパスの任意のオブジェクトに、`INotifyPropertyChanged`インターフェイスです。 最後のバインドが、最初の変更に反応するなど、`Label`ため、`Text`プロパティが変更されました。 

バインド パスでは、プロパティを実装しないかどうか`INotifyPropertyChanged`、そのプロパティへの変更は無視されます。 いくつかの変更には、決してプロパティとサブプロパティの文字列が無効になる場合にのみ、この手法を使用する必要がありますので、バインド パスが無効にまったく可能性があります。



## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 帳からのデータ バインディング章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
