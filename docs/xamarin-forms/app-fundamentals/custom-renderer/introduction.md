---
title: カスタム レンダラーの概要
description: この記事では、カスタムのレンダラーを紹介し、カスタム レンダラーを作成するプロセスの概要を示します。
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: fa22be081433bdd0c59a0d921511d3f3d83d4448
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35241765"
---
# <a name="introduction-to-custom-renderers"></a>カスタム レンダラーの概要

_カスタムのレンダラーでは、Xamarin.Forms コントロールの動作と外観をカスタマイズするための強力な方法を提供します。これらは、小規模のスタイル設定の変更または高度なプラットフォーム固有のレイアウトと動作のカスタマイズ使用できます。この記事では、カスタムのレンダラーを紹介し、カスタム レンダラーを作成するプロセスの概要を示します。_

Xamarin.Forms[ページ、レイアウトとコントロール](~/xamarin-forms/user-interface/controls/index.md)クロスプラット フォーム モバイル ユーザー インターフェイスを記述する一般的な API を提供します。 各ページ、レイアウト、およびコントロールを各プラットフォームの異なる方法で表示を使用して、 `Renderer` (Xamarin.Forms 表記に対応する)、ネイティブなコントロールを作成するクラスを選択して、画面で、配置で指定された動作を追加します共有コードです。

開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 指定された型のカスタム レンダラーは、その他のプラットフォームで、既定の動作を許可するときに 1 つの場所でコントロールをカスタマイズする 1 つのアプリケーション プロジェクトに追加することができます。または、別のカスタム レンダラーは、iOS、Android、およびユニバーサル Windows プラットフォーム (UWP) にさまざまなルック アンド フィールを作成するには、各アプリケーション プロジェクトに追加することができます。 ただし、単純なコントロールのカスタマイズを実行するカスタム レンダラー クラスの実装は、ヘビーウェイト応答では多くの場合です。 効果は、このプロセスを簡略化しは通常小さなスタイルの変更の使用します。 詳細については、「[Effects](~/xamarin-forms/app-fundamentals/effects/index.md)」 (効果) を参照してください。

## <a name="examining-why-custom-renderers-are-necessary"></a>調査の理由カスタム レンダラーが必要

Xamarin.Forms コントロールの外観を変更するには、カスタム レンダラーを使用せず、サブクラス化すると、元のコントロールの代わりにカスタム コントロールを使用してカスタム コントロールの作成は、2 段階のプロセスです。 次のコード例は、サブクラス化の例を示します、`Entry`コントロール。

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry`コントロールは、`Entry`場所を制御、`BackgroundColor`に設定されているグレー、およびその場所の名前空間を宣言して、コントロール要素で名前空間プレフィックスを使用して Xaml で参照できます。 次のコード例に示す方法、`MyEntry`カスタム コントロールで使用できる、 `ContentPage`:

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

`local`名前空間プレフィックスでは、あらゆるものがあります。 ただし、`namespace`と`assembly`値がカスタム コントロールの詳細情報と一致する必要があります。 名前空間が宣言されると、カスタム コントロールを参照する、プレフィックスを使用します。

> [!NOTE]
> 定義する、`xmlns`共有プロジェクトよりも .NET 標準のライブラリ プロジェクトで非常に簡単です。 .NET 標準ライブラリがアセンブリにコンパイルされるため、簡単に何かを判断、`assembly=CustomRenderer`値でなければなりません。 共有プロジェクトを使用する場合 (XAML を含む) すべての共有アセットまとめられ、参照元のプロジェクトは、iOS、Android、および UWP つまりの各プロジェクトがある独自*アセンブリ名*ことはできませんに書き込む`xmlns`宣言値は、アプリケーションごとに別にする必要があるためです。 プロジェクトの共有については XAML でカスタム コントロールは、すべてのアプリケーション プロジェクトを同じアセンブリ名で構成する必要があります。

`MyEntry`背景が灰色で、各プラットフォームでは、次のスクリーン ショットに示すようにカスタム コントロールを表示し、します。

![](introduction-images/screenshots.png "各プラットフォームで MyEntry カスタム コントロール")

各プラットフォームでコントロールの背景色を変更するが、コントロールをサブクラス化して使用するだけで実行されました。 ただし、この手法は、新機能にするためのプラットフォーム固有の拡張とカスタマイズを活用することはできないために制限されます。 必須では、カスタム レンダラーを実装する必要があります。

## <a name="creating-a-custom-renderer-class"></a>カスタム レンダラー クラスを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. ネイティブのコントロールを描画するレンダラー クラスのサブクラスを作成します。
1. ネイティブ コントロールを表示するメソッドをオーバーライドし、コントロールをカスタマイズするためのロジックを記述します。 多くの場合、`OnElementChanged`メソッドは、対応する Xamarin.Forms コントロールの作成時に呼び出されるネイティブのコントロールを表示するために使用します。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラス Xamarin.Forms コントロールを表示するために使用することを指定します。 この属性を使用して、Xamarin.Forms を使用したカスタム レンダラーを登録します。

> [!NOTE]
> Xamarin.Forms のほとんどの要素は各プラットフォームのプロジェクトでのカスタム レンダラーを提供する省略可能です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 ただし、カスタム レンダラーが必要に各プラットフォームのプロジェクトでレンダリングするときに、[ビュー](https://developer.xamarin.com/api/type/Xamarin.Forms.View/)または[ViewCell](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewCell/)要素。

この一連のトピックは、デモンストレーションと Xamarin.Forms のさまざまな要素のこのプロセスの説明に表示されます。

## <a name="troubleshooting"></a>トラブルシューティング

カスタム コントロールが含まれている .NET 標準ライブラリ プロジェクトがソリューションに追加された、(つまりいない .NET 標準ライブラリ、Visual Studio for Mac/visual Studio Xamarin.Forms アプリ プロジェクト テンプレートによって作成された)、例外が iOS で発生する場合カスタム コントロールにアクセスしようとしています。 この問題は、カスタム コントロールへの参照を作成することで解決できますが発生した場合、`AppDelegate`クラス。

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

これにより、コンパイラで認識する、`ClassInPCL`の型でそれを解決します。 または、`Preserve`に属性を追加することができます、`AppDelegate`同じ結果を実現するクラス。

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

これがへの参照を作成、`ClassInPCL`型、実行時にことが必要なことを示すです。 詳細については、次を参照してください。[維持コード](~/ios/deploy-test/linker.md)です。

## <a name="summary"></a>まとめ

この記事では、カスタムのレンダラーの概要を提供し、カスタム レンダラーを作成するプロセスの概要を説明しました。 カスタムのレンダラーでは、Xamarin.Forms コントロールの動作と外観をカスタマイズするための強力な方法を提供します。 これらは、小規模のスタイル設定の変更または高度なプラットフォーム固有のレイアウトと動作のカスタマイズ使用できます。


## <a name="related-links"></a>関連リンク

- [エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)
