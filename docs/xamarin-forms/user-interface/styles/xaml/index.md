---
title: XAML スタイルを使用して Xamarin.Forms アプリのスタイル設定
description: このガイドでは、XAML スタイルを使用して、Xamarin.Forms アプリケーションの外観をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 2607298bdc0842f60a1d1a3299bed61bbea925a1
ms.sourcegitcommit: 2f6a5c1abf90fbdb0475fd8a3ce6de3cd7c7d575
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2018
ms.locfileid: "52459864"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>XAML スタイルを使用して Xamarin.Forms アプリのスタイル設定

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

Xamarin.Forms アプリケーションには、複数のコントロール同一の外観を持つには多くの場合が含まれます。 個々 のコントロールの外観の設定は繰り返し発生することがあります、エラーが発生します。 代わりに、スタイルを作成できますで、コントロールの外観をカスタマイズするには、グループ化とコントロールの種類に使用可能なプロパティを設定します。

## <a name="explicit-stylesexplicitmd"></a>[明示的なスタイル](explicit.md)

*明示的な*スタイルがコントロールに設定して選択的に適用される 1 つ、 [ `Style` ](xref:Xamarin.Forms.VisualElement.Style)プロパティ。

## <a name="implicit-stylesimplicitmd"></a>[暗黙的なスタイル](implicit.md)

*暗黙的な*スタイルは、同一のすべてのコントロールで使用される 1 つ[ `TargetType`](xref:Xamarin.Forms.Style.TargetType)スタイルを参照するには、各コントロールを必要とせずします。

## <a name="global-stylesapplicationmd"></a>[グローバル スタイル](application.md)

スタイルが利用できるグローバルに、アプリケーションに追加することによって[ `ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)します。 これは、ページまたはコントロールの間でスタイルの重複を回避するのに役立ちます。

## <a name="style-inheritanceinheritancemd"></a>[スタイルの継承](inheritance.md)

スタイルは、重複を削減し、再利用を有効にするには、その他のスタイルを継承できます。

## <a name="dynamic-stylesdynamicmd"></a>[動的なスタイル](dynamic.md)

スタイルはないプロパティの変更に応答し、アプリケーションの実行中は変更されません。 ただし、アプリケーションは、動的リソースを使用して実行時に動的にスタイルの変更に応答することができます。

## <a name="device-stylesdevicemd"></a>[デバイスのスタイル](device.md)

Xamarin.Forms には、6 が含まれます*動的*と呼ばれるスタイル*デバイス*スタイル設定で、 [ `Devices.Styles` ](xref:Xamarin.Forms.Device.Styles)クラス。 6 つのすべてのスタイルを適用できる[ `Label` ](xref:Xamarin.Forms.Label)インスタンスのみです。
