---
title: Xamarin ブック SDK の概要
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: ad7341776c2f37d4eb6238a26d5b12ee9e340ff7
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/18/2018
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Xamarin ブック SDK の概要

このドキュメントでは、Xamarin のブックの統合などの開発の概要のクイック ガイドを提供します。 ブックでは、安定した Xamarin、問題の大半は動作しますが、 **NuGet パッケージを使用して統合の読み込みは、ブック 1.3 ではサポートされてのみ**の執筆時点でアルファ チャネルにします。

## <a name="general-overview"></a>全般の概要

Xamarin のブックの統合は、小規模のライブラリを使用して、 [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] SDK Xamarin ブックおよびインスペクター拡張されたエクスペリエンスを提供するエージェントと統合します。

3 つの主要な手順との統合の開発の概要がある: ここでそれらの概要について説明しました。

## <a name="creating-the-integration-project"></a>統合プロジェクトの作成

統合ライブラリは、マルチプラット フォーム ライブラリとして最適な開発されています。 すべての利用可能なエージェント、過去および未来の最適な統合を提供するために広くサポートされている一連のライブラリを選択します。 広範なサポートの「ポータブル ライブラリ」テンプレートを使用することをお勧めします。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![ポータブル ライブラリ テンプレートで Visual Studio for Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![ポータブル ライブラリ テンプレートの Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

Visual Studio では、ポータブル ライブラリの次のターゲット プラットフォームを選択するかどうかを確認します。

[![ポータブル ライブラリ プラットフォームの Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

参照を追加、ライブラリ プロジェクトを作成すると、 `Xamarin.Workbooks.Integration` NuGet ライブラリ、NuGet パッケージ マネージャーを使用しています。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![NuGet の Visual Studio for Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![NuGet の Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

プロジェクトの一部として作成される空のクラスを削除します-しない必要がある、このです。 次の手順を完了するには、統合の構築を開始する準備ができたらです。

## <a name="building-an-integration"></a>ビルドとの統合

シンプルな統合をビルドします。 本当にぜひ色緑、ため、各オブジェクトに表現として緑の色を追加します。 いう新しいクラスを最初に、作成`SampleIntegration`、実装して、 [ `IAgentIntegration` ] [ integration-type]インターフェイス。

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

実行するには、追加、[表現](~/tools/workbooks/sdk/representations.md)緑色であるすべてのオブジェクト。 表現プロバイダーを使用してこれを実行します。 プロバイダーを継承、 [ `RepresentationProvider` ] [ reppr]クラスなど、マイクロソフトのだけいただくためにオーバーライド[ `ProvideRepresentations` ] [ prrep]:

```csharp
using Xamarin.Interactive.Representations;

class SampleRepresentationProvider : RepresentationProvider
{
    public override IEnumerable<object> ProvideRepresentations (object obj)
    {
        // This corresponds to Pantone 2250 XGC, our favorite color.
        yield return new Color (0.0, 0.702, 0.4314);
    }
}
```

返している、 [ `Color` ] [ color]、あらかじめ、SDK の表現の型。
ここでの戻り値の種類があるかがわかります、 `IEnumerable<object>`&mdash;表現の 1 つのプロバイダーは、オブジェクトの多くの表現を返す可能性があります。 形式のすべてのプロバイダーは、ことが重要にどのようなオブジェクトが渡されているについてどのような想定が作成されないようにすべてのオブジェクトと呼ばれます。

最後の手順では、実際には、エージェントに、プロバイダーを登録し、統合型を検索する場所をブックに指示します。 プロバイダーを登録するには、するには、このコードを追加、`IntegrateWith`メソッドで、`SampleIntegration`以前に作成したクラス。

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

統合の種類の設定は、アセンブリ全体にわたる属性を使用して行われます。 AssemblyInfo.cs、または利便性のため、統合型と同じクラス内で、これを配置することができます。

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

開発中は、した方を使用する方が便利[`AddProvider`オーバー ロード][ addprovider]で`RepresentationManager`できるように、ブックの内部表現を提供する単純なコールバックを登録するにはそのコードを移動し、`RepresentationProvider`実装が完了します。 表示するための使用例、 [ `OxyPlot` ] [ oxyplot] `PlotModel`次のようになります。

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> これらの Api は、実行を取得する簡単な方法を割り当てる一方ではお勧めだけでそれらを使用して、全体の統合を配布&mdash;クライアントによって、型を処理する方法をほとんど管理を提供します。

登録されている表現を統合している出荷できます!

## <a name="shipping-your-integration"></a>配布の統合

統合を出荷するには、NuGet パッケージに追加する必要があります。
既存のライブラリの NuGet を使用して発送するまたは新しいパッケージを作成する場合は、開始点としてこのテンプレートの .nuspec ファイルを使用することができます。
統合に関連するセクションを入力する必要があります。 最も重要な部分は、すべてのファイル、統合をする必要がありますに属している、`xamarin.interactive`パッケージのルートにあるディレクトリ。 これにより、既存のパッケージを使用して新規に作成したりするかどうかに関係なく、統合に関連するすべてのファイルを簡単に見つけることができます。

```xml
<?xml version="1.0"?>
<package xmlns="http://schemas.microsoft.com/packaging/2010/07/nuspec.xsd">
    <metadata>
      <id>$YourNuGetPackage$</id>
      <version>$YourVersion$</version>
      <authors>$YourNameHere$</authors>
      <projectUrl>$YourProjectPage$</projectUrl>
      <description>A short description of your library.</description>
    </metadata>
    <files>
      <file src="Path\To\Your\Integration.dll" target="xamarin.interactive" />
    </files>
</package>
```

NuGet をパックすることができます、.nuspec ファイルを作成したら、次のようにします。

```csharp
nuget pack MyIntegration.nuspec
```

発行および[NuGet][nugetorg]です。 後は、すべてのブックから参照し、動作を確認することができます。 以下のスクリーン ショット、このドキュメントでビルドし、ブック内の NuGet パッケージのインストールおサンプルの統合をパッケージしました。

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![統合が含まれたブック](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![統合が含まれたブック](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

表示されないことに注意してください`#r`ディレクティブや、統合を初期化するために何も: ブックが処理を行いますすべてが、バック グラウンドで!

## <a name="next-steps"></a>次の手順

チェック アウトの詳細については、SDK を構成する移動の個の他のドキュメントと当社[サンプルの統合](~/tools/workbooks/samples/index.md)の追加事項で実行されている javascript のコードを提供するように、統合から行うことができますブックのクライアント。

[integration-type]: https://developer.xamarin.com/api/type/Xamarin.Interactive.IAgentIntegration/
[repman-api]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[xir]: https://developer.xamarin.com/api/namespace/Xamarin.Interactive.Representations/
[reppr]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[prrep]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.RepresentationProvider.ProvideRepresentations/p/System.Object/
[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[addprovider]: https://developer.xamarin.com/api/member/Xamarin.Interactive.Representations.IRepresentationManager.AddProvider/
[oxyplot]: http://www.oxyplot.org/
