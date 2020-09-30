---
title: コントロールのリファレンス
description: アプリケーションの構築に使用されるすべてのユーザーインターフェイス要素の説明 Xamarin.Forms 。 この記事では、アプリケーションのユーザーインターフェイスを構成するコントロールグループの一覧を示し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: F2A02DEE-7137-42F4-9C0A-4E1CF75EA08F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 60c58ee17f68a12e3e51170f143adc9dc204e275
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91562770"
---
# <a name="controls-reference"></a>コントロールのリファレンス

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

アプリケーションのユーザーインターフェイス Xamarin.Forms は、各ターゲットプラットフォームのネイティブコントロールにマップされるオブジェクトで構成されます。 これにより、iOS、Android、およびユニバーサル Windows プラットフォーム用のプラットフォーム固有のアプリケーションで、 Xamarin.Forms [.NET Standard ライブラリ](~/cross-platform/app-fundamentals/net-standard.md)に含まれるコードを使用できるようになります。

アプリケーションのユーザーインターフェイスを作成するために使用される4つの主要なコントロールグループは次のとおりです Xamarin.Forms 。

- [**Pages**](pages.md)
- [**レイアウト**](layouts.md)
- [**Views**](views.md)
- [**セル**](cells.md)

Xamarin.Formsページは通常、画面全体を占めます。 このページには通常、ビューやその他のレイアウトを含むレイアウトが含まれています。 セルは、およびとの接続に使用される特殊なコンポーネントです [`TableView`](xref:Xamarin.Forms.TableView) [`ListView`](xref:Xamarin.Forms.ListView) 。 でユーザーインターフェイスを構築するために通常使用される型の階層を示すクラス図は、[ Xamarin.Forms [ Xamarin.Forms コントロールクラスの階層構造](~/xamarin-forms/internals/class-hierarchy.md)で見つかります。

[**ページ**](pages.md)、[**レイアウト**](layouts.md)、[**ビュー**](views.md)、および[**セル**](cells.md)に関する4つの記事では、各種類のコントロールについて、API ドキュメントへのリンク、使用方法を説明する記事 (存在する場合)、および1つ以上のサンプルプログラム (存在する場合) が記述されています。 また、各種類のコントロールには、iOS および Android デバイスで実行されている [**フォームギャラリー**](/samples/xamarin/xamarin-forms-samples/formsgallery) サンプルのページを示すスクリーンショットも付属しています。 各スクリーンショットの下には、C# ページのソースコード、同等の XAML ページ、XAML ページの c# 分離コードファイルへのリンクがあります。

> [!NOTE]
> ページ、レイアウト、およびビューは、クラスから派生し `VisualElement` ます。 クラスには、 `VisualElement` クラスの派生に役立つさまざまなプロパティ、メソッド、およびイベントが用意されています。 詳細については、「 [Visualelement のプロパティ」、「メソッド」、および「イベント](common-properties.md)」を参照してください。

に用意されているコントロールに加えて Xamarin.Forms 、サードパーティのコントロールを使用できます。 詳細については、「 [サードパーティ製のコントロール](thirdparty.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xamarin.Forms フォームギャラリーのサンプル](/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.Forms コントロールクラスの階層構造](~/xamarin-forms/internals/class-hierarchy.md)
- [API ドキュメント](/dotnet/api/xamarin.forms?view=xamarin-forms)