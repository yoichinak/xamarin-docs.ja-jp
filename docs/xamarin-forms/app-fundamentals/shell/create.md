---
title: Xamarin.Forms シェル アプリケーションを作成する
description: Xamarin.Forms シェル アプリケーションを作成するプロセスは、Shell クラスがサブクラス化された XAML ファイルを作成し、アプリケーションの App クラスの MainPage プロパティをサブクラス化した Shell オブジェクトに設定した後、サブクラス化した Shell クラス内にアプリケーションのビジュアル階層を記述することです。
ms.prod: xamarin
ms.assetid: 2A51D78F-6CD5-4BC4-A62E-11CEFA799987
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f4b15911f48b4260da8839f376800e95c63bf0e4
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93373628"
---
# <a name="create-a-no-locxamarinforms-shell-application"></a>Xamarin.Forms シェル アプリケーションを作成する

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

Xamarin.Forms シェル アプリケーションを作成するプロセスは次のとおりです。

1. 新しい Xamarin.Forms アプリケーションを作成するか、シェル アプリケーションに変換したい既存のアプリケーションを読み込む。
1. `Shell` クラスがサブクラス化された XAML ファイルを共有コード プロジェクトに追加する。 詳細については、「[Shell クラスをサブクラス化する](#subclass-the-shell-class)」をご覧ください。
1. アプリケーションの `App` クラスの [`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティをサブクラス化された `Shell` オブジェクトに設定する。 詳細については、「[シェル アプリケーションをブートストラップする](#bootstrap-the-shell-application)」をご覧ください。
1. サブクラス化された `Shell` クラスにアプリケーションのビジュアル階層を記述する。 詳細については、「[アプリケーションのビジュアル階層を記述する](#describe-the-visual-hierarchy-of-the-application)」をご覧ください。

## <a name="subclass-the-shell-class"></a>Shell クラスをサブクラス化する

Xamarin.Forms シェル アプリケーションを作成する最初の手順は、`Shell` クラスをサブクラス化する XAML ファイルを共有コード プロジェクトに追加することです。 このファイルには任意の名前を付けることができますが、 **AppShell** がお勧めです。 次のコード例は、新しく作成した **AppShell.xaml** ファイルを示しています。

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       x:Class="Xaminals.AppShell">

</Shell>
```

次の例は、分離コード ファイル **AppShell.xaml.cs** を示しています。

```csharp
using Xamarin.Forms;

namespace Xaminals
{
    public partial class AppShell : Shell
    {
        public AppShell()
        {
            InitializeComponent();
        }
    }
}
```

## <a name="bootstrap-the-shell-application"></a>シェル アプリケーションをブートストラップする

`Shell` オブジェクトがサブクラス化された XAML ファイルを作成した後は、`App` クラスの [`MainPage`](xref:Xamarin.Forms.Application.MainPage) プロパティをサブクラス化された `Shell` オブジェクトに設定する必要があります。

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

この例では、`AppShell` クラスは `Shell` クラスから派生した XAML ファイルです。

> [!NOTE]
> 空のシェル アプリケーションはビルドできますが、それを実行すると `InvalidOperationException` がスローされます。

## <a name="describe-the-visual-hierarchy-of-the-application"></a>アプリケーションのビジュアル階層を記述する

Xamarin.Forms シェル アプリケーション作成の最後の手順は、サブクラス化された `Shell` クラス内に、アプリケーションのビジュアル階層を記述することです。 サブクラス化された `Shell` クラスは、次の 3 つの主な階層オブジェクトで構成されています。

- `FlyoutItem` または `TabBar`。 `FlyoutItem` ではポップアップ内の 1 つまたは複数の項目を表します。これは、アプリケーションのナビゲーション パターンにポップアップが含まれている場合に使う必要があります。 `TabBar` では下部にあるタブ バーを表します。これは、アプリケーションのナビゲーション パターンが下部のタブから開始される場合に使う必要があります。 `FlyoutItem` オブジェクトまたは `TabBar` オブジェクトは、すべて `Shell` オブジェクトの子です。
- `Tab`。これは、下部のタブによって移動できるグループ化されたコンテンツを表します。 `Tab` オブジェクトはすべて、`FlyoutItem` オブジェクトか `TabBar` オブジェクトの子です。
- `ShellContent`。これは、アプリケーションの `ContentPage` オブジェクトを表します。 `ShellContent` オブジェクトはすべて、`Tab` オブジェクトの子です。 複数の `ShellContent` オブジェクトが `Tab` にある場合は、上部のタブによってそのオブジェクトをナビゲートできます。

これらのオブジェクトはどれも、ユーザー インターフェイスではなく、アプリケーションのビジュアル階層の編成を表しています。 シェルでは、これらのオブジェクトを取得して、コンテンツのナビゲーション ユーザー インターフェイスを生成します。

次の XAML では、サブクラス化された `Shell` クラスの例を示します。

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

実行すると、この XAML により `CatsPage`  が表示されます。サブクラス化された `Shell` クラスで宣言されたコンテンツの最初の項目であるためです。

[![iOS および Android 上のシェル アプリのスクリーンショット](create-images/cats.png "シェル アプリ")](create-images/cats-large.png#lightbox "シェル アプリ")

ハンバーガー アイコンを押すか、左からスワイプして、ポップアップを表示します。

[![iOS および Android 上のシェル フライアウトのスクリーンショット](create-images/flyout-reduced.png "シェル ポップアップ")](create-images/flyout-reduced-large.png#lightbox "シェル ポップアップ")

> [!IMPORTANT]
> シェル アプリケーションでは、`ShellContent` オブジェクトの子である各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) は、アプリケーションの起動中に作成されます。 この手法を利用してその他の `ShellContent` オブジェクトを追加すると、アプリケーションの起動時に追加のページが作成され、起動エクスペリエンスの低下を引き起こす場合があります。 ただし、シェルでは、ナビゲーションに応答して、必要に応じてページを作成することも可能です。 詳細については、「[Xamarin.Forms シェルのタブ](tabs.md)」ガイドにある「[効率的なページの読み込み](tabs.md#efficient-page-loading)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)