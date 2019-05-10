---
title: ApiDefinitions と StructsAndEnums ファイル
description: このドキュメントでは、目標油性を生成する ApiDefinitions.cs および StructsAndEnums.cs ファイルについて説明します。 これらのファイルがから OBJECTIVE-C コードへのアクセスに使用し、C#します。
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: 3b991f6105c6053f473b049d195aaef63cbcdd57
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977676"
---
# <a name="apidefinitions--structsandenums-files"></a>ApiDefinitions と StructsAndEnums ファイル

目的の油性が正常に実行されたらと、が生成されます`Binding/ApiDefinitions.cs`と`Binding/StructsAndEnums.cs`ファイル。
これら 2 つのファイルを Mac に Visual Studio でのバインド プロジェクトに追加またはに直接渡される、`btouch`または`bmac`最終的なバインドを生成するツール。

*一部*これら生成されたファイルがあります、必要な (など、フラグが設定されたツールによって自動的に処理できなかった問題を修正するファイルを生成、開発者はこれらを手動で変更する必要があります多くの場合、詳細がの場合[ `Verify`属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md))。

次の手順のとおりです。

- **名前を調整する**:場合がありますメソッドと、.NET Framework デザイン ガイドラインに一致するようにクラスの名前を調整するされます。
- **メソッドまたはプロパティ**:目標油性でも使用するヒューリスティックでは、プロパティに有効にする方法を選択します。 この時点では、これが意図した動作かどうかを決定できます。
- **イベントをフックする**:リンク、クラス、デリゲート クラスを使用して自動的にこれらのイベントを生成します。
- **通知フック**:純粋なヘッダー ファイルからの通知 API コントラクトを抽出することはできません、API のドキュメントへのトリップが必要になります。 厳密に型指定された通知を設定する場合は、結果を更新する必要があります。
- **API キュレーション**:この時点では、追加のコンス トラクターを提供するメソッドを追加できます (を許可するC#初期化の構築での構文)、演算子のオーバー ロードと、追加の定義ファイルに独自のインターフェイスを実装します。

参照してください、[バインド API](~/cross-platform/macios/binding/objective-c-libraries.md)説明を次の図に示すように、これらのファイルが、バインディング プロセスに適合させる方法を確認します。

![](apidefinitions-structsandenums-images/binding-flowchart.png "バインディング プロセスは、この図には")

参照してください、[型参照をバインド](~/cross-platform/macios/binding/binding-types-reference.md)のこれらのファイルの内容の詳細について。

