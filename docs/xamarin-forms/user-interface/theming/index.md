---
title: アプリケーションのテーマを Xamarin.Forms 適用する
description: Xamarin.Formsアプリケーションでは、テーマごとに ResourceDictionary を作成し、DynamicResource マークアップ拡張機能を使用してリソースを読み込むことによって、テーマをサポートしています。
ms.prod: xamarin
ms.assetId: BF92AEDD-EF23-4D08-A972-B089066E75F9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 80660ae7d3af0fe5948a5ae4ffdb35d2f9c2a40f
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136137"
---
# <a name="theming-a-xamarinforms-application"></a>アプリケーションのテーマを Xamarin.Forms 適用する

## <a name="theme-an-application"></a>[アプリケーションのテーマを適用する](theming.md)

Xamarin.Formsテーマは、 [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) 各テーマに対してを作成し、 `DynamicResource` マークアップ拡張機能を使用してリソースを読み込むことによって、アプリケーションに実装できます。

## <a name="respond-to-system-theme-changes"></a>[システム テーマの変更に対応する](system-theme-changes.md)

通常、デバイスには明るいテーマとダークテーマが含まれており、それぞれがオペレーティングシステムレベルで設定できるさまざまな外観設定を参照しています。 アプリケーションはこれらのシステムテーマを尊重し、システムテーマが変更されたときに直ちに応答する必要があります。
