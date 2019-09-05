---
title: Xamarin Workbooks での表示
description: このドキュメントでは、Xamarin Workbooks 表現パイプラインについて説明します。これにより、値を返すコードの豊富な結果を表示できます。
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: dde4e6b9c4903ccb0f23d8df82f39ff68030850e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292825"
---
# <a name="representations-in-xamarin-workbooks"></a>Xamarin Workbooks での表示

## <a name="representations"></a>表記

ブックまたはインスペクターセッション内では、実行されるコードが実行され、結果 (値を返すメソッドや式の結果など) が生成されます。 整数などのプリミティブを除き、すべてのオブジェクトは、対話的なメンバーグラフを生成するために反映され、クライアントがより高度にレンダリングできる代替表現を提供するプロセスを実行します。 遅延および対話的なリフレクションとリモート処理により、任意のサイズと深さのオブジェクトが安全にサポートされます (サイクルと無限列挙体を含む)。

Xamarin Workbooks には、すべてのエージェントとクライアントに共通するいくつかの種類があり、結果をリッチに表示できます。 `Color`このような型の一例として、たとえば iOS の場合は、エージェントがオブジェクト`CGColor`また`UIColor`はオブジェクトを`Xamarin.Interactive.Representations.Color`オブジェクトに変換する必要があります。

統合 SDK は、共通表現に加えて、エージェント内のカスタム表現をシリアル化し、クライアントで表現を表示するための Api を提供します。

## <a name="external-representations"></a>外部表現

`Xamarin.Interactive.IAgent.RepresentationManager`任意のオブジェクトから非依存`RepresentationProvider`の形式に変換するために実装する必要のあるを登録する機能を提供します。 これらの非依存フォームは`ISerializableObject` 、インターフェイスを実装する必要があります。

インターフェイスを`ISerializableObject`実装すると、オブジェクトをシリアル化する方法を厳密に制御する Serialize メソッドが追加されます。 メソッド`Serialize`では、シリアル化するプロパティと最終的な名前は、開発者が厳密に指定する必要があります。 `Person` [`KitchenSink`サンプル] [サンプル] のオブジェクトを見ると、このしくみを確認できます。

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

元のオブジェクトのスーパーセットまたはプロパティのサブセットを提供する場合は、を使用`Serialize`してこれを行うことができます。 たとえば、次のようにして、事前に計算`Age`されたプロパティをに`Person`提供します。

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
> オブジェクトを直接`ISerializableObject`生成する api は、 `RepresentationProvider`によって処理される必要はありません。 表示するオブジェクトがで**ない** `ISerializableObject`場合は、の`RepresentationProvider`ラップを処理する必要があります。

### <a name="rendering-a-representation"></a>表現のレンダリング

レンダラーは JavaScript で実装され、によっ`ISerializableObject`て表されるオブジェクトの javascript バージョンにアクセスできます。 JavaScript のコピーには、.net `$type`型名を示す文字列プロパティもあります。

クライアント統合コードに TypeScript を使用することをお勧めします。これは、もちろんバニラ JavaScript にコンパイルされます。 どちらの方法でも、SDK では、TypeScript によって直接参照できる、または単にバニラ JavaScript の記述が推奨されている場合は手動で参照できる、入力[を提供し][typings]ます。

レンダリングの主な統合ポイントは`xamarin.interactive.RendererRegistry`次のとおりです。

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

ここで`PersonRenderer`は、 `Renderer`インターフェイスを実装します。 詳細については、「[typings][typings]」を参照してください。

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
