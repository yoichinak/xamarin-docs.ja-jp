---
title: Xamarin Workbooks での表示
description: このドキュメントでは、値を返す任意のコードの豊富な結果を描画すること、Xamarin Workbooks 表現パイプラインについて説明します。
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: d9aafbe13e06875b6577a4d2308e419932fd1589
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103713"
---
# <a name="representations-in-xamarin-workbooks"></a>Xamarin Workbooks での表示

## <a name="representations"></a>表現

ブックまたはインスペクターのセッション内でコードで実行され、結果 (例: 値または式の結果を返すメソッド) が得られますが、エージェントで表現パイプラインを介して処理されます。 整数などのプリミティブを除く、すべてのオブジェクトは、対話型のメンバーのグラフを生成するために反映されより表現力豊かなクライアントをレンダリングする代替の表現を提供するプロセスを通過します。 安全に、任意のサイズ、深さのオブジェクトは遅延と対話型のリフレクションとリモート処理のため (サイクルと無限の列挙) を含むサポートされています。

Xamarin Workbooks は、すべてのエージェントと結果のリッチなレンダリングでは、クライアントへの一般的ないくつかの型を提供します。 [`Color`][xir-color] このような型の 1 つの例は、ios など、エージェントが変換`CGColor`または`UIColor`にオブジェクトを`Xamarin.Interactive.Representations.Color`オブジェクト。

共通の表現だけでなく統合 SDK には、エージェントのカスタム表現をシリアル化と、クライアントでの表現をレンダリング Api が提供されます。

## <a name="external-representations"></a>外部表現

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] 登録する機能を提供する[ `RepresentationProvider` ] [repp]統合が、任意のオブジェクトから表示するために依存しない形式に変換するために実装する必要があります。 これらに依存しない形式を実装する必要があります、 [ `ISerializableObject` ] [ serobj]インターフェイス。

実装する、`ISerializableObject`インターフェイス オブジェクトをシリアル化する方法を正確に制御する Serialize メソッドを追加します。 `Serialize`メソッドは、シリアル化するプロパティがあり、最終的な名前がなります、開発者が正確に指定ことが必要です。 見て、`Person`オブジェクトで、[`KitchenSink`サンプル] [サンプル] にこのしくみがわかります。

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

スーパー セットであるか、元のオブジェクトからプロパティのサブセットを提供する場合を使用して行いますできます`Serialize`します。 事前に計算を提供するこのようなコードを実行たとえば、`Age`プロパティ`Person`:

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
> 生成 Api`ISerializableObject`オブジェクトがによって処理される必要はありません直接、`RepresentationProvider`します。 オブジェクトを表示する場合は**いない**、`ISerializableObject`での折り返しを処理する、`RepresentationProvider`します。

### <a name="rendering-a-representation"></a>表現のレンダリング

レンダラーは JavaScript で実装され、JavaScript を使用して表されるオブジェクトのバージョンにアクセスできるよう`ISerializableObject`します。 JavaScript のコピーがあります、 `$type` string プロパティは、.NET 型名を示します。

TypeScript を使用してクライアント統合コード、もちろん vanilla JavaScript にコンパイルされることをお勧めします。 どちらの方法でも、SDK が提供[typings] [ typings]は TypeScript によって直接参照または単に場合にのみ参照を手動で vanilla JavaScript をお勧めを記述できます。

レンダリングの主な統合ポイントは`xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

ここでは、`PersonRenderer`実装、`Renderer`インターフェイス。 参照してください、 [typings] [ typings]の詳細。

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
