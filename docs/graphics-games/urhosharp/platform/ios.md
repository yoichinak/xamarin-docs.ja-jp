---
title: UrhoSharp の iOS および tvOS のサポート
description: このドキュメントには、iOS がについて説明し、UrhoSharp の tvOS のサポートします。 これには、プロジェクトを作成、構成し起動 Urho、および Urho のカスタムの埋め込みを実行する方法について説明します。
ms.prod: xamarin
ms.assetid: 7B06567E-E789-4EA1-A2A9-F3B2212EDD23
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: f15ae458c6bd613b59700908ad7c121315e377ab
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61302643"
---
# <a name="urhosharp-ios-and-tvos-support"></a>UrhoSharp の iOS および tvOS のサポート

Urho は、ポータブル クラス ライブラリであり、ゲーム ロジックにさまざまなプラットフォーム全体で使用する同じ API を使用する必要があります、プラットフォーム固有のドライバーと、場合によっては、Urho を初期化、プラットフォーム固有の機能を活用するためにします.

以下のページにある`MyGame`がの sublcass、`Application`クラス。

## <a name="ios-and-tvos"></a>iOS と tvOS

**サポートされているアーキテクチャ:** armv7、arm64、i386

## <a name="creating-a-project"></a>Visual C++ プロジェクト

IOS プロジェクトを作成し、し、データ リソース ディレクトリを追加し、すべてのファイルがあるかどうかを確認**BundleResource**として、**ビルド アクション**します。

![セットアップ プロジェクト](ios-images/image-4.png "リソース ディレクトリにデータの追加")

## <a name="configuring-and-launching-urho"></a>構成および Urho を起動します。

ステートメントを使用して追加、`Urho`と`Urho.iOS`名前空間、Urho、初期化と、アプリケーションを起動するには、このコードを追加します。

```csharp
new MyGame().Run();
```

Ios ために注意`FinishedLaunching`を完了するへの呼び出しのキューを配置する必要があります`Run()`メソッドの完了後を実行する一般的な表現形式になります。

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

既定の iOS PNG オプティマイザーが Urho が適切では現在利用できるいないイメージを生成するため PNG の最適化を無効にすることが重要します。

## <a name="custom-embedding-of-urho"></a>Urho のカスタムの埋め込み

ことができますまたはに Urho 引き継ぎ、アプリケーション全体の画面を作成することができます、アプリケーションのコンポーネントとして使用する、`UrhoSurface`これは、`UIView`を既存のアプリケーションに埋め込むことができます。

これは、何が行う必要があります。

```csharp
var view = new UrhoSurface () {
    Frame = new CGRect (100,100,200,200),
    BackgroundColor = UIColor.Red
}
window.AddSubview (view);
```

これは、ため、Urho クラスをホストするし、操作を行います。

```csharp
new MyGame().Run ();
```

