---
title: カスタム レンダラーの概要
description: この記事では、カスタム レンダラーでは、概要を示し、カスタム レンダラーを作成するためのプロセスの概要を説明します。
ms.prod: xamarin
ms.assetid: 264314BE-1C5C-4727-A14E-F6F98151CDBD
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/19/2016
ms.openlocfilehash: 180a196e0b95854815c8a74ef1d2df63407dd04f
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998005"
---
# <a name="introduction-to-custom-renderers"></a>カスタム レンダラーの概要

_カスタム レンダラーは、Xamarin.Forms コントロールの動作と外観をカスタマイズするための強力なアプローチを提供します。小規模なスタイルの変更または高度なプラットフォーム固有のレイアウトと動作のカスタマイズが使用できます。この記事では、カスタム レンダラーでは、概要を示し、カスタム レンダラーを作成するためのプロセスの概要を説明します。_

Xamarin.Forms[ページ、レイアウト、およびコントロール](~/xamarin-forms/user-interface/controls/index.md)クロス プラットフォーム モバイルのユーザー インターフェイスを記述する一般的な API を提供します。 各ページ、レイアウト、およびコントロールは、プラットフォームごとに異なってレンダリングを使用して、 `Renderer` (Xamarin.Forms の表現に対応する)、ネイティブ コントロールを作成するクラスが、画面上で並べられで指定された動作を追加します、共有コード。

開発者は独自の `Renderer` クラスを実装して、コントロールの外観や動作をカスタマイズできます。 指定された型のカスタム レンダラーは、一方で、既定の動作は他のプラットフォームで 1 つの場所でコントロールをカスタマイズする 1 つのアプリケーション プロジェクトに追加することができます。または、別のカスタム レンダラーは、iOS、Android、ユニバーサル Windows プラットフォーム (UWP) で、さまざまなルック アンド フィールを作成するには、各アプリケーション プロジェクトに追加できます。 ただし、単純なコントロールのカスタマイズを実行するカスタム レンダラー クラスの実装は、ヘビーウェイトな応答では多くの場合です。 効果は、このプロセスを簡略化しは通常小さなスタイルの変更の使用します。 詳細については、「[Effects](~/xamarin-forms/app-fundamentals/effects/index.md)」 (効果) を参照してください。

## <a name="examining-why-custom-renderers-are-necessary"></a>調査のなぜカスタム レンダラーが必要になります

Xamarin.Forms コントロールの外観を変更するには、カスタム レンダラーを使用せず、2 段階のプロセスでは、サブクラス化し、元のコントロールの代わりにカスタム コントロールを使用してカスタム コントロールを作成します。 次のコード例は、サブクラス化の例を示します、`Entry`コントロール。

```csharp
public class MyEntry : Entry
{
  public MyEntry ()
  {
    BackgroundColor = Color.Gray;
  }
}
```

`MyEntry`コントロールが、`Entry`場所を制御、`BackgroundColor`に設定されているグレーのおよびその場所の名前空間を宣言して、コントロール要素の名前空間プレフィックスを使用して Xaml で参照できます。 次のコード例に示す方法、`MyEntry`でカスタム コントロールを使用できる、 `ContentPage`:

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

`local`名前空間プレフィックスは任意に指定できます。 ただし、`namespace`と`assembly`値は、カスタム コントロールの詳細と一致する必要があります。 名前空間が宣言されると、プレフィックスを使用して、カスタム コントロールを参照します。

> [!NOTE]
> 定義する、`xmlns`共有プロジェクトよりも、.NET Standard ライブラリ プロジェクトではるかに簡単です。 .NET Standard ライブラリがアセンブリにコンパイルされるため、何かを判断するは簡単、`assembly=CustomRenderer`値にする必要があります。 (、XAML を含む) すべての共有資産が各参照元のプロジェクトは、iOS、Android、および UWP つまりにコンパイルされた共有プロジェクトを使用する場合のプロジェクトがある独自*アセンブリ名*ことはできませんを記述するには`xmlns`宣言、値は、アプリケーションごとに異なる必要があるためです。 共有プロジェクトの XAML でカスタム コントロールは、すべてのアプリケーション プロジェクトに同じアセンブリ名で構成する必要があります。

`MyEntry`カスタム コントロールは上に表示される各プラットフォームで、灰色の背景では、次のスクリーン ショットに示すようにします。

![](introduction-images/screenshots.png "各プラットフォームで MyEntry カスタム コントロール")

各プラットフォーム上のコントロールの背景色を変更するが、コントロールをサブクラス化して使用するだけで実現されています。 ただし、この手法はどのような実現されるようにプラットフォーム固有の機能拡張とカスタマイズを活用することはできませんで制限されます。 必要なカスタム レンダラーを実装する必要があります。

## <a name="creating-a-custom-renderer-class"></a>カスタム レンダラー クラスを作成します。

カスタム レンダラー クラスを作成するプロセスは次のとおりです。

1. ネイティブ コントロールを描画するレンダラー クラスのサブクラスを作成します。
1. ネイティブ コントロールをレンダリングするメソッドをオーバーライドし、制御をカスタマイズするロジックを記述します。 多くの場合、`OnElementChanged`メソッドは、対応する Xamarin.Forms コントロールの作成時に呼び出されるネイティブのコントロールを表示するために使用します。
1. 追加、`ExportRenderer`属性をカスタム レンダラー クラス Xamarin.Forms コントロールを表示するために使用することを指定します。 この属性は、Xamarin.Forms でのカスタム レンダラーの登録に使用されます。

> [!NOTE]
> ほとんどの Xamarin.Forms 要素では、各プラットフォーム プロジェクトにカスタム レンダラーを提供する省略可能です。 カスタム レンダラーが登録されていない場合は、コントロールの基底クラスの既定のレンダラーが使用されます。 表示するときに各プラットフォーム プロジェクトに必要なカスタム レンダラーのただし、[ビュー](xref:Xamarin.Forms.View)または[ViewCell](xref:Xamarin.Forms.ViewCell)要素。

このシリーズのトピックは、その他のデモと Xamarin.Forms のさまざまな要素のこのプロセスの説明に提供されます。

## <a name="troubleshooting"></a>トラブルシューティング

追加されている (つまりいないは .NET Standard ライブラリ、Visual Studio for Mac または Visual Studio の Xamarin.Forms アプリ プロジェクト テンプレートによって作成された)、ソリューションに、例外、.NET Standard ライブラリ プロジェクトが iOS で発生する可能性にカスタム コントロールが含まれている場合場合カスタム コントロールにアクセスしようとしています。 この問題が発生してからカスタム コントロールへの参照を作成することで解決できる場合、`AppDelegate`クラス。

```csharp
var temp = new ClassInPCL(); // in AppDelegate, but temp not used anywhere
```

これにより、コンパイラで認識する、`ClassInPCL`して解決の種類。 または、`Preserve`に属性を追加できる、`AppDelegate`クラスは、同じ結果を実現するために。

```csharp
[assembly: Preserve (typeof (ClassInPCL))]
```

これがへの参照を作成、`ClassInPCL`時ことが必要なことを示す型。 詳細については、次を参照してください。[維持コード](~/ios/deploy-test/linker.md)します。

## <a name="summary"></a>まとめ

この記事では、カスタム レンダラーでは、概要を提供し、カスタム レンダラーを作成するためのプロセスの概要を説明しました。 カスタム レンダラーは、Xamarin.Forms コントロールの動作と外観をカスタマイズするための強力なアプローチを提供します。 小規模なスタイルの変更または高度なプラットフォーム固有のレイアウトと動作のカスタマイズが使用できます。


## <a name="related-links"></a>関連リンク

- [エフェクト](~/xamarin-forms/app-fundamentals/effects/index.md)
