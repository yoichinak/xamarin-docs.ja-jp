---
title: Xamarin.Forms シェルのページの構成
description: Shell クラスでは、Xamarin.Forms シェル アプリケーションのページの外観の構成に使える添付プロパティを定義できます。 これには、ページの色の設定、ナビゲーション バーの無効化、タブ バーの無効化、およびナビゲーション バーでのビューの表示が含まれます。
ms.prod: xamarin
ms.assetid: 3FC2FBD1-C30B-4408-97B2-B04E3A2E4F03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/05/2019
ms.openlocfilehash: e207949d607219393ffeb51fce818ddfb68ae344
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75489909"
---
# <a name="xamarinforms-shell-page-configuration"></a>Xamarin.Forms シェルのページの構成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

`Shell` クラスでは、Xamarin.Forms シェル アプリケーションのページの外観の構成に使える添付プロパティを定義できます。 これには、ページの色の設定、ナビゲーション バーの無効化、タブ バーの無効化、およびナビゲーション バーでのビューの表示が含まれます。

## <a name="set-page-colors"></a>ページの色を設定する

`Shell` クラスでは、シェル アプリケーションのページの色の設定に使える次の添付プロパティを定義できます。

- `BackgroundColor`: `Color` 型、シェル クロームの背景色を定義します。 シェル コンテンツの背後は、色で塗りつぶされません。
- `DisabledColor`: `Color` 型、無効化されているテキストとアイコンを網掛けで表示するための色を定義します。
- `ForegroundColor`: `Color` 型、テキストとアイコンを網掛けで表示するための色を定義します。
- `TitleColor`:、`Color` 型、現在のページのタイトルに使用される色を定義します。
- `UnselectedColor`: `Color` 型、シェル クロームにおいて、選択されていないテキストとアイコンに使用する色を定義します。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティをデータ バインディングの対象にして、XAML スタイルを使ってスタイル設定することができます。 さらに、このプロパティはカスケード スタイル シート (CSS) を使用して設定できます。 詳しくは、「[Xamarin.Forms シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)」をご覧ください。

> [!NOTE]
> タブの色を定義できるようにするプロパティもあります。 詳細については、「[タブの外観](tabs.md#tab-appearance)」をご覧ください。

次の XAML では、サブクラス化された `Shell` クラスでの色のプロパティの設定を示しています。

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell"
       BackgroundColor="#455A64"
       ForegroundColor="White"
       TitleColor="White"
       DisabledColor="#B4FFFFFF"
       UnselectedColor="#95FFFFFF">

</Shell>
```

この例では、ページ レベルでオーバーライドされない限り、色の値がシェル アプリケーションのすべてのページに適用されます。

色のプロパティは添付プロパティであるため、これらを個々のページ上で設定して、そのページの色を設定することもできます。

```xaml
<ContentPage ...
             Shell.BackgroundColor="Gray"
             Shell.ForegroundColor="White"
             Shell.TitleColor="Blue"
             Shell.DisabledColor="#95FFFFFF"
             Shell.UnselectedColor="#B4FFFFFF">

</ContentPage>
```

または、XAML スタイルを使って色のプロパティを設定することもできます。

```xaml
<Style x:Key="DomesticShell"
       TargetType="Element" >
    <Setter Property="Shell.BackgroundColor"
            Value="#039BE6" />
    <Setter Property="Shell.ForegroundColor"
            Value="White" />
    <Setter Property="Shell.TitleColor"
            Value="White" />
    <Setter Property="Shell.DisabledColor"
            Value="#B4FFFFFF" />
    <Setter Property="Shell.UnselectedColor"
            Value="#95FFFFFF" />
</Style>
```

XAML スタイルの詳細については、「[XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)」をご覧ください。

## <a name="enable-navigation-bar-shadow"></a>ナビゲーション バーの影を有効にする

`Shell` クラスでは、ナビゲーション バーを影付きにするかどうかを制御する、`bool` 型の添付プロパティ `NavBarHasShadow` を定義できます。 既定では、このプロパティの値は `false` です。

このプロパティはサブクラス化された `Shell` オブジェクト上で設定できますが、ナビゲーション バーの影を有効にする任意のページ上でも設定できます。 たとえば、次の XAML では、[`ContentPage`](xref:Xamarin.Forms.ContentPage) でのナビゲーション バーの影の有効化を示しています。

```xaml
<ContentPage ...
             Shell.NavBarHasShadow="true">
    ...
</ContentPage>
```

これにより、ナビゲーション バーの影が有効になります。

## <a name="disable-the-navigation-bar"></a>ナビゲーション バーを無効にする

`Shell` クラスでは、ページが表示されたときにナビゲーション バーを表示するかどうかを制御する、`bool` 型の添付プロパティ `NavBarIsVisible` を定義できます。 既定では、このプロパティの値は `true` です。

このプロパティはサブクラス化された `Shell` オブジェクト上で設定できますが、通常は、ナビゲーション バーを非表示にしたい任意のページ上で設定します。 たとえば、次の XAML では、[`ContentPage`](xref:Xamarin.Forms.ContentPage) のナビゲーション バーの無効化を示しています。

```xaml
<ContentPage ...
             Shell.NavBarIsVisible="false">
    ...
</ContentPage>
```

これで、ページが表示されたときにナビゲーション バーが非表示になります。

![iOS と Android での、ナビゲーション バーを非表示にしたシェル ページのスクリーンショット](configuration-images/navigationbar-invisible.png "ナビゲーション バーを非表示にしたシェル ページ")

## <a name="disable-the-tab-bar"></a>タブ バーを無効にする

`Shell` クラスでは、ページが表示されたときにタブ バーを表示するかどうかを制御する、`bool` 型の添付プロパティ `TabBarIsVisible` を定義できます。 既定では、このプロパティの値は `true` です。

このプロパティはサブクラス化された `Shell` オブジェクト上で設定できますが、通常は、タブ バーを非表示にしたい任意のページ上で設定します。 たとえば、次の XAML では、[`ContentPage`](xref:Xamarin.Forms.ContentPage) のタブ バーの無効化を示しています。

```xaml
<ContentPage ...
             Shell.TabBarIsVisible="false">
    ...
</ContentPage>
```

これで、ページが表示されたときにタブ バーが非表示になります。

![iOS と Android での、タブ バーを非表示にしたシェル ページのスクリーンショット](configuration-images/tabbar-invisible.png "タブ バーを非表示にしたシェル ページ")

## <a name="display-views-in-the-navigation-bar"></a>ナビゲーション バーにビューを表示する

`Shell` クラスでは、`View` 型の添付プロパティ `TitleView` を定義できます。これにより、ナビゲーション バーに任意の Xamarin.Forms [`View`](xref:Xamarin.Forms.View) を表示させることができます。

このプロパティはサブクラス化された `Shell` オブジェクト上で設定できますが、ナビゲーション バーにビューを表示させたい任意のページ上でも設定できます。 たとえば、次の XAML では、[`ContentPage`](xref:Xamarin.Forms.ContentPage) のナビゲーション バーで [`Image`](xref:Xamarin.Forms.Image) を表示させる方法を示しています。

```xaml
<ContentPage ...>
    <Shell.TitleView>
        <Image Source="xamarin_logo.png"
               HorizontalOptions="Center"
               VerticalOptions="Center" />
    </Shell.TitleView>
    ...
</ContentPage>
```

これにより、ページ上のナビゲーション バーに画像が表示されます。

![iOS と Android での、タイトル ビューを備えたシェル ページのスクリーンショット](configuration-images/titleview.png "タイトル ビューを備えたシェル ページ")

> [!IMPORTANT]
> `NavBarIsVisible` 添付プロパティを使ってナビゲーション バーを非表示にした場合、タイトル ビューは表示されません。

ビューのサイズを [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) および [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) プロパティで指定するか、ビューの位置を [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) および [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) プロパティで指定しない限り、多くのビューはナビゲーション バーに表示されません。

[`Layout`](xref:Xamarin.Forms.Layout) クラスは [`View`](xref:Xamarin.Forms.View) クラスから派生しているため、複数のビューを含むレイアウト クラスを表示するように `TitleView` 添付プロパティを設定することができます。 同様に、[`ContentView`](xref:Xamarin.Forms.ContentView) クラスは最終的に [`View`](xref:Xamarin.Forms.View) クラスから派生しているため、単一のビューを含む `ContentView` を表示するように `TitleView` 添付プロパティを設定することができます。

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Xamarin.Forms CSS シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)
