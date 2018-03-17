---
title: "デバイスの向き"
description: "縦方向と横の向きで見栄えのアプリケーションをレイアウトする方法を理解します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: cb17c224fc6102d9e0dc25853c2222734299647a
ms.sourcegitcommit: 5fc1c4d17cd9c755604092cf7ff038a6358f8646
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/17/2018
---
# <a name="device-orientation"></a>デバイスの向き

アプリケーションの使用方法と、ユーザー エクスペリエンスを向上させる方法横向きに組み込むことに注意してください。 複数の向きに合わせて個々 のレイアウトを設計することができ、最適なが使用可能な領域を使用します。 アプリケーション レベルでは、回転を無効または有効にすることができます。

この記事では、指示に従って、デバイスの向きの機能を活用したアプリを作成し、次のセクションがあります。

- **[方向を制御する](#Controlling_Orientation)** &ndash;を各プラットフォーム間で、アプリ レベルでの方向を制御する方法を理解します。
- **[印刷の向きの変更に反応する](#Reacting_to_Changes_in_Orientation)** &ndash;を通知して対処する方法を学習、方向に変更します。
- **[レスポンシブ レイアウト](#Responsive_Layout)** &ndash;横と縦向きの間で自動的に動作するレイアウトを作成する方法について説明します。

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>方向を制御します。

Xamarin.Forms を使用する場合、デバイスの方向を制御するサポートされているメソッドは、それぞれのプロジェクトの設定を使用するです。

> [!NOTE]
> Xamarin.Forms 1.5.0 が原因でバグがある時点で、カスタム レンダラー ベースは失敗する方向を制御しようとします。 参照してください[このディスカッション](https://forums.xamarin.com/discussion/46653/forcing-landscape-for-a-single-page-in-ios#latest)詳細については、Xamarin のフォーラムでは、この説明します。

### <a name="ios"></a>iOS

使用するアプリケーションで、iOS デバイスの方向が構成されている、 **Info.plist**ファイル。 アプリは、ターゲットとして含まれている場合、このファイルは iPhone と iPod、向きの設定と iPad の設定を含まれます。 IDE に固有の手順を次に示します。 このドキュメントの上部にある IDE オプションを使用すると、表示するにはどの手順を選択します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio で、iOS プロジェクトを開き、開き**Info.plist**です。 以降、iPhone の展開情報 タブで、構成パネルにファイルが開きます。

![iPhone Visual Studio での展開情報](device-orientation-images/orientation-vs-iphone.png)

IPad の向きを構成するには、選択、 **iPad 展開情報**の左パネル、し、利用可能な向きから上部にあるタブ。

![Visual Studio でサポートされているデバイスの向き](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac 用 Visual Studio で iOS プロジェクトを開き、開き**Info.plist**です。 下にある、**アプリケーション** タブで、セクションが利用できる印刷の向きを設定します。

![iPhone で Visual Studio for Mac の展開情報](device-orientation-images/orientation-xam-ui.png)

キーと値のエディターのインターフェイスでは、選択を使用して値の編集を希望する場合、**ソース**> 画面の下部にあるタブ。

![Mac 用 Visual Studio でのデバイスの向きをサポート](device-orientation-images/orientation-xam-source.png)

-----


### <a name="android"></a>Android

Android の方向を制御するには、開く**MainActivity.cs**属性装飾を使用して印刷の向きを設定し、`MainActivity`クラス。

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android には、印刷の向きを指定するためのいくつかのオプションがサポートされています。

- **ランドス ケープ**&ndash;センサー データに関係なく、横にあるアプリケーションの向きを強制します。
- **縦**&ndash;センサー データに関係なく、縦向きにするアプリケーションの向きを強制します。
- **ユーザー** &ndash;により、アプリケーションは、ユーザーの表示に望ましい方向を使用して表示します。
- **背後にある**&ndash;の方向と同じである、アプリケーションの向きをにより、[アクティビティ](https://developer.xamarin.com/api/type/Android.App.Activity/)背後にあります。
- **センサー** &ndash;ユーザーが自動的に回転を無効な場合でも、センサーが決定する、アプリケーションの向きをによりします。
- **SensorLandscape** &ndash;により、アプリケーションの画面が (ようにとして上下逆に、画面が表示されていない) に直接つながっている方向を変更するセンサー データを使用しているときに横向きに印刷します。
- **SensorPortrait** &ndash;により、アプリケーションの画面が (ようにとして上下逆に、画面が表示されていない) に直接つながっている方向を変更するセンサー データを使用しているときに縦向きを使用します。
- **ReverseLandscape** &ndash;により、アプリケーションは通常、「上下逆にします」が表示するための反対方向が直面している、横方向を使用するには。
- **ReversePortrait** &ndash;により、アプリケーションは通常、「上下逆にします」が表示するための反対方向が直面している、縦向きを使用するには。
- **FullSensor** &ndash;により、アプリケーションは (外の考えられる 4) 正しい向きを選択するセンサー データに依存します。
- **FullUser** &ndash;により、ユーザーの印刷の向きの設定を使用するアプリケーション。 自動的に回転が有効になっている 4 つすべての向きを使用できます。
- **UserLandscape** &ndash; _\[はサポートされていません\]_によって、横方向を使用するアプリケーションをユーザーが有効にすると、自動的に回転を使用している場合、向きを決定するセンサーです。 このオプションは、コンパイルに中断されます。
- **UserPortrait** &ndash; _\[はサポートされていません\]_によって、縦向きを使用するアプリケーションをユーザーが有効にすると、自動的に回転を使用している場合、向きを決定するセンサーです。 このオプションは、コンパイルに中断されます。
- **ロックされている** &ndash; _\[はサポートされていません\]_により、アプリケーションは、画面の向きを使用するものは、起動時にデバイスでの変更に応答することがなくの物理方向。 このオプションは、コンパイルに中断されます。

ネイティブの Android Api は、多数の向きを管理する方法を制御を提供する、基本設定を表す、ユーザーを明示的に矛盾するオプションを含むことに注意してください。

### <a name="windows-phone"></a>Windows Phone

Windows Phone RT でサポートされる向きの設定、 <span class="UIItem">Package.appxmanifest</span>ファイル。 マニフェストを開くと、サポートされる向きを選択できる構成パネルが表示されます。

![](device-orientation-images/vs-winrt-config.png "Package.appxmanifest Visual Editor")

Windows Phone 8 (Silverlight) でサポートされる向きを設定して、コードで、 <span class="UIItem">MainPage.xaml.cs</span>ファイル。 既定のプロジェクト テンプレートでは、次のコード行で値が既に設定します。

```csharp
SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;
```

Windows Phone 上向きのオプションを指定するには、必要な向きを有効にするコードで置き換えること。

```csharp
SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;
SupportedOrientations = SupportedPageOrientation.Portrait; // portrait only
SupportedOrientations = SupportedPageOrientation.Landscape; // landscape only
```

注 (から見た縦)、Windows Phone が両方の横向きビューをサポートする方向を左から右へまたは右から左。 使用するを指定することはできません。

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>印刷の向きの変更に反応します。

Xamarin.Forms では、共有コードで向きの変更のアプリに通知するため、ネイティブのイベントを提供していません。 ただし、`SizeChanged`のイベント、`Page`ときに発生幅または高さのいずれか、`Page`変更します。 ときの幅、`Page`が高さよりも大きい、デバイスが横モードにします。 詳細については、次を参照してください。[画像を画面の向きに基づく表示](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)です。

> [!NOTE]
> 共有コードで印刷の向きの変更の通知を受信するため、既存の無料の NuGet パッケージがあります。 参照してください、 [GitHub リポジトリの](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation)詳細についてはします。

代わりに、オーバーライドされる可能性が、 [ `OnSizeAllocated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnSizeAllocated(System.Double,System.Double)/)メソッドを`Page`ロジックがありますを変更して任意のレイアウトを挿入します。 `OnSizeAllocated`メソッドが呼び出されるたびに、 `Page` whenver デバイスの回転で発生する、新しいサイズが割り当てられます。 なおの基本実装`OnSizeAllocated`するオーバーライドの基本実装を呼び出すことが重要であるため、重要なレイアウト機能を実行します。

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

その手順を実行するエラーは、機能していないページになります。

なお、`OnSizeAllocated`デバイスを回転したときに、メソッドで何度もする呼び出すことがあります。 たびに、レイアウトを変更するリソースの浪費になります、ちらつきにつながることができます。 印刷の向きは横または縦、かどうかを追跡するために、ページ内のインスタンス変数の使用を検討し、変更があるときにのみ再描画します。

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

デバイスの向きの変更が検出されると、追加または使用可能な領域の変更に反応するため、ユーザー インターフェイスとの間追加のビューを削除する可能性があります。 たとえば、縦向きでは、各プラットフォームで組み込みの電卓を考慮してください。

![](device-orientation-images/calculator-portrait.png "縦向きで電卓アプリケーション")

ランドス ケープ:

![](device-orientation-images/calculator-landscape.png "横に電卓アプリケーション")

アプリの使用可能な領域を利用とりまく環境で多くの機能を追加することを確認します。

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>レスポンシブ レイアウト

デバイスを回転したときに正常に遷移する組み込みのレイアウトを使用してデザイン インターフェイスを行うことができます。 引き続き向きの変更に応答する場合に、魅力的なインターフェイスを設計するときは、次の一般的な規則を考慮してください。

- **比率に注意を払う**&ndash;向きの変更は、比に関する仮定が行われると、問題が発生します。 たとえば、縦向きで画面の縦方向の空白の 1/3 に十分な領域は、ビューは可能性がありますランドス ケープの垂直方向のスペースの 1/3 に適合しません。
- **絶対値で指定するには注意する**&ndash;縦向きで意味のある絶対値 (ピクセル) で指定することがあります、とりまく環境で意味を持ちません。 絶対値が必要なときに、その影響を分離するのに入れ子になったレイアウトを使用します。 たとえば、ことができますの絶対値を使用する、 `TableView` `ItemTemplate`項目テンプレートが保証される uniform 高さが場合。

上記の規則は、ベスト プラクティスと見なされる一般的な複数の画面サイズとは、インターフェイスを実装するときにも適用されます。 このガイドの残りの部分では、xamarin.forms を使用して、プライマリのレイアウトの各応答のレイアウトの具体的な例についてを説明します。

> [!NOTE]
> 次のセクションでは、デモンストレーションの 1 つの種類を使用して応答のレイアウトを実装する方法をわかりやすくするため、`Layout`一度にです。 実際はできるだけを混在させる`Layout`単純または最も直観的なを使用して目的のレイアウトを実現するために s`Layout`コンポーネントごとにします。

### <a name="stacklayout"></a>StackLayout

縦に表示される、次のアプリケーションについて考えてみます。

![](device-orientation-images/photo-stack-portrait.png "縦向きでフォト アプリケーション")

ランドス ケープ:

![](device-orientation-images/photo-stack-landscape.png "写真のアプリケーション (横向き)")

次の xaml を行われます。

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

いくつか c# 使用の方向を変更する`outerStack`デバイスの向きに基づきます。

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

- `outerStack` 最適な活用するために使用可能な領域の向きに応じて水平または垂直スタックとしてイメージとコントロールを表示するには、調整されます。


### <a name="absolutelayout"></a>AbsoluteLayout

縦に表示される、次のアプリケーションについて考えてみます。

![](device-orientation-images/photo-abs-portrait.png "縦向きでフォト アプリケーション")

ランドス ケープ:

![](device-orientation-images/photo-abs-landscape.png "写真のアプリケーション (横向き)")

次の xaml を行われます。

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

- ページのレイアウトされているため、応答性を紹介する手順のコードの必要はありません。
- `ScrollView`が場合でも、画面の高さ固定のボタンと、イメージの高さの合計より小さいかを表示するラベルを許可するが使用されています。


### <a name="relativelayout"></a>RelativeLayout

縦に表示される、次のアプリケーションについて考えてみます。

![](device-orientation-images/photo-rel-portrait.png "縦向きでフォト アプリケーション")

ランドス ケープ:

![](device-orientation-images/photo-rel-landscape.png "写真のアプリケーション (横向き)")

次の xaml を行われます。

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

- ページのレイアウトされているため、応答性を紹介する手順のコードの必要はありません。
- `ScrollView`が場合でも、画面の高さ固定のボタンと、イメージの高さの合計より小さいかを表示するラベルを許可するが使用されています。

### <a name="grid"></a>グリッド

縦に表示される、次のアプリケーションについて考えてみます。

![](device-orientation-images/photo-grid-portrait.png "縦向きでフォト アプリケーション")

ランドス ケープ:

![](device-orientation-images/photo-grid-landscape.png "写真のアプリケーション (横向き)")

次の xaml を行われます。

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

- ページのレイアウトされているため、コントロールのグリッド配置を変更する方法があります。


## <a name="related-links"></a>関連リンク

- [レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble 例 (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [レスポンシブ レイアウト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [画面の向きに基づくイメージを表示します。](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)
