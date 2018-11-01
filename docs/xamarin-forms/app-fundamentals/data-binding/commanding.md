---
title: Xamarin.Forms コマンド インターフェイス
description: この記事では、Xamarin.Forms のデータ バインドとコマンド プロパティを実装する方法について説明します。 コマンド実行のインターフェイスは、MVVM アーキテクチャに適しているコマンドの実装に代わるアプローチを提供します。
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 68c7869254ae861cef8307431d925368082be921
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675256"
---
# <a name="the-xamarinforms-command-interface"></a>Xamarin.Forms コマンド インターフェイス

これは一般にから派生したクラス、ビューモデルのプロパティ間アーキテクチャでは、モデル-ビュー-ビューモデル (MVVM)、データ バインディングが定義されている`INotifyPropertyChanged`、し、これは一般に、XAML ファイルと、ビューのプロパティ。 場合があります、アプリケーションでは、ビューモデルで何かに影響するコマンドを開始するユーザーを要求することで、これらのプロパティのバインドを超える必要があります。 従来のハンドラーで分離コード ファイルで処理されるとこれらのコマンド ボタンのクリックによって通知は通常、または本の指でタップ、`Clicked`のイベント、`Button`または`Tapped`のイベントを`TapGestureRecognizer`します。

コマンド実行のインターフェイスは、MVVM アーキテクチャに適しているコマンドの実装に代わるアプローチを提供します。 ViewModel 自体は、コマンド、ビューで特定のアクティビティに反応などが実行されるメソッドを含めることができます、 `Button`  をクリックします。 これらのコマンドの間でデータ バインドの定義と`Button`します。

間のデータ バインディングを許可する、 `Button` 、ViewModel、 `Button` 2 つのプロパティを定義します。

- [`Command`](xref:Xamarin.Forms.Button.Command) 型の [`System.Windows.Input.ICommand`](xref:System.Windows.Input.ICommand)
- [`CommandParameter`](xref:Xamarin.Forms.Button.CommandParameter) 型の `Object`

対象とするデータ バインドを定義するコマンド インターフェイスを使用する、`Command`のプロパティ、`Button`型の ViewModel のプロパティのソースの`ICommand`します。 ビューモデルには、関連するコードが含まれています。`ICommand`ボタンがクリックされたときに実行されるプロパティ。 設定できる`CommandParameter`すべてしている場合、複数のボタンを区別するために任意のデータを同じバインド`ICommand`ビューモデルのプロパティ。

`Command`と`CommandParameter`プロパティは、次のクラスによっても定義します。

- [`MenuItem`](xref:Xamarin.Forms.MenuItem) そのため、および[ `ToolbarItem`](xref:Xamarin.Forms.ToolbarItem)から派生します。 `MenuItem`
- [`TextCell`](xref:Xamarin.Forms.TextCell) そのため、および[ `ImageCell`](xref:Xamarin.Forms.ImageCell)から派生します。 `TextCell`
- [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer)

[`SearchBar`](xref:Xamarin.Forms.SearchBar) 定義、 [ `SearchCommand` ](xref:Xamarin.Forms.SearchBar.SearchCommand)型のプロパティ`ICommand`と[ `SearchCommandParameter` ](xref:Xamarin.Forms.SearchBar.SearchCommandParameter)プロパティ。 [ `RefreshCommand` ](xref:Xamarin.Forms.ListView.RefreshCommand)プロパティの[ `ListView` ](xref:Xamarin.Forms.ListView)型も`ICommand`します。

これらすべてのコマンドは、ビューモデル、ビュー内の特定のユーザー インターフェイス オブジェクトに依存しない方法で処理できます。

## <a name="the-icommand-interface"></a>ICommand インターフェイス

[ `System.Windows.Input.ICommand` ](xref:System.Windows.Input.ICommand)インターフェイスは、Xamarin.Forms の一部ではありません。 代わりに定義されている、 [System.Windows.Input](xref:System.Windows.Input)名前空間には、2 つのメソッドと 1 つのイベントで構成されます。

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

自分の ViewModel にはコマンド インターフェイスを使用するには型のプロパティが含まれています`ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

ビューモデルを実装するクラスを参照する必要がありますも、`ICommand`インターフェイス。 このクラスはすぐに説明します。 ビューで、`Command`のプロパティを`Button`そのプロパティにバインドされます。

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

押されたとき、 `Button`、`Button`呼び出し、`Execute`メソッドで、`ICommand`オブジェクトにバインドされてその`Command`プロパティ。 コマンド実行のインターフェイスの簡単な部分です。

`CanExecute`メソッドはより複雑です。 バインディングで最初に定義されている場合、`Command`のプロパティ、 `Button`、何らかの方法でデータ バインディングが変更されたときと、`Button`呼び出し、`CanExecute`メソッドで、`ICommand`オブジェクト。 場合`CanExecute`返します`false`、`Button`自体を無効にします。 これは、特定のコマンドが利用できない、または無効なが現在ことを示します。

`Button`もでハンドラーをアタッチ、`CanExecuteChanged`のイベント`ICommand`します。 イベントはビューモデル内から起動されます。 そのイベントが発生したときに、`Button`呼び出し`CanExecute`もう一度です。 `Button`場合を有効にする`CanExecute`返します`true`と無効になります。`CanExecute`返します`false`します。

> [!IMPORTANT]
> 使用しないでください、`IsEnabled`プロパティの`Button`コマンド インターフェイスを使用している場合。  

## <a name="the-command-class"></a>コマンド クラス

自分の ViewModel が型のプロパティを定義する場合`ICommand`、ビューモデルが含まれても、または実装するクラスを参照する必要があります、`ICommand`インターフェイス。 このクラスは、含むまたは参照する必要があります、`Execute`と`CanExecute`メソッド、および火災、`CanExecuteChanged`イベントたびに、`CanExecute`メソッドは、別の値を返す可能性があります。

このようなクラスを自分で記述できます。 または他のユーザーが記述するクラスを使用することができます。 `ICommand`一部の Microsoft Windows では、使用された Windows MVVM アプリケーションの年。 実装する Windows クラスを使用して`ICommand`Windows アプリケーションと Xamarin.Forms アプリケーションの間、ViewModels を共有することができます。

Windows と Xamarin.Forms のビューモデルの共有は、問題ではないかどうかは、使用することができます、 [ `Command` ](xref:Xamarin.Forms.Command)または[ `Command<T>` ](xref:Xamarin.Forms.Command`1) を実装するためにXamarin.Formsに含まれるクラス`ICommand`インターフェイス。 本文を指定することはこれらのクラス、`Execute`と`CanExecute`クラスのコンス トラクター内のメソッド。 使用して、`Command<T>`を使用すると、`CommandParameter`複数のビューを区別するためにプロパティに同じバインド`ICommand`プロパティ、およびより単純な`Command`クラスの要件ではありません。

## <a name="basic-commanding"></a>基本的なコマンドの実行

**Person エントリ**ページで、 [**データ バインディング デモ**](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)プログラムは、ViewModel で実装されたいくつかの単純なコマンドを示します。

`PersonViewModel`という名前の 3 つのプロパティを定義します。 `Name`、 `Age`、および`Skills`人を定義します。 このクラスは*いない*を含む`ICommand`プロパティ。

```csharp
public class PersonViewModel : INotifyPropertyChanged
{
    string name;
    double age;
    string skills;

    public event PropertyChangedEventHandler PropertyChanged;

    public string Name
    {
        set { SetProperty(ref name, value); }
        get { return name; }
    }

    public double Age
    {
        set { SetProperty(ref age, value); }
        get { return age; }
    }

    public string Skills
    {
        set { SetProperty(ref skills, value); }
        get { return skills; }
    }

    public override string ToString()
    {
        return Name + ", age " + Age;
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

`PersonCollectionViewModel`に示す次の種類の新しいオブジェクトを作成します`PersonViewModel`ユーザーにデータを入力できるようにします。 プロパティを定義するクラスは、そのため、`IsEditing`型の`bool`と`PersonEdit`型の`PersonViewModel`します。 さらに、クラスは型の 3 つのプロパティを定義します`ICommand`という名前のプロパティと`Persons`型の`IList<PersonViewModel>`:。

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{
    PersonViewModel personEdit;
    bool isEditing;

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public bool IsEditing
    {
        private set { SetProperty(ref isEditing, value); }
        get { return isEditing; }
    }

    public PersonViewModel PersonEdit
    {
        set { SetProperty(ref personEdit, value); }
        get { return personEdit; }
    }

    public ICommand NewCommand { private set; get; }

    public ICommand SubmitCommand { private set; get; }

    public ICommand CancelCommand { private set; get; }

    public IList<PersonViewModel> Persons { get; } = new ObservableCollection<PersonViewModel>();

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

この簡略化したリストには、クラスのコンス トラクターが含まれません型の 3 つのプロパティ`ICommand`定義すると、間もなく表示されます。 型の 3 つのプロパティに変化`ICommand`と`Persons`プロパティが発生しない`PropertyChanged`のイベントが発生します。 これらのプロパティは、すべての設定は、クラスが最初に作成した場合と、その後は変更しないでください。

コンス トラクターを調べる前に、`PersonCollectionViewModel`クラスでは、XAML ファイルを見てみましょう、 **Person エントリ**プログラム。 これが含まれています、`Grid`でその`BindingContext`プロパティに設定、`PersonCollectionViewModel`します。 `Grid`が含まれています、`Button`テキスト**新規**でその`Command`プロパティにバインドされて、`NewCommand`ビューモデルでプロパティをプロパティを持つ入力フォームがバインドされている、`IsEditing`プロパティとしてプロパティとしても`PersonViewModel`にバインドされている他の 2 つのボタンと、`SubmitCommand`と`CancelCommand`ビューモデルのプロパティ。 最終的な`ListView`既に入力された人物のコレクションを表示します。

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.PersonEntryPage"
             Title="Person Entry">
    <Grid Margin="10">
        <Grid.BindingContext>
            <local:PersonCollectionViewModel />
        </Grid.BindingContext>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <!-- New Button -->
        <Button Text="New"
                Grid.Row="0"
                Command="{Binding NewCommand}"
                HorizontalOptions="Start" />

        <!-- Entry Form -->
        <Grid Grid.Row="1"
              IsEnabled="{Binding IsEditing}">

            <Grid BindingContext="{Binding PersonEdit}">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>

                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>

                <Label Text="Name: " Grid.Row="0" Grid.Column="0" />
                <Entry Text="{Binding Name}"
                       Grid.Row="0" Grid.Column="1" />

                <Label Text="Age: " Grid.Row="1" Grid.Column="0" />
                <StackLayout Orientation="Horizontal"
                             Grid.Row="1" Grid.Column="1">
                    <Stepper Value="{Binding Age}"
                             Maximum="100" />
                    <Label Text="{Binding Age, StringFormat='{0} years old'}"
                           VerticalOptions="Center" />
                </StackLayout>

                <Label Text="Skills: " Grid.Row="2" Grid.Column="0" />
                <Entry Text="{Binding Skills}"
                       Grid.Row="2" Grid.Column="1" />

            </Grid>
        </Grid>

        <!-- Submit and Cancel Buttons -->
        <Grid Grid.Row="2">
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="*" />
                <ColumnDefinition Width="*" />
            </Grid.ColumnDefinitions>

            <Button Text="Submit"
                    Grid.Column="0"
                    Command="{Binding SubmitCommand}"
                    VerticalOptions="CenterAndExpand" />

            <Button Text="Cancel"
                    Grid.Column="1"
                    Command="{Binding CancelCommand}"
                    VerticalOptions="CenterAndExpand" />
        </Grid>

        <!-- List of Persons -->
        <ListView Grid.Row="3"
                  ItemsSource="{Binding Persons}" />
    </Grid>
</ContentPage>
```

そのしくみを次に示します: ユーザーの最初が、**新規**ボタンをクリックします。 これにより、入力フォームが無効にします、**新規**ボタンをクリックします。 ユーザーは、名前、年齢、およびスキルを入力します。 ユーザーが押す、編集中にいつでも、**キャンセル**最初からやり直すボタンをクリックします。 名前と年齢が無効ですが入力されている場合のみ、**送信**ボタンが有効です。 このキーを押して**送信**ボタンによって表示されるコレクションにユーザーを転送する、`ListView`します。 いずれかの後に、**キャンセル**または**送信**ボタンが押された、入力フォームがオフになって、**新規**ボタンをもう一度有効にします。

有効な時間を入力する前に、左側にある iOS の画面はレイアウトを示します。 Android、UWP の画面表示、**送信**年齢が設定された後に有効になっているボタン。

[![ユーザー エントリ](commanding-images/personentry-small.png "Person エントリ")](commanding-images/personentry-large.png#lightbox "Person エントリ")

プログラムでは、既存のエントリを編集するための機能がないし、ページから移動するときに、エントリは保存されません。

すべてのロジック、**新規**、**送信**、および**キャンセル**でボタンが処理される`PersonCollectionViewModel`での定義、 `NewCommand`、`SubmitCommand`と`CancelCommand`プロパティ。 コンス トラクター、`PersonCollectionViewModel`型のオブジェクトにこれら 3 つのプロパティを設定`Command`します。  

A[コンス トラクター](xref:Xamarin.Forms.Command.%23ctor(System.Action,System.Func{System.Boolean}))の`Command`クラス型の引数を渡すことができる`Action`と`Func<bool>`に対応する、`Execute`と`CanExecute`メソッド。 ラムダ関数内で直接としてこれらのアクションと関数を定義する最も簡単ですが、`Command`コンス トラクター。 定義をここでは、`Command`オブジェクト、`NewCommand`プロパティ。

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {
        NewCommand = new Command(
            execute: () =>
            {
                PersonEdit = new PersonViewModel();
                PersonEdit.PropertyChanged += OnPersonEditPropertyChanged;
                IsEditing = true;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return !IsEditing;
            });

        ···

    }

    void OnPersonEditPropertyChanged(object sender, PropertyChangedEventArgs args)
    {
        (SubmitCommand as Command).ChangeCanExecute();
    }

    void RefreshCanExecutes()
    {
        (NewCommand as Command).ChangeCanExecute();
        (SubmitCommand as Command).ChangeCanExecute();
        (CancelCommand as Command).ChangeCanExecute();
    }

    ···

}
```

ユーザーがクリックすると、**新規** ボタン、`execute`に渡される関数、`Command`コンス トラクターが実行します。 これは、新しいを作成します`PersonViewModel`オブジェクト、そのオブジェクトの上のハンドラーを設定します。`PropertyChanged`イベント、`IsEditing`に`true`、を呼び出すと、`RefreshCanExecutes`コンス トラクターに定義されたメソッド。

実装するだけでなく、 `ICommand` 、インターフェイス、`Command`クラスは、という名前のメソッドも定義されています。`ChangeCanExecute`します。 自分の ViewModel を呼び出す必要があります`ChangeCanExecute`の`ICommand`何かが発生するたびにプロパティの戻り値を変更する可能性があります、`CanExecute`メソッド。 呼び出し`ChangeCanExecute`により、`Command`させるクラス、`CanExecuteChanged`メソッド。 `Button`がそのイベントのハンドラーをアタッチし、呼び出すことによって応答`CanExecute`ここでも、そのメソッドの戻り値に基づく自体を有効にするとします。

ときに、`execute`メソッドの`NewCommand`呼び出し`RefreshCanExecutes`、`NewCommand`プロパティへの呼び出しを取得します`ChangeCanExecute`、および`Button`呼び出し、`canExecute`メソッドを返すようになりました`false`ため、 `IsEditing`。プロパティは、現在`true`します。

`PropertyChanged`新しいハンドラー`PersonViewModel`オブジェクトの呼び出し、`ChangeCanExecute`メソッドの`SubmitCommand`します。 そのコマンドのプロパティを実装する方法を次に示します。


```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        SubmitCommand = new Command(
            execute: () =>
            {
                Persons.Add(PersonEdit);
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return PersonEdit != null &&
                       PersonEdit.Name != null &&
                       PersonEdit.Name.Length > 1 &&
                       PersonEdit.Age > 0;
            });

        ···
    }

    ···

}
```

`canExecute`機能`SubmitCommand`で変更されたプロパティがあるたびに呼び出されます、`PersonViewModel`編集されているオブジェクトします。 返します`true`場合にのみ、`Name`プロパティは、少なくとも 1 つの文字と`Age`が 0 より大きい。 その時点で、**送信**ボタンが有効になります。

`execute`関数**送信**からプロパティ変更ハンドラーを削除します、`PersonViewModel`にオブジェクトを追加、`Persons`コレクション、初期条件をすべて返します。

`execute`関数を**キャンセル**ボタンはすべて、**送信**ボタンは除くオブジェクトをコレクションに追加。

```csharp
public class PersonCollectionViewModel : INotifyPropertyChanged
{

    ···

    public PersonCollectionViewModel()
    {

        ···

        CancelCommand = new Command(
            execute: () =>
            {
                PersonEdit.PropertyChanged -= OnPersonEditPropertyChanged;
                PersonEdit = null;
                IsEditing = false;
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return IsEditing;
            });
    }

    ···

}
```

`canExecute`メソッドを返します。 `true` 、いつでも、`PersonViewModel`編集されています。

これらの手法はより複雑なシナリオに適用可能: プロパティ`PersonCollectionViewModel`にバインドすることが、`SelectedItem`のプロパティ、`ListView`既存の項目を編集するため、**削除** ボタンを削除する追加でしたこれらの項目。

定義する必要はありません、`execute`と`canExecute`ラムダ関数のメソッド。 ビューモデルで、通常のプライベート メソッドを記述し、で参照、`Command`コンス トラクター。 ただし、この方法はたくさんのビューモデルで 1 回だけ参照されているメソッドで発生する傾向があります。

## <a name="using-command-parameters"></a>コマンドのパラメーターを使用します。

同じ共有に 1 つまたは複数のボタン (またはその他のユーザー インターフェイス オブジェクト) の便利な場合があります`ICommand`ビューモデルのプロパティ。 この場合、使用して、`CommandParameter`プロパティをボタンを区別します。

引き続き使用できます、`Command`これらの共有クラス`ICommand`プロパティ。 クラスは、定義、[代替コンス トラクターが](xref:Xamarin.Forms.Command.%23ctor(System.Action{System.Object},System.Func{System.Object,System.Boolean}))を受け入れる`execute`と`canExecute`型のパラメーターを持つメソッド`Object`します。 これは、どのように`CommandParameter`これらのメソッドに渡されます。

ただしを使用する場合`CommandParameter`は、ジェネリックを使用する最も簡単な[ `Command<T>` ](xref:Xamarin.Forms.Command`1)クラスに設定するオブジェクトの種類を指定する`CommandParameter`します。 `execute`と`canExecute`を指定するメソッドは、その型のパラメーターを指定します。

**10 進数のキーボード**ページは、10 進数を入力するためのキーパッドを実装する方法を示す、この方法を示します。 `BindingContext`の`Grid`は、`DecimalKeypadViewModel`します。 `Entry`このビューモデルのプロパティにバインドする、`Text`のプロパティを`Label`します。 すべての`Button`オブジェクトは、ViewModel でさまざまなコマンドにバインドされます: `ClearCommand`、 `BackspaceCommand`、および`DigitCommand`:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.DecimalKeypadPage"
             Title="Decimal Keyboard">

    <Grid WidthRequest="240"
          HeightRequest="480"
          ColumnSpacing="2"
          RowSpacing="2"
          HorizontalOptions="Center"
          VerticalOptions="Center">

        <Grid.BindingContext>
            <local:DecimalKeypadViewModel />
        </Grid.BindingContext>

        <Grid.Resources>
            <ResourceDictionary>
                <Style TargetType="Button">
                    <Setter Property="FontSize" Value="32" />
                    <Setter Property="BorderWidth" Value="1" />
                    <Setter Property="BorderColor" Value="Black" />
                </Style>
            </ResourceDictionary>
        </Grid.Resources>

        <Label Text="{Binding Entry}"
               Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="3"
               FontSize="32"
               LineBreakMode="HeadTruncation"
               VerticalTextAlignment="Center"
               HorizontalTextAlignment="End" />

        <Button Text="CLEAR"
                Grid.Row="1" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding ClearCommand}" />

        <Button Text="&#x21E6;"
                Grid.Row="1" Grid.Column="2"
                Command="{Binding BackspaceCommand}" />

        <Button Text="7"
                Grid.Row="2" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="7" />

        <Button Text="8"
                Grid.Row="2" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="8" />

        <Button Text="9"
                Grid.Row="2" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="9" />

        <Button Text="4"
                Grid.Row="3" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="4" />

        <Button Text="5"
                Grid.Row="3" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="5" />

        <Button Text="6"
                Grid.Row="3" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="6" />

        <Button Text="1"
                Grid.Row="4" Grid.Column="0"
                Command="{Binding DigitCommand}"
                CommandParameter="1" />

        <Button Text="2"
                Grid.Row="4" Grid.Column="1"
                Command="{Binding DigitCommand}"
                CommandParameter="2" />

        <Button Text="3"
                Grid.Row="4" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="3" />

        <Button Text="0"
                Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2"
                Command="{Binding DigitCommand}"
                CommandParameter="0" />

        <Button Text="&#x00B7;"
                Grid.Row="5" Grid.Column="2"
                Command="{Binding DigitCommand}"
                CommandParameter="." />
    </Grid>
</ContentPage>
```

10 桁と小数部の 11 のボタンの共有へのバインドを`DigitCommand`します。 `CommandParameter`これらのボタンを識別するためです。 値に設定`CommandParameter`一般的にはわかりやすくするための中央のドット文字で表示される小数点を除くボタンによって表示されるテキストと同じです。

アクションで、プログラムを示します。

[![10 進数のキーボード](commanding-images/decimalkeyboard-small.png "10 進数のキーボード")](commanding-images/decimalkeyboard-large.png#lightbox "10 進数のキーボード")

入力した数値に小数点 10 進数が既に含まれているため、次の 3 つすべてのスクリーン ショットでは、小数点のボタンが無効になっていることを確認します。

`DecimalKeypadViewModel`定義、`Entry`型のプロパティ`string`(をトリガーする唯一のプロパティは、`PropertyChanged`イベント) 型の 3 つのプロパティと`ICommand`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{
    string entry = "0";

    public event PropertyChangedEventHandler PropertyChanged;

    ···

    public string Entry
    {
        private set
        {
            if (entry != value)
            {
                entry = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs("Entry"));
            }
        }
        get
        {
            return entry;
        }
    }

    public ICommand ClearCommand { private set; get; }

    public ICommand BackspaceCommand { private set; get; }

    public ICommand DigitCommand { private set; get; }
}
```

対応するボタン、`ClearCommand`常に有効になっているし、単にエントリを「0」を設定します。

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {
        ClearCommand = new Command(
            execute: () =>
            {
                Entry = "0";
                RefreshCanExecutes();
            });

        ···

    }

    void RefreshCanExecutes()
    {
        ((Command)BackspaceCommand).ChangeCanExecute();
        ((Command)DigitCommand).ChangeCanExecute();
    }

    ···

}
```

指定する必要はありません、ボタンが常に有効になって、`canExecute`引数、`Command`コンス トラクター。

に番号を入力して、バック スペースのロジックは少し注意が必要桁の数字が入力されていない場合、`Entry`プロパティが「0」の文字列。 場合は、ユーザーが複数のゼロ、`Entry`まだ 1 つだけ含まれているゼロ。 その他の任意の数字を入力すると、その桁はゼロを置き換えます。 場合は、ユーザーを小数点 10 進数の他の任意の数字の前に型しますが、`Entry`は文字列「0。」です。

**Backspace**エントリの長さが 1 より大きい場合にのみ、または場合に、ボタンが有効になっている`Entry`文字列「0」と等しくないです。

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        BackspaceCommand = new Command(
            execute: () =>
            {
                Entry = Entry.Substring(0, Entry.Length - 1);
                if (Entry == "")
                {
                    Entry = "0";
                }
                RefreshCanExecutes();
            },
            canExecute: () =>
            {
                return Entry.Length > 1 || Entry != "0";
            });

        ···

    }

    ···

}
```

ロジックを`execute`関数を**Backspace**ボタンにより、 `Entry` 「0」の文字列には少なくともです。

`DigitCommand`プロパティ自体と識別の 11 のボタンにバインドする、`CommandParameter`プロパティ。 `DigitCommand` 、通常のインスタンスに設定できる`Command`がクラスの使いやすく、`Command<T>`ジェネリック クラスです。 コマンド実行のインターフェイスを XAML を使用する場合、`CommandParameter`プロパティは、文字列では、通常、およびジェネリック引数の型です。 `execute`と`canExecute`関数が、型の引数を持つ`string`:

```csharp
public class DecimalKeypadViewModel : INotifyPropertyChanged
{

    ···

    public DecimalKeypadViewModel()
    {

        ···

        DigitCommand = new Command<string>(
            execute: (string arg) =>
            {
                Entry += arg;
                if (Entry.StartsWith("0") && !Entry.StartsWith("0."))
                {
                    Entry = Entry.Substring(1);
                }
                RefreshCanExecutes();
            },
            canExecute: (string arg) =>
            {
                return !(arg == "." && Entry.Contains("."));
            });
    }

    ···

}
```

`execute`メソッドへの文字列引数の追加、`Entry`プロパティ。 ただし、結果は 0 (がいないゼロと小数点 10 進数) で始まる場合、その初期 0 削除する必要を使用して、`Substring`関数。

`canExecute`メソッドを返します。`false`引数が小数点 (小数部が押されたことを示す) 場合にのみ、`Entry`小数点 10 進数が既に含まれています。

すべての`execute`メソッドを呼び出す`RefreshCanExecutes`、呼び出す`ChangeCanExecute`両方の`DigitCommand`と`ClearCommand`します。 これにより、小数点と backspace ボタンが有効になっていること、または入力した数字の現在のシーケンスに基づいて無効になっています。

## <a name="adding-commands-to-existing-views"></a>既存のビューにコマンドを追加します。

サポートされていないビューでコマンド実行のインターフェイスを使用したい場合は、イベントをコマンドに変換する Xamarin.Forms の動作を使用すること。 これは、記事、「 [**再利用可能な EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md)します。

## <a name="asynchronous-commanding-for-navigation-menus"></a>非同期のナビゲーション メニューのコマンド実行

などのナビゲーション メニューを実装するための便利なコマンドの実行は、 [**データ バインディング デモ**](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)自体をプログラムします。 一部を次に示します**MainPage.xaml**:

```xaml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:DataBindingDemos"
             x:Class="DataBindingDemos.MainPage"
             Title="Data Binding Demos"
             Padding="10">
    <TableView Intent="Menu">
        <TableRoot>
            <TableSection Title="Basic Bindings">

                <TextCell Text="Basic Code Binding"
                          Detail="Define a data-binding in code"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicCodeBindingPage}" />

                <TextCell Text="Basic XAML Binding"
                          Detail="Define a data-binding in XAML"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:BasicXamlBindingPage}" />

                <TextCell Text="Alternative Code Binding"
                          Detail="Define a data-binding in code without a BindingContext"
                          Command="{Binding NavigateCommand}"
                          CommandParameter="{x:Type local:AlternativeCodeBindingPage}" />

                ···

            </TableSection>
        </TableRoot>
    </TableView>
</ContentPage>
```

XAML でコマンドの実行を使用する場合`CommandParameter`プロパティは、通常、文字列に設定します。 この場合、ただし、XAML マークアップ拡張機能は使用ように、`CommandParameter`の種類は`System.Type`します。

各`Command`プロパティという名前のプロパティにバインドする`NavigateCommand`します。 プロパティが、分離コード ファイルで定義されている**MainPage.xaml.cs**:

```csharp
public partial class MainPage : ContentPage
{
    public MainPage()
    {
        InitializeComponent();

        NavigateCommand = new Command<Type>(
            async (Type pageType) =>
            {
                Page page = (Page)Activator.CreateInstance(pageType);
                await Navigation.PushAsync(page);
            });

        BindingContext = this;
    }

    public ICommand NavigateCommand { private set; get; }
}
```

コンス トラクターのセット、`NavigateCommand`プロパティを`execute`インスタンス化するメソッド、`System.Type`パラメーターを移動するとします。 `PushAsync`呼び出しが必要です、 `await` 、演算子、`execute`メソッドに非同期フラグを設定する必要があります。 これを行うと、`async`パラメーター リストの前にキーワード。

コンス トラクターも設定、`BindingContext`自体をページのバインドで参照するため、`NavigateCommand`このクラスにします。

このコンス トラクター内のコードの順序の違い:`InitializeComponent`プロパティにバインドの名前をその時点で呼び出すと、XAML を解析することは`NavigateCommand`ために解決できません`BindingContext`に設定されている`null`します。 場合、`BindingContext`コンス トラクターで設定されている*する前に*`NavigateCommand`バインドを解決できる場合に、設定が`BindingContext`が設定されているが、その時点で`NavigateCommand`が`null`します。 設定`NavigateCommand`後`BindingContext`効果はありません、バインディングのための変更`NavigateCommand`が作動する、`PropertyChanged`イベント、およびバインディングを知らない`NavigateCommand`は有効です。

両方を設定`NavigateCommand`と`BindingContext`(で任意の順序) を呼び出す前`InitializeComponent`は、XAML パーサーは、バインド定義を検出したときに、バインドの両方のコンポーネントが設定されているためです。

データ バインドが複雑になることができる場合がありますが、この一連の記事で説明したように、強力で用途が広く、ユーザー インターフェイスから基になるロジックを分離することで、コードを整理する助けにします。

## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms book からデータ バインド」の章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
