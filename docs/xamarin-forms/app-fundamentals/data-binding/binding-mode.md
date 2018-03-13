---
title: "バインド モード"
description: "ソースとターゲット間の情報のフローを制御します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: D087C389-2E9E-47B9-A341-5B14AC732C45
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 887dc3cf710fb75d05d02af179bc218c15d31f97
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="binding-mode"></a>バインド モード

[前の記事](basic-bindings.md)、**代替コード バインド**と**代わりに XAML バインド**ページ機能を備えた、`Label`でその`Scale`プロパティバインドされている、`Value`のプロパティ、`Slider`です。 `Slider`初期値が 0 の場合これの原因となった、`Scale`のプロパティ、 `Label` 1 ではなく 0 に設定して、`Label`消失しました。

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

最初は、これに思えます後方: ようになりました、`Label`データ バインド ソースは、および`Slider`ターゲットであります。 バインドの参照、`Opacity`のプロパティ、 `Label`1 の既定値を持ちます。

ご想像、`Slider`初期値を 1 に初期化される`Opacity`の値`Label`です。 左側の iOS のスクリーン ショットにこれが表示されます。

[![バインドを反転](binding-mode-images/reversebinding-small.png "バインドを反転")](binding-mode-images/reversebinding-large.png#lightbox "逆バインディング")

必要がある意外に思いました、`Slider`は引き続き動作、Android および UWP スクリーン ショットが示すようにします。 これは、場合にお勧めのデータ バインドの動作を提案しているよう、`Slider`バインディング ターゲットではなく、`Label`予定であることと同じように、初期化が動作するためです。

間の違い、**逆バインディング**サンプルと前のサンプルでは、*バインド モード*です。

## <a name="the-default-binding-mode"></a>既定のバインド モード

メンバーでは、バインディング モードを指定してください。、 [ `BindingMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BindingMode/)列挙します。 

- [`Default`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.Default/) 
- [`TwoWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.TwoWay/) &ndash; データは、ソースとターゲット間の双方向を配置します。
- [`OneWay`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWay/) &ndash; データは、ソースからターゲットになった
- [`OneWayToSource`](https://developer.xamarin.com/api/field/Xamarin.Forms.BindingMode.OneWayToSource/) &ndash; データがターゲットからソースへ移動します。

すべてのバインド可能なプロパティが既定のバインド、バインド可能なプロパティが作成されるときに設定されているモードとはから利用可能な[ `DefaultBindingMode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableProperty.DefaultBindingMode/)のプロパティ、`BindableProperty`オブジェクト。 この既定のバインド モードは、そのプロパティがデータ バインディング ターゲットのモードを示します。

など、ほとんどのプロパティの既定のバインド モード`Rotation`、 `Scale`、および`Opacity`は`OneWay`します。 これらのプロパティがデータ バインドのターゲットの場合は、元の target プロパティが設定されます。

ただしの既定のバインド モード、`Value`プロパティ`Slider`は`TwoWay`します。 つまり、`Value`プロパティは、データ バインディングのターゲットとし、(通常どおりに) ターゲットがソースから設定が、ターゲットからソースを設定してもします。 これは、`Slider`を最初から設定する`Opacity`値。

この双方向のバインドが、無限ループを作成するように思えるかもしれませんが、するは発生しません。 バインド可能なプロパティは、プロパティが実際に変更しない限り、プロパティの変更をシグナルされません。 これは、無限ループを防ぎます。

### <a name="two-way-bindings"></a>双方向のバインディング

最もバインド可能なプロパティの既定のバインド モードを持っている`OneWay`が次のプロパティの既定のバインド モード`TwoWay`:

- `Date` プロパティ `DatePicker`
- `Text` プロパティの`Editor`、 `Entry`、 `SearchBar`、および `EntryCell`
- `IsRefreshing` プロパティ `ListView`
- `SelectedItem` プロパティ `MultiPage`
- `SelectedIndex` および`SelectedItem`のプロパティ `Picker`
- `Value` プロパティの`Slider`と `Stepper`
- `IsToggled` プロパティ `Switch` 
- `On` プロパティ `SwitchCell`
- `Time` プロパティ `TimePicker`

これらの特定のプロパティとして定義されている`TwoWay`の非常に良い理由。 

ViewModel クラスは、データ バインディング ソース、およびビューのように構成されると、ビュー モデル View-viewmodel (MVVM) アプリケーションのアーキテクチャとデータ バインディングを使用している場合`Slider`、データ バインディングのターゲットです。 MVVM バインドのようになります、**バインド反転**前のサンプルのバインディングより多くのサンプルです。 各ページのビューに、ViewModel に対応するプロパティの値で初期化されるがビュー内の変更は、ViewModel プロパティにも影響が非常に高くなります。

プロパティの既定のバインド モードと`TwoWay`される MVVM シナリオで使用される最も可能性の高いプロパティです。

### <a name="one-way-to-source-bindings"></a>一-方向からソースへのバインド

読み取り専用のバインド可能なプロパティの既定のバインド モードを持っている`OneWayToSource`です。 既定のバインド モードを持つ 1 つだけの読み取り/書き込みのバインド可能なプロパティがある`OneWayToSource`:

- `SelectedItem` プロパティ `ListView`

理論的根拠は上のバインド、`SelectedItem`プロパティがバインディング ソースを設定することになります。 この記事の後半の例では、その動作を上書きします。

## <a name="viewmodels-and-property-change-notifications"></a>ViewModels とプロパティの変更通知

**単純なカラー セレクター**ページは、単純な ViewModel の使用方法を示します。 データ バインディングを使用して 3 つの色を選択するユーザーを許可する`Slider`色合い、鮮やかさ、および明るさの要素。

ViewModel とは、データ バインディングのソースです。 ViewModel は*いない*バインド可能なプロパティを定義しますが、プロパティの値が変更されたときに通知する、バインド インフラストラクチャは、通知のメカニズムを実装しています。 この通知メカニズムは、 [ `INotifyPropertyChanged` ](https://developer.xamarin.com/api/type/System.ComponentModel.INotifyPropertyChanged/)という名前の単一プロパティを定義するインターフェイス[ `PropertyChanged`](https://developer.xamarin.com/api/event/System.ComponentModel.INotifyPropertyChanged.PropertyChanged/)です。 通常、このインターフェイスを実装するクラスは、そのパブリック プロパティのいずれかの値が変更されたときにイベントを発生させます。 イベントは、プロパティを変更しない場合に発生する必要はありません。 (、`INotifyPropertyChanged`インターフェイスが実装しても`BindableObject`と`PropertyChanged`バインド可能なプロパティ値が変更されるたびにイベントが発生します)。

`HslColorViewModel`クラスは、5 つのプロパティを定義します。、 `Hue`、 `Saturation`、 `Luminosity`、と`Color`プロパティが相互に関連します。 3 つのいずれかのカラー コンポーネント変更値、ときに、`Color`プロパティが再計算と`PropertyChanged`4 つのプロパティをすべてのイベントは発生しません。

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

ときに、`Color`プロパティが変更された、静的な`GetNearestColorName`メソッドで、`NamedColor`クラス (にも含まれて、 **DataBindingDemos**ソリューション) 名前付きの最も近い色を取得し、設定、`Name`プロパティです。 これは、`Name`プロパティにプライベート`set`アクセサー、ため、クラスの外部から設定することはできません。

バインディング ソースとして、ViewModel を設定すると、バインド インフラストラクチャは、アタッチにハンドラーを`PropertyChanged`イベント。 この方法でバインドは、プロパティに対する変更の通知を受け取ることができ、変更された値から、ターゲットのプロパティを設定することができます。

**単純なカラー セレクター** XAML ファイルのインスタンスを作成、`HslColorViewModel`でページのリソース ディクショナリを初期化します、`Color`プロパティです。 `BindingContext`のプロパティ、`Grid`に設定されている、`StaticResource`バインド拡張機能をそのリソースを参照します。

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

`BoxView`、 `Label`、3 つおよび`Slider`ビュー、バインディング コンテキストからの継承、`Grid`です。 これらのビューは、ソース、ViewModel のプロパティを参照するすべてのバインディング ターゲットです。 `Color`のプロパティ、 `BoxView`、および`Text`のプロパティ、 `Label`、データ バインディングは`OneWay`: ViewModel で指定したプロパティのビュー内のプロパティが設定されます。

`Value`のプロパティ、 `Slider`、ただし、`TwoWay`です。 これにより、各`Slider`、ViewModel および各から設定する ViewModel も設定する`Slider`です。 

プログラムが最初に実行時に、 `BoxView`、 `Label`、し、次の 3 つ`Slider`要素は、すべてのセット最初に基づく ViewModel から`Color`ViewModel がインスタンス化されるときに設定するプロパティ。 これは、左側にある iOS のスクリーン ショットで示されます。

[![単純なカラー セレクター](binding-mode-images/simplecolorselector-small.png "単純なカラー セレクター")](binding-mode-images/simplecolorselector-large.png#lightbox "単純なカラー セレクター")

スライダーを操作するとき、`BoxView`と`Label`Android および UWP スクリーン ショットに示すように、それに応じて更新されます。

リソース ディクショナリで ViewModel をインスタンス化は、1 つの一般的なアプローチです。 ViewModel プロパティ要素タグ内のインスタンスを作成することも、`BindingContext`プロパティです。 **単純なカラー セレクター** XAML ファイルを削除してください、`HslColorViewModel`リソース ディクショナリからに設定し、`BindingContext`のプロパティ、`Grid`次のように。

```xaml
<Grid>
    <Grid.BindingContext>
        <local:HslColorViewModel Color="MediumTurquoise" />
    </Grid.BindingContext>
        
    ···

</Grid>
```

さまざまな方法では、バインディング コンテキストを設定できます。 場合によっては、分離コード ファイルは、ViewModel をインスタンス化し設定、`BindingContext`ページのプロパティです。 これらは、すべての有効なアプローチです。

## <a name="overriding-the-binding-mode"></a>バインド モードをオーバーライドします。

ターゲット プロパティに既定のバインド モードが特定のデータ バインディングに適していない場合は、可能であればそれをオーバーライドするには、 [ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindingBase.Mode/)プロパティ`Binding`(または[ `Mode` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Xaml.BindingExtension.Mode/)のプロパティ、`Binding`マークアップ拡張機能) のメンバーのいずれかに、`BindingMode`列挙します。

ただし、設定、`Mode`プロパティを`TwoWay`期待どおりに常に機能しません。 などを変更してください、**代わりに XAML バインディング**XAML ファイルに含める`TwoWay`バインディングの定義で。

```xaml
<Label Text="TEXT"
       FontSize="40"
       HorizontalOptions="Center"
       VerticalOptions="CenterAndExpand"
       Scale="{Binding Source={x:Reference slider},
                       Path=Value,
                       Mode=TwoWay}" />
```

予想される場合がありますを`Slider`の初期値に初期化すると、`Scale`プロパティを 1 に設定されているが、するは発生しません。 ときに、`TwoWay`バインドが初期化されて、ターゲットに設定されたソースから最初に、つまり、`Scale`プロパティに設定されている、`Slider`既定値は 0 です。 ときに、`TwoWay`にバインディングが設定されて、 `Slider`、`Slider`ソースから最初に設定されます。

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

今すぐ、`Slider`は 1 に初期化 (の既定値`Scale`) が操作に使用する、`Slider`には影響しません、`Scale`プロパティのため、これは非常に便利ではありません。

既定のバインド モードをオーバーライドする非常に便利なアプリケーション`TwoWay`では、`SelectedItem`プロパティの`ListView`します。 既定のバインド モードは`OneWayToSource`します。 データ バインディングを設定すると、`SelectedItem`からそのソースのプロパティを設定し、ViewModel でソース プロパティを参照するプロパティ、`ListView`選択します。 ただし、状況によっては、することも、 `ListView` ViewModel から初期化されるようにします。

**サンプル設定**ページは、この手法を示します。 このページは、このような ViewModel で定義されているほとんどの場合、アプリケーションの設定の簡単な実装を表す`SampleSettingsViewModel`ファイル。

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

各アプリケーション設定は、という名前のメソッドで Xamarin.Forms プロパティ辞書に保存されているプロパティ`SaveState`コンス トラクターでは、そのディクショナリから読み込まれたとします。 クラスの一番下まで ViewModels を効率化し、間違いを犯しにくいように 2 つのメソッドは、します。 `OnPropertyChanged`下部にあるメソッドが呼び出し元のプロパティに設定されているオプションのパラメーターです。 これにより、文字列として、プロパティの名前を指定する場合、スペル ミスを回避できます。 

`SetProperty`メソッドがクラスにはさらに多く: フィールドとして格納されている値をプロパティに設定されている値を比較し、のみを呼び出して`OnPropertyChanged`ときに 次の 2 つの値が等しくないです。 

`SampleSettingsViewModel`クラスの背景色の 2 つのプロパティを定義します。、`BackgroundNamedColor`プロパティの型は`NamedColor`、クラスもに含まれている、 **DataBindingDemos**ソリューションです。 `BackgroundColor`プロパティの型は`Color`から取得されると、`Color`のプロパティ、`NamedColor`オブジェクト。

`NamedColor`クラスでは、.NET リフレクションを使用して、すべての静的パブリック フィールド、xamarin.forms を列挙`Color`構造、および静的からアクセス可能なコレクション内の名と共に保存する`All`プロパティ。

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

`App`クラス内で、 **DataBindingDemos**という名前のプロパティを定義するプロジェクト`Settings`型の`SampleSettingsViewModel`します。 このプロパティが初期化されるときに、`App`クラスをインスタンス化すると、および`SaveState`メソッドが呼び出されます、`OnSleep`メソッドが呼び出されます。

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

アプリケーションのライフ サイクル メソッドの詳細については、記事を参照してください。 [**アプリのライフ サイクル**](~/xamarin-forms/app-fundamentals/app-lifecycle.md)です。

他のほとんどを処理、 **SampleSettingsPage.xaml**ファイル。 `BindingContext`ページの設定を使用して、`Binding`マークアップ拡張機能: バインド ソースは、静的な`Application.Current`には、インスタンス プロパティの`App`、プロジェクト内のクラスと`Path`に設定されている、 `Settings`プロパティである、`SampleSettingsViewModel`オブジェクト。

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

ページのすべての子では、バインディング コンテキストを継承します。 このページで、その他のバインディングのほとんどのプロパティには、`SampleSettingsViewModel`です。 `BackgroundColor`プロパティが設定に使用される、`BackgroundColor`のプロパティ、 `StackLayout`、および`Entry`、 `DatePicker`、 `Switch`、および`Stepper`プロパティはすべて、ViewModel でその他のプロパティにバインドします。

`ItemsSource`のプロパティ、 `ListView` 、静的に設定されている`NamedColor.All`プロパティです。 これによって、入力、`ListView`すべて、`NamedColor`インスタンス。 項目ごとに、 `ListView`、品目のバインディング コンテキストに設定されて、`NamedColor`オブジェクト。 `BoxView`と`Label`で、`ViewCell`でプロパティにバインドされた`NamedColor`です。

`SelectedItem`のプロパティ、`ListView`の種類は`NamedColor`にバインドし、`BackgroundNamedColor`プロパティの`SampleSettingsViewModel`:

```xaml
SelectedItem="{Binding BackgroundNamedColor, Mode=TwoWay}"
```

既定のバインド モード`SelectedItem`は`OneWayToSource`、選択項目から ViewModel プロパティを設定します。 `TwoWay`モードでは、 `SelectedItem` ViewModel から初期化されるようにします。 

ただし、ときに、`SelectedItem`この方法で設定されている、`ListView`自動的に選択したアイテムを表示するスクロールしません。 分離コード ファイルにコードを少し必要があります。

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

左側にある iOS スクリーン ショットは、最初の実行時にプログラムを示します。 コンス トラクター`SampleSettingsViewModel`白に初期化、背景色をで選択した内容は、 `ListView`:

[![設定のサンプル](binding-mode-images/samplesettings-small.png "設定のサンプル")](binding-mode-images/samplesettings-large.png#lightbox "設定のサンプル")

その他の 2 つのスクリーン ショットは、変更された設定を表示します。 このページを試す場合は、スリープ状態や、デバイスまたはエミュレーターが実行されていることに終了するプログラムを入力してください。 Visual Studio デバッガーからプログラムを終了してされません、`OnSleep`内の上書き、`App`クラスを呼び出します。

次の記事の内容を指定する方法が分かります[**文字列の書式設定**](string-formatting.md)に設定されているデータ バインディングの`Text`プロパティの`Label`します。


## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 帳からのデータ バインディング章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter16.md)
