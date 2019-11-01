---
title: IOS ライブラリのバインド
description: このドキュメントでは、目的C# C コードへのバインディングを作成する方法について説明します。これにより、ネイティブライブラリを使用して、Xamarin. iOS アプリケーションで Cocods を作成できるようになります。
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 61d1adfc997b34302f1f89f56653af906ca90135
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022224"
---
# <a name="binding-ios-libraries"></a>IOS ライブラリのバインド

次のリンクを使用して、ターゲット C ライブラリのバインドと Xamarin 用の Cocoアポストロフィのバインドについて学習します。

- [**概要**](~/cross-platform/macios/binding/overview.md)-
  バインドのしくみについて説明します。
- 「[**目標 c ライブラリのバインド**](~/cross-platform/macios/binding/objective-c-libraries.md)」では、Xamarin プロジェクトで使用する目的の c ライブラリをバインドする方法について -
  説明します。
- [**型定義のリファレンスガイド**](~/cross-platform/macios/binding/binding-types-reference.md)-
  バインド作成者がバインディング生成プロセスを駆動するために使用できるすべての属性について説明します。

## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

目標マジックペンは、バインドの最初のパスをブートストラップするためのコマンドラインツールです。
これは、ネイティブライブラリのヘッダーファイルを解析して、パブリック API を[バインディング定義](~/cross-platform/macios/binding/objective-c-libraries.md)にマップすることによって機能します (それ以外で手動で実行するプロセス)。 目標マジックペンは、それ自体ではバインドを作成しませんが、作業を開始するのに役立ちます。

目標マジックペン3.0 では、Cocoアポストロフィを直接バインドする機能が導入されました。

## <a name="walkthrough---binding-an-ios-objective-c-librarywalkthroughmd"></a>[チュートリアル-iOS の目的 C ライブラリのバインド](walkthrough.md)

このページでは、オープンソースの[**Infcolorpicker**](https://github.com/InfinitApps/InfColorPicker)目標 C プロジェクトを例として使用して、iOS バインドプロジェクトを作成する手順について説明します。 **Infcolorpicker**ライブラリには、ユーザーが HSB 表現に基づいて色を選択できるようにする再利用可能なビューコントローラーが用意されており、色の選択がよりわかりやすくなっています。
目標マジックペンは、バインディングプロセスを支援するために使用されます。

## <a name="video"></a>ビデオ

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**C/C++ビデオにおける IOS のバインド**

## <a name="related-links"></a>関連リンク

- [Objective-C のバインド](~/cross-platform/macios/binding/index.md)
- [Mac バインド](~/mac/platform/binding.md)
