---
title: Xamarin Forms State Manager
description: コードから設定した visual state に基づく XAML 要素を変更するには、Visual State Manager を使用します。
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2020
ms.openlocfilehash: 0149806f3ab3772bc206cea9540a989d997c817b
ms.sourcegitcommit: f43d5ecafd19cbc5cce39201916a83927a34617a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2020
ms.locfileid: "78215001"
---
# <a name="xamarinforms-visual-state-manager"></a>Xamarin Forms State Manager

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Visual State Manager を使用して、コードから設定されたビジュアルの状態に基づいて XAML 要素を変更します。_

Visual State Manager (VSM) は、コードからユーザーインターフェイスを視覚的に変更できるように構造化された方法を提供します。 ほとんどの場合、アプリケーションのユーザーインターフェイスは XAML で定義され、この XAML には Visual State Manager がユーザーインターフェイスの表示に影響を与える方法を記述するマークアップが含まれています。

VSM には、視覚的な_状態_の概念が導入されています。 `Button` などの Xamarin 形式のビューでは、基になる状態に応じて、視覚化が無効になっているか、押されているか、または入力フォーカスがあるか &mdash; に応じて、さまざまな外観を表示できます。 これらが、ボタンの状態です。

ビジュアル状態は、表示_状態グループ_で収集されます。 すべての visual state group 内の visual state は、相互に排他的です。 visual state と visual state group の両方は、単純なテキスト文字列によって識別されます。

Xamarin 形式の Visual State Manager は、"CommonStates" という名前の1つの表示状態グループを定義します。表示状態は次のとおりです。

- "Normal"
- "Disabled"
- "Focused"
- オフ

この表示状態グループは、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)から派生したすべてのクラスでサポートされています。これは[`View`](xref:Xamarin.Forms.View)と[`Page`](xref:Xamarin.Forms.Page)の基本クラスです。

この記事でも説明しますが、独自の visual state group や visual state を定義することもできます。

> [!NOTE]
> Xamarin。[トリガー](~/xamarin-forms/app-fundamentals/triggers.md)を使い慣れている場合は、ビューのプロパティの変更またはイベントの発生に基づいて、トリガーによってユーザーインターフェイスのビジュアルが変更される可能性があることに注意してください。 ただし、これらの変更のさまざまな組み合わせを処理するためにトリガーを使用すると、かなり紛らわしくなります。 歴史的に、Visual State Manager は、visual state の組み合わせに起因する混乱を軽減するために、Windows の XAML ベースの環境で導入されました。 Visual State Manager を使うと、visual state group 内の visual state は、常に相互に排他的です。 いかなるときも、グループ内の1つの状態だけが、現在の状態になります。

## <a name="common-states"></a>一般的な状態

ビジュアル状態マネージャーを使用すると、XAML ファイルにマークアップを含めることができます。これにより、ビューが通常、または無効になっている場合や、入力フォーカスがある場合にビューの外観を変更できます。 これらは_共通の状態_と呼ばれます。

たとえば、ページに `Entry` ビューがあり、`Entry` の外観を次のように変更したいとします。

- `Entry` が無効になっている場合、`Entry` にはピンク色の背景が必要です。
- `Entry` には、通常はライムの背景が必要です。
- 入力フォーカスがある場合、`Entry` は通常の高さの2倍になるように拡張する必要があります。

個々のビューに VSM マークアップを記述することができます。また複数のビューに適用する場合は、スタイルで定義することもできます。 次の 2 つのセクションでは、これらの方法について説明します。

### <a name="vsm-markup-on-a-view"></a>ビューのマークアップを VSM

VSM マークアップを `Entry` ビューにアタッチするには、最初に `Entry` を開始タグと終了タグに分割します。

```xaml
<Entry FontSize="18">

</Entry>
```

状態の1つは、`FontSize` プロパティを使用して `Entry`内のテキストのサイズを2倍にするため、明示的なフォントサイズが指定されています。

次に、タグの間に `VisualStateManager.VisualStateGroups` タグを挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty)は、 [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager)クラスによって定義された、アタッチ可能なバインド可能プロパティです。 (アタッチ可能なバインド可能なプロパティの詳細については、「[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」を参照してください)。これは、`VisualStateGroups` プロパティが `Entry` オブジェクトにアタッチされる方法です。

`VisualStateGroups` プロパティは、 [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup)オブジェクトのコレクションである[`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList)型です。 `VisualStateManager.VisualStateGroups` タグ内に、含めるビジュアル状態のグループごとに、`VisualStateGroup` タグのペアを挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualStateGroup` タグには、グループの名前を示す `x:Name` 属性があることに注意してください。 `VisualStateGroup` クラスは、代わりに使用できる `Name` プロパティを定義します。

```xaml
<VisualStateGroup Name="CommonStates">
```

`x:Name` または `Name` のいずれかを使用できますが、両方を同じ要素内で使用することはできません。

`VisualStateGroup` クラスは、 [`VisualState`](xref:Xamarin.Forms.VisualState)オブジェクトのコレクションである[`States`](xref:Xamarin.Forms.VisualStateGroup.States)という名前のプロパティを定義します。 `States` は `VisualStateGroups` の_content プロパティ_であるため、`VisualStateGroup` タグの間に `VisualState` タグを直接含めることができます。 (コンテンツプロパティについては、「[基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)」で説明しています)。

次の手順では、グループ内のすべての表示状態にタグのペアを追加します。 これらは、`x:Name` または `Name`を使用して識別することもできます。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">

            </VisualState>

            <VisualState x:Name="Focused">

            </VisualState>

            <VisualState x:Name="Disabled">

            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualState` は、 [`Setter`](xref:Xamarin.Forms.Setter)オブジェクトのコレクションである[`Setters`](xref:Xamarin.Forms.VisualState.Setters)という名前のプロパティを定義します。 これらは、 [`Style`](xref:Xamarin.Forms.Style)オブジェクトで使用するのと同じ `Setter` オブジェクトです。

`Setters` は `VisualState`の content プロパティでは_ない_ため、`Setters` プロパティの property 要素タグを含める必要があります。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>

                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`Setters` タグの各ペアの間に1つ以上の `Setter` オブジェクトを挿入できるようになりました。 前に説明した表示状態を定義する `Setter` オブジェクトを次に示します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">
            <VisualState x:Name="Normal">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Lime" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Focused">
                <VisualState.Setters>
                    <Setter Property="FontSize" Value="36" />
                </VisualState.Setters>
            </VisualState>

            <VisualState x:Name="Disabled">
                <VisualState.Setters>
                    <Setter Property="BackgroundColor" Value="Pink" />
                </VisualState.Setters>
            </VisualState>
        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

各 `Setter` タグは、その状態が current であるときの特定のプロパティの値を示します。 `Setter` オブジェクトによって参照されるプロパティは、バインド可能なプロパティによってサポートされている必要があります。

これに似たマークアップは、 **[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルプログラムの **[ビューの VSM** ] ページの基礎となります。 このページには3つの `Entry` ビューが含まれていますが、2つ目のビューには、VSM マークアップがアタッチされています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:VsmDemos"
             x:Class="VsmDemos.MainPage"
             Title="VSM Demos">

    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />
        <Entry />
        <Label Text="Entry with VSM: " />
        <Entry>
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Focused">
                        <VisualState.Setters>
                            <Setter Property="FontSize" Value="36" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState x:Name="Disabled">
                        <VisualState.Setters>
                            <Setter Property="BackgroundColor" Value="Pink" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>
        <Label Text="Entry to enable 2nd Entry:" />
        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

2番目の `Entry` にも、`Trigger` コレクションの一部として `DataTrigger` が含まれていることに注意してください。 これにより、3つ目の `Entry`に何かが入力されるまで、`Entry` が無効になります。 iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) で実行されている起動時のページを次に示します。

[![ビューの VSM: 無効](vsm-images/VsmOnViewDisabled.png "ビューの VSM-無効")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

現在の表示状態は "Disabled" であるため、2番目の `Entry` の背景は、iOS と Android の画面でピンク色になります。 `Entry` の UWP 実装では、`Entry` が無効になっている場合に背景色を設定することはできません。

3番目の `Entry`にテキストを入力すると、2番目の `Entry` は "通常" の状態に切り替わり、背景は "ライム" になります。

[![ビューの VSM: 通常](vsm-images/VsmOnViewNormal.png "ビューの VSM-標準")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

2番目の `Entry`をタッチすると、入力フォーカスが取得されます。 これにより、「優先」の状態に切り替わり、高さが 2 倍に拡大されます。

[![ビューの VSM: フォーカスされる](vsm-images/VsmOnViewFocused.png "ビューにフォーカスされる VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

`Entry` は、入力フォーカスを取得するときに、ライムの背景を保持しないことに注意してください。 Visual State Manager は、表示状態の間で切り替わるため、以前の状態によって設定されたプロパティはリセットされます。 状態表示は相互に排他的であることを覚えておいてください。 "Normal" 状態は、`Entry` が有効になっていることだけを意味するわけではありません。 `Entry` が有効になっていて、入力フォーカスがないことを意味します。

`Entry` が "フォーカスされた" 状態でライムの背景を持つようにするには、そのビジュアル状態に別の `Setter` を追加します。

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

これらの `Setter` オブジェクトが正常に機能するためには、`VisualStateGroup` に、そのグループ内のすべての状態の `VisualState` オブジェクトが含まれている必要があります。 `Setter` オブジェクトのない視覚的な状態がある場合は、空のタグとして追加します。

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>Style での Visual State Manager マークアップ

2 つ以上のビューの間では、同じ Visual State Manager マークアップを共有することがしばしば必要になります。 この場合は、`Style` 定義にマークアップを配置する必要があります。

次に、 **VSM On ビュー**ページの `Entry` 要素の既存の暗黙的な `Style` を示します。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

`VisualStateManager.VisualStateGroups` アタッチされているバインド可能なプロパティに `Setter` タグを追加します。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

`Setter` のコンテンツプロパティは `Value`ので、`Value` プロパティの値は、これらのタグ内で直接指定できます。 このプロパティの型は `VisualStateGroupList`です。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>

        </VisualStateGroupList>
    </Setter>
</Style>
```

これらのタグ内には、次の `VisualStateGroup` オブジェクトの1つを含めることができます。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">
        <VisualStateGroupList>
            <VisualStateGroup x:Name="CommonStates">

            </VisualStateGroup>
        </VisualStateGroupList>
    </Setter>
</Style>
```

VSM マークアップの残りの部分では前に、と同じです。

次に、vsm の完全なマークアップを示す**vsm のスタイルページを**示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmInStylePage"
             Title="VSM in Style">
    <StackLayout>
        <StackLayout.Resources>
            <Style TargetType="Entry">
                <Setter Property="Margin" Value="20, 0" />
                <Setter Property="FontSize" Value="18" />
                <Setter Property="VisualStateManager.VisualStateGroups">
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Focused">
                                <VisualState.Setters>
                                    <Setter Property="FontSize" Value="36" />
                                    <Setter Property="BackgroundColor" Value="Lime" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Disabled">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor" Value="Pink" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </Setter>
            </Style>

            <Style TargetType="Label">
                <Setter Property="Margin" Value="20, 30, 20, 0" />
                <Setter Property="FontSize" Value="Large" />
            </Style>
        </StackLayout.Resources>

        <Label Text="Normal Entry:" />
        <Entry />
        <Label Text="Entry with VSM: " />
        <Entry>
            <Entry.Triggers>
                <DataTrigger TargetType="Entry"
                             Binding="{Binding Source={x:Reference entry3},
                                               Path=Text.Length}"
                             Value="0">
                    <Setter Property="IsEnabled" Value="False" />
                </DataTrigger>
            </Entry.Triggers>
        </Entry>
        <Label Text="Entry to enable 2nd Entry:" />
        <Entry x:Name="entry3"
               Text=""
               Placeholder="Type something to enable 2nd Entry" />
    </StackLayout>
</ContentPage>
```

これで、このページのすべての `Entry` ビューが、表示状態と同じように応答するようになりました。 また、"フォーカスされた" 状態には2番目の `Setter` が含まれるようになりました。これにより、各 `Entry` に入力フォーカスがある場合にも、緑の背景が表示されます。

[![VSM (スタイル)](vsm-images/VsmInStyle.png "VSM (スタイル)")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="visual-states-in-xamarinforms"></a>Xamarin. Forms の表示状態

次の表に、Xamarin で定義されている表示状態の一覧を示します。

| クラス | 状態 | 詳細情報 |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [ボタンの表示状態](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CarouselView` | `DefaultItem`、`CurrentItem`、`PreviousItem`, `NextItem` | [CarouselView の視覚的状態](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `ImageButton` | `Pressed` | [ImageButton ビジュアルの状態](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `VisualElement` | `Normal`、`Disabled`、`Focused`, `Selected` | [一般的な状態](#common-states) |

これらの各状態には、`CommonStates`という名前の表示状態グループを使用してアクセスできます。

さらに、`CollectionView` は `Selected` の状態を実装します。 詳細については、「[選択した項目の色を変更](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color)する」を参照してください。

## <a name="set-state-on-multiple-elements"></a>複数の要素の状態を設定する

前の例では、ビジュアルの状態が1つの要素にアタッチされ、操作されていました。 ただし、1つの要素に関連付けられているが、同じスコープ内の他の要素にプロパティを設定する表示状態を作成することもできます。 これにより、状態が動作する各要素に対して視覚的状態を繰り返す必要がなくなります。

[`Setter`](xref:Xamarin.Forms.Setter)型には、`string`型の `TargetName` プロパティがあります。これは、ビジュアル状態の `Setter` が操作するターゲット要素を表します。 `TargetName` プロパティが定義されている場合、`Setter` は `TargetName` で定義されている要素の `Property` を `Value`に設定します。

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

この例では、`label` という名前の `Label` の `TextColor` プロパティが `Red`に設定されています。 `TargetName` プロパティを設定する場合は、`Property`でプロパティへの完全なパスを指定する必要があります。 したがって、`Label`の `TextColor` プロパティを設定するには、`Property` を `Label.TextColor`として指定します。

> [!NOTE]
> `Setter` オブジェクトによって参照されるプロパティは、バインド可能なプロパティによってサポートされている必要があります。

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルの **[Setter with Setter TargetName]** ページでは、1つのビジュアル状態グループから複数の要素の状態を設定する方法を示します。 XAML ファイルは、`Label` 要素、`Entry`、および `Button`を含む `StackLayout` で構成されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmSetterTargetNamePage"
             Title="VSM with Setter TargetName">
    <StackLayout Margin="10">
        <Label Text="What is the capital of France?" />
        <Entry x:Name="entry"
               Placeholder="Enter answer" />
        <Button Text="Reveal answer">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup x:Name="CommonStates">
                    <VisualState x:Name="Normal" />
                    <VisualState x:Name="Pressed">
                        <VisualState.Setters>
                            <Setter Property="Scale"
                                    Value="0.8" />
                            <Setter TargetName="entry"
                                    Property="Entry.Text"
                                    Value="Paris" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

VSM マークアップは `StackLayout`にアタッチされます。 "Normal" と "押された" という名前の2つの相互排他的な状態があり、各状態には `VisualState` のタグが含まれています。

"Normal" 状態は、`Button` が押されていない場合にアクティブになり、その質問に対する応答を入力できます。

[![VSM Setter TargetName: 通常の状態](vsm-images/VsmSetterTargetNameNormal.png "VSM setter targetname-標準")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

`Button` が押されると、"押された" 状態がアクティブになります。

[![VSM Setter TargetName: 押された状態](vsm-images/VsmSetterTargetNamePressed.png "VSM setter targetname-押された状態")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

"押された" `VisualState` は、`Button` が押されると、その `Scale` プロパティが既定値の1から0.8 に変更されることを指定します。 また、`entry` という名前の `Entry` の `Text` プロパティがパリに設定されます。 そのため、`Button` を押すと、スケールが少し小さくなり、`Entry` にパリが表示されます。 `Button` が解放されると、スケールは既定値の1になり、前に入力したテキストが `Entry` に表示されます。

> [!IMPORTANT]
> プロパティパスは、`TargetName` プロパティを指定する `Setter` 要素では現在サポートされていません。

## <a name="define-your-own-visual-states"></a>独自の視覚的状態を定義する

`VisualElement` から派生するすべてのクラスは、"Normal"、"フォーカスされた"、"Disabled" という共通の状態をサポートします。 また、`CollectionView` クラスでは、"Selected" 状態がサポートされています。 内部的には、 [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs)クラスは、有効または無効になっているか、フォーカスまたは見るされたことを検出し、静的な[`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String))メソッドを呼び出します。

```csharp
VisualStateManager.GoToState(this, "Focused");
```

これは、`VisualElement` クラスに含まれている唯一の表示状態マネージャーコードです。 `VisualElement`から派生したすべてのクラスに基づいてすべてのオブジェクトに対して `GoToState` が呼び出されるため、Visual State Manager を任意の `VisualElement` オブジェクトと共に使用して、これらの変更に応答することができます。

興味深いことに、ビジュアル状態グループ "CommonStates" の名前は、`VisualElement`で明示的に参照されていません。 グループ名は、Visual State Manager 用の API の一部ではありません。 これまでに示した 2 つのサンプルプログラムの 1 つでは、グループ名を "CommonStates" から別の名前に変更することができ、そのプログラムは引き続き動作します。 グループ名は、単にそのグループ内の状態の一般的な説明にすぎません。 どのグループの表示状態も相互に排他的であるということが暗黙的に認識されます。いつでもただ 1 つの状態だけが現在の状態になります。

独自のビジュアル状態を実装する場合は、コードから `VisualStateManager.GoToState` を呼び出す必要があります。 ほとんどの場合、ページクラスの分離コードファイルからこのメソッドを呼ぶことになるでしょう。

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルの**VSM 検証**ページでは、入力検証と共に Visual State Manager を使用する方法を示しています。 XAML ファイルは、2つの `Label` 要素、`Entry`、および `Button`を含む `StackLayout` で構成されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout x:Name="stackLayout"
                 Padding="10, 10">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">
                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter TargetName="helpLabel"
                                    Property="Label.TextColor"
                                    Value="Transparent" />
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Lime" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter TargetName="entry"
                                    Property="Entry.BackgroundColor"
                                    Value="Pink" />
                            <Setter TargetName="submitButton"
                                    Property="Button.IsEnabled"
                                    Value="False" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />
        <Entry x:Name="entry"
               Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />
        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1" />
        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center" />
    </StackLayout>
</ContentPage>
```

VSM マークアップは、`StackLayout` (名前付き `stackLayout`) にアタッチされます。 "Valid" と "Invalid" という名前の2つの相互排他的な状態があり、各状態には `VisualState` のタグが含まれています。

`Entry` に有効な電話番号が含まれていない場合、現在の状態は "無効" になります。したがって、`Entry` はピンク色で、2番目の `Label` が表示され、`Button` は無効になります。

[![VSM 検証: 状態が無効です](vsm-images/VsmValidationInvalid.png "VSM 検証-無効")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

有効な電話番号を入力すると、現在の状態は「有効」になります。 `Entry` によって、ライムの背景が表示され、2番目の `Label` が消え、`Button` が有効になります。

[![VSM 検証: 有効な状態](vsm-images/VsmValidationValid.png "VSM 検証-有効")](vsm-images/VsmValidationValid-Large.png#lightbox)

分離コードファイルは、`Entry`からの `TextChanged` イベントの処理を担当します。 ハンドラーは、入力文字列が有効かどうかを判断するのに正規表現を使用します。 `GoToState` という名前の分離コードファイル内のメソッドは、`stackLayout`の静的 `VisualStateManager.GoToState` メソッドを呼び出します。

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage()
    {
        InitializeComponent();

        GoToState(false);
    }

    void OnTextChanged(object sender, TextChangedEventArgs args)
    {
        bool isValid = Regex.IsMatch(args.NewTextValue, @"^[2-9]\d{2}-\d{3}-\d{4}$");
        GoToState(isValid);
    }

    void GoToState(bool isValid)
    {
        string visualState = isValid ? "Valid" : "Invalid";
        VisualStateManager.GoToState(stackLayout, visualState);
    }
}
```

`GoToState` メソッドは、状態を初期化するためにコンストラクターから呼び出されることにも注意してください。 現在の状態が常にする必要があります。 それに遠く及ばず、コードでがあります、表示状態グループの名前への参照わかりやすくするための目的で"ValidationStates"として、XAML で参照されています。

分離コードファイルは、表示状態を定義するページ上のオブジェクトと、このオブジェクトに対して `VisualStateManager.GoToState` を呼び出す必要があることに注意してください。 これは、両方の表示状態がページ上の複数のオブジェクトを対象としているためです。

コードビハインドファイルが視覚的な状態を定義するページのオブジェクトを参照する必要がある場合は、分離コードファイルがこのオブジェクトと他のオブジェクトに直接アクセスできないのはなぜですか。 これは間違いでした。 ただし、VSM を使用する利点は、どの視覚的要素を制御できることだけで、XAML UI 設計のすべてを 1 つの場所に保持する別の状態に対応します。 視覚的な外観の設定を視覚的要素を分離コードから直接アクセスすることによって回避できます。

## <a name="visual-state-triggers"></a>ビジュアル状態のトリガー

表示状態には、状態トリガーがサポートされています。これは、 [`VisualState`](xref:Xamarin.Forms.VisualState)を適用する条件を定義する、特殊なトリガーのグループです。

状態トリガーは、 [`VisualState`](xref:Xamarin.Forms.VisualState)の[`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers)コレクションに追加されます。 このコレクションには、1つの状態トリガー、または複数の状態トリガーを含めることができます。 コレクション内の状態トリガーがアクティブになると、 [`VisualState`](xref:Xamarin.Forms.VisualState)が適用されます。

状態トリガーを使用して視覚的な状態を制御する場合、Xamarin は次の優先順位規則を使用して、アクティブにするトリガー (および対応する[`VisualState`](xref:Xamarin.Forms.VisualState)) を決定します。

1. [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase)から派生したすべてのトリガー。
1. [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth)条件に一致したため、 [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)アクティブ化されました。
1. [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight)条件に一致したため、 [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)アクティブ化されました。

複数のトリガーが同時にアクティブになっている場合 (たとえば、2つのカスタムトリガーの場合)、マークアップで宣言された最初のトリガーが優先されます。

状態トリガーの詳細については、「[状態トリガー](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)」を参照してください。

<a name="adaptive-layout" />

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>アダプティブレイアウトでのビジュアル状態マネージャーの使用

スマート フォンで実行されているアプリケーションは、縦または横の縦横比とデスクトップで実行されている Xamarin.Forms プログラムに通常表示できます Xamarin.Forms と多くのさまざまなサイズと縦横比を想定するサイズを変更できます。 適切に設計されたアプリケーションがこれらのさまざまなページまたはウィンドウ フォーム ファクターの異なる方法では、そのコンテンツを表示します。

この手法は、_アダプティブレイアウト_とも呼ばれます。 アダプティブ レイアウトには、プログラムのビジュアルのみが含まれる、ため、Visual State Manager の理想的なアプリケーションになります。

簡単な例は、アプリケーションのコンテンツに影響するボタンの小規模なコレクションを表示するアプリケーションです。 縦モードの場合でこれらのボタンをページ上部にある水平方向の行で表示される可能性があります。

[![VSM アダプティブレイアウト: 縦](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM アダプティブレイアウト-縦")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

横モードでボタンの配列が 1 つの側に移動され、列に表示。

[![VSM アダプティブレイアウト: 横](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM アダプティブレイアウト-横")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

上から下に、プログラムはユニバーサル Windows プラットフォーム、Android、iOS で実行されています。

[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)サンプルの**VSM アダプティブレイアウト**ページでは、"縦" と "横" という名前の2つの表示状態を持つ "OrientationStates" という名前のグループを定義します。 (より複雑なアプローチでは、様々な異なるページやウィンドウの幅に基づくことがあります。)

VSM マークアップは、XAML ファイルの 4 箇所 で発生します。 `mainStack` という名前の `StackLayout` には、`Image` 要素であるメニューとコンテンツの両方が含まれています。 この `StackLayout` では、縦モードの垂直方向と横向きモードの水平方向を持つ必要があります。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmAdaptiveLayoutPage"
             Title="VSM Adaptive Layout">

    <StackLayout x:Name="mainStack">
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup Name="OrientationStates">
                <VisualState Name="Portrait">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Vertical" />
                    </VisualState.Setters>
                </VisualState>
                <VisualState Name="Landscape">
                    <VisualState.Setters>
                        <Setter Property="Orientation" Value="Horizontal" />
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <ScrollView x:Name="menuScroll">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="OrientationStates">
                    <VisualState Name="Portrait">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Horizontal" />
                        </VisualState.Setters>
                    </VisualState>
                    <VisualState Name="Landscape">
                        <VisualState.Setters>
                            <Setter Property="Orientation" Value="Vertical" />
                        </VisualState.Setters>
                    </VisualState>
                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>

            <StackLayout x:Name="menuStack">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroup Name="OrientationStates">
                        <VisualState Name="Portrait">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Horizontal" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState Name="Landscape">
                            <VisualState.Setters>
                                <Setter Property="Orientation" Value="Vertical" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateManager.VisualStateGroups>

                <StackLayout.Resources>
                    <Style TargetType="Button">
                        <Setter Property="VisualStateManager.VisualStateGroups">
                            <VisualStateGroupList>
                                <VisualStateGroup Name="OrientationStates">
                                    <VisualState Name="Portrait">
                                        <VisualState.Setters>
                                            <Setter Property="HorizontalOptions" Value="CenterAndExpand" />
                                            <Setter Property="Margin" Value="10, 5" />
                                        </VisualState.Setters>
                                    </VisualState>
                                    <VisualState Name="Landscape">
                                        <VisualState.Setters>
                                            <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                                            <Setter Property="HorizontalOptions" Value="Center" />
                                            <Setter Property="Margin" Value="10" />
                                        </VisualState.Setters>
                                    </VisualState>
                                </VisualStateGroup>
                            </VisualStateGroupList>
                        </Setter>
                    </Style>
                </StackLayout.Resources>

                <Button Text="Banana"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="Banana.jpg" />
                <Button Text="Face Palm"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="FacePalm.jpg" />
                <Button Text="Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="monkey.png" />
                <Button Text="Seated Monkey"
                        Command="{Binding SelectedCommand}"
                        CommandParameter="SeatedMonkey.jpg" />
            </StackLayout>
        </ScrollView>

        <Image x:Name="image"
               VerticalOptions="FillAndExpand"
               HorizontalOptions="FillAndExpand" />
    </StackLayout>
</ContentPage>
```

`menuScroll` という名前の内部 `ScrollView` と `menuStack` という名前の `StackLayout` には、ボタンのメニューが実装されています。 これらのレイアウトの向きは、`mainStack`とは逆です。 メニューは縦向きモードでは水平に、横向きモードでは垂直である必要があります。

VSM マークアップの 4 番目のセクションは、ボタン自体の暗黙的なスタイルが使用されています。 このマークアップは、縦と横の向きに固有の `VerticalOptions`、`HorizontalOptions`、および `Margin` プロパティを設定します。

分離コードファイルは、`menuStack` の `BindingContext` プロパティを設定して `Button` のコマンドを実装し、さらに、ページの `SizeChanged` イベントにハンドラーをアタッチします。

```csharp
public partial class VsmAdaptiveLayoutPage : ContentPage
{
    public VsmAdaptiveLayoutPage ()
    {
        InitializeComponent ();

        SizeChanged += (sender, args) =>
        {
            string visualState = Width > Height ? "Landscape" : "Portrait";
            VisualStateManager.GoToState(mainStack, visualState);
            VisualStateManager.GoToState(menuScroll, visualState);
            VisualStateManager.GoToState(menuStack, visualState);

            foreach (View child in menuStack.Children)
            {
                VisualStateManager.GoToState(child, visualState);
            }
        };

        SelectedCommand = new Command<string>((filename) =>
        {
            image.Source = ImageSource.FromResource("VsmDemos.Images." + filename);
        });

        menuStack.BindingContext = this;
    }

    public ICommand SelectedCommand { private set; get; }
}
```

`SizeChanged` ハンドラーは、2つの `StackLayout` および `ScrollView` 要素の `VisualStateManager.GoToState` を呼び出し、`menuStack` の子をループ処理して `VisualStateManager.GoToState` 要素の `Button` を呼び出します。

分離コードファイルで、XAML ファイルの要素のプロパティを設定すれば、より直接的に向きの変更を処理できるかのように思われるかもしれませんが、Visual State Manager は間違いなくより構造化されたアプローチです。 全てのビジュアルは XAML ファイル内で保持されるため、調査、メンテナンス、変更がしやすくなります。

## <a name="visual-state-manager-with-xamarinuniversity"></a>Xamarin.University で visual State Manager

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin. Forms 3.0 Visual State Manager ビデオ**

## <a name="related-links"></a>関連リンク

- [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [状態トリガー](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)
