---
title: UrhoSharp の Mac のサポート
description: このドキュメントでは、UrhoSharp の macOS のサポートについて説明します。 プロジェクトを作成する方法について説明し、いくつかのサンプル コードへのリンクを提供します。
ms.prod: xamarin
ms.assetid: 95FFBD36-14E9-4C17-B1E8-9A04E81E824D
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: ee0a03d168b6e628893b18a27d73b46d3fa2fbc2
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67832656"
---
# <a name="urhosharp-mac-support"></a>UrhoSharp の Mac のサポート

_Mac の具体的な設定と機能_

Urho は、ポータブル クラス ライブラリであり、ゲーム ロジックにさまざまなプラットフォーム全体で使用する同じ API を使用する必要があります、プラットフォーム固有のドライバーと、場合によっては、Urho を初期化、プラットフォーム固有の機能を活用するためにします.

以下のページにある`MyGame`のサブクラスには、`Application`クラス。

## <a name="macos"></a>macOS

**サポートされているアーキテクチャ:** x86/x86-64 の 32 ビットと 64 ビット。

## <a name="creating-a-project"></a>プロジェクトの作成

コンソール プロジェクトを作成、Urho NuGet の参照、および資産 (データ ディレクトリを含むディレクトリ) を特定できることを確認します。

```csharp
DesktopUrhoInitializer.AssetsDirectory = "../Assets";
new MyGame().Run();
```

## <a name="example"></a>例

[完全な例](https://github.com/xamarin/urho-samples/tree/master/FeatureSamples/Cocoa)
