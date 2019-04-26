---
title: Xamarin Workbooks SDK の概要
description: このドキュメントでは、Xamarin Workbooks の統合を開発するために使用できる Xamarin ブック SDK を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
author: lobrien
ms.author: laobri
ms.date: 03/30/2017
ms.openlocfilehash: 5800e98acbff147735ae4a6125979a4b47be2367
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61382756"
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Xamarin Workbooks SDK の概要

このドキュメントでは、簡単なガイドを Xamarin Workbooks の統合などの開発の概要を説明します。 安定版の Xamarin Workbooks では、多くが、**ブック 1.3 では NuGet パッケージを使用して統合の読み込みはサポートされてのみ**の執筆時点でアルファ チャネルにします。

## <a name="general-overview"></a>[全般] の概要

Xamarin Workbooks の統合が小規模のライブラリを使用して、 [ `Xamarin.Workbooks.Integrations` NuGet] [ nuget] SDK Xamarin Workbooks および Inspector と強化されたエクスペリエンスを提供するエージェントを統合します。

開発、統合の概要を 3 つの主要な手順は、それらをここで説明します。

## <a name="creating-the-integration-project"></a>統合プロジェクトを作成します。

統合ライブラリは、最適なマルチプラット フォーム ライブラリとして作成されます。 すべての使用可能なエージェント、過去、将来の最適な統合を提供するために幅広くサポートされている一連のライブラリを選択します。 「ポータブル ライブラリ」テンプレートを使用して、最も広範なサポートをお勧めします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![ポータブル ライブラリ テンプレートで Visual Studio for Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ポータブル ライブラリ テンプレートの Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

Visual Studio では、ポータブル ライブラリの次のターゲット プラットフォームを選択するかどうかを確認する必要があります。

[![ポータブル ライブラリのプラットフォームの Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

参照を追加、ライブラリ プロジェクトを作成すると、 `Xamarin.Workbooks.Integration` NuGet ライブラリ NuGet パッケージ マネージャーを使用しています。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![NuGet Visual Studio for Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

プロジェクトの一部として作成される空のクラスを削除します-するは必要ありません、このします。 次の手順を完了すると、統合の作成を開始する準備ができました。

## <a name="building-an-integration"></a>統合環境を構築

シンプルな統合をビルドします。 本当にぜひ色緑、緑の色の各オブジェクトの表現として追加しますので。 いう新しいクラスを最初に、作成`SampleIntegration`、し、実装、 [ `IAgentIntegration` ] [ integration-type]インターフェイス。

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

実行するには、追加、[表現](~/tools/workbooks/sdk/representations.md)緑の色であるすべてのオブジェクト。 表現のプロバイダーを使用してこれを実行します。 プロバイダーが継承、 [ `RepresentationProvider` ] [ reppr]クラスなど、私たちのだけをオーバーライドする[ `ProvideRepresentations` ] [ prrep]:

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

返します、 [ `Color` ] [ color]、既成の SDK での表現の型。
ここでの戻り値の型は、 `IEnumerable<object>`&mdash;表現の 1 つのプロバイダーは、オブジェクトの多くの表現を返す可能性があります。 渡されるオブジェクトについて何も想定することが重要であるため、すべてのオブジェクトの表現のすべてのプロバイダーと呼びます。

最後の手順では、実際には、エージェントに、プロバイダーを登録し、統合の種類を検索する場所をブックに指示します。 プロバイダーを登録するには、このコードを追加、`IntegrateWith`メソッドで、`SampleIntegration`以前に作成したクラス。

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

統合型の設定は、アセンブリ レベル属性を使用して行われます。 AssemblyInfo.cs、または利便性のため、統合型と同じクラスに配置できます。

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

開発中は、かもしれませんを使用する方が便利[`AddProvider`オーバー ロード][ addprovider]で`RepresentationManager`ブック内の表現を提供する単純なコールバックを登録することができますそのコードを移動し、`RepresentationProvider`実装が終わるとします。 レンダリングの例を[ `OxyPlot` ] [ oxyplot] `PlotModel`ようになります。

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> これらの Api を使用すると簡単にセットアップし、実行の入手が全体の統合を配布を使用することのみお勧めできません&mdash;クライアントによって、型を処理する方法のごくわずかな制御を提供します。

登録されている表現は、統合している出荷できます。

## <a name="shipping-your-integration"></a>配布の統合

統合を出荷するには、NuGet パッケージに追加する必要があります。
それを出荷するには、既存のライブラリの nuget をまたは新しいパッケージを作成する場合は、開始点としてこのテンプレートの .nuspec ファイルを使用できます。
統合に関連するセクションでは、記入する必要があります。 最も重要な部分はすべての統合するためのファイル内でなければならないこと、`xamarin.interactive`パッケージのルートにあるディレクトリ。 これにより、既存のパッケージを使用して、または、新しく作成するかどうかに関係なく、統合に関連するすべてのファイルを簡単に見つけることができます。

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

発行および[NuGet][nugetorg]します。 後ブックからを参照し、動作を確認することができます。 次のスクリーン ショットで、サンプルの統合このドキュメントで構築し、ブックで NuGet パッケージをインストールをパッケージ化したしました。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![ブックとの統合](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ブックとの統合](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

表示されないことに注意してください`#r`ディレクティブなど、統合を初期化するために、ブックは処理を実行するすべてが、バック グラウンドで!

## <a name="next-steps"></a>次の手順

チェック アウト、SDK を構成する動的な部分の詳細については、他のドキュメントと[統合サンプル](~/tools/workbooks/samples/index.md)の追加の処理で実行される javascript のコードを提供するように、統合から行うことができますブックのクライアント。

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
