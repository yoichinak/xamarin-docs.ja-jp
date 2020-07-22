---
title: XAML のコンパイルXamarin.Forms
description: この記事では、xaml コンパイラ (XAMLC) を使用して、必要に応じて、XAML を中間言語 (IL) に直接コンパイルする方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: eebbb3040175118320639bcb4482ec77b5c16ac7
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137294"
---
# <a name="xaml-compilation-in-xamarinforms"></a>XAML のコンパイルXamarin.Forms

_XAML は任意で、XAML コンパイラ (XAMLC) を利用し、中間言語 (IL) に直接コンパイルできます。_

XAML のコンパイルには、次のような多くの利点があります。

- XAML のコンパイル時チェックを実行し、エラーがあればユーザーに知らせます。
- XAML 要素の読み込みとインスタンス化の時間を短縮します。
- .xaml ファイルを含めないことで、最終アセンブリのファイル サイズを減らします。

XAML のコンパイルは、下位互換性を確保するために既定で無効になっています。 属性を追加することによって、アセンブリレベルとクラスレベルの両方で有効にすることができ [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) ます。

アセンブリレベルで XAML コンパイルを有効にするコード例を次に示します。

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

この例では、アセンブリに含まれるすべての XAML のコンパイル時チェックが実行されます。 XAML エラーは、実行時ではなくコンパイル時に報告されます。 したがって、 `assembly` 属性のプレフィックスは、 [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 属性がアセンブリ全体に適用されることを指定します。

> [!NOTE]
> [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)属性と [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) 列挙体は名前空間に存在し、 `Xamarin.Forms.Xaml` それらを使用するためにインポートする必要があります。

次のコード例は、クラスレベルで XAML コンパイルを有効にする方法を示しています。

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

この例では、クラスの XAML のコンパイル時チェック `HomePage` が実行され、コンパイルプロセスの一部としてエラーが報告されます。

> [!NOTE]
> コンパイル済みバインディングは、アプリケーションでのデータバインディングのパフォーマンスを向上させるために有効にすることができ Xamarin.Forms ます。 詳しくは、「[コンパイル済みのバインド](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
