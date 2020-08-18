---
title: Xamarin.Forms シェルのレイアウト
description: シェル アプリケーションにおけるポップアップの次のレベルのナビゲーションは、下部のタブ バーです。 または、アプリケーションのナビゲーション パターンを下部のタブから始めて、ポップアップを使用しないようにすることができます。 どちらの場合も下部のタブに複数のページが含まれる場合は、上部のタブからページをナビゲートできます。
ms.prod: xamarin
ms.assetid: 318D81DB-E456-4E44-B083-36A27DBD9523
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/06/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9ecdc3aca3264b52163d35e29659f434f521147f
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918622"
---
# <a name="no-locxamarinforms-shell-tabs"></a>Xamarin.Forms シェルのタブ

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

アプリケーションのナビゲーション パターンに、ポップアップが含まれている場合、アプリケーションにおける次のレベルのナビゲーションは、下部のタブ バーです。 さらに、ポップアップを閉じた場合、下部のタブ バーを最上位のナビゲーションと見なすことができます。

または、アプリケーションのナビゲーション パターンを下部のタブから始めて、ポップアップを使用しないようにすることができます。 このシナリオでは、`Shell` オブジェクトの子を、下部のタブ バーを表す `TabBar` オブジェクトにする必要があります。

> [!NOTE]
> `TabBar` 型では、ポップアップが無効になります。

各 `FlyoutItem` または `TabBar` オブジェクトには 1 つ以上の `Tab` オブジェクトを含めることができ、各 `Tab` オブジェクトは下部のタブ バー上の 1 つのタブを表します。 各 `Tab` オブジェクトには 1 つ以上の `ShellContent` オブジェクトを含めることができ、各 `ShellContent` オブジェクトにより 1 つの [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが表示されます。 複数の `ShellContent` オブジェクトが `Tab` オブジェクト内にある場合は、上部タブから `ContentPage` オブジェクトをナビゲートできます。

各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクト内で、追加の `ContentPage` オブジェクトに移動できます。 ナビゲーションの詳細については、「[Xamarin.Forms シェルのナビゲーション](navigation.md)」を参照してください。

## <a name="single-page-application"></a>シングル ページ アプリケーション

最もシンプルなシェル アプリケーションはシングル ページ アプリケーションです。これは `TabBar` オブジェクトに `Tab` オブジェクトを 1 つ追加することで作成できます。 `Tab` オブジェクト内で、`ShellContent` オブジェクトを [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトに設定する必要があります。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </TabBar>
</Shell>
```

このコード例により、次のシングル ページ アプリケーションが得られます。

[![iOS および Android 上のシェル シングル ページ アプリのスクリーンショット](tabs-images/single-page-app.png "シェル シングル ページ アプリ")](tabs-images/single-page-app-large.png#lightbox "シェル シングル ページ アプリ")

> [!NOTE]
> 必要に応じてナビゲーション バーを非表示にすることができます。それには、[`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクト上で `Shell.NavBarIsVisible` 添付プロパティを `false` に設定します。

シェルには暗黙的な変換演算子が備わっています。これにより、ビジュアル ツリーに追加のビューを導入することなくシェルの視覚階層を単純化できます。 これが可能になるのは、サブクラス化された `Shell` オブジェクトには `FlyoutItem` オブジェクトまたは `TabBar` オブジェクトしか含めることができず、それには `Tab` オブジェクトしか含めることができず、それには `ShellContent` オブジェクトしか含めることができないためです。 このような暗黙的な変換演算子を使って、前の例から `TabBar`、`Tab`、`ShellContent` オブジェクトを削除することができます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell"
       FlyoutBehavior="Disabled">
    <views:CatsPage />
</Shell>
```

この暗黙的な変換では、[`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが自動的に `ShellContent` オブジェクトでラップされ、それが `Tab` オブジェクトでラップされ、それが `FlyoutItem` オブジェクトでラップされます。 シングル ページ アプリケーションではポップアップは不要です。そのため、`Shell.FlyoutBehavior` プロパティを `Disabled` に設定しています。

> [!IMPORTANT]
> シェル アプリケーションでは、`ShellContent` オブジェクトの子である各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) は、アプリケーションの起動中に作成されます。 この手法を利用してその他の `ShellContent` オブジェクトを追加すると、アプリケーションの起動時に追加のページが作成され、起動エクスペリエンスの低下を引き起こす場合があります。 ただし、シェルでは、ナビゲーションに応答して、必要に応じてページを作成することも可能です。 詳細については、「[効率的なページの読み込み](tabs.md#efficient-page-loading)」をご覧ください。

## <a name="bottom-tabs"></a>下部のタブ

1 つの `TabBar` オブジェクト内に複数の `Tab` オブジェクトがある場合、`Tab` オブジェクトは下部のタブとしてレンダリングされます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
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
    </TabBar>
</Shell>
```

タブのタイトルとアイコンは各 `Tab` オブジェクト上で設定され、下部のタブに表示されます。

[![下部タブが表示された、iOS および Android 上のシェル 2 ページ アプリのスクリーンショット](tabs-images/two-page-app-bottom-tabs.png "下部タブが表示されたシェル 2 ページ アプリ")](tabs-images/two-page-app-bottom-tabs-large.png#lightbox "下部タブが表示されたシェル 2 ページ アプリ")

6 個以上のタブがある場合、 **[その他]** タブが表示され、これを使用して追加のタブにアクセスできます。

[![[その他] タブが表示された、iOS および Android 上のシェル アプリのスクリーン ショット](tabs-images/more-tabs.png "[その他] タブが表示されたシェル アプリ")](tabs-images/more-tabs-large.png#lightbox "[その他] タブが表示されたシェル アプリ")

または、シェルの暗黙的な変換演算子を使って、前の例から `ShellContent` および `Tab` オブジェクトを削除することができます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <views:CatsPage IconImageSource="cat.png" />
        <views:DogsPage IconImageSource="dog.png" />
    </TabBar>
</Shell>
```

この暗黙的な変換では、各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトが自動的に `ShellContent` オブジェクトでラップされ、今度はそれが両方とも `Tab` オブジェクトでラップされます。

> [!IMPORTANT]
> シェル アプリケーションでは、`ShellContent` オブジェクトの子である各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) は、アプリケーションの起動中に作成されます。 この手法を利用してその他の `ShellContent` オブジェクトを追加すると、アプリケーションの起動時に追加のページが作成され、起動エクスペリエンスの低下を引き起こす場合があります。 ただし、シェルでは、ナビゲーションに応答して、必要に応じてページを作成することも可能です。 詳細については、「[効率的なページの読み込み](tabs.md#efficient-page-loading)」をご覧ください。

### <a name="tab-class"></a>Tab クラス

`Tab` クラスには、タブの外観と動作を制御する次のプロパティが含まれています。

- `CurrentItem`: `ShellContent` 型、選択された項目。
- `FlyoutDisplayOptions`: `FlyoutDisplayOptions` 型、ポップアップで項目とその子を表示する方法を定義します。 既定値は `AsSingleItem` です。
- `FlyoutIcon`: `ImageSource` 型、ポップアップに表示するアイコンを定義します。
- `Icon`: `ImageSource` 型、ポップアップではないクロムのパーツに表示するアイコンを定義します。
- `IsChecked`: `boolean` 型、現在ポップアップで項目を強調表示するかどうかを定義します。
- `IsEnabled`: `boolean` 型、クロム内の項目を選択できるかどうかを定義します。
- `IsTabStop`: `bool` 型、タブのナビゲーションに `Tab` を含めるかどうかを指定します。 既定値は `true` で、値を `false` にすると、`Tab` は、`TabIndex` が設定されているかどうかにかかわらず、タブ ナビゲーション インフラストラクチャによって無視されます。
- `Items`: `IList<ShellContent>` 型、`Tab` 内のすべてのコンテンツを定義します。
- `TabIndex`: `int` 型、ユーザーが Tab キーを押して項目間を移動するときに、`Tab` オブジェクトがフォーカスを受け取る順序を指定します。 このプロパティの既定値は 0 です。
- `Title`: `string` 型、UI のタブに表示するタイトル。

## <a name="shell-content"></a>シェル コンテンツ

すべての `Tab` オブジェクトの子は `ShellContent` オブジェクトで、その `Content` プロパティは [`ContentPage`](xref:Xamarin.Forms.ContentPage) に設定されます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
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
    </TabBar>
</Shell>
```

各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクト内で、追加の `ContentPage` オブジェクトに移動できます。 ナビゲーションの詳細については、「[Xamarin.Forms シェルのナビゲーション](navigation.md)」を参照してください。

> [!NOTE]
> 各 `ShellContent` オブジェクトの [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) は、親 `Tab` オブジェクトから継承されます。

### <a name="shellcontent-class"></a>ShellContent クラス

`ShellContent` クラスには、タブのコンテンツの外観と動作を制御する次のプロパティが含まれています。

- `Content`: `object` 型、`ShellContent` のコンテンツ。
- `ContentTemplate`: `DataTemplate` 型、`ShellContent` のコンテンツを動的に拡張するために使うテンプレート。
- `FlyoutIcon`: `ImageSource` 型、ポップアップに表示するアイコンを定義します。
- `Icon`: `ImageSource` 型、ポップアップではないクロムのパーツに表示するアイコンを定義します。
- `IsChecked`: `boolean` 型、現在ポップアップで項目を強調表示するかどうかを定義します。
- `IsEnabled`: `boolean` 型、クロム内の項目を選択できるかどうかを定義します。
- `IsVisible`: `bool` 型、`ShellContent` がすべての UI 構造で非表示になっているかどうかを示します。 既定値は `true` です。
- `MenuItems`: `MenuItemCollection` 型、この `ShellContent` が表示ページであるときに、ポップアップに表示するメニュー項目。
- `Title`: `string` 型、UI に表示するタイトル。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティはデータ バインディングの対象にすることができます。

## <a name="bottom-and-top-tabs"></a>下部と上部のタブ

`Tab` オブジェクト内に複数の `ShellContent` オブジェクトがある場合、下部のタブに上部タブ バーが追加されます。これを使って [`ContentPage`](xref:Xamarin.Forms.ContentPage) オブジェクトを移動できます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
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
    </TabBar>
</Shell>
```

これで、次のスクリーンショットに示すようなレイアウトになります。

[![上部タブと下部タブが表示された、iOS および Android 上のシェル 2 ページ アプリのスクリーンショット](tabs-images/two-page-app-top-tabs.png "上部タブと下部タブが表示されたシェル 2 ページ アプリ")](tabs-images/two-page-app-top-tabs-large.png#lightbox "上部タブと下部タブが表示されたシェル 2 ページ アプリ")

または、シェルの暗黙的な変換演算子を使って、前の例から `ShellContent` オブジェクトと 2 つ目の `Tab` オブジェクトを削除することができます。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <TabBar>
        <Tab Title="Domestic"
             Icon="domestic.png">
            <views:CatsPage />
            <views:DogsPage />
        </Tab>
        <views:MonkeysPage IconImageSource="monkey.png" />
    </TabBar>
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
    <TabBar>
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
    </TabBar>    
</Shell>
```

この XAML では `CatsPage` を作成して表示していますが、それは、これがサブクラス化された `Shell` オブジェクト内で宣言されたコンテンツの最初の項目であるためです。 下部のタブを使って `DogsPage` と `MonkeysPage` に移動できます。そのページは、ユーザーがそれらに移動したときにのみ作成されます。 この方法の利点は、起動エクスペリエンスの低下を回避できることです。ページが、アプリケーションの起動時ではなく、必要に応じてナビゲーションへの応答として作成されるためです。

## <a name="tab-appearance"></a>タブの外観

`Shell` クラスでは、タブの外観を制御する次の添付プロパティを定義しています。

- `TabBarBackgroundColor`:`Color` 型、タブ バーの背景色を定義します。 プロパティが設定されていない場合、`BackgroundColor` プロパティ値が使われます。
- `TabBarDisabledColor`:`Color` 型、タブ バーの無効な色を定義します。 プロパティが設定されていない場合、`DisabledColor` プロパティ値が使われます。
- `TabBarForegroundColor`:`Color` 型、タブ バーの前景色を定義します。 プロパティが設定されていない場合、`ForegroundColor` プロパティ値が使われます。
- `TabBarTitleColor`:`Color` 型、タブ バーのタイトルの色を定義します。 プロパティが設定されていない場合、`TitleColor` プロパティ値が使われます。
- `TabBarUnselectedColor`:`Color` 型、タブ バーの選択されていない色を定義します。 プロパティが設定されていない場合、`UnselectedColor` プロパティ値が使われます。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティをデータ バインディングの対象にして、スタイル設定することができます。

次の例は、さまざまなタブの色のプロパティを設定する XAML スタイルを示しています。

```xaml
<Style x:Key="BaseStyle"
       TargetType="Element">
    <Setter Property="Shell.TabBarBackgroundColor"
            Value="#3498DB" />
    <Setter Property="Shell.TabBarTitleColor"
            Value="White" />
    <Setter Property="Shell.TabBarUnselectedColor"
            Value="#B4FFFFFF" />
</Style>
```

さらに、タブはカスケード スタイル シート (CSS) を使ってスタイル設定することもできます。 詳細については、「[Xamarin.Forms シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms シェルのナビゲーション](navigation.md)
- [Xamarin.Forms CSS シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
