---
title: Xamarin.Forms プログレス
description: Xamarin.FormsProgressBar は、フロートプロパティに基づいて塗りつぶされる水平バーとして進行状況を視覚的に表すコントロールです。
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 824851c1406dfefb5f276be069f92040d03a5c98
ms.sourcegitcommit: ebdc016b3ec0b06915170d0cbbd9e0e2469763b9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93370521"
---
# <a name="no-locxamarinforms-progressbar"></a>Xamarin.Forms プログレス
[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

このコントロールは、 Xamarin.Forms [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) 進行状況を、値によって表されるパーセントに設定された水平バーとして視覚的に表し `float` ます。 クラスは、 `ProgressBar` から継承さ [`View`](xref:Xamarin.Forms.View) れます。

次のスクリーンショットは、iOS と Android での `ProgressBar` を示しています。

![IOS と Android の ProgressBar のスクリーンショット](progressbar-images/progressbars-default.png "IOS と Android の ProgressBar")

コントロールは、 `ProgressBar` 次の2つのプロパティを定義します。

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)`float`現在の進行状況を 0 ~ 1 の値として表す値を指定します。 `Progress` 0未満の値は0に固定され、1より大きい値は1にクランプされます。
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor) は、 `Color` 現在の進行状況を表す内部バーの色に影響を与えるです。

これらのプロパティは、オブジェクトによって支えられています [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) 。つまり、を `ProgressBar` スタイル設定し、データバインディングのターゲットにすることができます。

`ProgressBar`また、コントロールは、 `ProgressTo` バーを現在の値から指定された値にアニメーション化するメソッドも定義します。 詳細については、「 [ProgressBar をアニメーション化する](#animate-a-progressbar)」を参照してください。

> [!NOTE]
> は、 `ProgressBar` Tab キーを使用してコントロールを選択したときにスキップされるように、ユーザー操作を受け入れません。

## <a name="create-a-progressbar"></a>ProgressBar を作成する

は、 `ProgressBar` XAML でインスタンス化できます。 この `Progress` プロパティは、内部の色分けされたバーの塗りつぶしの割合を決定します。 既定の `Progress` プロパティ値は0です。 次の例は、 `ProgressBar` 省略可能なプロパティセットを使用して、XAML でをインスタンス化する方法を示してい `Progress` ます。

```xaml
<ProgressBar Progress="0.5" />
```

は、 `ProgressBar` コードで作成することもできます。

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> 、、などの制約のない水平レイアウトオプションは `Center` 、では使用しないで `Start` `End` `ProgressBar` ください。 UWP では、は、 `ProgressBar` 幅が0のバーに折りたたまれます。 `HorizontalOptions`レイアウトにを配置する場合は、の既定値をそのままに `Fill` して、の幅を使用しないようにし `Auto` `ProgressBar` `Grid` ます。

## <a name="progressbar-appearance-properties"></a>ProgressBar の外観のプロパティ

プロパティは、 `ProgressColor` `Progress` プロパティが0より大きい場合に、内部バーの色を定義します。 次の例は、プロパティセットを使用して、XAML でをインスタンス化する方法を示してい `ProgressBar` `ProgressColor` ます。

```xaml
<ProgressBar ProgressColor="Orange" />
```

`ProgressColor`プロパティは、コードでを作成するときに設定することもでき `ProgressBar` ます。

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

次のスクリーンショットは、 `ProgressBar` `ProgressColor` IOS および Android でプロパティがに設定されているを示してい `Color.Orange` ます。

![IOS と Android のスタイルバーのスクリーンショット](progressbar-images/progressbars-styled.png "IOS と Android のスタイルバー")

## <a name="animate-a-progressbar"></a>ProgressBar をアニメーション化する

`ProgressTo`メソッドは、の `ProgressBar` 現在の値から指定された値までをアニメーション化し `Progress` ます。 メソッドは、 `float` 進行状況の値 ( `uint` ミリ秒単位) と列挙値を受け取り、を `Easing` 返し `Task<bool>` ます。 次のコードは、をアニメーション化する方法を示してい `ProgressBar` ます。

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

列挙体の詳細については `Easing` 、「 [」 Xamarin.Forms の「イージング関数](~/xamarin-forms/user-interface/animation/easing.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [ProgressBar のデモ](/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)