---
title: "XAML のコンパイル"
description: "XAML は任意で、XAML コンパイラ (XAMLC) を利用し、中間言語 (IL) に直接コンパイルできます。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: c6fb404919621e1b22217b4461597ae07a5624c4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
---
# <a name="xaml-compilation"></a>XAML のコンパイル

_XAML は、XAML コンパイラ (XAMLC) で中間言語 (IL) に直接オプションでコンパイルすることができます。_

XAMLC にはさまざまな長所があります。

- XAML のコンパイル時チェックを実行し、エラーがあればユーザーに知らせます。
- XAML 要素の読み込みとインスタンス化の時間を短縮します。
- .xaml ファイルを含めないことで、最終アセンブリのファイル サイズを減らします。

XAMLC は下位互換性のために既定で無効になっています。 追加することによって、アセンブリとクラスの両方のレベルで有効にする、`XamlCompilation`属性。

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

この例では、すべての XAML のコンパイル時のチェックに含まれる、`PhotoApp`名前空間が実行されて、実行時ではなく、コンパイル時に報告されている XAML エラーが発生します。
`assembly`プレフィックスに、`XamlCompilation`属性は、属性がアセンブリ全体に適用されることを指定します。

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

この例では、コンパイル時の XAML のチェックでは、`HomePage`クラスが実行され、エラー、コンパイル処理の一部として報告します。

> [!NOTE]
> `XamlCompilation`属性および`XamlCompilationOptions`に列挙型が存在する、`Xamarin.Forms.Xaml0`名前空間は、それらを使用してにインポートする必要があります。


## <a name="related-links"></a>関連リンク

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
