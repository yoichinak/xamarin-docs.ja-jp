---
title: Xamarin.Forms シェルのライフサイクル
description: シェル アプリケーションでは Xamarin.Forms のライフサイクルが尊重され、ページが画面に表示されるときに Appearing イベントが発生し、ページが画面から消えるときに Disappearing イベントが発生します。
ms.prod: xamarin
ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2019
ms.openlocfilehash: 2ed51763b5866c15e91d88a6a1a58c7285fb5973
ms.sourcegitcommit: e71474f91639bb43159b22f5d534325c3270ba93
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/22/2019
ms.locfileid: "72749767"
---
# <a name="xamarinforms-shell-lifecycle"></a>Xamarin.Forms シェルのライフサイクル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

シェル アプリケーションでは Xamarin.Forms のライフサイクルが尊重され、ページが画面に表示されるときに `Appearing` イベントが発生し、ページが画面から消えるときに `Disappearing` イベントが発生します。 これらのイベントはページに反映され、ページの [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) メソッドまたは [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) メソッドをオーバーライドすることで処理できます。

> [!NOTE]
> シェル アプリケーションでは、`Appearing` イベントと `Disappearing` イベントは、プラットフォーム コードによってページを表示される、またはページが画面から削除される前に、クロスプラットフォーム コードから生成されます。

Xamarin.Forms アプリのライフサイクルの詳細については、「[Xamarin.Forms App Lifecycle](~/xamarin-forms/app-fundamentals/app-lifecycle.md)」 (Xamarin.Forms アプリのライフサイクル) を参照してください。

## <a name="hierarchical-navigation"></a>階層ナビゲーション

シェル アプリケーションでは、ページをナビゲーション スタックにプッシュすると、現在表示されている `ShellContent` オブジェクトとそのページ コンテンツが生成され、`Disappearing` イベントが発生します。 同様に、ナビゲーション スタックから最後のページがポップされると、新しく表示される `ShellContent` オブジェクトとそのページ コンテンツが生成され、`Appearing` イベントが発生します。

階層ナビゲーションの詳細については、[Xamarin.Forms の階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)に関するページを参照してください。

## <a name="modal-navigation"></a>モーダル ナビゲーション

シェル アプリケーションでは、モーダル ページをモーダル ナビゲーション スタックにプッシュすると、すべての可視シェル オブジェクトが生成され、`Disappearing` イベントが発生します。 同様に、モーダル ナビゲーション スタックから最後のモーダル ページがポップされると、すべての可視シェル オブジェクトが生成され、`Appearing` イベントが発生します。

モーダル ナビゲーションの詳細については、「[Xamarin.Forms モーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms アプリのライフサイクル](~/xamarin-forms/app-fundamentals/app-lifecycle.md)
- [Xamarin.Forms モーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)
