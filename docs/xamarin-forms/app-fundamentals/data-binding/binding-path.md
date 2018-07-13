---
title: Xamarin.Forms のバインド パス
description: この記事では、Xamarin.Forms のデータ バインディングを使用してサブプロパティとバインディング クラスのパスのプロパティを持つコレクションのメンバーにアクセスする方法について説明します。
ms.prod: xamarin
ms.assetid: 3CF721A5-E157-468B-AD3A-DA0A45E58E8D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 887a20f1791a190c182e6d179cfabb46c6e0eb48
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998948"
---
# <a name="xamarinforms-binding-path"></a>Xamarin.Forms のバインド パス

前のデータ バインディング例をすべてで、 [ `Path` ](xref:Xamarin.Forms.Binding.Path)のプロパティ、`Binding`クラス (または[ `Path` ](xref:Xamarin.Forms.Xaml.BindingExtension.Path)のプロパティ、`Binding`マークアップ拡張機能) が設定されています。1 つのプロパティ。 実際には、設定することは`Path`を*サブプロパティ*(プロパティのプロパティ)、またはコレクションのメンバーにします。

たとえば、ページが含まれています、 `TimePicker`:

```xaml
<TimePicker x:Name="timePicker">
```

`Time`プロパティの`TimePicker`の種類は`TimeSpan`を参照するデータ バインディングを作成する場合がありますが、`TotalSeconds`プロパティを`TimeSpan`値。 データ バインディングを次に示します。

```xaml
{Binding Source={x:Reference timePicker},
         Path=Time.TotalSeconds}
```

`Time`プロパティの型は`TimeSpan`を持つ、`TotalSeconds`プロパティ。 `Time`と`TotalSeconds`プロパティは、ピリオドで接続しています。 内の項目、`Path`プロパティには、これらのプロパティの型が文字列は常に参照します。

例と他のいくつかのユーザーに表示されている、**パス バリエーション**ページ。

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

2 番目の`Label`、バインディング ソースは、ページ自体。 `Content`プロパティの型は`StackLayout`を持つ、`Children`型のプロパティ`IList<View>`を持つ、`Count`子の数を示すプロパティです。

## <a name="paths-with-indexers"></a>インデクサーのパス

3 番目のバインド`Label`で、**パス バリエーション**ページを参照、 [ `CultureInfo` ](xref:System.Globalization.CultureInfo)クラス、`System.Globalization`名前空間。

```xaml
<Label Text="{Binding Source={x:Static globe:CultureInfo.CurrentCulture},
                      Path=DateTimeFormat.DayNames[3],
                      StringFormat='The middle day of the week is {0}'}" />
```

ソースが、静的に設定されている`CultureInfo.CurrentCulture`型のオブジェクトであるプロパティ`CultureInfo`します。 クラスがという名前のプロパティを定義する`DateTimeFormat`型の[ `DateTimeFormatInfo` ](xref:System.Globalization.DateTimeFormatInfo)を格納している、`DayNames`コレクション。 インデックスは、4 番目の項目を選択します。

4 番目`Label`フランスに関連付けられているカルチャが同様の処理を実行します。 `Source`バインドのプロパティに設定されて`CultureInfo`コンス トラクターを持つオブジェクト。

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

最後に、最後の例では、似ていますが、2 番目の子のいずれかを参照して、 `StackLayout`:

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content.Children[1].Text.Length,
                      StringFormat='The first Label has {0} characters'}" />
```

その子は、`Label`を持つ、`Text`型のプロパティ`String`を持つ、`Length`プロパティ。 最初の`Label`レポート、`TimeSpan`設定、`TimePicker`そのテキストが変更されたときに、最終的な`Label`にも変更します。

3 つすべてのプラットフォームで実行されているプログラムを次に示します。

[![パスのバリエーション](binding-path-images/pathvariations-small.png "パス バリエーション")](binding-path-images/pathvariations-large.png#lightbox "パスのバリエーション")

## <a name="debugging-complex-paths"></a>複雑なパスのデバッグ

複合パスの定義の作成は難しい。 各サブ プロパティの型か、[次へ] サブ プロパティが正常に追加するコレクション内の項目の種類を知っておく必要がありますが、型自体は、パスには表示されません。 1 つの優れた手法では、インクリメンタル ビルドをパスして、中間結果を見てです。 その最後の例のなしで開始でした`Path`ですべての定義。

```xaml
<Label Text="{Binding Source={x:Reference page},
                      StringFormat='{0}'}" />
```

バインディング ソースの種類を表示するか、`DataBindingDemos.PathVariationsPage`します。 わかって`PathVariationsPage`から派生した`ContentPage`のでがある、`Content`プロパティ。

```xaml
<Label Text="{Binding Source={x:Reference page},
                      Path=Content,
                      StringFormat='{0}'}" />
```

種類、`Content`プロパティにある明らかになったようになりました`Xamarin.Forms.StackLayout`します。 追加、`Children`プロパティを`Path`、種類が`Xamarin.Forms.ElementCollection'1[Xamarin.Forms.View]`、これは、Xamarin.Forms が明らかに、コレクション型の内部クラスです。 インデックスを追加して、型が`Xamarin.Forms.Label`します。 この方法で続行します。

Xamarin.Forms では、バインド パスを処理すると、インストール、`PropertyChanged`ハンドラーを実装するパスの任意のオブジェクト、`INotifyPropertyChanged`インターフェイス。 最終的なバインドが 1 つ目の変更に反応するなど、`Label`ため、`Text`プロパティの変更。

かどうか、バインド パスにプロパティを実装しません`INotifyPropertyChanged`、そのプロパティに変更が無視されます。 いくつかの変更には、決してプロパティおよびサブプロパティの文字列が無効になる場合にのみ、この手法を使用する必要がありますので、バインド パスが無効にまったくでした。



## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms book からデータ バインド」の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
