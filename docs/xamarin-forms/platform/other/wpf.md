---
title: WPF プラットフォームのセットアップ
description: Xamarin. Forms で WPF プラットフォームのプレビューがサポートされるようになりました
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/05/2018
ms.openlocfilehash: 38d9b42b3a29ea46d05a1d1cc4e38641d2445786
ms.sourcegitcommit: 4cf434b126eb7df6b2fd9bb1d71613bf2b6aac0e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/07/2019
ms.locfileid: "71997171"
---
# <a name="wpf-platform-setup"></a>WPF プラットフォームのセットアップ

![[プレビュー]](~/media/shared/preview.png)

Xamarin. Forms では、Windows Presentation Foundation (WPF) のプレビューがサポートされるようになりました。 この記事では、WPF プロジェクトを Xamarin. Forms ソリューションに追加する方法について説明します。

> [!IMPORTANT]
> WPF の Xamarin. Forms サポートは、コミュニティによって提供されます。 詳細については、「 [Xamarin. Forms Platform Support](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)」を参照してください。

開始する前に、Visual Studio 2019 で新しい Xamarin. Forms ソリューションを作成するか、既存の Xamarin. Forms ソリューション ( [**Boxviewclock**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)など) を使用します。 WPF アプリは、Windows の Xamarin. Forms ソリューションにのみ追加できます。

## <a name="add-a-wpf-project-to-a-xamarinforms-app-with-xamarinuniversity"></a>Xamarin. 大学で Xamarin. Forms アプリに WPF プロジェクトを追加する

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin. Forms 3.0 WPF サポートビデオ**

## <a name="adding-a-wpf-app"></a>WPF アプリの追加

Windows 7、8、および10のデスクトップで実行される WPF アプリを追加するには、次の手順に従います。

1. Visual Studio 2019 で、**ソリューションエクスプローラー**でソリューション名を右クリックし、 **[> 新しいプロジェクトの追加]** を選択します。

2. **[新しいプロジェクト]** ウィンドウで、左側の [**ビジュアルC#** と**Windows クラシックデスクトップ**] を選択します。 プロジェクトの種類の一覧で、 **[WPF アプリ (.NET Framework)]** を選択します。

3. **Wpf**拡張機能を使用してプロジェクトの名前を入力します。たとえば、「 **BOXVIEWCLOCK. WPF**」と入力します。 **参照**ボタンをクリックし、 **boxviewclock**フォルダーを選択して、 **[フォルダーの選択]** をクリックします。 これにより、ソリューション内の他のプロジェクトと同じディレクトリに WPF プロジェクトが配置されます。

    ![新しい wpf プロジェクトを追加]する(wpf-images/add-new-project.png "新しい wpf プロジェクトを追加する")

    [OK] を押して、プロジェクトを作成します。

4. **ソリューションエクスプローラー**で、新しい**BOXVIEWCLOCK. WPF**プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 **[参照]** タブを選択し、 **[プレリリースを含める]** チェックボックスをオンにして、「 **Xamarin. フォーム**」を検索します。

    ![Nuget パッケージを選択]する(wpf-images/select-nuget-package.png "nuget パッケージを選択")する

    そのパッケージを選択し、 **[インストール]** ボタンをクリックします。

5. 次に、 **Xamarin. platform.string**パッケージを検索し、それをインストールします。 パッケージが Microsoft からのものであることを確認してください。

6. **ソリューションエクスプローラー**でソリューション名を右クリックし、 **[ソリューションの NuGet パッケージの管理]** を選択します。 **[更新]** タブと **[Xamarin]** パッケージを選択します。 すべてのプロジェクトを選択し、同じ Xamarin. Forms バージョンに更新します。

    ![Nuget パッケージを更新]する(wpf-images/update-nuget-package.png "nuget パッケージを更新")する

7. WPF プロジェクトで、 **[参照]** を右クリックします。 **[参照マネージャー]** ダイアログで、左側の **[プロジェクト]** を選択し、 **boxviewclock**プロジェクトの横にあるチェックボックスをオンにします。

    共有プロジェクト参照![の参照](wpf-images/reference-shared-project.png "共有プロジェクト")

8. WPF プロジェクトの**mainwindow.xaml**ファイルを編集します。 @No__t-0 タグに、 **Xamarin** . .xml アセンブリと名前空間の XML 名前空間宣言を追加します。

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    ここで、`Window` タグを `wpf:FormsApplicationPage` に変更します。 @No__t-0 設定をアプリケーションの名前 ( **Boxviewclock**など) に変更します。 完成した XAML ファイルは次のようになります。

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

9. WPF プロジェクトの**MainWindow.xaml.cs**ファイルを編集します。 2つの新しい `using` ディレクティブを追加します。

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    @No__t-0 の基本クラスを `Window` から `FormsApplicationPage` に変更します。 @No__t-0 呼び出しの後に、次の2つのステートメントを追加します。

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    コメントや未使用の `using` ディレクティブを除き、完全な**MainWindows.xaml.cs**ファイルは次のようになります。

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

10. **ソリューションエクスプローラー**で WPF プロジェクトを右クリックし、 **[スタートアッププロジェクトに設定]** を選択します。 F5 キーを押して、Windows デスクトップで Visual Studio デバッガーを使用してプログラムを実行します。

    ![Wpf BoxView clock](wpf-images/wpf-boxviewclock.png "Wpf boxview clock" )

## <a name="next-steps"></a>次の手順

### <a name="platform-specifics"></a>プラットフォーム固有設定

コードまたは XAML から、Xamarin アプリケーションが実行されているプラットフォームを特定できます。 これにより、WPF 上で実行されているときに、プログラムの特性を変更することができます。 コードで `Device.RuntimePlatform` の値を `Device.WPF` の定数 (文字列 "WPF" に相当) と比較します。 一致するものがある場合、アプリケーションは WPF で実行されています。

XAML では、`OnPlatform` タグを使用して、プラットフォームに固有のプロパティ値を選択できます。

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

WPF の**mainwindow.xaml**ファイルで、ウィンドウの初期サイズを調整できます。

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>懸案事項

これはプレビューなので、運用環境の準備ができているわけではありません。 Xamarin のすべての NuGet パッケージが WPF 用に準備されているわけではなく、一部の機能が完全に動作していない可能性があります。
