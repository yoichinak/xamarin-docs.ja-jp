---
title: Xamarin.Forms ビジュアル状態マネージャー
description: Visual State Manager を使用して、コードから設定されたビジュアルの状態に基づいて XAML 要素を変更します。
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/19/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7e59cddbe9192f29ca1636c567131aad60157066
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91556582"
---
# <a name="no-locxamarinforms-visual-state-manager"></a>Xamarin.Forms ビジュアル状態マネージャー

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Visual State Manager を使用して、コードから設定されたビジュアルの状態に基づいて XAML 要素を変更します。_

Visual State Manager (VSM) は、コードからユーザーインターフェイスを視覚的に変更できるように構造化された方法を提供します。 ほとんどの場合、アプリケーションのユーザーインターフェイスは XAML で定義され、この XAML には、Visual State Manager がユーザーインターフェイスのビジュアルにどのように影響するかを説明するマークアップが含まれます。

VSM には、視覚的な _状態_の概念が導入されています。 などのビューは、その Xamarin.Forms `Button` 基になる状態に応じて、無効に &mdash; なっているか、押されているか、または入力フォーカスがあるかによって、さまざまな視覚外観を持つことができます。 これは、ボタンの状態です。

ビジュアル状態は、表示 _状態グループ_で収集されます。 ビジュアル状態グループ内のすべての表示状態は、相互に排他的です。 視覚的な状態と表示状態の両方のグループは、単純なテキスト文字列によって識別されます。

Xamarin.FormsVisual State Manager は、"CommonStates" という名前の1つの表示状態グループを定義します。表示状態は次のとおりです。

- "Normal"
- 無効に
- フォーカス
- オフ

この表示状態グループは [`VisualElement`](xref:Xamarin.Forms.VisualElement) 、およびの基本クラスであるから派生したすべてのクラスでサポートされてい [`View`](xref:Xamarin.Forms.View) [`Page`](xref:Xamarin.Forms.Page) ます。

この記事で説明するように、独自のビジュアル状態グループと視覚的な状態を定義することもできます。

> [!NOTE]
> Xamarin.Forms トリガーを使い慣れている開発 [者は、](~/xamarin-forms/app-fundamentals/triggers.md) ビューのプロパティの変更またはイベントの発生に基づいて、ユーザーインターフェイスのビジュアルをトリガーが変更できることに注意してください。 ただし、これらの変更のさまざまな組み合わせに対応するためにトリガーを使用すると、大幅に混乱する可能性があります。 従来、visual State Manager は、視覚的な状態の組み合わせによって生じる混乱を軽減するために、Windows XAML ベースの環境で導入されました。 VSM では、表示状態グループ内のビジュアル状態は常に相互に排他的です。 各グループの状態は、いつでも現在の状態になります。

## <a name="common-states"></a>一般的な状態

ビジュアル状態マネージャーを使用すると、XAML ファイルにマークアップを含めることができます。これにより、ビューが通常、または無効になっている場合や、入力フォーカスがある場合にビューの外観を変更できます。 これらは _共通の状態_と呼ばれます。

たとえば、 `Entry` ページにビューがあり、の視覚的な外観を次のように変更するとし `Entry` ます。

- が無効になっている場合、には `Entry` ピンク色の背景が必要 `Entry` です。
- には、 `Entry` 通常、ライムの背景が必要です。
- `Entry`入力フォーカスがある場合、は通常の高さの2倍になるように展開する必要があります。

個々のビューに VSM マークアップをアタッチすることも、複数のビューに適用する場合はスタイルで定義することもできます。 次の2つのセクションでは、これらの方法について説明します。

### <a name="vsm-markup-on-a-view"></a>ビューの VSM マークアップ

VSM マークアップをビューにアタッチするに `Entry` は、最初にを `Entry` 開始タグと終了タグに分割します。

```xaml
<Entry FontSize="18">

</Entry>
```

状態の1つは、プロパティを使用して `FontSize` 内のテキストのサイズを2倍にするため、明示的なフォントサイズが指定されて `Entry` います。

次に、タグの間にタグを挿入し `VisualStateManager.VisualStateGroups` ます。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) は、アタッチ可能なバインド可能なプロパティで、クラスによって定義されて [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) います。 (アタッチ可能なバインド可能なプロパティの詳細については、「 [添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」を参照してください)。これは、 `VisualStateGroups` プロパティがオブジェクトにアタッチされる方法です `Entry` 。

`VisualStateGroups`プロパティは [`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList) 、オブジェクトのコレクションである型です [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) 。 タグ内に `VisualStateManager.VisualStateGroups` 、 `VisualStateGroup` 含めるビジュアル状態のグループごとにタグのペアを挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualStateGroup`タグには、 `x:Name` グループの名前を示す属性があることに注意してください。 クラスは、 `VisualStateGroup` `Name` 代わりに使用できるプロパティを定義します。

```xaml
<VisualStateGroup Name="CommonStates">
```

またはのいずれかを使用でき `x:Name` `Name` ますが、両方を同じ要素内で使用することはできません。

`VisualStateGroup`クラスは、オブジェクトのコレクションであるという名前のプロパティを定義し [`States`](xref:Xamarin.Forms.VisualStateGroup.States) [`VisualState`](xref:Xamarin.Forms.VisualState) ます。 `States` はの _コンテンツプロパティ_ であるため、タグの `VisualStateGroups` 間に直接タグを含めることができ `VisualState` `VisualStateGroup` ます。 (コンテンツプロパティについては、「 [基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)」で説明しています)。

次の手順では、そのグループ内のすべてのビジュアル状態のタグのペアを含めます。 これらは、またはを使用して識別することもでき `x:Name` `Name` ます。

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

`VisualState` オブジェクトのコレクションであるという名前のプロパティを定義し [`Setters`](xref:Xamarin.Forms.VisualState.Setters) [`Setter`](xref:Xamarin.Forms.Setter) ます。 これらは、 `Setter` オブジェクトで使用するオブジェクトと同じ [`Style`](xref:Xamarin.Forms.Style) です。

`Setters` はのコンテンツプロパティでは _ない_ ため、プロパティ `VisualState` のプロパティ要素タグを含める必要があり `Setters` ます。

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

`Setter`タグの各ペアの間に1つ以上のオブジェクトを挿入できるようになりました `Setters` 。 `Setter`前に説明した表示状態を定義するオブジェクトを次に示します。

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

各 `Setter` タグは、その状態が current であるときの特定のプロパティの値を示します。 オブジェクトによって参照 `Setter` されるプロパティは、バインド可能なプロパティによってサポートされている必要があります。

これに似たマークアップは、 **[VsmDemos](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルプログラムの **[ビューの VSM** ] ページの基礎となります。 このページには3つのビューが含まれてい `Entry` ますが、2つ目のビューには、VSM マークアップがアタッチされています。

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

2番目のは、 `Entry` `DataTrigger` コレクションの一部としても含まれていることに注意して `Trigger` ください。 これにより、 `Entry` 3 番目のに何かが入力されるまで、が無効になり `Entry` ます。 IOS、Android、ユニバーサル Windows プラットフォーム (UWP) で実行される起動時のページを次に示します。

[![ビューの VSM: 無効](vsm-images/VsmOnViewDisabled.png "ビューの VSM-無効")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

現在の表示状態は "無効" です。そのため、iOS と Android の画面では、2番目の背景 `Entry` がピンク色になります。 の UWP 実装では `Entry` 、が無効になっている場合に背景色を設定することはできません `Entry` 。

3番目の部分にテキストを入力すると、 `Entry` 2 番目のは `Entry` "通常" の状態に切り替わり、背景は "ライム" になります。

[![ビューの VSM: 通常](vsm-images/VsmOnViewNormal.png "ビューの VSM-標準")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

2番目のをタッチすると、 `Entry` 入力フォーカスが取得されます。 "フォーカスされた" 状態に切り替わり、2倍の高さに拡張されます。

[![ビューの VSM: フォーカスされる](vsm-images/VsmOnViewFocused.png "ビューにフォーカスされる VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

は、 `Entry` 入力フォーカスを取得するときに、ライムの背景を保持しないことに注意してください。 ビジュアル状態マネージャーは、表示状態を切り替えるときに、以前の状態に設定されたプロパティの設定が解除されます。 視覚的な状態は相互に排他的であることに注意してください。 "Normal" 状態は、が有効になっているだけではありません `Entry` 。 は、 `Entry` が有効になっていて、入力フォーカスがないことを意味します。

が "フォーカスされた `Entry` " 状態でライムの背景を持つようにするには、そのビジュアル状態に別の背景を追加し `Setter` ます。

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

これらのオブジェクトが `Setter` 正常に機能するためには、 `VisualStateGroup` `VisualState` そのグループ内のすべての状態のオブジェクトがに含まれている必要があります。 オブジェクトが含まれていないビジュアル状態がある場合は `Setter` 、空のタグとして追加します。

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>スタイルのビジュアル状態マネージャーマークアップ

多くの場合、2つ以上のビューで同じ Visual State Manager マークアップを共有する必要があります。 この場合は、定義にマークアップを配置する必要が `Style` あります。

次に、 `Style` `Entry` **VSM On ビュー** ページの要素に対する既存の暗黙的な例を示します。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

`Setter`アタッチ可能なバインド可能なプロパティのタグを追加し `VisualStateManager.VisualStateGroups` ます。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

のコンテンツプロパティ `Setter` は `Value` であるため、プロパティの値は、 `Value` これらのタグ内で直接指定できます。 このプロパティの型は `VisualStateGroupList` 次のとおりです。

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

これらのタグ内には、次のオブジェクトのいずれかを含めることができ `VisualStateGroup` ます。

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

次に、vsm の完全なマークアップを示す **vsm のスタイルページを** 示します。

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

これで、 `Entry` このページのすべてのビューが、表示状態と同じように応答するようになりました。 また、"フォーカスされた" 状態には、 `Setter` `Entry` 入力フォーカスがあるときに、それぞれの背景をライムにするための2番目のが含まれるようになりました。

[![VSM (スタイル)](vsm-images/VsmInStyle.png "VSM (スタイル)")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="visual-states-in-no-locxamarinforms"></a>表示状態 Xamarin.Forms

次の表に、で定義されている表示状態の一覧を示し Xamarin.Forms ます。

| クラス | 状態 | 詳細情報 |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [ボタンの表示状態](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CheckBox` | `IsChecked` | [チェックボックスの表示状態](~/xamarin-forms/user-interface/checkbox.md#checkbox-visual-states) |
| `CarouselView` | `DefaultItem`, `CurrentItem`, `PreviousItem`, `NextItem` | [CarouselView の視覚的状態](~/xamarin-forms/user-interface/carouselview/interaction.md#define-visual-states) |
| `ImageButton` | `Pressed` | [ImageButton ビジュアルの状態](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `RadioButton` | `IsChecked` | [RadioButton の表示状態](~/xamarin-forms/user-interface/radiobutton.md#radiobutton-visual-states) |
| `Switch` | `On`, `Off` | [ビジュアル状態の切り替え](~/xamarin-forms/user-interface/switch.md#switch-visual-states) |
| `VisualElement` | `Normal`, `Disabled`, `Focused`, `Selected` | [一般的な状態](#common-states) |

これらの各状態には、という名前の表示状態グループを使用してアクセスでき `CommonStates` ます。

さらに、は `CollectionView` 状態を実装し `Selected` ます。 詳細については、「 [選択した項目の色を変更](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color)する」を参照してください。

## <a name="set-state-on-multiple-elements"></a>複数の要素の状態を設定する

前の例では、ビジュアルの状態が1つの要素にアタッチされ、操作されていました。 ただし、1つの要素に関連付けられているが、同じスコープ内の他の要素にプロパティを設定する表示状態を作成することもできます。 これにより、状態が動作する各要素に対して視覚的状態を繰り返す必要がなくなります。

[`Setter`](xref:Xamarin.Forms.Setter)型には、 `TargetName` 型のプロパティがあり `string` ます。これは、ビジュアル状態のが操作するターゲット要素を表し `Setter` ます。 プロパティが定義されると、 `TargetName` は `Setter` `Property` で定義されている要素のをに設定し `TargetName` `Value` ます。

```xaml
<Setter TargetName="label"
        Property="Label.TextColor"
        Value="Red" />
```

この例では、 `Label` という名前のの `label` プロパティがに設定されてい `TextColor` `Red` ます。 プロパティを設定するときは、 `TargetName` のプロパティへの完全なパスを指定する必要があり `Property` ます。 したがって、でプロパティを設定するに `TextColor` `Label` は、を `Property` として指定し `Label.TextColor` ます。

> [!NOTE]
> オブジェクトによって参照 `Setter` されるプロパティは、バインド可能なプロパティによってサポートされている必要があります。

**[VsmDemos](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルの [ **Setter with Setter TargetName** ] ページでは、1つのビジュアル状態グループから複数の要素の状態を設定する方法を示します。 XAML ファイルは、 `StackLayout` `Label` 要素、、およびを含むで構成され `Entry` `Button` ます。

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

VSM マークアップはにアタッチされ `StackLayout` ます。 "Normal" と "押された" という名前の2つの相互排他的な状態があり、各状態にはタグが含まれてい `VisualState` ます。

が押されていない場合は "Normal" 状態がアクティブになり、 `Button` 質問に対する応答を入力できます。

[![VSM Setter TargetName: 通常の状態](vsm-images/VsmSetterTargetNameNormal.png "VSM setter targetname-標準")](vsm-images/VsmSetterTargetNameNormal-Large.png#lightbox)

が押されると、"押された" 状態がアクティブになり `Button` ます。

[![VSM Setter TargetName: 押された状態](vsm-images/VsmSetterTargetNamePressed.png "VSM setter targetname-押された状態")](vsm-images/VsmSetterTargetNamePressed-Large.png#lightbox)

"押された" は、 `VisualState` が押されると、 `Button` その `Scale` プロパティが既定値の1から0.8 に変更されることを指定します。 また、という名前のでは、 `Entry` `entry` `Text` プロパティがパリに設定されます。 結果として、が押されると、 `Button` スケールが少し小さくなり、に `Entry` パリが表示されます。 その後、 `Button` が解放されると、スケールは既定値の1になり、に `Entry` 以前に入力したテキストが表示されます。

> [!IMPORTANT]
> プロパティパスは `Setter` 、現在、プロパティを指定する要素ではサポートされていません `TargetName` 。

## <a name="define-your-own-visual-states"></a>独自の視覚的状態を定義する

から派生するすべてのクラス `VisualElement` は、共通の状態 "Normal"、"フォーカスされた"、および "Disabled" をサポートしています。 また、クラスは `CollectionView` "Selected" 状態をサポートします。 内部的には、クラスは、 [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) 有効または無効になったとき、またはフォーカスまたは見るされたことを検出し、static [ `VisualStateManager.GoToState` ] (xref: を呼び出します Xamarin.Forms 。VisualStateManager GoToState ( Xamarin.Forms .VisualElement, System.string) メソッド:

```csharp
VisualStateManager.GoToState(this, "Focused");
```

これは、クラスで見つかった唯一の表示状態マネージャーコードです `VisualElement` 。 `GoToState`は、から派生したすべてのクラスに基づいてすべてのオブジェクトに対して呼び出されるので `VisualElement` 、Visual State Manager を任意のオブジェクトと共に使用して、これらの変更に応答することができ `VisualElement` ます。

興味深いことに、ビジュアル状態グループ "CommonStates" の名前はで明示的に参照されていません `VisualElement` 。 グループ名は、Visual State Manager の API の一部ではありません。 これまでに示した2つのサンプルプログラムのいずれかで、グループの名前を "CommonStates" から他のものに変更することができ、プログラムは引き続き機能します。 グループ名は、そのグループ内の状態の一般的な説明にすぎません。 すべてのグループのビジュアル状態は相互に排他的であることが暗黙的に認識されます。1つの状態であり、いつでも1つの状態のみが最新です。

独自のビジュアル状態を実装する場合は、コードからを呼び出す必要があり `VisualStateManager.GoToState` ます。 ほとんどの場合、この呼び出しはページクラスの分離コードファイルから行います。

**[VsmDemos](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルの**VSM 検証**ページでは、入力検証と共に Visual State Manager を使用する方法を示しています。 XAML ファイルは、、 `StackLayout` 、およびという2つの要素で構成され `Label` `Entry` `Button` ます。

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

VSM マークアップは、 `StackLayout` (という名前の) にアタッチされ `stackLayout` ます。 "Valid" と "Invalid" という名前の2つの相互排他的な状態があり、各状態にはタグが含まれてい `VisualState` ます。

に `Entry` 有効な電話番号が含まれていない場合、現在の状態は "無効" になります。したがって、は `Entry` ピンク色の背景を持ち、2番目のは表示され、は無効になり `Label` `Button` ます。

[![VSM 検証: 状態が無効です](vsm-images/VsmValidationInvalid.png "VSM 検証-無効")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

有効な電話番号を入力すると、現在の状態は "有効" になります。 は、 `Entry` ライムの背景を取得し、2番目のが消え、 `Label` `Button` が有効になりました。

[![VSM 検証: 有効な状態](vsm-images/VsmValidationValid.png "VSM 検証-有効")](vsm-images/VsmValidationValid-Large.png#lightbox)

分離コードファイルは、からのイベントを処理し `TextChanged` `Entry` ます。 ハンドラーは、正規表現を使用して、入力文字列が有効かどうかを判断します。 という分離コードファイル内のメソッドは、 `GoToState` の静的 `VisualStateManager.GoToState` メソッドを呼び出し `stackLayout` ます。

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

分離コードファイルでは、表示状態を定義するページ上のオブジェクトと、このオブジェクトを呼び出す必要があることに注意して `VisualStateManager.GoToState` ください。 これは、両方の表示状態がページ上の複数のオブジェクトを対象としているためです。

コードビハインドファイルが視覚的な状態を定義するページのオブジェクトを参照する必要がある場合は、分離コードファイルがこのオブジェクトと他のオブジェクトに直接アクセスできないのはなぜですか。 確かにできます。 ただし、VSM を使用する利点は、すべての UI デザインを1つの場所に保持することで、ビジュアル要素がどのような状態になるかを XAML 全体で制御できることです。 これにより、分離コードからビジュアル要素に直接アクセスすることで、視覚的な外観を設定することが回避されます。

## <a name="visual-state-triggers"></a>ビジュアル状態のトリガー

表示状態は、を適用する条件を定義する特殊なトリガーグループである状態トリガーをサポート [`VisualState`](xref:Xamarin.Forms.VisualState) します。

状態トリガーは、[`VisualState`](xref:Xamarin.Forms.VisualState) の [`StateTriggers`](xref:Xamarin.Forms.VisualState.StateTriggers) コレクションに追加されます。 このコレクションには、1 つの状態トリガーを含めることも、複数の状態トリガーを含めることもできます。 コレクション内のいずれかの状態トリガーがアクティブになっていると、[`VisualState`](xref:Xamarin.Forms.VisualState) が適用されます。

状態トリガーを使用してビジュアルの状態を制御する場合、Xamarin.Forms では、アクティブにするトリガー (および対応する [`VisualState`](xref:Xamarin.Forms.VisualState)) を決定するために、次の優先順位規則が使用されます。

1. [`StateTriggerBase`](xref:Xamarin.Forms.StateTriggerBase) から派生したトリガー。
1. [`MinWindowWidth`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowWidth) 条件の適用によってアクティブにされた [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)。
1. [`MinWindowHeight`](xref:Xamarin.Forms.AdaptiveTrigger.MinWindowHeight) 条件の適用によってアクティブにされた [`AdaptiveTrigger`](xref:Xamarin.Forms.AdaptiveTrigger)。

複数のトリガーが同時にアクティブにされた場合 (たとえば、2 つのカスタム トリガー)、マークアップで最初に宣言されたトリガーが優先されます。

状態トリガーの詳細については、「[状態トリガー](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)」を参照してください。

## <a name="use-the-visual-state-manager-for-adaptive-layout"></a>アダプティブレイアウトでのビジュアル状態マネージャーの使用

Xamarin.Formsスマートフォンで実行されるアプリケーションは、通常、縦または横の縦横比で表示でき、 Xamarin.Forms デスクトップで実行されているプログラムのサイズを変更して、さまざまなサイズや縦横比を想定することができます。 適切にデザインされたアプリケーションでは、さまざまなページまたはウィンドウのフォームファクターに応じてコンテンツが異なる方法で表示されることがあります。

この手法は、 _アダプティブレイアウト_とも呼ばれます。 アダプティブレイアウトでは、プログラムのビジュアルのみが必要であるため、ビジュアル状態マネージャーの理想的なアプリケーションです。

単純な例として、アプリケーションのコンテンツに影響を与える小さなボタンのコレクションを表示するアプリケーションがあります。 縦モードでは、これらのボタンはページ上部の水平方向の行に表示されることがあります。

[![VSM アダプティブレイアウト: 縦](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM アダプティブレイアウト-縦")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

横モードでは、ボタンの配列が一方向に移動され、列に表示されることがあります。

[![VSM アダプティブレイアウト: 横](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM アダプティブレイアウト-横")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

このプログラムは、上から下に、ユニバーサル Windows プラットフォーム、Android、および iOS で実行されています。

[VsmDemos](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)サンプルの**VSM アダプティブレイアウト**ページでは、"縦" と "横" という名前の2つの表示状態を持つ "OrientationStates" という名前のグループを定義します。 (より複雑な方法は、複数の異なるページやウィンドウの幅に基づいている場合があります)。

VSM マークアップは、XAML ファイル内の4つの場所で実行されます。 `StackLayout`という名前のには、要素である `mainStack` メニューとコンテンツの両方が含まれてい `Image` ます。 縦 `StackLayout` モードの垂直方向と横向きモードの水平方向を持つ必要があります。

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

という `ScrollView` 名前のとは、 `menuScroll` `StackLayout` `menuStack` ボタンのメニューを実装します。 これらのレイアウトの向きは、の反対です `mainStack` 。 メニューは縦モードで水平にし、横モードで垂直にする必要があります。

VSM マークアップの4番目のセクションは、ボタン自体の暗黙的なスタイルです。 このマークアップは `VerticalOptions` 、 `HorizontalOptions` `Margin` 縦と横の向きに固有の、、およびの各プロパティを設定します。

分離コードファイルは、 `BindingContext` `menuStack` コマンド実行を実装するためにのプロパティを設定 `Button` し、さらにページのイベントにハンドラーをアタッチし `SizeChanged` ます。

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

この `SizeChanged` ハンドラーは、 `VisualStateManager.GoToState` 2 つの要素および要素を呼び出し、の子をループ処理して `StackLayout` `ScrollView` `menuStack` `VisualStateManager.GoToState` 要素を呼び出し `Button` ます。

XAML ファイルの要素のプロパティを設定することによって、分離コードファイルで向きの変更をより直接的に処理できるように見えるかもしれませんが、視覚的な状態マネージャーは、明らかに構造化されたアプローチです。 すべてのビジュアルは XAML ファイルに保持され、簡単に調査、保守、および変更できます。

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visual State Manager と Xamarin 大学

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms 3.0 Visual State Manager ビデオ**

## <a name="related-links"></a>関連リンク

- [VsmDemos](/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
- [状態トリガー](~/xamarin-forms/app-fundamentals/triggers.md#state-triggers)