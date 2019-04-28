---
title: UrhoSharp の Mac のサポート
description: このドキュメントでは、UrhoSharp の macOS のサポートについて説明します。 プロジェクトを作成する方法について説明し、いくつかのサンプル コードへのリンクを提供します。
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 6d0a048020284319682c1bee0f9a1d7f9af00977
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61386365"
---
# <a name="urhosharp-mac-support"></a>UrhoSharp の Mac のサポート

_Mac の具体的な設定と機能_

Urho は、ポータブル クラス ライブラリであり、ゲーム ロジックにさまざまなプラットフォーム全体で使用する同じ API を使用する必要があります、プラットフォーム固有のドライバーと、場合によっては、Urho を初期化、プラットフォーム固有の機能を活用するためにします.

以下のページにある`MyGame`のサブクラスには、`Application`クラス。

## <a name="macos"></a>macOS

**サポートされているアーキテクチャ:** x86/x86-64 の 32 ビットと 64 ビット。

## <a name="creating-a-project"></a>Visual C++ プロジェクト

コンソール プロジェクトを作成、Urho NuGet の参照、および資産 (データ ディレクトリを含むディレクトリ) を特定できることを確認します。

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>例

[完全な例](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)


