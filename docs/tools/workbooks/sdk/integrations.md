---
title: 高度な統合に関するトピック
description: このドキュメントでは、Xamarin Workbooks 統合に関連する高度なトピックについて説明します。 このトピックでは、xamarin ブック内での Xamarin. Workbook と API の公開について説明します。
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: f8105c8285e696f8754799c33c30e31ce5356870
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283892"
---
# <a name="advanced-integration-topics"></a>高度な統合に関するトピック

統合アセンブリは[ `Xamarin.Workbooks.Integrations` NuGet][nuget]を参照する必要があります。 NuGet パッケージの概要については、[クイックスタートドキュメント](~/tools/workbooks/sdk/index.md)を参照してください。

クライアント統合もサポートされており、同じディレクトリ内のエージェント統合アセンブリと同じ名前の JavaScript または CSS ファイルを配置することによって開始されます。 たとえば、(NuGet を参照する) エージェント統合アセンブリにという名前が`SampleExternalIntegration.dll`付け`SampleExternalIntegration.js`られ`SampleExternalIntegration.css`ている場合、とは、クライアントにも統合されます (存在する場合)。 クライアント統合は任意です。

外部統合自体は、NuGet としてパッケージ化し、エージェントをホストしているアプリケーション内で直接参照することも、単`.workbook`にそれを使用するファイルと共に配置することもできます。

NuGet パッケージの外部統合 (エージェントとクライアント) は、パッケージの参照時に自動的に読み込まれます。クイックスタートドキュメントの場合と同様に、ブックと共に出荷された統合アセンブリは、次のように参照する必要があります。

```csharp
#r "SampleExternalIntegration.dll"
```

このように統合を参照する場合は、クライアント&mdash;によって読み込まれないようにするために、統合からコードを呼び出して読み込む必要があります。 今後、このバグに対処する予定です。

PCL `Xamarin.Interactive`には、いくつかの重要な統合 api が用意されています。 すべての統合で、少なくとも統合エントリポイントを提供する必要があります。

```csharp
using Xamarin.Interactive;

[assembly: AgentIntegration (typeof (AgentIntegration))]

class AgentIntegration : IAgentIntegration
{
    const string TAG = nameof (AgentIntegration);

    public void IntegrateWith (IAgent agent)
    {
        // hook into IAgent APIs
    }
}
```

この時点で、統合アセンブリが参照されると、クライアントは JavaScript と CSS の統合ファイルを暗黙的に読み込みます。

## <a name="apis"></a>API

ブックまたはライブ検査セッションによって参照されているアセンブリと同様に、そのパブリック Api はいずれもセッションにアクセスできます。 そのため、ユーザーが調査するために、安全で実用的な API サーフェスを用意することが重要です。

統合アセンブリは、実質的にはアプリケーションまたは SDK とセッションの間でブリッジされます。 これは、特にブックまたはライブ検査セッションのコンテキストで意味を持つ新しい Api を提供したり、パブリック Api を提供したり、オブジェクト[表現](~/tools/workbooks/sdk/representations.md)の生成などの "背後で" バックグラウンドで実行したりすることができます。

> [!NOTE]
> パブリックである必要がありますが、IntelliSense を使用して表示しない api `[EditorBrowsable (EditorBrowsableState.Never)]`は、通常の属性でマークできます。

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
