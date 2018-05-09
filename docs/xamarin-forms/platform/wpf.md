---
title: WPF のプラットフォームのセットアップ
description: Xamarin.Forms ようになりました、WPF プラットフォーム用のプレビューがサポート
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: charlespetzold
ms.author: chape
ms.date: 04/05/2018
ms.openlocfilehash: 416e33f131c6e1ef144608f98964fd8372f454f8
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="wpf-platform-setup"></a>WPF のプラットフォームのセットアップ

![[プレビュー]](~/media/shared/preview.png)

Xamarin.Forms では、Windows Presentation Foundation (WPF) のプレビューがサポートできるようになりました。 この記事では、まず Xamarin.Forms ソリューションを WPF プロジェクトを追加する方法を示します。

開始、Visual Studio 2017 で新しい Xamarin.Forms ソリューションを作成または既存の Xamarin.Forms ソリューションをたとえば、使用前に[ **BoxViewClock**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/BoxViewClock/)です。 WPF アプリは Xamarin.Forms ソリューションを Windows にのみ追加できます。

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>WPF プロジェクト Xamarin.University と Xamarin.Forms のアプリを追加します。

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF をサポートして[Xamarin 大学](https://university.xamarin.com/)**

## <a name="adding-a-wpf-app"></a>WPF アプリを追加します。

Windows 7、8、および 10 のデスクトップで実行される WPF アプリを追加する手順に従います。

1. Visual Studio 2017 でソリューション名を右クリックし、**ソリューション エクスプ ローラー**選択**追加 > 新しいプロジェクト.**.

2. **新しいプロジェクト**ウィンドウで、左クリックで**Visual c#** と**Windows クラシック デスクトップ**です。 プロジェクトの種類の一覧で選択**WPF アプリケーション (.NET Framework)** です。 

3. 使用してプロジェクトの名前を入力、 **WPF**拡張子、たとえば、 **BoxViewClock.WPF**です。 をクリックして、**参照**ボタン、、 **BoxViewClock**フォルダー、およびキーを押して**フォルダーの選択**です。 これにより、WPF プロジェクト、ソリューション内の他のプロジェクトと同じディレクトリに入ります。

    ![新しい WPF プロジェクトの追加](wpf-images/add-new-project.png "新しい WPF プロジェクトの追加")

    プロジェクトを作成するには、[ok] を押します。

4. **ソリューション エクスプ ローラー**、 を右クリックして、新しい**BoxViewClock.WPF**プロジェクトし、選択**NuGet パッケージの管理**です。 選択、**参照**タブをクリックし、**プレリリースを含める** チェック ボックスを検索し、 **Xamarin.Forms**です。

    ![NuGet パッケージを選択して](wpf-images/select-nuget-package.png "NuGet パッケージの選択")

    パッケージ化しをクリックしてあることを選択、**インストール**ボタンをクリックします。

5. 今すぐ検索**Xamarin.Forms.Platform.WPF**パッケージ化し、その 1 つもインストールします。 パッケージが Microsoft から確認してください。

6. ソリューション名を右クリックして、**ソリューション エクスプ ローラー**選択**Manage NuGet Packages for Solution**です。 選択、**更新** タブおよび**Xamarin.Forms**パッケージです。 すべてのプロジェクトを選択し、同じ Xamarin.Forms バージョンに更新します。

    ![NuGet パッケージを更新](wpf-images/update-nuget-package.png "NuGet パッケージの更新") 

7. 右クリックし、WPF プロジェクトで**参照**です。 **参照マネージャー**ダイアログで、**プロジェクト**、左右のチェックの横にあるチェック ボックスで、 **BoxViewClock**プロジェクト。

    ![共有プロジェクト参照](wpf-images/reference-shared-project.png "共有プロジェクト参照")

8. 編集、 **MainWindow.xaml** WPF プロジェクトのファイルです。 `Window`タグの XML 名前空間宣言を追加、 **Xamarin.Forms.Platform.WPF**アセンブリと名前空間。

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    ここで変更、`Window`タグを`wpf:FormsApplicationPage`です。 変更、`Title`設定、アプリケーションの名前を**BoxViewClock**です。 完成した XAML ファイルは、次のようになります。

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

9. 編集、 **MainWindow.xaml.cs** WPF プロジェクトのファイルです。 新しい 2 つ追加`using`ディレクティブ。

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    基本クラスを変更する`MainWindow`から`Window`に`FormsApplicationPage`です。 次の`InitializeComponent`呼び出し、次の 2 つのステートメントを追加します。

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```
    
    コメントを除くと未使用`using`ディレクティブ、完全な**MainWindows.xaml.cs**ファイルは次のようになります。

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

10. WPF プロジェクトを右クリックし、**ソリューション エクスプ ローラー**選択**スタートアップ プロジェクトとして設定**です。 F5 キーを押して、Windows デスクトップで、Visual Studio デバッガーでプログラムを実行します。

    ![WPF BoxView クロック](wpf-images/wpf-boxviewclock.png "WPF BoxView クロック" )

## <a name="next-steps"></a>次の手順

### <a name="platform-specifics"></a>プラットフォームの詳細

コードか XAML のいずれかから Xamarin.Forms アプリケーションが実行されているプラットフォームを指定できます。 これにより、WPF で実行されているときに、プログラムの特性を変更することができます。 コードでは、値を比較`Device.RuntimePlatform`で、`Device.WPF`定数 (これは、文字列"WPF"に相当します)。 一致がある場合は、WPF で、アプリケーションが実行されています。

XAML では、使用することができます、`OnPlatform`タグをプラットフォームに固有のプロパティ値を選択します。

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

これは、プレビューわけではないが実稼働の準備完了であることが予想されます。 Xamarin.Forms のすべての NuGet パッケージの準備ができて、WPF と一部の機能が完全に機能していない可能性があります。

