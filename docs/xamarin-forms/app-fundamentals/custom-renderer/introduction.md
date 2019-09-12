---
title: カスタム レンダラーの概要
description: この記事では、カスタム レンダラーの概要を示し、カスタム レンダラーを作成するプロセスについて説明します。
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: ad2868a82f662f45066a6111a1dd3bd2aacad671
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2019
ms.locfileid: "70771879"
---
# <a name="introduction-to-custom-renderers"></a>カスタム レンダラーの概要

_カスタム レンダラーにより、Xamarin.Forms コントロールの外観とビヘイビアーをカスタマイズするための強力な方法が提供されます。それらは、スタイルに関する小さな変更や、洗練されたプラットフォーム固有のレイアウトおよびビヘイビアーのカスタマイズのために使用できます。この記事では、カスタム レンダラーの概要を示し、カスタム レンダラーを作成するプロセスについて説明します。_

Xamarin.Forms の [Pages、Layouts、Controls](~/xamarin-forms/user-interface/controls/index.md) には、クロスプラットフォームのモバイル ユーザー インターフェイスを記述するための共通 API が用意されています。 各ページ、レイアウトおよびコントロールは、`Renderer` クラスを使用してプラットフォームごとに異なる方法でレンダリングされます。次に (Xamarin.Forms の処理形式に対応する) ネイティブ コントロールが作成され、画面に配置され、共有コードに指定された動作が追加されます。

開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 特定の種類のカスタム レンダラーを 1 つのアプリケーション プロジェクトに追加して、ある場所のコントロールをカスタマイズし、さらに他のプラットフォーム上の既定の動作を許可することができます。また、異なるカスタム レンダラーを各アプリケーション プロジェクトに追加して、iOS、Android、ユニバーサル Windows プラットフォーム (UWP) 上で異なる外観を作成することができます。 ただし、カスタム レンダラー クラスを実装してシンプルなコントロールのカスタマイズを実行すると、多くの場合、応答はヘビーウェイトになります。 効果によってこのプロセスは簡略化されます。通常、効果はわずかなスタイルの変更に使用されます。 詳細については、「[Effects](~/xamarin-forms/app-fundamentals/effects/index.md)」 (効果) を参照してください。

## <a name="examining-why-custom-renderers-are-necessary"></a>カスタム レンダラーが必要な理由の確認

カスタム レンダラーを使用せずに Xamarin.Forms コントロールの外観を変更するには、サブクラス化によってカスタム コントロールを作成し、元のコントロールの代わりにカスタム コントロールを使用するという 2 段階のプロセスがあります。 次のコード例に、`Entry` コントロールをサブクラス化する例を示します。

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry` コントロールは、`BackgroundColor` がグレーに設定された `Entry` コントロールです。参照するには、Xaml でその場所の名前空間を宣言し、コントロール要素の名前空間プレフィックスを使用します。 次のコード例は、`ContentPage` がどのように `MyEntry` カスタム コントロールを使用できるかを示しています。

```xaml
<ContentPage
    ...
    xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer"
    ...>
    ...
    <local:MyEntry Text="In Shared Code" />
    ...
</ContentPage>
```

任意の `local` 名前空間プレフィックスを指定できます。 ただし、`namespace` と `assembly` の値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用してカスタム コントロールが参照されます。

> [!NOTE]
> `xmlns` の定義は、共有プロジェクトよりも .NET Standard ライブラリ プロジェクトの方がはるかに簡単です。 .NET Standard ライブラリはアセンブリにコンパイルされるので、`assembly=CustomRenderer` の値を決定することは簡単です。 共有プロジェクトを使用する場合、すべての共有アセット (XAML を含む) は個々の参照プロジェクトにコンパイルされます。つまり、iOS、Android、および UWP プロジェクトにそれぞれ*アセンブリ名*がある場合、アプリケーションごとに異なる値にする必要があるため、`xmlns` 宣言を記述することはできません。 共有プロジェクト用の XAML のカスタム コントロールでは、すべてのアプリケーション プロジェクトを同じアセンブリ名を使用して構成する必要があります。

次のスクリーンショットに示すように、`MyEntry` カスタム コントロールは、各プラットフォームにグレーの背景でレンダリングされます。

![](introduction-images/screenshots.png "プラットフォームごとの MyEntry カスタム コントロール")

各プラットフォームのコントロールの背景色を変更するには、コントロールをサブクラス化する必要があります。 ただし、プラットフォーム固有の拡張機能やカスタマイズを利用できないため、この手法は実行できるものに限られています。 必要な場合は、カスタム レンダラーを実装する必要があります。

## <a name="creating-a-custom-renderer-class"></a>カスタム レンダラー クラスの作成

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. ネイティブ コントロールをレンダリングするレンダラー クラスのサブクラスを作成します。
1. ネイティブ コントロールをレンダリングするメソッドをオーバーライドして、ロジックを書き込み、コントロールをカスタマイズします。 多くの場合、`OnElementChanged` メソッドはネイティブ コントロールのレンダリングに使用され、対応する Xamarin.Forms コントロールが作成されるときに呼び出されます。
1. `ExportRenderer` 属性をカスタム レンダラー クラスに追加して、Xamarin.Forms コントロールのレンダリングに使用されるように指定します。 この属性は、Xamarin.Forms にカスタム レンダラーを登録するために使用します。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、プラットフォーム プロジェクトごとにカスタム レンダラーを指定するかどうかは任意です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、[View](xref:Xamarin.Forms.View) または [ViewCell](xref:Xamarin.Forms.ViewCell) 要素をレンダリングするときは、各プラットフォーム プロジェクトにカスタム レンダラーが必要です。

このシリーズのトピックでは、さまざまな Xamarin.Forms 要素に対するこのプロセスの例を示し、説明します。

## <a name="troubleshooting"></a>トラブルシューティング

ソリューションに追加された .NET Standard ライブラリ プロジェクトにカスタム コントロールが含まれる場合 (つまり、Visual Studio for Mac/Visual Studio Xamarin.Forms アプリ プロジェクト テンプレートで作成された .NET Standard ライブラリではない場合)、カスタム コントロールにアクセスしようとすると、iOS で例外が発生することがあります。 この問題が発生した場合は、`AppDelegate` クラスからカスタム コントロールへの参照を作成することで解決できます。

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

これにより、強制的に `ClassInPCL` 型を解決してコンパイラに認識させることができます。 代わりに、`Preserve` 属性を `AppDelegate` クラスに追加して同じ結果を得ることもできます。

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

これで実行時に必要であることを示す `ClassInPCL` 型への参照が作成されます。 詳細については、[コードの保存](~/ios/deploy-test/linker.md)に関するページを参照してください。

## <a name="summary"></a>まとめ

この記事では、カスタム レンダラーの概要を示し、カスタム レンダラーを作成するプロセスについて説明しました。 カスタム レンダラーにより、Xamarin.Forms コントロールの外観とビヘイビアーをカスタマイズするための強力な方法が提供されます。 それらは、スタイルに関する小さな変更や、洗練されたプラットフォーム固有のレイアウトおよびビヘイビアーのカスタマイズのために使用できます。

## <a name="related-links"></a>関連リンク

- [エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)
