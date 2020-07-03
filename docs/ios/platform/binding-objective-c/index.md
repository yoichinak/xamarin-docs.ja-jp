---
title: IOS ライブラリのバインド
description: このドキュメントでは、C 言語のコードに対して C# バインドを作成し、Xamarin. iOS アプリケーションでネイティブライブラリを使用したり、Ds を作成したりできるようにする方法について説明します。
ms.prod: xamarin
ms.assetid: EBDC50DC-B44B-4003-AB2B-1EEB868A5E01
ms.technology: xamarin-ios
ms.custom: xamu-video
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 1e0342a41587b7479381ad763790227aba2ef414
ms.sourcegitcommit: a3f13a216fab4fc20a9adf343895b9d6a54634a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/02/2020
ms.locfileid: "85853091"
---
# <a name="binding-ios-libraries"></a>IOS ライブラリのバインド

> [!IMPORTANT]
> 現在、Xamarin プラットフォームでのカスタムバインディングの使用を調査しています。 今後の開発作業については、[**この調査**](https://www.surveymonkey.com/r/KKBHNLT)にご連絡ください。

次のリンクを使用して、ターゲット C ライブラリのバインドと Xamarin 用の Cocoアポストロフィのバインドについて学習します。

- [**概要**](~/cross-platform/macios/binding/overview.md) -
  バインディングのしくみについて説明します。
- [**バインディングの目的 C ライブラリ**](~/cross-platform/macios/binding/objective-c-libraries.md) -
  Xamarin プロジェクトで使用する目的の C ライブラリをバインドする方法について説明します。
- [**型定義のリファレンスガイド**](~/cross-platform/macios/binding/binding-types-reference.md) -
  バインディングの生成プロセスを実行するために作成者が使用できるすべての属性について説明します。

## <a name="objective-sharpie"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

目標マジックペンは、バインドの最初のパスをブートストラップするためのコマンドラインツールです。
これは、ネイティブライブラリのヘッダーファイルを解析して、パブリック API を[バインディング定義](~/cross-platform/macios/binding/objective-c-libraries.md)にマップすることによって機能します (それ以外で手動で実行するプロセス)。 目標マジックペンは、それ自体ではバインドを作成しませんが、作業を開始するのに役立ちます。

目標マジックペン3.0 では、Cocoアポストロフィを直接バインドする機能が導入されました。

## <a name="walkthrough---binding-an-ios-objective-c-library"></a>[チュートリアル-iOS の目的 C ライブラリのバインド](walkthrough.md)

このページでは、オープンソースの[**Infcolorpicker**](https://github.com/InfinitApps/InfColorPicker)目標 C プロジェクトを例として使用して、iOS バインドプロジェクトを作成する手順について説明します。 **Infcolorpicker**ライブラリには、ユーザーが HSB 表現に基づいて色を選択できるようにする再利用可能なビューコントローラーが用意されており、色の選択がよりわかりやすくなっています。
目標マジックペンは、バインディングプロセスを支援するために使用されます。

## <a name="video"></a>ビデオ

> [!VIDEO https://youtube.com/embed/ZUoPLcmnf1o]

**C/c + + での iOS のバインドに関するビデオ**

## <a name="related-links"></a>関連リンク

- [Objective-C のバインディング](~/cross-platform/macios/binding/index.md)
- [Mac バインド](~/mac/platform/binding.md)
