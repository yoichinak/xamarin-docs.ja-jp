---
title: UrhoSharp iOS および tvOS のサポート
description: iOS および tvOS 特定のセットアップと UrhoSharp に対して機能します。
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 57dbd1e9f65361c1bcc8fe3959cded72922c3496
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp iOS および tvOS のサポート

_iOS および tvOS と機能の特定のセットアップ_

Urho、ポータブル クラス ライブラリで、さまざまなプラットフォーム全体にわたる、ゲーム ロジックに使用する同じ API を使用する必要があります、プラットフォーム固有のドライバーと、場合によっては、Urho を初期化中には、特定のプラットフォーム機能を活用するためにはたいです.

次のページであると想定`MyGame`の sublcass は、`Application`クラスです。

## <a name="ios-and-tvos"></a>iOS および tvOS

**サポートされているアーキテクチャ:** armv7、arm64、i386

## <a name="creating-a-project"></a>Visual C++ プロジェクト

IOS プロジェクトを作成し、リソース ディレクトリにデータを追加し、すべてのファイルがあるかどうかを確認**BundleResource**として、**ビルド アクション**です。

![プロジェクトのセットアップ](ios-images/image-4.png "リソース ディレクトリにデータの追加")

## <a name="configuring-and-launching-urho"></a>構成および Urho を起動します。

ステートメントを使用して追加、`Urho`と`Urho.iOS`名前空間、Urho、初期化中だけでなく、アプリケーションを起動するのには、このコードを追加します。

```csharp
new MyGame().Run();
```

Ios のでことに注意して`FinishedLaunching`を完了する必要がありますキューに配置し、呼び出し`Run()`実行するには、メソッドが完了した後、これは、共通の表現形式。

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    LaunchGame();
    return true;
}

async void LaunchGame()
{
    await Task.Yield();
    new SamplyGame().Run();
}
```

既定の iOS PNG オプティマイザーが Urho いない現在正しく使用することがイメージを生成するため PNG の最適化を無効にすることが重要であります。

## <a name="custom-embedding-of-urho"></a>カスタムの埋め込み Urho の

引き継ぐことができる別の方法としてを持つように Urho アプリケーション全体 画面とこれを使用するアプリケーションのコンポーネントとして作成することができます、`UrhoSurface`これは、`UIView`既存のアプリケーションに埋め込むことができます。

新機能する必要がありますを行うには次に示します。

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

これは、ため、Urho クラスをホストする、タスクは実行します。

```csharp
new MyGame().Run ();
```

