---
ms.openlocfilehash: d0b00cf71a6121026dfa368592da8ce8d3be0373
ms.sourcegitcommit: a5a5c5de7d04f046a64e4875e180fc93227bf495
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98635001"
---
# <a name="visual-studio"></a>[Visual Studio](#tab/vswin)

このチュートリアルを完了するには、 **.NET によるモバイル開発** ワークロードがインストールされた、Visual Studio 2019 (最新リリース) が必要です。 さらに、iOS でチュートリアル アプリケーションを構築するには、ペアリング済みの Mac が必要になります。 Xamarin プラットフォームのインストールについては、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。 Mac ビルド ホストへの Visual Studio 2019 の接続については、「[Xamarin.iOS 開発のために Mac とペアリングする](~/ios/get-started/installation/windows/connecting-to-mac/index.md)」を参照してください。

1. Visual Studio を起動し、**AppLifecycleTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**AppLifecycleTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **ソリューション エクスプローラー** の **[AppLifecycleTutorial]** プロジェクトで、**[App.xaml]** を展開し、**[App.xaml.cs]** をダブルクリックして開きます。 次に、以下のように **[App.xaml.cs]** で、`OnStart`、`OnSleep`、および `OnResume` のオーバーライドを更新します。

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    このコードでは、各メソッドが呼び出されたタイミングを示す `Console.WriteLine` ステートメントで、アプリケーションのライフサイクル メソッドのオーバーライドを更新します。

    - `OnStart` メソッドは、アプリケーションの起動時に呼び出されます。
    - `OnSleep` メソッドは、アプリケーションがバックグラウンドに移行したときに呼び出されます。
    - `OnResume` メソッドは、アプリケーションがバックグラウンドから再開したときに呼び出されます。

    > [!NOTE]
    > アプリケーションの終了のメソッドはありません。 通常の状況では、アプリケーションの終了は `OnSleep` メソッドから発生します。

1. Visual Studio ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択したリモート iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 アプリケーションが起動すると、`OnStart` メソッドが呼び出され、**OnStart** が Visual Studio の **[出力]** ウィンドウに出力されます。

    ```
    [Mono] Found as 'java_interop_jnienv_get_object_array_element'.
    OnStart
    [OpenGLRenderer] HWUI GL Pipeline
    ```

    (iOS または Android では [ホーム] ボタンをタップすることで) アプリケーションがバックグラウンド化されたときに、`OnSleep` メソッドが呼び出されます。

    ```
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    OnSleep
    [Mono] Image addref System.Runtime.Serialization[0x83ee19c0] -> System.Runtime.Serialization.dll[0x83f57b00]: 2
    ```

    その後、アプリケーションがバックグラウンドから再開した (iOS ではアプリケーション アイコンをタップし、Android では [概要] ボタンをタップして AppLifecycleTutorial アプリケーションを選択した) ときに、`OnResume` メソッドが呼び出されます。

    ```
    Thread finished: <Thread Pool> #5
    OnResume
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    ```

    > [!NOTE]
    > これらのコード ブロックは、Android でアプリケーションを実行した場合の出力例を示しています。

    Visual Studio で、アプリケーションを停止します。

    Xamarin.Forms アプリのライフサイクルの詳細については、「[Xamarin.Forms App Lifecycle](~/xamarin-forms/app-fundamentals/app-lifecycle.md)」 (Xamarin.Forms アプリのライフサイクル) を参照してください。

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/vsmac)

このチュートリアルを完了するには、iOS と Android のプラットフォームのサポートがインストールされた Visual Studio for Mac (最新リリース) が必要です。 さらに、Xcode (最新リリース) も必要になります。 Xamarin プラットフォームのインストールについて詳しくは、「[Xamarin のインストール](~/get-started/installation/index.md)」を参照してください。

1. Visual Studio for Mac を起動し、**AppLifecycleTutorial** という名前の新しい空の Xamarin.Forms アプリを作成します。

    > [!IMPORTANT]
    > このチュートリアルの C# スニペットと XAML スニペットでは、**AppLifecycleTutorial** という名前のソリューションが必要です。 別の名前を使用すると、コードをこのチュートリアルからソリューションにコピーするときに、ビルド エラーが発生します。

    作成される .NET Standard ライブラリの詳細については、[Xamarin.Forms クイック スタート Deep Dive](~/get-started/first-app/index.md) の [Xamarin.Forms アプリケーションの構造](~/get-started/first-app/index.md)に関するページを参照してください。

1. **Solution Pad** の **[AppLifecycleTutorial]** プロジェクトで、**[App.xaml]** を展開し、**[App.xaml.cs]** をダブルクリックして開きます。 次に、以下のように **[App.xaml.cs]** で、`OnStart`、`OnSleep`、および `OnResume` のオーバーライドを更新します。

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    このコードでは、各メソッドが呼び出されたタイミングを示す `Console.WriteLine` ステートメントで、アプリケーションのライフサイクル メソッドのオーバーライドを更新します。

    - `OnStart` メソッドは、アプリケーションの起動時に呼び出されます。
    - `OnSleep` メソッドは、アプリケーションがバックグラウンドに移行したときに呼び出されます。
    - `OnResume` メソッドは、アプリケーションがバックグラウンドから再開したときに呼び出されます。

    > [!NOTE]
    > アプリケーションの終了のメソッドはありません。 通常の状況では、アプリケーションの終了は `OnSleep` メソッドから発生します。

1. Visual Studio for Mac ツール バーで、 **[開始]** ボタン ([再生] ボタンに似た三角形のボタン) を押し、選択した iOS シミュレーターまたは Android エミュレーター内でアプリケーションを起動します。 アプリケーションが起動すると、`OnStart` メソッドが呼び出され、**OnStart** が Visual Studio for Mac の **[アプリケーション出力]** ウィンドウに出力されます。

    ```
    2019-02-11 12:05:23.164761+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:05:23.165297+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    2019-02-11 12:05:23.685430+0000 AppLifecycleTutorial.iOS[4089:361037] OnStart
    ```

    (iOS または Android では [ホーム] ボタンをタップすることで) アプリケーションがバックグラウンド化されたときに、`OnSleep` メソッドが呼び出されます。

    ```
    2019-02-11 12:06:12.394301+0000 AppLifecycleTutorial.iOS[4089:361037] OnSleep
    Thread started: <Thread Pool> #3
    Thread started: <Thread Pool> #4
    Thread started: <Thread Pool> #5
    Thread started: <Thread Pool> #6
    2019-02-11 12:06:13.056968+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:06:13.057264+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    ```

    その後、アプリケーションがバックグラウンドから再開した (iOS ではアプリケーション アイコンをタップし、Android では [概要] ボタンをタップして AppLifecycleTutorial アプリケーションを選択した) ときに、`OnResume` メソッドが呼び出されます。

    ```
    2019-02-11 12:07:10.321905+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4130
    2019-02-11 12:07:10.322557+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskCopyDebugDescription: AppLifecycleTuto[4130]/0#-1 LF=0
    2019-02-11 12:07:17.110938+0000 AppLifecycleTutorial.iOS[4130:365695] OnResume
    ```

    > [!NOTE]
    > これらのコード ブロックは、iOS でアプリケーションを実行した場合の出力例を示しています。

    Visual Studio for Mac で、アプリケーションを停止します。

    Xamarin.Forms アプリのライフサイクルの詳細については、「[Xamarin.Forms App Lifecycle](~/xamarin-forms/app-fundamentals/app-lifecycle.md)」 (Xamarin.Forms アプリのライフサイクル) を参照してください。
