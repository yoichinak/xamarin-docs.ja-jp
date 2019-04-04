---
title: デバイスの向き
description: この記事で説明します美しく表示縦長と横長の向きで Xamarin.Forms アプリケーションのレイアウト方法。
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: a57775776b32f34d2c9b976a22cc92cc22f3c879
ms.sourcegitcommit: 771e65583e978ff2b9c652b953a21b0bbb10a5d1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/01/2019
ms.locfileid: "58782501"
---
# <a name="device-orientation"></a>デバイスの向き

[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)

アプリケーションの使用方法と、ユーザー エクスペリエンスを向上させるためにどのようにランドス ケープ方向を組み込むことが考慮すべき重要なは。 複数の向きに対応する個々 のレイアウトを設計することができ、最適なが使用可能な領域を使用します。 アプリケーション レベルでは、回転を無効または有効にすることができます。

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>方向を制御します。

Xamarin.Forms を使用する場合、デバイスの向きを制御するためのサポートされているメソッドは、個々 のプロジェクトの設定を使用するがします。

### <a name="ios"></a>iOS

Ios では、デバイスの向きを使用してアプリケーション用に構成された、 **Info.plist**ファイル。 アプリは、ターゲットとして含まれている場合、このファイルは iPhone と iPod の向きの設定と iPad の設定を含まれます。 お使いの IDE に固有の手順を次に示します。 このドキュメントの上部にある IDE オプションを使用すると、表示するにはどの手順を選択します。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio で iOS プロジェクトを開き、開く**Info.plist**します。 以降、iPhone 展開情報 タブで、構成パネルにファイルが開きます。

![iPhone Visual Studio での展開情報](device-orientation-images/orientation-vs-iphone.png)

IPad の向きを構成するには、選択、 **iPad 展開情報**の左、パネル、使用できる向きから上部のタブ。

![Visual Studio でサポートされているデバイスの向き](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual studio for Mac、iOS プロジェクトを開き、開く**Info.plist**します。 で、**アプリケーション** タブのセクションでは印刷の向きを設定を利用できなくなります。

![iPhone では、Visual Studio for Mac の展開情報](device-orientation-images/orientation-xam-ui.png)

キー/値エディターのインターフェイスでは、選択を使用して値を編集する場合、**ソース**>、画面の下部にあるタブ。

![Visual studio for Mac デバイスの向きをサポート](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Android の方向を制御するには、開く**MainActivity.cs**属性の修飾を使用して、印刷の向きを設定して、`MainActivity`クラス。

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

- **ランドス ケープ**&ndash;センサー データに関係なく、横にあるアプリケーションの向きを強制します。
- **縦**&ndash;センサー データに関係なく、縦向きにするアプリケーションの向きを強制します。
- **ユーザー** &ndash;により、アプリケーションは、ユーザーの表示に望ましい方向で表示されます。
- **背後にある**&ndash;により、アプリケーションの向きの方向と同じである、[アクティビティ](https://developer.xamarin.com/api/type/Android.App.Activity/)その背後にあります。
- **センサー** &ndash;センサーが決定する、アプリケーションの向きが場合でも、ユーザーが自動的に回転を無効にします。
- **SensorLandscape** &ndash;により横向き (その画面は、上下として認識されていない)、画面が向いている方向を変更するセンサー データを使用しているときに使用するアプリケーション。
- **SensorPortrait** &ndash;と縦向き (その画面は、上下として認識されていない)、画面が向いている方向を変更するセンサー データを使用しているときに使用するアプリケーションが発生します。
- **ReverseLandscape** &ndash;により、アプリケーションは、「上下」に表示されるため、通常から反対方向に接続する横方向を使用するには。
- **ReversePortrait** &ndash;により、アプリケーションは「上下」に表示されるため、通常から反対方向に接続する、縦向きを使用するには。
- **FullSensor** &ndash;により、アプリケーションは (可能な 4) から正しい向きを選択するセンサー データに依存します。
- **FullUser** &ndash;により、アプリケーションはユーザーの向きの設定を使用します。 自動ローテーションが有効になっているすべての 4 つの向きを使用できます。
- **UserLandscape** &ndash; _\[はサポートされていません\]_ 、横方向を使用するアプリケーションをユーザーが有効にすると、自動的に回転に使用されますが、印刷の向きを決定するセンサーです。 このオプションは、コンパイルに中断されます。
- **UserPortrait** &ndash; _\[はサポートされていません\]_ 縦向きを使用するアプリケーションをユーザーが有効にすると、自動的に回転に使用されますが、印刷の向きを決定するセンサーです。 このオプションは、コンパイルに中断されます。
- **ロックされている** &ndash; _\[はサポートされていません\]_ により、アプリケーションは、画面の向きを使用するデバイスでの変更に応答することがなくは起動時の物理方向。 このオプションは、コンパイルに中断されます。

ネイティブの Android API の向きを管理する方法を制御の多くを提供する、基本設定が表されるユーザーを明示的に矛盾するオプションを含むことに注意してください。

### <a name="universal-windows-platform"></a>ユニバーサル Windows プラットフォーム

ユニバーサル Windows プラットフォーム (UWP) 上でサポートされる向きが設定されて、 **Package.appxmanifest**ファイル。 マニフェストを開くと、サポートされる向きを選択できる構成パネルが表示されます。

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>印刷の向きの変更に反応します。

Xamarin.Forms は、アプリの共有コードで向きの変更を通知するため、ネイティブのイベントを提供していません。 ただし、`SizeChanged`のイベント、`Page`ときに発生の高さまたは幅、`Page`変更します。 ときの幅、`Page`が高さよりも大きい、デバイスが横モードでします。 詳細については、[画面の向きに基づいてイメージを表示](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)を参照してください。

> [!NOTE]
> 共有コードで向きの変更の通知を受信するため、既存の無料の NuGet パッケージがあります。 参照してください、 [GitHub リポジトリ](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation)詳細についてはします。

代わりに、オーバーライドすることは、 [ `OnSizeAllocated` ](xref:Xamarin.Forms.Page.OnSizeAllocated*)メソッドを`Page`、任意のレイアウトを挿入するロジックがありますを変更します。 `OnSizeAllocated`メソッドが呼び出されるたびに、`Page`場合は、デバイスを回転するたびに、新しいサイズが割り当てられます。 注意の基本実装`OnSizeAllocated`オーバーライド時に基本の実装を呼び出すには、ので、重要なレイアウト機能を実行します。

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

その手順を実行に失敗すると、機能していないページが発生します。

なお、`OnSizeAllocated`デバイスを回転したときに、メソッドで何度もするということがあります。 毎回レイアウトの変更はリソースの無駄であり、ちらつきする可能性があります。 印刷の向きが横または縦、かどうかを追跡するために、ページ内のインスタンス変数の使用を検討し、変更があるときにのみ再描画します。

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

![](device-orientation-images/calculator-portrait.png "縦向きで電卓アプリケーション")

ランドス ケープ:

![](device-orientation-images/calculator-landscape.png "横に電卓アプリケーション")

アプリがランドス ケープで多くの機能を追加することで、使用可能な領域の利点を実行することに注意してください。

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>レスポンシブ レイアウト

組み込みのレイアウトを使用して、デバイスを回転したときに正常に移行するようにデザイン インターフェイスを行うことができます。 引き続き向きの変更に応答するときに、魅力的なインターフェイスを設計するときに、次の一般的な規則を考慮してください。

- **比率に注意を払う**&ndash;向きの変更は、比率に関する仮定が行われると、問題が発生します。 たとえば、縦向きで、画面の垂直方向のスペースの 1/3 に必要十分な領域がビューはランドス ケープの垂直方向のスペースの 1/3 に適合しない可能性です。
- **絶対値気を付ける**&ndash;絶対値 (ピクセル) で指定する縦向きで意味のある可能性があります、ランドス ケープで意味を成しません。 絶対値が必要な場合は、その影響を分離する入れ子になったレイアウトを使用します。 などにするが妥当で絶対値を使用して、 `TableView` `ItemTemplate`項目テンプレートが保証された均一な高さを持っている場合。

上記の規則は、ベスト プラクティスと見なされる通常の複数の画面のサイズとは、インターフェイスを実装するときにも適用されます。 このガイドの残りの部分には、xamarin.forms を使用して、プライマリのレイアウトの各応答性の高いレイアウトの具体的な例については説明します。

> [!NOTE]
> 次のセクションでは、デモンストレーションの 1 つの種類を使用してレスポンシブのレイアウトを実装する方法をわかりやすくするため、`Layout`一度にします。 実際が多くの場合、簡単に混在させる`Layout`簡単または最も直感的なを使用して目的のレイアウトを実現するために s`Layout`コンポーネントごとにします。

### <a name="stacklayout"></a>StackLayout

縦に表示される、次のアプリケーションを検討してください。

![](device-orientation-images/photo-stack-portrait.png "フォト アプリケーション (縦向き)")

ランドス ケープ:

![](device-orientation-images/photo-stack-landscape.png "フォト アプリケーション (横向き)")

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

いくつか c# を使用の方向を変更する`outerStack`ベースのデバイスの向き。

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

次の点に注意してください。

- `outerStack` 最適な活用するために使用可能な領域の向きに応じて、水平または垂直スタックとして、画像とコントロールを表示するには、調整されます。


### <a name="absolutelayout"></a>AbsoluteLayout

縦に表示される、次のアプリケーションを検討してください。

![](device-orientation-images/photo-abs-portrait.png "フォト アプリケーション (縦向き)")

ランドス ケープ:

![](device-orientation-images/photo-abs-landscape.png "フォト アプリケーション (横向き)")

次の XAML とことで実現されます。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImage="deer.jpg">
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

次の点に注意してください。

- ページのレイアウトされているため、手続き型コードの応答性を導入する必要はありません。
- `ScrollView`が場合でも、画面の高さのボタンとイメージの固定の高さの合計より小さいかを表示するラベルに使用されています。


### <a name="relativelayout"></a>RelativeLayout

縦に表示される、次のアプリケーションを検討してください。

![](device-orientation-images/photo-rel-portrait.png "フォト アプリケーション (縦向き)")

ランドス ケープ:

![](device-orientation-images/photo-rel-landscape.png "フォト アプリケーション (横向き)")

次の XAML とことで実現されます。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImage="deer.jpg">
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

次の点に注意してください。

- ページのレイアウトされているため、手続き型コードの応答性を導入する必要はありません。
- `ScrollView`が場合でも、画面の高さのボタンとイメージの固定の高さの合計より小さいかを表示するラベルに使用されています。

### <a name="grid"></a>グリッド

縦に表示される、次のアプリケーションを検討してください。

![](device-orientation-images/photo-grid-portrait.png "フォト アプリケーション (縦向き)")

ランドス ケープ:

![](device-orientation-images/photo-grid-landscape.png "フォト アプリケーション (横向き)")

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

次の点に注意してください。

- ページのレイアウトされているため、コントロールのグリッドの配置を変更する方法があります。


## <a name="related-links"></a>関連リンク

- [レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 例 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [レスポンシブ レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [画面の向きに基づくイメージを表示します。](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/screen-orientation)
