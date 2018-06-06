---
title: ApiDefinitions & StructsAndEnums ファイル
description: このドキュメントでは、目標ペンを使わずを生成する ApiDefinitions.cs および StructsAndEnums.cs ファイルについて説明します。 これらのファイルは、コードへのアクセス、OBJECTIVE-C c# から使用されます。
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3b991f6105c6053f473b049d195aaef63cbcdd57
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780894"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions & StructsAndEnums ファイル

目標ペンを使わずが正常に実行する場合は生成`Binding/ApiDefinitions.cs`と`Binding/StructsAndEnums.cs`ファイル。
これら 2 つのファイルがプロジェクトに追加、バインド Visual Studio for Mac またはに直接渡されます、`btouch`または`bmac`最後のバインドを作成するためのツールです。

*一部*これら生成されたファイルがあります、必要なすべてが、(そのフラグが設定されたなどのツールによって自動的に処理できなかった問題を修正するファイルを生成、開発者がこれらを手動で変更する必要があります頻度が高い場合[ `Verify`属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md))。

次の手順のとおりです。

- **名前を調整する**: メソッドと .NET Framework デザイン ガイドラインを照合するクラスの名前を調整したいが場合があります。
- **メソッドまたはプロパティ**: 目標ペンを使わずでも使用するヒューリスティックはプロパティに変換するメソッドを取得します。 この時点では、かどうかであるが意図した動作かを決定する可能性があります。
- **イベントをフック**: クラス、デリゲート クラスとをリンクし、それらのイベントを自動的に生成することができます。
- **通知フック**: 純粋なヘッダー ファイルからの通知 API コントラクトを抽出することはできません、API のドキュメントへのトリップが必要です。 厳密に型指定された通知する場合は、結果を更新する必要があります。
- **API キュレーション**: この時点に余分なコンス トラクターを提供、(c# の初期化の構築に構文の許可) をメソッド、演算子のオーバー ロード、実装を選択する可能性があります、余分な定義ファイルに独自のインターフェイスです。

参照してください、 [API をバインド](~/cross-platform/macios/binding/objective-c-libraries.md)の説明を次の図に示すように、バインディング プロセスにこれらのファイルがどのように適合するかを参照してください。

![](apidefinitions-structsandenums-images/binding-flowchart.png "バインディング プロセスは、このダイアグラムに表示されます。")

参照してください、[型参照をバインド](~/cross-platform/macios/binding/binding-types-reference.md)のこれらのファイルの内容に関する詳細情報。

