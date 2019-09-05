---
title: Xamarin Workbooks SDK を使用したはじめに
description: このドキュメントでは、Xamarin Workbooks の統合を開発するために使用できる Xamarin Workbooks SDK の概要について説明します。
ms.prod: xamarin
ms.assetid: FAED4445-9F37-46D8-B408-E694060969B9
author: conceptdev
ms.author: crdun
ms.date: 03/30/2017
ms.openlocfilehash: 8e3dc65f9f615ff893f3526d53d99da25045c794
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283967"
---
# <a name="getting-started-with-the-xamarin-workbooks-sdk"></a>Xamarin Workbooks SDK を使用したはじめに

このドキュメントでは、Xamarin Workbooks の統合開発の概要について簡単に説明します。 この多くは安定した Xamarin Workbooks で機能しますが、NuGet パッケージを使用した統合の読み込みは、書き込み時の alpha チャネルの**ブック1.3 でのみサポート**されています。

## <a name="general-overview"></a>全般の概要

Xamarin Workbooks 統合は、 [ `Xamarin.Workbooks.Integrations` NuGet][nuget] SDK を使用して Xamarin Workbooks およびインスペクターエージェントと統合し、拡張されたエクスペリエンスを提供する小さなライブラリです。

統合の開発を開始するための主な手順は3つあります。ここでは、その概要を説明します。

## <a name="creating-the-integration-project"></a>統合プロジェクトの作成

統合ライブラリは、マルチプラットフォームライブラリとして開発することをお勧めします。 使用可能なすべてのエージェント (過去および将来) に最適な統合を提供するため、広範にサポートされているライブラリのセットを選択する必要があります。 最も広範なサポートには、"ポータブルライブラリ" テンプレートを使用することをお勧めします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![ポータブルライブラリテンプレート Visual Studio for Mac](images/xamarin-studio-pcl.png)](images/xamarin-studio-pcl.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![ポータブルライブラリテンプレート Visual Studio](images/visual-studio-pcl.png)](images/visual-studio-pcl.png#lightbox)

Visual Studio では、ポータブルライブラリに対して次のターゲットプラットフォームを選択する必要があります。

[![ポータブルライブラリプラットフォーム Visual Studio](images/visual-studio-pcl-platforms.png)](images/visual-studio-pcl-platforms.png#lightbox)

-----

ライブラリプロジェクトを作成したら、nuget パッケージマネージャーを使用`Xamarin.Workbooks.Integration`して nuget ライブラリへの参照を追加します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![NuGet Visual Studio for Mac](images/xamarin-studio-nuget.png)](images/xamarin-studio-nuget.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![NuGet Visual Studio](images/visual-studio-nuget.png)](images/visual-studio-nuget.png#lightbox)

-----

プロジェクトの一部として作成された空のクラスを削除する必要があります。これには必要ありません。 これらの手順を完了すると、統合の構築を開始する準備が整います。

## <a name="building-an-integration"></a>統合の構築

単純な統合を構築します。 私たちは緑色の色を気に入っているので、各オブジェクトの表現として緑色の色を追加します。 最初に、という名前`SampleIntegration`の新しいクラスを作成し、 `IAgentIntegration`インターフェイスを実装します。

```csharp
using Xamarin.Interactive;

public class SampleIntegration : IAgentIntegration
{
    public void IntegrateWith (IAgent agent)
    {
    }
}
```

ここでは、緑の色を持つすべてのオブジェクトの[表現](~/tools/workbooks/sdk/representations.md)を追加します。 これを行うには、表現プロバイダーを使用します。 プロバイダーは`RepresentationProvider`クラスから継承します。私たちは、次の`ProvideRepresentations`ようなオーバーライドを行うだけです。

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

SDK で事前に`Color`構築された表現型であるを返しています。
ここでの戻り値の型は`IEnumerable<object>` &mdash;、1つの表現プロバイダーがオブジェクトに対して多くの表現を返す場合があることに注意してください。 すべての表現プロバイダーはすべてのオブジェクトに対して呼び出されるため、どのオブジェクトが渡されているかを想定しないことが重要です。

最後の手順では、実際にプロバイダーをエージェントに登録し、統合の種類を見つける場所をブックに指示します。 プロバイダーを登録するには、前の手順`IntegrateWith` `SampleIntegration`で作成したクラスのメソッドに次のコードを追加します。

```csharp
agent.RepresentationManager.AddProvider (new SampleRepresentationProvider ());
```

統合の種類の設定は、アセンブリ全体の属性を使用して行います。 これは、AssemblyInfo.cs に配置することも、統合の種類と同じクラスで使いやすくすることもできます。

```csharp
[assembly: AgentIntegration (typeof (SampleIntegration))]
````

開発時には、で`AddProvider` `RepresentationManager`オーバーロードを使用する方が便利な場合があります。これにより、簡単なコールバックを登録してブック内に表現`RepresentationProvider`を提供し、そのコードを実装に1回移動できます。完了しました。 を表示[`OxyPlot`][oxyplot] `PlotModel`する例は次のようになります。

```csharp
InteractiveAgent.RepresentationManager.AddProvider<PlotModel> (
  plotModel => Image (new SvgExporter {
      Width = 300,
      Height = 250
    }.ExportToString (plotModel)));
```

> [!NOTE]
> これらの api を使用すると、すぐに稼働状態にすることができますが、統合全体を使用する&mdash;ことはお勧めしません。これにより、クライアントによる型の処理方法をごくわずかに制御できるようになります。

この表現が登録されると、統合の準備ができました。

## <a name="shipping-your-integration"></a>統合の配布

統合を配布するには、NuGet パッケージに追加する必要があります。
既存のライブラリの NuGet を使用して配布することも、新しいパッケージを作成する場合は、この nuspec ファイルを出発点として使用することもできます。
統合に関連するセクションを入力する必要があります。 最も重要なのは、統合のすべてのファイルが、パッケージのルートに`xamarin.interactive`あるディレクトリに存在する必要があることです。 これにより、既存のパッケージを使用するか、新しいパッケージを作成するかに関係なく、統合に関連するすべてのファイルを簡単に見つけることができます。

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

Nuspec ファイルを作成したら、次のように NuGet をパックできます。

```csharp
nuget pack MyIntegration.nuspec
```

その後、 [NuGet][nugetorg]に発行します。 これが完了すると、任意のブックからそれを参照し、動作していることを確認できるようになります。 次のスクリーンショットでは、このドキュメントで構築したサンプル統合をパッケージ化し、ブックに NuGet パッケージをインストールしました。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[![統合されるブック](images/mac-workbooks-integrated.png)](images/mac-workbooks-integrated.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![統合されるブック](images/windows-workbooks-integrated.png)](images/windows-workbooks-integrated.png#lightbox)

-----

統合を初期化するための`#r`ディレクティブや何も表示されないことに注意してください。ブックは、すべての操作をバックグラウンドで実行しています。

## <a name="next-steps"></a>次の手順

SDK の構成要素の詳細については、他のドキュメントを参照してください。また、ブッククライアントで実行されるカスタム JavaScript の提供など、統合によって可能なその他の作業の[サンプル統合](~/tools/workbooks/samples/index.md)もご確認ください。

[nugetorg]: https://nuget.org
[nuget]: https://nuget.org/packages/Xamarin.Workbooks.Integration
[oxyplot]: http://www.oxyplot.org/
