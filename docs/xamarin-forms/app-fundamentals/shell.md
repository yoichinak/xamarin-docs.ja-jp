---
title: Xamarin.Forms シェル
description: この記事では、Xamarin.Forms シェルの使用方法について説明します。このシェルによって、ほとんどのアプリケーションが必要とする基本的な UI 機能が提供され、Xamarin.Forms アプリケーションの複雑さが軽減されます。
ms.prod: xamarin
ms.assetid: 1A674212-72DB-4AA4-B626-A4EC135AD1A0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2018
ms.openlocfilehash: 41530399bfc2210e7c3eda461688c12c6235ef79
ms.sourcegitcommit: 56b2f5cda7c37874618736d6129f19a8976826f0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2019
ms.locfileid: "54418674"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms シェル

![[プレビュー]](~/media/shared/preview.png)

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/Microsoft/TailwindTraders-Mobile)

Xamarin.Forms シェルは、アプリケーションのコンテナーです。これにより、アプリケーションの主要なワークロードに集中したままで、ほとんどのアプリケーションで必要とされる基本的な UI 機能が提供されます。 シェル アプリケーションは、次の機能で提供されます。

- アプリケーションの視覚的な構造を示す 1 つの場所
- 一般的なナビゲーション ユーザー インターフェイス
- ディープ リンクの設定を含むナビゲーション サービス
- 統合された検索ハンドラー

この機能によって、アプリケーションの複雑さが軽減され、開発者の生産性が向上します。 さらに、シェルはレンダリング速度とメモリ消費量を念頭に置いて記述されます。

> [!IMPORTANT]
> 既存の iOS と Android アプリケーションではシェルを採用することができ、ナビゲーション、パフォーマンス、および拡張性の強化をすぐに活用できます。

シェルでは、ポップアップとタブに基づく、厳密なナビゲーション ユーザー インターフェイスが提供されます。 シェル アプリケーション内のナビゲーションの上位レベルはポップアップです。

![ポップアップ](shell-images/flyout.png "ポップアップ")

ナビゲーションの次のレベルは、下部タブ バーです。

![下部タブ](shell-images/bottom-tabs-full.png "下部タブ")

ポップアップが開かない場合は、下部タブ バーはナビゲーションの上部レベルと考慮されます。

それぞれの下部タブ内で、各タブが `ContentPage` の場合、次のナビゲーション レベルは上部タブ バーになります。

![上部タブ](shell-images/top-tabs-full.png "上部タブ")

それぞれの `ContentPage` 内で、追加の `ContentPage` インスタンスをナビゲーション スタックに追加したり、ナビゲーション スタックから削除したりすることができます。

## <a name="bootstrapping-a-shell-application"></a>シェル アプリケーションをブートストラップする

`App` クラスの `MainPage` プロパティをシェル ファイルのインスタンスに設定することで、シェル アプリケーションがブートストラップされます。

```csharp
namespace TailwindTraders.Mobile
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();

            MainPage = new TheShell();
        }
    }
}
```

`TheShell` クラスは、ご利用のアプリケーションの視覚的な構造を示す XAML ファイルです。

> [!IMPORTANT]
> 現在、シェルは試験段階であり、`Forms.Init` メソッドを呼び出す前にご利用のプラットフォーム プロジェクトに `Forms.SetFlags("Shell_Experimental");` を追加することによってのみ使用できます。

# <a name="androidtabandroid"></a>[Android](#tab/android)

```csharp
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        global::Xamarin.Forms.Forms.SetFlags("Shell_Experimental");

        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);

        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App());
    }
}
```

# <a name="iostabios"></a>[iOS](#tab/ios)

```csharp
public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate
{
    public override bool FinishedLaunching(UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.SetFlags("Shell_Experimental");

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new App());

        return base.FinishedLaunching(app, options);
    }
}
```

----

## <a name="shell-file-hierarchy"></a>シェル ファイルの階層

シェル ファイルは、次の 3 つの階層形式の要素で構成されています。

- `ShellItem`。 ポップアップ内の 1 つまたは複数の項目。 すべての `ShellItem` は `Shell` の子です。
- `ShellSection`。 下部タブで移動できるグループ化されたコンテンツ。 すべての `ShellSection` は `ShellItem` の子です。
- `ShellContent`。 上部タブで移動できるご利用のアプリケーションの `ContentPage` インスタンス。 すべての `ShellContent` は `ShellSection` の子です。

これらの要素はどれもユーザー インターフェイスを表しませんが、アプリケーションの視覚的な構造の組織を表します。 シェルではこれらの要素を取得し、コンテンツにナビゲーション ユーザー インターフェイスを生成します。

次の XAML では、シェル ファイルのシンプルな例を示しています。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Mobile.Features.Shell"
       x:Class="TailwindTraders.Mobile.Features.Shell.TheShell"
       Title="TailwindTraders">
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    </ShellItem>
</Shell>
```

> [!NOTE]
> 各 `ShellItem` で `FlyoutIcon` プロパティを `ImageSource` に設定することもできます。これはタイトルの左側に表示されます。

この XAML では `HomePage` を表示します。これは、シェル ファイルで宣言されたコンテンツの最初の項目であるためです。

![ホーム ページ](shell-images/homepage.png "ホーム ページ")

ハンバーガー ボタンを押すと、ポップアップが表示されます。

![ポップアップを開く](shell-images/flyout-initial.png "ポップアップを開く")

ポップアップ内の項目数は、さらに `ShellItem` インスタンスをシェル ファイルに追加することによって増やすことができます。 しかし、`HomePage` はアプリケーションの起動時に作成され、この手法で `ShellItem` インスタンスを追加すると、これらのページもアプリケーションの起動時に作成されることに注意してください。 これは、`ShellContent.ContentTemplate` プロパティを `DataTemplate` に設定することで、禁止することができます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Mobile.Features.Shell"
       x:Class="TailwindTraders.Mobile.Features.Shell.TheShell"
       Title="TailwindTraders">
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    <ShellItem>
    <ShellContent Title="Profile"
                  ContentTemplate="{DataTemplate local:ProfilePage}" />
</Shell>
```

この手法では、アプリケーションの起動時ではなく、ユーザーがそのページに移動したときにのみ、`ProfilePage` が作成されます。 また、`ProfilePage` では、`Title` プロパティは `ShellContent` インスタンスで適切に定義され、`ShellItem` オブジェクトと `ShellSection` オブジェクトが省略されることに注意してください。 この簡潔な手法では、(ビジュアル ツリーに追加のビューを導入することなく) シェルで必須の論理ラッパーが提供され、必要なマークアップが少なくて済みます。

さらに、各 `ShellItem` の外観は、`Shell.ItemTemplate` プロパティを `DataTemplate` に設定することでカスタマイズできます。

```xaml
<Shell.ItemTemplate>
    <DataTemplate>
        <ContentView HeightRequest="32">
            <ContentView.Padding>
                <Thickness Left="32"
                           Top="16" />
            </ContentView.Padding>
            <Label Text="{Binding Title}" />
        </ContentView>
    </DataTemplate>
</Shell.ItemTemplate>          
```

このコードでは、ポップアップ内の `ShellItem` ごとに単にテキストを再配置します。 シェルによって `DataTemplate` の `BindingContext` に `Title` (および `Icon`) プロパティが提供されることに注意してください。

## <a name="flyout"></a>ポップアップ

ポップアップはアプリケーションのルート メニューであり、ポップアップ ヘッダーとメニュー項目で構成されます。

![注釈付きのポップアップ](shell-images/flyout-annotated.png "注釈付きのポップアップ")

### <a name="flyout-behavior"></a>ポップアップの動作

ポップアップは、ハンバーガー ボタンから、または画面の側面からスワイプすることでアクセスできます。 しかし、`Shell.FlyoutBehavior` プロパティを `FlyoutBehavior` 列挙メンバーのいずれかに設定することで、この動作を変更することができます。

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

`FlyoutBehavior` プロパティを `Disabled` に設定すると、ポップアップが非表示になり、`ShellItem` が 1 つのみの場合に便利です。 他の有効な `FlyoutBehavior` 値は、`Flyout` (既定値)、および `Locked` です。

### <a name="flyout-header"></a>ポップアップのヘッダー

このポップアップ ヘッダーは、`Shell.FlyoutHeader` プロパティの値によって設定できる `View` で定義されている外観で、必要に応じて、ポップアップの上部に表示されるコンテンツです。

```xaml
<Shell.FlyoutHeader>
    <local:FlyoutHeader />
</Shell.FlyoutHeader>
```

代わりに、ポップアップ ヘッダーの外観は、`Shell.FlyoutHeaderTemplate` プロパティを `DataTemplate` に設定することで定義できます。

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <StackLayout HorizontalOptions="Fill"
                     VerticalOptions="Fill"
                     BackgroundColor="White"
                     Padding="16">
            <Label FontSize="Medium"
                   Text="Smart Shopping">
                <Label.Margin>
                    <Thickness Left="8" />
                </Label.Margin>
            </Label>
            <Button Image="photo"
                    Text="By taking a photo">
                <Button.Margin>
                    <Thickness Top="16" />
                </Button.Margin>
            </Button>
            <Button Image="ia"
                    Text="By using AR">
                <Button.Margin>
                    <Thickness Top="8" />
                </Button.Margin>
            </Button>
        </StackLayout>
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

この XAML の結果、次のポップアップ ヘッダーが得られます。

![ポップアップ ヘッダー](shell-images/flyout-header.png "ポップアップ ヘッダー")

既定では、ポップアップ ヘッダーはポップアップ内で固定されますが、十分な項目がある場合は以下のコンテンツはスクロールされます。 しかし、`Shell.FlyoutHeaderBehavior` プロパティを `FlyoutHeaderBehavior` 列挙メンバーのいずれかに設定することで、この動作を変更することができます。

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

`FlyoutHeaderBehavior` を `CollapseOnScroll` に設定すると、スクロールするときにポップアップが折りたたまれます。 他の有効な `FlyoutHeaderBehavior` の値は、`Default`、`Fixed`、および `Scroll` (メニュー項目でスクロール) です。

## <a name="menu-items"></a>メニュー項目

ポップアップ内の項目数は、さらに `ShellItem` インスタンスを追加することによって増やすことができます。 しかし、`MenuItem` インスタンスをポップアップに追加することもできます。 これにより、`MenuItem.CommandParameter` プロパティからデータを渡す間に、同じページに移動するなどのシナリオが許可されます。

`MenuItem` インスタンスは `Shell.MenuItems` コレクションに追加する必要があります。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:local="clr-namespace:TailwindTraders.Views"
       x:Class="TailwindTraders.Shell"
       FlyoutHeaderBehavior="Fixed"
       Title="Tailwind Traders"
       ...>
    <Shell.MenuItems>
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="1"
                  Text="Holiday decorations" />
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="2"
                  Text="Appliances" />
        <MenuItem Command="{Binding ProductTypeCommand}"
                  CommandParameter="3"
                  Text="Bathrooms" />
        ...
    </Shell.MenuItems>
    <ShellItem Title="Home">
        <ShellSection>
            <ShellContent>
                <local:HomePage />
            </ShellContent>
        </ShellSection>
    </ShellItem>    
</Shell>
```

> [!NOTE]
> 各 `MenuItem` で `Icon` プロパティを `FileImageSource` に設定することもできます。これはテキストの左側に表示されます。

この XAML の結果、次のポップアップが得られます。

![完全なポップアップ](shell-images/flyout-full.png "完全なポップアップ")

さらに、`MenuItem` の外観は、`Shell.MenuItemTemplate` プロパティを `DataTemplate` に設定することでカスタマイズできます。

```xaml
<Shell.MenuItemTemplate>
    <DataTemplate>
        <ContentView HeightRequest="32">
            <ContentView.Padding>
                <Thickness Left="32"
                           Top="16" />
            </ContentView.Padding>
            <Label Text="{Binding Text}"
                   FontAttributes="Bold" />
        </ContentView>
    </DataTemplate>
</Shell.MenuItemTemplate>
```

この結果、`MenuItem` インスタンスのテキストが太字で表示されます。

![太字のメニュー項目](shell-images/menuitems-bold.png "太字のメニュー項目")

## <a name="tabs"></a>タブ

1 つの `ShellItem` 内に複数の `ShellSection` インスタンスがある場合、`ShellSection` 項目は下部タブに表示されます。

```xaml
<ShellItem Title="Bottom Tab Sample"
           Style="{StaticResource BaseStyle}">
    <ShellSection Title="AR" Icon="ia.png">
        <ShellContent ContentTemplate="{DataTemplate local:ARPage}"/>
    </ShellSection>
    <ShellSection Title="Photo" Icon="photo.png">
        <ShellContent ContentTemplate="{DataTemplate local:PhotoPage}"/>
    </ShellSection>
</ShellItem>
```

この例では、`ShellSection` インスタンスは下部タブとして表示されます。

![下部タブ](shell-images/tabs-bottom.png "下部タブ")

1 つの `ShellSection` 内に複数の `ShellContent` インスタンスがある場合、`ShellContent` 項目は上部タブに表示されます。

```xaml
<ShellItem Title="Store Home"
           Shell.TitleView="Store Home"
           Style="{StaticResource BaseStyle}">
    <ShellSection Title="Browse Product">
        <ShellContent Title="Featured"
                      ContentTemplate="{DataTemplate local:FeaturedPage}" />
        <ShellContent Title="On Sale"
                      ContentTemplate="{DataTemplate local:SalePage}" />
    </ShellSection>
</ShellItem>
```

この例では、`ShellContent` インスタンスは上部タブとして表示されます。

![上部タブ](shell-images/tabs-top.png "上部タブ")

XAML スタイルを使用するか、カスタム レンダラーを指定することによって、タブのスタイルを設定することができます。 たとえば、次の例はタブの色を設定する XAML スタイルを示しています。

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.ShellTabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.ShellTabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.ShellTabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

## <a name="navigation"></a>ナビゲーション

シェルには、URI ベースのナビゲーション操作が含まれています。 URI では、ナビゲーション階層の設定に従うことなく、アプリケーション内の任意のページへの移動を許可する、強化されたナビゲーション操作が提供されます。 さらに、これにより、ナビゲーション スタックのすべてのページにアクセスすることなく、後方に移動する機能も提供されます。

この URI ベースのナビゲーションは、アプリケーション内の移動に使用される URI セグメントであるルートで実現されます。 シェル ファイルでは、ルート スキーマ、ルート ホスト、およびルートを宣言する必要があります。

```xaml
<Shell ...
       Route="tailwindtraders"
       RouteHost="www.microsoft.com"
       RouteScheme="app">
    ...
</Shell>
```

組み合わせると、`app://www.microsoft.com/tailwindtraders` ルート URI からの `RouteScheme`、`RouteHost`、および `Route` プロパティ値になります。

シェル ファイルの各要素は、プログラムのナビゲーションで使用できるルート プロパティを定義することもできます。

シェル ファイル コンストラクター、またはルートが呼び出される前に実行されるその他の任意の場所では、追加のルートをシェル要素 (`MenuItem` インスタンスなど) によって表されていない任意のページに対して明示的に登録できます。

```csharp
Routing.RegisterRoute("productcategory", typeof(ProductCategoryPage));
```

### <a name="implementing-navigation"></a>ナビゲーションの実装

メニュー項目でナビゲーションを実装するために使用できる `Command` プロパティが表示されます。

```csharp
public ICommand ProductTypeCommand { get; } = new Command<string>(NavigateToProductType);

static void NavigateToProductType(string typeId)
{
  (App.Current.MainPage as Xamarin.Forms.Shell).GoToAsync($"app:///tailwindtraders/productcategory?id={typeId}", true);
}
```

ナビゲーションを呼び出すには、`Application` クラスの `MainPage` プロパティからアプリケーション `Shell` への参照を取得する必要があります。 次に、引数として有効な URI を渡して、`GoToAsync` メソッドを呼び出すことにより、ナビゲーションを呼び出すことができます。 `GoToAsync` メソッドは `ShellNavigationState` オブジェクトを使用して移動されます。このオブジェクトは `string` または `Uri` から構築されます。

### <a name="passing-data"></a>データを渡す

クエリ文字列パラメーターからページ間でデータを渡すことができます。 クエリ プロパティの属性をクラスに追加するときに、シェルでは `ContentPage` または ViewModel にクエリ文字列パラメーター値を設定します。

```csharp
[QueryProperty("TypeID", "id")]
public partial class ProductCategoryPage : ContentPage
{
    private string _typeId;

    public ProductCategoryPage()
    {
        InitializeComponent();

        BindingContext = new ProductCategoryViewModel();
    }

    public string TypeID
    {
        get => _typeId;
        set => MyLabel.Text = value;
    }
}
```

`QueryProperty` 属性では、`TypeID` で `GoToAsync` メソッドの呼び出しの URI から `id` クエリ文字列パラメーターに渡されたデータを受け取ることを指定します。

### <a name="intercepting-navigation"></a>ナビゲーションのインターセプト

シェルでは、完了する前、および完了した後に、ナビゲーションのルーティングにフックする機能が提供されます。 これは、`Navigating` イベントと `Navigated` イベントに対するイベント ハンドラーをそれぞれ登録することで実現できます。

```xaml
<Shell ...
       Navigating="OnShellNavigating">
    ...
</Shell>
```

```csharp
void OnShellNavigating(object sender, ShellNavigatingEventArgs e)
{
  if (...)
  {
    e.Cancel(); // Cancel the navigation
  }
}
```

`ShellNavigatingEventArgs` クラスでは次のプロパティが提供されます。

| プロパティ | 型 | 説明 |
|---|---|---|
| 現在 | `ShellNavigationState` | 現在のページの URI。 |
| ソース | `ShellNavigationState` | ナビゲーションの発信元を表す URI。 |
| ターゲット | `ShellNavigationSource`  | ナビゲーションの発信先を表す URI。 |
| CanCancel  | `bool` | ナビゲーションをキャンセルできるかどうかを示す値。 |
| 取り消し済み  | `bool` | ナビゲーションがキャンセルされたかどうかを示す値。 |

さらに、`ShellNavigatingEventArgs` クラスでは `Cancel` メソッドを提供します。

`ShellNavigatedEventArgs` クラスでは次のプロパティが提供されます。

| プロパティ | 型 | 説明 |
|---|---|---|
| 現在 | `ShellNavigationState` | 現在のページの URI。 |
| 前へ| `ShellNavigationState` | 前のページの URI。 |
| ソース  | `ShellNavigationSource` | ナビゲーションの発信元を表す URI。 |

さらに、シェルでは、[戻る] ボタンを押すことをインターセプトするために使用できる、オーバーライド可能な `OnBackButtonPressed` メソッドが提供されます。

## <a name="related-links"></a>関連リンク

- [Tailwind Traders (サンプル)](https://github.com/Microsoft/TailwindTraders-Mobile)
