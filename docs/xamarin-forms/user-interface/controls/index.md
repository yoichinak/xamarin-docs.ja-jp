---
title: コントロールのリファレンス
description: Xamarin. フォームアプリケーションを構築するために使用されるすべてのユーザーインターフェイス要素の説明。 この記事では、Xamarin.Forms アプリケーションのユーザー インターフェイスを構成するコントロールのグループを一覧表示します。
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2019
ms.openlocfilehash: b9436603c17eb8008f470e75a52a93e8e122034f
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488116"
---
# <a name="controls-reference"></a>コントロールのリファレンス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

Xamarin. Forms アプリケーションのユーザーインターフェイスは、各ターゲットプラットフォームのネイティブコントロールにマップされるオブジェクトで構成されます。 これにより、iOS、Android、およびユニバーサル Windows プラットフォーム用のプラットフォーム固有のアプリケーションで、 [.NET Standard ライブラリ](~/cross-platform/app-fundamentals/net-standard.md)に含まれる Xamarin. Forms コードを使用できるようになります。

Xamarin. フォームアプリケーションのユーザーインターフェイスを作成するために使用される4つのコントロールグループは次のとおりです。

- [**ページ**](pages.md)
- [**レイアウト**](layouts.md)
- [**表示モード**](views.md)
- [**セル**](cells.md)

Xamarin.Forms ページは、一般に、画面全体を占有します。 ページには、通常、ビューやその他のレイアウトを含むレイアウトが含まれます。 セルで使用される特殊なコンポーネントは、 [ `TableView` ](views.md#tableview)と[ `ListView`](views.md#listview)します。 Xamarin でユーザーインターフェイスを構築するために通常使用される型の階層を示すクラス図。フォーム[コントロールクラスの階層構造](~/xamarin-forms/internals/class-hierarchy.md)を参照してください。

次の 4 つの記事で[**ページ**](pages.md)、 [**レイアウト**](layouts.md)、 [**ビュー** ](views.md)、および[**セル**](cells.md)、(存在する場合) とその API のドキュメント、記事の使用 (存在する場合、および 1 つまたは複数のサンプル プログラムへのリンク コントロールの種類が説明されています。 また、各種類のコントロールには、iOS および Android デバイスで実行されている[**フォームギャラリー**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)サンプルのページを示すスクリーンショットも付属しています。 次の各スクリーン ショットは c# ページ、同等の XAML ページのソース コードへのリンクと、(適切な場合)、XAML ページの c# 分離コード ファイル。

> [!NOTE]
> ページ、レイアウト、およびビューは、`VisualElement` クラスから派生します。 `VisualElement` クラスには、クラスの派生に役立つさまざまなプロパティ、メソッド、およびイベントが用意されています。 詳細については、「 [Visualelement のプロパティ」、「メソッド」、および「イベント](common-properties.md)」を参照してください。

Xamarin に用意されているコントロールに加えて、サードパーティ製のコントロールも使用できます。 詳細については、「[サードパーティ製のコントロール](thirdparty.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms FormsGallery サンプル](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms Controls クラスの階層構造](~/xamarin-forms/internals/class-hierarchy.md)
- [API ドキュメント](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
