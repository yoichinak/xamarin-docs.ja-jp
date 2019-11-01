---
title: 'GTK # プラットフォームのセットアップ'
description: 'Xamarin. フォームで GTK # プラットフォームのプレビューがサポートされるようになりました'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: cbc3bceffacd9669c1e2e667faadc2939fd4aa1f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73005926"
---
# <a name="gtk-platform-setup"></a>GTK # プラットフォームのセットアップ

![[プレビュー]](~/media/shared/preview.png)

Xamarin. フォームで、GTK # アプリのプレビューがサポートされるようになりました。 GTK # は、GTK + toolkit とさまざまな GNOME ライブラリをリンクするグラフィカルユーザーインターフェイスツールキットで、Mono と .NET を使用して完全にネイティブな GNOME グラフィックスアプリを開発できます。 この記事では、GTK # プロジェクトを Xamarin. Forms ソリューションに追加する方法について説明します。

> [!IMPORTANT]
> GTK # の Xamarin. Forms サポートはコミュニティによって提供されます。 詳細については、「 [Xamarin. Forms Platform Support](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)」を参照してください。

開始する前に、新しい Xamarin. Forms ソリューションを作成するか、既存の Xamarin. Forms ソリューションを使用し[**てください。** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)たとえば、「」というようにします。

> [!NOTE]
> この記事では、VS2017 と Visual Studio for Mac の Xamarin. Forms ソリューションに GTK # アプリを追加することに焦点を当てていますが、Linux 用の[MonoDevelop](https://www.monodevelop.com/)でも実行できます。

## <a name="adding-a-gtk-app"></a>GTK # アプリの追加

MacOS および Linux 用の GTK # は、 [Mono](https://www.mono-project.com/download/stable/)の一部としてインストールされます。 Gtk #[インストーラー](https://www.mono-project.com/download/stable/#download-win)を使用すると、gtk # for .Net を Windows にインストールできます。

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Windows デスクトップで実行される GTK # アプリを追加するには、次の手順に従います。

1. Visual Studio 2019 で**ソリューションエクスプローラー**でソリューション名を右クリックし、 **[> 新しいプロジェクトの追加]** を選択します。

2. **[新しいプロジェクト]** ウィンドウで、左側の [**ビジュアルC#** と**Windows クラシックデスクトップ**] を選択します。 プロジェクトの種類の一覧で、 **[クラスライブラリ (.NET Framework)]** を選択し、 **[Framework]** ドロップダウンが .NET Framework 4.7 の最小値に設定されていることを確認します。

3. **Gtk**拡張子を持つプロジェクトの名前を入力します **。たとえば、** 「」と入力します。 **[参照]** ボタンをクリックし、他のプラットフォームプロジェクトが含まれているフォルダーを選択して、 **[フォルダーの選択]** をクリックします。 これにより、ソリューション内の他のプロジェクトと同じディレクトリに GTK プロジェクトが配置されます。

    ![新しい GTK プロジェクトを追加する](gtk-images/win/add-new-project.png "新しい GTK プロジェクトを追加する")

    プロジェクトを作成するには、 **[OK** ] をクリックします。

4. **ソリューションエクスプローラー**で、新しい GTK プロジェクトを右クリックし、 **[NuGet パッケージの管理]** を選択します。 **[参照]** タブを選択し、「 **Xamarin. Forms** 3.0 以上」を検索します。

    ![Xamarin. Forms NuGet パッケージを選択します。](gtk-images/win/select-forms-nuget-package.png "Xamarin. Forms NuGet パッケージを選択します。")

    パッケージを選択し、 **[インストール]** ボタンをクリックします。

5. 次に、 **Xamarin** . 3.0 パッケージ以上を検索します。

    ![Xamarin. 形式の NuGet パッケージを選択します。](gtk-images/win/select-forms-platform-nuget-package.png "Xamarin. 形式の NuGet パッケージを選択します。")

    パッケージを選択し、 **[インストール]** ボタンをクリックします。

6. **ソリューションエクスプローラー**で、ソリューション名を右クリックし、 **[ソリューションの NuGet パッケージの管理]** を選択します。 **[更新]** タブと **[Xamarin]** パッケージを選択します。 すべてのプロジェクトを選択し、GTK プロジェクトで使用されているものと同じ Xamarin 形式のバージョンに更新します。

7. **ソリューションエクスプローラー**で、GTK プロジェクトの **[参照]** を右クリックします。 **[参照マネージャー]** ダイアログで、左側の **[プロジェクト]** を選択し、.NET Standard または共有プロジェクトの横にあるチェックボックスをオンにします。

    ![共有プロジェクトの参照](gtk-images/win/reference-shared-project.png "共有プロジェクトの参照")

8. **[参照マネージャー]** ダイアログで、 **[参照]** ボタンをクリックし、 **C:\Program files (x86) \GtkSharp\2.12\lib**フォルダーを参照して、 **atk-sharp**、 **gdk-sharp**、 **glade-sharp**を**選択します。glib-sharp**、 **gtk-dotnet**、 **gtk-sharp**の各ファイル。

    ![GTK # ライブラリを参照する](gtk-images/win/reference-gtk-libraries.png "GTK # ライブラリを参照する")

    参照を追加するには、 **[OK** ] をクリックします。

9. GTK プロジェクトで、 **Class1.cs**の名前を**Program.cs**に変更します。

10. GTK プロジェクトで、次のコードのように**Program.cs**ファイルを編集します。

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    このコードは、GTK # と Xamarin. フォームを初期化し、アプリケーションウィンドウを作成して、アプリを実行します。

11. **ソリューションエクスプローラー**で、GTK プロジェクトを右クリックし、 **[プロパティ]** を選択します。

12. **[プロパティ]** ウィンドウで、 **[アプリケーション]** タブを選択し、 **[出力の種類]** ドロップダウンを **[Windows アプリケーション]** に変更します。

    ![プロジェクトの出力の種類を変更する](gtk-images/win/change-project-output-type.png "プロジェクトの出力の種類を変更する")

13. **ソリューションエクスプローラー**で、GTK プロジェクトを右クリックし、 **[スタートアッププロジェクトに設定]** を選択します。 F5 キーを押して、Windows デスクトップで Visual Studio デバッガーを使用してプログラムを実行します。

    ![GTK # の人生ゲーム](gtk-images/win/gtk-gameoflife.png "GTK # の人生ゲーム")

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Mac デスクトップで実行される GTK # アプリを追加するには、次の手順に従います。

1. Visual Studio for Mac で、Xamarin. Forms ソリューションを右クリックし、[**追加] > [新しいプロジェクトの追加**] の順に選択します。

2. **[新しいプロジェクト]** ウィンドウで、 **[その他 > .net > Gtk # 2.0 プロジェクト]** を選択し、 **[次へ]** をクリックします。

3. **Gtk**という拡張子を持つプロジェクトの名前を入力し、[**作成** **] を**クリックします。

4. **Solution Pad**で、[パッケージ] を右クリックして、GTK プロジェクトの [パッケージ **> 追加**] をクリックし、Xamarin 3.0 プレリリースの NuGet パッケージ以上を追加します。

    ![Xamarin. Forms NuGet パッケージを選択します。](gtk-images/mac/select-forms-nuget-package.png "Xamarin. Forms NuGet パッケージを選択します。")

5. **Solution Pad**で、[パッケージ] を右クリックして、gtk プロジェクトの [パッケージ **> 追加**] をクリックし、Xamarin 3.0 プレリリースの NuGet パッケージ以上を追加します。

    ![Xamarin. 形式の NuGet パッケージを選択します。](gtk-images/mac/select-forms-platform-nuget-package.png "Xamarin. 形式の NuGet パッケージを選択します。")

6. 他のプラットフォームプロジェクトを更新して、GTK プロジェクトで使用されているものと同じ Xamarin 形式のバージョンを使用するようにします。

7. **Solution Pad**で、[参照] を右クリックし、GTK プロジェクトの [参照の**編集] >** して、Xamarin. Forms プロジェクトへの参照を追加します (.NET Standard または共有プロジェクト)。

    ![共有プロジェクトの参照](gtk-images/mac/reference-shared-project.png "共有プロジェクトの参照")

8. 次のコードのように、GTK プロジェクトの**Program.cs**ファイルを編集します。

    ```csharp
    using System;
    using Xamarin.Forms;
    using Xamarin.Forms.Platform.GTK;

    namespace GameOfLife.GTK
    {
        class MainClass
        {
            [STAThread]
            public static void Main(string[] args)
            {
                Gtk.Application.Init();
                Forms.Init();

                var app = new App();
                var window = new FormsWindow();
                window.LoadApplication(app);
                window.SetApplicationTitle("Game of Life");
                window.Show();

                Gtk.Application.Run();
            }
        }
    }
    ```

    このコードは、GTK # と Xamarin. フォームを初期化し、アプリケーションウィンドウを作成して、アプリを実行します。

9. **Solution Pad**で、GTK プロジェクトを右クリックし、 **[スタートアッププロジェクトに設定]** を選択します。

10. Visual Studio for Mac ツールバーで、 **[スタート]** ボタン (再生ボタンに似た三角形のボタン) を押してアプリを起動します。

    ![GTK # の人生ゲーム](gtk-images/mac/gtk-gameoflife.png "GTK # の人生ゲーム")

-----

## <a name="next-steps"></a>次のステップ

### <a name="platform-specifics"></a>プラットフォーム固有設定

XAML またはコードから、Xamarin アプリケーションが実行されているプラットフォームを特定できます。 これにより、GTK # で実行されているプログラムの特性を変更することができます。 コードで、`Device.RuntimePlatform` の値を `Device.GTK` 定数 (文字列 "GTK" に相当) と比較します。 一致するものがある場合、アプリケーションは GTK # で実行されています。

XAML では、`OnPlatform` タグを使用して、プラットフォームに固有のプロパティ値を選択できます。

```xaml
<Button.TextColor>
    <OnPlatform x:TypeArguments="Color">
        <On Platform="iOS" Value="White" />
        <On Platform="macOS" Value="White" />
        <On Platform="Android" Value="Black" />
        <On Platform="GTK" Value="Blue" />
    </OnPlatform>
</Button.TextColor>
```

### <a name="application-icon"></a>アプリケーション アイコン

スタートアップ時にアプリアイコンを設定できます。

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>テーマ

GTK # にはさまざまなテーマが用意されており、これらは Xamarin. Forms アプリから使用できます。

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>ネイティブ フォーム

ネイティブフォームを使用すると、Xamarin. Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage)の派生ページを、GTK # プロジェクトなどのネイティブプロジェクトで使用できます。 これは、 [`ContentPage`](xref:Xamarin.Forms.ContentPage)派生ページのインスタンスを作成し、`CreateContainer` 拡張メソッドを使用してネイティブの GTK # 型に変換することによって実現できます。

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

ネイティブフォームの詳細については、「[ネイティブフォーム](~/xamarin-forms/platform/native-forms.md)」を参照してください。

## <a name="issues"></a>懸案事項

これはプレビューなので、運用環境の準備ができているわけではありません。 現在の実装状態については、「 [status](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)」を参照してください。現在の既知の問題については、「 [Pending & 既知の問題](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md)」を参照してください。
