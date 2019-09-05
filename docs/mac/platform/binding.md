---
title: Xamarin. Mac 用の Mac ライブラリのバインド
description: このドキュメントでは、マジックペンとサンプルコードを含む、Xamarin. Mac アプリケーションでの C のバインドの使用方法について説明しているガイドにリンクしています。
ms.prod: xamarin
ms.assetid: 521707CD-79D3-488A-84CB-A37EBF93AC94
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 01/13/2017
ms.openlocfilehash: 59ac5a4f9949f1e65e67b9629c43ddb4b822bf43
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290061"
---
# <a name="binding-mac-libraries-for-xamarinmac"></a>Xamarin. Mac 用の Mac ライブラリのバインド

Xamarin. Mac での目的 C ライブラリのバインドについては、次のリンク先を参照してください。

- [**概要**](~/cross-platform/macios/binding/overview.md)-
  バインディングのしくみについて説明します。
- [**バインディングの目的 C ライブラリ**](~/cross-platform/macios/binding/objective-c-libraries.md)-
  Xamarin プロジェクトで使用する目的の C ライブラリをバインドする方法について説明します。
- [**型定義のリファレンスガイド**](~/cross-platform/macios/binding/binding-types-reference.md)-
  バインディングの生成プロセスを実行するために作成者が使用できるすべての属性について説明します。

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

目標マジックペンは、バインディングの最初のパスをブートストラップするためのコマンドラインツールです。
これは、ネイティブライブラリのヘッダーファイルを解析して、パブリック API を[バインディング定義](~/cross-platform/macios/binding/binding-types-reference.md)にマップすることによって機能します (それ以外で手動で実行するプロセス)。 目標マジックペンは、それ自体ではバインドを作成しませんが、作業を開始するのに役立ちます。

## <a name="examples"></a>使用例

バインドプロジェクトを使用して Mac バインドを作成する方法については、 [Xmbindingexample mac サンプル](https://github.com/xamarin/mac-samples/tree/master/XMBindingExample)を参照してください。

## <a name="related-links"></a>関連リンク

- [Objective-C のバインド](~/cross-platform/macios/binding/index.md)
- [IOS ライブラリのバインド](~/ios/platform/binding-objective-c/index.md)
