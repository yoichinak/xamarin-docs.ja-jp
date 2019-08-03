---
title: Xamarin. フォーム ProgressBar
description: Xamarin. フォーム ProgressBar は、フロートプロパティに基づいて塗りつぶされる水平バーとして進行状況を視覚的に表すコントロールです。
ms.prod: xamarin
ms.assetId: C2F85FED-797C-466B-A0FD-E73CFB79B267
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/09/2019
ms.openlocfilehash: 78c5f38428e20a2e0c6a15d0964f8fd505a8d082
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739411"
---
# <a name="xamarinforms-progressbar"></a>Xamarin. フォーム ProgressBar
[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)

Xamarin. フォーム[`ProgressBar`](xref:Xamarin.Forms.ProgressBar)は、進行状況を、 `float`値で表されるパーセントに設定された水平バーとして視覚的に表すコントロールです。 クラス`ProgressBar`は、から[`View`](xref:Xamarin.Forms.View)継承されます。

次のスクリーンショットは`ProgressBar` 、iOS と Android のを示しています。

![IOS と Android の ProgressBar のスクリーンショット](progressbar-images/progressbars-default.png "IOS と Android の ProgressBar")

コントロール`ProgressBar`は、次の2つのプロパティを定義します。

* [`Progress`](xref:Xamarin.Forms.ProgressBar.Progress)現在の`float`進行状況を 0 ~ 1 の値として表す値を指定します。 `Progress`0未満の値は0に固定され、1より大きい値は1にクランプされます。
* [`ProgressColor`](xref:Xamarin.Forms.ProgressBar.ProgressColor)は、 `Color`現在の進行状況を表す内部バーの色に影響を与えるです。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えら`ProgressBar`れています。つまり、をスタイル設定し、データバインディングのターゲットにすることができます。

また`ProgressBar` 、コントロールは、 `ProgressTo`バーを現在の値から指定された値にアニメーション化するメソッドも定義します。 詳細については、「 [ProgressBar をアニメーション化する](#animate-a-progressbar)」を参照してください。

> [!NOTE]
> は`ProgressBar` 、Tab キーを使用してコントロールを選択したときにスキップされるように、ユーザー操作を受け入れません。

## <a name="create-a-progressbar"></a>ProgressBar を作成する

は`ProgressBar` 、XAML でインスタンス化できます。 この`Progress`プロパティを設定して、内部の色分けされたバーの塗りつぶしの割合を決定できます。 `Progress`プロパティが設定されていない場合、既定値は0です。 次の例は、省略可能`ProgressBar` `Progress`なプロパティセットを使用して、XAML でをインスタンス化する方法を示しています。

```xaml
<ProgressBar Progress="0.5" />
```

は`ProgressBar` 、コードで作成することもできます。

```csharp
ProgressBar progressBar = new ProgressBar { Progress = 0.5f };
```

> [!WARNING]
> `Center`、、などの制約のない水平レイアウトオプション`Start`は、 `End`で`ProgressBar`は使用しないでください。 UWP では、 `ProgressBar`は、幅が0のバーに折りたたまれます。 `HorizontalOptions` `Fill` `Auto`レイアウトに`ProgressBar`を配置する場合は、の既定値をそのままにして、の幅を使用しないようにします。 `Grid`

## <a name="progressbar-appearance-properties"></a>ProgressBar の外観のプロパティ

プロパティは、 `Progress`プロパティが0より大きい場合に、内部バーの色を定義するように設定できます。 `ProgressColor` 次の例は、 `ProgressBar` `ProgressColor`プロパティセットを使用して、XAML でをインスタンス化する方法を示しています。

```xaml
<ProgressBar OnColor="Orange" />
```

プロパティ`ProgressColor`は、コードでを`ProgressBar`作成するときに設定することもできます。

```csharp
ProgressBar progressBar = new ProgressBar { ProgressColor = Color.Orange };
```

次のスクリーンショットは`ProgressBar` 、iOS `ProgressColor`および Android で`Color.Orange`プロパティがに設定されたを示しています。

![IOS と Android のスタイルバーのスクリーンショット](progressbar-images/progressbars-styled.png "IOS と Android のスタイルバー")

## <a name="animate-a-progressbar"></a>ProgressBar をアニメーション化する

メソッド`ProgressTo`は、の`ProgressBar`現在`Progress`の値から指定された値までをアニメーション化します。 メソッドは、 `float`進行状況の値`uint` (ミリ秒単位) `Easing`と列挙値を受け取り、 `Task<bool>`を返します。 次のコードは、を`ProgressBar`アニメーション化する方法を示しています。

```csharp
// animate to 75% progress over 500 milliseconds with linear easing
await progressBar.ProgressTo(0.75, 500, Easing.Linear);
```

列挙体の`Easing`詳細については、「 [Xamarin. Forms のイージング関数](~/xamarin-forms/user-interface/animation/easing.md)」を参照してください。

## <a name="related-links"></a>関連リンク

* [ProgressBar のデモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-progressbardemos/)
