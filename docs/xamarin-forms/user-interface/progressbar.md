---
title: Xamarin. フォーム ProgressBar
description: Xamarin. フォーム ProgressBar は、フロートプロパティに基づいて塗りつぶされる水平バーとして進行状況を視覚的に表すコントロールです。
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: 40f3c9a5af29d1782249775b9f3166698eb6221a
ms.sourcegitcommit: 7acff5c5a03a0351962c05beebfc347503d83fe6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2019
ms.locfileid: "73201948"
---
# <a name="xamarinforms-progressbar"></a>Xamarin. フォーム ProgressBar
[![サンプルのダウンロード](~/media/shared/download.png)サンプルのダウンロード](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

Xamarin [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)コントロールは、進行状況を、`float` 値で表される割合に設定された水平バーとして視覚的に表現します。 `ProgressBar` クラスは[`View`](xref:Xamarin.Forms.View)から継承されます。

次のスクリーンショットは、iOS と Android の `ProgressBar` を示しています。

![IOS と Android の ProgressBar のスクリーンショット](progressbar-images/progressbars-default.png "IOS と Android の ProgressBar")

`ProgressBar` コントロールは、次の2つのプロパティを定義します。

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)は、現在の進行状況を 0 ~ 1 の値として表す `float` 値です。 0未満の `Progress` 値は0に固定され、1より大きい値は1にクランプされます。
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor)は、現在の進行状況を表す内部バーの色に影響を与える `Color` です。

これらのプロパティは[`BindableProperty`](xref:Xamarin.Forms.BindableProperty)のオブジェクトによって支えられています。つまり、`ProgressBar` のスタイルを設定し、データバインディングのターゲットにすることができます。

また、`ProgressBar` コントロールは、バーを現在の値から指定された値にアニメーション化する `ProgressTo` メソッドも定義します。 詳細については、「 [ProgressBar をアニメーション化する](#animate-a-progressbar)」を参照してください。

> [!NOTE]
> `ProgressBar` は、Tab キーを使用してコントロールを選択したときにスキップされるように、ユーザー操作を受け入れません。

## <a name="create-a-progressbar"></a>ProgressBar を作成する

`ProgressBar` は、XAML でインスタンス化できます。 `Progress` プロパティによって、内部の色分けされたバーの塗りつぶしの割合が決まります。 既定の `Progress` プロパティ値は0です。 次の例は、省略可能な `Progress` プロパティセットを使用して、XAML で `ProgressBar` をインスタンス化する方法を示しています。

```xaml
<ProgressBar Progress="0.5" />
```

コードでは、`ProgressBar` を作成することもできます。

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> `ProgressBar`では、`Center`、`Start`、`End` などの制約のない水平レイアウトオプションは使用しないでください。 UWP では、`ProgressBar` は、幅がゼロのバーに折りたたまれます。 `Fill` の既定の `HorizontalOptions` 値をそのまま使用し、`Grid` レイアウトに `ProgressBar` を配置するときに `Auto` の幅を使用しないようにします。

## <a name="progressbar-appearance-properties"></a>ProgressBar の外観のプロパティ

`ProgressColor` プロパティは、`Progress` プロパティが0より大きい場合に、内部バーの色を定義します。 次の例は、`ProgressColor` プロパティセットを使用して XAML で `ProgressBar` をインスタンス化する方法を示しています。

```xaml
<ProgressBar ProgressColor="Orange" />
```

`ProgressColor` プロパティは、コードで `ProgressBar` を作成するときに設定することもできます。

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

次のスクリーンショットは、iOS および Android で `ProgressColor` プロパティが `Color.Orange` に設定されている `ProgressBar` を示しています。

![IOS と Android のスタイルバーのスクリーンショット](progressbar-images/progressbars-styled.png "IOS と Android のスタイルバー")

## <a name="animate-a-progressbar"></a>ProgressBar をアニメーション化する

`ProgressTo` メソッドは、現在の `Progress` 値から指定された値まで `ProgressBar` を時間の経過と共にアニメーション化します。 メソッドは、`float` の進行状況値、`uint` の期間 (ミリ秒単位)、`Easing` 列挙値を受け取り、`Task<bool>`を返します。 次のコードは、`ProgressBar`をアニメーション化する方法を示しています。

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

`Easing` 列挙体の詳細については、「 [Xamarin. Forms のイージング関数](~/xamarin-forms/user-interface/animation/easing.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [ProgressBar のデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)
