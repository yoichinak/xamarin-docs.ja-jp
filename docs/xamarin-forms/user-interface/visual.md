---
title: Xamarin.Forms のビジュアル
description: この記事では、Xamarin.Forms のビジュアルは、iOS および Android 上の同じ、またはほぼ同じビューの表示について説明します。
ms.prod: xamarin
ms.assetid: 80BF9C72-AC28-4AAF-9DDD-B60CBDD1CD59
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2018
ms.openlocfilehash: df2351e35817a6468fed236752eea8e5ad11024f
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2018
ms.locfileid: "53061887"
---
# <a name="xamarinforms-visual"></a>Xamarin.Forms のビジュアル

![[プレビュー]](~/media/shared/preview.png)

_この記事では、Xamarin.Forms のビジュアルは、iOS および Android 上の同じ、またはほぼ同じビューの表示について説明します。_

多くの開発者は iOS と Android でまったく同じ、またはほぼ同じですが、外観の Xamarin.Forms アプリケーションを作成します。 Xamarin.Forms の 4.0 pre1 にアプリケーションを使用した、外観を選択すると、視覚的な外観を実装するその他のレンダラーを含めるためのメカニズムが含まれています、`Visual`プロパティ。

```xaml
<ContentPage ...
             Visual="Material">
    ...
</ContentPage>    
```

視覚的な外観を実装するレンダラーは、既定のレンダラーではなく、レンダラーのビューに使用されます。 レンダラーの選択時に、`Visual`ビューのプロパティを検査および表示機能の選択プロセスに含まれています。 さらに場合、`Visual`実行時にプロパティが変更された、すべての子と共に指定したレンダラーが再作成します。

> [!IMPORTANT]
> `Visual`でプロパティが定義されている、`VisualElement`クラスのインスタンス ビューが継承すると、`Visual`その親からのプロパティの値。 そのため、設定、`Visual`プロパティを`ContentPage`により、ページで、サポートされているビューがその視覚的な外観を使用するようになります。 さらに、`Visual`ビューのプロパティをオーバーライドできます。

Xamarin.Forms の 4.0 pre1 には、素材のレンダラーと呼ばれるされているレンダラーの素材のデザインに基づく実験用の visual 外観が含まれています。 素材のレンダラーは iOS と Android では、次のビューに現在含まれています。

- [`Button`](xref:Xamarin.Forms.Button)
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)

機能的には、素材のレンダラーは既定のレンダラーと変わりません。 ただしが現在実験段階し、次のコードの行を追加することでのみ使用できます、`AppDelegate`クラスでは、iOS、または、 `MainActivity` android では、クラスを呼び出す前に`Forms.Init`:

```csharp
Forms.SetFlags("Visual_Experimental");
```

さらに、ios では、プラットフォーム プロジェクトにする必要がありますが、 [Xamarin.iOS.MaterialComponents](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents/) NuGet パッケージをインストールします。 Android では、Visual API 29 でのみ、サポート ライブラリの v28 を使う必要がありマテリアル コンポーネント テーマから継承または AppCompat のテーマをテーマにいくつか新しいテーマの属性を追加するときに継承する継続のテーマの設定、プラットフォーム プロジェクトいます。 詳細については、次を参照してください。 [Android 用資料のコンポーネントの概要](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md)します。

次のスクリーン ショットを表示するマテリアルのレンダラーは存在するが、既定のレンダラーを使用してレンダリングの 4 つのビューを含むユーザー インターフェイス。

[![既定のレンダラー](visual-images/default-renderers.png "既定レンダラーを使用してビュー")](visual-images/default-renderers-large.png#lightbox)

次のスクリーン ショットは、素材のレンダラーを使用するレンダリングと同じユーザー インターフェイスを表示します。

[![素材のレンダラー](visual-images/material-renderers.png "素材のレンダラーを使用してビュー")](visual-images/material-renderers-large.png#lightbox)

> [!NOTE]
> 素材のレンダラーでは、レンダリングされたコントロールは引き続きネイティブ コントロールしたがってされますが、フォント、影、色、および昇格などの分野のプラットフォーム間でのユーザー インターフェイスの違い。

## <a name="related-links"></a>関連リンク

- [カスタム レンダラー](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
