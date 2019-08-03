---
title: Xamarin. Forms のアクティビティインジケーター
description: ActivityIndicator コントロールは、進行状況を示すことなく、アプリケーションが時間のかかるアクティビティに関与していることをユーザーに示します。 この記事では、XAML とコードで ActivityIndicator を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: e13a46e1022f4e33ace6f9f19bb5cea5d1ac784b
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739163"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin. Forms ActivityIndicator
[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)

Xamarin. フォーム[`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)は、アプリケーションが時間のかかるアクティビティに関与していることを示すアニメーションを表示するコントロールです。 とは異なり、 `ActivityIndicator`では進行状況が示されません。 [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) は`ActivityIndicator` 、から[`View`](xref:Xamarin.Forms.View)継承されます。

次のスクリーンショットは`ActivityIndicator` 、iOS と Android のコントロールを示しています。

![IOS と Android での ActivityIndicator のスクリーンショット](activityindicator-images/activityindicators-default.png "IOS と Android での ActivityIndicator のスクリーンショット")

コントロール`ActivityIndicator`は、次のプロパティを定義します。

* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning)を表示、アニメーション、または`ActivityIndicator`非表示にするかどうかを示す値です。`bool` 値が`false`の場合、 `ActivityIndicator`は表示されません。
* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color)の表示色を定義する`Color`値です`ActivityIndicator`。

これらのプロパティは、 [`BindableProperty`](xref:Xamarin.Forms.BindableProperty)オブジェクトによって支えら`ActivityIndicator`れています。つまり、をスタイル設定し、データバインディングのターゲットにすることができます。

## <a name="create-an-activityindicator"></a>ActivityIndicator を作成する

は`ActivityIndicator` 、XAML でインスタンス化できます。 その`IsRunning`プロパティを設定して、コントロールが表示され、アニメーション化されるかどうかを判断できます。 プロパティが`IsRunning`設定されていない場合`false`は、 `ActivityIndicator`既定でに設定され、は表示されません。 次の例は、省略可能`ActivityIndicator` `IsRunning`なプロパティセットを使用して、XAML でをインスタンス化する方法を示しています。

```xaml
<ActivityIndicator IsRunning="true" />
```

コードでを作成することもできます。`ActivityIndicator`

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>ActivityIndicator の外観のプロパティ

プロパティを設定して、 `ActivityIndicator`色を定義できます。 `Color` 次の例は、プロパティセットを`ActivityIndicator`使用し`Color`て XAML でをインスタンス化する方法を示しています。

```xaml
<ActivityIndicator Color="Orange" />
```

プロパティ`Color`は、コードでを`ActivityIndicator`作成するときに設定することもできます。

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

次のスクリーンショットは`ActivityIndicator` 、iOS `Color`および Android で`Color.Orange`プロパティがに設定されたを示しています。

![IOS と Android のスタイル付き ActivityIndicator のスクリーンショット](activityindicator-images/activityindicators-styled.png "IOS と Android のスタイル付き ActivityIndicator のスクリーンショット")

## <a name="related-links"></a>関連リンク

* [ActivityIndicator デモ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-activityindicatordemos/)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
