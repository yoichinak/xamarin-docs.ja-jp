---
title: Xamarin.Forms の ProgressBar
description: Xamarin.Forms ProgressBar は、コントロールを水平バーとして設定されている float プロパティに基づいて進行状況を視覚的に表すです。
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: 2e2917077575ebc78dbdda8ae55aa230a39a7ba1
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67837012"
---
# <a name="xamarinforms-progressbar"></a>Xamarin.Forms の ProgressBar
[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos)

Xamarin.Forms [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar)コントロールで表されるパーセンテージに設定されている横棒として進行状況を視覚的に表す、`float`値。 `ProgressBar`クラスから継承[ `View`](xref:Xamarin.Forms.View)します。

次のスクリーン ショット、 `ProgressBar` iOS と Android で。

![IOS と Android では、スクリーン ショットの ProgressBar](progressbar-images/progressbars-default.png "iOS や Android 上の ProgressBar")

`ProgressBar`コントロールは、2 つのプロパティを定義します。

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress) `float`値として 0 から 1 を現在の進行状況を表す値です。 `Progress` 0 は 0、1 は 1 にクランプするよりも大きい値に固定されます未満の値します。
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor) `Color`内部に影響するバーの現在の進行状況を表す色。

これらのプロパティが支え[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクト、つまり、`ProgressBar`スタイルを設定できるし、データ バインディングのターゲットにします。

`ProgressBar`コントロールも定義、`ProgressTo`指定の値を現在の値から、バーがアニメーション化するメソッド。 詳細については、次を参照してください。 [ProgressBar をアニメーション化する](#animate-a-progressbar)します。

> [!NOTE]
> `ProgressBar`ユーザー操作を承認されませんし、Tab キーを使用してコントロールを選択する場合はスキップされます。

## <a name="create-a-progressbar"></a>ProgressBar を作成します。

A `ProgressBar` XAML でインスタンス化することができます。 その`Progress`内部の色付きのバーの塗りつぶしの割合を決定するプロパティを設定できます。 場合、`Progress`プロパティが設定されていない、既定値は 0。 次の例では、インスタンス化する方法を示しています、 `ProgressBar` XAML で、オプションで`Progress`プロパティ セット。

```xaml
<ProgressBar Progress="0.5" />
```

A`ProgressBar`コードで作成することもできます。

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> などの制約のない水平レイアウト オプションを使用しない`Center`、 `Start`、または`End`で`ProgressBar`します。 UWP での`ProgressBar`ゼロ幅のバーに折りたたまれます。 既定値を保持`HorizontalOptions`値`Fill`の幅を使用しないと`Auto`設定時に、`ProgressBar`で、`Grid`レイアウト。

## <a name="progressbar-appearance-properties"></a>ProgressBar の外観のプロパティ

`ProgressColor`プロパティを設定して、内部のバーを定義するときに色、`Progress`プロパティが 0 より大きい。 次の例では、インスタンス化する方法を示しています、`ProgressBar`を XAML で、`ProgressColor`プロパティ セット。

```xaml
<ProgressBar OnColor="Orange" />
```

`ProgressColor`作成するときに、プロパティを設定することも、`ProgressBar`コード。

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

次のスクリーン ショット、`ProgressBar`で、`ProgressColor`プロパティに設定`Color.Orange`iOS と Android で。

![スタイルの ProgressBar で iOS と Android のスクリーン ショット](progressbar-images/progressbars-styled.png "iOS や Android 上の ProgressBar のスタイル")

## <a name="animate-a-progressbar"></a>ProgressBar をアニメーション化します。

`ProgressTo`メソッドをアニメーション化、`ProgressBar`から現在の`Progress`値を時間の経過と共に指定された値。 メソッドを受け入れます、`float`値、進行状況を`uint`期間 (ミリ秒単位)、`Easing`列挙型の値を返します、`Task<bool>`します。 次のコードは、アニメーション化する方法を示して、 `ProgressBar`:

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

詳細については、`Easing`列挙型を参照してください[イージング関数を Xamarin.Forms で](~/xamarin-forms/user-interface/animation/easing.md)します。

## <a name="related-links"></a>関連リンク

* [ProgressBar のデモ](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ProgressBarDemos)
