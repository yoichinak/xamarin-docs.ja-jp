---
title: Xamarin.Forms RadioButton
description: Xamarin.FormsRadioButton は、ユーザーがセットから1つのオプションを選択できるようにするためのボタンの一種です。 各オプションは1つのラジオボタンで表され、1つのグループ内で選択できるラジオボタンは1つだけです。
ms.prod: xamarin
ms.assetid: E2AA40E0-69A5-41DF-BFC4-C151CA657451
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/02/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f1f36329c36dfae3f2ca0754ecbb867520d4c530
ms.sourcegitcommit: bd8ce0ae64698246db92a6c4396983290dc54a22
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/02/2021
ms.locfileid: "99427747"
---
# <a name="xamarinforms-radiobutton"></a>Xamarin.Forms RadioButton

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)

Xamarin.Forms [`RadioButton`](xref:Xamarin.Forms.RadioButton) は、ユーザーがセットから1つのオプションを選択できるようにするボタンの種類です。 各オプションは1つのラジオボタンで表され、1つのグループ内で選択できるラジオボタンは1つだけです。 既定では、それぞれの [`RadioButton`](xref:Xamarin.Forms.RadioButton) テキストが表示されます。

![既定の Radiobutton のスクリーンショット](radiobutton-images/radiobuttons-default.png "既定の Radiobutton")

ただし、一部のプラットフォームでは、が [`RadioButton`](xref:Xamarin.Forms.RadioButton) 表示される場合があり、 [`View`](xref:Xamarin.Forms.View) すべてのプラットフォームで、次のようにそれぞれの外観を再 `RadioButton` 定義でき [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) ます。

![再定義した Radiobutton のスクリーンショット](radiobutton-images/radiobuttons-controltemplate.png "Radiobutton の再定義")

コントロールは、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) 次のプロパティを定義します。

- `Content`に `object` `string` [`View`](xref:Xamarin.Forms.View) よって表示されるまたはを定義する、型の `RadioButton` 。
- `IsChecked``bool`がチェックされているかどうかを定義する型の `RadioButton` 。 このプロパティは、 `TwoWay` バインディングを使用し、既定値は `false` です。
- `GroupName`型の `string` 。これは、相互に排他的なコントロールを指定する名前を定義し `RadioButton` ます。 このプロパティの既定値は `null` です。
- `Value`型の `object` 。に関連付けられているオプションの一意の値を定義し `RadioButton` ます。
- `BorderColor`境界線の [`Color`](xref:Xamarin.Forms.Color) 色を定義する型の。
- `BorderWidth``double`境界線の幅を定義する型の `RadioButton` 。
- `CharacterSpacing`型の `double` 。表示されているテキストの文字間の間隔を定義します。
- `CornerRadius`型の `int` 。の角の半径を定義し `RadioButton` ます。
- `FontAttributes`[`FontAttributes`](xref:Xamarin.Forms.FontAttributes)テキストスタイルを決定する型の。
- `FontFamily``string`フォントファミリを定義する型の。
- `FontSize``double`フォントサイズを定義する型の。
- `TextColor`[`Color`](xref:Xamarin.Forms.Color)表示されるテキストの色を定義する型の。
- `TextTransform`型の `TextTransform` 。表示されているテキストの大文字と小文字の区別を定義します。

これらのプロパティは、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトが基になっています。つまり、これらは、データ バインディングの対象にすることができ、スタイルを設定できます。

また、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) コントロールは、 `CheckedChanged` `IsChecked` ユーザーまたはプログラムによる操作によってプロパティが変更されたときに発生するイベントも定義します。 `CheckedChangedEventArgs`イベントに付随するオブジェクト `CheckedChanged` には、型のという名前のプロパティが1つあり `Value` `bool` ます。 イベントが発生すると、プロパティの値は `CheckedChangedEventArgs.Value` プロパティの新しい値に設定され `IsChecked` ます。

[`RadioButton`](xref:Xamarin.Forms.RadioButton) グループ化は、 `RadioButtonGroup` 次の添付プロパティを定義するクラスによって管理できます。

-   `GroupName`型の `string` 。に含まれるオブジェクトのグループ名を定義し `RadioButton` `Layout<View>` ます。
- `SelectedValue`グループ内のチェックされた `object` オブジェクトの値を表す、型の `RadioButton` `Layout<View>` 。 この添付プロパティは、 `TwoWay` 既定でバインドを使用します。

添付プロパティの詳細については `GroupName` 、「 [Group radiobutton](#group-radiobuttons)」を参照してください。 添付プロパティの詳細につい `SelectedValue` ては、「 [RadioButton 状態の変更への応答](#respond-to-radiobutton-state-changes)」を参照してください。

## <a name="create-radiobuttons"></a>Radiobutton の作成

の外観は、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) プロパティに割り当てられたデータの型によって定義され `RadioButton.Content` ます。

- プロパティに `RadioButton.Content` が割り当てられると `string` 、各プラットフォームに、オプションボタンの円の横に水平に配置されて表示されます。
- に `RadioButton.Content` が割り当てられると [`View`](xref:Xamarin.Forms.View) 、サポートされているプラットフォーム (IOS、UWP) に表示されますが、サポートされていないプラットフォームは `View` オブジェクト (Android) の文字列形式にフォールバックします。 どちらの場合も、コンテンツは、ラジオボタンの円の横に水平方向に配置されます。
- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)がに適用されると [`RadioButton`](xref:Xamarin.Forms.RadioButton) 、 [`View`](xref:Xamarin.Forms.View) すべてのプラットフォームのプロパティにを割り当てることができ `RadioButton.Content` ます。 詳細については、「 [再定義オプションボタンの外観](#redefine-radiobutton-appearance)」を参照してください。

### <a name="display-string-based-content"></a>文字列ベースのコンテンツを表示する

は、プロパティにが [`RadioButton`](xref:Xamarin.Forms.RadioButton) 割り当てられている場合にテキストを表示し `Content` `string` ます。

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton Content="Cat" />
    <RadioButton Content="Dog" />
    <RadioButton Content="Elephant" />
    <RadioButton Content="Monkey"
                 IsChecked="true" />
</StackLayout>
```

この例で [`RadioButton`](xref:Xamarin.Forms.RadioButton) は、オブジェクトは同じ親コンテナー内に暗黙的にグループ化されています。 この XAML は、次のスクリーンショットに示されているような外観になります。

![テキストベースの Radiobutton のスクリーンショット](radiobutton-images/radiobuttons-text.png "テキストベースの Radiobutton")

### <a name="display-arbitrary-content"></a>任意のコンテンツを表示する

IOS と UWP では、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) プロパティにが割り当てられている場合、は任意のコンテンツを表示でき `Content` [`View`](xref:Xamarin.Forms.View) ます。

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton>
        <RadioButton.Content>
            <Image Source="cat.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton>
        <RadioButton.Content>
            <Image Source="dog.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton>
        <RadioButton.Content>
            <Image Source="elephant.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton>
        <RadioButton.Content>
            <Image Source="monkey.png" />
        </RadioButton.Content>
    </RadioButton>
</StackLayout>
```

この例で [`RadioButton`](xref:Xamarin.Forms.RadioButton) は、オブジェクトは同じ親コンテナー内に暗黙的にグループ化されています。 この XAML は、次のスクリーンショットに示されているような外観になります。

![ビューベースの Radiobutton のスクリーンショット](radiobutton-images/radiobuttons-view.png "ビューベースの Radiobutton")

Android では、オブジェクトには、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) [`View`](xref:Xamarin.Forms.View) コンテンツとして設定されたオブジェクトの文字列ベースの表現が表示されます。

![Android のビューベースの RadioButton のスクリーンショット](radiobutton-images/radiobutton-view-android.png "Android でのビューベースの RadioButton")

> [!NOTE]
> [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate)がに適用されると [`RadioButton`](xref:Xamarin.Forms.RadioButton) 、 [`View`](xref:Xamarin.Forms.View) すべてのプラットフォームのプロパティにを割り当てることができ `RadioButton.Content` ます。 詳細については、「 [再定義オプションボタンの外観](#redefine-radiobutton-appearance)」を参照してください。

## <a name="associate-values-with-radiobuttons"></a>値と Radiobutton の関連付け

各 [`RadioButton`](xref:Xamarin.Forms.RadioButton) オブジェクトには、 `Value` 型のプロパティがあります。このプロパティは `object` 、オプションボタンに関連付けられるオプションの一意の値を定義します。 これにより、の値をそのコンテンツとは異なるものにすることができ、 `RadioButton` `RadioButton` オブジェクトがオブジェクトを表示するときに特に便利です [`View`](xref:Xamarin.Forms.View) 。

次の XAML は、 `Content` `Value` 各オブジェクトのプロパティとプロパティを設定する方法を示してい [`RadioButton`](xref:Xamarin.Forms.RadioButton) ます。

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <RadioButton Value="Cat">
        <RadioButton.Content>
            <Image Source="cat.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton Value="Dog">
        <RadioButton.Content>
            <Image Source="dog.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton Value="Elephant">
        <RadioButton.Content>
            <Image Source="elephant.png" />
        </RadioButton.Content>
    </RadioButton>
    <RadioButton Value="Monkey">
        <RadioButton.Content>
            <Image Source="monkey.png" />
        </RadioButton.Content>
    </RadioButton>
</StackLayout>
```

この例では、それぞれ [`RadioButton`](xref:Xamarin.Forms.RadioButton) が [`Image`](xref:Xamarin.Forms.Image) コンテンツとしてを持ち、文字列ベースの値も定義しています。 これにより、チェックされたラジオボタンの値を簡単に識別できるようになります。

## <a name="group-radiobuttons"></a>グループの Radiobutton

オプションボタンはグループで機能します。オプションボタンをグループ化するには、次の3つの方法があります。

- 同じ親コンテナー内に配置します。 これは *暗黙的* なグループ化と呼ばれます。
- `GroupName`グループ内の各オプションボタンのプロパティを同じ値に設定します。 これを *明示的* なグループ化と呼びます。
- `RadioButtonGroup.GroupName`親コンテナーの添付プロパティを設定します。これにより、 `GroupName` コンテナー内の任意のオブジェクトのプロパティが設定され [`RadioButton`](xref:Xamarin.Forms.RadioButton) ます。 これは、 *明示的* なグループ化とも呼ばれます。

> [!IMPORTANT]
> [`RadioButton`](xref:Xamarin.Forms.RadioButton) オブジェクトは、グループ化する同じ親に属している必要はありません。 グループ名を共有することを指定した場合、これらは相互に排他的です。

### <a name="explicit-grouping-with-the-groupname-property"></a>GroupName プロパティを使用した明示的なグループ化

次の XAML の例では、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) プロパティを設定することによってオブジェクトを明示的にグループ化してい `GroupName` ます。

```xaml
<Label Text="What's your favorite color?" />
<RadioButton Content="Red"
             GroupName="colors" />
<RadioButton Content="Green"
             GroupName="colors" />
<RadioButton Content="Blue"
             GroupName="colors" />
<RadioButton Content="Other"
             GroupName="colors" />
```

この例では、それぞれ [`RadioButton`](xref:Xamarin.Forms.RadioButton) が同じ値を共有するため、相互に排他的です `GroupName` 。

### <a name="explicit-grouping-with-the-radiobuttongroupgroupname-attached-property"></a>RadioButtonGroup 添付プロパティを使用した明示的なグループ化

クラスは、 `RadioButtonGroup` `GroupName` `string` オブジェクトに対して設定できる型の添付プロパティを定義し `Layout<View>` ます。 これにより、任意のレイアウトをラジオボタングループに切り替えることができます。

```xaml
<StackLayout RadioButtonGroup.GroupName="colors">
    <Label Text="What's your favorite color?" />
    <RadioButton Content="Red" />
    <RadioButton Content="Green" />
    <RadioButton Content="Blue" />
    <RadioButton Content="Other" />
</StackLayout>
```

この例では、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) の各で、 [`StackLayout`](xref:Xamarin.Forms.StackLayout) プロパティがに設定され、 `GroupName` 相互に排他的になり `fruits` ます。

> [!NOTE]
> `Layout<View`添付プロパティを設定する> オブジェクトに、そのプロパティを `RadioButtonGroup.GroupName` 設定するが含まれている場合、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) `GroupName` プロパティの値が `RadioButton.GroupName` 優先されます。

## <a name="respond-to-radiobutton-state-changes"></a>RadioButton 状態の変更に応答する

ラジオ ボタンには、チェックされているかチェックが解除されているかの 2 つの状態があります。 オプションボタンがオンになっている場合、その `IsChecked` プロパティはに `true` なります。 オプションボタンがオフの場合、 `IsChecked` プロパティはに `false` なります。 ラジオボタンは、同じグループ内の別のオプションボタンをタップすることでクリアできますが、もう一度タップしてクリアすることはできません。 ただし、プログラムで `IsChecked` プロパティを `false` に設定すると、ラジオ ボタンをオフにすることができます。

### <a name="respond-to-an-event-firing"></a>イベントの発生に応答する

`IsChecked`ユーザーまたはプログラムによる操作によってプロパティが変更されると、 `CheckedChanged` イベントが発生します。 このイベントのイベントハンドラーは、変更に応答するように登録できます。

```xaml
<RadioButton Content="Red"
             GroupName="colors"
             CheckedChanged="OnColorsRadioButtonCheckedChanged" />
```

分離コードには、イベントのハンドラーが含まれてい `CheckedChanged` ます。

```csharp
void OnColorsRadioButtonCheckedChanged(object sender, CheckedChangedEventArgs e)
{
    // Perform required operation
}
```

`sender`引数は、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) このイベントを担当するです。 これを使用すると、オブジェクトにアクセスし [`RadioButton`](xref:Xamarin.Forms.RadioButton) たり、同じイベントハンドラーを共有する複数のオブジェクトを区別したりでき `RadioButton` `CheckedChanged` ます。

### <a name="respond-to-a-property-change"></a>プロパティの変更に応答する

クラスは、 `RadioButtonGroup` `SelectedValue` `object` オブジェクトに対して設定できる型の添付プロパティを定義し `Layout<View>` ます。 この添付プロパティは、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) レイアウトで定義されたグループ内でチェックされるの値を表します。

`IsChecked`ユーザーまたはプログラムによる操作によってプロパティが変更されると、 `RadioButtonGroup.SelectedValue` 添付プロパティも変更されます。 したがって、 `RadioButtonGroup.SelectedValue` 添付プロパティは、ユーザーの選択内容を格納するプロパティにデータをバインドすることができます。

```xaml
<StackLayout RadioButtonGroup.GroupName="{Binding GroupName}"
             RadioButtonGroup.SelectedValue="{Binding Selection}">
    <Label Text="What's your favorite animal?" />
    <RadioButton Content="Cat"
                 Value="Cat" />
    <RadioButton Content="Dog"
                 Value="Dog" />
    <RadioButton Content="Elephant"
                 Value="Elephant" />
    <RadioButton Content="Monkey"
                 Value="Monkey"/>
    <Label x:Name="animalLabel">
        <Label.FormattedText>
            <FormattedString>
                <Span Text="You have chosen:" />
                <Span Text="{Binding Selection}" />
            </FormattedString>
        </Label.FormattedText>
    </Label>
</StackLayout>
```

この例で `RadioButtonGroup.GroupName` は、添付プロパティの値は、バインディングコンテキストのプロパティによって設定され `GroupName` ます。 同様に、 `RadioButtonGroup.SelectedValue` 添付プロパティの値は、バインディングコンテキストのプロパティによって設定され `Selection` ます。 また、プロパティは `Selection` 、チェックされたのプロパティに更新され `Value` [`RadioButton`](xref:Xamarin.Forms.RadioButton) ます。

## <a name="radiobutton-visual-states"></a>RadioButton の表示状態

[`RadioButton`](xref:Xamarin.Forms.RadioButton)`Checked` `Unchecked` がオンまたはオフになったときにビジュアルの変更を開始するために使用できる、オブジェクトと表示状態があり `RadioButton` ます。

次の XAML の例は、との状態の表示状態を定義する方法を示してい `Checked` `Unchecked` ます。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="VisualStateManager.VisualStateGroups">
                <VisualStateGroupList>
                    <VisualStateGroup x:Name="CheckedStates">
                        <VisualState x:Name="Checked">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Green" />
                                <Setter Property="Opacity"
                                        Value="1" />
                            </VisualState.Setters>
                        </VisualState>
                        <VisualState x:Name="Unchecked">
                            <VisualState.Setters>
                                <Setter Property="TextColor"
                                        Value="Red" />
                                <Setter Property="Opacity"
                                        Value="0.5" />
                            </VisualState.Setters>
                        </VisualState>
                    </VisualStateGroup>
                </VisualStateGroupList>
            </Setter>
        </Style>
    </ContentPage.Resources>
    <StackLayout>
        <Label Text="What's your favorite mode of transport?" />
        <RadioButton Content="Car" />
        <RadioButton Content="Bike" />
        <RadioButton Content="Train" />
        <RadioButton Content="Walking" />
    </StackLayout>
</ContentPage>
```

この例では、暗黙的な [`Style`](xref:Xamarin.Forms.Style) によって [`RadioButton`](xref:Xamarin.Forms.RadioButton) オブジェクトがターゲットにされています。 をオンにすると、その `Checked` [`VisualState`](xref:Xamarin.Forms.VisualState) `RadioButton` `TextColor` プロパティが緑色に設定され、値が1に設定され `Opacity` ます。 がチェックされて `Unchecked` `VisualState` いない状態の場合 `RadioButton` 、その `TextColor` プロパティは赤に設定され、 `Opacity` 値は0.5 になります。 したがって、全体の効果として、がオフになっている場合、 `RadioButton` 赤と部分的に透明になり、チェックされると透明度のない緑になります。

![RadioButton の表示状態のスクリーンショット](radiobutton-images/radiobuttons-visualstates.png "RadioButton の表示状態")

ビジュアルの状態の詳細については、「[Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)」をご覧ください。

## <a name="redefine-radiobutton-appearance"></a>RadioButton の外観の再定義

既定では、 [`RadioButton`](xref:Xamarin.Forms.RadioButton) オブジェクトはプラットフォームレンダラーを使用して、サポートされているプラットフォームでネイティブコントロールを利用します。 ただし、 `RadioButton` [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) オブジェクトの外観がすべてのプラットフォームで同じになるように、ビジュアル構造をで再定義することもでき `RadioButton` ます。 [`RadioButton`](xref:Xamarin.Forms.RadioButton)クラスはクラスから継承されるため、これが可能です [`TemplatedView`](xref:Xamarin.Forms.TemplatedView) 。

次の XAML は、 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) オブジェクトのビジュアル構造を再定義するために使用できるを示してい [`RadioButton`](xref:Xamarin.Forms.RadioButton) ます。

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <ControlTemplate x:Key="RadioButtonTemplate">
            <Frame BorderColor="#F3F2F1"
                   BackgroundColor="#F3F2F1"
                   HasShadow="False"
                   HeightRequest="100"
                   WidthRequest="100"
                   HorizontalOptions="Start"
                   VerticalOptions="Start"
                   Padding="0">
                <VisualStateManager.VisualStateGroups>
                    <VisualStateGroupList>
                        <VisualStateGroup x:Name="CheckedStates">
                            <VisualState x:Name="Checked">
                                <VisualState.Setters>
                                    <Setter Property="BorderColor"
                                            Value="#FF3300" />
                                    <Setter TargetName="check"
                                            Property="Opacity"
                                            Value="1" />
                                </VisualState.Setters>
                            </VisualState>
                            <VisualState x:Name="Unchecked">
                                <VisualState.Setters>
                                    <Setter Property="BackgroundColor"
                                            Value="#F3F2F1" />
                                    <Setter Property="BorderColor"
                                            Value="#F3F2F1" />
                                    <Setter TargetName="check"
                                            Property="Opacity"
                                            Value="0" />
                                </VisualState.Setters>
                            </VisualState>
                        </VisualStateGroup>
                    </VisualStateGroupList>
                </VisualStateManager.VisualStateGroups>
                <Grid Margin="4"
                      WidthRequest="100">
                    <Grid WidthRequest="18"
                          HeightRequest="18"
                          HorizontalOptions="End"
                          VerticalOptions="Start">
                        <Ellipse Stroke="Blue"
                                 Fill="White"
                                 WidthRequest="16"
                                 HeightRequest="16"
                                 HorizontalOptions="Center"
                                 VerticalOptions="Center" />
                        <Ellipse x:Name="check"
                                 Fill="Blue"
                                 WidthRequest="8"
                                 HeightRequest="8"
                                 HorizontalOptions="Center"
                                 VerticalOptions="Center" />
                    </Grid>
                    <ContentPresenter />
                </Grid>
            </Frame>
        </ControlTemplate>

        <Style TargetType="RadioButton">
            <Setter Property="ControlTemplate"
                    Value="{StaticResource RadioButtonTemplate}" />
        </Style>
    </ContentPage.Resources>
    <!-- Page content -->
</ContentPage>
```

この例では、のルート要素 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) は、と表示状態を [`Frame`](xref:Xamarin.Forms.Frame) 定義するオブジェクトです `Checked` `Unchecked` 。 `Frame`オブジェクトは、、、およびの各オブジェクトの組み合わせを使用して、 [`Grid`](xref:Xamarin.Forms.Grid) [`Ellipse`](xref:Xamarin.Forms.Shapes.Ellipse) [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) のビジュアル構造を定義し [`RadioButton`](xref:Xamarin.Forms.RadioButton) ます。 この例には、  `RadioButtonTemplate` `ControlTemplate` ページ上の任意のオブジェクトのプロパティにを割り当てる暗黙的なスタイルも含まれてい `RadioButton` ます。

> [!NOTE]
> オブジェクトは、 [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) コンテンツが表示されるビジュアル構造内の場所をマークし [`RadioButton`](xref:Xamarin.Forms.RadioButton) ます。

次の XAML は [`RadioButton`](xref:Xamarin.Forms.RadioButton) 、暗黙的なスタイルを使用してを使用するオブジェクトを示してい [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) ます。 

```xaml
<StackLayout>
    <Label Text="What's your favorite animal?" />
    <StackLayout RadioButtonGroup.GroupName="animals"
                 Orientation="Horizontal">
        <RadioButton Value="Cat">
            <RadioButton.Content>
                <StackLayout>
                    <Image Source="cat.png"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand" />
                    <Label Text="Cat"
                           HorizontalOptions="Center"
                           VerticalOptions="End" />
                </StackLayout>
            </RadioButton.Content>
        </RadioButton>
        <RadioButton Value="Dog">
            <RadioButton.Content>
                <StackLayout>
                    <Image Source="dog.png"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand" />
                    <Label Text="Dog"
                           HorizontalOptions="Center"
                           VerticalOptions="End" />
                </StackLayout>
            </RadioButton.Content>
        </RadioButton>
        <RadioButton Value="Elephant">
            <RadioButton.Content>
                <StackLayout>
                    <Image Source="elephant.png"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand" />
                    <Label Text="Elephant"
                           HorizontalOptions="Center"
                           VerticalOptions="End" />
                </StackLayout>
            </RadioButton.Content>
        </RadioButton>
        <RadioButton Value="Monkey">
            <RadioButton.Content>
                <StackLayout>
                    <Image Source="monkey.png"
                           HorizontalOptions="Center"
                           VerticalOptions="CenterAndExpand" />
                    <Label Text="Monkey"
                           HorizontalOptions="Center"
                           VerticalOptions="End" />
                </StackLayout>
            </RadioButton.Content>
        </RadioButton>
    </StackLayout>
</StackLayout>
```

この例では、それぞれに対して定義されているビジュアル構造 [`RadioButton`](xref:Xamarin.Forms.RadioButton) が、で定義されているビジュアル構造に置き換えられ [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) ます。そのため、実行時に、内のオブジェクトが `ControlTemplate` それぞれのビジュアルツリーの一部になり `RadioButton` ます。 また、それぞれののコンテンツ `RadioButton` は、 [`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) コントロールテンプレートで定義されているに置き換えられます。 この結果、次の `RadioButton` ように表示されます。

![テンプレートを持つ Radiobutton のスクリーンショット](radiobutton-images/radiobuttons-templated.png "テンプレートベースの Radiobutton")

コントロールテンプレートの詳細については、「 [ Xamarin.Forms コントロールテンプレート](~/xamarin-forms/app-fundamentals/templates/control-template.md)」を参照してください。

## <a name="disable-a-radiobutton"></a>RadioButton を無効にする

アプリケーションが、チェックされるが有効な操作ではない状態になること `RadioButton` があります。 このような場合は、 `RadioButton` プロパティをに設定することで、を無効にすることができ `IsEnabled` `false` ます。

## <a name="related-links"></a>関連リンク

- [RadioButton のデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-radiobuttondemos/)
- [Xamarin.Forms ボタン](~/xamarin-forms/user-interface/button.md)
- [Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md)
