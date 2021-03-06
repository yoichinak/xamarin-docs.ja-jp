---
title: Xamarin.Forms の DependencyService
description: Xamarin.Forms の DependencyService クラスは、Xamarin.Forms アプリケーションで共有コードからネイティブ プラットフォームの機能を起動できるようにするサービス ロケーターです。
ms.prod: xamarin
ms.assetid: 403479F2-6751-41F2-ADCE-3AF595062FE4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/05/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 126e2d2373bad923fe1d66fe355ad811c15fbe4f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138373"
---
# <a name="xamarinforms-dependencyservice"></a>Xamarin.Forms の DependencyService

## <a name="introduction"></a>[はじめに](introduction.md)

[`DependencyService`](xref:Xamarin.Forms.DependencyService) クラスは、Xamarin.Forms アプリケーションで共有コードからネイティブ プラットフォームの機能を起動できるようにするサービス ロケーターです。

## <a name="registration-and-resolution"></a>[登録と解決](registration-and-resolution.md)

プラットフォームの実装を [`DependencyService`](xref:Xamarin.Forms.DependencyService) に登録し、共有コードから解決してそれらを呼び出す必要があります。

## <a name="picking-a-photo-from-the-library"></a>[ライブラリから写真を選択する](photo-picker.md)

この記事では、Xamarin.Forms の [`DependencyService`](xref:Xamarin.Forms.DependencyService) クラスを使用して、電話の画像ライブラリから写真を選択する方法について説明します。
