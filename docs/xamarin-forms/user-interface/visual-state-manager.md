---
title: Xamarin Forms State Manager
description: Visual State Manager を使用して、コードから設定されたビジュアルの状態に基づいて XAML 要素を変更します。
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 02/21/2020
ms.openlocfilehash: c6930f3361394b04e90083594e2343b50dac64ab
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517510"
---
# <a name="xamarinforms-visual-state-manager"></a>Xamarin Forms State Manager

[![](~/media/shared/download.png)サンプルをダウンロードするサンプルをダウンロードする](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Visual State Manager を使用して、コードから設定されたビジュアルの状態に基づいて XAML 要素を変更します。_

Visual State Manager (VSM) は、コードからユーザーインターフェイスを視覚的に変更できるように構造化された方法を提供します。 ほとんどの場合、アプリケーションのユーザーインターフェイスは XAML で定義され、この XAML には、Visual State Manager がユーザーインターフェイスのビジュアルにどのように影響するかを説明するマークアップが含まれます。

VSM には、視覚的な_状態_の概念が導入されています。 などの Xamarin 形式のビューでは`Button` 、基になる状態&mdash;に応じて、無効になっているか、押されたか、または入力フォーカスがあるかによって、さまざまな外観を表示できます。 これは、ボタンの状態です。

ビジュアル状態は、表示_状態グループ_で収集されます。 ビジュアル状態グループ内のすべての表示状態は、相互に排他的です。 視覚的な状態と表示状態の両方のグループは、単純なテキスト文字列によって識別されます。

Xamarin 形式の Visual State Manager は、"CommonStates" という名前の1つの表示状態グループを定義します。表示状態は次のとおりです。

- "Normal"
- 無効に
- フォーカス
- オフ

この表示状態グループは、および[`VisualElement`](xref:Xamarin.Forms.VisualElement) [`View`](xref:Xamarin.Forms.View) [`Page`](xref:Xamarin.Forms.Page)の基本クラスであるから派生したすべてのクラスでサポートされています。

この記事で説明するように、独自のビジュアル状態グループと視覚的な状態を定義することもできます。

> [!NOTE]
> Xamarin。[トリガー](~/xamarin-forms/app-fundamentals/triggers.md)を使い慣れている場合は、ビューのプロパティの変更またはイベントの発生に基づいて、トリガーによってユーザーインターフェイスのビジュアルが変更される可能性があることに注意してください。 ただし、これらの変更のさまざまな組み合わせに対応するためにトリガーを使用すると、大幅に混乱する可能性があります。 従来、visual State Manager は、視覚的な状態の組み合わせによって生じる混乱を軽減するために、Windows XAML ベースの環境で導入されました。 VSM では、表示状態グループ内のビジュアル状態は常に相互に排他的です。 各グループの状態は、いつでも現在の状態になります。

## <a name="common-states"></a>一般的な状態

ビジュアル状態マネージャーを使用すると、XAML ファイルにマークアップを含めることができます。これにより、ビューが通常、または無効になっている場合や、入力フォーカスがある場合にビューの外観を変更できます。 これらは_共通の状態_と呼ばれます。

たとえば、ページに`Entry`ビューがあり、の視覚的な外観`Entry`を次のように変更するとします。

- が無効になっている場合、 `Entry`にはピンク色の背景が必要です。 `Entry`
- に`Entry`は、通常、ライムの背景が必要です。
- 入力`Entry`フォーカスがある場合、は通常の高さの2倍になるように展開する必要があります。

個々のビューに VSM マークアップをアタッチすることも、複数のビューに適用する場合はスタイルで定義することもできます。 次の2つのセクションでは、これらの方法について説明します。

### <a name="vsm-markup-on-a-view"></a>ビューの VSM マークアップ

VSM マークアップを`Entry`ビューにアタッチするには、 `Entry`最初にを開始タグと終了タグに分割します。

```xaml
<Entry FontSize="18">

</Entry>
```

状態の1つは、 `FontSize` `Entry`プロパティを使用して内のテキストのサイズを2倍にするため、明示的なフォントサイズが指定されています。

次に、 `VisualStateManager.VisualStateGroups`タグの間にタグを挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty)は、 [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager)アタッチ可能なバインド可能なプロパティで、クラスによって定義されています。 (アタッチ可能なバインド可能なプロパティの詳細については、「[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」を参照してください)。これは、 `VisualStateGroups`プロパティが`Entry`オブジェクトにアタッチされる方法です。

`VisualStateGroups`プロパティは、オブジェクトの[`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList) [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup)コレクションである型です。 `VisualStateManager.VisualStateGroups`タグ内に、含めるビジュアル状態の`VisualStateGroup`グループごとにタグのペアを挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

タグには`VisualStateGroup` 、グループの`x:Name`名前を示す属性があることに注意してください。 クラス`VisualStateGroup`は、代わりに`Name`使用できるプロパティを定義します。

```xaml
<VisualStateGroup Name="CommonStates">
```

または`Name`のいずれ`x:Name`かを使用できますが、両方を同じ要素内で使用することはできません。

クラス`VisualStateGroup`は、オブジェクトの[`States`](xref:Xamarin.Forms.VisualStateGroup.States) [`VisualState`](xref:Xamarin.Forms.VisualState)コレクションであるという名前のプロパティを定義します。 `States`は`VisualStateGroups`の_コンテンツプロパティ_であるため、タグの`VisualState`間`VisualStateGroup`に直接タグを含めることができます。 (コンテンツプロパティについては、「[基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)」で説明しています)。

次の手順では、そのグループ内のすべてのビジュアル状態のタグのペアを含めます。 これらは、または`x:Name` `Name`を使用して識別することもできます。

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

`VisualState`オブジェクトの[`Setter`](xref:Xamarin.Forms.Setter)コレクションで[`Setters`](xref:Xamarin.Forms.VisualState.Setters)あるという名前のプロパティを定義します。 これらは、 [`Style`](xref:Xamarin.Forms.Style)オブジェクト`Setter`で使用するオブジェクトと同じです。

`Setters`は_not_の`VisualState`コンテンツプロパティではないため、プロパティの`Setters`プロパティ要素タグを含める必要があります。

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

タグの`Setters`各ペアの間に`Setter` 1 つ以上のオブジェクトを挿入できるようになりました。 前に説明`Setter`した表示状態を定義するオブジェクトを次に示します。

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

各`Setter`タグは、その状態が current であるときの特定のプロパティの値を示します。 `Setter`オブジェクトによって参照されるプロパティは、バインド可能なプロパティによってサポートされている必要があります。

これに似たマークアップは、 **[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルプログラムの **[ビューの VSM** ] ページの基礎となります。 このページには`Entry` 3 つのビューが含まれていますが、2つ目のビューには、VSM マークアップがアタッチされています。

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

2番目`Entry`のは、 `DataTrigger` `Trigger`コレクションの一部としても含まれていることに注意してください。 これにより`Entry` 、3番目`Entry`のに何かが入力されるまで、が無効になります。 IOS、Android、ユニバーサル Windows プラットフォーム (UWP) で実行される起動時のページを次に示します。

[![ビューの VSM: 無効](vsm-images/VsmOnViewDisabled.png "ビューの VSM-無効")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

現在の表示状態は "無効" です。そのため、iOS `Entry`と Android の画面では、2番目の背景がピンク色になります。 の`Entry` UWP 実装では、 `Entry`が無効になっている場合に背景色を設定することはできません。

3番目`Entry`の部分にテキストを入力すると`Entry` 、2番目のは "通常" の状態に切り替わり、背景は "ライム" になります。

[![ビューの VSM: 通常](vsm-images/VsmOnViewNormal.png "ビューの VSM-標準")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

2番目`Entry`のをタッチすると、入力フォーカスが取得されます。 "フォーカスされた" 状態に切り替わり、2倍の高さに拡張されます。

[![ビューの VSM: フォーカスされる](vsm-images/VsmOnViewFocused.png "ビューにフォーカスされる VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

は、入力`Entry`フォーカスを取得するときに、ライムの背景を保持しないことに注意してください。 ビジュアル状態マネージャーは、表示状態を切り替えるときに、以前の状態に設定されたプロパティの設定が解除されます。 視覚的な状態は相互に排他的であることに注意してください。 "Normal" 状態は、 `Entry`が有効になっているだけではありません。 は、が有効`Entry`になっていて、入力フォーカスがないことを意味します。

が`Entry` "フォーカスされた" 状態でライムの背景を持つようにするに`Setter`は、そのビジュアル状態に別の背景を追加します。

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

これら`Setter`のオブジェクトが正常に機能するために`VisualStateGroup`は、 `VisualState`そのグループ内のすべての状態のオブジェクトがに含まれている必要があります。 `Setter`オブジェクトが含まれていないビジュアル状態がある場合は、空のタグとして追加します。

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>スタイルのビジュアル状態マネージャーマークアップ

多くの場合、2つ以上のビューで同じ Visual State Manager マークアップを共有する必要があります。 この場合は、 `Style`定義にマークアップを配置する必要があります。

次に、 **VSM On ビュー**ページ`Entry`の要素に対する既存の暗黙的`Style`な例を示します。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

アタッチ`Setter`可能なバインド`VisualStateManager.VisualStateGroups`可能なプロパティのタグを追加します。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

の`Setter`コンテンツプロパティは`Value`であるため、 `Value`プロパティの値は、これらのタグ内で直接指定できます。 このプロパティの型`VisualStateGroupList`は次のとおりです。

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

これらのタグ内には、次の`VisualStateGroup`オブジェクトのいずれかを含めることができます。

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

VSM マークアップの残りの部分は、以前と同じです。

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

これで、 `Entry`このページのすべてのビューが、表示状態と同じように応答するようになりました。 また、"フォーカスされた" 状態には、 `Setter`入力フォーカスが`Entry`あるときに、それぞれの背景をライムにするための2番目のが含まれるようになりました。

[![VSM (スタイル)](vsm-images/VsmInStyle.png "VSM (スタイル)")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="visual-states-in-xamarinforms"></a>Xamarin. Forms の表示状態

次の表に、Xamarin で定義されている表示状態の一覧を示します。

| クラス | 状態 | 詳細情報 |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [ボタンの表示状態](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CheckBox` | `IsChecked` | [チェックボックスの表示状態](~/xamarin-forms/user-interface/checkbox.md#checkbox-visual-states) |
| `CarouselView` | `DefaultItem`, `CurrentItem`, `PreviousItem`, `NextItem` | [CarouselView の視覚的状態](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `ImageButton` | `Pressed` | [ImageButton ビジュアルの状態](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `RadioButton` | `IsChecked` | [RadioButton の表示状態](~/xamarin-forms/user-interface/radiobutton.md#radiobutton-visual-states) |
| `VisualElement` | `Normal`, `Disabled`, `Focused`, `Selected` | [一般的な状態](#common-states) |

これらの各状態には、という名前`CommonStates`の表示状態グループを使用してアクセスできます。

さらに、は`CollectionView`状態を`Selected`実装します。 詳細については、「[選択した項目の色を変更](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color)する」を参照してください。

## <a name="set-state-on-multiple-elements"></a>複数の要素の状態を設定する

前の例では、ビジュアルの状態が1つの要素にアタッチされ、操作されていました。 ただし、1つの要素に関連付けられているが、同じスコープ内の他の要素にプロパティを設定する表示状態を作成することもできます。 これにより、状態が動作する各要素に対して視覚的状態を繰り返す必要がなくなります。

[`Setter`](xref:Xamarin.Forms.Setter)型には、 `TargetName`型`string`のプロパティがあります。これは、ビジュアル状態`Setter`のが操作するターゲット要素を表します。 `TargetName`プロパティが定義されると、 `Setter`はで`Property` `TargetName`定義されている要素`Value`のをに設定します。

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

この例では、 `Label`と`label`いう名前の`TextColor`のプロパティが`Red`に設定されています。 `TargetName`プロパティを設定するときは、の`Property`プロパティへの完全なパスを指定する必要があります。 したがっ`TextColor`て、でプロパティを設定する`Label`に`Property`は、を`Label.TextColor`として指定します。

> [!NOTE]
> `Setter`オブジェクトによって参照されるプロパティは、バインド可能なプロパティによってサポートされている必要があります。

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルの [ **Setter with Setter TargetName** ] ページでは、1つのビジュアル状態グループから複数の要素の状態を設定する方法を示します。 XAML `StackLayout`ファイルは、 `Label`要素、 `Entry`、およびを`Button`含むで構成されます。

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

VSM マークアップはにアタッチ`StackLayout`されます。 "Normal" と "押された" という名前の2つの相互排他的な状態`VisualState`があり、各状態にはタグが含まれています。

が`Button`押されていない場合は "Normal" 状態がアクティブになり、質問に対する応答を入力できます。

[![VSM Setter TargetName: 通常の状態](vsm-images/VsmSetterTargetNameNormal.png "VSM setter targetname-標準")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

`Button`が押されると、"押された" 状態がアクティブになります。

[![VSM Setter TargetName: 押された状態](vsm-images/VsmSetterTargetNamePressed.png "VSM setter targetname-押された状態")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

"押された`VisualState` " は、 `Button`が押されると、 `Scale`そのプロパティが既定値の1から0.8 に変更されることを指定します。 また、という`Entry`名前`entry`のでは`Text` 、プロパティがパリに設定されます。 結果として、 `Button`が押されると、スケールが少し小さくなり、にパリ`Entry`が表示されます。 その後、 `Button`が解放されると、スケールは既定値の1になり、 `Entry`に以前に入力したテキストが表示されます。

> [!IMPORTANT]
> プロパティパスは、現在、 `Setter` `TargetName`プロパティを指定する要素ではサポートされていません。

## <a name="define-your-own-visual-states"></a>独自の視覚的状態を定義する

から`VisualElement`派生するすべてのクラスは、共通の状態 "Normal"、"フォーカスされた"、および "Disabled" をサポートしています。 また、クラスは`CollectionView` "Selected" 状態をサポートします。 内部的に[`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs)は、クラスは、有効または無効になったとき、またはフォーカスまたは[`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String))見るされたことを検出し、静的メソッドを呼び出します。

```csharp
VisualStateManager.GoToState(this, "Focused");
```

これは、 `VisualElement`クラスで見つかった唯一の表示状態マネージャーコードです。 は`GoToState` 、から`VisualElement`派生したすべてのクラスに基づいてすべてのオブジェクトに対して呼び出されるので、 `VisualElement` Visual State Manager を任意のオブジェクトと共に使用して、これらの変更に応答することができます。

興味深いことに、ビジュアル状態グループ "CommonStates" の名前はで`VisualElement`明示的に参照されていません。 グループ名は、Visual State Manager の API の一部ではありません。 これまでに示した2つのサンプルプログラムのいずれかで、グループの名前を "CommonStates" から他のものに変更することができ、プログラムは引き続き機能します。 グループ名は、そのグループ内の状態の一般的な説明にすぎません。 すべてのグループのビジュアル状態は相互に排他的であることが暗黙的に認識されます。1つの状態であり、いつでも1つの状態のみが最新です。

独自のビジュアル状態を実装する場合は、コードからを呼び出す`VisualStateManager.GoToState`必要があります。 ほとんどの場合、この呼び出しはページクラスの分離コードファイルから行います。

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルの**VSM 検証**ページでは、入力検証と共に Visual State Manager を使用する方法を示しています。 `StackLayout` XAML ファイルは、、 `Label` `Entry`、およびという2つの要素で`Button`構成されます。

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

VSM マークアップは、( `StackLayout`という`stackLayout`名前の) にアタッチされます。 "Valid" と "Invalid" という名前の2つの相互排他的な状態があり`VisualState` 、各状態にはタグが含まれています。

に`Entry`有効な電話番号が含まれていない場合、現在の状態は "無効" になり`Entry`ます。したがって、はピンク`Label`色の背景を持ち`Button` 、2番目のは表示され、は無効になります。

[![VSM 検証: 状態が無効です](vsm-images/VsmValidationInvalid.png "VSM 検証-無効")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

有効な電話番号を入力すると、現在の状態は "有効" になります。 は`Entry` 、ライムの背景を取得し`Label` 、2番目`Button`のが消え、が有効になりました。

[![VSM 検証: 有効な状態](vsm-images/VsmValidationValid.png "VSM 検証-有効")](vsm-images/VsmValidationValid-Large.png#lightbox)

分離コードファイルは、 `TextChanged` `Entry`からのイベントを処理します。 ハンドラーは、正規表現を使用して、入力文字列が有効かどうかを判断します。 という`GoToState`分離コードファイル内のメソッドは、の`VisualStateManager.GoToState` `stackLayout`静的メソッドを呼び出します。

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

`GoToState`メソッドは、状態を初期化するためにコンストラクターから呼び出されることにも注意してください。 常に現在の状態になっている必要があります。 しかし、コードには、表示状態グループの名前への参照が含まれていますが、わかりやすくするために、XAML では "ValidationStates" として参照されています。

分離コードファイルでは、表示状態を定義するページ上のオブジェクトと、このオブジェクトを呼び出す`VisualStateManager.GoToState`必要があることに注意してください。 これは、両方の表示状態がページ上の複数のオブジェクトを対象としているためです。

コードビハインドファイルが視覚的な状態を定義するページのオブジェクトを参照する必要がある場合は、分離コードファイルがこのオブジェクトと他のオブジェクトに直接アクセスできないのはなぜですか。 確かにできます。 ただし、VSM を使用する利点は、すべての UI デザインを1つの場所に保持することで、ビジュアル要素がどのような状態になるかを XAML 全体で制御できることです。 これにより、分離コードからビジュアル要素に直接アクセスすることで、視覚的な外観を設定することが回避されます。

## <a name="visual-state-triggers"></a>ビジュアル状態のトリガー

表示状態は、を[`VisualState`](xref:Xamarin.Forms.VisualState)適用する条件を定義する特殊なトリガーグループである状態トリガーをサポートします。

状態トリガーは、 [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) [`VisualState`](xref:Xamarin.Forms.VisualState)のコレクションに追加されます。 このコレクションには、1つの状態トリガー、または複数の状態トリガーを含めることができます。 コレクション[`VisualState`](xref:Xamarin.Forms.VisualState)内の状態トリガーがアクティブになると、が適用されます。

状態トリガーを使用して視覚的な状態を制御する場合、Xamarin は次の優先順位規則を使用し[`VisualState`](xref:Xamarin.Forms.VisualState)て、アクティブになるトリガー (および対応する) を決定します。

1. から[`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase)派生したすべてのトリガー。
1. 条件[`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)が満たされた[`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth)ためにアクティブ化された。
1. 条件[`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)が満たされた[`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight)ためにアクティブ化された。

複数のトリガーが同時にアクティブになっている場合 (たとえば、2つのカスタムトリガーの場合)、マークアップで宣言された最初のトリガーが優先されます。

状態トリガーの詳細については、「[状態トリガー](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)」を参照してください。

<a name="adaptive-layout" />

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>アダプティブレイアウトでのビジュアル状態マネージャーの使用

スマートフォンで実行されている Xamarin. フォームアプリケーションは、通常、縦または横の縦横比で表示できます。また、デスクトップで実行されている Xamarin. フォームプログラムは、さまざまなサイズや縦横比を想定するようにサイズ変更できます。 適切にデザインされたアプリケーションでは、さまざまなページまたはウィンドウのフォームファクターに応じてコンテンツが異なる方法で表示されることがあります。

この手法は、_アダプティブレイアウト_とも呼ばれます。 アダプティブレイアウトでは、プログラムのビジュアルのみが必要であるため、ビジュアル状態マネージャーの理想的なアプリケーションです。

単純な例として、アプリケーションのコンテンツに影響を与える小さなボタンのコレクションを表示するアプリケーションがあります。 縦モードでは、これらのボタンはページ上部の水平方向の行に表示されることがあります。

[![VSM アダプティブレイアウト: 縦](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM アダプティブレイアウト-縦")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

横モードでは、ボタンの配列が一方向に移動され、列に表示されることがあります。

[![VSM アダプティブレイアウト: 横](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM アダプティブレイアウト-横")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

このプログラムは、上から下に、ユニバーサル Windows プラットフォーム、Android、および iOS で実行されています。

[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)サンプルの**VSM アダプティブレイアウト**ページでは、"縦" と "横" という名前の2つの表示状態を持つ "OrientationStates" という名前のグループを定義します。 (より複雑な方法は、複数の異なるページやウィンドウの幅に基づいている場合があります)。

VSM マークアップは、XAML ファイル内の4つの場所で実行されます。 と`StackLayout`いう`mainStack`名前のには、 `Image`要素であるメニューとコンテンツの両方が含まれています。 縦`StackLayout`モードの垂直方向と横向きモードの水平方向を持つ必要があります。

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

という`ScrollView`名前`menuScroll`のと`StackLayout`は`menuStack` 、ボタンのメニューを実装します。 これらのレイアウトの向きは、の`mainStack`反対です。 メニューは縦モードで水平にし、横モードで垂直にする必要があります。

VSM マークアップの4番目のセクションは、ボタン自体の暗黙的なスタイルです。 このマークアップ`VerticalOptions`は`HorizontalOptions`、縦`Margin`と横の向きに固有の、、およびの各プロパティを設定します。

分離コードファイルは、コマンド実行`BindingContext`を実装`menuStack` `Button`するためにのプロパティを設定し、さらに`SizeChanged`ページのイベントにハンドラーをアタッチします。

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

この`SizeChanged`ハンドラーは`VisualStateManager.GoToState` 、2つ`StackLayout`の`ScrollView`要素および要素を呼び出し、の`menuStack`子をループ処理`VisualStateManager.GoToState`して`Button`要素を呼び出します。

XAML ファイルの要素のプロパティを設定することによって、分離コードファイルで向きの変更をより直接的に処理できるように見えるかもしれませんが、視覚的な状態マネージャーは、明らかに構造化されたアプローチです。 すべてのビジュアルは XAML ファイルに保持され、簡単に調査、保守、および変更できます。

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visual State Manager と Xamarin 大学

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin. Forms 3.0 Visual State Manager ビデオ**

## <a name="related-links"></a>関連リンク

- [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [状態トリガー](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)
