---
title: テキストの入力Xamarin.Forms
description: Xamarin.Formsには、テキストを操作するための3つの主要なビューがあります。この記事では、アプリケーションにテキストを入力して表示する方法について説明し Xamarin.Forms ます。
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/26/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b44a0b3e3542638874ee366a86967d73c0d53652
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/18/2020
ms.locfileid: "84573795"
---
# <a name="text-in-xamarinforms"></a>テキストの入力Xamarin.Forms

[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_を使用して Xamarin.Forms テキストを入力または表示します。_

Xamarin.Formsには、テキストを操作するための3つの主なビューがあります。

- **[ラベル](#label)** &mdash;単一行または複数行のテキストを表示します。 複数の書式設定オプションを含むテキストを同じ行に表示できます。
- **[エントリ](#entry)** &mdash;1行のみのテキストを入力する場合。 エントリにはパスワードモードがあります。
- **[エディター](#editor)** &mdash;複数の行を受け取る可能性のあるテキストを入力する場合。

テキストの外観は、組み込みスタイルまたはカスタム[スタイル](#styles)を使用して変更できます。また、カスタム[フォント](#fonts)をサポートするコントロールもあります。

## <a name="label"></a>[Label](label.md)

`Label`ビューはテキストの表示に使用されます。 複数行のテキストまたは1行のテキストを表示できます。 `Label`インラインで使用される複数の書式設定オプションを使用してテキストを表示できます。 ラベルビューでは、1行に収まらないときにテキストを折り返すことができます。

![ラベルの例](images/label.png)

詳細については、[ラベル](label.md)に関する記事をご覧ください。

ラベルで使用されるフォントをカスタマイズする方法については、「[フォント](fonts.md)」を参照してください。

## <a name="entry"></a>[エントリ](entry.md)

`Entry`は、単一行のテキスト入力を許可するために使用されます。 `Entry`色とフォントに対する制御を提供します。 `Entry`にはパスワードモードがあり、テキストが入力されるまでプレースホルダーテキストを表示できます。

![エントリの例](images/entry.png)

詳細については、「[エントリ](entry.md)」を参照してください。

とは異なり、 `Label` は `Entry` カスタムフォント設定を持つことができません。

## <a name="editor"></a>[[エディター]](editor.md)

`Editor`は、複数行のテキスト入力を許可するために使用されます。 `Editor`色とフォントに対する制御を提供します。

![エディターの例](images/editor.png)

詳細については、[エディター](editor.md)の記事を参照してください。

## <a name="fonts"></a>[フォント](fonts.md)

多くのコントロールは、各プラットフォームで組み込みフォントを使用するか、アプリに含まれるカスタムフォントを使用して、さまざまなフォント設定をサポートしています。 詳細については、「[フォント](fonts.md)」を参照してください。

## <a name="styles"></a>[スタイル](styles.md)

複数のコントロールに適用されるフォント、[色](~/xamarin-forms/user-interface/colors.md)、その他の表示プロパティを設定する方法については、「[スタイルの操作](~/xamarin-forms/user-interface/styles/index.md)」を参照してください。

## <a name="related-links"></a>関連リンク

- [Text (サンプル)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
