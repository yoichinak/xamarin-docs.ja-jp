---
title: Xamarin.Forms のコントロール テンプレートの概要
description: Xamarin.Forms のコントロール テンプレートには、実行時にアプリケーション ページを簡単にテーマ設定および再テーマ設定する機能が備わっています。 この記事では、コントロール テンプレートの概要を説明します。
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 70646999154297592137c6966626b318fb73897c
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/21/2019
ms.locfileid: "70771267"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Xamarin.Forms のコントロール テンプレートの概要

_Xamarin.Forms のコントロール テンプレートには、実行時にアプリケーション ページを簡単にテーマ設定および再テーマ設定する機能が備わっています。この記事では、コントロール テンプレートの概要を説明します。_

コントロールには、`BackgroundColor` や `TextColor` などのさまざまなプロパティがあり、コントロールの外観の側面を定義できます。 これらのプロパティは、[スタイル](~/xamarin-forms/user-interface/styles/index.md)を使用して設定できます。スタイルは、実行時に変更して基本的なテーマを実装できます。 ただし、スタイルでは、ページの外観とそのコンテンツが明確に分離されておらず、そのようなプロパティを設定して加えることができる変更は限られています。

コントロール テンプレートを使うとページの外観とそのコンテンツを明確に分離することができるので、テーマを設定しやすいページを作成できます。 たとえば、アプリケーションには、ダーク テーマとライト テーマを提供するアプリケーションレベルのコントロール テンプレートを含めることができます。 アプリケーションの各 [`ContentPage`](xref:Xamarin.Forms.ContentPage) には、各ページに表示されているコンテンツを変更することなく、コントロール テンプレートのいずれかを適用することでテーマを設定できます。 さらに、コントロール テンプレートに提供されているテーマは、コントロールのプロパティの変更に限定されていません。 テーマを実装するために使用されるコントロールを変更することもできます。

## <a name="creating-a-controltemplate"></a>ControlTemplate の作成

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) にはページまたはビューの外観が指定され、ルート レイアウトが含まれています。また、レイアウト内には、テンプレートを実装するコントロールが含まれています。 通常、`ControlTemplate` では、ページまたはビューによって表示されるコンテンツが表示される場所をマークするために、[`ContentPresenter`](xref:Xamarin.Forms.ContentPresenter) が利用されます。 `ControlTemplate` を使用するページまたはビューでは、`ContentPresenter` によって表示されるコンテンツを定義します。 次の図は、青い四角形でマークされた `ContentPresenter` を含め、多数のコントロールがあるページの `ControlTemplate` を示しています。

![](introduction-images/control-template.png "Control Template for a Page")

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) を以下の種類に適用するには、その `ControlTemplate` プロパティを設定します。

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) が作成され、このような種類に割り当てられると、既存のすべての外観は `ControlTemplate` に定義されている外観に置き換えられます。 さらに、`ControlTemplate` プロパティを使用して外観を設定するだけでなく、テーマの機能をさらに拡張するスタイルを使用して、コントロール テンプレートを適用することもできます。

> [!NOTE]
> *`TemplatedPage` と `TemplatedView` の種類とは* `TemplatedPage` は `ContentPage` の基底クラスであり、Xamarin.Forms に用意されている最も基本的なページの種類です。 `ContentPage` とは異なり、`TemplatedPage` には `Content` プロパティがありません。 そのため、コンテンツを `TemplatedPage` インスタンスに直接追加することはできません。 代わりに、`TemplatedPage` インスタンスのコントロール テンプレートを設定してコンテンツを追加します。 同様に、`TemplatedView` は `ContentView` の基底クラスです。 `ContentView` とは異なり、`TemplatedView` には `Content` プロパティがありません。 そのため、コンテンツを `TemplatedView` インスタンスに直接追加することはできません。 代わりに、`TemplatedView` インスタンスのコントロール テンプレートを設定してコンテンツを追加します。

コントロール テンプレートは、XAML と C# で作成できます。

- XAML で作成されたコントロール テンプレートは、[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) で定義されます。これは、ページの [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) コレクション、さらに一般的にはアプリケーションの [`Resources`](xref:Xamarin.Forms.Application.Resources) コレクションに割り当てられます。
- C# で作成されたコントロール テンプレートは、通常、ページのクラス、またはグローバルにアクセスできるクラスで定義されます。

[`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) インスタンスを定義する場所の選択は、使用できる場所に影響があります。

- ページレベルで定義された [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) インスタンスは、そのページにのみ適用できます。
- アプリケーションレベルで定義された [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) インスタンスは、アプリケーション全体のページに適用できます。

ビュー階層で下位にあるコントロール テンプレートは、上位の定義済みコントロール テンプレートよりも優先されます。 たとえば、ページレベルで定義されている `DarkTheme` という [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) は、アプリケーションレベルで定義されている同じ名前のテンプレートよりも優先されます。 そのため、アプリケーションの各ページに適用されるテーマを定義するコントロール テンプレートは、アプリケーションレベルで定義する必要があります。

## <a name="related-links"></a>関連リンク

- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
