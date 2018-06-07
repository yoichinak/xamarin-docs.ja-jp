---
title: UrhoSharp Mac サポート
description: このドキュメントでは、UrhoSharp macOS サポートについて説明します。 プロジェクトを作成する方法について説明し、いくつかのサンプル コードへのリンクを提供します。
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: aae7b09231ae0e8f88bb9435f50fadd2ff822c1a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783344"
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


