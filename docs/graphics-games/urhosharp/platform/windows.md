---
title: UrhoSharp Windows のサポート
description: このドキュメントでは、UrhoSharp 用 Windows のサポートについて説明します。 プロジェクトを作成、構成する方法を説明し、および Urho を起動して、WPF との統合、UWP と統合します。
ms.prod: xamarin
ms.assetid: A4F36014-AE4E-4F07-A1AC-F264AAA68ACF
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 094eaf0ebe84ce8c1771bd6481ee897463349856
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783234"
---
# <a name="urhosharp-windows-support"></a>UrhoSharp Windows のサポート

Urho、ポータブル クラス ライブラリで、さまざまなプラットフォーム全体にわたる、ゲーム ロジックに使用する同じ API を使用する必要があります、プラットフォーム固有のドライバーと、場合によっては、Urho を初期化中には、特定のプラットフォーム機能を活用するためにはたいです.

次のページであると想定`MyGame`のサブクラスは、`Application`クラスです。

**サポートされているアーキテクチャ:** 64 ビット Windows のみです。

これを使用する方法を示す完全な例を確認できます、[サンプル](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples)

## <a name="standalone-project"></a>スタンドアロン プロジェクト

### <a name="creating-a-project"></a>Visual C++ プロジェクト

コンソール プロジェクトを作成、Urho NuGet の参照し、資産 (データ ディレクトリを含んでいるディレクトリ) を検索できることを確認します。

### <a name="configuring-and-launching-urho"></a>構成および Urho を起動します。

アプリケーションを起動するには、これの操作を行います。

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

### <a name="example"></a>例

[完全な例](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Desktop)

## <a name="integrated-with-wpf"></a>WPF と統合

### <a name="creating-a-project"></a>Visual C++ プロジェクト

WPF プロジェクトを作成、Urho NuGet の参照し、資産 (データ ディレクトリを含んでいるディレクトリ) を検索できることを確認します。

### <a name="configuring-and-launching-urho-from-wpf"></a>構成および Urho WPF からを起動します。

サブクラスを作成`Window`し、次のように、資産の構成します。

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

## <a name="integrated-with-uwp"></a>UWP と統合

### <a name="creating-a-project"></a>Visual C++ プロジェクト

UWP プロジェクトを作成、Urho NuGet の参照し、資産 (データ ディレクトリを含んでいるディレクトリ) を検索できることを確認します。

### <a name="configuring-and-launching-urho-from-uwp"></a>構成および Urho UWP からを起動します。

サブクラスを作成`Window`し、次のように、資産の構成します。

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

## <a name="integrated-with-windowsforms"></a>Windows.Forms と統合

### <a name="creating-a-project"></a>Visual C++ プロジェクト

Windows.Forms プロジェクトを作成し、Urho NuGet の参照を特定できること、資産 (データ ディレクトリを含んでいるディレクトリ) ことを確認します。

### <a name="configuring-and-launching-urho-from-windowsforms"></a>構成および Urho Windows.Forms からを起動します。

フォームから Urho を起動してを参照してください[完全なサンプル](https://github.com/xamarin/urho-samples/blob/master/FeatureSamples/WinForms/SamplesForm.cs)
