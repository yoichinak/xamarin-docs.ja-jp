---
title: UrhoSharp Android サポート
description: このドキュメントでは、Android 固有のセットアップと UrhoSharp の機能に関連する情報について説明します。 具体的には、サポートされているアーキテクチャについて説明します Urho、およびカスタムの埋め込み Urho の起動の構成と、プロジェクトを作成する方法。
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: e7371fa85fd5955e9a0fd285adb32844001821b3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105436"
---
# <a name="urhosharp-android-support"></a>UrhoSharp Android サポート

_特定の android のセットアップと機能_

Urho は、ポータブル クラス ライブラリであり、ゲーム ロジックにさまざまなプラットフォーム全体で使用する同じ API を使用する必要があります、プラットフォーム固有のドライバーと、場合によっては、Urho を初期化、プラットフォーム固有の機能を活用するためにします.

以下のページにある`MyGame`のサブクラスには、`Application`クラス。

## <a name="architectures"></a>アーキテクチャ

**サポートされているアーキテクチャ**: x86、armeabi、armeabi v7a

## <a name="create-a-project"></a>プロジェクトの作成

Android プロジェクトを作成し、UrhoSharp の NuGet パッケージを追加します。

アセットを格納しているデータを追加、**資産**すべてのファイルがあるディレクトリを確認**AndroidAsset**として、**ビルド アクション**します。

![セットアップ プロジェクト](android-images/image-3.png "Assets ディレクトリに資産を格納しているデータの追加")

## <a name="configure-and-launching-urho"></a>構成し、Urho を起動します。

追加のステートメントを使用して、`Urho`と`Urho.Android`名前空間、Urho、初期化と、アプリケーションを起動するには、このコードを追加します。

MyGame クラスに実装されているゲームを実行する最も簡単な方法を呼び出すことです。

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

コンテンツとしてゲームと全画面表示のアクティビティが表示されます。

## <a name="custom-embedding-of-urho"></a>Urho のカスタムの埋め込み

ことができますまたはに Urho 引き継ぎ、アプリケーション全体の画面を作成することができます、アプリケーションのコンポーネントとして使用する、`SurfaceView`経由。

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

いくつかのイベントをアクティビティに転送 UrhoSharp、する必要がありますも例。

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

同じ処理を実行する必要があります: `OnResume`、 `OnPause`、 `OnLowMemory`、 `OnDestroy`、`DispatchKeyEvent`と`OnWindowFocusChanged`します。

これは、ゲームを起動する一般的なアクティビティを示しています。

```csharp
[Activity(Label = "MyUrhoApp", MainLauncher = true,
    Icon = "@drawable/icon", Theme = "@android:style/Theme.NoTitleBar.Fullscreen",
    ConfigurationChanges = ConfigChanges.KeyboardHidden | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Portrait)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        var mLayout = new AbsoluteLayout(this);
        var surface = UrhoSurface.CreateSurface<MyUrhoApp>(this);
        mLayout.AddView(surface);
        SetContentView(mLayout);
    }

    protected override void OnResume()
    {
        UrhoSurface.OnResume();
        base.OnResume();
    }

    protected override void OnPause()
    {
        UrhoSurface.OnPause();
        base.OnPause();
    }

    public override void OnLowMemory()
    {
        UrhoSurface.OnLowMemory();
        base.OnLowMemory();
    }

    protected override void OnDestroy()
    {
        UrhoSurface.OnDestroy();
        base.OnDestroy();
    }

    public override bool DispatchKeyEvent(KeyEvent e)
    {
        if (!UrhoSurface.DispatchKeyEvent(e))
            return false;
        return base.DispatchKeyEvent(e);
    }

    public override void OnWindowFocusChanged(bool hasFocus)
    {
        UrhoSurface.OnWindowFocusChanged(hasFocus);
        base.OnWindowFocusChanged(hasFocus);
    }
}
```

