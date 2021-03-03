---
title: Xamarin.Forms シェルのページ構成
description: Shell クラスでは、Xamarin.Forms シェル アプリケーションのページの外観の構成に使用できる添付プロパティを定義します。 これには、ページの色の設定、ナビゲーション バーの無効化、タブ バーの無効化、およびナビゲーション バーでのビューの表示が含まれます。
ms.prod: xamarin
ms.assetid: 3FC2FBD1-C30B-4408-97B2-B04E3A2E4F03
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c5be4e600b370c39387302aea42645c8684501ea
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93374384"
---
# <a name="xamarinforms-shell-page-configuration"></a>Xamarin.Forms シェルのページ構成

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

`Shell` クラスでは、Xamarin.Forms シェル アプリケーションのページの外観の構成に使用できる添付プロパティを定義します。 これには、ページの色の設定、ページのプレゼンテーション モードの設定、ナビゲーション バーの無効化、タブ バーの無効化、およびナビゲーション バーでのビューの表示が含まれます。

## <a name="set-page-colors"></a>ページの色を設定する

`Shell` クラスでは、シェル アプリケーションのページの色の設定に使える次の添付プロパティを定義できます。

- `BackgroundColor`: `Color` 型、シェル クロームの背景色を定義します。 シェル コンテンツの背後は、色で塗りつぶされません。
- `DisabledColor`: `Color` 型、無効化されているテキストとアイコンを網掛けで表示するための色を定義します。
- `ForegroundColor`: `Color` 型、テキストとアイコンを網掛けで表示するための色を定義します。
- `TitleColor`:、`Color` 型、現在のページのタイトルに使用される色を定義します。
- `UnselectedColor`: `Color` 型、シェル クロームにおいて、選択されていないテキストとアイコンに使用する色を定義します。

これらのプロパティはすべて、[`BindableProperty`](xref:Xamarin.Forms.BindableProperty) オブジェクトを基盤としています。つまり、プロパティをデータ バインディングの対象にして、XAML スタイルを使ってスタイル設定することができます。 さらに、このプロパティはカスケード スタイル シート (CSS) を使用して設定できます。 詳細については、「[Xamarin.Forms シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)」を参照してください。

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

XAML スタイルの詳細については、「[XAML スタイルを使用した Xamarin.Forms アプリのスタイル設定](~/xamarin-forms/user-interface/styles/xaml/index.md)」を参照してください。

## <a name="set-page-presentation-mode"></a>ページのプレゼンテーション モードを設定する

既定では、`GoToAsync` メソッドを使用してページに移動したときに、小さなナビゲーションのアニメーションが発生します。 ただし、[`ContentPage`](xref:Xamarin.Forms.ContentPage) の `Shell.PresentationMode` 添付プロパティを次の `PresentationMode` 列挙メンバーのいずれかに設定することで、この動作を変更することができます。

- `NotAnimated`: ナビゲーションのアニメーションなしでページが表示されることを示します。
- `Animated`: ナビゲーションのアニメーションを使用してページが表示されることを示します。 これは `Shell.PresentationMode` 添付プロパティの既定値です。
- `Modal`: ページがモーダル ページとして表示されることを示します。
- `ModalAnimated`: ページがモーダル ページとして、ナビゲーションのアニメーションを使用して表示されることを示します。
- `ModalNotAnimated`: ページがモーダル ページとして、ナビゲーションのアニメーションなしで表示されることを示します。

> [!IMPORTANT]
> `PresentationMode` 型はフラグの列挙型です。 つまり、コード内で列挙メンバーを組み合わせて適用することができます。 ただし、XAML での使用を容易にするために、`ModalAnimated` メンバーは `Animated` および `Modal` メンバーの組み合わせ、`ModalNotAnimated` メンバーは `NotAnimated` および `Modal` メンバーの組み合わせになっています。 フラグの列挙型の詳細については、「[ビット フラグとしての列挙型](/dotnet/csharp/language-reference/builtin-types/enum#enumeration-types-as-bit-flags)」をご覧ください。

次の XAML の例では、[`ContentPage`](xref:Xamarin.Forms.ContentPage) の `Shell.PresentationMode` 添付プロパティを設定しています。

```xaml
<ContentPage ...
             Shell.PresentationMode="Modal">
    ...             
</ContentPage>
```

この例では、`GoToAsync` メソッドを使用してページに移動したときに、[`ContentPage`](xref:Xamarin.Forms.ContentPage) がモーダル ページとして表示されるように設定しています。

## <a name="enable-navigation-bar-shadow"></a>ナビゲーション バーの影を有効にする

`Shell` クラスでは、ナビゲーション バーを影付きにするかどうかを制御する、`bool` 型の添付プロパティ `NavBarHasShadow` を定義できます。 既定では、このプロパティの値は、iOS では `false`、Android では `true` です。

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

`Shell` クラスでは、`View` 型の添付プロパティ `TitleView` を定義します。これにより、ナビゲーション バーに任意の Xamarin.Forms [`View`](xref:Xamarin.Forms.View) を表示させることができます。

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

## <a name="page-visibility"></a>ページの可視性

シェルは、[`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible) プロパティを使用して設定される、ページの可視性に従います。 したがって、ページの `IsVisible` プロパティが `false` に設定されている場合、それはシェル アプリケーションには表示されず、移動することもできません。

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [XAML スタイルを使用して Xamarin.Forms アプリのスタイルを設定する](~/xamarin-forms/user-interface/styles/xaml/index.md)
- [Xamarin.Forms CSS シェル固有のプロパティ](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties)