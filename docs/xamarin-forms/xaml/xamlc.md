---
title: Xamarin.Forms の XAML のコンパイル
description: この記事では、どの XAML コンパイルできます Xamarin.Forms XAML コンパイラ (XAMLC) で中間言語 (IL) に直接について説明します。
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 07/02/2018
ms.openlocfilehash: 4635857fc850b2985f988f8c20ff854e487f79ed
ms.sourcegitcommit: 081a2d094774c6f75437d28b71d22607e33aae71
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37403404"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Xamarin.Forms の XAML のコンパイル

_XAML は、必要に応じて、XAML コンパイラ (XAMLC) で中間言語 (IL) に直接コンパイルできます。_

XAMLC にはさまざまな長所があります。

- XAML のコンパイル時チェックを実行し、エラーがあればユーザーに知らせます。
- XAML 要素の読み込みとインスタンス化の時間を短縮します。
- .xaml ファイルを含めないことで、最終アセンブリのファイル サイズを減らします。

XAMLC は下位互換性のために既定で無効になっています。 追加することで、アセンブリとクラス レベルで有効にする、`XamlCompilation`属性。

次のコード例では、アセンブリ レベルで XAMLC の有効化を示しています。

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

この例では、コンパイル時に、アセンブリ内に含まれるすべての XAML のチェックが実行され、実行時ではなく、コンパイル時に報告されている XAML エラー。 そのため、`assembly`プレフィックス、`XamlCompilation`属性は、属性がアセンブリ全体に適用されることを指定します。

次のコード例では、クラス レベルで XAMLC の有効化を示しています。

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

この例では、コンパイル時に XAML のチェックでは、`HomePage`クラスが実行され、エラーは、コンパイル プロセスの一部として報告します。

> [!NOTE]
> `XamlCompilation`属性と`XamlCompilationOptions`に列挙型が存在する、`Xamarin.Forms.Xaml`名前空間は、それらを使用するインポートする必要があります。


## <a name="related-links"></a>関連リンク

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
