---
title: Xamarin.Forms のバインド モード
description: この記事では、ソースと BindingMode 列挙体のメンバーを指定したバインド モードを使用してターゲットの間で情報のフローを制御する方法について説明します。 すべてのバインド可能なプロパティには、プロパティがデータ バインディングのターゲットは、有効なモードを示す既定のバインド モードがあります。
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 05/01/2018
ms.openlocfilehash: 420c1de0691de419180dd497a9031ea5e7dd1054
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/24/2018
ms.locfileid: "37935668"
---
# <a name="xamarinforms-binding-mode"></a>Xamarin.Forms のバインド モード

[前の記事](basic-bindings.md)、**代わりにコードのバインディング**と**代わりに XAML バインディング**ページ機能を備えた、`Label`でその`Scale`プロパティバインドされている、`Value`のプロパティを`Slider`します。 `Slider`初期値は 0 で、これが原因、`Scale`のプロパティ、 `Label` 1 ではなく 0 に設定して、`Label`消えています。

[ **DataBindingDemos** ](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)サンプルについては、**逆バインディング**ページ似ていますが、前の記事では、プログラムを上のデータバインディングが定義されています。`Slider`ではなく、 `Label`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="DataBindingDemos.ReverseBindingPage"
             Title="Reverse Binding">
    <StackLayout Padding="10, 0">

        <Label x:Name="label"
               Text="TEXT"
               FontSize="80"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />

        <Slider x:Name="slider"
                VerticalOptions="CenterAndExpand"
                Value="{Binding Source={x:Reference label},
                                Path=Opacity}" />
    </StackLayout>
</ContentPage>
```

最初は、これに見えるかもしれません内を後方に向かって: ようになりました、 `Label` 、データ バインディング ソースと`Slider`はターゲットです。 バインドの参照、`Opacity`のプロパティ、`Label`は既定値は 1 です。

ご想像のとおり、`Slider`初期から値 1 に初期化されます`Opacity`@property`Label`します。 これは、左側の iOS のスクリーン ショットに示されます。

[![バインディングの逆](binding-mode-images/reversebinding-small.png "バインドを逆")](binding-mode-images/reversebinding-large.png#lightbox "バインディングの反転")

必要があるスタジオに驚き、 `Slider` Android、UWP のスクリーン ショットが示すようにするのには、続行します。 場合にお勧めのデータ バインドの動作を提案するよう、`Slider`はバインディング ターゲットではなく、`Label`初期化が予期していたように動作するためです。

間の差、**リバース バインド**サンプルと以前のサンプルでは、*バインド モード*します。

## <a name="the-default-binding-mode"></a>既定のバインド モード

メンバーとバインド モードが指定されて、 [ `BindingMode` ](xref:Xamarin.Forms.BindingMode)列挙体。

- [`Default`](xref:Xamarin.Forms.BindingMode.Default)
- [`TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay) &ndash; データは、ソースとターゲット間の両方向を移動します。
- [`OneWay`](xref:Xamarin.Forms.BindingMode.OneWay) &ndash; データがソースからターゲットに
- [`OneWayToSource`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; データは、ターゲットからソースにアクセスします。
- [`OneTime`](xref:Xamarin.Forms.BindingMode.OneWayToSource) &ndash; データがソースからターゲットが場合にのみが、`BindingContext`変更 (新しい Xamarin.Forms 3.0)

すべてのバインド可能なプロパティが既定のバインド、バインド可能なプロパティが作成されるときに設定されているモードとから利用できる、 [ `DefaultBindingMode` ](xref:Xamarin.Forms.BindableProperty.DefaultBindingMode)のプロパティ、`BindableProperty`オブジェクト。 この既定のバインド モードは、そのプロパティがデータ バインディング ターゲットのモードを示します。

などのほとんどのプロパティの既定のバインド モード`Rotation`、 `Scale`、および`Opacity`は`OneWay`します。 これらのプロパティは、データ バインドのターゲットは、ときに、ターゲット プロパティは、ソースから設定されます。

ただしの既定のバインド モード、`Value`プロパティの`Slider`は`TwoWay`します。 つまり、`Value`プロパティは、データ バインディングのターゲットとし、ターゲットがソースから (通常どおり) 設定しますが、ターゲットからソースを設定しても。 これは、ことができます、`Slider`最初から設定する`Opacity`値。

この双方向のバインドが、無限ループを作成するかもしれませんが、自動的に開かない。 バインド可能なプロパティは、実際には、プロパティを変更しない限り、プロパティの変更を通知されません。 これには、無限ループができないようにします。

### <a name="two-way-bindings"></a>双方向のバインディング

最もバインド可能なプロパティの既定のバインド モードがある`OneWay`、次のプロパティの既定のバインド モードが、 `TwoWay`:

- `Date` プロパティ `DatePicker`
- `Text` プロパティの`Editor`、 `Entry`、`SearchBar`と `EntryCell`
- `IsRefreshing` プロパティ `ListView`
- `SelectedItem` プロパティ `MultiPage`
- `SelectedIndex` `SelectedItem`のプロパティ `Picker`
- `Value` プロパティの`Slider`と `Stepper`
- `IsToggled` プロパティ `Switch`
- `On` プロパティ `SwitchCell`
- `Time` プロパティ `TimePicker`

これらの特定のプロパティとして定義されます`TwoWay`は非常に十分な理由。

モデル-ビュー-ビューモデル (MVVM) アプリケーションのアーキテクチャでデータ バインドを使用している場合、データ バインディング ソースと、ビューなどのビューで構成される、ViewModel クラスは、 `Slider`、データ バインディングのターゲットします。 MVVM のバインドのように、**バインド リバース**サンプルよりも前のサンプル内のバインディング。 ViewModel で対応するプロパティの値で初期化されるページで、それぞれのビューがビュー内の変更は、ViewModel のプロパティにも影響する必要がありますに非常に高くなります。

プロパティの既定のバインド モードで`TwoWay`MVVM シナリオで使用される最も可能性の高いこれらのプロパティします。

### <a name="one-way-to-source-bindings"></a>うちの 1-方向からソースへのバインド

読み取り専用のバインド可能なプロパティの既定のバインド モードがある`OneWayToSource`します。 既定のバインド モードになっている 1 つだけの読み取り/書き込みのバインド可能なプロパティがある`OneWayToSource`:

- `SelectedItem` プロパティ `ListView`

"The rationale"はバインドに、`SelectedItem`バインディング ソースを設定するプロパティが発生する必要があります。 この記事で後述する例では、その動作をオーバーライドします。

### <a name="one-time-bindings"></a>1 回限りのバインド

いくつかのプロパティの既定のバインド モードがある`OneTime`します。 これらの数値は、次のとおりです。

- `IsTextPredictionEnabled` プロパティ `Entry`
- `Text`、 `BackgroundColor`、および`Style`プロパティの`Span`します。

バインド モードを使ってプロパティをターゲット`OneTime`バインド コンテキストが変更された場合にのみ更新されます。 これらのターゲット プロパティのバインディング、こうなりますバインド インフラストラクチャ、ソースのプロパティの変更を監視する必要はありませんので。

## <a name="viewmodels-and-property-change-notifications"></a>Viewmodel とプロパティ変更通知

**単純なカラー セレクター**ページは、単純な ViewModel の使用を示します。 データ バインドを使用して 3 つの色を選択するユーザーを許可する`Slider`色合い、鮮やかさ、および明るさの要素。

ViewModel は、データ バインディングのソースです。 ViewModel は*いない*バインド可能なプロパティを定義しますが、により、プロパティの値が変更されたときに通知するバインド インフラストラクチャに通知メカニズムを実装しています。 この通知メカニズムは、 [ `INotifyPropertyChanged` ](xref:System.ComponentModel.INotifyPropertyChanged)をという名前の 1 つのプロパティを定義するインターフェイス[ `PropertyChanged`](xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged)します。 通常、このインターフェイスを実装するクラスは、そのパブリック プロパティのいずれかの値が変更されたときのイベントを発生させます。 イベントは、プロパティが変更しない場合に発生する必要はありません。 (、`INotifyPropertyChanged`インターフェイスはによって実装も`BindableObject`と`PropertyChanged`バインド可能なプロパティ値が変更されるたびにイベントが発生します)。

`HslColorViewModel`クラスは、5 つのプロパティを定義します。 `Hue`、 `Saturation`、 `Luminosity`、および`Color`プロパティは相互に関連します。 色コンポーネントの変更の値の 3 つのいずれかの場合、`Color`プロパティが再計算と`PropertyChanged`4 つのプロパティをすべてのイベントが発生しました。

```csharp
public class HslColorViewModel : INotifyPropertyChanged
{
    Color color;
    string name;

    public event PropertyChangedEventHandler PropertyChanged;

    public double Hue
    {
        set
        {
            if (color.Hue != value)
            {
                Color = Color.FromHsla(value, color.Saturation, color.Luminosity);
            }
        }
        get
        {
            return color.Hue;
        }
    }

    public double Saturation
    {
        set
        {
            if (color.Saturation != value)
            {
                Color = Color.FromHsla(color.Hue, value, color.Luminosity);
            }
        }
        get
        {
            return color.Saturation;
        }
    }

    public double Luminosity
    {
        set
        {
            if (color.Luminosity != value)
            {
                Color = Color.FromHsla(color.Hue, color.Saturation, value);
            }
        }
        get
        {
            return color.Luminosity;
        }
    }

    public Color Color
    {
        set
        {
            if (color != value)
            {
                color = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Hue"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Saturation"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Luminosity"));
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Color"));

                Name = NamedColor.GetNearestColorName(color);
            }
        }
        get
        {
            return color;
        }
    }

    public string Name
    {
        private set
        {
            if (name != value)
            {
                name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Name"));
            }
        }
        get
        {
            return name;
        }
    }
}
```

ときに、`Color`プロパティの変更、静的な`GetNearestColorName`メソッドで、`NamedColor`クラス (も含まれています、 **DataBindingDemos**ソリューション) 最も近い名前付きの色を取得し、設定、`Name`プロパティ。 これは、`Name`プロパティがプライベート`set`アクセサー、ため、クラスの外部から設定することはできません。

バインド インフラストラクチャにハンドラーをアタッチ、ViewModel がバインディング ソースとして設定されている場合、`PropertyChanged`イベント。 この方法では、バインディングは、プロパティの変更通知を受け取ることができ、変更された値からターゲットのプロパティを設定することができます。

ただし、ときに、ターゲット プロパティ (または`Binding`ターゲット プロパティで定義) が、`BindingMode`の`OneTime`、バインド インフラストラクチャにハンドラーをアタッチする必要はありません、`PropertyChanged`イベント。 ターゲット プロパティが更新された場合にのみ、`BindingContext`変更していないソース プロパティ自体が変更されたとき。

**単純なカラー セレクター** XAML ファイルのインスタンスを作成、`HslColorViewModel`でページのリソース ディクショナリを初期化します、`Color`プロパティ。 `BindingContext`のプロパティ、`Grid`に設定されている、`StaticResource`バインド拡張機能をそのリソースを参照します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SimpleColorSelectorPage">

    <ContentPage.Resources>
        <ResourceDictionary>
            <local:HslColorViewModel x:Key="viewModel"
                                     Color="MediumTurquoise" />

            <Style TargetType="Slider">
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>

    <Grid BindingContext="{StaticResource viewModel}">
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <BoxView Color="{Binding Color}"
                 Grid.Row="0" />

        <StackLayout Grid.Row="1"
                     Margin="10, 0">

            <Label Text="{Binding Name}"
                   HorizontalTextAlignment="Center" />

            <Slider Value="{Binding Hue}" />

            <Slider Value="{Binding Saturation}" />

            <Slider Value="{Binding Luminosity}" />
        </StackLayout>
    </Grid>
</ContentPage>
```

`BoxView`、 `Label`、3 つと`Slider`ビューからのバインディング コンテキストを継承する、`Grid`します。 これらのビューは、ソース、ViewModel のプロパティを参照するすべてのバインドのターゲットです。 `Color`のプロパティ、 `BoxView`、および`Text`のプロパティ、 `Label`、データ バインドは`OneWay`: ビューモデルのプロパティから、ビューのプロパティが設定されます。

`Value`のプロパティ、 `Slider`、ただし、`TwoWay`します。 これにより、各`Slider`、ViewModel からおよび各から設定するビューモデルに設定する`Slider`します。

プログラムを最初に実行時に、 `BoxView`、 `Label`、し、次の 3 つ`Slider`要素は、最初に基づく ViewModel からすべてのセット`Color`プロパティは、ViewModel がインスタンス化されたときに設定します。 これは、左側にある iOS のスクリーン ショットに示されます。

[![単純な色セレクター](binding-mode-images/simplecolorselector-small.png "単純な色セレクター")](binding-mode-images/simplecolorselector-large.png#lightbox "単純な色セレクター")

スライダーを操作するとき、`BoxView`と`Label`Android、UWP のスクリーン ショットに示すように、それに応じて更新されます。

リソース ディクショナリに ViewModel のインスタンス化は、1 つの一般的なアプローチです。 ViewModel のプロパティ要素タグ内のインスタンスを作成することも、`BindingContext`プロパティ。 **単純なカラー セレクター** XAML ファイルを削除してみてください、`HslColorViewModel`リソース ディクショナリからに設定し、`BindingContext`のプロパティ、`Grid`次のように。

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>

    ···

</Grid>
```

さまざまな方法では、バインディング コンテキストを設定できます。 場合によっては、分離コード ファイルは、ViewModel のインスタンスを作成しに設定、`BindingContext`ページのプロパティ。 これらは、すべての有効なアプローチです。

## <a name="overriding-the-binding-mode"></a>バインド モードをオーバーライドします。

ターゲット プロパティに既定のバインド モードが特定のデータ バインドに適していない場合は、設定によって上書きすること、 [ `Mode` ](xref:Xamarin.Forms.BindingBase.Mode)プロパティの`Binding`(または[ `Mode` ](xref:Xamarin.Forms.Xaml.BindingExtension.Mode)のプロパティ、`Binding`マークアップ拡張機能) のメンバーの 1 つに、`BindingMode`列挙体。

ただし、設定、`Mode`プロパティを`TwoWay`ご想像どおりに動作しません。 などを変更してみて、**代わりに XAML バインディング**XAML ファイルに含める`TwoWay`バインディングの定義で。

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

予想されますが、`Slider`の初期値に初期化すると、`Scale`プロパティを 1 に設定されているが、自動的に開かない。 ときに、`TwoWay`バインドが初期化されて、ターゲットがソースからの設定、最初に、です。 つまり、`Scale`プロパティに設定されて、`Slider`既定値は 0。 ときに、`TwoWay`バインドを設定、 `Slider`、`Slider`ソースから最初に設定されます。

バインド モードを設定することができます`OneWayToSource`で、**代わりに XAML バインディング**サンプル。

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=OneWayToSource}" />
```

今すぐ、 `Slider` 1 に初期化されます (の既定値`Scale`) 操作ですが、`Slider`には影響しません、`Scale`プロパティ、これは非常に便利ではありません。

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement)クラスも定義[ `ScaleX` ](xref:Xamarin.Forms.VisualElement.ScaleX)と[ `ScaleY` ](xref:Xamarin.Forms.VisualElement.ScaleY)プロパティ、拡張することが、`VisualElement`異なる方法で、水平および垂直方向。

既定のバインド モードをオーバーライドする側の非常に便利なアプリケーション`TwoWay`では、`SelectedItem`プロパティの`ListView`します。 既定のバインド モードは`OneWayToSource`します。 データ バインディングを設定すると、`SelectedItem`からそのソースのプロパティを設定し、ViewModel でソースのプロパティを参照するプロパティを`ListView`選択します。 ただし、状況によっては、することも、 `ListView` ViewModel から初期化します。

**サンプル設定**ページは、この手法を示します。 このページは、このような ViewModel で定義されている非常に多くの場合、アプリケーション設定の単純な実装を表します`SampleSettingsViewModel`ファイル。

```csharp
public class SampleSettingsViewModel : INotifyPropertyChanged
{
    string name;
    DateTime birthDate;
    bool codesInCSharp;
    double numberOfCopies;
    NamedColor backgroundNamedColor;

    public event PropertyChangedEventHandler PropertyChanged;

    public SampleSettingsViewModel(IDictionary<string, object> dictionary)
    {
        Name = GetDictionaryEntry<string>(dictionary, "Name");
        BirthDate = GetDictionaryEntry(dictionary, "BirthDate", new DateTime(1980, 1, 1));
        CodesInCSharp = GetDictionaryEntry<bool>(dictionary, "CodesInCSharp");
        NumberOfCopies = GetDictionaryEntry(dictionary, "NumberOfCopies", 1.0);
        BackgroundNamedColor = NamedColor.Find(GetDictionaryEntry(dictionary, "BackgroundNamedColor", "White"));
    }

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public DateTime BirthDate
    {
        set { SetProperty(ref birthDate, value); }
        get { return birthDate; }
    }

    public bool CodesInCSharp
    {
        set { SetProperty(ref codesInCSharp, value); }
        get { return codesInCSharp; }
    }

    public double NumberOfCopies
    {
        set { SetProperty(ref numberOfCopies, value); }
        get { return numberOfCopies; }
    }

    public NamedColor BackgroundNamedColor
    {
        set
        {
            if (SetProperty(ref backgroundNamedColor, value))
            {
                OnPropertyChanged("BackgroundColor");
            }
        }
        get { return backgroundNamedColor; }
    }

    public Color BackgroundColor
    {
        get { return BackgroundNamedColor?.Color ?? Color.White; }
    }

    public void SaveState(IDictionary<string, object> dictionary)
    {
        dictionary["Name"] = Name;
        dictionary["BirthDate"] = BirthDate;
        dictionary["CodesInCSharp"] = CodesInCSharp;
        dictionary["NumberOfCopies"] = NumberOfCopies;
        dictionary["BackgroundNamedColor"] = BackgroundNamedColor.Name;
    }

    T GetDictionaryEntry<T>(IDictionary<string, object> dictionary, string key, T defaultValue = default(T))
    {
        return dictionary.ContainsKey(key) ? (T)dictionary[key] : defaultValue;
    }

    bool SetProperty<T>(ref T storage, T value, [CallerMemberName] string propertyName = null)
    {
        if (Object.Equals(storage, value))
            return false;

        storage = value;
        OnPropertyChanged(propertyName);
        return true;
    }

    protected void OnPropertyChanged([CallerMemberName] string propertyName = null)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

各アプリケーション設定がという名前のメソッドで Xamarin.Forms プロパティ ディクショナリに保存されているプロパティ`SaveState`コンス トラクターでは、そのディクショナリから読み込まれたとします。 クラスの下部に、ViewModels を効率化し、エラーが発生しにくくように 2 つのメソッドには。 `OnPropertyChanged`下部にあるメソッドが呼び出し元のプロパティに設定されているオプションのパラメーター。 これにより、文字列として、プロパティの名前を指定するときに、スペル ミスを回避できます。

`SetProperty`クラスのメソッドには: フィールドとして格納されている値をプロパティに設定される値が比較され、のみを呼び出して`OnPropertyChanged`2 つの値が等しくない場合。

`SampleSettingsViewModel`クラスの背景色の 2 つのプロパティを定義します。、`BackgroundNamedColor`プロパティの型は`NamedColor`、クラスも含まれているで、 **DataBindingDemos**ソリューション。 `BackgroundColor`プロパティの型は`Color`から取得し、`Color`のプロパティ、`NamedColor`オブジェクト。

`NamedColor`クラスは .NET リフレクションを使用して、Xamarin.Forms で static public フィールドを列挙する`Color`構造、および静的なからアクセス可能なコレクションの名前と共に保存する`All`プロパティ。

```csharp
public class NamedColor : IEquatable<NamedColor>, IComparable<NamedColor>
{
    // Instance members
    private NamedColor()
    {
    }

    public string Name { private set; get; }

    public string FriendlyName { private set; get; }

    public Color Color { private set; get; }

    public string RgbDisplay { private set; get; }

    public bool Equals(NamedColor other)
    {
        return Name.Equals(other.Name);
    }

    public int CompareTo(NamedColor other)
    {
        return Name.CompareTo(other.Name);
    }

    // Static members
    static NamedColor()
    {
        List<NamedColor> all = new List<NamedColor>();
        StringBuilder stringBuilder = new StringBuilder();

        // Loop through the public static fields of the Color structure.
        foreach (FieldInfo fieldInfo in typeof(Color).GetRuntimeFields())
        {
            if (fieldInfo.IsPublic &&
                fieldInfo.IsStatic &&
                fieldInfo.FieldType == typeof(Color))
            {
                // Convert the name to a friendly name.
                string name = fieldInfo.Name;
                stringBuilder.Clear();
                int index = 0;

                foreach (char ch in name)
                {
                    if (index != 0 && Char.IsUpper(ch))
                    {
                        stringBuilder.Append(' ');
                    }
                    stringBuilder.Append(ch);
                    index++;
                }

                // Instantiate a NamedColor object.
                Color color = (Color)fieldInfo.GetValue(null);

                NamedColor namedColor = new NamedColor
                {
                    Name = name,
                    FriendlyName = stringBuilder.ToString(),
                    Color = color,
                    RgbDisplay = String.Format("{0:X2}-{1:X2}-{2:X2}",
                                                (int)(255 * color.R),
                                                (int)(255 * color.G),
                                                (int)(255 * color.B))
                };

                // Add it to the collection.
                all.Add(namedColor);
            }
        }
        all.TrimExcess();
        all.Sort();
        All = all;
    }

    public static IList<NamedColor> All { private set; get; }

    public static NamedColor Find(string name)
    {
        return ((List<NamedColor>)All).Find(nc => nc.Name == name);
    }

    public static string GetNearestColorName(Color color)
    {
        double shortestDistance = 1000;
        NamedColor closestColor = null;

        foreach (NamedColor namedColor in NamedColor.All)
        {
            double distance = Math.Sqrt(Math.Pow(color.R - namedColor.Color.R, 2) +
                                        Math.Pow(color.G - namedColor.Color.G, 2) +
                                        Math.Pow(color.B - namedColor.Color.B, 2));

            if (distance < shortestDistance)
            {
                shortestDistance = distance;
                closestColor = namedColor;
            }
        }
        return closestColor.Name;
    }
}
```

`App`クラス、 **DataBindingDemos**プロジェクトという名前のプロパティを定義する`Settings`型の`SampleSettingsViewModel`します。 このプロパティが初期化されるときに、`App`クラスをインスタンス化すると、および`SaveState`メソッドが呼び出されます、`OnSleep`メソッドが呼び出されます。

```csharp
public partial class App : Application
{
    public App()
    {
        InitializeComponent();

        Settings = new SampleSettingsViewModel(Current.Properties);

        MainPage = new NavigationPage(new MainPage());
    }

    public SampleSettingsViewModel Settings { private set; get; }

    protected override void OnStart()
    {
        // Handle when your app starts
    }

    protected override void OnSleep()
    {
        // Handle when your app sleeps
        Settings.SaveState(Current.Properties);
    }

    protected override void OnResume()
    {
        // Handle when your app resumes
    }
}
```

アプリケーションのライフ サイクル メソッドの詳細については、記事を参照してください。 [**アプリのライフ サイクル**](~/xamarin-forms/app-fundamentals/app-lifecycle.md)します。

他のほとんどの処理、 **SampleSettingsPage.xaml**ファイル。 `BindingContext`ページの設定を使用して、`Binding`マークアップ拡張機能: バインディング ソースは、静的な`Application.Current`インスタンスであるプロパティの`App`プロジェクトで、クラス、`Path`に設定されている、 `Settings`これは、プロパティ、`SampleSettingsViewModel`オブジェクト。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.SampleSettingsPage"
             Title="Sample Settings"
             BindingContext="{Binding Source={x:Static Application.Current},
                                      Path=Settings}">

    <StackLayout BackgroundColor="{Binding BackgroundColor}"
                 Padding="10"
                 Spacing="10">

        <StackLayout Orientation="Horizontal">
            <Label Text="Name: "
                   VerticalOptions="Center" />

            <Entry Text="{Binding Name}"
                   Placeholder="your name"
                   HorizontalOptions="FillAndExpand"
                   VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Birth Date: "
                   VerticalOptions="Center" />

            <DatePicker Date="{Binding BirthDate}"
                        HorizontalOptions="FillAndExpand"
                        VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Do you code in C#? "
                   VerticalOptions="Center" />

            <Switch IsToggled="{Binding CodesInCSharp}"
                    VerticalOptions="Center" />
        </StackLayout>

        <StackLayout Orientation="Horizontal">
            <Label Text="Number of Copies: "
                   VerticalOptions="Center" />

            <Stepper Value="{Binding NumberOfCopies}"
                     VerticalOptions="Center" />

            <Label Text="{Binding NumberOfCopies}"
                   VerticalOptions="Center" />
        </StackLayout>

        <Label Text="Background Color:" />

        <ListView x:Name="colorListView"
                  ItemsSource="{x:Static local:NamedColor.All}"
                  SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
                  VerticalOptions="FillAndExpand"
                  RowHeight="40">
            <ListView.ItemTemplate>
                <DataTemplate>
                    <ViewCell>
                        <StackLayout Orientation="Horizontal">
                            <BoxView Color="{Binding Color}"
                                     HeightRequest="32"
                                     WidthRequest="32"
                                     VerticalOptions="Center" />

                            <Label Text="{Binding FriendlyName}"
                                   FontSize="24"
                                   VerticalOptions="Center" />
                        </StackLayout>                        
                    </ViewCell>
                </DataTemplate>
            </ListView.ItemTemplate>
        </ListView>
    </StackLayout>
</ContentPage>
```

ページのすべての子では、バインディング コンテキストを継承します。 このページで、その他のバインディングのほとんどは、プロパティに`SampleSettingsViewModel`します。 `BackgroundColor`プロパティを設定するため、`BackgroundColor`のプロパティ、 `StackLayout`、および`Entry`、 `DatePicker`、 `Switch`、および`Stepper`プロパティはすべて、ViewModel で他のプロパティにバインドします。

`ItemsSource`のプロパティ、 `ListView` 、静的に設定されている`NamedColor.All`プロパティ。 これによって、入力、`ListView`すべて、`NamedColor`インスタンス。 各項目に対して、 `ListView`、品目のバインディング コンテキストに設定されて、`NamedColor`オブジェクト。 `BoxView`と`Label`で、`ViewCell`でプロパティにバインドされた`NamedColor`します。

`SelectedItem`のプロパティ、`ListView`の種類は`NamedColor`にバインドし、`BackgroundNamedColor`プロパティの`SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

既定のバインド モード`SelectedItem`は`OneWayToSource`、選択した項目から ViewModel プロパティを設定します。 `TwoWay`モードにより、 `SelectedItem` ViewModel から初期化します。

ただし、ときに、`SelectedItem`この方法で設定されている、`ListView`自動的に選択したアイテムを表示するスクロールしません。 分離コード ファイルで、少しのコードは、必要があります。

```csharp
public partial class SampleSettingsPage : ContentPage
{
    public SampleSettingsPage()
    {
        InitializeComponent();

        if (colorListView.SelectedItem != null)
        {
            colorListView.ScrollTo(colorListView.SelectedItem,
                                   ScrollToPosition.MakeVisible,
                                   false);
        }
    }
}
```

左側にある iOS のスクリーン ショットは、最初の実行時にプログラムを示します。 コンス トラクター`SampleSettingsViewModel`を初期化します、白の背景色し、で選択した内容は、 `ListView`:

[![設定のサンプル](binding-mode-images/samplesettings-small.png "設定のサンプル")](binding-mode-images/samplesettings-large.png#lightbox "設定のサンプル")

その他の 2 つのスクリーン ショットは、変更の設定を表示します。 このページを試す場合は、プログラムのスリープ状態し、デバイスまたはが実行されているエミュレーターに終了状態に注意してください。 Visual Studio デバッガーからプログラムを終了していますが、`OnSleep`で上書き、`App`呼び出されるクラス。

次の記事で指定する方法が紹介[**文字列の書式設定**](string-formatting.md)に設定されているデータ バインドの`Text`プロパティの`Label`します。


## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms book からデータ バインド」の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
