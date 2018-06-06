---
title: Xamarin のブックの表示
description: このドキュメントでは、値を返す任意のコードの豊富な結果の表示を有効、Xamarin ブック表現パイプラインについて説明します。
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: d4d8fa164b9f52e2c5331aa2c08fdddf232572d4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794164"
---
# <a name="representations-in-xamarin-workbooks"></a>Xamarin のブックの表示

## <a name="representations"></a>表現

ブックやインスペクターのセッション内でが実行され、結果 (値または式の結果を返すメソッドなど) を生成するコードは、エージェントで、形式をパイプラインを介して処理されます。 整数などのプリミティブを除く、すべてのオブジェクトは、対話型のメンバーのグラフを生成するために反映され、クライアントが詳しく表示できる代替の表現を提供するためのプロセスを通過します。 オブジェクトのサイズと深さは、レイジーと対話型のリフレクションとリモート処理のため (サイクルと無限列挙型) を含む安全にサポートします。

Xamarin のブックでは、すべてのエージェントと結果の豊富なレンダリングできるようにするクライアントに一般的ないくつかの型を提供します。 [`Color`][xir-color] このような型の 1 つの例の iOS の例では、エージェントの変換を行います`CGColor`または`UIColor`オブジェクトを`Xamarin.Interactive.Representations.Color`オブジェクト。

共通の表現に加えては、SDK の統合は、エージェントのカスタム形式をシリアル化とクライアントでの表現を描画 Api を提供します。

## <a name="external-representations"></a>外部表現

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] 登録する機能を提供する[ `RepresentationProvider` ] [repp]との統合が任意のオブジェクトから表示するために依存しない形式に変換するために実装する必要があります。 これらの依存しない形式を実装する必要があります、 [ `ISerializableObject` ] [ serobj]インターフェイスです。

実装する、`ISerializableObject`インターフェイスは、オブジェクトをシリアル化する方法を正確に制御するシリアル化メソッドを追加します。 `Serialize`メソッドは、開発者ではどのプロパティがシリアル化するにし、なります最終的な名前を指定しますだけことが必要です。 見て、`Person`オブジェクトで、[`KitchenSink`サンプル] [サンプル] をこのしくみがわかります。

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

スーパー セットでも、元のオブジェクトからプロパティのサブセットを提供する場合、これにはで`Serialize`です。 たとえば、事前計算済みを提供する次のようなお行う可能性があります`Age`プロパティ`Person`:

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
> 生成 Api`ISerializableObject`オブジェクトがによって処理する必要はありません直接、`RepresentationProvider`です。 オブジェクトを表示する場合は**いない**、`ISerializableObject`での折り返しを処理する必要は、`RepresentationProvider`です。

### <a name="rendering-a-representation"></a>内部表現を表示

レンダラーが JavaScript で実装され、経由で表されるオブジェクトの JavaScript のバージョンへのアクセスを持つ`ISerializableObject`します。 また、JavaScript のコピーが必要があります、`$type`文字列を .NET 型の名前を示すプロパティです。

もちろんバニラ JavaScript にコンパイルされるクライアント統合コードの TypeScript を使用することをお勧めします。 どちらの方法では、SDK の提供[型指定][ typings]は TypeScript によって直接参照または単に参照を手動でバニラを記述して、JavaScript が優先される場合できます。

表示するための主な統合のポイントは`xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

ここでは、`PersonRenderer`を実装する、`Renderer`インターフェイスです。 参照してください、[型指定][ typings]詳細についてはします。

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
