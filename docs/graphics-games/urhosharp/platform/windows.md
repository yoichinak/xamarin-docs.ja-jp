---
title: UrhoSharp の Windows のサポート
description: このドキュメントでは、Windows UrhoSharp のサポートについて説明します。 これには、プロジェクトを作成、構成、Urho を起動および wpf では、統合、および UWP と統合する方法について説明します。
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 46a028da577a4c49e18cccb681351d7614bb196b
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832644"
---
# <a name="urhosharp-windows-support"></a>UrhoSharp の Windows のサポート

Urho は、ポータブル クラス ライブラリであり、ゲーム ロジックにさまざまなプラットフォーム全体で使用する同じ API を使用する必要があります、プラットフォーム固有のドライバーと、場合によっては、Urho を初期化、プラットフォーム固有の機能を活用するためにします.

以下のページにある`MyGame`のサブクラスには、`Application`クラス。

**サポートされているアーキテクチャ:** Windows 64 ビットのみです。

これを使用する方法を示す完全な例を参照できます、[サンプル](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

## <a name="standalone-project"></a>スタンドアロン プロジェクト

### <a name="creating-a-project"></a>プロジェクトの作成

コンソール プロジェクトを作成、Urho NuGet の参照、および資産 (データ ディレクトリを含むディレクトリ) を特定できることを確認します。

### <a name="configuring-and-launching-urho"></a>構成および Urho を起動します。

アプリケーションを起動するには、これを実行します。

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

### <a name="example"></a>例

[完全な例](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

## <a name="integrated-with-wpf"></a>WPF と統合

### <a name="creating-a-project"></a>プロジェクトの作成

WPF プロジェクトを作成、Urho NuGet の参照し、資産 (データ ディレクトリを含むディレクトリ) を特定できることを確認します。

### <a name="configuring-and-launching-urho-from-wpf"></a>構成および Urho WPF からを起動します。

サブクラスを作成`Window`し、次のように、資産を構成します。

```csharp
    public partial class MainWindow : Window
    {
        Application currentApplication;

        public MainWindow()
        {
            InitializeComponent();
            DesktopUrhoInitializer.AssetsDirectory = @"../../Assets";
            Loaded += (s,e) => RunGame (new MyGame ());
        }

        async void RunGame(MyGame game)
        {
            var urhoSurface = new Panel { Dock = DockStyle.Fill };
            WindowsFormsHost.Child = urhoSurface;
            WindowsFormsHost.Focus();
            urhoSurface.Focus();
            await Task.Yield();
            var appOptions = new ApplicationOptions(assetsFolder: "Data")
                {
                    ExternalWindow = RunInSdlWindow.IsChecked.Value ? IntPtr.Zero : urhoSurface.Handle,
                    LimitFps = false, //true means "limit to 200fps"
                };
            currentApplication = Urho.Application.CreateInstance(value.Type, appOptions);
            currentApplication.Run();
        }
    }
```

### <a name="example"></a>例

[完全な例](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/WPF)

## <a name="integrated-with-uwp"></a>UWP 統合

### <a name="creating-a-project"></a>プロジェクトの作成

UWP プロジェクトを作成、Urho NuGet の参照、および資産 (データ ディレクトリを含むディレクトリ) を特定できることを確認します。

### <a name="configuring-and-launching-urho-from-uwp"></a>構成および Urho UWP からを起動します。

サブクラスを作成`Window`し、次のように、資産を構成します。

```csharp
    {
        InitializeComponent();
        GameTypes = typeof(Sample).GetTypeInfo().Assembly.GetTypes()
            .Where(t => t.GetTypeInfo().IsSubclassOf(typeof(Application)) && t != typeof(Sample))
            .Select((t, i) => new TypeInfo(t, $"{i + 1}. {t.Name}", ""))
            .ToArray();
        DataContext = this;
        Loaded += (s, e) => RunGame (new MyGame ());
    }

    public void RunGame(TypeInfo value)
    {
        //at this moment, UWP supports assets only in pak files (see PackageTool)
        currentApplication = UrhoSurface.Run(value.Type, "Data.pak");
    }
}
```

### <a name="example"></a>例

[完全な例](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/UWP)

## <a name="integrated-with-windows-forms"></a>Windows フォームの統合

### <a name="creating-a-project"></a>プロジェクトの作成

Windows フォーム プロジェクトを作成、Urho NuGet の参照、および資産 (データ ディレクトリを含むディレクトリ) を特定できることを確認します。

### <a name="configuring-and-launching-urho-from-windowsforms"></a>構成および Urho Windows.Forms からを起動します。

フォームから Urho を起動してを参照してください[完全なサンプル](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)
