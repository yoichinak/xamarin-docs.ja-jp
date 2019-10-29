---
title: StructsAndEnums ファイル & ApiDefinitions
description: このドキュメントでは、目標マジックペンによって生成される ApiDefinitions.cs ファイルと StructsAndEnums.cs ファイルについて説明します。 これらのファイルは、からのC#目的 C コードにアクセスするために使用されます。
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 870733554f3f69b7a0cd9b35c1b89e24c62b264d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016196"
---
# <a name="apidefinitions--structsandenums-files"></a>StructsAndEnums ファイル & ApiDefinitions

目標マジックペンが正常に実行されると、`Binding/ApiDefinitions.cs` と `Binding/StructsAndEnums.cs` ファイルが生成されます。
これら2つのファイルは Visual Studio for Mac のバインドプロジェクトに追加されるか、`btouch` または `bmac` ツールに直接渡されて最終的なバインドが生成されます。

これらの生成されたファイルが必要*な場合も*ありますが、多くの場合、開発者は、ツールで自動的に処理されなかった問題を修正するために、これらの生成されたファイルを手動で変更する必要があります (たとえば、`Verify` でフラグが設定されている場合など)。 [属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md))。

次の手順には、次のようなものがあります。

- **名前の調整**: .NET Framework のデザインガイドラインに合わせてメソッドとクラスの名前を調整することが必要になる場合があります。
- **メソッドまたはプロパティ**: 目的のマジックペンによって使用されるヒューリスティックによって、プロパティに変換するメソッドが選択されることがあります。 この時点で、これが意図した動作であるかどうかを判断できます。
- **イベントのフック**: クラスをデリゲートクラスにリンクし、それらのイベントを自動的に生成することができます。
- **通知をフック**する: 純粋なヘッダーファイルから通知の api コントラクトを抽出することはできません。これを行うには、api ドキュメントへのトリップが必要になります。 厳密に型指定された通知が必要な場合は、結果を更新する必要があります。
- **API**の機能: この時点では、追加のコンストラクターを提供し、メソッドを追加して ( C#初期構築の構文を使用できるようにするために)、演算子をオーバーロードし、追加の定義ファイルに独自のインターフェイスを実装することができます。

次の図に示すように、API の説明を[バインド](~/cross-platform/macios/binding/objective-c-libraries.md)して、これらのファイルがバインドプロセスにどのように適合するかを確認します。

![](apidefinitions-structsandenums-images/binding-flowchart.png "The binding process is shown in this diagram")

これらのファイルの内容の詳細については、「[バインディングの型のリファレンス](~/cross-platform/macios/binding/binding-types-reference.md)」を参照してください。
