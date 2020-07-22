---
title: デバイスの向き
description: この記事では Xamarin.Forms 、縦と横の向きで見栄えの良いアプリケーションをレイアウトする方法について説明します。
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0b1a47d4dcc92fca4d280708a2cbbe9374c17da8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573298"
---
# <a name="device-orientation"></a>デバイスの向き

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)

アプリケーションを使用する方法と、ユーザーエクスペリエンスを向上させるためにランドスケープの向きを組み込む方法を検討することが重要です。 個々のレイアウトは、複数の向きに対応し、使用可能な領域を最適に使用するように設計できます。 アプリケーションレベルでは、ローテーションを無効または有効にすることができます。

## <a name="controlling-orientation"></a>向きの制御

を使用する場合 Xamarin.Forms 、デバイスの向きを制御する方法として、個々のプロジェクトごとに設定を使用することをお勧めします。

### <a name="ios"></a>iOS

IOS では、デバイスの向きは、**情報 plist**ファイルを使用するアプリケーション用に構成されます。 このドキュメントの上部にある IDE オプションを使用して、表示する手順を選択します。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Visual Studio で iOS プロジェクトを開き、[ **plist**] を開きます。 ファイルが構成パネルに表示されます。開始するには、iPhone の [展開情報] タブを使用します。

![Visual Studio の iPhone 展開情報](device-orientation-images/orientation-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で、iOS プロジェクトを開き、[ **plist**] を開きます。 [**アプリケーション**] タブで、[方向の設定] セクションを使用できるようになります。

![Visual Studio for Mac の iPhone 展開情報](device-orientation-images/orientation-vsmac.png)

キー値エディターインターフェイスを使用して値を編集する場合は、画面の下部にある [**ソース**>] タブを選択します。

![Visual Studio for Mac でサポートされているデバイスの向き](device-orientation-images/orientation-source-vsmac.png)

-----

### <a name="android"></a>Android

Android の向きを制御するには、 **MainActivity.cs**を開き、クラスを装飾する属性を使用して方向を設定し `MainActivity` ます。

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : FormsAppCompatActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin Android では、方向を指定するためのオプションがいくつかサポートされています。

- **横** &ndash;センサーデータに関係なく、アプリケーションの向きを強制的に横にします。
- **縦** &ndash;センサーデータに関係なく、アプリケーションの向きを強制的に縦にします。
- **ユーザー** &ndash;ユーザーの優先方向を使用してアプリケーションを表示します。
- **背後** &ndash;アプリケーションの向きが、その背後にある[アクティビティ](xref:Android.App.Activity)の向きと同じになるようにします。
- **センサー** &ndash;ユーザーが自動回転を無効にしている場合でも、センサーによってアプリケーションの向きが決定されます。
- **Sensorlandscape** &ndash;センサーデータを使用して画面の向きを変更するときに、アプリケーションが横向きの向きを使用するようにします (画面が上下反転されないようにするため)。
- **Sensorportrait** &ndash;アプリケーションで、センサーデータを使用して画面の向きを変更するときに縦方向を使用します (画面が上下反転されないようにします)。
- **ReverseLandscape** &ndash;アプリケーションで横向きの向きが使用されるようにします。これにより、通常とは逆の方向になります。
- **ReversePortrait** &ndash;アプリケーションが縦向きを使用するようにします。通常とは逆方向に接続され、"逆さま" に見えるようになります。
- **Fullsensor** &ndash;アプリケーションがセンサーデータを使用して正しい向きを選択するようにします (可能な4つのうちの一部)。
- **Fulluser** &ndash;アプリケーションでユーザーの向きの設定を使用します。 自動回転が有効になっている場合は、4つの方向すべてを使用できます。
- **Userlandscape** &ndash;[ _ \[ サポート \] されていません_] を指定すると、ユーザーが自動回転を有効にしていない限り、アプリケーションは横向きの向きを使用します。この場合、センサーを使用して向きが決定されます。 このオプションを選択すると、コンパイルが中断されます。
- **Userportrait** &ndash;[ _ \[ サポート \] されていません_] を指定すると、ユーザーが自動回転を有効にしていない限り、アプリケーションは縦向きを使用します。この場合は、センサーを使用して向きが決定されます。 このオプションを選択すると、コンパイルが中断されます。
- **ロック** &ndash; 済み[ _ \[ サポート \] されていません_] を指定すると、アプリケーションは、デバイスの物理的な向きの変化に応答せずに、起動中の任意の画面の向きを使用します。 このオプションを選択すると、コンパイルが中断されます。

ネイティブ Android Api では、ユーザーが表現する設定を明示的に矛盾させるオプションなど、向きの管理方法を細かく制御できることに注意してください。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) では、サポートされる向きは**package.appxmanifest**ファイルで設定されます。 マニフェストを開くと、サポートされている向きを選択できる構成パネルが表示されます。

## <a name="reacting-to-changes-in-orientation"></a>向きの変化への対応

Xamarin.Formsでは、共有コードの向きの変更をアプリに通知するためのネイティブイベントは提供されません。 ただし、に [Xamarin.Essentials](~/essentials/index.md) は、 `DeviceDisplay` 向きの変更の通知を提供する [] クラスが含まれています。

を使用せずに向きを検出するには、の Xamarin.Essentials イベントを監視し `SizeChanged` ます。これは、の `Page` 幅または高さのいずれかが変化したときに発生し `Page` ます。 の幅が `Page` 高さよりも大きい場合、デバイスは横モードになります。 詳細については、「[画面の向きに基づいてイメージを表示する](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)」を参照してください。

または、のメソッドをオーバーライドし [`OnSizeAllocated`](xref:Xamarin.Forms.Page.OnSizeAllocated*) `Page` 、そこにレイアウト変更ロジックを挿入することもできます。 メソッドは、 `OnSizeAllocated` に新しいサイズが割り当てられるたびに呼び出され `Page` ます。これは、デバイスがローテーションされるたびに発生します。 の基本実装では `OnSizeAllocated` 、重要なレイアウト関数が実行されるため、オーバーライドで基本実装を呼び出すことが重要です。

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

この手順を実行しないと、機能していないページが表示されます。

`OnSizeAllocated`デバイスがローテーションされると、メソッドが何度も呼び出される可能性があることに注意してください。 レイアウトを変更するたびにリソースが無駄になり、ちらつきが発生する可能性があります。 ページ内でインスタンス変数を使用して、向きが横または縦であるかどうかを追跡し、変更がある場合にのみ再描画することを検討してください。

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

デバイスの向きの変更が検出されたら、使用可能な領域の変更に対応するために、ユーザーインターフェイスに対して追加のビューの追加または削除を行うことができます。 たとえば、各プラットフォームの組み込みの電卓を縦方向に検討します。

![](device-orientation-images/calculator-portrait.png "Calculator Application in Portrait")

横:

![](device-orientation-images/calculator-landscape.png "Calculator Application in Landscape")

アプリでは、横長に機能を追加することで、使用可能な領域を活用できます。

## <a name="responsive-layout"></a>応答性の高いレイアウト

組み込みのレイアウトを使用してインターフェイスを設計し、デバイスのローテーション時に適切に移行できるようにすることができます。 向きの変化に対応するときに引き続き魅力的なインターフェイスを設計する場合は、次の一般的な規則を考慮してください。

- 比率に注意してください**Pay attention to ratios** &ndash;方向を変更すると、特定の仮定が比率に関して行われた場合に問題が発生する可能性があります。 たとえば、画面の垂直方向の領域が1/3 に非常に多く含まれているビューは、横の垂直方向の領域の1/3 には収まりません。
- 絶対値に注意し**てください** &ndash;縦方向に意味がある絶対値 (ピクセル) 値は、横では意味がない可能性があります。 絶対値が必要な場合は、入れ子になったレイアウトを使用して、その影響を分離します。 たとえば、 `TableView` `ItemTemplate` 項目テンプレートに均一な高さが保証されている場合、では絶対値を使用するのが妥当です。

上記の規則は、複数の画面サイズ用のインターフェイスを実装する場合にも適用され、一般にベストプラクティスと見なされます。 このガイドの残りの部分では、の各主要レイアウトを使用した応答性の高いレイアウトの例について説明し Xamarin.Forms ます。

> [!NOTE]
> 以下のセクションでは、わかりやすくするために、一度に1種類のを使用して応答性の高いレイアウトを実装する方法を示し `Layout` ます。 実際には、 `Layout` 各コンポーネントでより単純な、または最も直感的な方法を使用して目的のレイアウトを実現する方が、多くの場合、より簡単です `Layout` 。

### <a name="stacklayout"></a>StackLayout

縦に表示される次のアプリケーションについて考えてみましょう。

![](device-orientation-images/photo-stack-portrait.png "Photo Application in Portrait")

横:

![](device-orientation-images/photo-stack-landscape.png "Photo Application in Landscape")

これは、次の XAML を使用して実現されます。

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

一部の C# は、 `outerStack` デバイスの向きに基づいての方向を変更するために使用されます。

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

- `outerStack`は、イメージとコントロールを向きに応じて水平方向または垂直方向のスタックとして表示し、使用可能な領域を最大限に活用できるように調整されます。

### <a name="absolutelayout"></a>AbsoluteLayout

縦に表示される次のアプリケーションについて考えてみましょう。

![](device-orientation-images/photo-abs-portrait.png "Photo Application in Portrait")

横:

![](device-orientation-images/photo-abs-landscape.png "Photo Application in Landscape")

これは、次の XAML を使用して実現されます。

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

- ページのレイアウト方法によっては、プロシージャコードを使用して応答性を向上させる必要はありません。
- は、 `ScrollView` 画面の高さがボタンとイメージの固定された高さの合計よりも小さい場合でも、ラベルを表示できるようにするために使用されています。

### <a name="relativelayout"></a>RelativeLayout

縦に表示される次のアプリケーションについて考えてみましょう。

![](device-orientation-images/photo-rel-portrait.png "Photo Application in Portrait")

横:

![](device-orientation-images/photo-rel-landscape.png "Photo Application in Landscape")

これは、次の XAML を使用して実現されます。

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

- ページのレイアウト方法によっては、プロシージャコードを使用して応答性を向上させる必要はありません。
- は、 `ScrollView` 画面の高さがボタンとイメージの固定された高さの合計よりも小さい場合でも、ラベルを表示できるようにするために使用されています。

### <a name="grid"></a>グリッド

縦に表示される次のアプリケーションについて考えてみましょう。

![](device-orientation-images/photo-grid-portrait.png "Photo Application in Portrait")

横:

![](device-orientation-images/photo-grid-landscape.png "Photo Application in Landscape")

これは、次の XAML を使用して実現されます。

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

次の手順に従って、ローテーションの変化を処理します。

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

- ページのレイアウト方法により、コントロールのグリッド位置を変更するメソッドがあります。

## <a name="related-links"></a>関連リンク

- [レイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble の例 (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
- [応答性の高いレイアウト (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-responsivelayout)
- [画面の向きに基づいてイメージを表示する](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
