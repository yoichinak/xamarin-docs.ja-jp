---
title: Xamarin.Forms Visual 状態マネージャー
description: コードから設定する表示状態に基づく XAML 要素を変更するのにには、Visual State Manager を使用します。
ms.prod: xamarin
ms.assetid: 17296F14-640D-484B-A24C-A4E9B7013E4F
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 05/07/2018
ms.openlocfilehash: 14553bc9484ecc236fb4ceefd687ec7742109758
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/09/2018
---
# <a name="the-xamarinforms-visual-state-manager"></a>Xamarin.Forms Visual 状態マネージャー

_コードから設定する表示状態に基づく XAML 要素を変更するのにには、Visual State Manager を使用します。_

Visual 状態マネージャー (VSM) は、Xamarin.Forms 3.0 の新機能です。 VSM は、視覚的に変更を加える、ユーザー インターフェイスのコードから構造化された方法を提供します。 XAML では、ほとんどの場合、アプリケーションのユーザー インターフェイスが定義されているこの XAML には Visual State Manager が、ユーザー インターフェイスのビジュアルに与える影響を説明するマークアップが含まれています。

概念を導入して、VSM_ビジュアル状態_です。 Xamarin.Forms ビューなど、`Button`の基になる状態に応じていくつかの異なる視覚的外観を持つことができます&mdash;かが無効なまたは押された、またはに入力フォーカスがします。 これらは、ボタンの状態です。

収集されます。 ビジュアル状態_表示状態グループ_です。 表示状態グループ内のすべてのビジュアル状態は相互に排他的です。 単純なテキスト文字列では、表示状態と表示状態グループの両方が識別されます。

Xamarin.Forms Visual State Manager では、1 つの表示状態グループが 3 つの表示状態を"CommonStates"をという名前を定義します。

- "Normal"
- "Disabled"
- 「フォーカスされている」

この表示状態グループから派生したすべてのクラスではサポートされて[ `VisualElement`](xref:Xamarin.Forms.VisualElement)の基本クラスである[ `View` ](xref:Xamarin.Forms.View)と[ `Page`](xref:Xamarin.Forms.Page)です。 

独自の表示状態グループを定義することもできます。 と表示状態、この記事の内容としては方法を説明します。

> [!NOTE]
> 慣れている Xamarin.Forms 開発者[トリガー](~/xamarin-forms/app-fundamentals/triggers.md)はトリガーがビューのプロパティまたはイベントの発生の変更に基づくユーザー インターフェイスでのビジュアルで変更をすることもことに注意してください。 ただし、これらの変更のさまざまな組み合わせを処理するトリガーを使用してに紛らわしくなります。 従来は、Visual State Manager は、表示状態の組み合わせによる混乱を軽減するための Windows の XAML ベースの環境で導入されました。 VSM と表示状態グループ内で視覚的な状態は相互に排他的にもします。 いつでもは、各グループ内の 1 つだけの状態は、現在の状態です。

## <a name="the-common-states"></a>一般的な状態

Visual State Manager を使用すると、ビューは通常、または無効になっている、または入力フォーカスがある場合、ビューの外観を変更できるは、XAML ファイルでセクションを含めることができます。 これらと呼ばれますが、_一般的な状態_です。

たとえば、ある、`Entry`ページで、ビューの外観をして、`Entry`を次のように変更します。

- `Entry` 、ピンクが必要時にバック グラウンド、`Entry`は無効になります。
- `Entry`通常ライム背景を持つ必要があります。
- `Entry`に入力フォーカスがある場合と通常の高さに 2 回展開する必要があります。

個々 のビューに VSM マークアップをアタッチすることができます、または複数のビューに適用する場合は、スタイルで定義することができます。 次の 2 つのセクションでは、これらの方法について説明します。

### <a name="vsm-markup-on-a-view"></a>ビューに VSM マークアップ

VSM マークアップをアタッチする、`Entry`ビューで、最初の分割、`Entry`開始タグと終了タグに。

```xaml
<Entry FontSize="18">

</Entry>
```

状態のいずれかが使用されるため、明示的なフォント サイズ与え、`FontSize`内のテキストのサイズを 2 倍にするプロパティ、`Entry`です。

次に、挿入`VisualStateManager.VisualStateGroups`これらのタグの間のタグ。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>

    </VisualStateManager.VisualStateGroups>
</Entry>
```

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) によって定義される添付バインド可能なプロパティ、 [ `VisualStateManager` ](xref:Xamarin.Forms.VisualStateManager)クラスです。 (接続されているバインド可能なプロパティの詳細については、記事を参照してください[アタッチされるプロパティ](~/xamarin-forms/xaml/attached-properties.md)。)。これは、どのように`VisualStateGroups`プロパティが添付される、`Entry`オブジェクト。

`VisualStateGroups`プロパティの型は[ `VisualStateGroupList`](xref:Xamarin.Forms.VisualStateGroupList)の集合である[ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup)オブジェクト。 内で、 `VisualStateManager.VisualStateGroups` 、タグのペアを挿入する`VisualStateGroup`タグを含める表示状態のグループごとに。

```xaml
<Entry FontSize="18">
    <VisualStateManager.VisualStateGroups>
        <VisualStateGroup x:Name="CommonStates">

        </VisualStateGroup>
    </VisualStateManager.VisualStateGroups>
</Entry>
```

注意して、`VisualStateGroup`タグが、`x:Name`グループの名前を示す属性です。 `VisualStateGroup`クラスを定義、`Name`代わりに使用できるプロパティ。

```xaml
<VisualStateGroup Name="CommonStates">
```

いずれかを使用する`x:Name`または`Name`同じ要素の両方ではなくです。

`VisualStateGroup`クラスという名前のプロパティを定義する[ `States`](xref:Xamarin.Forms.VisualStateGroup.States)の集合である[ `VisualState` ](xref:Xamarin.Forms.VisualState)オブジェクト。 `States` _コンテンツ プロパティ_の`VisualStateGroups`利用できるようにする、`VisualState`タグの間で直接、`VisualStateGroup`タグ。 (コンテンツ プロパティは、記事で説明した[重要なは、XAML 構文](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md#content-properties))。

次の手順では、そのグループにすべてのビューステートのタグのペアを追加します。 これらも識別できますを使用して`x:Name`または`Name`:

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

`VisualState` という名前のプロパティを定義[ `Setters`](xref:Xamarin.Forms.VisualState.Setters)の集合である[ `Setter` ](xref:Xamarin.Forms.Setter)オブジェクト。 これらは同じ`Setter`で使用するオブジェクトを[ `Style` ](xref:Xamarin.Forms.Style)オブジェクト。

`Setters` _いない_の content プロパティ`VisualState`にプロパティ要素タグを含める必要があるため、`Setters`プロパティ。

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

1 つまたは複数を挿入できるようになりました`Setter`の各ペア間でオブジェクト`Setters`タグ。 これらは、`Setter`前に説明した表示状態を定義するオブジェクト。

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

各`Setter`タグは、その状態が現在のときに特定のプロパティの値を示します。 によって参照されるすべてのプロパティ、`Setter`バインド可能なプロパティでオブジェクトをバックアップする必要があります。

次のようなマークアップの基になる、**ビュー VSM**  ページで、 **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** サンプル プログラムです。 3 つのページが含まれます`Entry`ビューが 2 番目の 1 つだけがそれに付随する VSM マークアップ。

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

注意して、2 つ目`Entry`も、`DataTrigger`の一部としてその`Trigger`コレクション。 これにより、 `Entry` 、3 番目に何かが入力されるまで無効にする`Entry`です。 IOS、Android、およびユニバーサル Windows プラットフォーム (UWP) で実行されているスタートアップでページを次に示します。

[![ビューで VSM: 無効になっている](vsm-images/VsmOnViewDisabled.png "VSM ビュー - 無効になっています")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

現在の状態が「無効」ため、2 番目のバック グラウンド`Entry`ピンクで iOS および Android の画面であります。 UWP 実装`Entry`背景を設定することはありません色のときに、`Entry`は無効になります。 

3 番目にいくつかのテキストを入力すると`Entry`、2 番目`Entry`ライム"Normal"状態、およびバック グラウンドへの切り替えは。

[![ビューで VSM: 標準](vsm-images/VsmOnViewNormal.png "VSM ビュー - 標準")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

2 つ目に触れると`Entry`、入力フォーカスを取得します。 これにより、"Focused"状態に切り替わりますされ、その高さの 2 倍に拡張されます。

[![ビューで VSM: 重点を置いて](vsm-images/VsmOnViewFocused.png "ビュー - フォーカスされた VSM")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

注意して、`Entry`入力フォーカスを取得したとき、ライム背景は保持されません。 Visual State Manager は、視覚的な状態の間で切り替える、以前の状態によって設定されたプロパティが設定されていません。 視覚的な状態が相互に排他的なことに注意してください。 "Normal"状態とは限りませんだけを`Entry`を有効にします。 つまり、`Entry`が有効になっているし、入力フォーカスを持っていません。 

場合は、`Entry`にライム バック グラウンド"Focused"状態で、別の追加`Setter`そのビジュアルの状態にします。

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

これらの順序で`Setter`正常に機能するオブジェクト、`VisualStateGroup`含める必要があります`VisualState`そのグループ内のすべての状態のオブジェクト。 表示状態が含まれないことがある場合`Setter`空のタグとしてリビジョン、オブジェクトを含めます。

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>スタイルでビジュアル状態マネージャー マークアップ

2 つ以上のビューの間で同じ Visual State Manager マークアップを共有する必要があります。 ここでは、マークアップを配置にしておく、`Style`定義します。

ここでは、既存の暗黙的な`Style`の`Entry`内の要素、 **VSM でビュー**ページ。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
</Style> 
```

追加`Setter`のタグ、`VisualStateManager.VisualStateGroups`添付バインド可能なプロパティ。

```xaml
<Style TargetType="Entry">
    <Setter Property="Margin" Value="20, 0" />
    <Setter Property="FontSize" Value="18" />
    <Setter Property="VisualStateManager.VisualStateGroups">

    </Setter>
</Style> 
```

コンテンツ プロパティ`Setter`は`Value`ための値、`Value`プロパティは、これらのタグ内で直接指定できます。 型のプロパティが`VisualStateGroupList`:

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

これらのタグ内の 1 つ以上含めることができます`VisualStateGroup`オブジェクト。

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

ここでは、**スタイルで VSM** VSM の完全なマークアップを表示するページ。

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

今すぐすべて、`Entry`のこのページ ビューの応答を視覚の状態と同様です。 "Focused"状態が、2 番目には今すぐに含まれているにも注意してください`Setter`により、各`Entry`とそれに入力フォーカスが、ライムもバック グラウンドします。

[![スタイルで VSM](vsm-images/VsmInStyle.png "スタイルで VSM")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>独自の表示状態を定義します。

すべてのクラスから派生した`VisualElement`3 一般的な状態"Normal"、「フォーカスされている」、および"Disabled"をサポートしています。 内部的には、 [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs)有効または無効になっている、または対象を絞ったまたはフォーカスされていないになる場合は、静的なを呼び出すクラスを検出した[ `VisualStateManager.GoToState` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualStateManager.GoToState/p/Xamarin.Forms.VisualElement/System.String/)メソッド。

```csharp
VisualStateManager.GoToState(this, "Focused");
```

これにのみの Visual State Manager コード、`VisualElement`クラスです。 `GoToState`から派生するすべてのクラスに基づくすべてのオブジェクトが呼び出されると`VisualElement`、いずれかで Visual State Manager を使用できる`VisualElement`これらの変更に応答するオブジェクト。

興味深いことに、表示状態グループ"CommonStates"の名前が明示的に参照されないで`VisualElement`です。 グループ名は、Visual State Manager 用の API の一部ではありません。 これまでに示す 2 つのサンプル プログラムのいずれか、以外では、"CommonStates"からのグループの名前を変更することができ、プログラムは引き続き動作します。 グループ名は、そのグループ内の状態の一般的な説明だけです。 任意のグループで視覚的な状態は相互に排他的であるが暗黙的に認識します。 1 つの状態と 1 つの状態はいつでも現在します。

呼び出す必要があります、独自の表示状態を実装する場合は、`VisualStateManager.GoToState`コードからです。 ほとんどの場合、ページ クラスの分離コード ファイルからこの呼び出しを行うします。

**VSM 検証** ページで、 **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** サンプル入力の検証に関連して Visual State Manager を使用する方法を示します。 XAML ファイルには、2 つの`Label`、要素、 `Entry`、および`Button`:

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

VSM マークアップは、2 番目にアタッチされて`Label`(という名前`helpLabel`) および`Button`(という`submitButton`)。 「有効」および「が無効です」という名前の 2 つの相互に排他的な状態があります。 これらの 2 つの"ValidationState"グループに含まれる`VisualState`"Valid"と「無効」の両方のタグのうちの 1 つは各ケースで空にします。 

場合、`Entry`は有効な電話番号、現在の状態が「無効」し、2 番目`Label`が表示され、`Button`は無効になります。

[![VSM 検証: 無効な状態](vsm-images/VsmValidationInvalid.png "VSM 検証 - 無効")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

有効な電話番号を入力すると、現在の状態になります"Valid"です。 2 番目`Entry`は表示されなくなりますと`Button`が有効になりました。

[![VSM 検証: 有効な状態](vsm-images/VsmValidationValid.png "VSM 検証 - 有効")](vsm-images/VsmValidationValid-Large.png#lightbox)

分離コード ファイルが処理するための広範囲、`TextChanged`からイベントを`Entry`です。 ハンドラーでは、正規表現を使用して、入力文字列が有効かどうを調べます。 という名前の分離コード ファイル内のメソッド`GoToState`呼び出す静的`VisualStateManager.GoToState`両方のメソッド`helpLabel`と`submitButton`:

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

また、`GoToState`メソッドは、状態を初期化するコンス トラクターから呼び出されます。 現在の状態は、常にあります。 含まれていませんコードではあります表示状態グループの名前への参照わかりやすくするための目的で"ValidationStates"としての XAML で参照されています。 

分離コード ファイルも、これらのビジュアルの状態を呼び出すには、すべてのオブジェクトのアカウントに影響を受けるページを受け取る必要がありますに注意してください。`VisualStateManager.GoToState`のこれらの各オブジェクトです。 この例では 2 つのオブジェクト (、`Label`と`Button`)、複数ある可能性がありますが、詳細です。

でしょうか場合は、分離コード ファイルには、これらの表示状態の影響を受けるページ上のすべてのオブジェクトを参照する必要があります、理由ことはできません、分離コード ファイルだけで、オブジェクトに直接アクセスしますか?。 間違いなく可能性があります。 ただし、VSM を使用する利点は、どのようにビジュアル要素を制御できることを XAML UI のデザインのすべてを 1 つの場所に保持するだけでさまざまな状態に対応します。 設定視覚的な外観を視覚的要素を分離コードから直接アクセスすることによって回避できます。

クラスを派生することが検討される可能性があります`Entry`とおそらく外部検証関数に設定できるプロパティを定義します。 派生したクラス`Entry`呼び出すことができますし、`VisualStateManager.GoToState`メソッドです。 このスキームは場合にのみが正常に機能は、`Entry`されたビジュアル状態の影響を受けたオブジェクトのみです。 この例では、`Label`と`Button`も影響を受けます。 VSM マークアップがアタッチされているために、方法はありません、 `Entry` VSM マークアップでは、表示状態の変更を別のオブジェクトから参照するには、その他のオブジェクトをこれらに接続 ページで、および一切の他のオブジェクトを制御します。

<a name="adaptive-layout" />

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>レイアウトのアダプティブ Visual State Manager を使用します。

スマート フォンで実行されているアプリケーションは、縦または横の縦横比、あり、Xamarin.Forms プログラムでデスクトップ上で実行で通常表示できる Xamarin.Forms と多くのさまざまなサイズと縦横比を想定するサイズを変更できます。 適切に設計されたアプリケーションがこれらのさまざまなページまたはウィンドウ フォーム ファクターの異なる方法でそのコンテンツを表示します。 

この手法とも呼ばれます_アダプティブ レイアウト_です。 アダプティブ レイアウトには、プログラムのビジュアルのみ含まれているために、Visual State Manager の理想的なアプリケーションを勧めします。

単純な例は、アプリケーションのコンテンツに影響するボタンの小規模のコレクションを表示するアプリケーションです。 縦向きモードの場合は、これらのボタンに表示されるページの上部の水平方向の行。

[![レイアウトのアダプティブ VSM: 縦](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM アダプティブ レイアウト - 縦")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

横モードでボタンの配列を一方の側に移動され、列に表示される可能性があります。

[![レイアウトのアダプティブ VSM: 横](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM アダプティブ レイアウト - 横")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

上から下に、プログラムのユニバーサル Windows プラットフォーム、Android、および iOS を実行します。

**VSM アダプティブ レイアウト** ページで、 [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)サンプルが 2 つの表示状態が「縦」および「横 ()」という名前に"OrientationStates"をという名前のグループを定義します。 (より複雑な方法はいくつかの異なるページまたはウィンドウの幅に基づく可能性があります)。 

VSM マークアップは、XAML ファイルで 4 つの場所で発生します。 `StackLayout`という`mainStack`、メニューとは、コンテンツの両方を含む、`Image`要素。 これは、`StackLayout`縦向きモードの向きを縦方向と横モードで水平方向にある必要があります。

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

内部`ScrollView`という名前`menuScroll`と`StackLayout`という名前`menuStack`ボタンのメニューを実装します。 これらのレイアウトの方向が逆の`mainStack`します。 縦向きモードの水平方向および横モードで垂直、メニューがあります。

VSM マークアップの 4 番目のセクションでは、自身のボタンの暗黙的なスタイルでです。 このマークアップ`VerticalOptions`、 `HorizontalOptions`、および`Margin`portait 方向と横方向に固有のプロパティです。

分離コード ファイルのセット、`BindingContext`プロパティの`menuStack`を実装する`Button`、コマンド実行ともにハンドラーをアタッチ、`SizeChanged`ページのイベント。

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

`SizeChanged`ハンドラーの呼び出し`VisualStateManager.GoToState`、2 つの`StackLayout`と`ScrollView`要素、および、ループの子を`menuStack`を呼び出す`VisualStateManager.GoToState`の`Button`要素。

分離コード ファイルは、XAML ファイルで要素のプロパティを設定して直接向きの変更を処理できますが、Visual State Manager は、構造化されたアプローチ、明らかかのように思われる場合があります。 すべてのビジュアルに確認しやすくなる、XAML ファイルで保持されますを維持し、変更します。

## <a name="visual-state-manager-with-xamarinuniversity"></a>Xamarin.University で visual State Manager

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms 3.0 Visual State Manager により、 [Xamarin 大学](https://university.xamarin.com/)**

## <a name="related-links"></a>関連リンク

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

