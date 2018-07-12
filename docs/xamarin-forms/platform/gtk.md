---
title: 'GTK # プラットフォームのセットアップ'
description: 'Xamarin.Forms にはなりました GTK # プラットフォームのプレビューをサポート'
ms.prod: xamarin
ms.assetid: 3417FB95-3E4B-47DA-85D0-F34832747236
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
ms.openlocfilehash: 7f68b7c8affc11b50bdb4a2fc9589f8dcbfb45ec
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2018
ms.locfileid: "38830481"
---
# <a name="gtk-platform-setup"></a>GTK # プラットフォームのセットアップ

![[プレビュー]](~/media/shared/preview.png)

Xamarin.Forms では、GTK # アプリのプレビューをサポートできるようになりました。 GTK # は、グラフィカル ユーザー インターフェイス ツールキット GTK + toolkit とさまざまな GNOME ライブラリをリンクする Mono と .NET を使用して完全にネイティブの「なしグラフィックス アプリの開発を許可します。 この記事では、GTK # プロジェクトを Xamarin.Forms ソリューションに追加する方法を示します。

開始、新しい Xamarin.Forms ソリューションを作成または既存の Xamarin.Forms ソリューションを使用して、たとえば、前に[ **GameOfLife**](https://developer.xamarin.com/samples/xamarin-forms/BoxView/GameOfLife/)します。

> [!NOTE]
> この記事は、Mac の VS2017 および Visual Studio で Xamarin.Forms ソリューションに GTK # アプリを追加する方法では、これもで実行できる[MonoDevelop](http://www.monodevelop.com/) for Linux。

## <a name="adding-a-gtk-app"></a>GTK # アプリを追加します。

GTK # の macOS および Linux がの一部としてインストールされている[Mono](http://www.mono-project.com/download/stable/)します。 GTK # for .NET をインストールできるで Windows、 [GTK # インストーラー](http://www.mono-project.com/download/stable/#download-win)します。

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Windows デスクトップで実行される GTK # アプリを追加するこれらの手順に従います。

1. ソリューション名を右クリックし、Visual Studio 2017 で**ソリューション エクスプ ローラー**選択**追加 > 新しいプロジェクト.**.

2. **新しいプロジェクト**ウィンドウで、左側の選択で**Visual c#** と**Windows クラシック デスクトップ**します。 プロジェクトの種類の一覧で選択**クラス ライブラリ (.NET Framework)**、いることを確認し、 **Framework**ドロップダウンは、.NET Framework 4.7 の最小値に設定されます。

3. 使用してプロジェクトの名前を入力、 **GTK**拡張機能の例では、 **GameOfLife.GTK**します。 をクリックして、**参照**ボタン、その他のプラットフォームを含むフォルダーを選択してプロジェクト、およびキーを押して**フォルダーの選択**。 GTK プロジェクトをソリューション内の他のプロジェクトと同じディレクトリに、これには。

    ![新しい GTK プロジェクト追加](gtk-images/win/add-new-project.png "新しい GTK プロジェクトの追加")

    キーを押して、 **OK**プロジェクトを作成するボタンをクリックします。

4. **ソリューション エクスプ ローラー**新しい GTK プロジェクトを右クリックし、選択、 **NuGet パッケージの管理**します。 選択、**参照**タブをクリックし、検索**Xamarin.Forms** 3.0 以降。

    ![Xamarin.Forms NuGet パッケージを選択して](gtk-images/win/select-forms-nuget-package.png "Xamarin.Forms NuGet パッケージを選択します。")

    パッケージを選択し、をクリックして、**インストール**ボタンをクリックします。

5. 今すぐ検索、 **Xamarin.Forms.Platform.GTK** 3.0 パッケージ以上。

    ![Xamarin.Forms.Platform.GTK NuGet パッケージを選択して](gtk-images/win/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet パッケージを選択します。")

    パッケージを選択し、をクリックして、**インストール**ボタンをクリックします。

6. **ソリューション エクスプ ローラー**でソリューション名を右クリックし、選択**ソリューションの NuGet パッケージの管理**します。 選択、 **Update**タブおよび**Xamarin.Forms**パッケージ。 すべてのプロジェクトを選択し、GTK プロジェクトによって使用されると同じ Xamarin.Forms バージョンを更新します。

7. **ソリューション エクスプ ローラー**を右クリックして**参照**GTK プロジェクト。 **参照マネージャー**ダイアログ ボックスで、**プロジェクト**左、および .NET Standard または共有プロジェクトの横にあるチェック ボックスをチェックします。

    ![共有プロジェクト参照](gtk-images/win/reference-shared-project.png "共有プロジェクトの参照")

8. **参照マネージャー**ダイアログ ボックスで、キーを押して、**参照**ボタンをクリックしを参照、 **C:\Program Files (x86)\GtkSharp\2.12\lib**フォルダーと選択、 **atk sharp.dll**、 **gdk sharp.dll**、 **glade sharp.dll**、 **glib sharp.dll**、 **gtk dotnet.dll**、 **gtk sharp.dll**ファイル。

    ![GTK # ライブラリを参照](gtk-images/win/reference-gtk-libraries.png "GTK # ライブラリを参照")

    キーを押して、 **OK**参照を追加するボタンをクリックします。

9. GTK プロジェクトで名前を変更**Class1.cs**に**Program.cs**します。

10. GTK プロジェクトでは、編集、 **Program.cs**ファイルの次のコードのようにします。

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

11. **ソリューション エクスプ ローラー**GTK プロジェクトを右クリックし、選択、**プロパティ**します。

12. **プロパティ**ウィンドウで、**アプリケーション**タブを変更、**出力の種類**ドロップダウンを**Windows アプリケーション**します。

    ![プロジェクト出力の種類を変更する](gtk-images/win/change-project-output-type.png "プロジェクト出力の種類を変更します。")

13. **ソリューション エクスプ ローラー**を WPF プロジェクトを右クリックし、**スタートアップ プロジェクトとして設定**します。 F5 キーを押して、Windows デスクトップで、Visual Studio デバッガーでプログラムを実行します。

    ![GTK # の耐用年数のゲーム](gtk-images/win/gtk-gameoflife.png "GTK # の耐用年数のゲーム")

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Mac のデスクトップで実行される GTK # アプリを追加するこれらの手順に従います。

1. Visual studio for Mac では、Xamarin.Forms ソリューションを右クリックし、選択**追加 > 新しいプロジェクトの追加.**.

2. **新しいプロジェクト**ウィンドウを選択**他 > .NET > Gtk # 2.0 プロジェクト**キーを押します**次**。

3. 使用してプロジェクトの名前を入力、 **GTK**拡張機能の例では、 **GameOfLife.GTK**、キーを押します**作成**です。

4. **Solution Pad**を右クリックして**パッケージ > パッケージを追加しています.** GTK のプロジェクトし、Xamarin.Forms の 3.0 のプレリリース版の NuGet パッケージを追加またはそれ以上。

    ![Xamarin.Forms NuGet パッケージを選択して](gtk-images/mac/select-forms-nuget-package.png "Xamarin.Forms NuGet パッケージを選択します。")

5. **Solution Pad**を右クリックして**パッケージ > パッケージを追加しています.** GTK のプロジェクト、および Xamarin.Forms.Platform.GTK 3.0 のプレリリース版の NuGet パッケージを追加またはそれ以上。

    ![Xamarin.Forms.Platform.GTK NuGet パッケージを選択して](gtk-images/mac/select-forms-platform-nuget-package.png "Xamarin.Forms.Platform.GTK NuGet パッケージを選択します。")

6. GTK プロジェクトによって使用されると同じ Xamarin.Forms バージョンを使用する他のプラットフォーム プロジェクトを更新します。

7. **Solution Pad**を右クリックして**参照 > 参照の編集.** GTK のプロジェクト、および Xamarin.Forms プロジェクト (.NET Standard、または共有プロジェクト) への参照を追加します。

    ![共有プロジェクト参照](gtk-images/mac/reference-shared-project.png "共有プロジェクトの参照")

8. 編集、 **Program.cs**を次のコードと似ていますので、GTK プロジェクトのファイル。

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

9. **Solution Pad**GTK プロジェクトを右クリックし、選択、**スタートアップ プロジェクトとして設定**します。

10. Visual Studio for Mac ツールバーでは、キーを押して、**開始**ボタン (再生ボタンに似た三角形のボタン)、アプリを起動します。

    ![GTK # の耐用年数のゲーム](gtk-images/mac/gtk-gameoflife.png "GTK # の耐用年数のゲーム")

-----

## <a name="next-steps"></a>次の手順

### <a name="platform-specifics"></a>プラットフォーム固有設定

Xamarin.Forms アプリケーションは XAML またはコードのいずれかで実行されているプラットフォームを判断できます。 これにより、GTK # で実行されているときに、プログラムの特性を変更することができます。 コードでは、値を比較`Device.RuntimePlatform`で、`Device.GTK`定数 (これは、文字列"GTK"と等しくなります)。 一致がある場合は、GTK # で、アプリケーションが実行されています。

XAML で使用できます、`OnPlatform`タグをプラットフォームに固有のプロパティ値を選択します。

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

スタートアップ時にアプリのアイコンを設定できます。

```csharp
window.SetApplicationIcon("icon.png");
```

### <a name="themes"></a>テーマ

GTK # の使用可能なさまざまなテーマは、Xamarin.Forms アプリから使用できます。

```csharp
GtkThemes.Init ();
GtkThemes.LoadCustomTheme ("Themes/gtkrc");
```

### <a name="native-forms"></a>ネイティブ フォーム

ネイティブ フォームは、Xamarin.Forms [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-GTK # プロジェクトなどのネイティブ プロジェクトで使用するページを派生します。 インスタンスを作成してこれを実現できます、 [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)-派生ページと、ネイティブ GTK # を使用して型に変換すること、`CreateContainer`拡張メソッド。

```csharp
var settingsView = new SettingsView().CreateContainer();
vbox.PackEnd(settingsView, true, true, 0);
```

ネイティブ数字形式の詳細については、次を参照してください。[ネイティブ フォーム](~/xamarin-forms/platform/native-forms.md)します。

## <a name="issues"></a>Issues

これは、その運用環境の準備がすべてではないことを想定する必要がありますのプレビューです。 現在の実装の状態を参照してください。[状態](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Status.md)、および現在の既知の問題では、次を参照してください。[保留中と既知の問題](https://github.com/jsuarezruiz/forms-gtk-progress/blob/master/Issues-Pending.md)します。
