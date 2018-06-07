---
title: UrhoSharp Android のサポート
description: このドキュメントでは、Android 固有のセットアップや UrhoSharp 機能に関する情報について説明します。 具体的には、サポートされているアーキテクチャでは、についても説明を構成および起動 Urho、およびカスタムの埋め込み Urho のプロジェクトを作成する方法です。
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 6e489f52712989b5f94fa52d5ec6f22a13ce6252
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783783"
---
# <a name="urhosharp-android-support"></a>UrhoSharp Android のサポート

_特定の android のセットアップと機能_

Urho、ポータブル クラス ライブラリで、さまざまなプラットフォーム全体にわたる、ゲーム ロジックに使用する同じ API を使用する必要があります、プラットフォーム固有のドライバーと、場合によっては、Urho を初期化中には、特定のプラットフォーム機能を活用するためにはたいです.

次のページであると想定`MyGame`のサブクラスは、`Application`クラスです。

## <a name="architectures"></a>アーキテクチャ

**サポートされているアーキテクチャ**: x86、armeabi、armeabi v7a

## <a name="create-a-project"></a>プロジェクトの作成

Android プロジェクトを作成し、UrhoSharp NuGet パッケージを追加します。

資産に含まれているデータを追加、**資産**ディレクトリと必ずすべてのファイルがある**AndroidAsset**として、**ビルド アクション**です。

![プロジェクトのセットアップ](android-images/image-3.png "資産ディレクトリに資産を含むデータの追加")

## <a name="configure-and-launching-urho"></a>構成および Urho を起動します。

追加のステートメントを使用して、`Urho`と`Urho.Android`名前空間、Urho、初期化中だけでなく、アプリケーションを起動するのには、このコードを追加します。

MyGame クラスに実装されているゲームを実行する最も簡単な方法を呼び出すことです。

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

ゲームと、コンテンツとしてフルスクリーン アクティビティが開きます。

## <a name="custom-embedding-of-urho"></a>カスタムの埋め込み Urho の

引き継ぐことができる別の方法としてを持つように Urho アプリケーション全体 画面とこれを使用するアプリケーションのコンポーネントとして作成することができます、`SurfaceView`経由。

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

同じ操作を実行する必要があります: `OnResume`、 `OnPause`、 `OnLowMemory`、 `OnDestroy`、`DispatchKeyEvent`と`OnWindowFocusChanged`です。

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

