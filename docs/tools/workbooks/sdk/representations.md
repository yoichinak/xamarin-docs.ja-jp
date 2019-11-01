---
title: Xamarin Workbooks での表示
description: このドキュメントでは、Xamarin Workbooks 表現パイプラインについて説明します。これにより、値を返すコードの豊富な結果を表示できます。
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: davidortinau
ms.author: daortin
ms.date: 03/30/2017
ms.openlocfilehash: c1afba6943faa03ee07a3ec70624f668748041cb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73018590"
---
# <a name="representations-in-xamarin-workbooks"></a>Xamarin Workbooks での表示

## <a name="representations"></a>表記

ブックまたはインスペクターセッション内では、実行されるコードが実行され、結果 (値を返すメソッドや式の結果など) が生成されます。 整数などのプリミティブを除き、すべてのオブジェクトは、対話的なメンバーグラフを生成するために反映され、クライアントがより高度にレンダリングできる代替表現を提供するプロセスを実行します。 遅延および対話的なリフレクションとリモート処理により、任意のサイズと深さのオブジェクトが安全にサポートされます (サイクルと無限列挙体を含む)。

Xamarin Workbooks には、すべてのエージェントとクライアントに共通するいくつかの種類があり、結果をリッチに表示できます。 このような型の一例として、たとえば、iOS では、`CGColor` または `UIColor` オブジェクトを `Xamarin.Interactive.Representations.Color` オブジェクトに変換するエージェントが `Color` ます。

統合 SDK は、共通表現に加えて、エージェント内のカスタム表現をシリアル化し、クライアントで表現を表示するための Api を提供します。

## <a name="external-representations"></a>外部表現

`Xamarin.Interactive.IAgent.RepresentationManager` には、`RepresentationProvider`を登録する機能が用意されています。これは、任意のオブジェクトから非依存の形式に変換して表示するために、統合で実装する必要があります。 これらの非依存フォームは、`ISerializableObject` インターフェイスを実装する必要があります。

`ISerializableObject` インターフェイスを実装すると、オブジェクトをシリアル化する方法を厳密に制御する Serialize メソッドが追加されます。 `Serialize` メソッドでは、シリアル化するプロパティと最終的な名前は、開発者が厳密に指定する必要があります。 [`KitchenSink` サンプル] [サンプル] の `Person` オブジェクトを見ると、そのしくみを確認できます。

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

元のオブジェクトのスーパーセットまたはプロパティのサブセットを提供する場合は、`Serialize`を使用します。 たとえば、次のようにして、`Person`で事前に計算された `Age` プロパティを提供します。

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; set; }
  public DateTime DateOfBirth { get; set; }

  // <snip>

  void ISerializableObject.Serialize (ObjectSerializer serializer)
  {
    serializer.Property (nameof (Name), Name);
    serializer.Property (nameof (DateOfBirth), DateOfBirth);

    // Let's pre-compute an Age property that's the person's age in years,
    // so we don't have to compute it in the renderer.
    var age = (DateTime.MinValue + (DateTime.Now - DateOfBirth)).Year - 1;
    serializer.Property ("Age", age)
  }
}
```

> [!NOTE]
> `ISerializableObject` オブジェクトを直接生成する Api は、`RepresentationProvider`によって処理する必要はありません。 表示するオブジェクトが `ISerializableObject`でない場合は、`RepresentationProvider`でのラップを処理する必要が**あり**ます。

### <a name="rendering-a-representation"></a>表現のレンダリング

レンダラーは JavaScript で実装され、`ISerializableObject`によって表されるオブジェクトの JavaScript バージョンにアクセスできます。 JavaScript のコピーには、.NET 型名を示す `$type` 文字列プロパティもあります。

クライアント統合コードに TypeScript を使用することをお勧めします。これは、もちろんバニラ JavaScript にコンパイルされます。 どちらの方法でも、SDK では、TypeScript によって直接参照できる、または単にバニラ JavaScript の記述が推奨されている場合は手動で参照できる、入力[を提供し][typings]ます。

レンダリングの主な統合ポイントは `xamarin.interactive.RendererRegistry`次のとおりです。

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

ここでは、`PersonRenderer` が `Renderer` インターフェイスを実装しています。 詳細については、「 [typings][typings]使用」を参照してください。

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
