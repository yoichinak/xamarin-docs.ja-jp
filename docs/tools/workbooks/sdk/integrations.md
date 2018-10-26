---
title: 高度な統合に関するトピック
description: このドキュメントでは、Xamarin Workbooks の統合に関連する高度なトピックについて説明します。 Xamarin.Workbook.Integrations NuGet パッケージと、Xamarin のブック内の API の公開がについて説明します。
ms.prod: xamarin
ms.assetid: 002CE0B1-96CC-4AD7-97B7-43B233EF57A6
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 56ee709b78b8587c2717dc9d25a6357041812d23
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104207"
---
# <a name="advanced-integration-topics"></a>高度な統合に関するトピック

統合のアセンブリを参照する必要があります、 [ `Xamarin.Workbooks.Integrations` NuGet][nuget]します。 チェック アウト、[クイック スタート ドキュメント](~/tools/workbooks/sdk/index.md)NuGet パッケージの概要の詳細についてはします。

クライアント統合もサポートされているし、同じディレクトリに、エージェントの統合のアセンブリと同じ名前の JavaScript や CSS ファイルを配置することによって開始されます。 たとえば、(これは、NuGet を参照) エージェント統合アセンブリの名前が`SampleExternalIntegration.dll`、し`SampleExternalIntegration.js`と`SampleExternalIntegration.css`存在する場合にも、クライアントに統合されます。 クライアントの統合は省略可能です。

自体外部統合の NuGet としてパッケージ化、提供され、エージェントをホストしている、または単に並行して配置されるアプリケーション内で直接参照されていることができますが、`.workbook`がそれを使用するファイル。

パッケージが参照されると、クイック スタートのドキュメントに従ってブックと共に出荷された統合アセンブリが参照するために必要がありますが、NuGet パッケージの外部の統合 (エージェントとクライアント) は自動的に読み込みます。

```csharp
#r "SampleExternalIntegration.dll"
```

統合この方法を参照するときに読み込まれません、クライアントによってすぐ&mdash;読み込むへの統合からいくつかのコードを呼び出す必要があります。 私たち対処このバグ今後します。

`Xamarin.Interactive` PCL は、いくつかの重要な統合 Api を提供します。 すべての統合では、統合エントリ ポイントを提供する必要がありますには少なくとも。

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

この時点では、統合のアセンブリが参照されていると、クライアントは JavaScript と CSS の統合ファイルを読み込む暗黙的に。

## <a name="apis"></a>API

ブックで参照またはライブである任意のアセンブリでは、セッションを検査、そのパブリック Api のいずれかは、セッションにアクセスできます。 そのためユーザーを探索のための安全かつ実用的な API サーフェスに重要ですが。

統合のアセンブリは、事実上、アプリケーションまたは関心のある SDK と、セッション間のブリッジです。 セッションを検査またはパブリック Api を指定しないとオブジェクトを生成するように"バック グラウンドで"タスクを実行するだけ、具体的にはライブ ブックのコンテキストで意味のある新しい Api を提供できる[表現](~/tools/workbooks/sdk/representations.md)します。

> [!NOTE]
> パブリックである必要がありますが、IntelliSense で表示する必要がありますいない Api は、通常でマークできる`[EditorBrowsable (EditorBrowsableState.Never)]`属性。

[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
