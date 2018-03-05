---
title: "はじめに"
description: "Xamarin.Forms コントロール テンプレートでは、実行時にアプリケーション ページを簡単にテーマおよび re テーマする機能を提供します。 この記事では、コントロール テンプレートの概要を提供します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 8B8E2360-6531-44A3-A7C8-9A8808DE9B86
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: c3973b94168706e047ee5c312a8823503c9b0fd8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/27/2018
---
# <a name="introduction"></a>はじめに

_Xamarin.Forms コントロール テンプレートでは、実行時にアプリケーション ページを簡単にテーマおよび re テーマする機能を提供します。この記事では、コントロール テンプレートの概要を提供します。_

コントロールなど、別のプロパティがあります`BackgroundColor`と`TextColor`コントロールの外観の側面を定義することができます。 使用してこれらのプロパティを設定できる[スタイル](~/xamarin-forms/user-interface/styles/index.md)、基本的なテーマを実装する実行時に変更できます。 ただし、スタイルがページの外観とそのコンテンツ間で明確に分離を維持しないし、このようなプロパティを設定して実行できる変更は制限されます。

コントロールのテンプレートは、ページの外観とそのコンテンツは、そのため、テーマを簡単に付けるページの作成の有効化の間で明確に分離を提供します。 たとえば、アプリケーションには、濃色のテーマと light テーマを提供するテンプレートをアプリケーション レベルの制御があります。 各[ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)アプリケーションにテーマを付ける各ページで表示されているコンテンツを変更することがなく、コントロール テンプレートのいずれかを適用することで。 さらに、コントロール テンプレートで提供されるテーマは、コントロールのプロパティの変更に制限はありません。 テーマを実装するために使用するコントロールを変更することもできます。

## <a name="creating-a-controltemplate"></a>ControlTemplate の作成

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)ページまたはビューの外観を指定し、ルート レイアウトが含まれていますとテンプレートを実装するコントロールがレイアウト内で。 通常、`ControlTemplate`は利用、 [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)ページまたはビューに表示するコンテンツが表示される場所をマークします。 ページまたはビューを使用する、`ControlTemplate`によって表示されるコンテンツを次に、定義、`ContentPresenter`です。 次の図は、`ControlTemplate`など、コントロールの数値を含んだページの`ContentPresenter`青い四角形でマークします。

![](introduction-images/control-template.png "ページのコントロール テンプレート")

A [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)設定で、次の種類に適用できる、`ControlTemplate`プロパティ。

- [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/)
- [`ContentView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [`TemplatedPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedPage/)
- [`TemplatedView`](https://developer.xamarin.com/api/type/Xamarin.Forms.TemplatedView/)

ときに、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)が作成され、割り当てられているこれらの型への既存の外観が置き換えられますで定義されている外観、`ControlTemplate`です。 使用して外観を設定およびさらに、`ControlTemplate`テーマ機能を展開するプロパティ、コントロールのテンプレートは、さらにスタイルを使用しても適用できます。

> [!NOTE]
>  *`TemplatedPage`と`TemplatedView`型しますか?* `TemplatedPage` 基本クラスは、 `ContentPage`Xamarin.Forms で提供される最も基本的なページの種類とします。 異なり`ContentPage`、`TemplatedPage`はありません、`Content`プロパティです。 そのため、コンテンツ直接に追加できません、`TemplatedPage`インスタンス。 コントロール テンプレートを設定してコンテンツを追加する代わりに、`TemplatedPage`インスタンス。 同様に、`TemplatedView`の基本クラスは、`ContentView`です。 異なり`ContentView`、`TemplatedView`はありません、`Content`プロパティです。 そのため、コンテンツ直接に追加できません、`TemplatedView`インスタンス。 コントロール テンプレートを設定してコンテンツを追加する代わりに、`TemplatedView`インスタンス。

XAML および C# の場合、コントロール テンプレートを作成できます。

- XAML で作成したコントロールのテンプレートがで定義されている、 [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)に割り当てられている、 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/)のコレクションをページまたは一般的に、 [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Resources/) 、アプリケーションのコレクション。
- C# で作成したコントロールのテンプレートには、ページのクラス、またはグローバルにアクセス可能なクラスで通常定義されます。

定義する場所を選択する、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)インスタンスへの影響が使用できます。

- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) ページ レベルで定義されているインスタンスは、ページにのみ適用できます。
- [`ControlTemplate`](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) アプリケーション レベルで定義されているインスタンスは、アプリケーション全体のページに適用できます。

コントロール テンプレート ビュー階層の下位をそれ以上定義されているものよりも優先されます。 たとえば、 [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)という`DarkTheme`で定義されるページ レベルは優先アプリケーション レベルで定義されている同じ名前のテンプレートです。 そのため、アプリケーション レベルでアプリケーション内の各ページに適用するテーマを定義するコントロール テンプレートを定義する必要があります。


## <a name="related-links"></a>関連リンク

- [スタイル](~/xamarin-forms/user-interface/styles/index.md)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
