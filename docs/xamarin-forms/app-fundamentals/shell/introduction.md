---
title: Xamarin.Forms シェルの概要
description: Xamarin.Forms シェルでは、一般的なナビゲーション ユーザー エクスペリエンス、URI ベースのナビゲーション体系、統合された検索ハンドラーなど、ほとんどのアプリケーションが必要とする基本機能を提供しています。
ms.prod: xamarin
ms.assetid: 4604DCB5-83DA-458A-8B02-6508A740BE0E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 20d9fb79d03990824dd884b62138a3e29b3ee04f
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054482"
---
# <a name="xamarinforms-shell"></a>Xamarin.Forms シェル

![](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)

Xamarin.Forms シェルでは、次に示すようなほとんどのモバイル アプリケーションが必要としている基本機能を提供することで、モバイル アプリケーション開発の複雑さを軽減します。

- 1 つの場所にアプリケーションの視覚階層を示す機能。
- 一般的なナビゲーション ユーザー インターフェイス。
- アプリケーション内の任意のページへ移動できる URI ベースのナビゲーション体系。
- 統合された検索ハンドラー

さらに、シェル アプリケーションには、レンダリング速度が速くなり、メモリ使用量が減少するという利点があります。

> [!IMPORTANT]
> 既存の iOS と Android アプリケーションではシェルを採用することができ、ナビゲーション、パフォーマンス、および拡張性の強化をすぐに活用できます。

現在、シェルは試験段階であり、`Forms.Init` メソッドを呼び出す前にご利用のプラットフォーム プロジェクトに `Forms.SetFlags("Shell_Experimental");` を追加することによってのみ使用できます。

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

## <a name="shell-navigation-experience"></a>シェルのナビゲーション エクスペリエンス

シェルでは、ポップアップとタブに基づいて、厳格なナビゲーション エクスペリエンスを提供しています。 シェル アプリケーション内のナビゲーションの上位レベルはポップアップです。

[![iOS および Android 上での、シェルのポップアップのスクリーンショット](introduction-images/flyout.png "シェルのポップアップ")](introduction-images/flyout-large.png#lightbox "シェルのポップアップ")

ポップアップ項目を選択すると、項目が選択され表示された状態で、下部のタブが表示されます。

[![iOS および Android 上での、シェルの下部タブのスクリーンショット](introduction-images/monkeys.png "シェルの下部タブ")](introduction-images/monkeys-large.png#lightbox "シェルの下部タブ")

> [!NOTE]
> ポップアップが開いていないときは、下部のタブ バーはアプリケーション内の上位レベルのナビゲーションと見なされます。

各タブには、[`ContentPage`](xref:Xamarin.Forms.ContentPage) が表示されます。 ただし、下部のタブに複数のページが含まれる場合は、上部のタブ バーからページをナビゲートできます。

[![iOS および Android 上での、シェルの上部タブのスクリーンショット](introduction-images/cats.png "シェルの上部タブ")](introduction-images/cats-large.png#lightbox "シェルの上部タブ")

各タブ内で、追加の [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトに移動できます。

[![iOS および Android 上での、シェルのページ ナビゲーションのスクリーンショット](introduction-images/cat-details.png "シェル アプリ ナビゲーション")](introduction-images/cat-details-large.png#lightbox "シェル アプリ ナビゲーション")

## <a name="subclassing-the-shell-class"></a>シェル クラスをサブクラス化する

サブクラス化された `Shell` オブジェクトは、シェル アプリケーションの視覚階層を表しており、3 つの主要な階層オブジェクトから構成されます。

- `FlyoutItem`、ポップアップ内の 1 つまたは複数の項目を表します。 どの `FlyoutItem` オブジェクトも、`Shell` オブジェクトの子です。
- `Tab`、下部のタブによって移動できるグループ化されたコンテンツを表します。 どの `Tab` オブジェクトも、`FlyoutItem` オブジェクトの子です。
- `ShellContent`、お使いのアプリケーションにある `ContentPage` オブジェクトを表します。 どの `ShellContent` オブジェクトも、`Tab` オブジェクトの子です。 複数の `ShellContent` オブジェクトが `Tab` にある場合は、上部タブからオブジェクトをナビゲートできます。

これらのオブジェクトはどれもユーザー インターフェイスを表すのではなく、アプリケーションの視覚階層の編成を表します。 シェルでは、これらのオブジェクトを取得して、コンテンツに応じたナビゲーション ユーザー インターフェイスを生成します。

> [!NOTE]
> `FlyoutItem` クラスは `ShellItem` クラスの別名であり、`Tab` クラスは `ShellSection` クラスの別名です。 これらの別名は、API をよりわかりやすく使用できるように、設計されています。

次の XAML では、サブクラス化された `Shell` オブジェクトの例を示します。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    ...
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png">
                <views:CatsPage />
            </ShellContent>
            <ShellContent Title="Dogs"
                          Icon="dog.png">
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png">
            <views:MonkeysPage />
        </ShellContent>
        <ShellContent Title="Elephants"
                      Icon="elephant.png">  
            <views:ElephantsPage />
        </ShellContent>
        <ShellContent Title="Bears"
                      Icon="bear.png">
            <views:BearsPage />
        </ShellContent>
    </FlyoutItem>
    ...
</Shell>
```

サブクラス化された `Shell` オブジェクト内で宣言されたコンテンツの最初の項目であるため、実行時に、この XAML では `CatsPage` を表示します。

[![iOS および Android 上での、シェル アプリのスクリーン ショット](introduction-images/cats.png "シェル アプリ")](introduction-images/cats-large.png#lightbox "シェル アプリ")

ハンバーガーのアイコンを押すか、左からスワイプして、ポップアップを表示します。

[![iOS および Android 上での、シェル ポップアップのスクリーンショット](introduction-images/flyout-reduced.png "シェル ポップアップ")](introduction-images/flyout-reduced-large.png#lightbox "シェル ポップアップ")

> [!IMPORTANT]
> シェル アプリケーションでは、`ShellContent` オブジェクトの子である各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) は、アプリケーションの起動中に作成されます。 この手法を利用して追加の `ShellContent` オブジェクトを付け加えると、アプリケーションの起動時に追加のページが作成され、起動エクスペリエンスの低下を引き起こす場合があります。 ただし、シェルでは、ナビゲーションに応答して、オンデマンドでページを作成することも可能です。 詳しくは、「[Xamarin.Forms Shell Tabs (Xamarin.Forms シェルのタブ)](tabs.md)」ガイドにある「[Efficient page loading (効率的なページの読み込み)](tabs.md#efficient-page-loading)」をご覧ください。

## <a name="bootstrapping-a-shell-application"></a>シェル アプリケーションをブートストラップする

`App` クラスの [`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティにサブクラス化された `Shell` オブジェクトを設定すると、シェル アプリケーションがブートストラップされます。

```csharp
namespace Xaminals
{
    public partial class App : Application
    {
        public App()
        {
            InitializeComponent();
            MainPage = new AppShell();
        }
        ...
    }
}
```

この例では、`AppShell` クラスは `Shell` クラスから派生した XAML ファイルであり、アプリケーションの視覚階層を表します。

## <a name="shell-appearance"></a>シェルの外観

`Shell` クラスでは、シェル アプリケーションの外観を制御する次のプロパティを定義しています。

- `ShellBackgroundColor`: `Color` 型、シェル クロームの背景色を定義する添付プロパティ。 シェル コンテンツの背後は、色で塗りつぶされません。
- `ShellDisabledColor`: `Color` 型、無効化されているテキストとアイコンを網掛けで表示するための色を定義する添付プロパティ。
- `ShellForegroundColor`: `Color` 型、テキストとアイコンを網掛けで表示するための色を定義する添付プロパティ。
- `ShellTitleColor`:、`Color` 型、現在のページのタイトルに使用される色を定義する添付プロパティ。
- `ShellUnselectedColor`: `Color` 型、シェル クロームにおいて、選択されていないテキストとアイコンに使用する色を定義する添付プロパティ。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティがデータ バインディングの対象になる場合があります。

さらに、これらのプロパティは、カスケード スタイル シート (CSS) を使用して設定できます。 詳しくは、「[Xamarin.Forms シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)」をご覧ください。

## <a name="shell-content-layout"></a>シェル コンテンツのレイアウト

`Shell` クラスでは、シェル アプリケーションのコンテンツのレイアウトに影響を与える次のプロパティを定義しています。

- `NavBarIsVisible`: `boolean`型、ページが表示されているときに、ナビゲーション バーを表示する必要があるかを定義する添付プロパティ。 このプロパティはページ上に設定する必要があり、既定値は `true` です。
- `SetPaddingInsets`: `bool` 型、任意のシェル クローム下にもページ コンテンツを表示するかを制御する添付プロパティ。 このプロパティはページ上に設定する必要があり、既定値は `false` です。
- `TabBarIsVisible`: `bool`型、ページが表示されているときに、タブ バーを表示する必要があるかを定義する添付プロパティ。 このプロパティはページ上に設定する必要があり、既定値は `true` です。
- `TitleView`: `View` 型、ページの `TitleView` を定義する添付プロパティ。 このプロパティは、ページ上に設定する必要があります。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティがデータ バインディングの対象になる場合があります。

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)
- [Xamarin.Forms シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
