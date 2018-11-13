---
title: WPF プラットフォームのセットアップ
description: Xamarin.Forms が WPF プラットフォームのプレビューをサポート
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2018
ms.openlocfilehash: cdf115c4ea6d6613a1da2d0d2cfa14ed500086f8
ms.sourcegitcommit: 03dfb4a2c20ad68515875b415e7d84ee9b0a8cb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2018
ms.locfileid: "51563135"
---
# <a name="wpf-platform-setup"></a>WPF プラットフォームのセットアップ

![[プレビュー]](~/media/shared/preview.png)

Xamarin.Forms では、Windows Presentation Foundation (WPF) のプレビューをサポートできるようになりました。 この記事では、WPF プロジェクトを Xamarin.Forms ソリューションに追加する方法を示します。

開始、Visual Studio 2017 で新しい Xamarin.Forms ソリューションを作成または既存の Xamarin.Forms ソリューションを使用して、たとえば、前に[ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/)します。 WPF アプリは、Windows での Xamarin.Forms ソリューションにのみ追加できます。

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>WPF プロジェクト Xamarin.University で Xamarin.Forms アプリを追加します。

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF をサポートして[Xamarin University](https://university.xamarin.com/)**

## <a name="adding-a-wpf-app"></a>WPF アプリの追加

Windows 7、8、および 10 のデスクトップ上で実行される WPF アプリを追加するこれらの手順に従います。

1. Visual Studio 2017 でソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加 > 新しいプロジェクト.**.

2. **新しいプロジェクト**ウィンドウで、左側の選択で**Visual c#** と**Windows クラシック デスクトップ**します。 プロジェクトの種類の一覧で選択**WPF アプリ (.NET Framework)** します。 

3. 使用してプロジェクトの名前を入力、 **WPF**拡張子、たとえば、 **BoxViewClock.WPF**します。 をクリックして、**参照**ボタンを選択、 **BoxViewClock**フォルダー、およびキーを押して**フォルダーの選択**します。 これにより、WPF プロジェクト、ソリューション内の他のプロジェクトと同じディレクトリに入ります。

    ![新しい WPF プロジェクトの追加](wpf-images/add-new-project.png "新しい WPF プロジェクトの追加")

    プロジェクトを作成するには、[ok] を押します。

4. **ソリューション エクスプ ローラー**、新しいを右クリックして**BoxViewClock.WPF**順に選択して**NuGet パッケージの管理**します。 選択、**参照** タブで、をクリックして、**プレリリースを含める**チェック ボックスをオンの検索と**Xamarin.Forms**します。

    ![NuGet パッケージを選択して](wpf-images/select-nuget-package.png "NuGet パッケージを選択します。")

    パッケージ化し、をクリックするものを選択、**インストール**ボタンをクリックします。

5. 今すぐ検索**Xamarin.Forms.Platform.WPF**パッケージ化し、その 1 つもインストールします。 パッケージは、Microsoft から確認するには!

6. ソリューション名を右クリックして、**ソリューション エクスプ ローラー**選択**ソリューションの NuGet パッケージの管理**します。 選択、 **Update**タブおよび**Xamarin.Forms**パッケージ。 すべてのプロジェクトを選択し、同じ Xamarin.Forms バージョンに更新します。

    ![NuGet パッケージの更新](wpf-images/update-nuget-package.png "NuGet パッケージの更新") 

7. 右クリックし、WPF プロジェクトで**参照**します。 **参照マネージャー**ダイアログ ボックスで、**プロジェクト**チェックの横にあるチェック ボックス、左側にある、 **BoxViewClock**プロジェクト。

    ![共有プロジェクト参照](wpf-images/reference-shared-project.png "共有プロジェクトの参照")

8. 編集、 **MainWindow.xaml** WPF プロジェクトのファイル。 `Window`タグでの XML 名前空間宣言を追加、 **Xamarin.Forms.Platform.WPF**アセンブリと名前空間。

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    ここで変更、`Window`タグを`wpf:FormsApplicationPage`します。 変更、`Title`をアプリケーションの名前に設定**BoxViewClock**します。 完成した XAML ファイルは、次のようになります。

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>
        
        </Grid>
    </wpf:FormsApplicationPage>
    ```

9. 編集、 **MainWindow.xaml.cs** WPF プロジェクトのファイル。 2 つの新しい追加`using`ディレクティブ。

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    基本クラスを変更`MainWindow`から`Window`に`FormsApplicationPage`します。 次の`InitializeComponent`呼び出しは、次の 2 つのステートメントを追加します。

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    コメントを除くと、未使用`using`ディレクティブ、完全な**MainWindows.xaml.cs**ファイルは、次のようになります。

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;

    namespace BoxViewClock.WPF
    {
        public partial class MainWindow : FormsApplicationPage
        {
            public MainWindow()
            {
                InitializeComponent();

                Forms.Init();
                LoadApplication(new BoxViewClock.App());
            }
        }
    }
    ```

10. WPF プロジェクトを右クリックし、**ソリューション エクスプ ローラー**選択と**スタートアップ プロジェクトとして設定**。 F5 キーを押して、Windows デスクトップで、Visual Studio デバッガーでプログラムを実行します。

    ![BoxView の WPF クロック](wpf-images/wpf-boxviewclock.png "BoxView の WPF クロック" )

## <a name="next-steps"></a>次の手順

### <a name="platform-specifics"></a>プラットフォーム固有設定

コードまたは XAML のいずれかから、Xamarin.Forms アプリケーションが実行されているプラットフォームを判断できます。 これにより、WPF で実行されているときに、プログラムの特性を変更することができます。 コードでは、値を比較`Device.RuntimePlatform`で、`Device.WPF`定数 (これは、文字列"WPF"と等しくなります)。 一致がある場合は、WPF で、アプリケーションが実行されています。

XAML で使用できます、`OnPlatform`タグをプラットフォームに固有のプロパティ値を選択します。

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="WPF" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="window-size"></a>ウィンドウ サイズ

WPF のウィンドウの初期サイズを調整する**MainWindow.xaml**ファイル。

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>Issues

これは、その運用環境の準備がすべてではないことを想定する必要がありますのプレビューです。 Xamarin.Forms 用のすべての NuGet パッケージは、WPF の準備が整ったし、一部の機能が完全に機能していない可能性があります。

