---
title: Xamarin.Forms Visual State Manager
description: コードから設定した visual state に基づく XAML 要素を変更するには、Visual State Manager を使用します。
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2019
ms.openlocfilehash: 11de0ecf20c6748d4958d1f1f1bea80e6a87024e
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490013"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin.Forms Visual State Manager

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_コードから設定した visual state に基づく XAML 要素を変更するには、Visual State Manager を使用します。_

Visual State Manager (VSM) は、Xamarin.Forms 3.0 の新機能です。 VSM は、コードからユーザーインターフェースに視覚的変更を加える構造化された方法を提供します。 ほとんどの場合、アプリケーションのユーザーインターフェイスは XAML で定義され、この XAML には Visual State Manager がユーザーインターフェイスの表示に影響を与える方法を記述するマークアップが含まれています。

VSM は、_visual state_ という概念を導入しています。 `Button` などの Xamarin.Forms のビューは、基になる状態 &mdash;（無効な状態か、押されているか、入力フォーカスを持つか） に応じていくつかの異なる視覚的外観を持つことができます。 これらが、ボタンの状態です。

Visual state は、_visual state groups_ にまとめられます。 すべての visual state group 内の visual state は、相互に排他的です。 visual state と visual state group の両方は、単純なテキスト文字列によって識別されます。

Xamarin.Forms Visual State Manager では、3 つの visual state を使って "CommonStates" という名前の 1 つの visual state group を定義しています。

- "Normal"
- "Disabled"
- "Focused"

この visual state group は、 [`VisualElement`](xref:Xamarin.Forms.VisualElement) から派生したすべてのクラスでサポートされています。`VisualElement` は、[`View`](xref:Xamarin.Forms.View)と[`Page`](xref:Xamarin.Forms.Page) の基本クラスです。

この記事でも説明しますが、独自の visual state group や visual state を定義することもできます。

> [!NOTE]
> [トリガー](~/xamarin-forms/app-fundamentals/triggers.md) をよく知る Xamarin.Forms 開発者は、トリガーもまたビューのプロパティの変更やイベントの発生に基づいてユーザーインターフェイスの見た目を変更をすることができることを知っています。 ただし、これらの変更のさまざまな組み合わせを処理するためにトリガーを使用すると、かなり紛らわしくなります。 歴史的に、Visual State Manager は、visual state の組み合わせに起因する混乱を軽減するために、Windows の XAML ベースの環境で導入されました。 Visual State Manager を使うと、visual state group 内の visual state は、常に相互に排他的です。 いかなるときも、グループ内の1つの状態だけが、現在の状態になります。

## <a name="the-common-states"></a>Common state

Visual State Manager を使用すると、XAML ファイルに、ビューが通常か無効か、または入力フォーカスを持っているかによってビューの外観を変更できるセクションを含めることができます。 これらは _common states_ と呼ばれます。

たとえば、ページ上に `Entry` ビューがあるとすると、`Entry` ビューの外観を次のような方法で変更したくなるでしょう。

- `Entry` 、Pink」と入力する必要がありますがあるときにバック グラウンド、`Entry`は無効です。
- `Entry` は、通常時はライムの背景を持つ。
- `Entry`は、入力フォーカスがある場合、通常の高さの 2 倍に拡大する。

個々のビューに VSM マークアップを記述することができます。また複数のビューに適用する場合は、スタイルで定義することもできます。 次の 2 つのセクションでは、これらの方法について説明します。

### <a name="vsm-markup-on-a-view"></a>ビューのマークアップを VSM

`Entry` ビューに VSM マークアップを付加するには、まず `Entry` を開始タグと終了タグに分割します。

```xaml
<Entry FontSize="18">

</Entry>
```

状態のいずれかが使用されるため、明示的なフォント サイズ与え、`FontSize`プロパティ内のテキストのサイズを 2 倍にする、`Entry`します。

次に、`VisualStateManager.VisualStateGroups` タグをこれらのタグの間に挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) は、[`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) クラスによって定義された、バインド可能な添付プロパティです。 (アタッチ可能なバインド可能なプロパティの詳細については、「[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」を参照してください)。これは、`VisualStateGroups` プロパティが `Entry` オブジェクトにアタッチされる方法です。

`VisualStateGroups` プロパティの型は[`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList) で、[`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) オブジェクトのコレクションです。 `VisualStateManager.VisualStateGroups` タグ内に、含めたい visual state の各グループの `VisualStateGroup` タグを挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

`VisualStateGroup` タグに、グループの名前を示す `x:Name` 属性があることに注目してください。 `VisualStateGroup` クラスには、`x:Name` の代わりに使用できる `Name` プロパティも定義されています。

```xaml
<VisualStateGroup Name="CommonStates">
```

`x:Name` と `Name` のどちらも使用することができますが、同じ要素に両方を使うことはできません。

`VisualStateGroup` クラスは、[`States`](xref:Xamarin.Forms.VisualStateGroup.States) という名前のプロパティが定義されていて、それは [`VisualState`](xref:Xamarin.Forms.VisualState) オブジェクトのコレクションです。 `States` は、`VisualStateGroups` の _Content プロパティ_ なので、`VisualStateGroup` タグの間に直接 `VisualState` タグを含めることができます。 (Content プロパティについては、[重要な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties) の記事で説明します)。

次の手順では、グループ内のすべての表示状態にタグのペアを追加します。 これらのタグは、`x:Name` または `Name` を使用して識別することもできます。

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

`VisualState` は、[`Setters`](xref:Xamarin.Forms.VisualState.Setters) という名前のプロパティを定義します。これは [`Setter`](xref:Xamarin.Forms.Setter) オブジェクトのコレクションです。 これらは [`Style`](xref:Xamarin.Forms.Style) オブジェクトで使用するものと同じ `Setter` オブジェクトです。

`Setters` は `VisualState` のコンテンツ プロパティでは _ない_ ので、`Setters` プロパティのための要素タグを含める必要があります。

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

これで、1 つまたは複数の `Setter` オブジェクトを `Setters` タグの各ペアの間に挿入できるようになりました。 これらは、前述した表示状態を定義する `Setter` オブジェクトです。

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

各`Setter` タグは、その状態が最新のときの特定のプロパティ値を示します。 `Setter` オブジェクトによって参照されるすべてのプロパティは、バインド可能なプロパティでバックアップする必要があります。

このようなマークアップは、 **[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルプログラムの **VSM on View** ページの基礎となっています。 このページには、3 つの `Entry` ビューが含まれていますが、2 番目の `Entry` にのみ VSM マークアップが付加されています。

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

2 つ目の `Entry` には `Trigger` コレクション の一部として `DataTrigger` が含まれることに注目してください。 これにより、この `Entry` は、3 番目の `Entry` に何かが入力されるまで無効になります。 iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) で実行されている起動時のページを次に示します。

[![ビューの VSM: 無効](vsm-images/VsmOnViewDisabled.png "ビューの VSM-無効")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

現在の表示状態が「無効」のため、iOS および Android の画面では、2 番目の `Entry` の背景はピンクになっています。 `Entry` の UWP 実装では、`Entry` が無効の場合に背景色を設定することができません。

3 つ目の `Entry` にいくつかのテキストを入力すると、2 つ目の `Entry` は「標準」の状態に切り替わり、背景が黄緑色になります。

[![ビューの VSM: 通常](vsm-images/VsmOnViewNormal.png "ビューの VSM-標準")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

2 つ目の `Entry` に触れると、入力フォーカスを取得します。 これにより、「優先」の状態に切り替わり、高さが 2 倍に拡大されます。

[![ビューの VSM: フォーカスされる](vsm-images/VsmOnViewFocused.png "ビューにフォーカスされる VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

`Entry` が入力フォーカスを取得すると、黄緑色の背景は保持されないことに注意してください。 Visual State Manager は、表示状態の間で切り替わるため、以前の状態によって設定されたプロパティはリセットされます。 状態表示は相互に排他的であることを覚えておいてください。 「標準」状態は、`Entry` が単に有効であるというだけではありません。 それは、`Entry` が有効でかつ入力フォーカスがないということを意味します。

`Entry` を「優先」の状態で黄緑色の背景のままにするには、表示状態に別の `Setter` を追加します。

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

これらの `Setter` オブジェクトを正常に機能させるには、`VisualStateGroup` に、グループ内の全ての状態の `VisualState` オブジェクトが含まれている必要があります。 `Setter` オブジェクトが含まれていない表示状態がある場合は、空のタグとして含めます。

```xaml
<VisualState x:Name="Normal" />
```

### <a name="visual-state-manager-markup-in-a-style"></a>Style での Visual State Manager マークアップ

2 つ以上のビューの間では、同じ Visual State Manager マークアップを共有することがしばしば必要になります。 このような場合、`Style` 定義にマークアップを配置することができます。

以下に、**VSM On View** ページ内にある `Entry` 要素のための既存の暗黙的な `Style` を示します。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style>
```

`VisualStateManager.VisualStateGroups` 添付プロパティのための `Setter` タグを追加します。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style>
```

`Setter` のコンテンツ プロパティは `Value` なので、`Value` プロパティの値は、このタグ内に直接指定できます。 このプロパティの型は、`VisualStateGroupList` です。

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

これらのタグに 1 つ以上の `VisualStateGroup` オブジェクトを含めることができます。

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

ここでは、**VSM in Style** ページの完全な VSM マークアップを示します。

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

現在このページのすべての `Entry` ビューは、それぞれの表示状態に対して同じ反応をします。 「優先」状態には、各 `Entry` が入力フォーカスを持っている時、黄緑色の背景に変わる 2 つ目の `Setter` が含まれていることにも注意してください。

[![VSM (スタイル)](vsm-images/VsmInStyle.png "VSM (スタイル)")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="visual-states-in-xamarinforms"></a>Xamarin. Forms の表示状態

次の表に、Xamarin で定義されている表示状態の一覧を示します。

| &lt;クラス&gt; のすべてのオブジェクト | States | その他の情報 |
| ----- | ------ | ---------------- |
| `Button` | `Pressed` | [ボタンの表示状態](~/xamarin-forms/user-interface/button.md#button-visual-states) |
| `CollectionView` | `Selected` | [選択した項目の色の変更](~/xamarin-forms/user-interface/collectionview/selection.md#change-selected-item-color) |
| `ImageButton` | `Pressed` | [ImageButton ビジュアルの状態](~/xamarin-forms/user-interface/imagebutton.md#imagebutton-visual-states) |
| `VisualElement` | `Normal`では、 `Disabled`では、 `Focused` | [共通の状態](#the-common-states) |

これらの各状態には、`CommonStates`という名前の表示状態グループを使用してアクセスできます。

## <a name="defining-your-own-visual-states"></a>独自の表示状態を定義する

`VisualElement` から派生したすべてのクラスは、3 つの一般的な状態「標準」、「優先」、「無効」をサポートしています。 内部的には、[`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) クラスは、いつ有効/無効または優先/非優先になるかを検出し、静的な[`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String)) メソッドを呼び出します。

```csharp
VisualStateManager.GoToState(this, "Focused");
```

`VisualElement` クラス内で見つけることのできる Visual State Manager のコードはこれだけです。 `GoToState` は、`VisualElement` から派生するすべてのクラスに基づくすべてのオブジェクトのために呼び出されるため、どの `VisualElement` オブジェクトからも Visual State Manager を使用して、変更に対応することができます。

興味深いことに、"CommonStates" という表示状態グループの名前は、`VisualElement` 内で明示的に参照されていません。 グループ名は、Visual State Manager 用の API の一部ではありません。 これまでに示した 2 つのサンプルプログラムの 1 つでは、グループ名を "CommonStates" から別の名前に変更することができ、そのプログラムは引き続き動作します。 グループ名は、単にそのグループ内の状態の一般的な説明にすぎません。 どのグループの表示状態も相互に排他的であるということが暗黙的に認識されます。いつでもただ 1 つの状態だけが現在の状態になります。

独自の表示状態を実装する場合は、コードから `VisualStateManager.GoToState` を呼び出す必要があります。 ほとんどの場合、ページクラスの分離コードファイルからこのメソッドを呼ぶことになるでしょう。

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルの **VSM Validation** ページは、入力検証に関連する Visual State Manager の使用方法を示します。 以下の XAML ファイルには、`Entry` と `Button` の 2 つの`Label` 要素が含まれています。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="VsmDemos.VsmValidationPage"
             Title="VSM Validation">
    <StackLayout Padding="10, 10">

        <Label Text="Enter a U.S. phone number:"
               FontSize="Large" />

        <Entry Placeholder="555-555-5555"
               FontSize="Large"
               Margin="30, 0, 0, 0"
               TextChanged="OnTextChanged" />

        <Label x:Name="helpLabel"
               Text="Phone number must be of the form 555-555-5555, and not begin with a 0 or 1">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">

                    <VisualState Name="Valid">
                        <VisualState.Setters>
                            <Setter Property="TextColor" Value="Transparent" />
                        </VisualState.Setters>
                    </VisualState>

                    <VisualState Name="Invalid" />

                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Label>

        <Button x:Name="submitButton"
                Text="Submit"
                FontSize="Large"
                Margin="0, 20"
                VerticalOptions="Center"
                HorizontalOptions="Center">
            <VisualStateManager.VisualStateGroups>
                <VisualStateGroup Name="ValidityStates">

                    <VisualState Name="Valid" />

                    <VisualState Name="Invalid">
                        <VisualState.Setters>
                            <Setter Property="IsEnabled" Value="False" />
                        </VisualState.Setters>
                    </VisualState>

                </VisualStateGroup>
            </VisualStateManager.VisualStateGroups>
        </Button>
    </StackLayout>
</ContentPage>
```

VSM マークアップは、2 番目の (`helpLabel` という名前の) `Label` と (`submitButton` という名前の) `Button` に添付されています。 「有効」と「無効」という名前の 2 つの相互に排他的な状態があります。 これら 2 つの "ValidationState" グループのそれぞれで、「有効」と「無効」の両方に `VisualState` タグが含まれていますが、それぞれのケースでそのうちの 1 つは空になっていることに注意してください。

`Entry` に有効な電話番号が含まれない場合、現在の状態は「無効」になるので、 2 番目の `Label` が表示され、`Button` は無効になります。

[![VSM 検証: 状態が無効です](vsm-images/VsmValidationInvalid.png "VSM 検証-無効")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

有効な電話番号を入力すると、現在の状態は「有効」になります。 2 番目の `Entry` は消えて、`Button` が有効になります。

[![VSM 検証: 有効な状態](vsm-images/VsmValidationValid.png "VSM 検証-有効")](vsm-images/VsmValidationValid-Large.png#lightbox)

分離コード ファイルは、役割の処理を担います、`TextChanged`からイベントを`Entry`します。 ハンドラーは、入力文字列が有効かどうかを判断するのに正規表現を使用します。 という名前の分離コード ファイル内のメソッド`GoToState`呼び出す静的`VisualStateManager.GoToState`両方のメソッド`helpLabel`と`submitButton`:

```csharp
public partial class VsmValidationPage : ContentPage
{
    public VsmValidationPage ()
    {
        InitializeComponent ();

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
        VisualStateManager.GoToState(helpLabel, visualState);
        VisualStateManager.GoToState(submitButton, visualState);
    }
}
```

また、`GoToState`メソッドは、状態を初期化するコンス トラクターから呼び出されます。 現在の状態が常にする必要があります。 それに遠く及ばず、コードでがあります、表示状態グループの名前への参照わかりやすくするための目的で"ValidationStates"として、XAML で参照されています。

呼び出すと、これらの表示状態で、分離コード ファイルに影響を受けるページ上のすべてのオブジェクトのアカウントを実行する必要がありますに注意してください。`VisualStateManager.GoToState`のこれらの各オブジェクト。 この例では 2 つのオブジェクト (、 `Label` 、 `Button`)、いくつかが考えられますが、詳細。

疑問に思うかもしれません。 場合は、分離コード ファイルには、これらの表示状態の影響を受けるページのすべてのオブジェクトを参照する必要があります、分離コード ファイルだけにアクセスできない理由オブジェクト直接でしょうか。 これは間違いでした。 ただし、VSM を使用する利点は、どの視覚的要素を制御できることだけで、XAML UI 設計のすべてを 1 つの場所に保持する別の状態に対応します。 視覚的な外観の設定を視覚的要素を分離コードから直接アクセスすることによって回避できます。

クラスを派生することが検討される可能性があります`Entry`とおそらく外部検証関数を設定できるプロパティを定義します。 派生したクラス`Entry`を呼び出して、`VisualStateManager.GoToState`メソッド。 このスキームは場合にのみが正常に機能は、`Entry`されたさまざまな視覚的な状態の影響を受ける唯一のオブジェクト。 この例で、`Label`と`Button`も影響します。 VSM マークアップに接続されているために、方法はありません、 `Entry` VSM マークアップでは、ビジュアルの状態に変更を別のオブジェクトから参照するには、その他のオブジェクトをこれらに接続 ページと方法がないその他のオブジェクトを制御します。

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>アダプティブレイアウトのための Visual State Manager の使用

スマート フォンで実行されているアプリケーションは、縦または横の縦横比とデスクトップで実行されている Xamarin.Forms プログラムに通常表示できます Xamarin.Forms と多くのさまざまなサイズと縦横比を想定するサイズを変更できます。 適切に設計されたアプリケーションがこれらのさまざまなページまたはウィンドウ フォーム ファクターの異なる方法では、そのコンテンツを表示します。

この手法とも呼ばれる_アダプティブ レイアウト_します。 アダプティブ レイアウトには、プログラムのビジュアルのみが含まれる、ため、Visual State Manager の理想的なアプリケーションになります。

簡単な例は、アプリケーションのコンテンツに影響するボタンの小規模なコレクションを表示するアプリケーションです。 縦モードの場合でこれらのボタンをページ上部にある水平方向の行で表示される可能性があります。

[![VSM アダプティブレイアウト: 縦](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM アダプティブレイアウト-縦")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

横モードでボタンの配列が 1 つの側に移動され、列に表示。

[![VSM アダプティブレイアウト: 横](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM アダプティブレイアウト-横")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

上から下に、プログラムはユニバーサル Windows プラットフォーム、Android、iOS で実行されています。

**VsmDemos** サンプルの [VSM Adaptive Layout](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos) ページでは、「縦」と「横」という名前の 2 つの表示状態を持った "OrientationStates" という名前のグループが定義されています。 (より複雑なアプローチでは、様々な異なるページやウィンドウの幅に基づくことがあります。)

VSM マークアップは、XAML ファイルの 4 箇所 で発生します。 `StackLayout` という名前の `mainStack` は、`Image` 要素であるメニューとコンテンツの両方を含みます。 この `StackLayout` は、縦向きモード時に垂直方向で、横向きモードに水平方向である必要があります。

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

`ScrollView` という名前の内側の `menuScroll` と `StackLayout` という名前の `menuStack` は、ボタンのメニューを実装しています。 これらのレイアウトの方向は、`mainStack` とは反対になっています。 メニューは縦向きモードでは水平に、横向きモードでは垂直である必要があります。

VSM マークアップの 4 番目のセクションは、ボタン自体の暗黙的なスタイルが使用されています。 このマークアップは、縦向きと横向きに固有の `VerticalOptions`、`HorizontalOptions`、および `Margin` プロパティを設定します。

分離コードファイルでは、`BindingContext` の `menuStack` プロパティを `Button` のコマンドを実装するように設定し、ハンドラーをページの `SizeChanged` イベントも付加します。

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

`SizeChanged` ハンドラーは、2 つの `StackLayout` と `ScrollView` 要素に対して `VisualStateManager.GoToState` を呼び、その後 `menuStack` の子をループして `Button` 要素に対して `VisualStateManager.GoToState` を呼び出します。

分離コードファイルで、XAML ファイルの要素のプロパティを設定すれば、より直接的に向きの変更を処理できるかのように思われるかもしれませんが、Visual State Manager は間違いなくより構造化されたアプローチです。 全てのビジュアルは XAML ファイル内で保持されるため、調査、メンテナンス、変更がしやすくなります。

## <a name="visual-state-manager-with-xamarinuniversity"></a>Xamarin.University で visual State Manager

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin. Forms 3.0 Visual State Manager ビデオ**

## <a name="related-links"></a>関連リンク

- [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
