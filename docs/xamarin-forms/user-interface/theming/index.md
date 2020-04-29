---
title: Xamarin.Forms アプリケーションのテーマ
description: Xamarin アプリケーションは、テーマごとに ResourceDictionary を作成し、DynamicResource マークアップ拡張機能を使用してリソースを読み込むことによって、テーマをサポートします。
ms.prod: xamarin
ms.assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
ms.openlocfilehash: 5988437b40ac875b8b59f9af0f25d4b5c60ded97
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517138"
---
# <a name="theming-a-xamarinforms-application"></a>Xamarin. Forms アプリケーションのテーマ

## <a name="theme-an-application"></a>[アプリケーションのテーマを適用する](theming.md)

テーマは、各テーマに対してを[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)作成した後、 `DynamicResource`マークアップ拡張機能を使用してリソースを読み込むことによって、Xamarin. フォームアプリケーションで実装できます。

## <a name="respond-to-system-theme-changes"></a>[システムテーマの変更に応答する](system-theme-changes.md)

通常、デバイスには明るいテーマとダークテーマが含まれており、それぞれがオペレーティングシステムレベルで設定できるさまざまな外観設定を参照しています。 アプリケーションはこれらのシステムテーマを尊重し、システムテーマが変更されたときに直ちに応答する必要があります。
