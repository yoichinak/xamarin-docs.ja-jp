---
title: XAML のコンパイル Xamarin.Forms
description: この記事では、xaml コンパイラ (XAMLC) を使用して、必要に応じて、XAML を中間言語 (IL) に直接コンパイルする方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/03/2021
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8d53f80372062f4830d92213110f01d005f92018
ms.sourcegitcommit: 4f274920d1fe906cda0bf83b8e928b3b50147d40
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2021
ms.locfileid: "99509885"
---
# <a name="xaml-compilation-in-xamarinforms"></a>XAML のコンパイル Xamarin.Forms

_XAML は任意で、XAML コンパイラ (XAMLC) を利用し、中間言語 (IL) に直接コンパイルできます。_

XAML のコンパイルには、次のような多くの利点があります。

- XAML のコンパイル時チェックを実行し、エラーがあればユーザーに知らせます。
- XAML 要素の読み込みとインスタンス化の時間を短縮します。
- .xaml ファイルを含めないことで、最終アセンブリのファイル サイズを減らします。

XAML のコンパイルは、フレームワークでは既定で無効になっています。 ただし、新しいプロジェクトのテンプレートでは有効になっています。 属性を追加すること `XamlCompilationOptions.Skip` で、アセンブリレベルとクラスレベルの両方で明示的に有効または無効にすることができます () [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) 。

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

属性はどこにでも配置できますが、 **AssemblyInfo.cs** に配置することをお勧めします。

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
