---
title: Xamarin.Forms でのアクティビティのインジケーター
description: ActivityIndicator コントロールはユーザーに進行状況の情報を与えることがなく時間がかかる作業では、アプリケーションは関与していることを示します。 この記事では、XAML とコードで、ActivityIndicator を使用する方法について説明します。
ms.prod: xamarin
ms.assetid: 4CEED02D-5CA3-4C3A-B7ED-3193FC272261
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/10/2019
ms.openlocfilehash: abd8150e3aa4ec347c8d956004993340630208bf
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/11/2019
ms.locfileid: "67837062"
---
# <a name="xamarinforms-activityindicator"></a>Xamarin.Forms ActivityIndicator
[![サンプルのダウンロード](~/media/shared/download.png)サンプルをダウンロードします。](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos)

Xamarin.Forms[ `ActivityIndicator` ](xref:Xamarin.Forms.ActivityIndicator)は、アプリケーションは時間がかかる作業に関与しているかを表示するアニメーションを表示するコントロールです。 異なり、 [ `ProgressBar` ](xref:Xamarin.Forms.ProgressBar)、`ActivityIndicator`進行状況が表示されません。 `ActivityIndicator`継承[ `View`](xref:Xamarin.Forms.View)します。

次のスクリーン ショット、 `ActivityIndicator` iOS と Android 上のコントロール。

![IOS と Android では、スクリーン ショットの ActivityIndicator](activityindicator-images/activityindicators-default.png "iOS と Android で ActivityIndicator のスクリーン ショット")

`ActivityIndicator`コントロールは、次のプロパティを定義します。

* [`IsRunning`](xref:Xamarin.Forms.ActivityIndicator.IsRunning) `bool`を示す値かどうか、`ActivityIndicator`とアニメーション、表示/非表示にする必要があります。 値が`false`、`ActivityIndicator`には表示されません。
* [`Color`](xref:Xamarin.Forms.ActivityIndicator.Color) `Color`の表示の色を定義する値、`ActivityIndicator`します。

これらのプロパティが支え[ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty)オブジェクト、つまり、`ActivityIndicator`スタイルを設定できるし、データ バインディングのターゲットにします。

## <a name="create-an-activityindicator"></a>作成、ActivityIndicator

`ActivityIndicator` XAML でインスタンス化することができます。 その`IsRunning`コントロールが表示され、アニメーション化を確認するのにはプロパティを設定できます。 場合、`IsRunning`プロパティが設定されていない、既定値は`false`、`ActivityIndicator`には表示されません。 次の例では、インスタンス化する方法を示しています、 `ActivityIndicator` XAML で、オプションで`IsRunning`プロパティ セット。

```xaml
<ActivityIndicator IsRunning="true" />
```

`ActivityIndicator`コードで作成することもできます。

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { IsRunning = true };
```

## <a name="activityindicator-appearance-properties"></a>ActivityIndicator 外観のプロパティ

`Color`プロパティの設定を定義することができます、`ActivityIndicator`色。 次の例では、インスタンス化する方法を示しています、`ActivityIndicator`を XAML で、`Color`プロパティ セット。

```xaml
<ActivityIndicator Color="Orange" />
```

`Color`作成するときに、プロパティを設定することも、`ActivityIndicator`コード。

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator { Color = Color.Orange };
```

次のスクリーン ショット、`ActivityIndicator`で、`Color`プロパティに設定`Color.Orange`iOS と Android で。

![スタイル設定された ActivityIndicator ios と Android のスクリーン ショット](activityindicator-images/activityindicators-styled.png "スタイル ActivityIndicator ios と Android のスクリーン ショット")

## <a name="related-links"></a>関連リンク

* [ActivityIndicator デモ](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ActivityIndicatorDemos)
* [ProgressBar](~/xamarin-forms/user-interface/progressbar.md)
