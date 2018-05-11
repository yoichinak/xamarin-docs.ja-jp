---
title: 'GTK # プラットフォームのセットアップ'
description: 'Xamarin.Forms ようになりました GTK # プラットフォーム用のプレビューがサポート'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 275ec851a2fd8e96adecfeca5daf6a66add7bd92
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/10/2018
---
# <a name="gtk-platform-setup"></a>GTK # プラットフォームのセットアップ

![[プレビュー]](~/media/shared/preview.png)

Xamarin.Forms では、GTK # アプリ用のプレビューがサポートできるようになりました。 GTK # は、グラフィカル ユーザー インターフェイス ツールキット gtk toolkit とさまざまな GNOME ライブラリにリンクして .NET の Mono を使用して完全にネイティブの「なしグラフィックス アプリの開発を許可します。 この記事では、まず Xamarin.Forms ソリューションを GTK # プロジェクトを追加する方法を示します。

開始、新しい Xamarin.Forms ソリューションを作成またはなど、既存の Xamarin.Forms ソリューションを使用する前に[ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/)です。

> [!NOTE]
> この記事では、for Mac VS2017 および Visual Studio で、まず Xamarin.Forms ソリューションを GTK # アプリの追加の焦点を当てています、中に、実行することもで[MonoDevelop](http://www.monodevelop.com/) Linux 用です。

## <a name="adding-a-gtk-app"></a>GTK # アプリを追加します。

GTK # macOS 用と Linux がの一部としてインストールされている[モノラル](http://www.mono-project.com/download/stable/)です。 GTK # for .NET にインストールできる Windows、 [GTK # インストーラー](http://www.mono-project.com/download/stable/#download-win)です。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Windows デスクトップで実行される GTK # アプリを追加する手順に従います。

1. Visual Studio 2017 でのソリューション名を右クリックして**ソリューション エクスプ ローラー**選択**追加 > 新しいプロジェクト.**.

2. **新しいプロジェクト**ウィンドウで、左クリックで**Visual c#** と**Windows クラシック デスクトップ**です。 プロジェクトの種類の一覧で選択**クラス ライブラリ (.NET Framework)**、ことを確認して、 **Framework**ドロップダウンが .NET Framework 4.7 の最小値に設定します。

3. 使用してプロジェクトの名前を入力、 **GTK**拡張子、たとえば**GameOfLife.GTK**です。 をクリックして、**参照**ボタンを他のプラットフォームを含むフォルダーを選択し、プロジェクト、およびキーを押して**フォルダーの選択**です。 これにより、GTK プロジェクト、ソリューション内の他のプロジェクトと同じディレクトリに入ります。

    ![新しい GTK プロジェクトの追加](gtk-images/win/add-new-project.png "新しい GTK プロジェクトの追加")

    キーを押して、 **OK**プロジェクトを作成するボタンをクリックします。

4. **ソリューション エクスプ ローラー**新しい GTK プロジェクトを右クリックして、選択**NuGet パッケージの管理**です。 選択、**参照** タブで、をクリックして、 **Include prerelease**  チェック ボックスを検索および**Xamarin.Forms** 3.0 以上です。

    ![Xamarin.Forms NuGet パッケージを選択して](gtk-images/win/select-forms-nuget-package.png "Xamarin.Forms NuGet パッケージの選択")

    パッケージを選択し、をクリックして、**インストール**ボタンをクリックします。

5. 今すぐ検索、 **Xamarin.Forms.Platform.GTK** 3.0 パッケージ以上です。

    ![Xamarin.Forms.Platform.GTK NuGet パッケージを選択して](gtk-images/win/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet パッケージの選択")

    パッケージを選択し、をクリックして、**インストール**ボタンをクリックします。

6. **ソリューション エクスプ ローラー**ソリューション名を右クリックし、選択、 **Manage NuGet Packages for Solution**です。 選択、**更新** タブおよび**Xamarin.Forms**パッケージです。 すべてのプロジェクトを選択し、GTK プロジェクトによって使用されると同じ Xamarin.Forms バージョンに更新します。

7. **ソリューション エクスプ ローラー**を右クリックして**参照**GTK プロジェクト。 **参照マネージャー**ダイアログで、**プロジェクト**、左右のチェック、標準的な .NET または共有プロジェクトの横にあるチェック ボックスをオンにします。

    ![共有プロジェクト参照](gtk-images/win/reference-shared-project.png "共有プロジェクト参照")

8. **参照マネージャー**ダイアログで、キーを押して、**参照**ボタンをクリックしを参照、 **C:\Program Files (x86)\GtkSharp\2.12\lib**フォルダーを選択、 **atk sharp.dll**、 **gdk sharp.dll**、**林間地 sharp.dll**、 **glib sharp.dll**、 **gtk dotnet.dll**、 **gtk sharp.dll**ファイル。

    ![GTK # ライブラリを参照](gtk-images/win/reference-gtk-libraries.png "GTK # ライブラリを参照")

    キーを押して、 **OK**参照を追加するボタンをクリックします。

9. GTK プロジェクトで名前を変更**Class1.cs**に**Program.cs**です。

10. GTK プロジェクトで、編集、 **Program.cs**ファイルの次のコードのようにします。

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

    このコードは、GTK # と Xamarin.Forms を初期化します、アプリケーション ウィンドウを作成し、アプリを実行します。

11. **ソリューション エクスプ ローラー**GTK プロジェクトを右クリックして、選択**プロパティ**です。

12. **プロパティ**ウィンドウで、**アプリケーション** タブを変更、**出力の種類**ドロップダウンを**Windows アプリケーション**です。

    ![プロジェクト出力の種類を変更する](gtk-images/win/change-project-output-type.png "プロジェクト出力の種類を変更します。")

13. **ソリューション エクスプ ローラー**、WPF プロジェクトを右クリックし **スタートアップ プロジェクトとして設定**です。 F5 キーを押して、Windows デスクトップで、Visual Studio デバッガーでプログラムを実行します。

    ![GTK # 人生ゲーム](gtk-images/win/gtk-gameoflife.png "GTK # ライフのゲーム")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac のデスクトップで実行される GTK # アプリを追加する手順に従います。

1. Mac 用 Visual Studio で Xamarin.Forms ソリューションを右クリックし、選択**追加 > 新しいプロジェクトの追加.**.

2. **新しいプロジェクト**ウィンドウを選択**他 > .NET > Gtk # 2.0 プロジェクト**とキーを押します **[次へ]** です。

3. 使用してプロジェクトの名前を入力、 **GTK**拡張機能の例については、 **GameOfLife.GTK**、キーを押します**作成**です。

4. **ソリューション パッド**を右クリックして**パッケージ > パッケージを追加しています.** GTK のプロジェクト、および Xamarin.Forms 3.0 リリース前の NuGet パッケージを追加またはそれ以上です。

    ![Xamarin.Forms NuGet パッケージを選択して](gtk-images/mac/select-forms-nuget-package.png "Xamarin.Forms NuGet パッケージの選択")

5. **ソリューション パッド**を右クリックして**パッケージ > パッケージを追加しています.** GTK のプロジェクト、および Xamarin.Forms.Platform.GTK 3.0 リリース前の NuGet パッケージを追加またはそれ以上です。

    ![Xamarin.Forms.Platform.GTK NuGet パッケージを選択して](gtk-images/mac/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet パッケージの選択")

6. GTK プロジェクトによって使用されると同じ Xamarin.Forms バージョンを使用する他のプラットフォーム プロジェクトを更新します。

7. **ソリューション パッド**を右クリックして**参照 > 参照の編集.** GTK のプロジェクト、および Xamarin.Forms プロジェクト (.NET 標準またはプロジェクトの共有) への参照を追加します。

    ![共有プロジェクト参照](gtk-images/mac/reference-shared-project.png "共有プロジェクト参照")

8. 編集、 **Program.cs**が次のコードのようになりますので、GTK プロジェクトのファイル。

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

    このコードは、GTK # と Xamarin.Forms を初期化します、アプリケーション ウィンドウを作成し、アプリを実行します。

9. **ソリューション パッド**GTK プロジェクトを右クリックし、選択、**スタートアップ プロジェクトとして設定**です。

10. Mac のツールバーの Visual Studio でキーを押して、**開始**ボタン (再生 ボタンのような三角形のボタン) アプリを起動します。

    ![GTK # 人生ゲーム](gtk-images/mac/gtk-gameoflife.png "GTK # ライフのゲーム")

-----

## <a name="next-steps"></a>次の手順

### <a name="platform-specifics"></a>プラットフォームの詳細

Xamarin.Forms アプリケーションは、XAML またはコードからで実行されているプラットフォームを指定できます。 これにより、GTK # で実行されているときに、プログラムの特性を変更することができます。 コードでは、値を比較`Device.RuntimePlatform`で、`Device.GTK`定数 (これは、文字列"GTK"に相当します)。 一致がある場合は、GTK # でアプリケーションが実行されています。

XAML では、使用することができます、`OnPlatform`タグをプラットフォームに固有のプロパティ値を選択します。

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

起動時にアプリケーション アイコンを設定できます。

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>テーマ

GTK 場合は、使用できるさまざまなテーマがおよび Xamarin.Forms アプリから使用できます。

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>ネイティブのフォーム

ネイティブのフォームでは、Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-GTK # プロジェクトを含む、ネイティブ プロジェクトで使用されるページを派生します。 これには、インスタンスを作成することで、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生したページと、ネイティブ GTK # を使用して型への変換、`CreateContainer`拡張メソッド。

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

ネイティブ数字形式の詳細については、次を参照してください。[ネイティブ フォーム](~/xamarin-forms/platform/native-forms.md)です。

## <a name="issues"></a>Issues

これは、プレビューわけではないが実稼働の準備完了であることが予想されます。 現在の実装の状態を参照してください。[ステータス](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)、し、現在の既知の問題では、次を参照してください。[保留中と既知の問題](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md)です。
