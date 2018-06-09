---
title: XAML のスタイルを使用して Xamarin.Forms アプリのスタイルを設定
description: このガイドでは、XAML のスタイルを使用して、Xamarin.Forms アプリケーションの外観をカスタマイズする方法について説明します。
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f439e3ba888b67ac1752eae95149adcf55055943
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245873"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>XAML のスタイルを使用して Xamarin.Forms アプリのスタイルを設定

## <a name="introductionintroductionmd"></a>[はじめに](introduction.md)

Xamarin.Forms アプリケーションは、多くの場合と同じ外観を持つ複数のコントロールを含んでいます。 繰り返し発生することができます個々 のコントロールの外観を設定し、エラーが発生します。 代わりに、スタイルを作成できますコントロールの外観をカスタマイズしてグループ化と設定のプロパティ、コントロール型で使用できます。

## <a name="explicit-stylesexplicitmd"></a>[明示的なスタイル](explicit.md)

*明示的な*スタイルは、1 つを設定して、コントロールに適用される選択的にその[ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/)プロパティです。

## <a name="implicit-stylesimplicitmd"></a>[暗黙的なスタイル](implicit.md)

*暗黙的な*スタイルは、同一のすべてのコントロールで使用される[ `TargetType`](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/)スタイルを参照するには、各コントロールを必要とせずします。

## <a name="global-stylesapplicationmd"></a>[グローバル スタイル](application.md)

スタイルが利用できるグローバルにそれらをアプリケーションの追加[ `ResourceDictionary`](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)です。 これにより、ページまたはコントロールにわたってスタイルの重複を回避します。

## <a name="style-inheritanceinheritancemd"></a>[スタイルの継承](inheritance.md)

スタイルは、重複を減らすし、再利用できるようにするには、他のスタイルを継承できます。

## <a name="dynamic-stylesdynamicmd"></a>[動的なスタイル](dynamic.md)

せずスタイルにプロパティの変更に応答して、アプリケーションの実行中は変更されません。 ただし、アプリケーションは、動的なリソースを使用して、実行時に動的にスタイルの変更に応答できます。

## <a name="device-stylesdevicemd"></a>[デバイスのスタイル](device.md)

Xamarin.Forms では、6 つ含まれています*動的*スタイルと呼ばれる*デバイス*スタイルで、 [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/)クラスです。 6 つのすべてのスタイルを適用できる[ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/)インスタンスのみです。
