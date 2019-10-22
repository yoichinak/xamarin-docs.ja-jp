---
title: Xamarin. Forms Visual State Manager
description: Visual State Manager を使用して、コードから設定されたビジュアルの状態に基づいて XAML 要素を変更します。
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/07/2018
ms.openlocfilehash: 228501172ede71204c64e1efe1673ce92be424ea
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "68656057"
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin. Forms Visual State Manager

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)

_Visual State Manager を使用して、コードから設定されたビジュアルの状態に基づいて XAML 要素を変更します。_

Visual State Manager (VSM) は、Xamarin. Forms 3.0 で新しく追加されたものです。 VSM は、コードからユーザーインターフェイスを視覚的に変更できるように構造化された方法を提供します。 ほとんどの場合、アプリケーションのユーザーインターフェイスは XAML で定義され、この XAML には、Visual State Manager がユーザーインターフェイスのビジュアルにどのように影響するかを説明するマークアップが含まれます。

VSM には、視覚的な_状態_の概念が導入されています。 @No__t_0 などの Xamarin 形式のビューでは、基になる状態に応じて、視覚化が無効になっているか、押されているか、または入力フォーカスがあるか &mdash; に応じて、さまざまな外観を表示できます。 これは、ボタンの状態です。

ビジュアル状態は、表示_状態グループ_で収集されます。 ビジュアル状態グループ内のすべての表示状態は、相互に排他的です。 視覚的な状態と表示状態の両方のグループは、単純なテキスト文字列によって識別されます。

Xamarin 形式の Visual State Manager は、"CommonStates" という名前の1つの表示状態グループを定義します。表示状態は次の3つです。

- "Normal"
- 無効に
- フォーカス

この表示状態グループは、 [`VisualElement`](xref:Xamarin.Forms.VisualElement)から派生したすべてのクラスでサポートされています。これは[`View`](xref:Xamarin.Forms.View)と[`Page`](xref:Xamarin.Forms.Page)の基本クラスです。 

この記事で説明するように、独自のビジュアル状態グループと視覚的な状態を定義することもできます。

> [!NOTE]
> Xamarin。[トリガー](~/xamarin-forms/app-fundamentals/triggers.md)を使い慣れている場合は、ビューのプロパティの変更またはイベントの発生に基づいて、トリガーによってユーザーインターフェイスのビジュアルが変更される可能性があることに注意してください。 ただし、これらの変更のさまざまな組み合わせに対応するためにトリガーを使用すると、大幅に混乱する可能性があります。 従来、visual State Manager は、視覚的な状態の組み合わせによって生じる混乱を軽減するために、Windows XAML ベースの環境で導入されました。 VSM では、表示状態グループ内のビジュアル状態は常に相互に排他的です。 各グループの状態は、いつでも現在の状態になります。

## <a name="the-common-states"></a>共通の状態

表示状態マネージャーを使用すると、XAML ファイルにセクションを追加して、ビューが通常、無効、または入力フォーカスを持っている場合にビューの外観を変更することができます。 これらは_共通の状態_と呼ばれます。

たとえば、ページに `Entry` ビューがあり、`Entry` の外観を次のように変更したいとします。

- @No__t_1 が無効になっている場合、`Entry` にはピンク色の背景が必要です。
- @No__t_0 には、通常はライムの背景が必要です。
- 入力フォーカスがある場合、`Entry` は通常の高さの2倍になるように拡張する必要があります。

個々のビューに VSM マークアップをアタッチすることも、複数のビューに適用する場合はスタイルで定義することもできます。 次の2つのセクションでは、これらの方法について説明します。

### <a name="vsm-markup-on-a-view"></a>ビューの VSM マークアップ

VSM マークアップを `Entry` ビューにアタッチするには、最初に `Entry` を開始タグと終了タグに分割します。

```xaml
<Entry FontSize="18">

</Entry>
```

状態の1つは、`FontSize` プロパティを使用して `Entry` 内のテキストのサイズを2倍にするため、明示的なフォントサイズが指定されています。

次に、タグの間に `VisualStateManager.VisualStateGroups` タグを挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty)は、 [`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager)クラスによって定義された、アタッチ可能なバインド可能プロパティです。 (アタッチ可能なバインド可能なプロパティの詳細については、「[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md)」を参照してください)。これは、`VisualStateGroups` プロパティが `Entry` オブジェクトにアタッチされる方法です。

@No__t_0 プロパティは、 [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup)オブジェクトのコレクションである[`VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList)型です。 @No__t_0 タグ内に、含めるビジュアル状態のグループごとに、`VisualStateGroup` タグのペアを挿入します。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

@No__t_0 タグには、グループの名前を示す `x:Name` 属性があることに注意してください。 @No__t_0 クラスは、代わりに使用できる `Name` プロパティを定義します。

```xaml
<VisualStateGroup Name="CommonStates">
```

@No__t_0 または `Name` のいずれかを使用できますが、両方を同じ要素内で使用することはできません。

@No__t_0 クラスは、 [`VisualState`](xref:Xamarin.Forms.VisualState)オブジェクトのコレクションである[`States`](xref:Xamarin.Forms.VisualStateGroup.States)という名前のプロパティを定義します。 `States` は `VisualStateGroups` の_content プロパティ_であるため、`VisualStateGroup` タグの間に `VisualState` タグを直接含めることができます。 (コンテンツプロパティについては、「[基本的な XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties)」で説明しています)。

次の手順では、そのグループ内のすべてのビジュアル状態のタグのペアを含めます。 これらは、`x:Name` または `Name` を使用して識別することもできます。

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

`Setters` は `VisualState` の content プロパティでは_ない_ため、`Setters` プロパティの property 要素タグを含める必要があります。

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

@No__t_1 タグの各ペアの間に1つ以上の `Setter` オブジェクトを挿入できるようになりました。 前に説明した表示状態を定義する `Setter` オブジェクトを次に示します。

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

各 `Setter` タグは、その状態が current であるときの特定のプロパティの値を示します。 @No__t_0 オブジェクトによって参照されるプロパティは、バインド可能なプロパティによってサポートされている必要があります。

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

2番目の `Entry` にも、`Trigger` コレクションの一部として `DataTrigger` が含まれていることに注意してください。 これにより、3つ目の `Entry` に何かが入力されるまで、`Entry` が無効になります。 IOS、Android、ユニバーサル Windows プラットフォーム (UWP) で実行される起動時のページを次に示します。

[![ビューの VSM: 無効](vsm-images/VsmOnViewDisabled.png "ビューの VSM-無効")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

現在の表示状態は "Disabled" であるため、2番目の `Entry` の背景は、iOS と Android の画面でピンク色になります。 @No__t_0 の UWP 実装では、`Entry` が無効になっている場合に背景色を設定することはできません。 

3番目の `Entry` にテキストを入力すると、2番目の `Entry` は "通常" の状態に切り替わり、背景は "ライム" になります。

[![ビューの VSM: 通常](vsm-images/VsmOnViewNormal.png "ビューの VSM-標準")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

2番目の `Entry` をタッチすると、入力フォーカスが取得されます。 "フォーカスされた" 状態に切り替わり、2倍の高さに拡張されます。

[![ビューの VSM: フォーカスされる](vsm-images/VsmOnViewFocused.png "ビューにフォーカスされる VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

@No__t_0 は、入力フォーカスを取得するときに、ライムの背景を保持しないことに注意してください。 ビジュアル状態マネージャーは、表示状態を切り替えるときに、以前の状態に設定されたプロパティの設定が解除されます。 視覚的な状態は相互に排他的であることに注意してください。 "Normal" 状態は、`Entry` が有効になっていることだけを意味するわけではありません。 @No__t_0 が有効になっていて、入力フォーカスがないことを意味します。 

@No__t_0 が "フォーカスされた" 状態でライムの背景を持つようにするには、そのビジュアル状態に別の `Setter` を追加します。

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

これらの `Setter` オブジェクトが正常に機能するためには、`VisualStateGroup` に、そのグループ内のすべての状態の `VisualState` オブジェクトが含まれている必要があります。 @No__t_0 オブジェクトのない視覚的な状態がある場合は、空のタグとして追加します。

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>スタイルのビジュアル状態マネージャーマークアップ

多くの場合、2つ以上のビューで同じ Visual State Manager マークアップを共有する必要があります。 この場合は、`Style` 定義にマークアップを配置する必要があります。

次に、 **VSM On ビュー**ページの `Entry` 要素の既存の暗黙的な `Style` を示します。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

@No__t_1 アタッチされているバインド可能なプロパティに `Setter` タグを追加します。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

@No__t_0 のコンテンツプロパティは `Value` ので、`Value` プロパティの値は、これらのタグ内で直接指定できます。 このプロパティの型は `VisualStateGroupList` です。

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

これで、このページのすべての `Entry` ビューが、表示状態と同じように応答するようになりました。 また、"フォーカスされた" 状態には2番目の `Setter` が含まれるようになりました。これにより、各 `Entry` に入力フォーカスがある場合にも、緑の背景が表示されます。

[![VSM (スタイル)](vsm-images/VsmInStyle.png "VSM (スタイル)")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>独自の視覚的状態の定義

@No__t_0 から派生するすべてのクラスは、"Normal"、"フォーカスされた"、"Disabled" という3つの一般的な状態をサポートします。 内部的には、 [`VisualElement`](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs)クラスは、有効または無効になっているか、フォーカスまたは見るされたことを検出し、静的な[`VisualStateManager.GoToState`](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String))メソッドを呼び出します。

```csharp
VisualStateManager.GoToState(this, "Focused");
```

これは、`VisualElement` クラスに含まれている唯一の表示状態マネージャーコードです。 @No__t_1 から派生したすべてのクラスに基づいてすべてのオブジェクトに対して `GoToState` が呼び出されるため、Visual State Manager を任意の `VisualElement` オブジェクトと共に使用して、これらの変更に応答することができます。

興味深いことに、ビジュアル状態グループ "CommonStates" の名前は、`VisualElement` で明示的に参照されていません。 グループ名は、Visual State Manager の API の一部ではありません。 これまでに示した2つのサンプルプログラムのいずれかで、グループの名前を "CommonStates" から他のものに変更することができ、プログラムは引き続き機能します。 グループ名は、そのグループ内の状態の一般的な説明にすぎません。 すべてのグループのビジュアル状態は相互に排他的であることが暗黙的に認識されます。1つの状態であり、いつでも1つの状態のみが最新です。

独自のビジュアル状態を実装する場合は、コードから `VisualStateManager.GoToState` を呼び出す必要があります。 ほとんどの場合、この呼び出しはページクラスの分離コードファイルから行います。

**[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)** サンプルの**VSM 検証**ページでは、入力検証と共に Visual State Manager を使用する方法を示しています。 XAML ファイルは、2つの `Label` 要素、`Entry`、および `Button` で構成されています。

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

VSM マークアップは、2番目の `Label` (名前付き `helpLabel`) と `Button` (名前付き `submitButton`) にアタッチされます。 "Valid" と "Invalid" という名前の2つの相互排他的な状態があります。 2つの "ValidationState" グループのそれぞれに "Valid" と "Invalid" の両方のタグが含まれ `VisualState` ていることに注意してください。ただし、そのうちの1つは、各ケースでは空です。 

@No__t_0 に有効な電話番号が含まれていない場合、現在の状態は "無効" になります。そのため、2番目の `Label` が表示され、`Button` は無効になります。

[![VSM 検証: 状態が無効です](vsm-images/VsmValidationInvalid.png "VSM 検証-無効")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

有効な電話番号を入力すると、現在の状態は "有効" になります。 2番目の `Entry` が消え、`Button` が有効になりました。

[![VSM 検証: 有効な状態](vsm-images/VsmValidationValid.png "VSM 検証-有効")](vsm-images/VsmValidationValid-Large.png#lightbox)

分離コードファイルは、`Entry` からの `TextChanged` イベントを処理するための担いです。 ハンドラーは、正規表現を使用して、入力文字列が有効かどうかを判断します。 @No__t_0 という名前の分離コードファイル内のメソッドは、`helpLabel` と `submitButton` の両方に対して静的 `VisualStateManager.GoToState` メソッドを呼び出します。

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

@No__t_0 メソッドは、状態を初期化するためにコンストラクターから呼び出されることにも注意してください。 常に現在の状態になっている必要があります。 しかし、コードには、表示状態グループの名前への参照が含まれていますが、わかりやすくするために、XAML では "ValidationStates" として参照されています。 

分離コードファイルは、これらの表示状態の影響を受けるページ上のすべてのオブジェクトを考慮し、これらの各オブジェクトに対して `VisualStateManager.GoToState` を呼び出す必要があることに注意してください。 この例では、2つのオブジェクト (`Label` と `Button`) のみですが、いくつかの方法があります。

コードビハインドファイルが、これらの表示状態の影響を受けるページ上のすべてのオブジェクトを参照する必要がある場合は、分離コードファイルで単にオブジェクトに直接アクセスできないのはなぜですか。 確かにできます。 ただし、VSM を使用する利点は、すべての UI デザインを1つの場所に保持することで、ビジュアル要素がどのような状態になるかを XAML 全体で制御できることです。 これにより、分離コードからビジュアル要素に直接アクセスすることで、視覚的な外観を設定することが回避されます。

@No__t_0 からクラスを派生させ、外部検証関数に設定できるプロパティを定義することも考えられます。 @No__t_0 から派生したクラスは、`VisualStateManager.GoToState` メソッドを呼び出すことができます。 このスキームは正常に動作しますが、`Entry` がさまざまな表示状態の影響を受ける唯一のオブジェクトである場合にのみ機能します。 この例では、`Label` と `Button` も影響を受けます。 @No__t_0 に関連付けられている VSM マークアップによってページ上の他のオブジェクトを制御することはできません。また、他のオブジェクトに関連付けられている VSM マークアップによって、別のオブジェクトからの視覚的状態の変更を参照することもできません。

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>アダプティブレイアウトでの Visual State Manager の使用

スマートフォンで実行されている Xamarin. フォームアプリケーションは、通常、縦または横の縦横比で表示できます。また、デスクトップで実行されている Xamarin. フォームプログラムは、さまざまなサイズや縦横比を想定するようにサイズ変更できます。 適切にデザインされたアプリケーションでは、さまざまなページまたはウィンドウのフォームファクターに応じてコンテンツが異なる方法で表示されることがあります。 

この手法は、_アダプティブレイアウト_とも呼ばれます。 アダプティブレイアウトでは、プログラムのビジュアルのみが必要であるため、ビジュアル状態マネージャーの理想的なアプリケーションです。

単純な例として、アプリケーションのコンテンツに影響を与える小さなボタンのコレクションを表示するアプリケーションがあります。 縦モードでは、これらのボタンはページ上部の水平方向の行に表示されることがあります。

[![VSM アダプティブレイアウト: 縦](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM アダプティブレイアウト-縦")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

横モードでは、ボタンの配列が一方向に移動され、列に表示されることがあります。

[![VSM アダプティブレイアウト: 横](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM アダプティブレイアウト-横")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

このプログラムは、上から下に、ユニバーサル Windows プラットフォーム、Android、および iOS で実行されています。

[VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)サンプルの**VSM アダプティブレイアウト**ページでは、"縦" と "横" という名前の2つの表示状態を持つ "OrientationStates" という名前のグループを定義します。 (より複雑な方法は、複数の異なるページやウィンドウの幅に基づいている場合があります)。 

VSM マークアップは、XAML ファイル内の4つの場所で実行されます。 @No__t_1 という名前の `StackLayout` には、`Image` 要素であるメニューとコンテンツの両方が含まれています。 この `StackLayout` では、縦モードの垂直方向と横向きモードの水平方向を持つ必要があります。

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

@No__t_1 という名前の内部 `ScrollView` と `menuStack` という名前の `StackLayout` には、ボタンのメニューが実装されています。 これらのレイアウトの向きは、`mainStack` とは逆です。 メニューは縦モードで水平にし、横モードで垂直にする必要があります。

VSM マークアップの4番目のセクションは、ボタン自体の暗黙的なスタイルです。 このマークアップは、`VerticalOptions`、`HorizontalOptions`、および `Margin` プロパティを設定します。

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

@No__t_0 ハンドラーは、2つの `StackLayout` および `ScrollView` 要素の `VisualStateManager.GoToState` を呼び出し、`menuStack` の子をループ処理して `VisualStateManager.GoToState` 要素の `Button` を呼び出します。

XAML ファイルの要素のプロパティを設定することによって、分離コードファイルで向きの変更をより直接的に処理できるように見えるかもしれませんが、視覚的な状態マネージャーは、明らかに構造化されたアプローチです。 すべてのビジュアルは XAML ファイルに保持され、簡単に調査、保守、および変更できます。

## <a name="visual-state-manager-with-xamarinuniversity"></a>Visual State Manager と Xamarin 大学

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin. Forms 3.0 Visual State Manager ビデオ**

## <a name="related-links"></a>関連リンク

- [VsmDemos](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-vsmdemos)
