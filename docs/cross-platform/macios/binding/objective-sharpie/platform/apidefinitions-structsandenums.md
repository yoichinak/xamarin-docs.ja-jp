---
title: StructsAndEnums ファイル & ApiDefinitions
description: このドキュメントでは、目標マジックペンによって生成される ApiDefinitions.cs ファイルと StructsAndEnums.cs ファイルについて説明します。 これらのファイルは、C# から目的の C コードにアクセスするために使用されます。
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 8d4a05745d8d2ec6e05abd519aef4b9827655e06
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930430"
---
# <a name="apidefinitions--structsandenums-files"></a>StructsAndEnums ファイル & ApiDefinitions

目標マジックペンが正常に実行されると、とファイルが生成さ `Binding/ApiDefinitions.cs` `Binding/StructsAndEnums.cs` れます。
これら2つのファイルは Visual Studio for Mac のバインドプロジェクトに追加されるか、 `btouch` またはツールに直接渡され `bmac` て最終的なバインドが生成されます。

これらの生成されたファイルが必要*な場合も*ありますが、多くの場合、開発者は、このように生成されたファイルを手動で変更して、ツールによって自動的に処理されなかった問題 ( [ `Verify` 属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)でフラグが設定されているなど) を修正する必要があります。

次の手順には、次のようなものがあります。

- **名前の調整**: .NET Framework のデザインガイドラインに合わせてメソッドとクラスの名前を調整することが必要になる場合があります。
- **メソッドまたはプロパティ**: 目的のマジックペンによって使用されるヒューリスティックによって、プロパティに変換するメソッドが選択されることがあります。 この時点で、これが意図した動作であるかどうかを判断できます。
- **イベントのフック**: クラスをデリゲートクラスにリンクし、それらのイベントを自動的に生成することができます。
- **通知をフック**する: 純粋なヘッダーファイルから通知の api コントラクトを抽出することはできません。これを行うには、api ドキュメントへのトリップが必要になります。 厳密に型指定された通知が必要な場合は、結果を更新する必要があります。
- **API**の機能: この時点では、追加のコンストラクターを提供し、メソッドを追加し (C# の初期化の構文を使用できるようにするため)、演算子をオーバーロードし、追加の定義ファイルに独自のインターフェイスを実装することができます。

次の図に示すように、API の説明を[バインド](~/cross-platform/macios/binding/objective-c-libraries.md)して、これらのファイルがバインドプロセスにどのように適合するかを確認します。

![バインドプロセスを次の図に示します。](apidefinitions-structsandenums-images/binding-flowchart.png)

これらのファイルの内容の詳細については、「[バインディングの型のリファレンス](~/cross-platform/macios/binding/binding-types-reference.md)」を参照してください。
