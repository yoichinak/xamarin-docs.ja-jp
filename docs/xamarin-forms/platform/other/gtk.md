---
title: 'GTK # プラットフォームのセットアップ'
description: 'Xamarin.Formsでは、GTK # プラットフォームのプレビューがサポートされるようになりました'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a5635da9f7c083609ce1e0f120d0613fff9bd77b
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84198108"
---
# <a name="gtk-platform-setup"></a>GTK # プラットフォームのセットアップ

![プレビュー](~/media/shared/preview.png)

Xamarin.Formsでは、GTK # アプリのプレビューがサポートされるようになりました。 GTK # は、GTK + toolkit とさまざまな GNOME ライブラリをリンクするグラフィカルユーザーインターフェイスツールキットで、Mono と .NET を使用して完全にネイティブな GNOME グラフィックスアプリを開発できます。 この記事では、GTK # プロジェクトをソリューションに追加する方法について説明し Xamarin.Forms ます。

> [!IMPORTANT]
> Xamarin.Formsこのコミュニティでは、GTK # のサポートが提供されています。 詳細については、「 [ Xamarin.Forms プラットフォームのサポート](https://github.com/xamarin/Xamarin.Forms/wiki/Platform-Support)」を参照してください。

開始する前に、新しいソリューションを作成する Xamarin.Forms か、または既存 Xamarin.Forms のソリューション ( [**GameOfLife**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/boxview-gameoflife)例) を使用してください。

> [!NOTE]
> この記事では、VS2017 と Visual Studio for Mac のソリューションに GTK # アプリを追加することに焦点を当てて Xamarin.Forms いますが、Linux 用の[MonoDevelop](https://www.monodevelop.com/)でも実行できます。

## <a name="adding-a-gtk-app"></a>GTK # アプリの追加

MacOS および Linux 用の GTK # は、 [Mono](https://www.mono-project.com/download/stable/)の一部としてインストールされます。 Gtk #[インストーラー](https://www.mono-project.com/download/stable/#download-win)を使用すると、gtk # for .Net を Windows にインストールできます。

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Windows デスクトップで実行される GTK # アプリを追加するには、次の手順に従います。

1. Visual Studio 2019 で**ソリューションエクスプローラー**でソリューション名を右クリックし、[ **> 新しいプロジェクトの追加**] を選択します。

2. [**新しいプロジェクト**] ウィンドウで、左側にある [ **Visual C#** ] と [ **Windows クラシックデスクトップ**] を選択します。 プロジェクトの種類の一覧で、[**クラスライブラリ (.NET Framework)**] を選択し、[ **Framework** ] ドロップダウンが .NET Framework 4.7 の最小値に設定されていることを確認します。

3. **Gtk**拡張子を持つプロジェクトの名前を入力します **。たとえば、**「」と入力します。 [**参照**] ボタンをクリックし、他のプラットフォームプロジェクトが含まれているフォルダーを選択して、 **[フォルダーの選択]** をクリックします。 これにより、ソリューション内の他のプロジェクトと同じディレクトリに GTK プロジェクトが配置されます。

    ![新しい GTK プロジェクトを追加する](gtk-images/win/add-new-project.png "新しい GTK プロジェクトを追加する")

    プロジェクトを作成するには、 **[OK** ] をクリックします。

4. **ソリューションエクスプローラー**で、新しい GTK プロジェクトを右クリックし、[ **NuGet パッケージの管理**] を選択します。 [**参照**] タブを選択し、 **Xamarin.Forms** 3.0 以上を検索します。

    ![NuGet パッケージを選択します Xamarin.Forms](gtk-images/win/select-forms-nuget-package.png "[!ファンド.なし (Xamarin)] NuGet パッケージ")

    パッケージを選択し、[**インストール**] ボタンをクリックします。

5. 次に、を検索** Xamarin.Forms します。Platform. GTK** 3.0 パッケージ以上。

    ![を選択し Xamarin.Forms ます。Platform. GTK NuGet パッケージ](gtk-images/win/select-forms-platform-nuget-package.png "[!ファンド.なし (Xamarin. Forms)]。Platform. GTK NuGet パッケージ")

    パッケージを選択し、[**インストール**] ボタンをクリックします。

6. **ソリューションエクスプローラー**で、ソリューション名を右クリックし、[**ソリューションの NuGet パッケージの管理**] を選択します。 [**更新**] タブとパッケージを選択し **Xamarin.Forms** ます。 すべてのプロジェクトを選択し、 Xamarin.Forms GTK プロジェクトで使用されているものと同じバージョンに更新します。

7. **ソリューションエクスプローラー**で、GTK プロジェクトの [**参照**] を右クリックします。 [**参照マネージャー** ] ダイアログで、左側の [**プロジェクト**] を選択し、.NET Standard または共有プロジェクトの横にあるチェックボックスをオンにします。

    ![共有プロジェクトの参照](gtk-images/win/reference-shared-project.png "共有プロジェクトの参照")

8. [**参照マネージャー** ] ダイアログで、[**参照**] ボタンをクリックし、 **C:\Program files (x86) \GtkSharp\2.12\lib**フォルダーを参照して、 **atk-sharp.dll**、 **gdk-sharp.dll**、 **glade-sharp.dll**、 **glib-sharp.dll**、 **gtk-dotnet.dll**、 **gtk-sharp.dll**ファイルを選択します。

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

    このコードは、GTK # を初期化し Xamarin.Forms 、アプリケーションウィンドウを作成して、アプリを実行します。

11. **ソリューションエクスプローラー**で、GTK プロジェクトを右クリックし、[**プロパティ**] を選択します。

12. [**プロパティ**] ウィンドウで、[**アプリケーション**] タブを選択し、[**出力の種類**] ドロップダウンを [ **Windows アプリケーション**] に変更します。

    ![プロジェクトの出力の種類を変更する](gtk-images/win/change-project-output-type.png "プロジェクトの出力の種類を変更する")

13. **ソリューションエクスプローラー**で、GTK プロジェクトを右クリックし、[**スタートアッププロジェクトに設定**] を選択します。 F5 キーを押して、Windows デスクトップで Visual Studio デバッガーを使用してプログラムを実行します。

    ![GTK # の人生ゲーム](gtk-images/win/gtk-gameoflife.png "GTK # の人生ゲーム")

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/macos)

Mac デスクトップで実行される GTK # アプリを追加するには、次の手順に従います。

1. Visual Studio for Mac で、ソリューションを右クリックし、[ Xamarin.Forms **追加] > [新しいプロジェクトの追加**] の順に選択します。

2. [**新しいプロジェクト**] ウィンドウで、[**その他 > .net > Gtk # 2.0 プロジェクト**] を選択し、[**次へ**] をクリックします。

3. **Gtk**という拡張子を持つプロジェクトの名前を入力し、[**作成** **] を**クリックします。

4. **Solution Pad**で、GTK プロジェクトの [**パッケージ] > [パッケージの追加**] を右クリックし、 Xamarin.Forms 3.0 プレリリース版の NuGet パッケージを追加します。

    ![NuGet パッケージを選択します Xamarin.Forms](gtk-images/mac/select-forms-nuget-package.png "[!ファンド.なし (Xamarin)] NuGet パッケージ")

5. **Solution Pad**で、[パッケージ] を右クリックし、GTK プロジェクトの [パッケージ **> 追加**] をクリックして、を追加します。 Xamarin.FormsPlatform. GTK 3.0 プレリリース NuGet パッケージ以上。

    ![を選択し Xamarin.Forms ます。Platform. GTK NuGet パッケージ](gtk-images/mac/select-forms-platform-nuget-package.png "[!ファンド.なし (Xamarin. Forms)]。Platform. GTK NuGet パッケージ")

6. Xamarin.FormsGTK プロジェクトで使用されているものと同じバージョンを使用するように、他のプラットフォームプロジェクトを更新します。

7. **Solution Pad**で、[参照] を右クリックして、GTK プロジェクトの [参照の**編集] >** し、プロジェクトへの参照を追加します Xamarin.Forms (.NET Standard または共有プロジェクトのいずれか)。

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

    このコードは、GTK # を初期化し Xamarin.Forms 、アプリケーションウィンドウを作成して、アプリを実行します。

9. **Solution Pad**で、GTK プロジェクトを右クリックし、[**スタートアッププロジェクトに設定**] を選択します。

10. Visual Studio for Mac ツールバーで、[**スタート**] ボタン (再生ボタンに似た三角形のボタン) を押してアプリを起動します。

    ![GTK # の人生ゲーム](gtk-images/mac/gtk-gameoflife.png "GTK # の人生ゲーム")

-----

## <a name="next-steps"></a>次の手順

### <a name="platform-specifics"></a>プラットフォーム固有設定

Xamarin.FormsXAML またはコードから、アプリケーションが実行されているプラットフォームを特定できます。 これにより、GTK # で実行されているプログラムの特性を変更することができます。 コードで、の値と `Device.RuntimePlatform` `Device.GTK` 定数 (文字列 "GTK" に等しい) を比較します。 一致するものがある場合、アプリケーションは GTK # で実行されています。

XAML では、タグを使用し `OnPlatform` て、プラットフォームに固有のプロパティ値を選択できます。

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

GTK # にはさまざまなテーマが用意されており、アプリから使用でき Xamarin.Forms ます。

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>ネイティブ フォーム

ネイティブフォームを使用 Xamarin.Forms [`ContentPage`](xref:Xamarin.Forms.ContentPage) すると、の派生ページを、GTK # プロジェクトなどのネイティブプロジェクトで使用できます。 これを実現するには、の派生ページのインスタンスを作成 [`ContentPage`](xref:Xamarin.Forms.ContentPage) し、拡張メソッドを使用してネイティブの GTK # 型に変換し `CreateContainer` ます。

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

ネイティブフォームの詳細については、「[ネイティブフォーム](~/xamarin-forms/platform/native-forms.md)」を参照してください。

## <a name="issues"></a>発行

これはプレビューなので、運用環境の準備ができているわけではありません。 現在の実装状態については、「 [status](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)」を参照してください。現在の既知の問題については、「 [Pending & 既知の問題](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md)」を参照してください。
