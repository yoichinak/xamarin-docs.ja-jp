---
title: "UrhoSharp Mac サポート"
description: "Mac 固有のセットアップと UrhoSharp の機能です。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.openlocfilehash: fff96a19d5f5286f2c9483407fcaaab6d15ff2b5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="urhosharp-mac-support"></a>UrhoSharp Mac サポート

_Mac 固有のセットアップと機能_

Urho、ポータブル クラス ライブラリで、さまざまなプラットフォーム全体にわたる、ゲーム ロジックに使用する同じ API を使用する必要があります、プラットフォーム固有のドライバーと、場合によっては、Urho を初期化中には、特定のプラットフォーム機能を活用するためにはたいです.

次のページであると想定`MyGame`のサブクラスは、`Application`クラスです。

## <a name="macos"></a>macOS

**サポートされているアーキテクチャ:** 32 ビットと 64 ビットの x86/x86-64 します。

## <a name="creating-a-project"></a>Visual C++ プロジェクト

コンソール プロジェクトを作成、Urho NuGet の参照し、資産 (データ ディレクトリを含んでいるディレクトリ) を検索できることを確認します。

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>例

[完全な例](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


