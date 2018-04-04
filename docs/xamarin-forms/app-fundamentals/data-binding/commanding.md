---
title: コマンド インターフェイス
description: 実装、`Command`のデータ バインディング プロパティ
ms.prod: xamarin
ms.assetid: 69922284-F398-45C3-B4CC-B8E29BB4C533
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/05/2018
ms.openlocfilehash: 7f8b40624b9434347f69a473eed3bdff5c1d3d33
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/04/2018
---
# <a name="the-command-interface"></a>コマンド インターフェイス

派生したクラスでは、通常、ViewModel プロパティ間アーキテクチャでは、モデル View-viewmodel (MVVM)、データ バインディングが定義されている`INotifyPropertyChanged`、およびビューでは、これは一般に、XAML ファイルのプロパティです。 場合があります、アプリケーションをユーザーが、ViewModel で何かに影響するコマンドの開始を要求することによって、これらのプロパティ バインディングを超える必要があります。 これらのコマンド ボタンのクリックによって通知は、通常、タップにお問い合わせや従来のハンドラーで分離コード ファイルで処理される、`Clicked`のイベント、`Button`または`Tapped`のイベント、`TapGestureRecognizer`です。 

コマンド実行のインターフェイスは、使用が適しています MVVM アーキテクチャには、コマンドを実装する別のアプローチを提供します。 ViewModel 自体は、コマンドは、ビューで、特定のアクティビティに反応などが実行されるメソッドを含めることができます、 `Button`  をクリックします。 これらのコマンド間でデータ バインディングが定義されていると、`Button`です。

間のデータ バインドを許可する、`Button`と ViewModel、 `Button` 2 つのプロパティを定義します。

- [`Command`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.Command/) 型の <xref:System.Windows.Input.ICommand>
- [`CommandParameter`](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.CommandParameter/) 型の `Object`

対象とするデータ バインドを定義するコマンド インターフェイスを使用する、`Command`のプロパティ、`Button`型の ViewModel では、プロパティは、ソースの場所`ICommand`です。 ViewModel に関連付けられているコードが含まれています`ICommand`ボタンがクリックされたときに実行されるプロパティです。 設定することができます`CommandParameter`すべてしている場合、複数のボタンを区別するために任意のデータを同じバインド`ICommand`ViewModel プロパティ。

`Command`と`CommandParameter`プロパティは、次のクラスでも定義されています。

- [`MenuItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.MenuItem/) したがって、 [ `ToolbarItem`](https://developer.xamarin.com/api/type/Xamarin.Forms.ToolbarItem/)から派生しています `MenuItem`
- [`TextCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.TextCell/) したがって、 [ `ImageCell`](https://developer.xamarin.com/api/type/Xamarin.Forms.ImageCell/)から派生しています `TextCell`
- [`TapGestureRecognizer`](https://developer.xamarin.com/api/type/Xamarin.Forms.TapGestureRecognizer/)

[`SearchBar`](https://developer.xamarin.com/api/type/Xamarin.Forms.SearchBar/) 定義、 [ `SearchCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommand/)型のプロパティ`ICommand`と[ `SearchCommandParameter` ](https://developer.xamarin.com/api/property/Xamarin.Forms.SearchBar.SearchCommandParameter/)プロパティです。 [ `RefreshCommand` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ListView.RefreshCommand/)プロパティ[ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/)型も`ICommand`します。 

これらすべてのコマンドは、ビュー内の特定のユーザー インターフェイス オブジェクトに依存しない方法で ViewModel 内で処理できます。

## <a name="the-icommand-interface"></a>ICommand インターフェイス

<xref:System.Windows.Input.ICommand>インターフェイスは、Xamarin.Forms の一部ではありません。 代わりに定義されている、 [System.Windows.Input](xref:System.Windows.Input)名前空間には、2 つのメソッドと 1 つのイベントで構成されます。

```csharp
public interface ICommand
{
    public void Execute (Object parameter);

    public bool CanExecute (Object parameter);

    public event EventHandler CanExecuteChanged;
}
```

ViewModel にはコマンド インターフェイスを使用するには型のプロパティが含まれています`ICommand`:

```csharp
public ICommand MyCommand { private set; get; }
```

ViewModel 必要がありますを実装するクラスを参照しても、`ICommand`インターフェイスです。 このクラスは、少し説明します。 ビューで、`Command`のプロパティ、`Button`そのプロパティにバインドします。 

```xaml
<Button Text="Execute command"
        Command="{Binding MyCommand}" />
```

押されたとき、 `Button`、`Button`呼び出し、`Execute`メソッドで、`ICommand`オブジェクトにバインドされてその`Command`プロパティです。 コマンド実行のインターフェイスの最も簡単な一部であります。

`CanExecute`メソッドは複雑です。 バインディングで最初に定義されている場合、`Command`のプロパティ、 `Button`、およびデータ バインディングがいくつかの方法で変更されたときに、`Button`呼び出し、`CanExecute`メソッドで、`ICommand`オブジェクト。 場合`CanExecute`返します`false`、`Button`自体が無効になります。 これは、特定のコマンドが現在使用できないか無効なことを示します。

`Button`ものハンドラーをアタッチ、`CanExecuteChanged`のイベント`ICommand`です。 イベントは、ViewModel 内で起動します。 そのイベントが発生したときに、`Button`呼び出し`CanExecute`もう一度です。 `Button`場合を有効にする`CanExecute`返します`true`場合自体または無効に`CanExecute`を返します`false`です。

> [!IMPORTANT]
> 使用しないで、`IsEnabled`プロパティ`Button`コマンド インターフェイスを使用している場合。  

## <a name="the-command-class"></a>コマンド クラス

ViewModel が型のプロパティを定義する場合`ICommand`、ViewModel が含まれても、またはを実装するクラスを参照する必要があります、`ICommand`インターフェイスです。 このクラスが含むまたは参照する必要があります、`Execute`と`CanExecute`メソッド、および火災、`CanExecuteChanged`イベントされるたびに、`CanExecute`メソッドは、別の値を返す場合があります。

自分で、このようなクラスを記述できます。 または他のユーザーが書き込まクラスを使用することができます。 `ICommand`一部である Microsoft windows を使用した Windows MVVM アプリケーションと長年にわたっています。 Windows を実装するクラスを使用して`ICommand`Windows アプリケーションと Xamarin.Forms のアプリケーション間、ViewModels を共有することができます。

ViewModels Windows と Xamarin.Forms の間の共有は問題にならなければではないかどうかは、使用することができます、 [ `Command` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command/)または[ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/) を実装するXamarin.Formsに含まれるクラス`ICommand`インターフェイスです。 本文を指定することはこれらのクラス、`Execute`と`CanExecute`クラスのコンス トラクターのメソッドです。 使用して`Command<T>`を使用する場合、`CommandParameter`複数のビューを区別するためにプロパティに同じバインド`ICommand`プロパティ、およびより単純な`Command`要件ではないクラスです。

## <a name="basic-commanding"></a>基本的なコマンドの実行

**人記事** ページで、 [**データ バインディング デモ**](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)プログラムを ViewModel に実装されているいくつかの簡単なコマンドを示しています。

`PersonViewModel`という 3 つのプロパティを定義`Name`、 `Age`、および`Skills`人物を定義します。 このクラスは*いない*を含める`ICommand`プロパティ。

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

`PersonCollectionViewModel`表示以下の種類の新しいオブジェクトを作成`PersonViewModel`でき、ユーザーがデータを入力します。 クラスはプロパティを定義するために、`IsEditing`型の`bool`と`PersonEdit`型の`PersonViewModel`します。 クラスが型の 3 つのプロパティを定義するさらに、`ICommand`という名前のプロパティと`Persons`型の`IList<PersonViewModel>`: 

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

この簡単な一覧は、クラスのコンス トラクターは含まれません型の 3 つのプロパティ`ICommand`定義は間もなく表示されます。 型の 3 つのプロパティに変わることに注意してください`ICommand`と`Persons`プロパティが発生しない`PropertyChanged`のイベントが発生します。 これらのプロパティは、すべての設定は、クラスが最初に作成した場合と、それ以降に変更しないでください。

コンス トラクターを調べる前に、`PersonCollectionViewModel`クラスの XAML ファイルを見てみましょう、**人記事**プログラムです。 これが含まれています、`Grid`でその`BindingContext`プロパティに設定、`PersonCollectionViewModel`です。 `Grid`が含まれています、`Button`テキストで**新規**でその`Command`プロパティにバインドされて、 `NewCommand` 、ViewModel でプロパティをプロパティを持つ入力フォームがバインドされている、`IsEditing`プロパティとしてプロパティとしても`PersonViewModel`にバインドされている 2 つのボタンを増やすと、`SubmitCommand`と`CancelCommand`ViewModel プロパティ。 最終的な`ListView`担当者が入力済みのコレクションを表示します。

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

動作方法を次に示します: ユーザーの先頭が、**新規**ボタンをクリックします。 これにより、入力フォームが無効にします**新規**ボタンをクリックします。 ユーザーは、名前、年齢、およびスキルを入力します。 編集中にいつでも、キーを押す、**キャンセル**やり直すボタンをクリックします。 名前と有効な経過日数が入力された場合のみ、**送信**有効になっているボタンをクリックします。 このキーを押して**送信**ボタンによって表示されるコレクションにユーザーを転送する、`ListView`です。 いずれかの後に、**キャンセル**または**送信**ボタンが押された、入力フォームがオフになって、**新規**ボタンが再度有効にします。

有効な経過日数を入力する前に、左側にある iOS の画面はレイアウトを示します。 Android および UWP 画面表示、**送信**有効期間が設定された後に有効になっているボタン。

[![ユーザー エントリ](commanding-images/personentry-small.png "人記事")](commanding-images/personentry-large.png#lightbox "Person エントリ")

プログラムでは、既存のエントリを編集するための機能はありませんし、ページから移動するときに、エントリは保存されません。

すべてのロジック、**新規**、**送信**、および**キャンセル**でボタンが処理される`PersonCollectionViewModel`の定義を介して、 `NewCommand`、 `SubmitCommand`、および`CancelCommand`プロパティです。 コンス トラクター、`PersonCollectionViewModel`型のオブジェクトにこれら 3 つのプロパティを設定`Command`です。  

A[コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action/System.Func%7BSystem.Boolean%7D/)の`Command`クラス型の引数を渡すことができる`Action`と`Func<bool>`に対応する、`Execute`と`CanExecute`メソッドです。 ラムダの右側に関数としてこれらのアクションと関数を定義する最も簡単なである、`Command`コンス トラクターです。 定義をここでは、`Command`オブジェクトに対して、`NewCommand`プロパティ。

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

ユーザーがクリックしたとき、**新規** ボタン、`execute`に渡される関数、`Command`コンス トラクターが実行します。 こうと、新しい`PersonViewModel`オブジェクト、そのオブジェクトの上のハンドラーを設定`PropertyChanged`イベント、`IsEditing`に`true`を呼び出すと、`RefreshCanExecutes`コンス トラクターに定義されたメソッドです。

実装するだけでなく、 `ICommand` 、インターフェイス、`Command`クラスもという名前のメソッドを定義`ChangeCanExecute`です。 呼び出す必要があります、ViewModel`ChangeCanExecute`の`ICommand`何も発生するたびに、プロパティの戻り値を変更する可能性があります、`CanExecute`メソッドです。 呼び出し`ChangeCanExecute`により、`Command`発生させるクラス、`CanExecuteChanged`メソッドです。 `Button`はそのイベントのハンドラーをアタッチし、呼び出すことによって応答`CanExecute`もう一度、し、そのメソッドの戻り値に基づく自体を有効にします。

ときに、`execute`メソッドの`NewCommand`呼び出し`RefreshCanExecutes`、`NewCommand`プロパティへの呼び出しを取得する`ChangeCanExecute`、および`Button`呼び出し、`canExecute`今すぐを返すメソッド`false`のため、 `IsEditing`プロパティは、現在`true`です。

`PropertyChanged` 、新しいハンドラー`PersonViewModel`オブジェクトの呼び出し、`ChangeCanExecute`メソッドの`SubmitCommand`します。 次にそのコマンドのプロパティを実装する方法を示します。


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

`canExecute`関数を`SubmitCommand`で変更されたプロパティがあるたびに呼び出される、`PersonViewModel`編集されているオブジェクトします。 返します`true`される場合にのみ、`Name`プロパティは、少なくとも 1 文字以上、および`Age`が 0 より大きい。 その時点で、**送信**ボタンが有効になります。 

`execute`関数を**送信**からプロパティ変更ハンドラーを削除、 `PersonViewModel`、オブジェクトを追加、`Persons`コレクション、および初期条件へのすべてのものを返します。

`execute`関数を**キャンセル**ボタンはすべてのものを**送信**ボタンの機能は execept オブジェクトをコレクションに追加します。

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

`canExecute`メソッドを返します。`true`いつでも、`PersonViewModel`編集されています。

これらの手法より複雑なシナリオに適合させる可能性があります: プロパティに`PersonCollectionViewModel`にバインドされている可能性があります、`SelectedItem`のプロパティ、`ListView`既存の項目を編集するため、**削除**を削除するボタンを追加できませんでしたこれらの項目。

定義する必要はありません、`execute`と`canExecute`ラムダ関数のメソッドです。 ViewModel 内で正規のプライベート メソッドを記述しでそれらを参照することができます、`Command`コンス トラクターです。 ただし、この方法はたくさんの 1 回だけ、ViewModel で参照されているメソッドで発生する傾向があります。

## <a name="using-command-parameters"></a>コマンド パラメーターを使用します。

同じを共有する 1 つまたは複数のボタン (またはその他のユーザー インターフェイス オブジェクト) の便利な場合があります`ICommand`ViewModel プロパティ。 この場合、使用して、`CommandParameter`ボタンを区別するプロパティです。 

使用を続行することができます、`Command`これらの共有クラス`ICommand`プロパティです。 このクラスを定義、[代替コンス トラクター](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Command.Command/p/System.Action%7BSystem.Object%7D/System.Func%7BSystem.Object,System.Boolean%7D/)を受け入れる`execute`と`canExecute`型のパラメーターを持つメソッド`Object`です。 これは、どのように`CommandParameter`はこれらのメソッドに渡されます。

ただしを使用する場合`CommandParameter`、ジェネリックを使用する方が簡単[ `Command<T>` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Command%3CT%3E/)クラスに設定するオブジェクトの種類を指定する`CommandParameter`です。 `execute`と`canExecute`を指定する方法は、その型のパラメーターを持ちます。

**10 進数のキーボード**ページが 10 進数を入力するためのキーパッドを実装する方法を示すことでこの方法を示します。 `BindingContext`の`Grid`は、`DecimalKeypadViewModel`です。 `Entry`この ViewModel のプロパティにバインドされる、`Text`のプロパティ、`Label`です。 すべての`Button`オブジェクトは、ViewModel のさまざまなコマンドにバインドされて: `ClearCommand`、 `BackspaceCommand`、および`DigitCommand`:

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

10 桁の数字と小数点 11 ボタン共有へのバインド`DigitCommand`です。 `CommandParameter`これらのボタンを区別します。 設定されている値`CommandParameter`は、通常、中間のドット文字が表示され、これをわかりやすく小数点を除くボタンによって表示されるテキストと同じです。

アクションで、プログラムを次に示します。

[![10 進数のキーボード](commanding-images/decimalkeyboard-small.png "10 進数のキーボード")](commanding-images/decimalkeyboard-large.png#lightbox "10 進数のキーボード")

入力した数に既に小数点が含まれているために次の 3 つすべてのスクリーン ショットの中で小数点のボタンが無効になっていることを確認します。 

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

対応するボタン、`ClearCommand`常に有効にし、単に「0」にエントリを設定します。

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

指定する必要はありません、ボタンが常に有効になって、`canExecute`の引数、`Command`コンス トラクターです。

番号を入力して、バック スペースのロジックは少しわかりにくいため桁の数字が入力されていない場合、`Entry`プロパティが「0」の文字列。 ユーザーが複数 0 (ゼロ) を入力した場合、`Entry`まだ 1 つだけ含まれているゼロです。 その他の任意の数字を入力すると、その数字はゼロを置き換えます。 ユーザーが、その他の任意の数字の前に小数点を入力した場合、`Entry`文字列「0」です。

**Backspace**エントリの長さが 1 より大きい場合、または、ボタンが有効になっている`Entry`が文字列「0」と等しくないです。

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

ロジック、`execute`関数を**Backspace**ボタン確実に、 `Entry` 「0」の文字列には、少なくともです。

`DigitCommand` 11 ボタン、それぞれの識別には、自らにプロパティがバインドされて、`CommandParameter`プロパティです。 `DigitCommand` 、通常のインスタンスに設定できる`Command`クラスがの使いやすい、`Command<T>`ジェネリック クラスです。 コマンド実行のインターフェイスを XAML を使用すると、`CommandParameter`プロパティは、通常文字列、および汎用引数の型であります。 `execute`と`canExecute`関数は、型の引数を持つ`string`:

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

`execute`メソッドへの文字列引数の追加、`Entry`プロパティです。 ただし、結果が 0 (がいない 0 と小数点) で始まる場合、その初期 0 必要がありますは削除を使用して、`Substring`関数。

`canExecute`メソッドを返します。`false`引数が小数点 (小数が押されたされていることを示す) 場合にのみ、`Entry`小数点が既に存在します。 

すべての`execute`メソッド呼び出し`RefreshCanExecutes`、呼び出す`ChangeCanExecute`両方の`DigitCommand`と`ClearCommand`です。 小数点および backspace ボタンが有効になっているか、入力した数字の現在のシーケンスに基づいて無効になります。

## <a name="adding-commands-to-existing-views"></a>既存のビューへのコマンドの追加

サポートしているビューとコマンド実行のインターフェイスを使用する場合は、可能であれば、コマンドにイベントを変換する Xamarin.Forms の動作を使用します。 これは、記事で説明[**再利用可能な EventToCommandBehavior**](~/xamarin-forms/app-fundamentals/behaviors/reusable/event-to-command-behavior.md)です。

## <a name="asynchronous-commanding-for-navigation-menus"></a>非同期のナビゲーション メニューのコマンド実行

などのナビゲーション メニューを実装するための便利なコマンド実行は、 [**データ バインディング デモ**](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)プログラム自体です。 一部を次に示します**MainPage.xaml**:


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

XAML でのコマンドを使用する場合`CommandParameter`プロパティは通常の文字列に設定します。 この場合、ただし、XAML マークアップ拡張機能が使用できるように、`CommandParameter`の種類は`System.Type`します。

各`Command`プロパティがという名前のプロパティにバインドされる`NavigateCommand`です。 プロパティが、分離コード ファイルで定義されている**MainPage.xaml.cs**:

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

コンス トラクターのセット、`NavigateCommand`プロパティを`execute`インスタンス化するメソッド、`System.Type`パラメーターに移動するとします。 `PushAsync`呼び出しが必要です、 `await` 、演算子、`execute`メソッドに非同期とフラグを設定する必要があります。 これは、使用、`async`パラメーター リストの前にキーワード。 

コンス トラクターにも設定、`BindingContext`自身へのページのバインドを参照できるように、`NavigateCommand`このクラスでします。

このコンス トラクター内のコードの順序による違い:`InitializeComponent`呼び出しと、解析する XAML が、その時点でプロパティにバインドがという名前`NavigateCommand`ために解決できません。`BindingContext`に設定されている`null`です。 場合、`BindingContext`コンス トラクターで設定されている*する前に*`NavigateCommand`が設定された場合、バインディング解決できるときに`BindingContext`が設定されているが、その時点で`NavigateCommand`が`null`です。 設定`NavigateCommand`後`BindingContext`は効果がなく、バインディングのために変更`NavigateCommand`イベントは発生しません、`PropertyChanged`イベント、およびバインドしないことに注意して`NavigateCommand`は有効です。

両方を設定`NavigateCommand`と`BindingContext`で任意の順序) を呼び出す前`InitializeComponent`はバインドの両方のコンポーネントは、XAML パーサーは、バインド定義を検出したときに設定されているために機能します。 

データ バインディングが、わかりにくいことがありますが、この一連の記事で説明したよう、強力で多目的のユーザー インターフェイスから基になるロジックを分離することにより、コードを整理する助けにします。



## <a name="related-links"></a>関連リンク

- [データ バインディング デモ (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/DataBindingDemos/)
- [Xamarin.Forms 帳からのデータ バインディング章](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md)
