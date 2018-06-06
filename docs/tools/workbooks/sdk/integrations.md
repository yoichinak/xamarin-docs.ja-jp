---
title: 高度な統合に関するトピック
description: このドキュメントでは、Xamarin のブックの統合に関連する高度なトピックについて説明します。 Xamarin.Workbook.Integrations NuGet パッケージと Xamarin ブック内の API の露出がについて説明します。
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: 1aa6b5d0ca574345e1d349ea53df96f554c06bc4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/05/2018
ms.locfileid: "34793837"
---
# <a name="advanced-integration-topics"></a>高度な統合に関するトピック

統合のアセンブリを参照する必要があります、 [ `Xamarin.Workbooks.Integrations` NuGet][nuget]です。 チェック アウト、[クイック スタート ドキュメント](~/tools/workbooks/sdk/index.md)NuGet パッケージの概要の詳細についてはします。

クライアントの統合もサポートされ、同じディレクトリに、エージェントの統合のアセンブリと同じ名前の JavaScript または CSS ファイルを配置することによって開始されます。 たとえば、(これは、NuGet を参照)、エージェントの統合アセンブリがという名前`SampleExternalIntegration.dll`、し`SampleExternalIntegration.js`と`SampleExternalIntegration.css`存在する場合も、クライアントに統合されます。 クライアントの統合はオプションです。

外部統合自体 NuGet としてパッケージ化、提供して、エージェントをホストしているか、単に残しているアプリケーション内で直接参照されている、`.workbook`それを利用することを希望するファイル。

NuGet パッケージの外部の統合 (エージェントとクライアント) が自動的に読み込まれますパッケージは、参照、クイック スタート ドキュメントに従ってブックと一緒に出荷統合アセンブリは、そのために参照する必要があります。

```csharp
#r "SampleExternalIntegration.dll"
```

参照する場合、統合こうすると、それは読み込まれませんクライアントですぐ&mdash;読み込むへの統合からいくつかのコードを呼び出す必要があります。 このバグを今後アドレスするおします。

`Xamarin.Interactive` PCL がいくつかの重要な統合 Api を提供します。 すべての統合では、統合エントリ ポイントを提供する必要がありますには、少なくとも。

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

この時点では、統合アセンブリが参照されていると、クライアントは JavaScript と CSS の統合ファイルを読み込みます暗黙的にします。

## <a name="apis"></a>API

ブックで参照されているか、ライブである任意のアセンブリでは、セッションを検査、そのパブリック Api のいずれかは、セッションにアクセスできます。 そのためユーザーを探索するために安全にわかりやすい API サーフェスを理解しておくことができます。

統合アセンブリは、事実上、アプリケーションまたは目的の SDK とセッション間のブリッジです。 セッションを検査またはパブリック Api が用意されていないオブジェクトを生成するように"バック グラウンドで"タスクを実行するだけのブックまたはライブのコンテキストで具体的には意味のある新しい Api を提供する[表現](~/tools/workbooks/sdk/representations.md)です。

> [!NOTE]
> 通常でマークできる Api がパブリックである必要がありますが、IntelliSense を使用して表示される必要がありますいない`[EditorBrowsable (EditorBrowsableState.Never)]`属性。

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
