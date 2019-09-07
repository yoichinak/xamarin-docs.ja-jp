---
title: StructsAndEnums ファイル & ApiDefinitions
description: このドキュメントでは、目標マジックペンによって生成される ApiDefinitions.cs ファイルと StructsAndEnums.cs ファイルについて説明します。 これらのファイルは、からのC#目的 C コードにアクセスするために使用されます。
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: conceptdev
ms.author: crdun
ms.date: 03/29/2017
ms.openlocfilehash: 35ae1ba49ae774b21a8f629beb9144ccf3a5f170
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765691"
---
# <a name="apidefinitions--structsandenums-files"></a>StructsAndEnums ファイル & ApiDefinitions

目標マジックペンが正常に実行されると`Binding/ApiDefinitions.cs` 、 `Binding/StructsAndEnums.cs`とファイルが生成されます。
これら2つのファイルは Visual Studio for Mac のバインドプロジェクトに追加されるか、 `btouch`また`bmac`はツールに直接渡されて最終的なバインドが生成されます。

これらの生成されたファイルが必要*な場合も*ありますが、開発者は、ツールで自動的に処理できなかった問題を修正するために、これらの生成されたファイルを手動で変更する必要があります (たとえば、 [ `Verify`属性](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md))。

次の手順には、次のようなものがあります。

- **名前の調整**:場合によっては、.NET Framework のデザインガイドラインに合わせてメソッドとクラスの名前を調整する必要があります。
- **メソッドまたはプロパティ**:目的のマジックペンによって使用されるヒューリスティックによって、プロパティに変換するメソッドが選択される場合があります。 この時点で、これが意図した動作であるかどうかを判断できます。
- **イベントのフック**:クラスをデリゲートクラスにリンクし、それらのクラスのイベントを自動的に生成できます。
- **通知をフック**する:純粋なヘッダーファイルから通知の API コントラクトを抽出することはできません。これにより、API ドキュメントへのトリップが必要になります。 厳密に型指定された通知が必要な場合は、結果を更新する必要があります。
- **API**の実行:この時点で、追加のコンストラクターを提供し、メソッドを追加して (初期C#構築構文を使用できるようにする)、演算子をオーバーロードし、追加の定義ファイルに独自のインターフェイスを実装することができます。

次の図に示すように、API の説明を[バインド](~/cross-platform/macios/binding/objective-c-libraries.md)して、これらのファイルがバインドプロセスにどのように適合するかを確認します。

![](apidefinitions-structsandenums-images/binding-flowchart.png "バインドプロセスを次の図に示します。")

これらのファイルの内容の詳細については、「[バインディングの型のリファレンス](~/cross-platform/macios/binding/binding-types-reference.md)」を参照してください。
