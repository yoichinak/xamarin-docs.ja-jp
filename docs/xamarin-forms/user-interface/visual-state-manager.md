---
title: Xamarin.Forms Visual State Manager
description: コードから設定した visual state に基づく XAML 要素を変更するには、Visual State Manager を使用します。
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 0fdcbd6467547647089b436a894b1bc490ba5ee1
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37854819"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin.Forms Visual State Manager

_コードから設定した visual state に基づく XAML 要素を変更するには、Visual State Manager を使用します。_

Visual State Manager (VSM) は、Xamarin.Forms 3.0 の新機能です。 VSM は、コードからユーザーインターフェースに視覚的変更を加える構造化された方法を提供します。 ほとんどの場合、アプリケーションのユーザーインターフェイスは XAML で定義され、この XAML には Visual State Manager がユーザーインターフェイスの表示に影響を与える方法を記述するマークアップが含まれています。

VSM は、_visual state_ という概念を導入しています。`Button` などの Xamarin.Forms のビューは、基になる状態 （無効な状態か、押されているか、入力フォーカスを持つか） に応じていくつかの異なる視覚的外観を持つことができます。これらが、ボタンの状態です。

Visual state は、_visual state groups_ にまとめられます。すべての visual state group 内の visual state は、相互に排他的です。visual state と visual state group の両方は、単純なテキスト文字列によって識別されます。

Xamarin.Forms Visual State Manager では、3 つの visual state を使って "CommonStates" という名前の 1 つの visual state group を定義しています。

- "Normal"
- "Disabled"
- "Focused"

この visual state group は、 [`VisualElement`](xref:Xamarin.Forms.VisualElement) から派生したすべてのクラスでサポートされています。`VisualElement` は、[ `View` ](xref:Xamarin.Forms.View)と[ `Page`](xref:Xamarin.Forms.Page) の基本クラスです。 

この記事でも説明しますが、独自の visual state group を定義することもできます。

> [!NOTE]
> [トリガー](~/xamarin-forms/app-fundamentals/triggers.md) をよく知る Xamarin.Forms 開発者は、トリガーもまたビューのプロパティの変更やイベントの発生に基づいてユーザーインターフェイスの見た目を変更をすることができることを知っています。 ただし、これらの変更のさまざまな組み合わせを処理するためにトリガーを使用すると、かなり紛らわしくなります。歴史的に、Visual State Manager は、visual state の組み合わせに起因する混乱を軽減するために、Windows の XAML ベースの環境で導入されました。 Visual State Manager を使うと、visual state group 内の visual state は、常に相互に排他的です。いかなるときも、グループ内の1つの状態だけが、現在の状態になります。

## <a name="the-common-states"></a>Common state

Visual State Manager を使用すると、XAML ファイルに、ビューが通常か無効か、または入力フォーカスを持っているかによってビューの外観を変更できるセクションを含めることができます。 これらは _common states_ と呼ばれます。

たとえば、ページ上に `Entry` ビューがあるとすると、`Entry` ビューの外観を次のような方法で変更したくなるでしょう。

- `Entry` は、無効の場合、ピンクの背景を持つ。
- `Entry` は、通常時はライムの背景を持つ。
- `Entry` は、入力フォーカスがある場合、通常の高さの 2 倍に拡大する。

個々のビューに VSM マークアップを記述することができます。また複数のビューに適用する場合は、スタイルで定義することもできます。 次の 2 つのセクションでは、これらの方法について説明します。

### <a name="vsm-markup-on-a-view"></a>ビューのマークアップを VSM

`Entry` ビューに VSM マークアップを付加するには、まず `Entry` を開始タグと終了タグに分割します。

```xaml
<Entry FontSize="18">

</Entry>
```

状態の 1 つが、`Entry` のテキストサイズを2倍にするために、`FontSize` プロパティを使用するため、明示的にフォントサイズを与えています。

次に、`VisualStateManager.VisualStateGroups` タグをこれらのタグの間に挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) は、[ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager) クラスによって定義された、バインド可能な添付プロパティです。 (バインド可能な添付プロパティの詳細については、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md) の記事を参照してください)。このようにして、`Entry` オブジェクトに `VisualStateGroups` プロパティを添付します。

`VisualStateGroups` プロパティの型は[ `VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList) で、[ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) オブジェクトのコレクションです。 `VisualStateManager.VisualStateGroups` タグ内に、含めたい visual state の各グループの `VisualStateGroup` タグを挿入します。

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

`VisualStateGroup` クラスは、[ `States`](xref:Xamarin.Forms.VisualStateGroup.States) という名前のプロパティが定義されていて、それは [ `VisualState` ](xref:Xamarin.Forms.VisualState) オブジェクトのコレクションです。`States` は、`VisualStateGroups` の _Content プロパティ_ なので、`VisualStateGroup` タグの間に直接 `VisualState` タグを含めることができます。(Content プロパティについては、[重要な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties) の記事で説明します)。

次の手順では、グループにすべての visual state のタグのペアを追加します。これらのタグにも `x:Name` または `Name` を使用することができます。:

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

`VisualState` は、[ `Setters`](xref:Xamarin.Forms.VisualState.Setters) という名前のプロパティが定義されています。これは [ `Setter` ](xref:Xamarin.Forms.Setter) オブジェクトのコレクションです。 これらは [ `Style` ](xref:Xamarin.Forms.Style) オブジェクトで使用するものと同じ `Setter` オブジェクトです。

`Setters` は `VisualState` の Content プロパティでは _ない_ ので、`Setters` プロパティのための要素タグを含める必要があります。

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

これで、1 つまたは複数の `Setter` オブジェクトを `Setters` タグの各ペアの間に挿入できるようになりました。これらは、前述した visual state を 定義する `Setter` オブジェクトです。

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

各 `Setter` タグは、その状態のときの特定のプロパティの値を示します。 `Setter` オブジェクトによって参照されるすべてのプロパティは、バインド可能なプロパティでなければなりません。

このようなマークアップは、**[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** サンプルプログラムの **VSM on View** ページの基礎となっています。このページは、3 つの `Entry` ビューが含まれていますが、2 番目の `Entry` だけがそれに付加された VSM マークアップを持っています。

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

2 つ目の `Entry` は `Trigger` コレクション の一部として `DataTrigger` も持っていることに注意してください。これにより、この `Entry` は、3 番目の `Entry` に何かが入力されるまで無効になります。iOS、Android、および Universal Windows Platform (UWP) で実行されている起動時のページを次に示します。

[![VSM on View: disabled](vsm-images/VsmOnViewDisabled.png "VSM on View - disabled")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)


現在の visual state が "Disabled" のため、iOS および Android の画面では、2 番目の `Entry` の背景はピンクになっています。`Entry` の UWP 実装では、`Entry` が無効の場合の背景色を設定することができません。 

3 つ目の `Entry` にいくつかのテキストを入力すると、2 つ目の `Entry` は "Normal" 状態に切り替わり、背景がライムになります。

[![VSM on View: normal](vsm-images/VsmOnViewNormal.png "VSM on View - normal")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

2 つ目の `Entry` に触れると、入力フォーカスを取得します。これにより、"Focused" 状態に切り替わり、高さが 2 倍に拡大されます。

[![VSM on View: Focused](vsm-images/VsmOnViewFocused.png "VSM on view - focused")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

`Entry` が、入力フォーカスを取得したとき、ライムの背景は保持されないことに注意してください。Visual State Manager は、visual state の間で切り替えるため、以前の状態によって設定されたプロパティはリセットされます。 Visual state は相互に排他的であることを覚えておいてください。"Normal" 状態は、ただ `Entry` が有効であるというだけの意味ではありません。それは、`Entry` が有効でかつ入力フォーカスを持っていないということを意味します。 

`Entry` が "Focused" 状態の時にライムの背景を持たせたい場合は、visual state に別の `Setter` を追加します。

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

これらの `Setter` オブジェクトを正常に機能させるには、`VisualStateGroup` は、グループ内の全ての状態の `VisualState` オブジェクトを含める必要があります。`Setter` オブジェクトを 1 つも持たない visual state がある場合は、空のタグとして含めます。

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>Style での Visual State Manager マークアップ

2 つ以上のビューの間で同じ Visual State Manager マークアップを共有することがしばしば必要になります。 このような場合、`Style` 定義にマークアップしたいと思うでしょう。

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

`Setter` の Content プロパティは `Value` なので、`Value` プロパティの値は、このタグ内に直接指定できます。このプロパティの型は、`VisualStateGroupList` です。


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

今、このページのすべての `Entry` ビューは、それぞれの visual state に対して同じ反応をします。 "Focused" 状態には、各 `Entry` が入力フォーカスを持つ時、ライムの背景に変わるという 2 つ目の `Setter` が含まれていることにも注意してください。

[![VSM in Style](vsm-images/VsmInStyle.png "VSM in style")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>独自の Visual State を定義する

`VisualElement` から派生したすべてのクラスは、3 つの common state "Normal"、"Focused"、"Disabled" をサポートしています。内部的には、 [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs) クラスは、いつ有効・無効またはフォーカス・非フォーカスになるかを検出し、静的な [ `VisualStateManager.GoToState` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualStateManager.GoToState/p/Xamarin.Forms.VisualElement/System.String/) メソッドを呼び出します。

```csharp
VisualStateManager.GoToState(this, "Focused");
```

これだけが、`VisualElement` クラス内で見つけられる Visual State Manager のコードです。 `GoToState` は、`VisualElement` から派生するすべてのクラスに基づくすべてのオブジェクトのために呼び出されるため、どの `VisualElement` オブジェクトからも Visual State Manager を使用して、変更に対応することができます。

興味深いことに、"CommonStates" という visual state group の名前は、`VisualElement` 内で明示的に参照されていません。グループ名は、Visual State Manager 用の API の一部ではありません。これまでに示した 2 つのサンプルプログラムのひとつは、グループ名を "CommonStates" から別の名前に変更することができ、そのプログラムは引き続き動作します。グループ名は、単にそのグループ内の状態の一般的な説明にすぎません。どのグループの visual state も 相互に排他的であるということが暗黙的に認識されます。いつでもただ1つの状態だけが現在の状態になります。

独自の表示状態を実装する場合は、コードから `VisualStateManager.GoToState` を呼び出す必要があります。ほとんどの場合、ページクラスの分離コードファイルからこのメソッドを呼ぶことになるでしょう。

**[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** サンプルの **VSM Validation** ページは、入力検証に関連する Visual State Manager の使用方法を示します。以下のXAML ファイルには、2 つの`Label` 要素と `Entry` と `Button` が含まれています。

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

VSM マークアップは、2 番目の (`helpLabel` という名前の) `Label` と (`submitButton` という名前の) `Button` に添付されています。"Valid" と "Invalid" という名前の 2 つの相互に排他的な状態があります。 これらの 2 つの "ValidationState" グループのそれぞれに、"Valid" と "Invalid" の両方の `VisualState` タグが含まれていますが、それらの内の各1つは空になっていることに注意してください。

`Entry` に有効な電話番号が含まれない場合、現在の状態は "Invalid" になるので、 2 番目の `Label` が表示され、`Button` は無効になります。

[![[VSM Validation: Invalid State](vsm-images/VsmValidationInvalid.png "VSM validation - invalid")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

有効な電話番号を入力すると、現在の状態は "Valid" になります。2 番目の `Entry` は消えて、`Button`が有効になります。

[![VSM Validation: Valid State](vsm-images/VsmValidationValid.png "VSM validation - valid")](vsm-images/VsmValidationValid-Large.png#lightbox)

分離コードファイルは、`Entry` からの `TextChanged` イベントを処理する責任があります。そのハンドラーは正規表現を使用して、入力文字列が有効かどうかを判定します。 `GoToState` という名前の分離コードファイル内のメソッドは、`helpLabel` と `submitButton` の両方に対して静的な `VisualStateManager.GoToState` メソッドを呼び出します。

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

`GoToState` メソッドは、状態を初期化するためにコンストラクタから呼び出されていることにも注目してください。常に現在の状態は1つである必要があります。コードのどこにも、 visual state group の名前への参照がありませんが、それは明確にするために "ValidationStates" として XAML 内で参照されています。

分離コードファイルは、これらの visual state の影響を受けるページ上のすべてのオブジェクトを考慮して、これらのオブジェクトそれぞれに `VisualStateManager.GoToState` を呼ぶ必要があることに注意してください。この例では 2 つのオブジェクト (`Label` と `Button`) だけですが、これよりも多くなる可能性があります。

分離コードファイルが、これらの visual state の影響を受けるページ上のすべてのオブジェクトを参照しなければならないならば、なぜ分離コードファイルは直接オブジェクトに簡単にアクセスできないのか？と思うかもしれません。そう思うのは当然です。しかしながら、VSM を使用する利点は、XAML で完全に視覚要素が異なる状態に対応する方法を制御できることで、それは 1つの場所で UI デザイン の全てを保持できるということです。これは、分離コードから直接視覚要素へアクセスして視覚的外観を設定することを回避します。

`Entry` からクラスを派生させて、外部のバリデーション関数を設定できるプロパティを定義することを検討することがあるかもしれません。`Entry` から派生したクラスは、`VisualStateManager.GoToState` メソッドを呼び出すことができます。この計画は問題なく動作しますが、`Entry` が異なる visual state によって影響を受ける唯一のオブジェクトである場合に限ります。この例では、`Label` と `Button` も影響を受けます。ページ上の他のオブジェクトを制御するための `Entry` に付加する VSM マークアップ方法はありません。またこれらの他のオブジェクトに VSM マークアップを付加して、別のオブジェクトからの visual state の変更を参照する方法も存在しません。

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>アダプティブレイアウトのための Visual State Manager の使用

スマートフォンで実行される Xamarin.Forms アプリケーションは、通常、縦または横の縦横比で表示でき、デスクトップで実行される Xamarin.Forms プログラムは、リサイズして多くの異なるサイズと縦横比に対応することができます。 適切に設計されたアプリケーションは、これらのさまざまなページや Window Form の要因に対して、その内容を異なる形で表示することがあります。

この技術は、_アダプティブ レイアウト_ としても知られています。アダプティブ レイアウトは、プログラムのビジュアルにのみ影響するため、Visual State Manager の理想的なアプリケーションです。

シンプルな例として、アプリケーションのコンテンツに影響する小さなボタンのコレクションを表示するアプリケーションがあります。 縦向きモードの場合は、これらのボタンは、ページの上部に水平方向の行で表示されます。

[![VSM アダプティブレイアウト : 縦](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM アダプティブレイアウト - 縦")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

横向きモードでは、ボタンの配列は端に移動し、列で表示されます。

[![VSM アダプティブレイアウト : 横](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM アダプティブ レイアウト - 横")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

上から下に、Univarsal Windows Platform、Android、iOS のプログラムが実行されています。

[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/) サンプルの **VSM Adaptive Layout** ページには、 "Portrait" と "Landscape" という名前の 2  つの visual state を持った "OrientationStates" という名前のグループが定義されています。 (より複雑なアプローチでは、様々な異なるページやウィンドウの幅に基づくことがあります)

VSM マークアップは、XAML ファイルの 4 箇所 で発生します。`mainStack` という名前の `StackLayout` は、メニューとコンテンツの両方を含み、コンテンツは 1 つの `Image` 要素です。この `StackLayout` は、縦向きモード時に垂直方向で、横向きモードに水平方向である必要があります。

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

`menuScroll` という名前の内側の `ScrollView` と `menuStack` という名前の `StackLayout` は、ボタンのメニューを実装しています。これらのレイアウトの方向は、`mainStack` とは反対になっています。メニューは縦向きモードでは水平に、横向きモードでは垂直である必要があります。

VSM マークアップの 4 番目のセクションは、自身のボタンに対しての暗黙的なスタイルの中にあります。このマークアップは、縦向きと横向きに固有の `VerticalOptions`、 `HorizontalOptions`、および`Margin` プロパティを設定しています。

分離コードファイルでは、`menuStack` の `BindingContext` プロパティに `Button` のコマンド実装を設定し、またハンドラーにページの `SizeChanged` イベントを付加しています。

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

`SizeChanged` ハンドラーは、2 つの `StackLayout` と `ScrollView` 要素に対して`VisualStateManager.GoToState` を呼び、その後 `menuStack` の子をループして `Button` 要素に対して `VisualStateManager.GoToState` を呼び出します。

分離コードファイルで、XAML ファイルの要素のプロパティを設定すれば、より直接的に向きの変更を処理できるかのように思われるかもしれませんが、Visual State Manager は間違いなくより構造化されたアプローチです。 全てのビジュアルは XAML ファイル内で保持されるため、調査・メンテナンス・変更がしやすくなります。


## <a name="visual-state-manager-with-xamarinuniversity"></a>Xamarin.University で visual State Manager

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**[Xamarin University](https://university.xamarin.com/) で Xamarin.Forms 3.0 Visual State Manager**

## <a name="related-links"></a>関連リンク

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

