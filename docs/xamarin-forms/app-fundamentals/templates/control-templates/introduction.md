---
title: Xamarin.Forms コントロール テンプレートの概要
description: Xamarin.Forms コントロールのテンプレートは、実行時にアプリケーション ページに簡単にテーマおよび再テーマ設定する機能を提供します。 この記事では、コントロール テンプレートの概要を提供します。
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: 6b7a6c6d9c9c541e1d5e821fc2dac202e98bec62
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/12/2018
ms.locfileid: "38994426"
---
# <a name="introduction-to-xamarinforms-control-templates"></a>Xamarin.Forms コントロール テンプレートの概要

_Xamarin.Forms コントロールのテンプレートは、実行時にアプリケーション ページに簡単にテーマおよび再テーマ設定する機能を提供します。この記事では、コントロール テンプレートの概要を提供します。_

コントロールが別のプロパティをなどある`BackgroundColor`と`TextColor`コントロールの外観の側面を定義することができます。 使用してこれらのプロパティを設定できる[スタイル](~/xamarin-forms/user-interface/styles/index.md)、基本的なテーマを実装するために実行時に変更できます。 ただし、スタイルは、ページの外観とそのコンテンツ間を明確に分離を維持しないし、このようなプロパティを設定して可能な変更は制限されています。

コントロール テンプレートは、ページの外観とテーマが簡単なページの作成を有効にするため、そのコンテンツ間を明確に分離を提供します。 たとえば、アプリケーションでは、ダーク テーマとライト テーマを提供するアプリケーション レベルのコントロール テンプレートを含めることができます。 各[ `ContentPage` ](xref:Xamarin.Forms.ContentPage)アプリケーションでできるテーマが適用された各ページで表示されるコンテンツを変更することがなく、コントロール テンプレートのいずれかを適用することで。 さらに、コントロール テンプレートで提供されるテーマは、コントロールのプロパティの変更に限定されません。 テーマを実装するために使用されているコントロールを変更することもできます。

## <a name="creating-a-controltemplate"></a>ControlTemplate の作成

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)ページまたはビューの外観を指定し、ルート レイアウトが含まれていて、レイアウト、テンプレートを実装するコントロール内で。 通常、`ControlTemplate`は利用、 [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter)ページやビューに表示するコンテンツが表示される場所をマークします。 ページやビューを使用する、`ControlTemplate`によって表示されるコンテンツを次に、定義、`ContentPresenter`します。 次の図は、`ControlTemplate`など、コントロールの番号を含むページの`ContentPresenter`青い四角形でマークします。

![](introduction-images/control-template.png "ページのコントロール テンプレート")

A [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)を設定して、次の種類に適用できる、`ControlTemplate`プロパティ。

- [`ContentPage`](xref:Xamarin.Forms.ContentPage)
- [`ContentView`](xref:Xamarin.Forms.ContentView)
- [`TemplatedPage`](xref:Xamarin.Forms.TemplatedPage)
- [`TemplatedView`](xref:Xamarin.Forms.TemplatedView)

ときに、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)が作成され、割り当てられている、これらの型には、任意の既存の外観はで定義されている外観に置き換えられます、`ControlTemplate`します。 使用して外観を設定するほかにさらに、`ControlTemplate`テーマ機能を展開するプロパティ、コントロール テンプレートは、さらにスタイルを使用しても適用できます。

> [!NOTE]
>  *`TemplatedPage`と`TemplatedView`型でしょうか。* `TemplatedPage` 基本クラスは、 `ContentPage`、Xamarin.Forms によって提供される最も基本的なページの種類と。 異なり`ContentPage`、`TemplatedPage`はありません、`Content`プロパティ。 そのため、コンテンツ直接に追加できません、`TemplatedPage`インスタンス。 代わりに、コンテンツが追加のコントロール テンプレートを設定して、`TemplatedPage`インスタンス。 同様に、`TemplatedView`の基本クラスは、`ContentView`します。 異なり`ContentView`、`TemplatedView`はありません、`Content`プロパティ。 そのため、コンテンツ直接に追加できません、`TemplatedView`インスタンス。 代わりに、コンテンツが追加のコントロール テンプレートを設定して、`TemplatedView`インスタンス。

XAML では c# では、コントロール テンプレートを作成できます。

- XAML で作成したコントロールのテンプレートがで定義されている、 [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)に割り当てられた、 [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources)コレクションページでは、または一般的に、 [ `Resources` ](xref:Xamarin.Forms.Application.Resources)アプリケーションのコレクション。
- ページのクラス、またはグローバルにアクセスできるクラスで、c# で作成したコントロールのテンプレートが通常定義されます。

定義する場所を選択する、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)インスタンスへの影響に使用できます。

- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) ページ レベルで定義されているインスタンスは、ページにのみ適用できます。
- [`ControlTemplate`](xref:Xamarin.Forms.ControlTemplate) アプリケーション レベルで定義されているインスタンスは、アプリケーション全体のページに適用できます。

コントロール テンプレートのビュー階層の下位には、アップ以上定義されているものよりも優先されます。 たとえば、 [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate)という名前の`DarkTheme`で定義されているページ レベルは優先アプリケーション レベルで定義されており、同じ名前のテンプレート。 そのため、アプリケーション内の各ページに適用するテーマを定義するコントロール テンプレートの場合は、アプリケーション レベルで定義されている必要があります。


## <a name="related-links"></a>関連リンク

- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
