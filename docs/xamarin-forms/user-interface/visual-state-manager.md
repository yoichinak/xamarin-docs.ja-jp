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

VSM の概念を紹介する_visual state_します。 など、Xamarin.Forms のビューを`Button`の基になる状態に応じていくつかの異なる視覚的外観を持つことができます&mdash;は無効になっていること、または、押された状態またはに入力フォーカスが、かどうか。 これらは、ボタンの状態です。 これらが、ボタンの状態です。

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
- `Entry`は、通常時はライムの背景を持つ。
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

[`VisualStateGroups`](xref:Xamarin.Forms.VisualStateManager.VisualStateGroupsProperty) は、[`VisualStateManager`](xref:Xamarin.Forms.VisualStateManager) クラスによって定義された、バインド可能な添付プロパティです。 (バインド可能な添付プロパティの詳細については、[添付プロパティ](~/xamarin-forms/xaml/attached-properties.md) の記事を参照してください)。このようにして、`Entry` オブジェクトに `VisualStateGroups` プロパティを添付します。

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

次の手順では、そのグループの各ビジュアルの状態のタグのペアが含まれます。 これらも識別できますを使用して`x:Name`または`Name`:

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

`VisualState` という名前のプロパティを定義します。 [ `Setters` ](xref:Xamarin.Forms.VisualState.Setters)、のコレクションである[ `Setter` ](xref:Xamarin.Forms.Setter)オブジェクト。 これらは同じ`Setter`で使用するオブジェクトを[ `Style` ](xref:Xamarin.Forms.Style)オブジェクト。

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

1 つまたは複数挿入できます`Setter`の各ペアの間でオブジェクト`Setters`タグ。 これらは、`Setter`前に説明した表示状態を定義するオブジェクト。

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

各`Setter`タグがその状態が現在、特定のプロパティの値を示します。 によって参照される任意のプロパティを`Setter`バインド可能なプロパティでオブジェクトをバックアップする必要があります。

次のようなマークアップの基になる、**ビューで VSM**ページで、 **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** サンプル プログラムです。 3 つのページが含まれます`Entry`ビューが 2 つ目の 1 つだけが接続して VSM マークアップ。

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

注意して、2 つ目`Entry`もが、`DataTrigger`の一部としてその`Trigger`コレクション。 これにより、 `Entry` 、3 つ目に何かが入力されるまで無効にする`Entry`します。 IOS、Android、およびユニバーサル Windows プラットフォーム (UWP) で実行されているスタートアップでページを示します。

[![ビューで VSM: 無効になっている](vsm-images/VsmOnViewDisabled.png "VSM ビュー - 無効になっています")](vsm-images/VsmOnViewDisabled-Large.png#lightbox)

現在の表示状態が"Disabled"ため、2 つ目のバック グラウンド`Entry`ピンクで iOS および Android の画面であります。 UWP 実装`Entry`背景を設定することはない色のときに、`Entry`は無効です。 

3 番目にいくつかのテキストを入力すると`Entry`、2 番目の`Entry`"Normal"の状態と、バック グラウンドにスイッチがライム。

[![ビューで VSM: 標準](vsm-images/VsmOnViewNormal.png "VSM ビュー - 標準")](vsm-images/VsmOnViewNormal-Large.png#lightbox)

2 つ目をタッチする`Entry`、入力フォーカスを取得します。 「フォーカス」状態に切り替わります、その高さの 2 倍に展開されます。

[![ビューで VSM: 重点を置いた](vsm-images/VsmOnViewFocused.png "VSM ビューに重点を置いています")](vsm-images/VsmOnViewFocused-Large.png#lightbox)

なお、`Entry`入力フォーカスを取得した場合は、ライム バック グラウンドを保持しません。 Visual State Manager は切り替える表示状態の間、以前の状態によって設定されるプロパティは設定されません。 視覚的な状態は相互に排他的なことに注意してください。 "Normal"の状態があることを意味のみ、`Entry`を有効にします。 つまり、`Entry`を有効にして、入力フォーカスがないです。 

場合は、`Entry`にライム バック グラウンドでは、「フォーカス」状態で、もう 1 つの追加`Setter`そのビジュアルの状態。

```xaml
<VisualState x:Name="Focused">
    <VisualState.Setters>
        <Setter Property="FontSize" Value="36" />
        <Setter Property="BackgroundColor" Value="Lime" />
    </VisualState.Setters>
</VisualState>
```

これらの順序で`Setter`正常に機能するオブジェクト、`VisualStateGroup`含める必要があります`VisualState`そのグループ内のすべての状態のオブジェクト。 されていないビジュアルの状態がある場合`Setter`空タグとしてリビジョン、オブジェクトを含めます。

```xaml
<VisualState x:Name="Normal" />
``` 

### <a name="visual-state-manager-markup-in-a-style"></a>スタイルの表示状態マネージャーのマークアップ

2 つまたは複数のビューの間で共通の Visual State Manager マークアップを共有する必要があります。 マークアップを配置したいここで、`Style`定義します。

ここでは、既存の暗黙的な`Style`の`Entry`内の要素、**ビューで VSM**ページ。

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

コンテンツのプロパティの`Setter`は`Value`ための値、`Value`それらのタグ内で直接プロパティを指定することができます。 型のプロパティが`VisualStateGroupList`:

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

VSM マークアップの残りの部分では前に、と同じです。

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

今すぐすべて、`Entry`のこのページ ビューの表示状態にも同様の応答します。 「フォーカス」状態が 2 つ目を今すぐが含まれることにも注意してください`Setter`により、各`Entry`とそれに入力フォーカスをライムもバック グラウンドします。

[![スタイルで VSM](vsm-images/VsmInStyle.png "スタイルで VSM")](vsm-images/VsmInStyle-Large.png#lightbox)

## <a name="defining-your-own-visual-states"></a>独自のビジュアル状態を定義します。

すべてのクラスから派生した`VisualElement`3 つ一般的な状態"Normal"、「フォーカス」および"Disabled"をサポートしています。 内部的には、 [ `VisualElement` ](https://github.com/xamarin/Xamarin.Forms/blob/master/Xamarin.Forms.Core/VisualElement.cs)有効または無効になっている、またはフォーカスがある、またはフォーカスされていないになると呼び出す静的クラスを検出した[ `VisualStateManager.GoToState` ](xref:Xamarin.Forms.VisualStateManager.GoToState(Xamarin.Forms.VisualElement,System.String))メソッド。

```csharp
VisualStateManager.GoToState(this, "Focused");
```

これでのみの Visual State Manager コード、`VisualElement`クラス。 `GoToState`から派生したすべてのクラスに基づくすべてのオブジェクトが呼び出されると`VisualElement`、Visual State Manager を使用して、いずれかで`VisualElement`これらの変更に応答するオブジェクト。

興味深いことに、"CommonStates"の表示状態グループの名前が明記されていないで`VisualElement`します。 Visual State Manager の API の一部でないグループ名。 これまでに示した 2 つのサンプル プログラムの 1 つを"CommonStates"から、それ以外のグループの名前を変更することができ、プログラムは引き続き動作します。 グループ名は、そのグループの状態の一般的な説明だけです。 任意のグループの表示状態は相互に排他的であるが暗黙的に認識します。 1 つの状態と 1 つだけの状態はいつでも現在します。

呼び出す必要があります、独自のビジュアル状態を実装する場合は、`VisualStateManager.GoToState`コードから。 ほとんどの場合、ページ クラスの分離コード ファイルからのこの呼び出しを行います。

**VSM 検証**ページで、 **[VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)** サンプルは、入力の検証に関連 Visual State Manager を使用する方法を示します。 XAML ファイルには、2 つの`Label`、要素、 `Entry`、および`Button`:

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

VSM マークアップが、2 つ目に接続されている`Label`(という名前`helpLabel`) および`Button`(という`submitButton`)。 「有効」および「が無効です」という名前の 2 つの相互に排他的な状態があります。 これらの 2 つの"ValidationState"グループに含まれる`VisualState`"Valid"と「無効」の両方のタグのうち 1 つは各ケースで空にします。 

場合、`Entry`現在の状態は「無効」で、そのため、有効な電話番号を含まない、2 つ目`Label`が表示されると、`Button`は無効です。

[![VSM 検証: 無効な状態](vsm-images/VsmValidationInvalid.png "VSM の検証が無効です")](vsm-images/VsmValidationInvalid-Large.png#lightbox)

有効な電話番号を入力したらの 現在の状態が"Valid"。 2 番目の`Entry`は表示されなくなります、`Button`が有効になりました。

[![VSM 検証: 有効な状態](vsm-images/VsmValidationValid.png "有効な VSM の検証")](vsm-images/VsmValidationValid-Large.png#lightbox)

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

## <a name="using-the-visual-state-manager-for-adaptive-layout"></a>Visual State Manager を使用してアダプティブのレイアウト

スマート フォンで実行されているアプリケーションは、縦または横の縦横比とデスクトップで実行されている Xamarin.Forms プログラムに通常表示できます Xamarin.Forms と多くのさまざまなサイズと縦横比を想定するサイズを変更できます。 適切に設計されたアプリケーションがこれらのさまざまなページまたはウィンドウ フォーム ファクターの異なる方法では、そのコンテンツを表示します。 

この手法とも呼ばれる_アダプティブ レイアウト_します。 アダプティブ レイアウトには、プログラムのビジュアルのみが含まれる、ため、Visual State Manager の理想的なアプリケーションになります。

簡単な例は、アプリケーションのコンテンツに影響するボタンの小規模なコレクションを表示するアプリケーションです。 縦モードの場合でこれらのボタンをページ上部にある水平方向の行で表示される可能性があります。

[![VSM アダプティブ レイアウト: 縦](vsm-images/VsmAdaptiveLayoutPortrait.png "VSM アダプティブ レイアウト - 縦")](vsm-images/VsmAdaptiveLayoutPortrait-Large.png#lightbox)

横モードでボタンの配列が 1 つの側に移動され、列に表示。

[![VSM アダプティブ レイアウト: 横](vsm-images/VsmAdaptiveLayoutLandscape.png "VSM アダプティブ レイアウト - 横")](vsm-images/VsmAdaptiveLayoutLandscape-Large.png#lightbox)

上から下に、プログラムのユニバーサル Windows プラットフォーム、Android、iOS で実行します。

**VSM アダプティブ レイアウト**ページで、 [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)サンプルは、「Portrait」と「Landscape」という 2 つのビジュアル状態を"OrientationStates"をという名前のグループを定義します。 (より複雑な方法はいくつかの異なるページまたはウィンドウの幅に基づく可能性があります)。 

VSM マークアップは、XAML ファイル内の 4 つの場所で発生します。 `StackLayout`という`mainStack`、メニューとは、コンテンツの両方を含む、`Image`要素。 これは、`StackLayout`縦モードでの垂直方向と横モードでの水平方向にある必要があります。

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

内部`ScrollView`という名前の`menuScroll`と`StackLayout`という名前の`menuStack`ボタンのメニューを実装します。 これらのレイアウトの方向が逆の`mainStack`します。 メニューは、縦モードで水平方向と横モードで垂直にする必要があります。

VSM マークアップの 4 番目のセクションでは、ボタン自体の暗黙的なスタイルでです。 このマークアップは`VerticalOptions`、 `HorizontalOptions`、および`Margin`portait と横長の向きに固有のプロパティ。

分離コード ファイルのセット、`BindingContext`プロパティの`menuStack`を実装する`Button`コマンド、およびにハンドラーも、`SizeChanged`ページのイベント。

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

`SizeChanged`ハンドラー呼び出し`VisualStateManager.GoToState`、2 つの`StackLayout`と`ScrollView`要素、およびの子をループし`menuStack`を呼び出す`VisualStateManager.GoToState`の`Button`要素。

分離コード ファイルは、XAML ファイルで要素のプロパティを設定して直接向きの変更を処理することができますが、Visual State Manager より構造化されたアプローチでは間違いなく場合のように思えるかもしれません。 すべてのビジュアルは、調査が容易になる、XAML ファイルで保持されますにより、保守、および変更します。

## <a name="visual-state-manager-with-xamarinuniversity"></a>Xamarin.University で visual State Manager

> [!VIDEO https://youtube.com/embed/qhUHbVP5mIQ]

**Xamarin.Forms 3.0 Visual State Manager により、 [Xamarin University](https://university.xamarin.com/)**

## <a name="related-links"></a>関連リンク

- [VsmDemos](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/VsmDemos/)

