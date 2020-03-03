---
title: デバイスの向き
description: この記事で説明します美しく表示縦長と横長の向きで Xamarin.Forms アプリケーションのレイアウト方法。
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: f7526b6cecebadd30e95718b7e537026a6557adf
ms.sourcegitcommit: f43d5ecafd19cbc5cce39201916a83927a34617a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/02/2020
ms.locfileid: "78214994"
---
# <a name="device-orientation"></a>デバイスの向き

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)

アプリケーションの使用方法と、ユーザー エクスペリエンスを向上させるためにどのようにランドス ケープ方向を組み込むことが考慮すべき重要なは。 複数の向きに対応する個々 のレイアウトを設計することができ、最適なが使用可能な領域を使用します。 アプリケーション レベルでは、回転を無効または有効にすることができます。

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>方向を制御します。

Xamarin.Forms を使用する場合、デバイスの向きを制御するためのサポートされているメソッドは、個々 のプロジェクトの設定を使用するがします。

### <a name="ios"></a>iOS

IOS では、デバイスの向きは、**情報 plist**ファイルを使用するアプリケーション用に構成されます。 アプリは、ターゲットとして含まれている場合、このファイルは iPhone と iPod の向きの設定と iPad の設定を含まれます。 お使いの IDE に固有の手順を次に示します。 このドキュメントの上部にある IDE オプションを使用すると、表示するにはどの手順を選択します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio で iOS プロジェクトを開き、 **[plist]** を開きます。 以降、iPhone 展開情報 タブで、構成パネルにファイルが開きます。

![iPhone Visual Studio での展開情報](device-orientation-images/orientation-vs-iphone.png)

IPad の向きを構成するには、パネルの左上にある **[ipad のデプロイ情報]** タブを選択し、使用可能な向きから選択します。

![Visual Studio でサポートされているデバイスの向き](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で、iOS プロジェクトを開き、 **[plist]** を開きます。 **アプリケーション** タブで、方向の設定 セクションを使用できるようになります。

![iPhone では、Visual Studio for Mac の展開情報](device-orientation-images/orientation-xam-ui.png)

キー値エディターインターフェイスを使用して値を編集する場合は、画面の下部にある [**ソース**>] タブを選択します。

![Visual studio for Mac デバイスの向きをサポート](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Android の向きを制御するには、 **MainActivity.cs**を開き、`MainActivity` クラスを装飾する属性を使用して方向を設定します。

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android には、印刷の向きを指定するためのいくつかのオプションがサポートされています。

- **横**&ndash; は、センサーデータに関係なく、アプリケーションの向きを横向きにします。
- **縦**&ndash; は、センサーデータに関係なく、アプリケーションの向きを縦にします。
- **ユーザー** &ndash; を使用すると、ユーザーが希望する方向でアプリケーションが表示されます。
- &ndash; の**背後**では、アプリケーションの向きは、その背後にある[アクティビティ](xref:Android.App.Activity)の向きと同じになります。
- **センサー** &ndash; を使用すると、ユーザーが自動回転を無効にした場合でも、センサーによってアプリケーションの向きが決定されます。
- **Sensorlandscape** &ndash; を使用すると、アプリケーションでは、センサーデータを使用して画面の向きを変更しながら、画面が上下反転しないようにすることができます。
- **Sensorportrait** &ndash; を使用すると、アプリケーションは、画面の向きを変更するためにセンサーデータを使用しながら縦向きを使用します (画面が上下反転されないようにするため)。
- **ReverseLandscape** &ndash; を使用すると、アプリケーションは通常とは逆方向に接して横向きに表示されるようになります。
- **ReversePortrait** &ndash; を使用すると、アプリケーションは縦向きを使用し、通常とは反対の方向に接し、"上下" に表示されます。
- **Fullsensor** &ndash; を使用すると、アプリケーションはセンサーデータに依存して正しい向きを選択します (可能な限り4つ)。
- **Fulluser** &ndash; によって、アプリケーションはユーザーの向きの設定を使用します。 自動ローテーションが有効になっているすべての 4 つの向きを使用できます。
- **Userlandscape** &ndash; _\[サポートされていません\]_ では、ユーザーが自動的に回転を有効にしない限り、アプリケーションは横向きの向きを使用します。この場合は、センサーを使用して向きが決定されます。 このオプションは、コンパイルに中断されます。
- **Userportrait** &ndash; _\[サポートされていません\]_ では、ユーザーが自動的に回転を有効にしない限り、アプリケーションは縦向きを使用します。この場合、ユーザーはセンサーを使用して向きを決定します。 このオプションは、コンパイルに中断されます。
- **ロック**された &ndash; _\[サポートされていない\]_ 、アプリケーションは、デバイスの物理的な向きの変化に応答せずに、起動中の任意の画面の向きを使用します。 このオプションは、コンパイルに中断されます。

ネイティブの Android Api の向きを管理する方法を制御の多くを提供する、基本設定が表されるユーザーを明示的に矛盾するオプションを含むことに注意してください。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) では、サポートされる向きは**package.appxmanifest**ファイルで設定されます。 マニフェストを開くと、サポートされる向きを選択できる構成パネルが表示されます。

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>印刷の向きの変更に反応します。

Xamarin.Forms は、アプリの共有コードで向きの変更を通知するため、ネイティブのイベントを提供していません。 ただし、[Xamarin. Essentials](~/essentials/index.md)には、方向の変更の通知を提供する [`DeviceDisplay`] クラスが含まれています。

Xamarin を使用せずに向きを検出するには、`Page`の `SizeChanged` イベントを監視します。これは、`Page` の幅または高さのいずれかが変更されたときに発生します。 `Page` の幅が高さよりも大きい場合、デバイスは横モードになります。 詳細については、「[画面の向きに基づいてイメージを表示する](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)」を参照してください。

または、`Page`の[`OnSizeAllocated`](xref:Xamarin.Forms.Page.OnSizeAllocated*)メソッドをオーバーライドし、そこにレイアウト変更ロジックを挿入することもできます。 `OnSizeAllocated` メソッドは、`Page` に新しいサイズが割り当てられるたびに呼び出されます。これは、デバイスがローテーションされるたびに発生します。 `OnSizeAllocated` の基本実装では重要なレイアウト関数が実行されるため、オーバーライドで基本実装を呼び出すことが重要です。

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

その手順を実行に失敗すると、機能していないページが発生します。

`OnSizeAllocated` メソッドは、デバイスがローテーションされるときに何度も呼び出される可能性があることに注意してください。 毎回レイアウトの変更はリソースの無駄であり、ちらつきする可能性があります。 印刷の向きが横または縦、かどうかを追跡するために、ページ内のインスタンス変数の使用を検討し、変更があるときにのみ再描画します。

```csharp
private double width = 0;
private double height = 0;

protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
    if (this.width != width || this.height != height)
    {
        this.width = width;
        this.height = height;
        //reconfigure layout
    }
}
```

デバイスの向きの変更が検出されると、追加または使用可能な領域の変更に対応するため、ユーザー インターフェイスとの間追加のビューを削除することがあります。 たとえば、縦向きでは、各プラットフォーム上で組み込みの電卓を考えてみます。

![](device-orientation-images/calculator-portrait.png "Calculator Application in Portrait")

ランドス ケープ:

![](device-orientation-images/calculator-landscape.png "Calculator Application in Landscape")

アプリがランドス ケープで多くの機能を追加することで、使用可能な領域の利点を実行することに注意してください。

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>レスポンシブ レイアウト

組み込みのレイアウトを使用して、デバイスを回転したときに正常に移行するようにデザイン インターフェイスを行うことができます。 引き続き向きの変更に応答するときに、魅力的なインターフェイスを設計するときに、次の一般的な規則を考慮してください。

- 縦横比に**注意**してください &ndash; 向きが変化すると、比率に関して特定の仮定が行われたときに問題が発生する可能性があります。 たとえば、縦向きで、画面の垂直方向のスペースの 1/3 に必要十分な領域がビューはランドス ケープの垂直方向のスペースの 1/3 に適合しない可能性です。
- 絶対値 &ndash; 絶対値 (ピクセル) の値**に注意**してください。縦方向では意味がありません。 絶対値が必要な場合は、その影響を分離する入れ子になったレイアウトを使用します。 たとえば、項目テンプレートに均一な高さが保証されている場合は、`TableView` `ItemTemplate` で絶対値を使用するのが妥当です。

上記の規則は、ベスト プラクティスと見なされる通常の複数の画面のサイズとは、インターフェイスを実装するときにも適用されます。 このガイドの残りの部分には、xamarin.forms を使用して、プライマリのレイアウトの各応答性の高いレイアウトの具体的な例については説明します。

> [!NOTE]
> 以下のセクションでは、わかりやすくするために、一度に1種類の `Layout` のみを使用して応答性の高いレイアウトを実装する方法を示します。 実際には、`Layout`s を組み合わせて、各コンポーネントに対してより単純な、または最も直感的な `Layout` を使用して目的のレイアウトを実現する方が簡単です。

### <a name="stacklayout"></a>StackLayout

縦に表示される、次のアプリケーションを検討してください。

![](device-orientation-images/photo-stack-portrait.png "Photo Application in Portrait")

ランドス ケープ:

![](device-orientation-images/photo-stack-landscape.png "Photo Application in Landscape")

次の XAML とことで実現されます。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.StackLayoutPageXaml"
Title="Stack Photo Editor - XAML">
    <ContentPage.Content>
        <StackLayout Spacing="10" Padding="5" Orientation="Vertical"
        x:Name="outerStack"> <!-- can change orientation to make responsive -->
            <ScrollView>
                <StackLayout Spacing="5" HorizontalOptions="FillAndExpand"
                    WidthRequest="1000">
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Name: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer.jpg"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Date: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="07/05/2015"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Tags:" WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer, tiger"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Button Text="Save" HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                </StackLayout>
            </ScrollView>
            <Image  Source="deer.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

一部C#は、デバイスの向きに基づいて `outerStack` の向きを変更するために使用されます。

```csharp
protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            outerStack.Orientation = StackOrientation.Horizontal;
        } else {
            outerStack.Orientation = StackOrientation.Vertical;
        }
    }
}
```

次のことを考慮してください。

- `outerStack` は、使用可能な領域を最大限に活用するために、向きに応じてイメージとコントロールを水平方向または垂直方向のスタックとして表示するように調整されます。

### <a name="absolutelayout"></a>AbsoluteLayout

縦に表示される、次のアプリケーションを検討してください。

![](device-orientation-images/photo-abs-portrait.png "Photo Application in Portrait")

ランドス ケープ:

![](device-orientation-images/photo-abs-landscape.png "Photo Application in Landscape")

次の XAML とことで実現されます。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImageSource="deer.jpg">
    <ContentPage.Content>
        <AbsoluteLayout>
            <ScrollView AbsoluteLayout.LayoutBounds="0,0,1,1"
                AbsoluteLayout.LayoutFlags="PositionProportional,SizeProportional">
                <AbsoluteLayout>
                    <Image Source="deer.jpg"
                        AbsoluteLayout.LayoutBounds=".5,0,300,300"
                        AbsoluteLayout.LayoutFlags="PositionProportional" />
                    <BoxView Color="#CC1A7019" AbsoluteLayout.LayoutBounds=".5
                        300,.7,50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" />
                    <Label Text="deer.jpg" AbsoluteLayout.LayoutBounds = ".5
                        310,1, 50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" HorizontalTextAlignment="Center" TextColor="White" />
                </AbsoluteLayout>
            </ScrollView>
            <Button Text="Previous" AbsoluteLayout.LayoutBounds="0,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional"
                BackgroundColor="White" TextColor="Green" BorderRadius="0" />
            <Button Text="Next" AbsoluteLayout.LayoutBounds="1,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional" BackgroundColor="White"
                    TextColor="Green" BorderRadius="0" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

次のことを考慮してください。

- ページのレイアウトされているため、手続き型コードの応答性を導入する必要はありません。
- `ScrollView` は、画面の高さがボタンとイメージの固定高さの合計よりも小さい場合でも、ラベルを表示できるようにするために使用されています。

### <a name="relativelayout"></a>RelativeLayout

縦に表示される、次のアプリケーションを検討してください。

![](device-orientation-images/photo-rel-portrait.png "Photo Application in Portrait")

ランドス ケープ:

![](device-orientation-images/photo-rel-landscape.png "Photo Application in Landscape")

次の XAML とことで実現されます。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImageSource="deer.jpg">
    <ContentPage.Content>
        <RelativeLayout x:Name="outerLayout">
            <BoxView BackgroundColor="#AA1A7019"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}" />
            <ScrollView
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}">
                <RelativeLayout>
                    <Image Source="deer.jpg" x:Name="imageDeer"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.8}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.1}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=10}" />
                    <Label Text="deer.jpg" HorizontalTextAlignment="Center"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=1}"
                        RelativeLayout.HeightConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=75}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToView,ElementName=imageDeer,Property=Height,Factor=1,Constant=20}" />
                </RelativeLayout>

            </ScrollView>

            <Button Text="Previous" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                 />
            <Button Text="Next" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                />
        </RelativeLayout>
    </ContentPage.Content>
</ContentPage>

```

次のことを考慮してください。

- ページのレイアウトされているため、手続き型コードの応答性を導入する必要はありません。
- `ScrollView` は、画面の高さがボタンとイメージの固定高さの合計よりも小さい場合でも、ラベルを表示できるようにするために使用されています。

### <a name="grid"></a>グリッド

縦に表示される、次のアプリケーションを検討してください。

![](device-orientation-images/photo-grid-portrait.png "Photo Application in Portrait")

ランドス ケープ:

![](device-orientation-images/photo-grid-landscape.png "Photo Application in Landscape")

次の XAML とことで実現されます。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.GridPageXaml"
Title="Grid - XAML">
    <ContentPage.Content>
        <Grid x:Name="outerGrid">
            <Grid.RowDefinitions>
                <RowDefinition Height="*" />
                <RowDefinition Height="60" />
            </Grid.RowDefinitions>
            <Grid x:Name="innerGrid" Grid.Row="0" Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="deer.jpg" Grid.Row="0" Grid.Column="0" HeightRequest="300" WidthRequest="300" />
                <Grid x:Name="controlsGrid" Grid.Row="0" Grid.Column="1" >
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Label Text="Name:" Grid.Row="0" Grid.Column="0" />
                    <Label Text="Date:" Grid.Row="1" Grid.Column="0" />
                    <Label Text="Tags:" Grid.Row="2" Grid.Column="0" />
                    <Entry Grid.Row="0" Grid.Column="1" />
                    <Entry Grid.Row="1" Grid.Column="1" />
                    <Entry Grid.Row="2" Grid.Column="1" />
                </Grid>
            </Grid>
            <Grid x:Name="buttonsGrid" Grid.Row="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Previous" Grid.Column="0" />
                <Button Text="Save" Grid.Column="1" />
                <Button Text="Next" Grid.Column="2" />
            </Grid>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

回転の変更を処理する次の手続き型コードと共に

```csharp
private double width;
private double height;

protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.RowDefinitions.Add (new RowDefinition{ Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 1, 0);
        } else {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Auto) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 0, 1);
        }
    }
}
```

次のことを考慮してください。

- ページのレイアウトされているため、コントロールのグリッドの配置を変更する方法があります。

## <a name="related-links"></a>関連リンク

- [レイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble の例 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
- [応答性の高いレイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)
- [画面の向きに基づいてイメージを表示する](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
