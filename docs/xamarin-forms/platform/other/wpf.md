---
title: WPF プラットフォームのセットアップ
description: Xamarin.Forms WPF プラットフォームのプレビューサポートを提供するようになりました
ms.prod: xamarin
ms.assetid: 650723F2-4279-4B7B-B0A1-D7F8FF26BF1E
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 04/09/2020
ms.openlocfilehash: c9ec9fec2391d7d7a24f97f2ec20208a7d69dbc1
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992370"
---
# <a name="wpf-platform-setup"></a>WPF プラットフォームのセットアップ

![プレビュー](~/media/shared/preview.png)

Xamarin.Forms は、Windows プレゼンテーション ファウンデーション (WPF) のプレビューサポートを提供するようになりました。 この記事では、Xamarin.Forms ソリューションに WPF プロジェクトを追加する方法を示します。

> [!IMPORTANT]
> Xamarin.Forms WPF のサポートは、コミュニティによって提供されます。 詳細については、「 [Xamarin.Forms プラットフォーム のサポート](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)」を参照してください。

開始する前に、Visual Studio 2019 で新しい Xamarin.Forms ソリューションを作成するか、既存の Xamarin.Forms ソリューションを使用[**します**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-boxviewclock)。 WPF アプリは、Windows の Xamarin.Forms ソリューションにのみ追加できます。

## <a name="add-a-wpf-application"></a>WPF アプリケーションを追加する

Windows 7、8、および 10 のデスクトップで実行される WPF アプリケーションを追加するには、次の手順に従います。

1. Visual Studio 2019 で、**ソリューション エクスプローラー**でソリューション名を右クリックし、[**新しいプロジェクトの追加>]** を選択します。

2. [**新しいプロジェクトの追加**] ウィンドウで、[**言語**] ドロップダウンで **[C#]** を選択し、[**プラットフォーム**] ドロップダウンで **[Windows]** を選択し、[**プロジェクトの種類**] ドロップダウンで [**デスクトップ**] を選択します。 プロジェクトの種類の一覧で **、[WPF アプリケーション (.NET Framework)]** を選択します。

    ![新しい WPF プロジェクトを追加する](wpf-images/add-project.png "新しい WPF プロジェクトを追加する")

    **[次へ**] ボタンを押します。

3. [**新しいプロジェクトの構成]** ウィンドウで **、WPF**拡張機能を持つプロジェクトの名前を入力**します。** [**参照**] ボタンをクリックし **、[BoxViewClock]** フォルダーを選択し、[**フォルダーの選択]** をクリックして、ソリューション内の他のプロジェクトと同じディレクトリに WPF プロジェクトを配置します。

    ![新しい WPF プロジェクトを追加する](wpf-images/configure-project.png "新しい WPF プロジェクトを追加する")

    [**作成**]ボタンを押してプロジェクトを作成します。

4. ソリューション**エクスプローラー**で、新しい**BoxViewClock.WPF**プロジェクトを右クリックし **、[NuGet パッケージの管理.**[**参照**] タブを選択し **、Xamarin.Forms.Platform.WPF**を検索します。

    ![NuGet パッケージを選択します。](wpf-images/select-nuget-package.png "NuGet パッケージを選択します。")

    パッケージを選択し、[**インストール**] ボタンをクリックします。

5. **ソリューション エクスプローラー**でソリューション名を右クリックし、[**ソリューションの NuGet パッケージの管理]** を選択します。[**更新]** タブを選択し **、Xamarin.Forms**パッケージを選択します。 すべてのプロジェクトを選択し、それらを同じ Xamarin.Forms バージョンに更新します。

    ![NuGet パッケージを更新します。](wpf-images/update-nuget-package.png "NuGet パッケージを更新します。")

6. WPF プロジェクトで、[**参照**設定] を右クリックし、[**参照の追加.**[**参照マネージャー** ]ダイアログボックスで、左側の **[プロジェクト**]を選択し **、BoxViewClock**プロジェクトの横にあるチェックボックスをオンにします。

    ![共有プロジェクトを参照する](wpf-images/reference-shared-project.png "共有プロジェクトを参照する")

    **[OK]ボタン**を押します。

7. WPF プロジェクトの**MainWindow.xaml**ファイルを編集します。 タグに`Window`**、Xamarin.Forms.Platform.WPF**アセンブリと名前空間の XML 名前空間宣言を追加します。

    ```xaml
    xmlns:wpf="clr-namespace:Xamarin.Forms.Platform.WPF;assembly=Xamarin.Forms.Platform.WPF"
    ```

    次に、`Window`タグを`wpf:FormsApplicationPage`に変更します。 設定を`Title`アプリケーションの名前に変更**します。** 完成した XAML ファイルは次のようになります。

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

8. WPF プロジェクトの**MainWindow.xaml.cs**ファイルを編集します。 2 つの`using`新しいディレクティブを追加します。

    ```csharp
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.WPF;
    ```

    の基本クラスを`MainWindow`に`Window`変更`FormsApplicationPage`します。 呼び`InitializeComponent`出しの後に、次の 2 つのステートメントを追加します。

    ```csharp
    Forms.Init();
    LoadApplication(new BoxViewClock.App());
    ```

    コメントと未使用`using`のディレクティブを除いて、完全な**MainWindows.xaml.cs**ファイルは次のようになります。

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

9. **ソリューション エクスプローラー**で WPF プロジェクトを右クリックし、[**スタートアップ プロジェクトとして設定]** を選択します。 F5 キーを押して、Windows デスクトップ上の Visual Studio デバッガーでプログラムを実行します。

    ![WPF ボックスビュー クロック](wpf-images/wpf-boxviewclock.png "WPF ボックスビュー クロック" )

## <a name="platform-specifics"></a>プラットフォームの詳細

Xamarin.Forms アプリケーションが実行されているプラットフォームは、コードまたは XAML から判断できます。 これにより、WPF で実行されているプログラムの特性を変更できます。 コードでは、定数 (文字列`Device.RuntimePlatform`"WPF" と同じ) の値`Device.WPF`を比較します。 一致する場合、アプリケーションは WPF で実行されています。

XAML では、このタグを`OnPlatform`使用して、プラットフォームに固有のプロパティ値を選択できます。

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

ウィンドウの初期サイズは、WPF **MainWindow.xaml**ファイルで調整できます。

```xaml
Title="BoxViewClock" Height="450" Width="800"
```

## <a name="issues"></a>発行

これはプレビューなので、すべてが実稼働準備ができているわけではないことを期待する必要があります。 Xamarin.Forms のすべての NuGet パッケージが WPF の準備ができているわけではありませんし、一部の機能が完全に動作していない可能性があります。

## <a name="related-video"></a>関連ビデオ

> [!VIDEO https://youtube.com/embed/Fy9N6OSxK64]

**Xamarin.Forms 3.0 WPF サポートビデオ**
