---
title: WPF プラットフォームのセットアップ
description: Xamarin.Forms では、WPF プラットフォームのプレビューがサポートされています。
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 05/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 17db86eb6e6c767498f1d8b550b923377b905364
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91557440"
---
# <a name="wpf-platform-setup"></a>WPF プラットフォームのセットアップ

![[プレビュー]](~/media/shared/preview.png)

Xamarin.Forms では、Windows Presentation Foundation (WPF) のプレビューがサポートされており、.NET Core 3 で .NET Framework しています。 この記事では、.NET Framework を対象とする WPF プロジェクトをソリューションに追加する方法について説明し Xamarin.Forms ます。

> [!IMPORTANT]
> Xamarin.Forms WPF のサポートは、コミュニティによって提供されます。 詳細については、「 [ Xamarin.Forms プラットフォームのサポート](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)」を参照してください。

開始する前に、 Xamarin.Forms Visual Studio 2019 で新しいソリューションを作成するか、既存 Xamarin.Forms のソリューション ( [**boxviewclock**](/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)など) を使用します。 WPF アプリは、Windows のソリューションにのみ追加でき Xamarin.Forms ます。

## <a name="add-a-wpf-application"></a>WPF アプリケーションの追加

Windows 7、8、および10のデスクトップで実行される WPF アプリケーションを追加するには、次の手順に従います。

1. Visual Studio 2019 で、 **ソリューションエクスプローラー** でソリューション名を右クリックし、[ **> 新しいプロジェクトの追加**] を選択します。

2. [**新しいプロジェクトの追加**] ウィンドウで、[**言語**] ボックスの一覧の [ **C#** ] を選択し、[**プラットフォーム**] ドロップダウンで [ **Windows** ] を選択し、[**プロジェクトの種類**] ドロップダウンで [**デスクトップ**] を選択します。 プロジェクトの種類の一覧で、[ **WPF アプリ (.NET Framework)**] を選択します。

    ![新しい WPF プロジェクトを追加する](wpf-images/add-project.png "新しい WPF プロジェクトを追加する")

    [ **次へ** ] ボタンをクリックします。

    > [!NOTE]
    > Xamarin.Forms 4.7 には、.NET Core 3 で実行される WPF アプリのサポートが含まれています。

3. [ **新しいプロジェクトの構成** ] ウィンドウで、 **wpf** 拡張機能を含むプロジェクトの名前を入力します。たとえば、「 **boxviewclock. WPF**」と入力します。 [ **参照** ] ボタンをクリックし、[ **boxviewclock** ] フォルダーを選択し、 **[フォルダーの選択]** をクリックして、ソリューション内の他のプロジェクトと同じディレクトリに WPF プロジェクトを配置します。

    ![新しい WPF プロジェクトを追加する](wpf-images/configure-project.png "新しい WPF プロジェクトを追加する")

    [ **作成** ] ボタンを押して、プロジェクトを作成します。

4. **ソリューションエクスプローラー**で、新しい**BOXVIEWCLOCK. WPF**プロジェクトを右クリックし、[ **NuGet パッケージの管理...**] を選択します。[**参照**] タブを選択し、を検索し** Xamarin.Forms ます。Platform. WPF**:

    ![NuGet パッケージを選択します](wpf-images/select-nuget-package.png "NuGet パッケージを選択します")

    パッケージを選択し、[ **インストール** ] ボタンをクリックします。

5. **ソリューションエクスプローラー**でソリューション名を右クリックし、[**ソリューションの NuGet パッケージの管理...**] を選択します。[**更新**] タブを選択し、パッケージを選択し **Xamarin.Forms** ます。 すべてのプロジェクトを選択し、同じバージョンに更新し Xamarin.Forms ます。

    ![NuGet パッケージを更新する](wpf-images/update-nuget-package.png "NuGet パッケージを更新する")

6. WPF プロジェクトで、[ **参照** ] を右クリックし、[ **参照の追加**] を選択します。[ **参照マネージャー** ] ダイアログで、左側の [ **プロジェクト** ] を選択し、 **boxviewclock** プロジェクトの横にあるチェックボックスをオンにします。

    ![共有プロジェクトの参照](wpf-images/reference-shared-project.png "共有プロジェクトの参照")

    **[OK** ] をクリックします。

7. WPF プロジェクトの **mainwindow.xaml** ファイルを編集します。 タグに、の `Window` XML 名前空間宣言を追加し** Xamarin.Forms ます。Platform. WPF**アセンブリと名前空間:

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    次に、 `Window` タグをに変更 `wpf:FormsApplicationPage` します。 設定を `Title` アプリケーションの名前 ( **Boxviewclock**など) に変更します。 完成した XAML ファイルは次のようになります。

    ```xaml
    <wpf:FormsApplicationPage x:Class="BoxViewClock.WPF.MainWindow"
            xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
            xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
            xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
            xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
            xmlns:local="clr-namespace:BoxViewClock.WPF"
            xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"            
            mc:Ignorable="d"
            Title="BoxViewClock" Height="450" Width="800">
        <Grid>

        </Grid>
    </wpf:FormsApplicationPage>
    ```

8. WPF プロジェクトの **MainWindow.xaml.cs** ファイルを編集します。 2つの新しいディレクティブを追加し `using` ます。

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    の基本クラスを `MainWindow` から `Window` に変更 `FormsApplicationPage` します。 呼び出しの後 `InitializeComponent` に、次の2つのステートメントを追加します。

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    コメントや未使用のディレクティブを除き `using` 、完全な **MainWindows.xaml.cs** ファイルは次のようになります。

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

9. **ソリューションエクスプローラー**で WPF プロジェクトを右クリックし、[**スタートアッププロジェクトに設定**] を選択します。 F5 キーを押して、Windows デスクトップで Visual Studio デバッガーを使用してプログラムを実行します。

    ![WPF BoxView Clock](wpf-images/wpf-boxviewclock.png "WPF BoxView Clock" )

## <a name="platform-specifics"></a>プラットフォームの詳細

Xamarin.Formsアプリケーションが実行されているプラットフォームをコードまたは XAML から調べることができます。 これにより、WPF 上で実行されているときに、プログラムの特性を変更することができます。 コードで、の値を `Device.RuntimePlatform` `Device.WPF` 定数 (文字列 "WPF" に等しい) と比較します。 一致するものがある場合、アプリケーションは WPF で実行されています。

XAML では、タグを使用し `OnPlatform` て、プラットフォームに固有のプロパティ値を選択できます。

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

## <a name="window-size"></a>ウィンドウ サイズ

WPF の **mainwindow.xaml** ファイルで、ウィンドウの初期サイズを調整できます。

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>発行

これはプレビューなので、運用環境の準備ができているわけではありません。 のすべての NuGet パッケージ Xamarin.Forms が WPF 用に準備されているわけではありません。一部の機能が完全に動作していない可能性があります。

## <a name="related-video"></a>関連ビデオ

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF サポートビデオ**