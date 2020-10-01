---
title: Xamarin.Forms の複数バインド
description: この記事では、MultiBinding クラスを使用して、Binding オブジェクトのコレクションを 1 つのバインディング ターゲットのプロパティにアタッチする方法について説明します。
ms.prod: xamarin
ms.assetid: E73AE622-664C-4A90-B5B2-BD47D0E7A1A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/18/2020
ms.openlocfilehash: 0c10e73d8d6c2dcafacbb069eaf905a227030b87
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91557531"
---
# <a name="xamarinforms-multi-bindings"></a>Xamarin.Forms の複数バインド

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/databindingdemos)

複数バインドを使用すると、[`Binding`](xref:Xamarin.Forms.Binding) オブジェクトのコレクションを、1 つのバインディング ターゲットのプロパティにアタッチできます。 これらは `MultiBinding` クラスを使用して作成されます。このクラスでは、そのすべての `Binding` オブジェクトが評価され、アプリケーションによって提供される `IMultiValueConverter` インスタンスを介して 1 つの値が返されます。 さらに、`MultiBinding` では、バインドされたデータのいずれかが変更されたときに、そのすべての `Binding` オブジェクトが再評価されます。

`MultiBinding` クラスでは、次のプロパティが定義されます。

- `Bindings` (`IList<BindingBase>` 型): `MultiBinding` インスタンス内の [`Binding`](xref:Xamarin.Forms.Binding) オブジェクトのコレクションを表します。
- `Converter` (`IMultiValueConverter` 型): ソース値とターゲット値との間の変換に使用するコンバーターを表します。
- `ConverterParameter` (`object` 型): `Converter` に渡す省略可能なパラメーターを表します。

`Bindings` プロパティは `MultiBinding` クラスのコンテンツ プロパティであるため、XAML から明示的に設定する必要はありません。

さらに、`MultiBinding` クラスは、`BindingBase` クラスから次のプロパティを継承します。

- `FallbackValue` (`object` 型): 複数バインドから値を返すことができない場合に使用する値を表します。
- `Mode` ([`BindingMode`](xref:Xamarin.Forms.BindingMode) 型): 複数バインドのデータ フローの方向を示します。
- `StringFormat` (`string` 型): 複数バインドの結果が文字列として表示される場合に、どのように書式設定するかを指定します。
- `TargetNullValue` (`object` 型): ソース値が `null` の場合に、ターゲットで使用される値を表します。

`MultiBinding` では、`Bindings` コレクション内のバインドの値に基づき、`IMultiValueConverter` を使用してバインディング ターゲットの値を生成する必要があります。 たとえば、[`Color`](xref:Xamarin.Forms.Color) は赤、青、および緑の値から計算できますが、これらは同じまたは異なるバインディング ソース オブジェクトからの値にすることができます。 ターゲットからソースに値が移動すると、ターゲットのプロパティ値は、バインドにフィードバックされる値のセットに変換されます。

> [!IMPORTANT]
> `Bindings` コレクション内の個々のバインドは、独自の値コンバーターを持つことができます。

`Mode` プロパティの値によって、`MultiBinding` の機能が決まります。これは、個々のバインドによってプロパティがオーバーライドされない限り、コレクション内のすべてのバインドでバインド モードとして使用されます。 たとえば、`MultiBinding` オブジェクトの `Mode` プロパティが [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) に設定されている場合、いずれかのバインドで別の `Mode` 値を明示的に設定しない限り、コレクション内のすべてのバインドは `TwoWay` と見なされます。

## <a name="define-a-imultivalueconverter"></a>IMultiValueConverter を定義する

`IMultiValueConverter` インターフェイスを使用すると、カスタム ロジックを `MultiBinding` に適用できます。 コンバーターを `MultiBinding` に関連付けるには、`IMultiValueConverter` インターフェイスを実装するクラスを作成し、`Convert` メソッドと `ConvertBack` メソッドを実装します。

```csharp
public class AllTrueMultiConverter : IMultiValueConverter
{
    public object Convert(object[] values, Type targetType, object parameter, CultureInfo culture)
    {
        if (values == null || !targetType.IsAssignableFrom(typeof(bool)))
        {
            return false;
            // Alternatively, return BindableProperty.UnsetValue to use the binding FallbackValue
        }

        foreach (var value in values)
        {
            if (!(value is bool b))
            {
                return false;
                // Alternatively, return BindableProperty.UnsetValue to use the binding FallbackValue
            }
            else if (!b)
            {
                return false;
            }
        }
        return true;
    }

    public object[] ConvertBack(object value, Type[] targetTypes, object parameter, CultureInfo culture)
    {
        if (!(value is bool b) || targetTypes.Any(t => !t.IsAssignableFrom(typeof(bool))))
        {
            // Return null to indicate conversion back is not possible
            return null;
        }

        if (b)
        {
            return targetTypes.Select(t => (object)true).ToArray();
        }
        else
        {
            // Can't convert back from false because of ambiguity
            return null;
        }
    }
}
```

`Convert` メソッドでは、ソース値がバインディング ターゲットの値に変換されます。 Xamarin.Forms では、ソース バインドからの値をバインディング ターゲットに伝達するときに、このメソッドが呼び出されます。 このメソッドは、4 つの引数を受け取ります。

- `values` (`object[]` 型): `MultiBinding` 内のソース バインドによって生成される値の配列。
- `targetType` (`Type` 型): バインディング ターゲットのプロパティの型。
- `parameter` (`object` 型): 使用するコンバーター パラメーター。
- `culture` (`CultureInfo` 型): コンバーターで使用するカルチャ。

`Convert` メソッドでは、変換された値を表す `object` が返されます。 このメソッドでは、次を返す必要があります。

- `BindableProperty.UnsetValue`: コンバーターによって値が生成されなかったこと、およびバインドで `FallbackValue` が使用されることを示す場合。
- `Binding.DoNothing`: いかなる操作も実行しないように Xamarin.Forms に指示する場合。 たとえば、バインディング ターゲットに値を転送しないこと、または `FallbackValue` を使用しないことを Xamarin.Forms に指示する場合です。
- `null`: コンバーターで変換を実行できないこと、およびバインドで `TargetNullValue` が使用されることを示す場合。

> [!IMPORTANT]
> `Convert` メソッドから `BindableProperty.UnsetValue` を受け取る `MultiBinding` では、[`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) プロパティを定義する必要があります。 同様に、`Convert` メソッドから `null` を受け取る `MultiBinding` では、[`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) プロパティを定義する必要があります。

`ConvertBack` メソッドでは、バインディング ターゲットがソース バインドの値に変換されます。 このメソッドは、4 つの引数を受け取ります。

- `value` (`object` 型)。バインディング ターゲットによって生成される値。
- `targetTypes` (`Type[]` 型)。変換先の型の配列。 配列の長さは、メソッドの戻り値として推奨されている値の数と型を示します。
- `parameter` (`object` 型): 使用するコンバーター パラメーター。
- `culture` (`CultureInfo` 型): コンバーターで使用するカルチャ。

`ConvertBack` メソッドでは、ターゲット値からソース値に変換された、`object[]` 型の値の配列が返されます。 このメソッドでは、次を返す必要があります。

- `BindableProperty.UnsetValue` (位置 `i`): コンバーターでインデックス `i` のソース バインドの値を提供することができず、値が設定されないことを示す場合。
- `Binding.DoNothing` (位置 `i`): インデックス `i` のソース バインドに値が設定されないことを示す場合。
- `null`: コンバーターで変換を実行できないこと、または、この方向の変換がサポートされていないことを示す場合。

## <a name="consume-a-imultivalueconverter"></a>IMultiValueConverter を使用する

`IMultiValueConverter` を使用するには、リソース ディクショナリでインスタンス化した後、`StaticResource` マークアップ拡張を使用してそれを参照し、`MultiBinding.Converter` プロパティを設定します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MultiBindingConverterPage"
             Title="MultiBinding Converter demo">

    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />
        <local:InverterConverter x:Key="InverterConverter" />
    </ContentPage.Resources>

    <CheckBox>
        <CheckBox.IsChecked>
            <MultiBinding Converter="{StaticResource AllTrueConverter}">
                <Binding Path="Employee.IsOver16" />
                <Binding Path="Employee.HasPassedTest" />
                <Binding Path="Employee.IsSuspended"
                         Converter="{StaticResource InverterConverter}" />
            </MultiBinding>
        </CheckBox.IsChecked>
    </CheckBox>
</ContentPage>    
```

この例では、`MultiBinding` オブジェクトによって `AllTrueMultiConverter` インスタンスを使用して [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) プロパティが `true` に設定されます (3 つの [`Binding`](xref:Xamarin.Forms.Binding) オブジェクトが `true` に評価される場合)。 それ以外の場合は、`CheckBox.IsChecked` プロパティは `false` に設定されます。

既定では、[`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) プロパティでは [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) バインドが使用されます。 したがって、`AllTrueMultiConverter` インスタンスの `ConvertBack` メソッドは、ユーザーが [`CheckBox`](xref:Xamarin.Forms.CheckBox) をオフにしたときに実行されます。これにより、ソース バインドの値が `CheckBox.IsChecked` プロパティの値に設定されます。

## <a name="format-strings"></a>書式指定文字列

`MultiBinding` では、`StringFormat` プロパティを使用して、文字列として表示される複数バインドの結果を書式設定できます。 このプロパティは、プレースホルダーを含む標準の .NET 書式設定文字列に設定できます。これによって、複数バインドの結果を書式設定する方法が指定されます。

```xaml
<Label>
    <Label.Text>
        <MultiBinding StringFormat="{}{0} {1} {2}">
            <Binding Path="Employee1.Forename" />
            <Binding Path="Employee1.MiddleName" />
            <Binding Path="Employee1.Surname" />
        </MultiBinding>
    </Label.Text>
</Label>
```

この例では、`StringFormat` プロパティによって、3 つのバインドされた値が、[`Label`](xref:Xamarin.Forms.Label) によって表示される 1 つの文字列に結合されています。

> [!IMPORTANT]
> 複合文字列形式に含まれるパラメーターの数は、`MultiBinding` 内の子 `Binding` オブジェクトの数を超えることはできません。

`Converter` プロパティと `StringFormat` プロパティを設定する場合、最初にコンバーターがデータ値に適用され、その後 `StringFormat` が適用されます。

Xamarin.Forms での文字列の書式設定の詳細については、「[Xamarin.Forms の文字列の書式設定](string-formatting.md)」をご覧ください。

## <a name="provide-fallback-values"></a>フォールバック値を指定する

バインドのプロセスが失敗した場合に使うフォールバック値を定義することにより、データ バインディングをより堅牢にすることができます。 これを実現するには、必要に応じて、`MultiBinding` オブジェクトの [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) プロパティと [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) プロパティを定義します。

`MultiBinding` では、`IMultiValueConverter` インスタンスの `Convert` メソッドによって `BindableProperty.UnsetValue` が返された場合 (コンバーターで値が生成されなかったことを示します) に、その [`FallbackValue`](xref:Xamarin.Forms.BindingBase.FallbackValue) が使用されます。 `MultiBinding` では、`IMultiValueConverter` インスタンスの `Convert` メソッドによって `null` が返された場合 (コンバーターで変換を実行できないことを示します) に、その [`TargetNullValue`](xref:Xamarin.Forms.BindingBase.TargetNullValue) が使用されます。

バインドのフォールバックの詳細については、「[Xamarin.Forms のバインドのフォールバック](binding-fallbacks.md)」をご覧ください。

## <a name="nest-multibinding-objects"></a>MultiBinding オブジェクトを入れ子にする

`MultiBinding` オブジェクトは入れ子にすることができます。これにより、複数の `MultiBinding` オブジェクトが評価され、`IMultiValueConverter` インスタンスを介して値が返されます。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.NestedMultiBindingPage"
             Title="Nested MultiBinding demo">

    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />
        <local:AnyTrueMultiConverter x:Key="AnyTrueConverter" />
        <local:InverterConverter x:Key="InverterConverter" />
    </ContentPage.Resources>

    <CheckBox>
        <CheckBox.IsChecked>
            <MultiBinding Converter="{StaticResource AnyTrueConverter}">
                <MultiBinding Converter="{StaticResource AllTrueConverter}">
                    <Binding Path="Employee.IsOver16" />
                    <Binding Path="Employee.HasPassedTest" />
                    <Binding Path="Employee.IsSuspended" Converter="{StaticResource InverterConverter}" />                        
                </MultiBinding>
                <Binding Path="Employee.IsMonarch" />
            </MultiBinding>
        </CheckBox.IsChecked>
    </CheckBox>
</ContentPage>
```

この例では、`MultiBinding` オブジェクトによって、その `AnyTrueMultiConverter` インスタンスを使用して [`CheckBox.IsChecked`](xref:Xamarin.Forms.CheckBox.IsChecked) プロパティが `true` に設定されます (内部の `MultiBinding` オブジェクト内のすべての [`Binding`](xref:Xamarin.Forms.Binding) オブジェクトが `true` に評価される場合、または外側の `MultiBinding` オブジェクト内の `Binding` オブジェクトが `true` に評価される場合)。 それ以外の場合は、`CheckBox.IsChecked` プロパティは `false` に設定されます。

## <a name="use-a-relativesource-binding-in-a-multibinding"></a>MultiBinding で RelativeSource バインドを使用する

`MultiBinding` オブジェクトでは相対バインドがサポートされています。これを使用すると、バインディング ターゲットの位置を基準としてバインディング ソースを設定することができます。

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:DataBindingDemos">
    <ContentPage.Resources>
        <local:AllTrueMultiConverter x:Key="AllTrueConverter" />

        <ControlTemplate x:Key="CardViewExpanderControlTemplate">
            <Expander BindingContext="{Binding Source={RelativeSource TemplatedParent}}"
                      IsExpanded="{Binding IsExpanded, Source={RelativeSource TemplatedParent}}"
                      BackgroundColor="{Binding CardColor}">
                <Expander.IsVisible>
                    <MultiBinding Converter="{StaticResource AllTrueConverter}">
                        <Binding Path="IsExpanded" />
                        <Binding Path="IsEnabled" />
                    </MultiBinding>
                </Expander.IsVisible>
                <Expander.Header>
                    <Grid>
                        <!-- XAML that defines Expander header goes here -->
                    </Grid>
                </Expander.Header>
                <Grid>
                    <!-- XAML that defines Expander content goes here -->
                </Grid>
            </Expander>
        </ControlTemplate>
    </ContentPage.Resources>

    <StackLayout>
        <controls:CardViewExpander BorderColor="DarkGray"
                                   CardTitle="John Doe"
                                   CardDescription="Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla elit dolor, convallis non interdum."
                                   IconBackgroundColor="SlateGray"
                                   IconImageSource="user.png"
                                   ControlTemplate="{StaticResource CardViewExpanderControlTemplate}"
                                   IsEnabled="True"
                                   IsExpanded="True" />
    </StackLayout>
</ContentPage>
```

この例では、`TemplatedParent` 相対バインド モードを使用して、コントロール テンプレート内から、テンプレートが適用されるランタイム オブジェクト インスタンスへのバインドが行われています。 [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) のルート要素である `Expander` の `BindingContext` には、テンプレートが適用されるランタイム オブジェクト インスタンスが設定されています。 そのため、`Expander` とその子によって、そのバインド式、および [`Binding`](xref:Xamarin.Forms.Binding) オブジェクトが、`CardViewExpander` オブジェクトのプロパティに対して解決されます。 `MultiBinding` によって、`AllTrueMultiConverter` インスタンスを使用して `Expander.IsVisible` プロパティが `true` に設定されます (2 つの [`Binding`](xref:Xamarin.Forms.Binding) オブジェクトが `true` に評価される場合)。 それ以外の場合は、`Expander.IsVisible` プロパティは `false` に設定されます。

相対バインドの詳細については、「[Xamarin.Forms の相対バインド](relative-bindings.md)」を参照してください。 コントロール テンプレートの詳細については、「[Xamarin.Forms のコントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-template.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [データ バインディングのデモ (サンプル)](/samples/xamarin/xamarin-forms-samples/databindingdemos)
- [Xamarin.Forms の文字列の書式設定](string-formatting.md)
- [Xamarin.Forms のバインドのフォールバック](binding-fallbacks.md)
- [Xamarin.Forms の相対バインド](relative-bindings.md)
- [Xamarin.Forms のコントロール テンプレート](~/xamarin-forms/app-fundamentals/templates/control-template.md)