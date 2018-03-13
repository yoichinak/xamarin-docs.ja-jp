---
title: "テキスト"
description: "Xamarin.Forms を使用してテキストを入力または表示します。"
ms.topic: article
ms.prod: xamarin
ms.assetid: 4DBA7689-E5C8-4583-8FB4-02AB208B4416
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/22/2017
ms.openlocfilehash: 55d13589e4241e9f4e29aea9a55346a8f514f208
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="text"></a>テキスト

_Xamarin.Forms を使用してテキストを入力または表示します。_

Xamarin.Forms では、テキストを操作するための 3 つの主なビューがあります。

- **[ラベル](#Label)** &mdash; 1 つまたは複数の行のテキストを表示するためです。 同じ行に複数の書式設定オプションを使ってテキストを表示できます。
- **[エントリ](#Entry)** &mdash; 1 行のみであるテキストを入力します。 エントリには、パスワードのモードがあります。
- **[エディター](#Editor)**  &mdash;行以上がかかる可能性があるテキストを入力します。

組み込みまたはユーザー設定を使用してテキストの外観を変更することができます[スタイル](#Styles)一部のコントロールをカスタム サポートと[フォント](#Fonts)です。

<a name="Label" />

## <a name="labellabelmd"></a>[Label](label.md)

`Label`テキストを表示するビューを使用します。 これには、複数行のテキストまたは単一行のテキストを表示できます。 `Label` インラインで使用される複数の書式設定オプションでは、テキストを表示できます。 ラベル表示では、ラップしたり、1 つの行にできない場合、テキストが切り捨てすることができます。

![](images/label.png "ラベルの例")

参照してください、[ラベル](label.md)アーティクルの詳細についてはします。

ラベルで使用するフォントをカスタマイズする方法については、次を参照してください。[フォント](fonts.md)です。

<a name="Entry" />

## <a name="entryentrymd"></a>[エントリ](entry.md)

`Entry` 単一行のテキスト入力の受け入れに使用されます。 `Entry` オファーは、色を制御しますが、フォントをカスタマイズしたことはできません。 `Entry` パスワードのモードを持ち、テキストを入力するまで、プレース ホルダー テキストを表示することができます。

![](images/entry.png "エントリの例")

参照してください、[エントリ](entry.md)詳細については資料です。

なおとは異なり`Label`、`Entry`カスタム フォントの設定を持つことはできません。

<a name="Editor" />

## <a name="editoreditormd"></a>[[エディター]](editor.md)

`Editor` 複数行テキスト入力の受け入れに使用されます。 `Editor` カスタムの背景色がテキストの色を持つことができ、フォントを変更することはできません。

![](images/editor.png "エディターの使用例")

参照してください、[エディター](editor.md)詳細については資料です。

<a name="Fonts" />

## <a name="fontsfontsmd"></a>[フォント](fonts.md)

`Label`コントロールは、各プラットフォームまたはカスタム フォントをアプリに含まれている組み込みのフォントを使用して別のフォント設定をサポートしています。 参照してください、[フォント](fonts.md)アーティクルの詳細についてはします。

<a name="Styles" />

## <a name="stylesstylesmd"></a>[スタイル](styles.md)

参照してください[スタイルを使用する](~/xamarin-forms/user-interface/styles/index.md)フォントを設定する方法を学習する[色](~/xamarin-forms/user-interface/colors.md)、および複数のコントロール全体に適用されるその他の画面のプロパティです。



## <a name="related-links"></a>関連リンク

- [テキスト (サンプル)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
