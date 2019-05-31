---
title: Xamarin.Forms シェルのレイアウト
description: シェル アプリケーションにおけるポップアップの次のレベルのナビゲーションは、下部のタブ バーです。 タブに複数のページが含まれる場合は、上部のタブからページをナビゲートできます。
ms.prod: xamarin
ms.assetid: 318D81DB-E456-4E44-B083-36A27DBD9523
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: bc1ca01f4bf5cb8f7ef51c705319fb2cc1a0bd99
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054312"
---
# <a name="xamarinforms-shell-tabs"></a>Xamarin.Forms シェルのタブ

![](~/media/shared/preview.png "この API は現在プレリリースです")

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)

シェル アプリケーションにおけるポップアップの次のレベルのナビゲーションは、下部のタブ バーです。 または、ポップアップを閉じた場合、下部のタブ バーが最上位のナビゲーションと見なされます。

各 `FlyoutItem` オブジェクトには 1 つ以上の `Tab` オブジェクトを含めることができ、各 `Tab` オブジェクトは下部のタブ バー上の 1 つのタブを表します。 各 `Tab` オブジェクトには 1 つ以上の `ShellContent` オブジェクトを含めることができ、各 `ShellContent` オブジェクトにより 1 つの [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが表示されます。 複数の `ShellContent` オブジェクトが `Tab` オブジェクト内にある場合は、上部タブから `ContentPage` オブジェクトをナビゲートできます。

各 `ContentPage` オブジェクト内で、追加の `ContentPage` オブジェクトに移動できます。 ナビゲーションについて詳しくは、「[Xamarin.Forms シェルのナビゲーション](navigation.md)」をご覧ください。

## <a name="single-page-application"></a>シングル ページ アプリケーション

最もシンプルなシェル アプリケーションはシングル ページ アプリケーションです。これは `FlyoutItem` オブジェクトに `Tab` オブジェクトを 1 つ追加することで作成できます。 `Tab` オブジェクト内で、`ShellContent` オブジェクトを [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトに設定する必要があります。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

このコード例により、次のシングル ページ アプリケーションが得られます。

[![iOS と Android でのシェルのシングル ページ アプリのスクリーンショット](tabs-images/single-page-app.png "シェルのシングル ページ アプリ")](tabs-images/single-page-app-large.png#lightbox "シェルのシングル ページ アプリ")

シングル ページ アプリケーションではポップアップは不要です。そのため、`Shell.FlyoutBehavior` プロパティを `Disabled` に設定しています。

> [!NOTE]
> 必要に応じてナビゲーション バーを非表示にすることができます。それには、[`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクト上で `Shell.NavBarIsVisible` 添付プロパティを `false` に設定します。

シェルには暗黙的な変換演算子が備わっています。これにより、ビジュアル ツリーに追加のビューを導入することなくシェルの視覚階層を単純化できます。 これが可能になるのは、サブクラス化された `Shell` オブジェクトには `FlyoutItem` しか含めることができず、それには `Tab` オブジェクトしか含めることができず、それには `ShellContent` オブジェクトしか含めることができないためです。 このような暗黙的な変換演算子を使って、前の例から `FlyoutItem`、`Tab`、`ShellContent` オブジェクトを削除することができます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <views:CatsPage />
</Shell>
```

この暗黙的な変換では、[`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが自動的に `ShellContent` オブジェクトでラップされ、それが `Tab` オブジェクトでラップされ、それが `FlyoutItem` オブジェクトでラップされます。

> [!IMPORTANT]
> シェル アプリケーションでは、`ShellContent` オブジェクトの子である各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) は、アプリケーションの起動中に作成されます。 この手法を利用して追加の `ShellContent` オブジェクトを付け加えると、アプリケーションの起動時に追加のページが作成され、起動エクスペリエンスの低下を引き起こす場合があります。 ただし、シェルでは、ナビゲーションに応答して、必要に応じてページを作成することも可能です。 詳細については、「[効率的なページの読み込み](tabs.md#efficient-page-loading)」をご覧ください。

## <a name="bottom-tabs"></a>下部のタブ

1 つの `FlyoutItem` オブジェクト内に複数の `Tab` オブジェクトがある場合、`Tab` オブジェクトは下部のタブとしてレンダリングされます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

タブのタイトルとアイコンは各 `Tab` オブジェクト上で設定され、下部のタブに表示されます。

[![iOS と Android での、下部のタブを備えたシェルの 2 ページ アプリのスクリーンショット](tabs-images/two-page-app-bottom-tabs.png "下部のタブを備えたシェルの 2 ページ アプリ")](tabs-images/two-page-app-bottom-tabs-large.png#lightbox "下部のタブを備えたシェルの 2 ページ アプリ")

または、シェルの暗黙的な変換演算子を使って、前の例から `ShellContent` および `Tab` オブジェクトを削除することができます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <views:CatsPage Icon="cat.png" />
        <views:DogsPage Icon="dog.png" />
    </FlyoutItem>
</Shell>
```

この暗黙的な変換では、各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが自動的に `ShellContent` オブジェクトでラップされ、今度はそれが両方とも `Tab` オブジェクトでラップされます。

> [!IMPORTANT]
> シェル アプリケーションでは、`ShellContent` オブジェクトの子である各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) は、アプリケーションの起動中に作成されます。 この手法を利用して追加の `ShellContent` オブジェクトを付け加えると、アプリケーションの起動時に追加のページが作成され、起動エクスペリエンスの低下を引き起こす場合があります。 ただし、シェルでは、ナビゲーションに応答して、必要に応じてページを作成することも可能です。 詳細については、「[効率的なページの読み込み](tabs.md#efficient-page-loading)」をご覧ください。

### <a name="tab-class"></a>Tab クラス

`Tab` クラスには、タブの外観と動作を制御する次のプロパティが含まれています。

- `CurrentItem`: `ShellContent` 型、選択された項目。
- `FlyoutDisplayOptions`: `FlyoutDisplayOptions` 型、ポップアップで項目とその子を表示する方法を定義します。 既定値は `AsSingleItem` です。
- `FlyoutIcon`: `ImageSource` 型、ポップアップに表示するアイコンを定義します。
- `Icon`: `ImageSource` 型、ポップアップではないクロムのパーツに表示するアイコンを定義します。
- `IsChecked`: `boolean` 型、現在ポップアップで項目を強調表示するかどうかを定義します。
- `IsEnabled`: `boolean` 型、クロム内の項目を選択できるかどうかを定義します。
- `Items`: `ShellContentCollection` 型、`Tab` 内のすべてのコンテンツを定義します。
- `Title`: `string` 型、UI のタブに表示するタイトル。

## <a name="shell-content"></a>シェル コンテンツ

すべての `Tab` オブジェクトの子は `ShellContent` オブジェクトで、その `Content` プロパティは [`ContentPage`](xref:Xamarin.Forms.ContentPage) に設定されます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Cats"
             Icon="cat.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Dogs"
             Icon="dog.png">
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

各 `ContentPage` オブジェクト内で、追加の `ContentPage` オブジェクトに移動できます。 ナビゲーションについて詳しくは、「[Xamarin.Forms シェルのナビゲーション](navigation.md)」をご覧ください。

### <a name="shellcontent-class"></a>ShellContent クラス

`ShellContent` クラスには、タブのコンテンツの外観と動作を制御する次のプロパティが含まれています。

- `Content`: `object` 型、`ShellContent` のコンテンツ。
- `ContentTemplate`: `DataTemplate` 型、`ShellContent` のコンテンツを動的に拡張するために使うテンプレート。
- `FlyoutIcon`: `ImageSource` 型、ポップアップに表示するアイコンを定義します。
- `Icon`: `ImageSource` 型、ポップアップではないクロムのパーツに表示するアイコンを定義します。
- `IsChecked`: `boolean` 型、現在ポップアップで項目を強調表示するかどうかを定義します。
- `IsEnabled`: `boolean` 型、クロム内の項目を選択できるかどうかを定義します。
- `MenuItems`: `MenuItemCollection` 型、この `ShellContent` が表示ページであるときに、ポップアップに表示するメニュー項目。
- `Title`: `string` 型、UI に表示するタイトル。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティはデータ バインディングの対象にすることができます。

## <a name="bottom-and-top-tabs"></a>下部と上部のタブ

`Tab` オブジェクト内に複数の `ShellContent` オブジェクトがある場合、下部のタブに上部タブ バーが追加されます。これを使って `ContentPage` オブジェクトを移動できます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
        <Tab Title="Monkeys"
             Icon="monkey.png">
            <ShellContent>
                <views:MonkeysPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

これで、次のスクリーンショットに示すようなレイアウトになります。

[![iOS と Android での、上部と下部のタブを備えたシェルの 2 ページ アプリのスクリーンショット](tabs-images/two-page-app-top-tabs.png "上部と下部のタブを備えたシェルの 2 ページ アプリ")](tabs-images/two-page-app-top-tabs-large.png#lightbox "上部と下部のタブを備えたシェルの 2 ページ アプリ")

または、シェルの暗黙的な変換演算子を使って、前の例から `ShellContent` オブジェクトと 2 つ目の `Tab` オブジェクトを削除することができます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <FlyoutItem>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <views:CatsPage />
            <views:DogsPage />
        </Tab>
        <views:MonkeysPage Icon="monkey.png" />
    </FlyoutItem>
</Shell>
```

この暗黙的な変換では、`MonkeysPage` が自動的に `ShellContent` オブジェクトでラップされ、それが `Tab` オブジェクトでラップされます。 さらに、`CatsPage` と `DogsPage` は暗黙的に `ShellContent` オブジェクトでラップされます。

## <a name="efficient-page-loading"></a>効率的なページの読み込み

シェル アプリケーションでは、`ShellContent` オブジェクト内の [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトは、すべてアプリケーションの起動中に作成されます。これにより、起動エクスペリエンスが低下するおそれがあります。 しかし、シェルでは、ページの作成を必要に応じて、ナビゲーションへの応答として行うこともできます。 これを実現するには、`DataTemplate` マークアップ拡張を使って各 `ContentPage` を [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) に変換した後、その結果を `ShellContent.ContentTemplate` プロパティ値として設定します。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">    
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
    </FlyoutItem>    
</Shell>
```

この XAML では `CatsPage` を作成して表示していますが、それは、これがサブクラス化された `Shell` オブジェクト内で宣言されたコンテンツの最初の項目であるためです。 下部のタブを使って `CatsPage` と `MonkeysPage` に移動できます。そのページは、ユーザーがそれらに移動したときにのみ作成されます。 この方法の利点は、起動エクスペリエンスの低下を回避できることです。ページが、アプリケーションの起動時ではなく、必要に応じてナビゲーションへの応答として作成されるためです。

## <a name="tab-appearance"></a>タブの外観

`Shell` クラスでは、タブの外観を制御する次のプロパティを定義しています。

- `ShellTabBarBackgroundColor`: `Color` 型、タブ バーの背景色を定義する添付プロパティ。 プロパティが設定されていない場合、`ShellBackgroundColor` プロパティ値が使われます。
- `ShellTabBarDisabledColor`: `Color` 型、タブ バーの無効にされた色を定義する添付プロパティ。 プロパティが設定されていない場合、`ShellDisabledColor` プロパティ値が使われます。
- `ShellTabBarForegroundColor`: `Color` 型、タブ バーの前景色を定義する添付プロパティ。 プロパティが設定されていない場合、`ShellForegroundColor` プロパティ値が使われます。
- `ShellTabBarTitleColor`: `Color` 型、タブ バーのタイトルの色を定義する添付プロパティ。 プロパティが設定されていない場合、`ShellTitleColor` プロパティ値が使われます。
- `ShellTabBarUnselectedColor`: `Color` 型、タブ バーの選択されていない色を定義する添付プロパティ。 プロパティが設定されていない場合、`ShellUnselectedColor` プロパティ値が使われます。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティはデータ バインディングの対象にすることができます。

そのため、タブは XAML スタイルを使ってスタイル設定することができます。 次の例は、さまざまなタブの色のプロパティを設定する XAML スタイルを示しています。

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

さらに、タブはカスケード スタイル シート (CSS) を使ってスタイル設定することもできます。 詳しくは、「[Xamarin.Forms シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)」をご覧ください。

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)
- [Xamarin.Forms シェルのナビゲーション](navigation.md)
- [Xamarin.Forms シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
