---
title: Xamarin.Forms シェルのライフサイクル
description: シェル アプリケーションでは Xamarin.Forms のライフサイクルが尊重され、ページが画面に表示されようとしているときに Appearing イベントが発生し、ページが画面から消えようとしていときに Disappearing イベントが発生します。
ms.prod: xamarin
ms.assetid: 4E4EE50E-3BB4-441D-8355-CD9CD26ED1D0
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/25/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 1b2abf7925eabb79b1918a9b9fedb0ba7ecced38
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/30/2020
ms.locfileid: "91563160"
---
# <a name="no-locxamarinforms-shell-lifecycle"></a>Xamarin.Forms シェルのライフサイクル

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)

シェル アプリケーションでは Xamarin.Forms のライフサイクルが尊重され、ページが画面に表示されようとしているときに `Appearing` イベントが発生し、ページが画面から消えようとしているときに `Disappearing` イベントが発生します。 これらのイベントはページに反映され、ページの [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) メソッドまたは [`OnDisappearing`](xref:Xamarin.Forms.Page.OnDisappearing) メソッドをオーバーライドすることで処理できます。

> [!NOTE]
> シェル アプリケーションでは、`Appearing` イベントと `Disappearing` イベントは、プラットフォーム コードによってページを表示される、またはページが画面から削除される前に、クロスプラットフォーム コードから生成されます。

Xamarin.Forms アプリのライフサイクルの詳細については、「[Xamarin.Forms アプリのライフサイクル](~/xamarin-forms/app-fundamentals/app-lifecycle.md)」を参照してください。

## <a name="hierarchical-navigation"></a>階層ナビゲーション

シェル アプリケーションでは、ページをナビゲーション スタックにプッシュすると、現在表示されている `ShellContent` オブジェクトとそのページ コンテンツが生成され、`Disappearing` イベントが発生します。 同様に、ナビゲーション スタックから最後のページがポップされると、新しく表示される `ShellContent` オブジェクトとそのページ コンテンツが生成され、`Appearing` イベントが発生します。

階層ナビゲーションの詳細については、[Xamarin.Forms の階層ナビゲーション](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md)に関するページを参照してください。

## <a name="modal-navigation"></a>モーダル ナビゲーション

シェル アプリケーションでは、モーダル ページをモーダル ナビゲーション スタックにプッシュすると、すべての可視シェル オブジェクトが生成され、`Disappearing` イベントが発生します。 同様に、モーダル ナビゲーション スタックから最後のモーダル ページがポップされると、すべての可視シェル オブジェクトが生成され、`Appearing` イベントが発生します。

モーダル ナビゲーションの詳細については、「[Xamarin.Forms のモーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Xaminals (サンプル)](/samples/xamarin/xamarin-forms-samples/userinterface-xaminals/)
- [Xamarin.Forms アプリのライフサイクル](~/xamarin-forms/app-fundamentals/app-lifecycle.md)
- [Xamarin.Forms のモーダル ページ](~/xamarin-forms/app-fundamentals/navigation/modal.md)